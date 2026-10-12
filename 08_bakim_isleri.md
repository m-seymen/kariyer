# İş Eşleştirme Projesi — Bakım İşleri

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak
> **Sürüm: final**
> İlgili dokümanlar: `eslestirme-motoru-final-v3.md`, `aday-profili-final.md`, `isveren-paneli-v2.md`, `auth-kararlari.md`

---

## 0. Özet

### Bakım nedir, neden gerekli

Sistemdeki işlerin çoğu bir **olayla** tetiklenir: ilan yayınlandı → eşleştir, başvuru geldi → bildirim gönder. Ama bazı şeyler **hiçbir olay olmadan, sadece zaman geçtiği için** olmalı:

| Örnek | Kimse bir şey yapmadan… |
|---|---|
| İlan 60 gün oldu | → yayından kalkmalı |
| Davet 10 gündür cevapsız | → süresi dolmalı |
| Aday 30 gündür girmedi | → "fırsatlara açığım"a düşmeli |
| Eski skorlar birikti | → temizlenmeli |
| Sonuçlanan başvurunun üzerinden 1 yıl geçti | → kişisel ayrıntılar silinmeli |

Bunları bir kullanıcı aksiyonu tetiklemiyor — **biri saate bakıp yapmalı.** O da bakım işi. Bir binanın kapıcısı gibi: kimse çağırmasa da her sabah çöpleri toplar, süresi dolan ilanları panodan indirir.

### `pg_cron` — veritabanındaki saat

Bakım işleri Supabase'in **`pg_cron`** eklentisiyle çalışır: *"Şu sıklıkla, şu işi yap."*

```sql
SELECT cron.schedule(
  'ilan-sure-kontrol',                       -- işin adı
  '0 * * * *',                               -- her saat başı
  $$ SELECT ilan_surelerini_kontrol_et(); $$ -- ne yapılacak
);
```

| Yazım | Anlamı |
|---|---|
| `* * * * *` | Her dakika |
| `*/15 * * * *` | 15 dakikada bir |
| `0 * * * *` | Her saat başı |
| `0 3 * * *` | Her gün 03:00 (UTC) |

- Her çalışma `cron.job_run_details` tablosuna kaydedilir — ne zaman başladı, ne kadar sürdü, başarılı mı
- Saatler **UTC**
- Veritabanının içinde çalışır — bu yüzden ağır işler parça parça ve düşük trafik saatinde

**Neden çoğu SQL fonksiyonu:** Bakım işleri veritabanındaki veriyle çalışıyor, dış servis çağırmıyor. `pg_cron` → SQL fonksiyonu: ağ yok, ek sunucu yok. Bildirim gerektiren işler sadece **bildirim kuyruğuna yazar** — gönderimi bildirim işçisi yapar.

### Tüm işler

| # | İş | Grup | Sıklık |
|:---:|---|---|---|
| 1 | İlan süresi | Süreler | Saatlik |
| 2 | Başvuru ve davet süresi | Süreler | Saatlik |
| 3 | Otomatik düşme | Kullanıcı durumu | Günlük |
| 4 | Hesap askıya alma | Kullanıcı durumu | Günlük |
| 5 | Eşleştirme hata tekrarı | Eşleştirme sağlığı | 15 dakika |
| 6 | Sırayla tazeleme | Eşleştirme sağlığı | Günlük |
| 7 | Sonuç tablosu temizliği | Eşleştirme sağlığı | Günlük |
| 8 | Görüntülenme toplama | Sayaçlar | Dakikalık |
| 9 | Sayaç düzeltme | Sayaçlar | Günlük |
| 10 | Sonuçlananları listeden gizleme | Saklama | Günlük |
| 11 | Başvuru ayrıntılarını silme | Saklama | Günlük |
| 12 | Kapatılan şirketler ve belgeler | Saklama | Günlük |
| 13 | Zaman bölümleri | Saklama | Günlük |

---

## 1. İlan Süresi

**Ne yapar:** Yayın süresi dolan ilanları kapatır.

| | |
|---|---|
| Yayın süresi | **60 gün** |
| Uzatma | 15 / 30 / 60 gün — kesintisiz en fazla **180 gün** |
| Sıklık | **Saatlik** |
| Önceden hatırlatma | Yok — işveren ilan kartındaki "⏱ X gün kaldı"dan görür |

### Süre dolunca

> **Kural:** İlan nasıl kapanırsa kapansın — süresi dolsun ya da elle kapatılsın — **sadece Keşfet'ten çıkar.** Bekleyen işler devam eder.

| | Ne olur |
|---|---|
| İlan | `durum = kapali`, `kapanma_sebebi = suresi_doldu`; `ilan_yayin_gecmisi`'ne satır |
| Keşfet | Çıkar — yeni başvuru ve davet gelmez |
| **Bekleyen başvurular** | **Kapanmaz** — işveren kabul / red yapmaya devam eder; 30 günlük süre işler |
| **Cevap bekleyen davetler** | **Kapanmaz** — aday kabul / red yapmaya devam eder; 10 günlük süre işler |
| Süreçteki adaylar | Sohbet devam eder |
| Sonuç tablosu satırları | Silinir |
| Adayın gördüğü | ⏳ *Değerlendiriliyor · İlan yayından kalktı* |
| İlanın sorumlusu | 🔔 *"Senior Backend Developer ilanınızın süresi doldu. 3 bekleyen başvuru ve 2 cevap bekleyen davetiniz var."* **[Tekrar yayınla]** |

**Neden saatlik:** Günlük olsa ilan 23 saate kadar fazladan yayında kalabilir. Saatlik yük çok az — sadece o saatte süresi dolanlar işlenir.

**Neden bekleyenler kapanmıyor:** Süre dolması işverenin kararı değil — elindeki işi kesmeyelim.

### Tekrar yayınla — sadece süresi dolan ilanda

| | |
|---|---|
| İlan | `kapali` → `yayinda` · **aynı ilan kimliği** · `yayin_sayisi + 1` · yeni 60 gün |
| Sonuç tablosu | Sıfırdan hesaplanır — o anki aday havuzuyla |
| Embedding, LLM etiketleri | Yeniden üretilmez — metin aynı |
| Aksiyon tablosu | Korunur — gizlenen, reddedilen, başvurmuş adaylar tekrar listeye düşmez |

> **Elle kapatılan ilan tekrar yayınlanamaz** — **[Kopyala — yeni ilan oluştur]** ile yeni taslak açılır. Ayrıntı → eşleştirme dokümanı.

---

## 2. Başvuru ve Davet Süresi

**Ne yapar:** Cevapsız kalan başvuru ve davetleri sonuçlandırır.

| | Süre | Süre dolunca |
|---|:---:|---|
| **Başvuru** — işverenin cevabı | **30 gün** | ⌛ *Yanıtlanmadan sona erdi* |
| **Davet** — adayın cevabı | **10 gün** | ⌛ *Yanıtlanmadan sona erdi* |

| | Başvuru | Davet |
|---|---|---|
| Kime bildirim | Adaya 🔔 *"Senior Backend Developer başvurunuz yanıtlanmadan sona erdi."* | İlanın sorumlusuna 🔔 *"Ayşe Kaya'ya gönderdiğiniz davet yanıtlanmadan sona erdi."* |
| Push | Yok | Yok |
| Belge erişimi | İşverenin erişimi **kapanır** | — |
| Tekrar | Aday bu ilana tekrar başvuramaz | Bu ilandan bu adaya tekrar davet gönderilemez |
| Keşfet | İki tarafın Keşfet'ine bir daha düşmez | Aynı |
| Aşama | **En ileri aşama** korunur (rapor için) | Aynı |

| | |
|---|---|
| Sıklık | **Saatlik** |
| İlan kapalıysa | Fark etmez — süre işlemeye devam eder |
| Önceden hatırlatma | Yok |

**Neden saatlik — günlük değil:**

```
Ayşe 12 Eylül 14:20'de başvurdu → 12 Ekim 14:20'de süresi dolar
Saatlik: 12 Ekim 15:00'te yakalanır     → en fazla ~40 dk sapma
Günlük:  13 Ekim 03:00'te yakalanır     → 12 saatten fazla sapma
```

Saatlik aynı zamanda **daha hafif:** her çalışmada sadece o saatte dolanlar işlenir; günlükte tek seferde büyük yığın olurdu.

```sql
CREATE INDEX ON basvurular (basvuru_tarihi) WHERE sonuc = 'bekliyor';
CREATE INDEX ON davetler   (davet_tarihi)   WHERE sonuc = 'bekliyor';
```

**Neden push yok, sadece 🔔:** Acil değil; telefonu titretip "reddedildin" hissi vermesin. Cevap vermeyen taraf zaten durumu biliyor — sadece bekleyen taraf bilgilendirilir.

---

## 3. Otomatik Düşme

**Ne yapar:** Uzun süre uygulamayı açmayan adayların arama durumunu düşürür.

### Sorun

Aday 3 ay önce **"Aktif arıyorum"** seçti, başka yerden işe girdi, uygulamayı bir daha açmadı. İşveren onu 🟢 rozetiyle görüp davet atıyor — cevap gelmiyor. Bunu onlarca adayda yaşayan işveren platformu bırakır.

> **Ölü profiller ürünü sessizce öldürür.**

### Kural

```
🟢 Aktif arıyorum
      │  30 gün uygulamayı açmadı
      ↓
🟡 Fırsatlara açığım
      │  toplam 90 gün açmadı
      ↓
⚪ Şu an bakmıyorum   → işverenlerin listesinden çıkar, davet gelmez
```

| Gün | Ne olur |
|---|---|
| 0 | Son giriş |
| 23 | 📱 *"Bir süredir giriş yapmadın. Arama durumun 7 gün sonra 'Fırsatlara açığım' olacak."* |
| 30 | → 🟡 Fırsatlara açığım |
| 83 | 📱 *"Profilin 7 gün sonra işverenlerden gizlenecek."* |
| 90 | → ⚪ Şu an bakmıyorum · sonuç tablosu satırları silinir |
| Geri dönünce | *"Profilin gizli. Tekrar iş aramaya başlamak ister misin?"* **[Aktif et]** |

| | |
|---|---|
| "Giriş" ne demek | Uygulamayı **açmak** — bildirim almak sayılmaz |
| Başlangıç durumu | Fark etmez — zaten "fırsatlara açığım" olan da 90 günde düşer |
| Bekleyen başvuru / davet, süreçteki ilanlar | **Devam eder** |
| Profil verisi | Silinmez — sadece görünürlük değişir |
| Geri dönüşte | **Otomatik açılmaz** — belki işe girmiş, sadece mesajına bakıyor; kendisi seçer |
| Uyarı zamanı | Adayın saat diliminde sabah |
| Sıklık | **Günlük** — 30. mı 31. gün mü düştüğü önemli değil |
| Süreler | Veritabanında — gerçek veriyle ayarlanır |

---

## 4. Hesap Askıya Alma

**Ne yapar:** Kendisine şifre gönderilmiş ama **hiç giriş yapılmamış** hesapları 7 gün sonra askıya alır.

### Hangi hesaplar

| Hesap | Nasıl açılıyor |
|---|---|
| **Ekip üyesi** | Şirket sahibi Ekip'ten ekler → e-postayla rastgele şifre |
| **Yeni işveren (şirket sahibi)** | Admin başvuruyu onaylar → e-postayla rastgele şifre |

### Neden — güvenlik

| Risk | Örnek |
|---|---|
| Yanlış e-posta | Adres yanlış yazıldı — şifre başkasına gitti |
| Ele geçirilmiş e-posta | Mail kutusuna giren biri şifreyi bulur |
| Unutulmuş hesap | Kimsenin kullanmadığı ama çalışan bir hesap açık kalır |

### Askıya alınınca

| Hesap | Ne görür | Nasıl açılır |
|---|---|---|
| **Ekip üyesi** | *"Hesabınız askıya alındı. Şirket yöneticinizle iletişime geçin."* | Sahip Ekip'te **[Daveti yeniden gönder]** → yeni şifre, yeni 7 gün |
| **Şirket sahibi** | *"Hesabınız askıya alındı."* | **[Şifremi unuttum]** → e-postaya sıfırlama linki → yeni şifre |

> Sahip için sıfırlama linki yeterli: linke tıklayan, e-postanın sahibi olduğunu kanıtlamış olur; şirket zaten admin onayından geçmiş.

| | |
|---|---|
| Gün 5 | Üyeye ✉️ *"Hesabınız 2 gün içinde giriş yapılmazsa askıya alınacak."* |
| Gün 7 | Askıya alma · sahibe 🔔 *"Mehmet Kaya 7 gün içinde giriş yapmadığı için hesabı askıya alındı."* |
| Ekip ekranında | ⏸ *Askıda — hiç giriş yapmadı* **[Daveti yeniden gönder]** |
| Kapsam | Sadece **hiç giriş yapmamış** hesaplar |
| Sıklık | **Günlük** |

### İlk girişte şifre değişimi — zorunlu

```
Hoş geldiniz. Devam etmek için kendi şifrenizi belirleyin.
```

E-postayla gelen şifre ilk girişten sonra hiçbir işe yaramaz.

---

## 5. Eşleştirme Hata Tekrarı

**Ne yapar:** Yarıda kalan veya hata veren embedding / eşleştirme / ayrıştırma işlerini tekrar kuyruğa koyar.

### Neden

Kuyruk başarısız işi zaten tekrar dener — ama bazen iş **sessizce takılır:**

| Durum | Örnek |
|---|---|
| Dış servis uzun süre çöktü | Embedding sağlayıcısı 2 saat cevap vermedi — kuyruğun tekrar deneme hakkı bitti |
| İşçi tam ortada kapandı | Edge Function süre sınırına takıldı — iş "hesaplanıyor"da kaldı |
| Beklenmeyen veri | Bir profildeki tuhaf karakter işçiyi hata verdirdi |

Sonuç: aday boş Keşfet görür, **kimse nedenini bilmez.**

### Ne yapar

```
Her 15 dakikada:
  1. Durumu "hata" olanları bul
  2. 30 dakikadan uzun "hesaplanıyor"da kalanları bul (takılmış)
  3. Tekrar kuyruğa koy, deneme sayısını artır
```

| Deneme | Bekleme |
|---|---|
| 1. tekrar | 15 dk |
| 2. tekrar | 1 saat |
| 3. tekrar | 6 saat |
| 3'ten sonra | **Durur** → admin uyarısı |

| | |
|---|---|
| Neden giderek uzayan bekleme | Dış servis çöktüyse onu daha da zorlamamak |
| Neden 3'te duruyor | Üç kez başarısız olan iş büyük ihtimalle veri sorunu — insan bakmalı |
| Kullanıcı | Tekrar denenirken *"Hazırlanıyor..."* · 3 başarısızlıktan sonra *"Eşleşmeler şu an hazırlanamıyor, kısa süre içinde tekrar denenecek."* |
| Kapsam | Embedding, eşleştirme, ayrıştırma (LLM) |
| Sıklık | **15 dakika** |

---

## 6. Sırayla Tazeleme

**Ne yapar:** Uzun süredir yeniden hesaplanmamış yayındaki ilanların eşleşmelerini tazeler.

### Neden

Bazı değişiklikler **hiçbir olay tetiklemeden** sıralamayı değiştirir:

```
1 Eylül:  İlan yayınlandı → en iyi 200 aday hesaplandı
          Mehmet 215. sırada → listeye girmedi
10 gün:   Listedekilerden 30'u "bakmıyorum"a geçti,
          20'si hesabını kapattı, 40'ı gizlendi / davet edildi
          → Mehmet artık ilk 200'de olmalı — ama kimse hesaplamadı
```

Yeni kaydolan adaylar kendi hesaplarıyla zaten listeye giriyor; **sınırın altında kalıp sonra yükselenler** kaçıyor.

### Ne yapar

```
Her gün:
  Yayındaki ilanları son hesaplanma tarihine göre sırala
  → en eskilerden başlayarak belirli sayıda ilanı yeniden eşleştirme kuyruğuna koy
  → düşük öncelikle
```

| | |
|---|---|
| Kapsam | Yayındaki ilanlar · son **3 günden** eski hesaplananlar |
| Parça | Günde belirli sayıda (örn. 10.000) |
| Öncelik | **Düşük** — kullanıcı tetiklemeli işlerin önüne geçmez |
| Sıklık | **Günlük** |

**Neden hepsi her gün değil:** Çoğu ilanın listesi birkaç günde pek değişmiyor; aktif ilanlar zaten işveren açınca (24 saatten eskiyse) yenileniyor. Bu iş sadece **kimsenin dokunmadığı** ilanlar için güvenlik ağı.

---

## 7. Sonuç Tablosu Temizliği

**Ne yapar:** Eşleştirme sonuç tablosundaki eski ve gereksiz satırları siler.

### Neden

Sonuç tablosu en büyük tablo — her aday × her uygun ilan:

```
5 milyon aday × 200 eşleşme = 1 milyar satır
Temizlenmezse birkaç ayda birkaç milyar
```

Bu tablo **geçici bir önbellek** — silinen satır her zaman yeniden hesaplanabilir. Kalıcı bilgiler ayrı tablolarda, bu temizlikten etkilenmez.

### Ne silinir / ne silinmez

| Silinir | Neden |
|---|---|
| 14 günden eski hesaplananlar | Bayat — açılırsa yeniden hesaplanır |
| Kapanan ilanların satırları | Keşfet'te görünmüyor |
| "Şu an bakmıyorum" adayların satırları | Kimse görmüyor |
| Pasif (14+ gün girmemiş) kullanıcıların satırları | Açarsa anında hesaplanır |

| Silinmez | Neden |
|---|---|
| Aksiyon tablosu | "Bu adayı gizlemiştim" bilgisi kaybolmamalı |
| Başvuru / davet kayıtları + skor anlık görüntüsü | "Neden önerildi?" ve raporlar |
| Aşama geçmişi | Raporlar |

| | |
|---|---|
| Yöntem | 10.000'er satır, aralarında kısa ara · bölüm bölüm (sonuç tablosu 32 bölüm) |
| Sıklık | **Günlük**, en düşük trafik saatinde |

---

## 8. Görüntülenme Toplama

**Ne yapar:** Adayların ilan detayını açmalarını sayıp ilan kartındaki **👁 görüntülenme** sayacına ekler.

### Neden ayrı iş

```
Popüler ilan → dakikada 500 aday açıyor
Anında +1 yapılsa → aynı satıra dakikada 500 güncelleme → sıraya girer, tıkanır
```

### Biriktir, toplu ekle

```
1. Aday ilanı açar → küçük kayıt: "Aday 42, ilan 5, 14:03" → ilan satırına dokunulmaz
2. Her dakika: son dakikanın kayıtları ilan bazında sayılır → TEK güncelleme ile sayaca eklenir
```

| | |
|---|---|
| Tekil | Aynı aday aynı ilanı 10 kez açsa 1 sayılır |
| Ne sayılır | İlan **detayını** açmak — listede kartı görmek sayılmaz |
| Kim baktı | Kaydedilir, işverene gösterilmez |
| Gecikme | En fazla ~1 dakika |
| Ham kayıtlar | `ilan_goruntulemeleri` — aylık bölümlü, 12 ay (Bakım 13) |
| Sıklık | **Dakikalık** |

---

## 9. Sayaç Düzeltme

**Ne yapar:** İlan kartındaki sayıları gerçek kayıtlarla karşılaştırır, kayanları düzeltir.

### Neden sayaç tutuyoruz — anlık sayım değil

```
1 ilan kartı = 5 sayı · şirketin 50 ilanı · 60 sn'de bir yenileme
→ tek kullanıcı için dakikada 250 sayım
→ 10.000 işveren aynı anda → dakikada 2.500.000 sayım
```

Her sorgu hızlı ama milyonlarca kez tekrarlanınca veritabanı sürekli aynı sayıları saymakla meşgul olur. **Kural:** Bir sayı **okunduğundan çok daha az değişiyorsa**, hazır tutmak kazandırır.

| Sayı | Yöntem |
|---|---|
| Bekleyen başvuru, cevap bekleyen davet, süreçte, gizlenen | **Hazır sayaç** |
| Görüntülenme | **Hazır sayaç** + dakikalık toplama (tekil sayım çok pahalı) |
| Adaylar (uygun aday) | **Anlık** — sonuç tablosundan, indeksli |
| Raporlar, admin metrikleri | **Anlık** — nadiren bakılıyor |

### Sayaç neden kayar

| Sebep | Örnek |
|---|---|
| Yarıda kalan işlem | Kayıt yazıldı, sayaç güncellenmeden bağlantı koptu |
| Aynı anda iki işlem | Aynı saniyede iki başvuru |
| Toplu işlemler | "Süreci sonlandır" ile 50 başvuru |
| Elle düzeltmeler | Admin bir kaydı değiştirdi |

### Ne yapar

```
Her gün, her ilan için:
  gerçek sayım ↔ sayaçtaki değer
  farklıysa → düzelt + kaydet
```

| | |
|---|---|
| Kapsam | Yayındaki ilanlar + kapalı ama **bekleyeni olan** ilanlar |
| Düzeltme kaydı | Hangi ilan, hangi sayaç, ne kadar |
| **Erken uyarı** | Günlük düzeltme sayısı birden artarsa → kodda hata var, admin uyarısı |
| Sıklık | **Günlük** |

---

## 10. Sonuçlananları Listeden Gizleme

**Ne yapar:** 90 günden eski sonuçlanmış başvuru ve davetleri **Sonuçlanan** listelerinden kaldırır.

| | |
|---|---|
| Neden | Liste kalabalıklaşmasın, eski retler sürekli gözünün önünde olmasın |
| Ne yapar | `listede_gizli` işaretler — **silmez** |
| Aksiyon kaydı, aşamalar, skor görüntüsü | **Kalır** |
| Başvuru ayrıntıları | Kalır — 1 yıl dolunca Bakım 11 |
| Elle | Kartta **[Listeden kaldır]** — 90 günü beklemeden |
| Liste altında | *"Son 90 gündeki sonuçlanan başvuruların gösteriliyor."* |
| Kapsam | Sadece **sonuçlanmış** — bekleyen asla gizlenmez; süreçteki ilanlar bu kurala girmez |
| Taraflar | Aday (Başvurularım, Davetlerim) ve işveren (Başvuranlar, Davet Edilenler) |
| Sıklık | **Günlük** |

---

## 11. Başvuru Ayrıntılarını Silme

**Ne yapar:** Sonuçlanmasının üzerinden **1 yıl** geçen başvuru ve davetlerin **kişisel ayrıntılarını** siler.

### Neden — yasal

KVKK ve GDPR'ın temel kuralı: **kişisel veri, toplandığı amaç için gereken süreden fazla tutulmaz.**

| Bilgi | 1 yıl sonra | Neden |
|---|---|---|
| Maaş beklentisi, para birimi | 🗑 Silinir | Kişisel, artık gereksiz |
| Not | 🗑 Silinir | Kişisel serbest metin |
| Paylaşılan belgeler listesi | 🗑 Silinir | Kişisel |
| Aksiyon kaydı | ✅ Kalır | Aynı ilana tekrar düşmesin — meşru amaç |
| Aşama geçmişi | ✅ Kalır | Raporlar — kişisel ayrıntı yok |
| Skor anlık görüntüsü, tarihler, sonuç | ✅ Kalır | Raporlar, "neden bu sıralama" kaydı |

**Aydınlatma metninde:** *"Başvuru ayrıntılarınız sonuçlandıktan 1 yıl sonra silinir; başvuru yaptığınız bilgisi ve süreç adımları hizmetin işleyişi ve istatistik amacıyla saklanır."*

**Aday hesabını silerse** 1 yıl beklenmez — ayrıntılar hemen silinir, aksiyon ve aşamalar **anonimleşir** (Veri koruma dokümanı).

| | |
|---|---|
| Sıklık | **Günlük** |
| Süre | Başlangıç önerisi — **hukuki danışmanla** ve lansman ülkelerinin yasalarına göre kesinleşecek |

---

## 12. Kapatılan Şirketler ve Belgeler

### A — Şirket hesabı kapatılınca

Sahip **Şirket Yönetimi → Hesabı Kapat** der.

```
Gün 0    Kapanış süreci başlar — 30 gün içinde [Hesabı geri aç]
Gün 23   Sahibe ✉️ "Hesabınız 7 gün sonra kalıcı olarak silinecek."
Gün 30   Bakım işi kalıcı olarak kapatır
```

**Gün 0'da hemen:**

| | |
|---|---|
| İlanlar | Hepsi Keşfet'ten çıkar |
| Ekip üyeleri | Giriş yapamaz |
| Bekleyen başvurular | ⏹ *Bu pozisyon için süreç sona erdi* + adaylara 🔔 |
| Cevap bekleyen davetler | ↩ *Davet geri çekildi* + adaylara 🔔 |
| Süreçteki sohbetler | Sistem mesajı: *"Bu şirket hesabını kapattı."* — yeni mesaj gönderilemez |

> **Neden burada bekleyenler açık kalmıyor:** İlan kapatmada şirket hâlâ var, sonra değerlendirebilir. Hesap kapatmada değerlendirecek kimse kalmıyor — aday boşuna beklemesin.

**Gün 30'da:**

| Bilgi | Ne olur |
|---|---|
| Şirket profili, logo, tanıtım | 🗑 Silinir |
| Ekip üyelerinin hesapları | 🗑 Silinir |
| İlanlar | Anonimleşir — *"Kapatılmış şirket"*; raporlar için sayısal veri kalır |
| Sohbet mesajları | Adayın tarafında kalır, şirket adı *"Kapatılmış şirket"* |
| Adayların gizleme listeleri | Şirket düşer |

### B — Şirket belgeleri

| Belge | Ne kadar tutulur | Neden |
|---|---|---|
| Onaylı şirketin doğrulama belgeleri | Şirket aktif oldukça | Değişiklik taleplerinde karşılaştırma |
| Kapatılan şirketin belgeleri | Kapanıştan **1 yıl** sonra silinir | Sahtecilik, şikâyet, yasal talep için kanıt |
| Reddedilen işveren başvurusunun belgeleri | Retten **90 gün** sonra silinir | Tekrar başvuru kontrolü için yeterli |
| Değişiklik talebi belgeleri | Sonuçlandıktan **1 yıl** sonra | Kim neyi neden değiştirdi kanıtı |

Adayın kendi belgeleri bu işe girmez — aday hesabını silince hemen silinir.

| | |
|---|---|
| Sıklık | **Günlük** |
| Süreler | Veritabanında — hukuki danışmayla kesinleşecek |

---

## 13. Zaman Bölümleri

**Ne yapar:** Zamana göre bölümlü tablolarda yeni dönemin bölümünü **önceden açar**, saklama süresi dolan bölümleri **siler.**

| Tablo | Bölüm | Saklama | Neden bu süre |
|---|---|---|---|
| `ilan_goruntulemeleri` | Aylık | **12 ay** | Sayaca zaten eklendi; ham kayıt yıllık raporlar için |
| `bildirimler` | Aylık | **6 ay** | Eski 🔔 kimsenin işine yaramaz |
| Kuyruk / iş kayıtları | Haftalık | **4 hafta** | Sadece hata ayıklama |

### Neden zaman bölümü — silmenin en ucuz yolu

```sql
-- Bölüm olmasaydı: 50 milyon satırı tek tek sil → saatler, kilit
DELETE FROM ilan_goruntulemeleri WHERE tarih < '2025-10-01';

-- Bölüm varken: bütün ay tek komutla → milisaniyeler, kilit yok
DROP TABLE ilan_goruntulemeleri_2025_09;
```

**Neden önceden açılıyor:** Ayın ilk dakikasında bölüm yoksa yazmalar hata verir — birkaç dönem öncesinden hazırlanır.

**Araç: `pg_partman`** — bir kez ayarlanır (*bölüm: aylık · önceden aç: 3 · tut: 12*), `pg_cron` her gün çalıştırır.

| | |
|---|---|
| Hash bölümlü tablolar | **Girmez** — sonuç, aksiyon, mesajlar sayıya göre bölünmüş; temizlikleri kendi kurallarıyla |
| İzleme | Bölüm açılamazsa **acil uyarı** — yazmalar duracak |
| Sıklık | **Günlük** |

---

## 14. Çalışma Düzeni

### 14.1 Global ölçekte "gece" yok

Türkiye'de gece 3 iken ABD'de akşam, Asya'da sabah. Ağır işler **UTC'de trafiğin en düşük olduğu saatte** — ilk aylarda gerçek trafiğe bakılarak seçilir. Başlangıç: **UTC 03:00** civarı.

Bildirimler iş çalıştığında değil, **kullanıcının saat diliminde sabah** gönderilir — işler sadece bildirim kuyruğuna yazar.

### 14.2 Takvim

| # | İş | Sıklık | Saat (UTC) | Ağırlık |
|:---:|---|---|---|:---:|
| 8 | Görüntülenme toplama | Her dakika | — | Hafif |
| 5 | Hata tekrarı | 15 dakikada bir | — | Hafif |
| 1 | İlan süresi | Saatlik | :00 | Hafif |
| 2 | Başvuru / davet süresi | Saatlik | :05 | Hafif |
| 3 | Otomatik düşme | Günlük | 01:00 | Orta |
| 4 | Hesap askıya alma | Günlük | 01:15 | Hafif |
| 10 | Sonuçlananları gizleme | Günlük | 01:30 | Hafif |
| 6 | Sırayla tazeleme | Günlük | 02:00 | Orta |
| 13 | Zaman bölümleri | Günlük | 02:30 | Hafif |
| 7 | Sonuç tablosu temizliği | Günlük | 03:00 | **Ağır** |
| 9 | Sayaç düzeltme | Günlük | 04:00 | Orta |
| 11 | Başvuru ayrıntıları silme | Günlük | 04:30 | Orta |
| 12 | Kapatılan şirketler, belgeler | Günlük | 05:00 | Orta |

Dakikalar kaydırıldı — hepsi aynı anda başlamasın, ağır işler üst üste binmesin.

### 14.3 Dört kural

| Kural | Ne demek | Neden |
|---|---|---|
| **Parça parça** | Her seferinde en fazla 10.000 satır, aralarında kısa ara | Tablo uzun süre kilitlenmesin |
| **Üst üste binmez** | İş başlarken kilit alır; önceki bitmemişse yenisi **atlar** | Aynı iş iki kopya çalışmasın |
| **Tekrar çalışsa sorun çıkmaz** | "Süresi dolanları kapat" iki kez çalışsa ikincisi bir şey bulmaz | Yarıda kesilen iş sonraki çalışmada kaldığı yerden devam eder |
| **Süre sınırı** | Ağır iş belirli sürede bitmezse durur, kalanı ertesi güne | Gündüz trafiğine taşmasın |

### 14.4 İzleme

| İzlenen | Uyarı ne zaman |
|---|---|
| İş hiç çalışmadı | Beklenen zamanda kayıt yok |
| İş hata verdi | `cron.job_run_details`'te hata |
| İş çok uzun sürdü | Normal süresinin 3 katı |
| Sayaç düzeltme sayısı arttı | Kodda hata olabilir |
| Zaman bölümü açılamadı | **Acil** |
| Hata tekrarında biriken iş | Bir dış servis çökmüş olabilir |

Uyarılar admin panelinde + e-posta.

---

## 15. Açık Konular

| # | Konu | Ne zaman |
|:---:|---|---|
| 1 | Saklama süreleri (1 yıl, 90 gün, 30 gün) | Hukuki danışmanla, lansman ülkelerine göre |
| 2 | Ağır işlerin saati | İlk ayların trafik verisiyle |
| 3 | Otomatik düşme süreleri (30 / 90) | Kullanıcı davranışı verisiyle |
| 4 | Sırayla tazeleme günlük adedi | Kuyruk yüküne göre |
