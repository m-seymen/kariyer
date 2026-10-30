# İş Eşleştirme Projesi — Aday Paneli (Final)

> **Sürüm:** Final — aday tarafının tüm ekranları, akışları ve kuralları
> **Kapsam:** Aday uygulamayı açtığı andan itibaren gördüğü her ekran, bastığı her buton ve sonucunda olan her şey
> **Kapsam dışı:** Özgeçmiş adımlarının içi (eğitim, deneyim, yetenekler…) → `aday-profili-final.md` · Eşleştirmenin nasıl hesaplandığı → `eslestirme-motoru.md` · Zamanla çalışan işler → `bakim-isleri.md` · İşveren ekranları → `isveren-paneli-final.md` · Sohbetin içi → sohbet dokümanı *(yazılacak)*

---

## İçindekiler

| # | Bölüm | Kısaca |
|:---:|---|---|
| 0 | Bu doküman nasıl okunur | Yapı, işaretler, örnek aday |
| 1 | Temel kavramlar | Adayın bir ilanla yolculuğu, ortak parçalar, karar kimde |
| 2 | Ana yapı | Üst bar, arama durumu, alt menü, rozetler, açılış |
| 3 | Profilim | Hesap bilgileri |
| 4 | Ayarlar | Dil, bildirimler, güvenlik, hesap silme |
| 5 | Ortak parçalar | İlan başlığı, ilan gövdesi, açılır pencereler, işaretler |
| 6 | Keşfet | Liste, ilan kartı, Neden önerildi?, Bir daha gösterme, boş durumlar |
| 7 | İlan detayı | İş ve staj ilanı, alt bar, [⋮] |
| 8 | Paylaş ve İlanı bildir | |
| 9 | Başvuru ekranı | Maaş, not, belgeler, kontroller |
| 10 | Başvurularım | Bekleyen, Sonuçlanan, detay, geri çekme |
| 11 | Davetlerim | Bekleyen, Sonuçlanan, detay, kabul etme, daveti kabul et |
| 12 | Süreçteki İlanlar | Devam eden, Sonuçlanan (8 sonuç), detay, süreçten çekilme, işe alım onayı |
| 13 | Özgeçmiş sekmesi | Bu panelde değişenler |
| 14 | Bildirimler | Zil ekranı, kanallar, bildirim kataloğu |
| 15 | Hesabımı sil | Dört adım, arkada olanlar, işveren tarafında tüm ihtimaller |
| 16 | Süreler | Aday açısından tek tablo |
| 17 | Diğer dokümanlara etkisi ve açık konular | |

---

## 0. Bu Doküman Nasıl Okunur

### 0.1 Yapı

Her ekran aynı düzenle anlatılır:

1. **Ekran** — çizimi
2. **Ekranda ne var** — her parçanın anlamı
3. **Butonlar** — her buton: **basınca ne açılır → sonucunda ne olur → kime ne gider**
4. **Kurallar** — süreler, sınırlar, istisnalar

Her bölümün sonunda **akış şeması** var: hangi butona basınca hangi ekrana geçildiği.

### 0.2 Platform

Aday tarafı **önce mobil** düşünüldü — adayların çoğu telefondan girer. Çizimler telefon ekranıdır. Web'de aynı ekranlar daha geniş görünür; farklar Bölüm 2.6'da.

### 0.3 Çizimlerdeki işaretler

Çizimler her yazı tipinde düzgün hizalansın diye kutuların içinde emoji kullanılmadı. Gerçek arayüzde bu işaretler renkli rozet ve simge olarak tasarlanacak:

| Çizimde | Arayüzde | Anlamı |
|---|---|---|
| `[Buton]` | Buton | Dokunulabilir |
| `[Seçim ▾]` | Açılır liste | |
| `○` / `●` | Seçim düğmesi | Tek seçim — boş / seçili |
| `☐` / `☑` | Onay kutusu | Çoklu seçim |
| `[⋮]` | Üç nokta menüsü | Ek işlemler |
| `▔▔▔` | Alt çizgi | Seçili sekme |
| `▸` | | Seçili alt sekme |
| `(zil 3)` | Zil simgesi + sayı | Bildirimler |
| `(ayarlar)` | Dişli simgesi | Ayarlar |
| `(AK)` | Profil fotoğrafı / baş harfler | Profilim |
| `● Aktif iş arıyorum` | Yeşil rozet | Arama durumu |
| `○ Fırsatlara açığım` | Sarı rozet | Arama durumu |
| `– Şu an iş aramıyorum` | Gri rozet | Arama durumu |
| `● Yüksek uyum` · `○ Orta uyum` · `· Düşük uyum` | Yeşil · sarı · gri rozet | Uyum seviyesi |
| `İŞ İLANI` · `STAJ İLANI` | Farklı renkte rozetler | İlan tipi |
| `✓` | Yeşil tik | Sende var / uyuyor |
| `✕` | Kırmızı çarpı | Olmazsa olmaz, sende yok |
| `!` | Turuncu ünlem | Eksik (yıl az, süresi geçmiş) veya süre azaldı |
| `~` | Kum saati | Bekliyor / süre doldu |
| `*` | Kutlama simgesi | İşe alındı |
| `■` | Durdur simgesi | Süreç kapandı |
| `<-` | Geri ok | Geri çekildi / çekildin |
| `(2)` | Kırmızı sayı rozeti | Okunmamış / bekleyen sayısı |
| `•` | Kırmızı nokta | Yeni bir şey var |
| `(!)` | Kırmızı ünlem | Dikkat gerekiyor |
| `(i)` | Bilgi simgesi | Açıklama |

### 0.4 Örnek aday ve şirketler

| | |
|---|---|
| Aday | **Ayşe Kaya** — Backend Geliştirici, Kadıköy / İstanbul |
| Şirketler | **ABC Teknoloji** (sorumlu: Mehmet Kaya) · **PRS Teknoloji** · **XYZ Yazılım** · **TUV Analitik** · **DEF Yazılım** · **GHI Holding** |

---

## 1. Temel Kavramlar

### 1.1 Adayın bir ilanla yolculuğu

```
                        KEŞFET
                  (sana uygun ilanlar)
                           │
           ┌───────────────┴───────────────┐
           │ Aday başvurdu                 │ İşveren davet etti
           v                               v
      BAŞVURULARIM                     DAVETLERİM
   karar İŞVERENDE                   karar ADAYDA
   (30 gün)                          (10 gün)
           │                               │
           │ İşveren kabul etti            │ Aday kabul etti
           └───────────────┬───────────────┘
                           v
                  SÜREÇTEKİ İLANLAR
                (sohbet açık · 60 gün)
                           │
                           v
                       SONUÇ
        işe alındı · devam edilmedi · çekildi ·
        süre doldu · ilanın sonucu · şirket ayrıldı ·
        ilan kaldırıldı
```

| Yer | İçinde | Karar kimde |
|---|---|---|
| **Keşfet** | Adaya uygun, henüz bir şey yapılmamış ilanlar | Aday — başvursun mu |
| **Başvurularım** | Adayın başvurduğu ilanlar | **İşveren** — kabul / kabul etmeme |
| **Davetlerim** | İşverenin adayı davet ettiği ilanlar | **Aday** — kabul / kabul etmeme |
| **Süreçteki İlanlar** | İki tarafın da "evet" dediği ilanlar — sohbet burada | İşveren sonuçlandırır · aday çekilebilir |

**Bir ilan aynı anda sadece bir yerde olur** — başvurulan ilan Keşfet'ten kalkar, kabul edilen başvuru Başvurularım'dan çıkıp Süreçteki'ye geçer.

### 1.2 Bekleyen ve Sonuçlanan

Başvurularım, Davetlerim ve Süreçteki İlanlar ikiye ayrılır:

| Alt sekme | İçinde |
|---|---|
| **Bekleyen** *(Süreçteki'de: Devam eden)* | Henüz bitmemiş olanlar |
| **Sonuçlanan** | Bitmiş olanlar — **son 90 gün** |

**Olumlu sonuçlanan başvuru ve davet Sonuçlanan'a düşmez** — doğrudan Süreçteki İlanlar'a geçer. Başvurularım ve Davetlerim'in Sonuçlanan listesinde sadece **sürece girmeden kapananlar** vardır.

### 1.3 Ortak parçalar — her yerde aynı görünüm

Aday bir ilanı nerede görürse görsün **ilan aynı görünür**; değişen sadece üstündeki kendi bölümüdür.

| Parça | Ne | Ayrıntı |
|---|---|---|
| **İlan başlığı** | İlan tipi, uyum, unvan, şirket, şirket raporu, **ilanın durumu** | Bölüm 5.1 |
| **İlan gövdesi** | İlanın tüm bölümleri — işverenin girdiği her alan | Bölüm 5.2 |
| **Üst bölüm** | **BAŞVURUN** · **DAVETİN** · **SÜRECİN** — adayın o ilanla ilişkisi | Bölüm 10, 11, 12 |
| **Açılır pencereler** | Neden önerildi? · Şirket profili · Gönderdiğin bilgiler | Bölüm 5.3 |

### 1.4 Karar butonları nerede

| Ekran | Kartta | Detayda |
|---|---|---|
| Keşfet | **[Başvur]** ✅ | **[Başvur]** ✅ |
| Davetlerim | **[Kabul et]** · **[Kabul etme]** ✅ | ✅ |
| Başvurularım | **[Başvurumu geri çek]** ✅ | ✅ |
| Süreçteki İlanlar | ❌ **[Süreçten çekil] kartta yok** | ✅ + sohbetin [⋮] menüsünde |

**Neden adayda kartta karar butonu var:** Başvuru ve davet kabulünde **başvuru ekranı** yine açılıyor — aday ne gönderdiğini görerek karar veriyor. (İşverende kural farklı: "kartta incele, detayda karar ver".)

**Neden [Süreçten çekil] kartta yok:** Geri dönüşü olmayan bir işlem; liste kaydırılırken yanlışlıkla basılmasın.

---

## 2. Ana Yapı

### 2.1 Ekran

```
┌──────────────────────────────────────────────────────────┐
│ [● Aktif iş arıyorum ▾]          (zil 3) (ayarlar) (AK)  │  ← üst bar
├──────────────────────────────────────────────────────────┤
│                                                          │
│                                                          │
│                (seçili sekmenin içeriği)                 │
│                                                          │
│                                                          │
├──────────────────────────────────────────────────────────┤
│ Keşfet  Başvurularım  Davetlerim  Süreçteki  Özgeçmiş    │  ← alt menü
│  ▔▔▔▔        •           (2)         (3)                 │
└──────────────────────────────────────────────────────────┘
```

**Sekmenin adı üst barda yazmaz** — alt menüde seçili sekme zaten belli.

### 2.2 Üst bar

Her sekmede aynı, tek satır.

| Yer | Ne | Dokununca |
|---|---|---|
| Sol | **Arama durumu** — tam metinle | Arama durumu penceresi *(2.3)* |
| Sağ | **Zil** — okunmamış varsa sayı | Bildirimler *(Bölüm 14)* |
| Sağ | **Dişli** | Ayarlar *(Bölüm 4)* |
| Sağ | **Profil fotoğrafı / baş harfler** | Profilim *(Bölüm 3)* |

**İç ekranlarda** (ilan detayı, başvuru, ayarlar…) üst bar değişir: solda **← Geri**, ortada ekranın adı, sağda varsa **[⋮]**.

```
┌──────────────────────────────────────────────────────────┐
│ ← Geri               İlan detayı                     [⋮] │
├──────────────────────────────────────────────────────────┤
```

### 2.3 Arama durumu

| Durum | Üst barda | Etkisi |
|---|---|---|
| Aktif | **[● Aktif iş arıyorum ▾]** — yeşil | İşverenler adayı "aktif" rozetiyle görür |
| Fırsatlara açık *(varsayılan)* | **[○ Fırsatlara açığım ▾]** — sarı | İşverenler görür, rozet yok |
| Aramıyor | **[– Şu an iş aramıyorum ▾]** — gri | Özgeçmiş işverenlerden **gizlenir**, davet gelmez |

**Dokununca:**

```
┌──────────────────────────────────────────────┐
│ Arama durumun                                │
│                                              │
│ ● Aktif iş arıyorum                          │
│   İşverenler seni "aktif" rozetiyle görür.   │
│                                              │
│ ○ Fırsatlara açığım                          │
│   İşverenler seni görür, rozet yok.          │
│                                              │
│ ○ Şu an iş aramıyorum                        │
│   Özgeçmişin işverenlerden gizlenir.         │
│   Başvuruların, davetlerin ve süreçteki      │
│   ilanların devam eder.                      │
│                                              │
│ [Vazgeç]                       [Kaydet]      │
└──────────────────────────────────────────────┘
```

**"Şu an iş aramıyorum" iken** üst barın altında bant:

```
┌──────────────────────────────────────────────────────────┐
│ [– Şu an iş aramıyorum ▾]        (zil 3) (ayarlar) (AK)  │
├──────────────────────────────────────────────────────────┤
│ Özgeçmişin gizli — işverenler seni göremiyor [Aktif et]  │
├──────────────────────────────────────────────────────────┤
│                (seçili sekmenin içeriği)                 │
```

**[Aktif et]** → durum "Fırsatlara açığım" olur, bant kaybolur.

| Kural | |
|---|---|
| Otomatik düşme | 30 gün giriş yoksa → Fırsatlara açığım · 90 gün → Şu an iş aramıyorum *(7 gün önce uyarı — Bölüm 14)* |
| Geri dönünce | Otomatik açılmaz — aday kendisi seçer |
| "Aramıyor" iken | Keşfet'i görmeye ve başvurmaya devam edebilir · başvurular, davetler, süreçler devam eder |

### 2.4 Alt menü

| Sekme | İçinde | Rozet | Rozet ne zaman |
|---|---|---|---|
| **Keşfet** | Sana uygun ilanlar | — | — |
| **Başvurularım** | Yaptığın başvurular | **•** nokta | Yeni bir **sonuç** geldiyse · aday Sonuçlanan'ı açınca kaybolur |
| **Davetlerim** | Sana gelen davetler | **Sayı** | **Cevap bekleyen davet** sayısı |
| **Süreçteki** | İşverenle konuştuğun ilanlar | **Sayı** / **(!)** | Okunmamış mesaj sayısı · cevap bekleyen işe alım onayı varsa (!) |
| **Özgeçmiş** | İşverenin gördüğü özgeçmişin | **(!)** | Özgeçmiş yayında değilse |

**Rozet kuralı:** Sayı sadece **adayın bir şey yapması gereken** yerde. Başvurularım'da karar işverende — sayı yerine "yeni bir şey var" noktası.

### 2.5 Uygulama açılınca

| Durum | Açılan ekran |
|---|---|
| Normal | **Keşfet** |
| Bildirime dokunarak geldi | Bildirimin götürdüğü ekran |
| Özgeçmiş henüz yayında değil | **Özgeçmiş** |
| Paylaşılan bir ilan bağlantısından geldi | O ilanın detayı |

### 2.6 Web

| Mobil | Web |
|---|---|
| Alt menü | **Sol menü** — aynı beş bölüm, altında ayrıca **Ayarlar** |
| Üst bar | Sağ üstte arama durumu, zil, dişli, profil |
| Açılır pencereler | Ekranın ortasında pencere |
| Ekranların içi | Aynı — daha geniş |

### 2.7 Akış

```
Üst bar
  ├─ [Arama durumu ▾] ──> Arama durumu penceresi ──> [Kaydet]
  ├─ (zil) ─────────────> Bildirimler (Bölüm 14)
  ├─ (ayarlar) ─────────> Ayarlar (Bölüm 4)
  └─ (AK) ──────────────> Profilim (Bölüm 3)

Alt menü
  ├─ Keşfet ────────────> Bölüm 6
  ├─ Başvurularım ──────> Bölüm 10
  ├─ Davetlerim ────────> Bölüm 11
  ├─ Süreçteki ─────────> Bölüm 12
  └─ Özgeçmiş ──────────> Bölüm 13
```

---

## 3. Profilim

**Üst bar → profil fotoğrafı / baş harfler**

Profilim **hesabın sahibi** ile ilgili — işverenin görmediği bilgiler. İşverenin gördüğü her şey **Özgeçmiş**'te.

```
┌────────────────────────────────────────────────────────────┐
│ ← Geri                  Profilim                           │
├────────────────────────────────────────────────────────────┤
│        [AK]   Ayşe Kaya               [Fotoğrafı değiştir] │
│                                                            │
│ Ad soyad       Ayşe Kaya                         [Düzenle] │
│ E-posta        ayse@mail.com                       Kilitli │
│ Telefon        +90 532 123 45 67  ✓ Doğrulandı  [Değiştir] │
│ Doğum tarihi   12.03.1998                          Kilitli │
│ Şifre          ••••••••                         [Değiştir] │
│                                                            │
│ [Çıkış yap]                                                │
└────────────────────────────────────────────────────────────┘
```

| Alan | Kural |
|---|---|
| **Fotoğraf** | Opsiyonel · yoksa baş harfler · Özgeçmiş'teki fotoğrafla **aynı** — nereden değişirse ikisinde de değişir |
| **Ad soyad** | Düzenlenebilir · özgeçmişin en üstünde de görünür |
| **E-posta** | **Kilitli** — giriş hesabından gelir |
| **Telefon** | SMS ile doğrulanır · **özgeçmişin yayında olması için zorunlu** |
| **Doğum tarihi** | **Kilitli** — kayıtta bir kez alınır, 18 yaş kontrolü · yanlışsa Ayarlar → Bize ulaşın · işverene hiçbir yerde gösterilmez |
| **Şifre** | Mevcut şifre + yeni şifre |

**Neden doğum tarihi kilitli ama görünür:** Serbestçe değişirse 18 yaş kontrolü anlamsızlaşır. Görünür, çünkü aday hakkında hangi bilgiyi tuttuğumuzu görebilmeli.

**Telefon:**

| | |
|---|---|
| **[Değiştir]** | Yeni numara → SMS kodu → doğrulanınca eskisinin yerine geçer |
| Numara değişip doğrulanmazsa | Özgeçmiş **yayından çıkar** · başvurular, davetler, süreçler devam eder · bildirim 8.2 |
| Başka hesapta kayıtlı numara | *"Bu numara başka bir hesapta kayıtlı."* **[Bu numara benim, doğrula]** → SMS → yeni hesapta aktif, eskisinde pasif |

**[Çıkış yap]** → onay sorulmaz → giriş ekranı.

---

## 4. Ayarlar

**Üst bar → dişli**

```
┌──────────────────────────────────────────────────────────┐
│ ← Geri                  Ayarlar                          │
├──────────────────────────────────────────────────────────┤
│ ── Genel ─────────────────────────────────────────────── │
│ Dil                                           [Türkçe ▾] │
│ Saat dilimi                                 [İstanbul ▾] │
│ Görünüm                                       [Sistem ▾] │
│                                                          │
│ ── Bildirimler ───────────────────────────────────────── │
│ Telefona bildirim:                                       │
│   Davetler                                     [Açık  ●] │
│   Yeni mesaj                                   [Açık  ●] │
│   Başvuru sonuçları                            [Açık  ●] │
│   Süreç ve işe alım                            [Açık  ●] │
│   Sana uygun yeni ilanlar                      [Açık  ●] │
│ E-posta bildirimleri                           [Açık  ●] │
│ Uygulama içi bildirimler her zaman açık.                 │
│                                                          │
│ ── Güvenlik ──────────────────────────────────────────── │
│ Açık oturumlar        2 cihaz                    [Gör →] │
│                              [Tüm cihazlardan çıkış]     │
│                                                          │
│ ── Gizlilik ──────────────────────────────────────────── │
│ Verilerimi indir                                 [İndir] │
│                                                          │
│ ── Yardım ve yasal ───────────────────────────────────── │
│ Yardım merkezi                                       [→] │
│ Bize ulaşın                                          [→] │
│ Kullanım koşulları                                   [→] │
│ Gizlilik ve aydınlatma metni                         [→] │
│                                                          │
│ ── Hesap ─────────────────────────────────────────────── │
│ [Hesabımı sil]                                           │
│                                                          │
│ Sürüm 1.0.0                                              │
└──────────────────────────────────────────────────────────┘
```

| Ayar | Ne yapar |
|---|---|
| **Dil** | Uygulamanın dili — menüler, butonlar, bildirimler, e-postalar, tarih biçimi. İlk açılışta telefonun dili. **İlan metinlerini çevirmez** |
| **Saat dilimi** | Bildirim saatleri ve tarihler · ilk açılışta telefondan |
| **Görünüm** | Açık · Koyu · Sistem |
| **Telefona bildirim** | Beş tür, ayrı ayrı açılıp kapanır — hangisinin neyi kapattığı Bölüm 14.4 |
| **E-posta bildirimleri** | Tek anahtar · kapatılamayan güvenlik e-postaları hariç |
| **Uygulama içi** | Zil listesi — **kapatılamaz** |
| **Açık oturumlar** | Hangi cihazlarda giriş açık · **[Tüm cihazlardan çıkış]** |
| **Verilerimi indir** | KVKK / GDPR — adayın kendi verilerinin kopyası e-postayla gelir |
| **Hesabımı sil** | Bölüm 15 |

**Para birimi ayarı yok:** İlanlar kendi para biriminde gösterilir, kur çevrimi yapılmaz.

---
## 5. Ortak Parçalar

Bu parçalar birden fazla ekranda **aynı** kullanılır. Bir kez tasarlanır, her yerde aynı görünür.

### 5.1 İlan başlığı

```
│ İŞ İLANI                                  ● Yüksek uyum  │
│ Senior Backend Developer                                 │
│ ABC Teknoloji  ✓ Doğrulanmış                             │
│ %92 cevap verir · 12 işe alım                            │
│ İlan yayında · 3 gün önce yayınlandı                     │
```

| Satır | Ne | Kural |
|---|---|---|
| **İlan tipi** | İŞ İLANI · STAJ İLANI | Farklı renkte rozet |
| **Uyum** | ● Yüksek · ○ Orta · · Düşük | Aday puanına göre · Başvurularım, Davetlerim ve Süreçteki'de **başvuru / davet anındaki** uyum |
| **Unvan** | | |
| **Şirket** | Ad · ✓ Doğrulanmış | Şirket silindiyse *"Silinmiş şirket"* |
| **Şirket raporu** | *"%92 cevap verir · 12 işe alım"* | Son 12 ay · 20 başvurudan az veri varsa *"Platformda yeni"* |
| **İlanın durumu** | Aşağıdaki tablo | Her yerde görünür |

**İlanın durumu:**

| İlanın durumu | Başlıkta |
|---|---|
| Yayında | *"İlan yayında · 3 gün önce yayınlandı"* |
| Duraklatıldı | *"İlan geçici olarak durduruldu"* |
| Süresi doldu | *"İlanın yayın süresi doldu"* |
| Sonuçlandı | *"İlan kapandı"* |
| Silindi / kaldırıldı | *"İlan artık mevcut değil"* · *"İlan kaldırıldı"* |

### 5.2 İlan gövdesi

İşverenin ilan formunda girdiği **her alan**, formun sırasıyla. İş ve staj ilanında bölümler biraz farklı.

**İş ilanı:**

```
│ ── Pozisyon ──────────────────────────────────────────── │
│ Meslek alanı       Bilişim ve Yazılım                    │
│ Unvan              Senior Backend Developer              │
│ Şirket             ABC Teknoloji                         │
│ İş yeri konumu     Kadıköy, İstanbul, Türkiye            │
│ Çalışma şekli      Hibrit                                │
│ Çalışma tipi       Tam zamanlı                           │
│                                                          │
│ ── Maaş ve başlama ───────────────────────────────────── │
│ Maaş               80.000 – 110.000 TRY / ay (net)       │
│ Başlama zamanı     1 ay içinde                           │
│                                                          │
│ ── Yetenekler ────────────────────────────────────────── │
│ Olmazsa olmaz      React (5 yıl) ✓                       │
│                    .NET (8 yıl) ✕                        │
│                    PostgreSQL ✓                          │
│ Tercih edilen      Docker ✓                              │
│                    Kubernetes                            │
│                    AWS                                   │
│                                                          │
│ ── Tecrübe ve eğitim ─────────────────────────────────── │
│ Toplam tecrübe     3–6 yıl ✓                             │
│ Kimler başvurabilir  Sadece mezunlar ✓                   │
│ Eğitim seviyesi    Lisans, Yüksek lisans, Doktora ✓      │
│ Tercih edilen bölümler                                   │
│                    Bilgisayar Mühendisliği ✓             │
│                    Yazılım Mühendisliği                  │
│                                                          │
│ ── Sertifika ve belgeler ─────────────────────────────── │
│ Olmazsa olmaz      SRC belgesi ✓                         │
│ Tercih edilen      İSG eğitimi !  (sende süresi geçmiş)  │
│                                                          │
│ ── Dil ───────────────────────────────────────────────── │
│ İş dili            Türkçe, İngilizce ✓                   │
│                                                          │
│ ── Diğer şartlar ─────────────────────────────────────── │
│ Sürücü belgesi     Gerekli değil                         │
│ Seyahat            Seyahat engeli olmamalı ✓             │
│ Vardiya            Gerekli değil                         │
│ Askerlik           Askerlik engeli olmamalı ✓            │
│                                                          │
│ ── Diğer notlar ──────────────────────────────────────── │
│ Hafta içi 2 gün ofiste çalışılır. Ekip İngilizce         │
│ toplantı yapar.                                          │
│                                                          │
│ ── İş tanımı ─────────────────────────────────────────── │
│ Ödeme altyapımızda mikroservisler geliştirecek,          │
│ milyonlarca işlemi taşıyan sistemlerin performansından   │
│ sorumlu olacaksınız. Ekibimiz 12 kişiden oluşuyor;       │
│ mimari kararlarda söz sahibi olacaksınız.                │
```

**Staj ilanı:**

```
│ ── Pozisyon ──────────────────────────────────────────── │
│ Meslek alanı       Bilişim ve Yazılım                    │
│ Unvan              Yazılım Stajyeri                      │
│ Şirket             XYZ Yazılım                           │
│ İş yeri konumu     Ataşehir, İstanbul, Türkiye           │
│ Çalışma şekli      Ofis                                  │
│ Çalışma düzeni     Yarı zamanlı                          │
│                                                          │
│ ── Staj şartları ─────────────────────────────────────── │
│ Staj süresi        3 ay                                  │
│ Başlangıç          Yaz dönemi                            │
│ Ücret              15.000 TRY / ay (net)                 │
│ Staj türü          Zorunlu ve gönüllü staj  (i)          │
│                                                          │
│ ── Yetenekler ────────────────────────────────────────── │
│ Olmazsa olmaz      Python ✓                              │
│                    SQL ✓                                 │
│ Tercih edilen      Git                                   │
│                                                          │
│ ── Eğitim ────────────────────────────────────────────── │
│ Kimler başvurabilir  Sadece devam eden öğrenciler ✓      │
│ Eğitim seviyesi    Lisans, Yüksek lisans ✓               │
│ Tercih edilen bölümler                                   │
│                    Bilgisayar Mühendisliği ✓             │
│                    Yazılım Mühendisliği                  │
│                                                          │
│ ── Sertifika ve belgeler ─────────────────────────────── │
│ Olmazsa olmaz      Belirtilmedi                          │
│ Tercih edilen      Belirtilmedi                          │
│                                                          │
│ ── Dil ───────────────────────────────────────────────── │
│ İş dili            Türkçe ✓                              │
│                                                          │
│ ── Diğer şartlar ─────────────────────────────────────── │
│ Sürücü belgesi     Gerekli değil                         │
│ Seyahat            Gerekli değil                         │
│ Vardiya            Gerekli değil                         │
│                                                          │
│ ── Diğer notlar ──────────────────────────────────────── │
│ Staj sonunda başarılı stajyerlere iş teklifi             │
│ yapılmaktadır.                                           │
│                                                          │
│ ── Staj tanımı ───────────────────────────────────────── │
│ Yaz döneminde ürün ekibimizle birlikte gerçek projelerde │
│ çalışacaksınız. Python ile veri işleme araçları          │
│ geliştirecek, SQL sorguları yazacaksınız.                │
```

**İş ve staj farkı:**

| Bölüm | İş ilanı | Staj ilanı |
|---|---|---|
| Pozisyon | Çalışma **tipi** | Çalışma **düzeni** |
| Para ve zaman | **Maaş ve başlama** | **Staj şartları** — süre, başlangıç, ücret, staj türü |
| Tecrübe | Var — bölüm adı "Tecrübe ve eğitim" | **Yok** — bölüm adı "Eğitim" |
| Diğer şartlar | Sürücü · seyahat · vardiya · **askerlik** | Askerlik yok |
| Metin | İş tanımı | Staj tanımı |

**Formdaki her alan — gövdede nerede:**

| Form alanı | Gövdede |
|---|---|
| Meslek alanı · unvan · iş yeri konumu · çalışma şekli · çalışma tipi / düzeni | Pozisyon |
| Maaş / ücret · başlama zamanı / başlangıç · staj süresi · staj türü | Maaş ve başlama / Staj şartları |
| Olmazsa olmaz ve tercih edilen yetenekler | Yetenekler |
| Toplam tecrübe · kimler başvurabilir · eğitim seviyesi · tercih edilen bölümler | Tecrübe ve eğitim / Eğitim |
| Olmazsa olmaz ve tercih edilen sertifikalar | Sertifika ve belgeler |
| İş dili | Dil |
| Sürücü belgesi · seyahat · vardiya · askerlik | Diğer şartlar |
| Serbest şartlar | Diğer notlar |
| İş / staj tanımı | İş tanımı / Staj tanımı |

**Gösterim kuralları:**

| Kural | |
|---|---|
| **İşaretler** | ✓ sende var / uyuyor · ✕ olmazsa olmaz, sende yok · ! var ama eksik (yıl az, süresi geçmiş, tecrübe aralık dışında) · **tercih edilen eksikler işaretsiz** — aday gereksiz endişelenmesin |
| **Yetenekler** | Alt alta, yılıyla |
| **İşveren yazmadıysa** | *"Belirtilmedi"* |
| **"Gerekli değil" şartlar** | Yazılır, işaretsiz |
| **Konum** | Ülke dahil tam yol |
| **Maaş** | İşverenin yazdığı para birimi + ay + net / brüt · yazılmadıysa *"Belirtilmedi"* · stajda ücretsizse *"Ücretsiz"* |
| **Staj türü (i)** | Dokununca: *"Zorunlu staj: okulunun zorunlu tuttuğu staj — okul evrakı imzalanır, staj sigortası yapılır."* |
| **Gösterilmeyenler** | İlanın sorumlusu · görüntülenme ve başvuru sayısı · uzatma ve yayın sayısı · yapay zekanın algıladığı etiketler |

### 5.3 Açılır pencereler

Telefonda alttan yukarı kayar, ekranın büyük kısmını kaplar. **[✕]**, aşağı kaydırmak veya dışına dokunmak kapatır; aday ekranda kaldığı yere döner. Uzun içerik pencerenin içinde kayar.

#### Neden önerildi?

```
┌──────────────────────────────────────────────────────────┐
│                         ────                             │
│ Neden önerildi?                                      [✕] │
├──────────────────────────────────────────────────────────┤
│ Sana uyanlar                                             │
│                                                          │
│ ✓ Olmazsa olmaz yeteneklerden sende olanlar:             │
│   React (5 yıl) · PostgreSQL                             │
│ ✓ Tercih edilen yeteneklerden sende olanlar:             │
│   Docker                                                 │
│ ✓ Olmazsa olmaz sertifika sende var:                     │
│   SRC belgesi                                            │
│ ✓ Tecrüben istenen aralıkta:                             │
│   sende 4 yıl, istenen 3–6 yıl                           │
│ ✓ Bölümün tercih ediliyor:                               │
│   Bilgisayar Mühendisliği                                │
│                                                          │
│ Eksik olanlar                                            │
│                                                          │
│ ! Olmazsa olmaz yeteneklerden sende olmayanlar:          │
│   .NET                                                   │
│ ! React: sende 3 yıl, istenen 5 yıl                      │
└──────────────────────────────────────────────────────────┘
```

| Bölüm | Satır | Ne zaman görünür |
|---|---|---|
| **Sana uyanlar** | ✓ Olmazsa olmaz yeteneklerden sende olanlar | En az biri varsa |
| | ✓ Tercih edilen yeteneklerden sende olanlar | En az biri varsa |
| | ✓ Olmazsa olmaz sertifika sende var | Varsa ve geçerliyse |
| | ✓ Tercih edilen sertifika sende var | Varsa ve geçerliyse |
| | ✓ Tecrüben istenen aralıkta: sende X yıl, istenen Y–Z yıl | Aralıktaysa *(stajda yok)* |
| | ✓ Bölümün tercih ediliyor | Eşleşiyorsa |
| **Eksik olanlar** | ! Olmazsa olmaz yeteneklerden sende olmayanlar | En az biri eksikse |
| | ! React: sende 3 yıl, istenen 5 yıl | Yıl azsa |
| | ! Olmazsa olmaz sertifika sende yok | Eksikse |
| | ! İSG eğitiminin süresi geçmiş | Süresi dolmuşsa |
| | ! Tecrüben istenen aralıkta değil: sende X yıl, istenen Y–Z yıl | Aralık dışındaysa *(stajda yok)* |

| Kural | |
|---|---|
| Puan sayısı | **Gösterilmez** — sadece cümleler |
| Tercih edilen eksikler | **Gösterilmez** |
| İlan bir şey yazmadıysa | O satır yok · hiç eksik yoksa "Eksik olanlar" bölümü yok |
| Kaynak | Keşfet ve ilan detayında **güncel** puan · Başvurularım, Davetlerim, Süreçteki'de **başvuru / davet anındaki** kayıt |
| Üretim | Hazır şablonlar, adayın dilinde — yapay zeka kullanılmaz |

#### Şirket profili

```
┌────────────────────────────────────────────────────────────┐
│                         ────                               │
│ Şirket profili                                         [✕] │
├────────────────────────────────────────────────────────────┤
│ [Logo]  ABC Teknoloji  ✓ Doğrulanmış                       │
│         Bilişim ve Yazılım · 51-200 çalışan · 2015         │
│         Merkez: Kadıköy, İstanbul                          │
│         abcteknoloji.com · LinkedIn · X                    │
│                                                            │
│ ── Hakkında ────────────────────────────────────────────   │
│ 2015'ten beri ödeme sistemleri geliştiriyoruz. Türkiye     │
│ ve Avrupa'da 40'tan fazla bankaya altyapı sağlıyoruz.      │
│ Ekibimiz 120 kişiden oluşuyor.                             │
│                                                            │
│ ── İmkanlar ────────────────────────────────────────────   │
│ Yemek kartı · Özel sağlık sigortası ·                      │
│ Esnek çalışma saatleri · Yıllık eğitim bütçesi             │
│                                                            │
│ ── Lokasyonlar ─────────────────────────────────────────   │
│ Kadıköy, İstanbul (Merkez) · Çankaya, Ankara · Berlin      │
│                                                            │
│ ── İşe alım raporu (son 12 ay) ─────────────────────────   │
│ İlanlar                                          15        │
│    Şu an yayında                                  3        │
│    Pozisyon doldu                                 8        │
│    Uygun aday bulunamadı                          2        │
│    Alımdan vazgeçildi                             1        │
│    Sonuç bildirilmedi                             1        │
│ Başvurular                                      240        │
│    Kabul edildi                                  42  (%18) │
│    Reddedildi                                   179  (%74) │
│    Cevapsız kaldı                                19  (%8)  │
│ Davetler                                         30        │
│    Aday kabul etti                               18  (%60) │
│    Aday reddetti                                  8  (%27) │
│    Aday cevap vermedi                             4  (%13) │
│ Sürece alınan aday                               60        │
│ İşe alım                                         12        │
│    Aday tarafından onaylandı                     10        │
│                                                            │
│ ── Açık ilanlar (3) ────────────────────────────────────   │
│ Senior Backend Developer · Kadıköy        (bu ilan)        │
│ Muhasebe Uzmanı · Çankaya                       [İncele →] │
│ Depo Sorumlusu · Gebze                          [İncele →] │
└────────────────────────────────────────────────────────────┘
```

| Kural | |
|---|---|
| Yeni şirket *(20 başvurudan az)* | Rapor yerine *"Platformda yeni"* |
| **[İncele →]** | Pencere kapanır, o ilanın detayı açılır |
| Şu an bakılan ilan | *"(bu ilan)"* — buton yok |
| Şirket silindiyse | **[Şirket profili]** butonu görünmez |
| Rapor kuralları | `isveren-paneli-final.md` Bölüm 14.7 |

#### Gönderdiğin bilgiler

Sadece başvurduğu veya daveti kabul ettiği ilanlarda.

```
┌──────────────────────────────────────────────────────────┐
│                         ────                             │
│ Gönderdiğin bilgiler                                 [✕] │
├──────────────────────────────────────────────────────────┤
│ 12 Eylül 2026'da başvurdun                               │
│                                                          │
│ Maaş beklentin     90.000 TRY / ay (net, en az)          │
│                                                          │
│ Notun              Yan haklara göre maaşta esnek         │
│                    olabilirim.                           │
│                                                          │
│ Paylaştığın belgeler                                     │
│   Özgeçmiş                                      PDF      │
│   Portfolyo                                     PDF      │
│   SRC belgesi                                   PDF      │
│                                                          │
│ Bu bilgiler değiştirilemez; işveren bunları görmüş       │
│ olabilir.                                                │
└──────────────────────────────────────────────────────────┘
```

| Kural | |
|---|---|
| Davetle başlayan süreçte | En üstte ayrıca **işverenin davet notu** |
| Yazılmadıysa | *"Belirtmedin"* · belge yoksa *"Belge paylaşmadın"* |
| Belgeye dokununca | Adayın kendi dosyası açılır |
| Değiştirme | **Yok** |

---

## 6. Keşfet

### 6.1 Ekran

```
┌──────────────────────────────────────────────────────────┐
│ [● Aktif iş arıyorum ▾]          (zil 3) (ayarlar) (AK)  │
├──────────────────────────────────────────────────────────┤
│ Sana uygun 86 ilan                                       │
│ [Tümü 86]  [İş 64]  [Staj 22]           Sırala: [Uyum ▾] │
├──────────────────────────────────────────────────────────┤
│ ┌──────────────────────────────────────────────────────┐ │
│ │ İŞ İLANI                              ● Yüksek uyum  │ │
│ │                                                      │ │
│ │ Senior Backend Developer                             │ │
│ │ ABC Teknoloji  ✓ Doğrulanmış                         │ │
│ │ %92 cevap verir · 12 işe alım                        │ │
│ │                                                      │ │
│ │ Kadıköy, İstanbul · Hibrit · Tam zamanlı             │ │
│ │ 80.000 – 110.000 TRY (net)                           │ │
│ │                                                      │ │
│ │ Olmazsa olmaz:                                       │ │
│ │ React (5 yıl) ✓    .NET ✕    PostgreSQL ✓            │ │
│ │                                                      │ │
│ │ 3 gün önce yayınlandı                                │ │
│ │ Neden önerildi?                                   ▾  │ │
│ │                                                      │ │
│ │ [Bir daha gösterme]         [İncele]     [Başvur]    │ │
│ └──────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ STAJ İLANI                              ○ Orta uyum  │ │
│ │                                                      │ │
│ │ Yazılım Stajyeri                                     │ │
│ │ XYZ Yazılım  ✓ Doğrulanmış                           │ │
│ │ Platformda yeni                                      │ │
│ │                                                      │ │
│ │ Ataşehir, İstanbul · Ofis · Yarı zamanlı             │ │
│ │ 3 ay · Yaz dönemi · Zorunlu ve gönüllü staj          │ │
│ │ 15.000 TRY / ay (net)                                │ │
│ │                                                      │ │
│ │ Olmazsa olmaz:                                       │ │
│ │ Python ✓    SQL ✓                                    │ │
│ │                                                      │ │
│ │ 1 hafta önce yayınlandı                              │ │
│ │ Neden önerildi?                                   ▾  │ │
│ │                                                      │ │
│ │ [Bir daha gösterme]         [İncele]     [Başvur]    │ │
│ └──────────────────────────────────────────────────────┘ │
│                                                          │
│                   [Daha fazla getir]                     │
├──────────────────────────────────────────────────────────┤
│ Keşfet  Başvurularım  Davetlerim  Süreçteki  Özgeçmiş    │
│  ▔▔▔▔        •           (2)         (3)                 │
└──────────────────────────────────────────────────────────┘
```

### 6.2 Üst kısım

| Parça | Kural |
|---|---|
| **Sana uygun 86 ilan** | Listedeki ilan sayısı |
| **[Tümü] [İş] [Staj]** | **Sadece** Özgeçmiş'te *"Ne arıyor: İş + Staj"* seçiliyse görünür · tek tip arayanda bu satır yok |
| **Sırala** | Uyum *(varsayılan)* · En yeni |
| **Başka filtre** | **Yok** — konum, çalışma şekli, dil gibi tercihler Özgeçmiş'ten gelir; değiştirmek isteyen Özgeçmiş'ini değiştirir |
| Kaydırınca | Üst bar ve alt menü sabit · "Sana uygun…" ve tip seçimi kaybolur, yukarı kaydırınca geri gelir |
| Aşağı çekip bırakınca | Liste yenilenir |

**Neden filtre yok:** Filtreyle aday listeyi daraltıp iyi ilanları kaçırır; "bu ilan neden geldi" sorusu da karışır. Liste her zaman onun için hazır gelir.

### 6.3 İlan kartı

| Satır | İş ilanı | Staj ilanı | Kural |
|---|---|---|---|
| Rozetler | **İŞ İLANI** · uyum | **STAJ İLANI** · uyum | |
| Pozisyon | Unvan | Unvan | |
| Şirket | Ad · ✓ Doğrulanmış | Aynı | |
| Şirket raporu | *"%92 cevap verir · 12 işe alım"* | Aynı | 20 başvurudan az veride *"Platformda yeni"* |
| Nerede, nasıl | Konum · çalışma şekli · çalışma tipi | Konum · çalışma şekli · çalışma düzeni | |
| Staja özel | — | Süre · başlangıç · staj türü | |
| Para | Maaş aralığı | Aylık ücret | *"Maaş belirtilmedi"* · *"Ücretsiz staj"* |
| Olmazsa olmaz | ✓ / ✕ / ! | Aynı | En fazla 4, fazlası *"+2 daha"* · ilan yazmadıysa satır yok |
| Zaman | *"3 gün önce yayınlandı"* | Aynı | |
| **Neden önerildi? ▾** | Kapalı başlar · dokununca **kartın içinde** açılır | Aynı | İçerik 5.3 · stajda tecrübe satırı yok |

**Kartta olmayanlar:** İlanın sorumlusu · görüntülenme ve başvuru sayısı · iş tanımı · eğitim, dil ve diğer şartlar — bunlar ilan detayında.

### 6.4 Butonlar

| Buton | Görünüm | Ne olur |
|---|---|---|
| **[Başvur]** | Ana buton, renkli | **Başvuru ekranı** *(Bölüm 9)* |
| **[İncele]** / kartın boş yeri | İkincil | **İlan detayı** *(Bölüm 7)* |
| **[Bir daha gösterme]** | En sade | Onay penceresi *(6.5)* |
| **Neden önerildi? ▾** | | Kartın içinde açılır / kapanır |
| **[Daha fazla getir]** | Listenin sonunda | Liste yeniden hesaplanır — gördüğün ve işlem yaptığın ilanlar çıktığı için yeni ilanlarla tamamlanır |

### 6.5 Bir daha gösterme

```
┌──────────────────────────────────────────────┐
│ Bu ilanı bir daha görmek istemediğine        │
│ emin misin?                                  │
│                                              │
│ Bu işlem geri alınamaz.                      │
│                                              │
│ [Vazgeç]                [Evet, gösterme]     │
└──────────────────────────────────────────────┘
```

| | |
|---|---|
| Sonuç | Kart kaybolur · **kalıcı** · ayrı bir "gizlediklerim" listesi **yok** |
| Kapsam | Sadece bu ilan — şirketin diğer ilanları görünmeye devam eder |
| İşveren | Hiçbir şey görmez · adayı görmeye ve davet etmeye devam edebilir |

> "Gizlenecek şirketler" (Özgeçmiş) ile karıştırılmasın: o, adayın bütün bir şirketten saklanmasıdır.

### 6.6 Liste durumları

| Durum | Ekranda |
|---|---|
| Hesaplanıyor | *"Sana uygun ilanlar hazırlanıyor..."* — birkaç saniye |
| Liste bitti | *"Sana uygun tüm ilanları gördün."* · *Yeni ilan gelince haber ver* **[Açık]** |
| Hiç uygun ilan yok | *"Şu an sana uygun ilan yok."* + **arama özeti:** *"Bilişim alanında, İstanbul'da, Türkçe, ofis ve hibrit iş ilanları"* · **[Arama tercihlerini düzenle]** → Özgeçmiş |
| Özgeçmiş yayında değil | *"İlanları görmek için özgeçmişini tamamla."* · tamamlanma yüzdesi · **[Özgeçmişe devam et]** |
| Şu an iş aramıyorum | Liste görünür · üstte "özgeçmişin gizli" bandı · aday bakabilir ve başvurabilir |

**Arama özeti neden önemli:** Aday yanlışlıkla tek konum seçmiş veya bir iş dilini kaldırmışsa sebebini burada görür.

### 6.7 Başvurduktan sonra

Kart Keşfet'ten **kalkar** → **Başvurularım → Bekleyen**. Aynı ilan bir daha Keşfet'e düşmez.

### 6.8 Akış

```
Keşfet
  │
  ├─ [Başvur] ────────────────────> Başvuru ekranı (9) ──> Başvurularım → Bekleyen
  │
  ├─ [İncele] / kartın boş yeri ──> İlan detayı (7) ──> [Başvur] ──> Başvuru ekranı (9)
  │
  ├─ Neden önerildi? ▾ ───────────> Kartın içinde açılır / kapanır
  │
  ├─ [Bir daha gösterme] ─────────> Onay ──> Kart kaybolur (kalıcı)
  │
  ├─ [Tümü] [İş] [Staj] ──────────> Liste süzülür
  ├─ Sırala ──────────────────────> Uyum · En yeni
  ├─ Aşağı çekip bırak ───────────> Liste yenilenir
  └─ [Daha fazla getir] ──────────> Yeniden hesaplanır
```

---

## 7. İlan Detayı

**Nereden:** Keşfet kartında **[İncele]** veya kartın boş yeri · paylaşılan bağlantı · şirket profilindeki **[İncele →]**

### 7.1 Ekran

```
┌──────────────────────────────────────────────────────────┐
│ ← Geri                                               [⋮] │
├──────────────────────────────────────────────────────────┤
│ İŞ İLANI                                  ● Yüksek uyum  │
│                                       [Neden önerildi?]  │
│ Senior Backend Developer                                 │
│ ABC Teknoloji  ✓ Doğrulanmış            [Şirket profili] │
│ %92 cevap verir · 12 işe alım                            │
│ İlan yayında · 3 gün önce yayınlandı                     │
│                                                          │
│ ── Pozisyon ──────────────────────────────────────────── │
│ ── Maaş ve başlama ───────────────────────────────────── │
│ ── Yetenekler ────────────────────────────────────────── │
│ ── Tecrübe ve eğitim ─────────────────────────────────── │
│ ── Sertifika ve belgeler ─────────────────────────────── │
│ ── Dil ───────────────────────────────────────────────── │
│ ── Diğer şartlar ─────────────────────────────────────── │
│ ── Diğer notlar ──────────────────────────────────────── │
│ ── İş tanımı ─────────────────────────────────────────── │
│ (her bölüm Bölüm 5.2'deki gibi, eksiksiz)                │
├──────────────────────────────────────────────────────────┤
│ [Bir daha gösterme]                          [Başvur]    │  ← altta sabit
└──────────────────────────────────────────────────────────┘
```

Staj ilanında gövde Bölüm 5.2'deki staj düzeninde.

### 7.2 Butonlar

| Buton | Yer | Ne olur |
|---|---|---|
| **[Neden önerildi?]** | Başlıkta | Açılır pencere *(5.3)* |
| **[Şirket profili]** | Başlıkta | Açılır pencere *(5.3)* |
| **[Başvur]** | Altta sabit, ana buton | Başvuru ekranı *(Bölüm 9)* |
| **[Bir daha gösterme]** | Altta sabit | Onay → Keşfet'e döner, ilan kalıcı olarak gizlenir |
| **[⋮]** | Sağ üst | **Paylaş** · **İlanı bildir** *(Bölüm 8)* |

**Butonlar neden altta sabit:** Aday ilanı okurken nerede olursa olsun başvurabilsin.

### 7.3 Alttaki bar — adayın bu ilanla geçmişi varsa

| Durum | Altta |
|---|---|
| Normal | **[Bir daha gösterme]** · **[Başvur]** |
| Zaten **başvurdu** | *"Başvurdun · 5 gün önce"* · **[Başvurularım →]** |
| Bu ilan için **davet aldı** | *"Bu ilan için davetin var"* · **[Daveti gör →]** |
| **Süreçte** | *"Bu ilan için süreçtesin"* · **[Sohbete git →]** |
| İlan **yayından kalktı** | *"Bu ilan şu an yayında değil"* · buton yok |
| İlan **sonuçlandı / silindi** | *"Bu ilan artık mevcut değil"* |

Son iki durum aday ilana paylaşılan bir bağlantıdan veya eski bir bildirimden geldiğinde olur.

### 7.4 Akış

```
İlan detayı
  ├─ [Neden önerildi?] ──> Açılır pencere
  ├─ [Şirket profili] ───> Açılır pencere ──> [İncele →] ──> Başka ilanın detayı
  ├─ [Başvur] ───────────> Başvuru ekranı (9)
  ├─ [Bir daha gösterme] ─> Onay ──> Keşfet
  ├─ [⋮] → Paylaş ───────> Telefonun paylaşma ekranı (8.1)
  ├─ [⋮] → İlanı bildir ─> Bildir penceresi (8.2)
  └─ ← Geri ─────────────> Geldiği ekran
```

---

## 8. Paylaş ve İlanı Bildir

**Nereden:** İş ve staj ilanının **her detayında** — ilan detayı, başvuru detayı, davet detayı, süreç detayı — sağ üstteki **[⋮]**.

```
┌──────────────────────────────────────────────────────────┐
│ ← Geri                                               [⋮] │
│                                       ┌────────────────┐ │
│                                       │ Paylaş         │ │
│                                       │ İlanı bildir   │ │
│                                       └────────────────┘ │
```

### 8.1 Paylaş

**Neden:** Aday ilanı bir arkadaşına göndermek isteyebilir — *"bu ilan tam sana göre"*. Platform için ücretsiz büyüme.

**Adım 1 — Telefonun kendi paylaşma ekranı açılır** (WhatsApp, Telegram, Mesajlar, e-posta, bağlantıyı kopyala…). Liste telefona göre değişir; biz sadece mesajı hazırlarız.

**Adım 2 — Giden mesaj:**

```
ABC Teknoloji · Senior Backend Developer
Kadıköy, İstanbul · Hibrit · Tam zamanlı
https://uygulama.com/ilan/58231
```

| Mesajda | Mesajda değil |
|---|---|
| Şirket · unvan · konum · çalışma şekli ve tipi · bağlantı | Maaş · adayın uyumu · adayın adı |

Bağlantı WhatsApp gibi uygulamalarda **önizleme kartı** olarak görünür: şirket logosu, unvan, konum.

**Adım 3 — Bağlantıyı açan kişi:**

| Kişi | Ne olur |
|---|---|
| Uygulaması var, giriş yapmış **aday** | Uygulamada **ilan detayı** |
| Uygulaması var, giriş yapmamış | Giriş ekranı → girince ilan detayı |
| **Uygulaması yok** | Web'de **ilan tanıtım sayfası** |
| **İşveren** hesabıyla girmiş | *"Bu ilan adaylara yöneliktir."* + ilanın kısa hali |

**Web tanıtım sayfası:**

```
┌──────────────────────────────────────────────────────────┐
│ [Uygulama logosu]                                        │
├──────────────────────────────────────────────────────────┤
│ İŞ İLANI                                                 │
│ Senior Backend Developer                                 │
│ ABC Teknoloji  ✓ Doğrulanmış                             │
│                                                          │
│ Kadıköy, İstanbul · Hibrit · Tam zamanlı                 │
│ 3 gün önce yayınlandı                                    │
│                                                          │
│ İş tanımı                                                │
│ Ödeme altyapımızda mikroservisler geliştirecek,          │
│ milyonlarca işlemi taşıyan sistemlerin performansından   │
│ sorumlu olacaksınız...                                   │
│                                                          │
│ İlanın tamamını görmek ve başvurmak için kaydol.         │
│                                                          │
│          [Kaydol ve başvur]      [Giriş yap]             │
│                                                          │
│  [App Store'dan indir]    [Google Play'den indir]        │
└──────────────────────────────────────────────────────────┘
```

| | |
|---|---|
| Görünen | Unvan, şirket, konum, çalışma şekli ve tipi, yayın tarihi, iş tanımının başı |
| Görünmeyen | Maaş, aranan nitelikler, şirket raporu |
| **[Kaydol ve başvur]** | Kayıt → özgeçmiş → yayınlanınca bu ilan açılır |
| İlan yayında değilse | *"Bu ilan şu an yayında değil."* + **[Diğer ilanları keşfet]** |
| İlan sonuçlandı / silindi | *"Bu ilan artık mevcut değil."* + **[Diğer ilanları keşfet]** |

**Neden tamamı açık değil:** İlanlar dışarıdan toplanıp başka sitelere kopyalanmasın; kısa hali kayda yönlendirir.

**Ölçüm:** Paylaşım sayısı ve bağlantıdan gelen kayıtlar sayılır (admin istatistiği). **Kimin kimle paylaştığı tutulmaz.**

### 8.2 İlanı bildir

**Neden:** Sahte ilan ve *"işe başlamak için eğitim ücreti ödeyin"* gibi dolandırıcılıklar iş platformlarının en büyük güven sorunu. Adaylar en hızlı alarm.

**Adım 1 — Form:**

```
┌──────────────────────────────────────────────────────────┐
│                         ────                             │
│ İlanı bildir                                         [✕] │
├──────────────────────────────────────────────────────────┤
│ Senior Backend Developer · ABC Teknoloji                 │
│                                                          │
│ Sorun ne? *                                              │
│ ○ Sahte ilan — böyle bir iş yok                          │
│ ○ Benden para istendi                                    │
│ ○ Kişisel bilgi veya belge istendi                       │
│ ○ Ayrımcı veya uygunsuz ifade var                        │
│ ○ Yanlış veya yanıltıcı bilgi                            │
│ ○ Diğer                                                  │
│                                                          │
│ Açıklama (opsiyonel)                                     │
│ ┌──────────────────────────────────────────────────────┐ │
│ │                                                      │ │
│ └──────────────────────────────────────────────────────┘ │
│                                                   0/500  │
│                                                          │
│ Bildirimin gizli tutulur. İşveren kimin                  │
│ bildirdiğini göremez.                                    │
│                                                          │
│ [Vazgeç]                                   [Bildir]      │
└──────────────────────────────────────────────────────────┘
```

| Seçenek | Örnek |
|---|---|
| Sahte ilan | Böyle bir pozisyon yok, ilan kopya |
| **Benden para istendi** | *"Eğitim ücreti", "dosya masrafı", "kıyafet parası"* |
| Kişisel bilgi veya belge istendi | Kimlik fotokopisi, banka bilgisi — platform dışında |
| Ayrımcı veya uygunsuz ifade | *"Sadece erkek adaylar"*, yaş sınırı, hakaret |
| Yanlış veya yanıltıcı bilgi | Maaş, konum, iş tanımı gerçeği yansıtmıyor |
| Diğer | Açıklama **zorunlu** olur |

| Kural | |
|---|---|
| Sorun seçimi | Zorunlu · [Bildir] seçilince aktif |
| Açıklama | Opsiyonel · "Diğer"de zorunlu · en fazla 500 karakter |

**Adım 2 — Gönderince:**

```
┌──────────────────────────────────────────────────────────┐
│ Bildirimin için teşekkürler.                             │
│                                                          │
│ İlanı inceleyeceğiz. Gerekirse kaldıracağız.             │
│                                                          │
│ Bu ilanı bir daha görmek ister misin?                    │
│                                                          │
│ [Görmeye devam et]          [Bir daha gösterme]          │
└──────────────────────────────────────────────────────────┘
```

**Adım 3 — Tekrar açarsa:** [⋮] menüsünde *"Bildirildi ✓"* — gri, dokunulmaz.

| Nereye gider | |
|---|---|
| **Admin** | İnceleme kuyruğu: ilan, şirket, sorun, açıklama, tarih |
| **Öncelik** | *"Benden para istendi"* ve *"Kişisel bilgi istendi"* → **acil** · kısa sürede çok bildirim → **acil** |
| **İşveren** | Bildirimden haberi olmaz · kim bildirdiğini hiçbir zaman göremez |
| **Admin kararı** | İlanı kaldırır · şirkete uyarı · hesabı askıya alır · yersizse kapatır |
| **Adaya sonuç** | İlan kaldırıldıysa 🔔 *"Bildirdiğin ilan incelendi ve kaldırıldı. Teşekkürler."* · işlem gerekmediyse bildirim yok |
| **Süreçteki adaylar** | İlan kaldırılırsa o ilanda süreçte olan herkese uyarı (Bölüm 12.6, bildirim 5.6) |

| Kötüye kullanım | |
|---|---|
| Aynı aday aynı ilan | Bir kez |
| Bir aday günde | En fazla 10 |
| Sürekli yersiz bildiren | Bildirimleri düşük öncelikli olur |

---
## 9. Başvuru Ekranı

**Nereden:** Keşfet kartındaki veya ilan detayındaki **[Başvur]**

Özgeçmiş zaten hazır; burada sadece **bu ilana özel** bilgiler sorulur.

### 9.1 Ekran

```
┌──────────────────────────────────────────────────────────┐
│ ← Geri                  Başvuru                          │
├──────────────────────────────────────────────────────────┤
│ Senior Backend Developer                                 │
│ ABC Teknoloji · Kadıköy, İstanbul                        │
│ Maaş: 80.000 – 110.000 TRY / ay (net)                    │
│                                                          │
│ ── Maaş beklentin (opsiyonel) ────────────────────────── │
│ Net, aylık, en az:                                       │
│ [  90.000        ]  [TRY ▾]                              │
│                                                          │
│ ── İşverene not (opsiyonel) ──────────────────────────── │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ Yan haklara göre maaşta esnek olabilirim.            │ │
│ └──────────────────────────────────────────────────────┘ │
│                                                 43/300   │
│                                                          │
│ ── Paylaşılacak belgeler ─────────────────────────────── │
│ Belgeler                                                 │
│   ☑ Özgeçmiş                                    PDF      │
│   ☑ Portfolyo                                   PDF      │
│   ☑ Referans mektubu                            JPG      │
│ Sertifikalar                                             │
│   ☑ SRC belgesi                                 PDF      │
│   ☑ İSG eğitimi        (süresi geçmiş)          PDF      │
│                                                          │
│ (i) Özgeçmişin ve paylaştığın belgeler işveren           │
│     tarafından görüntülenebilir ve indirilebilir.        │
│                                                          │
│ [Özgeçmişimi önizle]                                     │
├──────────────────────────────────────────────────────────┤
│ [Vazgeç]                                     [Başvur]    │
└──────────────────────────────────────────────────────────┘
```

### 9.2 Alanlar

| Bölüm | Kural |
|---|---|
| **İlan özeti** | Unvan, şirket, konum, ilanın maaşı — aday neye başvurduğunu ve ilanın maaşını görerek yazar |
| **Maaş beklentin** | Opsiyonel · **net, aylık, en az** · tek rakam · para birimi varsayılan **ilanın para birimi**, değiştirilebilir · kur çevrimi yok |
| **İşverene not** | Opsiyonel · en fazla **300** karakter · telefon, e-posta, bağlantı **yazılamaz** → *"İletişim bilgilerini süreç başladıktan sonra paylaşabilirsin."* |
| **Paylaşılacak belgeler** | Özgeçmiş'teki belgeler (adayın verdiği adlarla) ve sertifika dosyaları, iki grup · varsayılan **hepsi işaretli** · aday istediğini kaldırır · hiç belgesi yoksa bölüm görünmez · süresi geçmiş sertifika da listede, yanında not |
| **Bilgi notu** | Her zaman görünür |
| **[Özgeçmişimi önizle]** | İşverenin göreceği özgeçmiş — açılır pencere |

**Maaş neden "en az":** Tek rakam, işveren bunu alt sınır olarak okur. Aralık sormak adayı zorlar, işverene net bilgi vermez.

**Belge seçimi neden var:** Mevcut işvereninden çekinen aday referans mektubunu her şirketle paylaşmak istemeyebilir; farklı pozisyonlar için farklı özgeçmiş yükleyen aday doğrusunu seçer.

### 9.3 [Başvur]'a basınca — kontroller

| Kontrol | Sorun varsa |
|---|---|
| **Telefon doğrulanmış mı** | *"Devam etmek için telefonunu doğrula"* → SMS → başvuru **kaldığı yerden** tamamlanır |
| **Gizlediğin şirket mi** | Uyarı penceresi ↓ |
| **İlan hâlâ yayında mı** | *"Bu ilan az önce yayından kalktı."* — başvuru gitmez |
| **Daha önce başvurdun mu** | *"Bu ilana zaten başvurdun."* → Başvurularım |

**Gizlediğin şirkete başvururken:**

```
┌──────────────────────────────────────────────┐
│ ABC Teknoloji'yi gizlediğin şirketler        │
│ arasına eklemişsin.                          │
│                                              │
│ Başvurursan bu şirket özgeçmişini            │
│ görebilecek.                                 │
│                                              │
│ [Vazgeç]                [Yine de başvur]     │
└──────────────────────────────────────────────┘
```

### 9.4 Başvuru gidince

```
┌──────────────────────────────────────────────────────────┐
│                                                          │
│                Başvurun gönderildi                       │
│                                                          │
│  ABC Teknoloji başvurunu değerlendirecek.                │
│  Sonuç gelince haber vereceğiz.                          │
│                                                          │
│  [Keşfet'e dön]                [Başvurularım →]          │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

| | |
|---|---|
| İlan | Keşfet'ten kalkar → **Başvurularım → Bekleyen** |
| İşverene | 📱 🔔 *"Senior Backend Developer ilanınıza yeni başvuru: Ayşe Kaya"* |
| Kayda geçen | Maaş beklentisi · not · paylaşılan belgeler · tarih · **başvuru anındaki uyum** ("Neden önerildi?" bu kayıttan gösterilir) |
| Süre | İşveren **30 gün** içinde cevap vermezse başvuru sona erer |

### 9.5 Akış

```
[Başvur] (Keşfet kartı veya ilan detayı)
   │
   v
Başvuru ekranı
   ├─ [Özgeçmişimi önizle] ──> Açılır pencere
   ├─ [Vazgeç] / ← Geri ─────> Geldiği ekran · hiçbir şey gönderilmez
   └─ [Başvur]
        │
        ├─ Telefon doğrulanmamış ──> SMS ──> kaldığı yerden devam
        ├─ Gizlenen şirket ────────> Uyarı ──> [Yine de başvur] / [Vazgeç]
        ├─ İlan yayından kalkmış ──> Hata mesajı
        ├─ Zaten başvurmuş ────────> Başvurularım
        │
        └─ Başarılı ──> "Başvurun gönderildi"
                          ├─ [Keşfet'e dön]
                          └─ [Başvurularım →]
```

---

## 10. Başvurularım

### 10.1 Bekleyen — liste

```
┌──────────────────────────────────────────────────────────┐
│ [● Aktif iş arıyorum ▾]          (zil 3) (ayarlar) (AK)  │
├──────────────────────────────────────────────────────────┤
│ ┌──────────────┬──────────────┐                          │
│ │▸ Bekleyen 3  │ Sonuçlanan 6 │                          │
│ └──────────────┴──────────────┘                          │
├──────────────────────────────────────────────────────────┤
│ ┌──────────────────────────────────────────────────────┐ │
│ │ İŞ İLANI                              ● Yüksek uyum  │ │
│ │ Senior Backend Developer                             │ │
│ │ ABC Teknoloji  ✓ Doğrulanmış                         │ │
│ │ İlan yayında                                         │ │
│ │                                                      │ │
│ │ ~ Değerlendiriliyor                                  │ │
│ │ 12 Eylül'de başvurdun · 5 gün önce                   │ │
│ │                                                      │ │
│ │ [Başvurumu geri çek]                    [Detay →]    │ │
│ └──────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ STAJ İLANI                              ○ Orta uyum  │ │
│ │ Yazılım Stajyeri                                     │ │
│ │ XYZ Yazılım  ✓ Doğrulanmış                           │ │
│ │ İlan geçici olarak durduruldu                        │ │
│ │                                                      │ │
│ │ ~ Değerlendiriliyor                                  │ │
│ │ 5 Eylül'de başvurdun · 12 gün önce                   │ │
│ │                                                      │ │
│ │ [Başvurumu geri çek]                    [Detay →]    │ │
│ └──────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ İŞ İLANI                                ○ Orta uyum  │ │
│ │ Veri Analisti                                        │ │
│ │ MNO Finans  ✓ Doğrulanmış                            │ │
│ │ İlanın yayın süresi doldu                            │ │
│ │                                                      │ │
│ │ ~ Değerlendiriliyor                                  │ │
│ │ 28 Ağustos'ta başvurdun · 20 gün önce                │ │
│ │                                                      │ │
│ │ [Başvurumu geri çek]                    [Detay →]    │ │
│ └──────────────────────────────────────────────────────┘ │
├──────────────────────────────────────────────────────────┤
│ Keşfet  Başvurularım  Davetlerim  Süreçteki  Özgeçmiş    │
│              ▔▔▔▔▔▔▔▔                                    │
└──────────────────────────────────────────────────────────┘
```

| Kartta | |
|---|---|
| **İlan başlığı** *(5.1)* | İlan tipi · uyum *(başvuru anındaki)* · unvan · şirket · **ilanın durumu** |
| **Başvurun** | ~ Değerlendiriliyor · ne zaman başvurdun |

İlan yayında, durdurulmuş veya süresi dolmuş olsa da başvuru **geçerli** — işveren cevap verebilir.

**Kartta bilerek olmayanlar:**

| | Neden |
|---|---|
| Kalan gün (30 günden) | Karar işverende — adaya stres yaratmasın |
| *"İşveren görüntüledi"* | Görüntüleyip cevap vermeyen işveren adayı daha çok üzer |

| Buton | Ne olur |
|---|---|
| **[Detay →]** / kartın boş yeri | Başvuru detayı *(10.2)* |
| **[Başvurumu geri çek]** | Onay penceresi *(10.3)* |

| Kural | |
|---|---|
| Sıra | En yeni başvuru üstte |
| Hiç bekleyen yoksa | *"Bekleyen başvurun yok."* · **[Keşfet'e git]** |

### 10.2 Başvuru detayı

Üstte **BAŞVURUN** bölümü, altında ilanın tamamı.

```
┌──────────────────────────────────────────────────────────┐
│ ← Geri                Başvurum                       [⋮] │
├──────────────────────────────────────────────────────────┤
│ ┌──────────────────────────────────────────────────────┐ │
│ │ BAŞVURUN                                             │ │
│ │ ~ Değerlendiriliyor                                  │ │
│ │ 12 Eylül'de başvurdun · 5 gün önce                   │ │
│ │                                                      │ │
│ │ [Gönderdiğin bilgiler]  [Neden önerildi?]            │ │
│ │ [Şirket profili]                                     │ │
│ └──────────────────────────────────────────────────────┘ │
│                                                          │
│ İŞ İLANI                                  ● Yüksek uyum  │
│ Senior Backend Developer                                 │
│ ABC Teknoloji  ✓ Doğrulanmış                             │
│ %92 cevap verir · 12 işe alım                            │
│ İlan yayında · 3 gün önce yayınlandı                     │
│                                                          │
│ ── Pozisyon ──────────────────────────────────────────── │
│ ── Maaş ve başlama ───────────────────────────────────── │
│ ── Yetenekler ────────────────────────────────────────── │
│ ── Tecrübe ve eğitim ─────────────────────────────────── │
│ ── Sertifika ve belgeler ─────────────────────────────── │
│ ── Dil ───────────────────────────────────────────────── │
│ ── Diğer şartlar ─────────────────────────────────────── │
│ ── Diğer notlar ──────────────────────────────────────── │
│ ── İş tanımı ─────────────────────────────────────────── │
│ (her bölüm Bölüm 5.2'deki gibi, eksiksiz)                │
├──────────────────────────────────────────────────────────┤
│ [Başvurumu geri çek]                                     │
└──────────────────────────────────────────────────────────┘
```

| Parça | Ne |
|---|---|
| **BAŞVURUN** | Durum · başvuru tarihi · üç açılır pencere |
| **[Gönderdiğin bilgiler]** | Maaş beklentisi · not · paylaşılan belgeler *(5.3)* |
| **[Neden önerildi?]** | **Başvuru anındaki** kayıttan *(5.3)* |
| **[Şirket profili]** | *(5.3)* |
| İlan | Başlık + gövde · ilanın **güncel** hali |
| İlan başvurudan sonra değiştiyse | İlan başlığının altında: *"İşveren ilanı başvurundan sonra güncelledi."* |
| İlan silindiyse | BAŞVURUN kalır · altında *"Bu ilan artık mevcut değil"* |
| Altta sabit | **[Başvurumu geri çek]** — sadece bekleyende |
| [⋮] | Paylaş · İlanı bildir |

### 10.3 Başvurumu geri çek

```
┌──────────────────────────────────────────────┐
│ Başvurunu geri çekmek istediğine             │
│ emin misin?                                  │
│                                              │
│ Bu ilana tekrar başvuramazsın.               │
│                                              │
│ [Vazgeç]                   [Geri çek]        │
└──────────────────────────────────────────────┘
```

| Sonra | |
|---|---|
| Kart | **Sonuçlanan** → *"<- Başvurunu geri çektin"* |
| İşverene | 🔔 *"Ayşe Kaya başvurusunu geri çekti."* · belgelere erişimi kapanır |
| Tekrar | Aynı ilana **tekrar başvurulamaz** |
| Şirket raporu | Geri çekilen başvuru **cevapsız sayılmaz** |

### 10.4 Sonuçlanan — liste

```
┌──────────────────────────────────────────────────────────┐
│ [● Aktif iş arıyorum ▾]          (zil 3) (ayarlar) (AK)  │
├──────────────────────────────────────────────────────────┤
│ ┌──────────────┬──────────────┐                          │
│ │  Bekleyen 3  │▸Sonuçlanan 6 │                          │
│ └──────────────┴──────────────┘                          │
├──────────────────────────────────────────────────────────┤
│ ┌──────────────────────────────────────────────────────┐ │
│ │ İŞ İLANI                                ○ Orta uyum  │ │
│ │ Frontend Developer                                   │ │
│ │ DEF Yazılım  ✓ Doğrulanmış                           │ │
│ │ İlan yayında                                         │ │
│ │                                                      │ │
│ │ ✕ Başka adaylarla ilerlendi                          │ │
│ │ 2 Eylül'de başvurdun · 18 Eylül'de sonuçlandı        │ │
│ │                                                      │ │
│ │ [Listeden kaldır]                       [Detay →]    │ │
│ └──────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ İŞ İLANI                              ● Yüksek uyum  │ │
│ │ Muhasebe Uzmanı                                      │ │
│ │ GHI Holding  ✓ Doğrulanmış                           │ │
│ │ İlan kapandı                                         │ │
│ │                                                      │ │
│ │ ■ Pozisyon doldu                                     │ │
│ │ 20 Ağustos'ta başvurdun · 10 Eylül'de sonuçlandı     │ │
│ │                                                      │ │
│ │ [Listeden kaldır]                       [Detay →]    │ │
│ └──────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ STAJ İLANI                              ○ Orta uyum  │ │
│ │ Pazarlama Stajyeri                                   │ │
│ │ JKL Medya  ✓ Doğrulanmış                             │ │
│ │ İlanın yayın süresi doldu                            │ │
│ │                                                      │ │
│ │ ~ Yanıtlanmadan sona erdi                            │ │
│ │ 1 Ağustos'ta başvurdun · 31 Ağustos'ta sonuçlandı    │ │
│ │                                                      │ │
│ │ [Listeden kaldır]                       [Detay →]    │ │
│ └──────────────────────────────────────────────────────┘ │
│                                                          │
│ Son 90 gündeki sonuçlanan başvuruların gösteriliyor.     │
├──────────────────────────────────────────────────────────┤
│ Keşfet  Başvurularım  Davetlerim  Süreçteki  Özgeçmiş    │
│              ▔▔▔▔▔▔▔▔                                    │
└──────────────────────────────────────────────────────────┘
```

### 10.5 Başvurunun yedi sonucu

| Ne oldu | Kim | Kartta | Adaya bildirim |
|---|---|---|---|
| İşveren kabul etmedi | İşveren | ✕ **Başka adaylarla ilerlendi** | 1.2 |
| İşveren sonuç bildirdi — pozisyon doldu | İşveren | ■ **Pozisyon doldu** | 5.1 |
| İşveren sonuç bildirdi — alımdan vazgeçti | İşveren | ■ **Pozisyon iptal edildi** | 5.2 |
| İşveren sonuç bildirdi — uygun aday bulamadı | İşveren | ■ **Süreç sona erdi** | 5.3 |
| İşveren 30 gün cevap vermedi | Süre | ~ **Yanıtlanmadan sona erdi** | 1.3 |
| Aday geri çekti | Aday | <- **Başvurunu geri çektin** | — |
| Şirket hesabını sildi | Şirket | ■ **Şirket platformdan ayrıldı** | 5.5 |

*(Bildirim numaraları → Bölüm 14.3 katalog)*

**Kabul edilen başvuru burada yok** — doğrudan **Süreçteki İlanlar**'a geçer (bildirim 1.1).

**Neden mesajlar farklı:** *Pozisyon doldu* ve *iptal* dürüstçe söylenir — kişisel bir ret değil. *Uygun aday bulunamadı* **nötr** kalır — aday *"ben uygun değildim"* diye okumasın.

### 10.6 Sonuçlanan — butonlar ve detay

| Buton | Ne olur |
|---|---|
| **[Detay →]** / kartın boş yeri | Başvuru detayı — BAŞVURUN bölümünde **sonuç** yazar (*"✕ Başka adaylarla ilerlendi · 18 Eylül'de sonuçlandı"*) · üç açılır pencere aynı · **altta buton yok** |
| **[Listeden kaldır]** | Onaysız · kart kalkar · kayıt silinmez |

| Kural | |
|---|---|
| Süre | Son **90 gün**, sonra kendiliğinden kalkar |
| Tekrar başvuru | Sonuçlanan ilana tekrar başvurulamaz |
| Şirket silindiyse | [Şirket profili] butonu yok · [Detay →] yok |
| Sıra | En yeni sonuç üstte |
| Başvurularım rozeti (•) | Yeni sonuç gelince · Sonuçlanan açılınca kaybolur |
| Hiç yoksa | *"Henüz sonuçlanan başvurun yok."* |

### 10.7 Akış

```
Başvurularım
  │
  ├─ Bekleyen
  │    ├─ [Detay →] / kart ──> Başvuru detayı
  │    │                         ├─ [Gönderdiğin bilgiler] ──> Açılır pencere
  │    │                         ├─ [Neden önerildi?] ───────> Açılır pencere
  │    │                         ├─ [Şirket profili] ────────> Açılır pencere
  │    │                         ├─ [⋮] ─────────────────────> Paylaş · İlanı bildir
  │    │                         └─ [Başvurumu geri çek] ────> Onay
  │    └─ [Başvurumu geri çek] ──> Onay ──> Sonuçlanan
  │
  ├─ İşveren kabul etti ──────────────────> Süreçteki İlanlar (Bölüm 12)
  │
  └─ Sonuçlanan
       ├─ [Detay →] / kart ──> Başvuru detayı (sonuç yazar, altta buton yok)
       └─ [Listeden kaldır] ─> Kart kalkar
```

---

## 11. Davetlerim

**Başvurularım'dan farkı:** Burada **karar adayda.** İşveren adayı davet etti; aday kabul edecek ya da etmeyecek.

### 11.1 Davet notu

İşveren davet ederken **isterse** not yazar:

| | |
|---|---|
| Zorunlu mu | Hayır |
| Uzunluk | En fazla 300 karakter |
| Yasak | Telefon, e-posta, bağlantı |
| Aday kabul ederse | Not **sohbetin ilk mesajı** olur |

| Yer | Notun gösterimi |
|---|---|
| Davet kartı | İlk **2 satır** · uzunsa *"…devamı"* |
| Davet detayı | Tamamı |
| Daveti kabul et ekranı | Tamamı, en üstte |
| Sohbet | İlk mesaj |

Not yazılmadıysa o satır hiç görünmez.

### 11.2 Bekleyen — liste

```
┌──────────────────────────────────────────────────────────┐
│ [● Aktif iş arıyorum ▾]          (zil 3) (ayarlar) (AK)  │
├──────────────────────────────────────────────────────────┤
│ ┌──────────────┬──────────────┐                          │
│ │▸ Bekleyen 2  │ Sonuçlanan 3 │                          │
│ └──────────────┴──────────────┘                          │
├──────────────────────────────────────────────────────────┤
│ ┌──────────────────────────────────────────────────────┐ │
│ │ İŞ İLANI                              ● Yüksek uyum  │ │
│ │ Backend Developer                                    │ │
│ │ PRS Teknoloji  ✓ Doğrulanmış                         │ │
│ │ İlan yayında                                         │ │
│ │                                                      │ │
│ │ Seni davet etti · 2 gün önce                         │ │
│ │ "Profilinizi çok beğendik, ödeme altyapısı           │ │
│ │  deneyiminiz tam aradığımız. Görüşmek isteriz."      │ │
│ │ 8 gün içinde cevap ver                               │ │
│ │                                                      │ │
│ │ [Kabul etme]        [Detay]          [Kabul et]      │ │
│ └──────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ STAJ İLANI                              ○ Orta uyum  │ │
│ │ Veri Stajyeri                                        │ │
│ │ TUV Analitik  ✓ Doğrulanmış                          │ │
│ │ İlan yayında                                         │ │
│ │                                                      │ │
│ │ Seni davet etti · 9 gün önce                         │ │
│ │ ! 1 gün içinde cevap ver                             │ │
│ │                                                      │ │
│ │ [Kabul etme]        [Detay]          [Kabul et]      │ │
│ └──────────────────────────────────────────────────────┘ │
├──────────────────────────────────────────────────────────┤
│ Keşfet  Başvurularım  Davetlerim  Süreçteki  Özgeçmiş    │
│                          ▔▔▔▔▔▔▔                         │
└──────────────────────────────────────────────────────────┘
```

| Kartta | |
|---|---|
| **İlan başlığı** *(5.1)* | İlan tipi · uyum *(davet anındaki)* · unvan · şirket · ilanın durumu |
| **Davetin** | *"Seni davet etti"* + ne zaman · işverenin notu · **cevap süresi** |

**Cevap süresi adaya gösterilir:** *"8 gün içinde cevap ver"* · 2 gün ve altında **! 1 gün içinde cevap ver**. Başvuruda kalan gün gösterilmiyordu çünkü karar işverendeydi; davette karar **adayda**, süreyi bilmezse daveti kaçırır.

| Buton | Ne olur |
|---|---|
| **[Kabul et]** | **Daveti kabul et** ekranı *(11.5)* |
| **[Detay]** / kartın boş yeri | Davet detayı *(11.4)* |
| **[Kabul etme]** | Onay penceresi *(11.3)* |

| Kural | |
|---|---|
| Sıra | **Süresi en az kalan üstte** — kaçırılmasın |
| Süre | **10 gün** · cevap verilmezse sona erer |
| Rozet | Alt menüde bekleyen davet sayısı |
| Hiç yoksa | *"Bekleyen davetin yok. Özgeçmişin ne kadar dolu olursa işverenler seni o kadar kolay bulur."* · **[Özgeçmişime git]** |
| Şu an iş aramıyorum iken | Yeni davet gelmez · gelmiş davetler durur, cevap verilebilir |

### 11.3 Kabul etme

```
┌──────────────────────────────────────────────┐
│ PRS Teknoloji'nin davetini kabul etmek       │
│ istemediğine emin misin?                     │
│                                              │
│ İşverene "davetini kabul etmedi" bilgisi     │
│ gidecek.                                     │
│                                              │
│ [Vazgeç]                  [Kabul etme]       │
└──────────────────────────────────────────────┘
```

| Sonra | |
|---|---|
| Kart | **Sonuçlanan** → *"✕ Daveti kabul etmedin"* |
| İşverene | 📱 🔔 *"Ayşe Kaya davetinizi kabul etmedi."* |
| İlan | Adayın Keşfet'ine bir daha düşmez |
| Tekrar | Bu ilandan bu adaya tekrar davet gelmez |
| Sebep | **Sorulmaz** |

### 11.4 Davet detayı

Üstte **DAVETİN** bölümü, altında ilanın tamamı. **[Gönderdiğin bilgiler] yok** — aday henüz bir şey göndermedi.

```
┌──────────────────────────────────────────────────────────┐
│ ← Geri                Davetim                        [⋮] │
├──────────────────────────────────────────────────────────┤
│ ┌──────────────────────────────────────────────────────┐ │
│ │ DAVETİN                                              │ │
│ │ PRS Teknoloji seni davet etti · 2 gün önce           │ │
│ │ 8 gün içinde cevap ver                               │ │
│ │                                                      │ │
│ │ İşverenin notu                                       │ │
│ │ "Profilinizi çok beğendik, ödeme altyapısı           │ │
│ │  deneyiminiz tam aradığımız. Görüşmek isteriz."      │ │
│ │                                                      │ │
│ │ [Neden önerildi?]       [Şirket profili]             │ │
│ └──────────────────────────────────────────────────────┘ │
│                                                          │
│ İŞ İLANI                                  ● Yüksek uyum  │
│ Backend Developer                                        │
│ PRS Teknoloji  ✓ Doğrulanmış                             │
│ %88 cevap verir · 6 işe alım                             │
│ İlan yayında · 1 hafta önce yayınlandı                   │
│                                                          │
│ ── Pozisyon ──────────────────────────────────────────── │
│ (her bölüm Bölüm 5.2'deki gibi, eksiksiz)                │
├──────────────────────────────────────────────────────────┤
│ [Kabul etme]                                 [Kabul et]  │
└──────────────────────────────────────────────────────────┘
```

| Parça | Ne |
|---|---|
| Kim, ne zaman | *"PRS Teknoloji seni davet etti · 2 gün önce"* |
| Cevap süresi | 2 gün ve altında **!** |
| İşverenin notu | Tamamı · yoksa görünmez |
| **[Neden önerildi?]** | **Davet anındaki** kayıttan |
| **[Şirket profili]** | |
| Altta sabit | **[Kabul etme]** · **[Kabul et]** |
| [⋮] | Paylaş · İlanı bildir |

### 11.5 Daveti kabul et

**Nereden:** Davet kartında veya davet detayında **[Kabul et]**

Başvuru ekranının **aynısı** — aday ilk kez bilgi gönderiyor. Tek fark: en üstte işverenin notu.

```
┌──────────────────────────────────────────────────────────┐
│ ← Geri               Daveti kabul et                     │
├──────────────────────────────────────────────────────────┤
│ Backend Developer                                        │
│ PRS Teknoloji · Maslak, İstanbul                         │
│ Maaş: 75.000 – 95.000 TRY / ay (net)                     │
│                                                          │
│ İşverenin notu                                           │
│ "Profilinizi çok beğendik, ödeme altyapısı deneyiminiz   │
│  tam aradığımız. Görüşmek isteriz."                      │
│                                                          │
│ ── Maaş beklentin (opsiyonel) ────────────────────────── │
│ Net, aylık, en az:                                       │
│ [  85.000        ]  [TRY ▾]                              │
│                                                          │
│ ── İşverene not (opsiyonel) ──────────────────────────── │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ Teşekkürler, görüşmek isterim.                       │ │
│ └──────────────────────────────────────────────────────┘ │
│                                                 30/300   │
│                                                          │
│ ── Paylaşılacak belgeler ─────────────────────────────── │
│ Belgeler                                                 │
│   ☑ Özgeçmiş                                    PDF      │
│   ☑ Portfolyo                                   PDF      │
│ Sertifikalar                                             │
│   ☑ SRC belgesi                                 PDF      │
│                                                          │
│ (i) Özgeçmişin ve paylaştığın belgeler işveren           │
│     tarafından görüntülenebilir ve indirilebilir.        │
│                                                          │
│ [Özgeçmişimi önizle]                                     │
├──────────────────────────────────────────────────────────┤
│ [Vazgeç]                                   [Kabul et]    │
└──────────────────────────────────────────────────────────┘
```

| | Başvuru ekranı | Daveti kabul et |
|---|---|---|
| Başlık | Başvuru | **Daveti kabul et** |
| En üstte | İlan özeti | İlan özeti + **işverenin notu** |
| Maaş, not, belgeler | Aynı kurallar *(9.2)* | Aynı kurallar |
| Buton | [Başvur] | **[Kabul et]** |
| Sonra | Başvurularım → Bekleyen *(işveren karar verecek)* | **Süreçteki İlanlar** *(süreç hemen başlar)* |

**Neden davette de soruluyor:** İşveren adayı davet ederken sadece özgeçmişini gördü; maaş beklentisini ve belgelerini bilmiyor. Aday kabul ederken bunları **ilk kez** paylaşıyor ve neyi paylaşacağına kendisi karar veriyor.

**[Kabul et]'e basınca — kontroller:**

| Kontrol | Sorun varsa |
|---|---|
| Telefon doğrulanmış mı | SMS → kabul kaldığı yerden tamamlanır |
| Davet hâlâ geçerli mi | *"Bu davetin süresi doldu."* veya *"İşveren davetini geri çekti."* — kabul olmaz |

| | |
|---|---|
| İlan yayında değilse | Davet **yine kabul edilebilir** — yayından kalkan ilanda içerideki işler devam eder |
| Gizlenen şirket kontrolü | **Yok** — gizlenen şirket adayı göremez, davet de edemez |

**Kabul edilince:**

```
┌──────────────────────────────────────────────────────────┐
│                                                          │
│                 Daveti kabul ettin                       │
│                                                          │
│  PRS Teknoloji ile görüşme süreci başladı.               │
│  Sohbetten yazışabilirsin.                               │
│                                                          │
│  [Davetlerime dön]                 [Sohbete git →]       │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

| | |
|---|---|
| Davet | Davetlerim'den kalkar → **Süreçteki İlanlar → Devam eden** |
| Sohbet | Açılır · **işverenin davet notu ilk mesaj**, **adayın notu ikinci mesaj** · ikisi de yoksa boş açılır |
| İşverene | 📱 🔔 *"Ayşe Kaya davetinizi kabul etti. Görüşmeye başlayabilirsiniz."* |
| İşveren görür | Maaş beklentisi · not · paylaşılan belgeler · özgeçmiş |
| Süre | Süreç **60 gün** başlar |

**[Vazgeç]** veya **← Geri** → hiçbir şey gönderilmez · davet Bekleyen'de kalır · süre işlemeye devam eder.

### 11.6 Sonuçlanan — liste

```
┌──────────────────────────────────────────────────────────┐
│ [● Aktif iş arıyorum ▾]          (zil 3) (ayarlar) (AK)  │
├──────────────────────────────────────────────────────────┤
│ ┌──────────────┬──────────────┐                          │
│ │  Bekleyen 2  │▸Sonuçlanan 3 │                          │
│ └──────────────┴──────────────┘                          │
├──────────────────────────────────────────────────────────┤
│ ┌──────────────────────────────────────────────────────┐ │
│ │ İŞ İLANI                                ○ Orta uyum  │ │
│ │ Satış Temsilcisi                                     │ │
│ │ WXY Ticaret  ✓ Doğrulanmış                           │ │
│ │ İlan yayında                                         │ │
│ │                                                      │ │
│ │ ✕ Daveti kabul etmedin                               │ │
│ │ 3 Eylül'de davet edildin · 5 Eylül'de sonuçlandı     │ │
│ │                                                      │ │
│ │ [Listeden kaldır]                       [Detay →]    │ │
│ └──────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ STAJ İLANI                              ○ Orta uyum  │ │
│ │ İK Stajyeri                                          │ │
│ │ ZAB Danışmanlık  ✓ Doğrulanmış                       │ │
│ │ İlan yayında                                         │ │
│ │                                                      │ │
│ │ ~ Cevap vermediğin için sona erdi                    │ │
│ │ 20 Ağustos'ta davet edildin · 30 Ağustos'ta bitti    │ │
│ │                                                      │ │
│ │ [Listeden kaldır]                       [Detay →]    │ │
│ └──────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ İŞ İLANI                              ● Yüksek uyum  │ │
│ │ Yazılım Mühendisi                                    │ │
│ │ CDE Teknoloji  ✓ Doğrulanmış                         │ │
│ │ İlan yayında                                         │ │
│ │                                                      │ │
│ │ <- İşveren davetini geri çekti                       │ │
│ │ 15 Ağustos'ta davet edildin · 18 Ağustos'ta bitti    │ │
│ │                                                      │ │
│ │ [Listeden kaldır]                       [Detay →]    │ │
│ └──────────────────────────────────────────────────────┘ │
│                                                          │
│ Son 90 gündeki sonuçlanan davetlerin gösteriliyor.       │
├──────────────────────────────────────────────────────────┤
│ Keşfet  Başvurularım  Davetlerim  Süreçteki  Özgeçmiş    │
│                          ▔▔▔▔▔▔▔                         │
└──────────────────────────────────────────────────────────┘
```

### 11.7 Davetin altı sonucu

| Ne oldu | Kim | Kartta | Adaya bildirim |
|---|---|---|---|
| Aday kabul etmedi | Aday | ✕ **Daveti kabul etmedin** | — |
| 10 gün cevap yok | Süre | ~ **Cevap vermediğin için sona erdi** | 2.3 |
| İşveren geri çekti | İşveren | <- **İşveren davetini geri çekti** | 2.4 |
| İlanın sonucu bildirildi, davet bekliyordu | İşveren | ■ **Pozisyon doldu** · **Pozisyon iptal edildi** · **Süreç sona erdi** | 5.1 · 5.2 · 5.3 |
| Şirket hesabını sildi | Şirket | ■ **Şirket platformdan ayrıldı** | 5.5 |

**Kabul edilen davet burada yok** — doğrudan **Süreçteki İlanlar**'a geçer.

### 11.8 Sonuçlanan — butonlar ve detay

| Buton | Ne olur |
|---|---|
| **[Detay →]** | Davet detayı — DAVETİN bölümünde **sonuç** yazar · işverenin notu ve iki açılır pencere aynı · **altta buton yok** |
| **[Listeden kaldır]** | Onaysız · kart kalkar · kayıt silinmez |

**Sonuçlanmış davetin detayı — üst bölüm:**

```
│ ┌──────────────────────────────────────────────────────┐ │
│ │ DAVETİN                                              │ │
│ │ <- İşveren davetini geri çekti                       │ │
│ │ 15 Ağustos'ta davet edildin · 18 Ağustos'ta          │ │
│ │ sonuçlandı                                           │ │
│ │                                                      │ │
│ │ İşverenin notu                                       │ │
│ │ "Ekibimize katılmanızı çok isteriz."                 │ │
│ │                                                      │ │
│ │ [Neden önerildi?]       [Şirket profili]             │ │
│ └──────────────────────────────────────────────────────┘ │
```

| Kural | |
|---|---|
| Tekrar | Sonuçlanan davetin ilanından bu adaya tekrar davet gelmez · aday bu ilana Keşfet'ten de başvuramaz |
| Süre | Son **90 gün** |
| Sıra | En yeni sonuç üstte |
| Hiç yoksa | *"Henüz sonuçlanan davetin yok."* |

### 11.9 Akış

```
Davetlerim
  │
  ├─ Bekleyen
  │    ├─ [Kabul et] ───────────> Daveti kabul et (11.5)
  │    │                            ├─ [Vazgeç] ──> Bekleyen'de kalır
  │    │                            └─ [Kabul et] ─> Kontroller ──> "Daveti kabul ettin"
  │    │                                               ├─ [Sohbete git →] ──> Sohbet
  │    │                                               └─ [Davetlerime dön]
  │    │                               (davet → Süreçteki İlanlar)
  │    │
  │    ├─ [Detay] / kart ────────> Davet detayı (11.4)
  │    │                            ├─ [Neden önerildi?] ──> Açılır pencere
  │    │                            ├─ [Şirket profili] ───> Açılır pencere
  │    │                            ├─ [⋮] ────────────────> Paylaş · İlanı bildir
  │    │                            ├─ [Kabul et] ─────────> Daveti kabul et (11.5)
  │    │                            └─ [Kabul etme] ───────> Onay
  │    │
  │    └─ [Kabul etme] ──────────> Onay ──> Sonuçlanan
  │
  └─ Sonuçlanan
       ├─ [Detay →] ──────────> Davet detayı (sonuç yazar, altta buton yok)
       └─ [Listeden kaldır] ──> Kart kalkar
```

---
## 12. Süreçteki İlanlar

İki tarafın da **"evet"** dediği ilanlar — başvurusu kabul edilenler ve daveti kabul edilenler. Burada **sohbet** var.

### 12.1 Devam eden — liste

```
┌───────────────────────────────────────────────────────────┐
│ [● Aktif iş arıyorum ▾]          (zil 3) (ayarlar) (AK)   │
├───────────────────────────────────────────────────────────┤
│ ┌────────────────┬──────────────┐                         │
│ │▸ Devam eden 3  │ Sonuçlanan 2 │                         │
│ └────────────────┴──────────────┘                         │
├───────────────────────────────────────────────────────────┤
│ ┌───────────────────────────────────────────────────────┐ │
│ │ İŞ İLANI                              ● Yüksek uyum   │ │
│ │ Senior Backend Developer                              │ │
│ │ ABC Teknoloji  ✓ Doğrulanmış                          │ │
│ │ İlan yayında                                          │ │
│ │                                                       │ │
│ │ Mehmet Kaya: "Salı 14:00 uygun mudur?"     10:32 (2)  │ │
│ │ Başvurdun · Süreç 15 Eylül'de başladı                 │ │
│ │ İletişim bilgini paylaştın                            │ │
│ │                                                       │ │
│ │ [Detay]                           [Sohbete git →]     │ │
│ └───────────────────────────────────────────────────────┘ │
│ ┌───────────────────────────────────────────────────────┐ │
│ │ İŞ İLANI                              ● Yüksek uyum   │ │
│ │ Backend Developer                                     │ │
│ │ PRS Teknoloji  ✓ Doğrulanmış                          │ │
│ │ İlan geçici olarak durduruldu                         │ │
│ │                                                       │ │
│ │ Sen: "Teşekkürler, görüşmek isterim."        Dün      │ │
│ │ Davet edildin · Süreç 20 Eylül'de başladı             │ │
│ │                                                       │ │
│ │ [Detay]                           [Sohbete git →]     │ │
│ └───────────────────────────────────────────────────────┘ │
│ ┌───────────────────────────────────────────────────────┐ │
│ │ STAJ İLANI                              ○ Orta uyum   │ │
│ │ Veri Stajyeri                                         │ │
│ │ TUV Analitik  ✓ Doğrulanmış                           │ │
│ │ İlan yayında                                          │ │
│ │                                                       │ │
│ │ Henüz mesaj yok                                       │ │
│ │ Davet edildin · Süreç 22 Eylül'de başladı             │ │
│ │                                                       │ │
│ │ [Detay]                           [Sohbete git →]     │ │
│ └───────────────────────────────────────────────────────┘ │
├───────────────────────────────────────────────────────────┤
│ Keşfet  Başvurularım  Davetlerim  Süreçteki  Özgeçmiş     │
│                                   ▔▔▔▔▔▔▔▔▔  (2)          │
└───────────────────────────────────────────────────────────┘
```

| Kartta | |
|---|---|
| **İlan başlığı** *(5.1)* | İlan tipi · uyum · unvan · şirket · ilanın durumu |
| **Son mesaj** | Yazanın adı + mesajın başı + saat · aday yazdıysa *"Sen:"* · **(2)** okunmamış · yoksa *"Henüz mesaj yok"* |
| **Nereden geldi** | *"Başvurdun"* veya *"Davet edildin"* + süreç başlangıcı |
| **İletişim** | *"İletişim bilgini paylaştın"* · paylaşmadıysa satır yok |

**Kartta bilerek olmayan — 60 günden kalan süre:** İşverene gösteriliyor çünkü sonuçlandırmak onun işi. Adaya gösterilmiyor; *"süren bitiyor"* hissi baskı yapar, oysa süreyi işletecek olan işveren.

| Buton | Ne olur |
|---|---|
| **[Sohbete git →]** / kartın boş yeri | **Sohbet** — bu sekmede en sık yapılan iş mesajlaşmak *(sohbet dokümanı)* |
| **[Detay]** | Süreç detayı *(12.2)* |

| Kural | |
|---|---|
| Sıra | Son mesaj en üstte, okunmamışı olan önde |
| Rozet | Alt menüde toplam okunmamış mesaj |
| İlan yayından kalksa | Süreç devam eder, sohbet açık |
| Şu an iş aramıyorum | Süreçler devam eder |
| Kartta [Süreçten çekil] | **Yok** — yanlışlıkla basılmasın |
| Hiç yoksa | *"Henüz süreçte olduğun ilan yok. Başvurun kabul edilince veya bir daveti kabul edince burada görünür."* |

### 12.2 Süreç detayı

Üstte **SÜRECİN** bölümü, altında ilanın tamamı.

```
┌──────────────────────────────────────────────────────────┐
│ ← Geri                Sürecim                        [⋮] │
├──────────────────────────────────────────────────────────┤
│ ┌──────────────────────────────────────────────────────┐ │
│ │ SÜRECİN                                              │ │
│ │ Görüşme devam ediyor                                 │ │
│ │ Başvurdun · 12 Eylül                                 │ │
│ │ Başvurun kabul edildi · Süreç 15 Eylül'de başladı    │ │
│ │ İletişim bilgini paylaştın                           │ │
│ │                                                      │ │
│ │ [Gönderdiğin bilgiler]  [Neden önerildi?]            │ │
│ │ [Şirket profili]                                     │ │
│ │                                                      │ │
│ │                              [Sohbete git →]  (2)    │ │
│ └──────────────────────────────────────────────────────┘ │
│                                                          │
│ İŞ İLANI                                  ● Yüksek uyum  │
│ Senior Backend Developer                                 │
│ ABC Teknoloji  ✓ Doğrulanmış                             │
│ %92 cevap verir · 12 işe alım                            │
│ İlan yayında · 3 hafta önce yayınlandı                   │
│                                                          │
│ ── Pozisyon ──────────────────────────────────────────── │
│ (her bölüm Bölüm 5.2'deki gibi, eksiksiz)                │
├──────────────────────────────────────────────────────────┤
│ [Süreçten çekil]                                         │
└──────────────────────────────────────────────────────────┘
```

| Parça | Ne |
|---|---|
| **Durum** | *"Görüşme devam ediyor"* |
| **Nasıl başladı** | Başvuruyla: *"Başvurdun · 12 Eylül"* + *"Başvurun kabul edildi · Süreç 15 Eylül'de başladı"* · Davetle: *"PRS Teknoloji seni davet etti · 18 Eylül"* + *"Daveti kabul ettin · Süreç 20 Eylül'de başladı"* |
| **İletişim** | *"İletişim bilgini paylaştın"* / *"İletişim bilgini henüz paylaşmadın"* |
| **[Gönderdiğin bilgiler]** | Maaş · not · belgeler · davetle başladıysa işverenin davet notu da |
| **[Neden önerildi?]** | Başvuru / davet anındaki kayıttan |
| **[Şirket profili]** | |
| **[Sohbete git →]** | Sohbet · yanında okunmamış sayısı |
| Altta sabit | **[Süreçten çekil]** |
| [⋮] | Paylaş · İlanı bildir |

### 12.3 Süreçten çekil

**İki yerde:** Süreç detayının altında **ve** sohbetin sağ üstündeki **[⋮]** menüsünde. **Kartta yok.**

```
Sohbet → [⋮]
┌──────────────────────────────────────────────────────────┐
│ ← Geri   ABC Teknoloji · Senior Backend Dev.         [⋮] │
│                                       ┌────────────────┐ │
│                                       │ İlan detayı    │ │
│                                       │ Süreçten çekil │ │
│                                       │ Bildir         │ │
│                                       └────────────────┘ │
```

İki yerden de **aynı onay penceresi:**

```
┌──────────────────────────────────────────────┐
│ ABC Teknoloji ile süreçten çekilmek          │
│ istediğine emin misin?                       │
│                                              │
│ Sohbet kapanacak ve bu ilana tekrar          │
│ başvuramayacaksın.                           │
│                                              │
│ Sebebini seçebilirsin (opsiyonel):           │
│ ○ Başka bir yerde işe başladım               │
│ ○ Şartlar bana uymadı                        │
│ ○ Artık iş aramıyorum                        │
│ ○ Diğer → [________________________]         │
│                                              │
│ Sebebin işverene gösterilmez.                │
│                                              │
│ [Vazgeç]                 [Süreçten çekil]    │
└──────────────────────────────────────────────┘
```

| | |
|---|---|
| Sebep | Opsiyonel · *"Diğer"* en fazla 200 karakter · **işverene gösterilmez** — aday rahatça çekilebilsin |
| **[Vazgeç]** | Hiçbir şey olmaz |
| **[Süreçten çekil]** | Süreç **Sonuçlanan**'a geçer → *"<- Süreçten çekildin"* · sohbet kapanır, geçmiş görünür |
| *"Başka bir yerde işe başladım"* seçildiyse | Ardından: *"Arama durumunu 'Şu an iş aramıyorum' yapmak ister misin?"* |
| Tekrar | Aynı ilana tekrar başvurulamaz |

### 12.4 Aday çekilince işveren ne görür

| Yer | İşveren görür |
|---|---|
| **Bildirim** | Sorumluya 📱 🔔 · sahip / yöneticiye 🔔: *"Ayşe Kaya, Senior Backend Developer sürecinden çekildi."* |
| **Süreçteki Adaylar → Sonuçlanan** | *"<- Aday süreçten çekildi · 28 Eylül"* · [Sohbeti gör] · **PDF yok** |
| **Sohbet** | Sistem mesajı: *"Ayşe Kaya süreçten çekildi · Bu sohbet kapandı."* · yazma kapanır · geçmiş görünür |
| **Belgeler** | ❌ Kapanır |
| **Telefon ve e-posta** *(paylaşıldıysa)* | ❌ **Gizlenir** — aday artık görüşmek istemiyor |
| **Çekilme sebebi** | ❌ Görmez |
| **Şirket raporu** | Aleyhine sayılmaz — "sürece alınan" olarak kalır |

### 12.5 Devam eden — akış

```
Süreçteki → Devam eden
   │
   ├─ Kartın boş yeri / [Sohbete git →] ──> Sohbet
   │                                          └─ [⋮] → Süreçten çekil ──> Onay (12.3)
   │
   └─ [Detay] ──> Süreç detayı
                    ├─ [Gönderdiğin bilgiler] ──> Açılır pencere
                    ├─ [Neden önerildi?] ───────> Açılır pencere
                    ├─ [Şirket profili] ────────> Açılır pencere
                    ├─ [Sohbete git →] ─────────> Sohbet
                    ├─ [⋮] ─────────────────────> Paylaş · İlanı bildir
                    └─ [Süreçten çekil] ────────> Onay
                                                   ├─ [Vazgeç] ──> Hiçbir şey olmaz
                                                   └─ [Süreçten çekil] ──> Sonuçlanan
                                                        + sohbet kapanır
                                                        + işverene bildirim
                                                        + (iş bulduysa) arama durumu sorusu
```

### 12.6 Sürecin sekiz sonucu

| # | Ne oldu | Kim | Nasıl | Adaya kartta | Bildirim |
|:---:|---|---|---|---|---|
| 1 | **İşe alındı** | İşveren | "Süreci sonuçlandır → İşe alındı" veya "Sonuç bildir → Pozisyon doldu" + bu adayı seçti | * **İşe alındığını bildirdi** + onay sorusu | 4.1 |
| 2 | **Devam edilmedi** | İşveren | "Süreci sonuçlandır → Devam etmiyoruz" | ✕ **Başka adaylarla ilerlendi** | 3.3 |
| 3 | **60 gün doldu** | Süre | İşveren sonuçlandırmadı | ~ **Süreç sona erdi** *(nötr)* | 3.4 |
| 4 | **Aday çekildi** | Aday | [Süreçten çekil] | <- **Süreçten çekildin** | — |
| 5 | **İlanın sonucu bildirildi**, bu aday seçilmedi | İşveren | "Sonuç bildir" | ■ **Pozisyon doldu** · **Pozisyon iptal edildi** · **Süreç sona erdi** | 5.1 · 5.2 · 5.3 |
| 6 | **İlan 75 günde sonuçlanmadı** | Süre | Süresi dolan ilana sonuç bildirilmedi | ~ **Süreç sona erdi** | 5.4 |
| 7 | **Şirket ayrıldı** | Şirket | Şirket hesabını sildi | ■ **Şirket platformdan ayrıldı** | 5.5 |
| 8 | **İlan kaldırıldı** | Admin | İlan kurallara aykırı bulundu | ■ **İlan platform kurallarına aykırı bulundu** + uyarı | 5.6 |

**60 gün dolunca** adaya nötr *"Süreç sona erdi"*, işverene *"Süre doldu — sonuçlandırılmadı"* yazar.

**5 nasıl olur:** İşveren birkaç adayla görüşür, birini işe alır ve "Sonuç bildir → Pozisyon doldu" der. Seçmediği adayların süreci **toplu** biter. 2'den farkı mesajda.

**6 pratikte nadir:** Süreç 60 günde biter; ilanın 75. günü önce gelirse bu yoldan biter.

### 12.7 Sonuçlanan — liste

*Kartların üstündeki "1 — İŞE ALINDI" gibi başlıklar sadece bu dokümanda açıklama için; uygulamada görünmez.*

```
┌───────────────────────────────────────────────────────────┐
│ [– Şu an iş aramıyorum ▾]        (zil 3) (ayarlar) (AK)   │
├───────────────────────────────────────────────────────────┤
│ ┌────────────────┬──────────────┐                         │
│ │  Devam eden 0  │▸Sonuçlanan 8 │                         │
│ └────────────────┴──────────────┘                         │
├───────────────────────────────────────────────────────────┤
│ ┌───────────────────────────────────────────────────────┐ │
│ │ 1 — İŞE ALINDI                                        │ │
│ │ İŞ İLANI                              ● Yüksek uyum   │ │
│ │ Senior Backend Developer                              │ │
│ │ ABC Teknoloji  ✓ Doğrulanmış                          │ │
│ │ İlan kapandı                                          │ │
│ │                                                       │ │
│ │ * İşe alındığını bildirdi · 28 Eylül                  │ │
│ │ ┌──────────────────────────────────────────────────┐  │ │
│ │ │ ABC Teknoloji seni işe aldığını bildirdi.        │  │ │
│ │ │ Doğru mu?                                        │  │ │
│ │ │ [Hayır]                  [Evet, işe alındım]     │  │ │
│ │ └──────────────────────────────────────────────────┘  │ │
│ │                                                       │ │
│ │ [Sohbeti gör]                           [Detay →]     │ │
│ └───────────────────────────────────────────────────────┘ │
│ ┌───────────────────────────────────────────────────────┐ │
│ │ 2 — İŞVEREN DEVAM ETMEDİ                              │ │
│ │ İŞ İLANI                                ○ Orta uyum   │ │
│ │ Backend Developer                                     │ │
│ │ PRS Teknoloji  ✓ Doğrulanmış                          │ │
│ │ İlan yayında                                          │ │
│ │                                                       │ │
│ │ ✕ Başka adaylarla ilerlendi · 25 Eylül                │ │
│ │                                                       │ │
│ │ [Listeden kaldır]  [Sohbeti gör]        [Detay →]     │ │
│ └───────────────────────────────────────────────────────┘ │
│ ┌───────────────────────────────────────────────────────┐ │
│ │ 3 — 60 GÜN DOLDU                                      │ │
│ │ STAJ İLANI                              ○ Orta uyum   │ │
│ │ Veri Stajyeri                                         │ │
│ │ TUV Analitik  ✓ Doğrulanmış                           │ │
│ │ İlan yayında                                          │ │
│ │                                                       │ │
│ │ ~ Süreç sona erdi · 21 Eylül                          │ │
│ │                                                       │ │
│ │ [Listeden kaldır]  [Sohbeti gör]        [Detay →]     │ │
│ └───────────────────────────────────────────────────────┘ │
│ ┌───────────────────────────────────────────────────────┐ │
│ │ 4 — ADAY ÇEKİLDİ                                      │ │
│ │ İŞ İLANI                                ○ Orta uyum   │ │
│ │ Yazılım Mühendisi                                     │ │
│ │ CDE Teknoloji  ✓ Doğrulanmış                          │ │
│ │ İlan yayında                                          │ │
│ │                                                       │ │
│ │ <- Süreçten çekildin · 18 Eylül                       │ │
│ │                                                       │ │
│ │ [Listeden kaldır]  [Sohbeti gör]        [Detay →]     │ │
│ └───────────────────────────────────────────────────────┘ │
│ ┌───────────────────────────────────────────────────────┐ │
│ │ 5 — İLANIN SONUCU BİLDİRİLDİ                          │ │
│ │ İŞ İLANI                              ● Yüksek uyum   │ │
│ │ Muhasebe Uzmanı                                       │ │
│ │ GHI Holding  ✓ Doğrulanmış                            │ │
│ │ İlan kapandı                                          │ │
│ │                                                       │ │
│ │ ■ Pozisyon doldu · 15 Eylül                           │ │
│ │                                                       │ │
│ │ [Listeden kaldır]  [Sohbeti gör]        [Detay →]     │ │
│ └───────────────────────────────────────────────────────┘ │
│ ┌───────────────────────────────────────────────────────┐ │
│ │ 6 — İLANIN SONUCU 75 GÜNDE BİLDİRİLMEDİ               │ │
│ │ İŞ İLANI                                ○ Orta uyum   │ │
│ │ Lojistik Uzmanı                                       │ │
│ │ RST Lojistik  ✓ Doğrulanmış                           │ │
│ │ İlan kapandı                                          │ │
│ │                                                       │ │
│ │ ~ Süreç sona erdi · 12 Eylül                          │ │
│ │                                                       │ │
│ │ [Listeden kaldır]  [Sohbeti gör]        [Detay →]     │ │
│ └───────────────────────────────────────────────────────┘ │
│ ┌───────────────────────────────────────────────────────┐ │
│ │ 7 — ŞİRKET AYRILDI                                    │ │
│ │ İŞ İLANI                                ○ Orta uyum   │ │
│ │ Satış Temsilcisi                                      │ │
│ │ Silinmiş şirket                                       │ │
│ │ İlan artık mevcut değil                               │ │
│ │                                                       │ │
│ │ ■ Şirket platformdan ayrıldı · 10 Eylül               │ │
│ │                                                       │ │
│ │ [Listeden kaldır]  [Sohbeti gör]                      │ │
│ └───────────────────────────────────────────────────────┘ │
│ ┌───────────────────────────────────────────────────────┐ │
│ │ 8 — ADMIN İLANI KALDIRDI                              │ │
│ │ İŞ İLANI                                ○ Orta uyum   │ │
│ │ Evden Paketleme Elemanı                               │ │
│ │ XYZ Ticaret                                           │ │
│ │ İlan kaldırıldı                                       │ │
│ │                                                       │ │
│ │ ■ İlan platform kurallarına aykırı bulundu            │ │
│ │ ! Senden para veya kişisel bilgi istenirse paylaşma.  │ │
│ │                                                       │ │
│ │ [Listeden kaldır]  [Sohbeti gör]                      │ │
│ └───────────────────────────────────────────────────────┘ │
│                                                           │
│ Son 90 gündeki sonuçlanan süreçlerin gösteriliyor.        │
├───────────────────────────────────────────────────────────┤
│ Keşfet  Başvurularım  Davetlerim  Süreçteki  Özgeçmiş     │
│                                   ▔▔▔▔▔▔▔▔▔  (!)          │
└───────────────────────────────────────────────────────────┘
```

| # | [Detay →] | [Sohbeti gör] | [Listeden kaldır] |
|:---:|:---:|:---:|:---:|
| 1 | ✅ | ✅ | ❌ onay süresi bitene kadar |
| 2 · 3 · 4 · 5 · 6 | ✅ | ✅ | ✅ |
| 7 · 8 | ❌ ilan yok | ✅ | ✅ |

### 12.8 İşe alım onayı

**Neden:** İşe alım platform dışında olur — sözleşme bize gelmez. İki tarafın da onaylaması şirket raporunun ve platformun başarısının **gerçek ölçüsü.**

**Nerede görünür:** Bildirim (4.1) · Süreçteki → Sonuçlanan kartı · süreç detayının SÜRECİN bölümü.

**Kartın dört hali:**

```
Cevap bekleniyor (14 gün)
│ * İşe alındığını bildirdi · 28 Eylül                 │
│ ┌──────────────────────────────────────────────────┐ │
│ │ ABC Teknoloji seni işe aldığını bildirdi.        │ │
│ │ Doğru mu?                                        │ │
│ │ [Hayır]                  [Evet, işe alındım]     │ │
│ └──────────────────────────────────────────────────┘ │

Evet dedi
│ * İşe alındın · 28 Eylül                             │
│ ✓ Onayladın                      [Cevabımı değiştir] │

Hayır dedi
│ * İşe alındığını bildirdi · 28 Eylül                 │
│ ✕ İşe alınmadığını bildirdin     [Cevabımı değiştir] │

14 gün cevap vermedi
│ * İşe alındığını bildirdi · 28 Eylül                 │
│ Cevap vermedin                           [Cevap ver] │
```

**[Evet, işe alındım]:**

```
┌──────────────────────────────────────────────┐
│ Tebrikler, işe alındın!                      │
│                                              │
│ Arama durumunu "Şu an iş aramıyorum"         │
│ yapmak ister misin? Özgeçmişin               │
│ işverenlerden gizlenir.                      │
│                                              │
│ [Şimdilik hayır]           [Evet, değiştir]  │
└──────────────────────────────────────────────┘
```

**[Hayır]:**

```
┌──────────────────────────────────────────────┐
│ ABC Teknoloji'nin seni işe almadığını        │
│ bildiriyorsun. Emin misin?                   │
│                                              │
│ [Vazgeç]                       [Eminim]      │
└──────────────────────────────────────────────┘
```

| Kural | |
|---|---|
| Cevap süresi | **14 gün** — sonra *"Cevap vermedin"* |
| Değiştirme süresi | İşverenin bildiriminden itibaren **30 gün** — sonra kayıt kesinleşir |
| **[Cevabımı değiştir]** / **[Cevap ver]** | Onay kutusu yeniden açılır |
| İşveren **[Adaya tekrar sor]** | **Bir kez** · onay kutusu yeniden çıkar · bildirim 4.2 |
| İşveren **"Yanlış işaretledim"** | Kart **3 — Süreç sona erdi** olur · bildirim gitmez |
| İşveren görür | ✓ Aday onayladı · ✕ Aday işe alınmadığını bildirdi · - Aday cevap vermedi |
| Hayır + 30 gün düzeltilmedi | Kayıt **tartışmalı** — admin listesine düşer |

### 12.9 Sonuçlanmış süreç detayı

```
┌──────────────────────────────────────────────────────────┐
│ ← Geri                Sürecim                        [⋮] │
├──────────────────────────────────────────────────────────┤
│ ┌──────────────────────────────────────────────────────┐ │
│ │ SÜRECİN                                              │ │
│ │ ✕ Başka adaylarla ilerlendi · 25 Eylül               │ │
│ │ Davet edildin · 18 Eylül                             │ │
│ │ Daveti kabul ettin · Süreç 20 Eylül'de başladı       │ │
│ │                                                      │ │
│ │ [Gönderdiğin bilgiler]  [Neden önerildi?]            │ │
│ │ [Şirket profili]                                     │ │
│ │                                                      │ │
│ │                                [Sohbeti gör →]       │ │
│ └──────────────────────────────────────────────────────┘ │
│                                                          │
│ İŞ İLANI                                  ○ Orta uyum    │
│ Backend Developer                                        │
│ PRS Teknoloji  ✓ Doğrulanmış                             │
│ (ilanın tüm bölümleri)                                   │
└──────────────────────────────────────────────────────────┘
```

| | |
|---|---|
| SÜRECİN | Sonuç + tarih · nasıl başladığı · üç açılır pencere · **[Sohbeti gör →]** salt okunur |
| İşe alınan süreçte | Onay kutusu SÜRECİN bölümünde de görünür |
| Altta | **Buton yok** |

### 12.10 Sonuçlanan — kurallar

| | |
|---|---|
| Sıra | **Cevap bekleyen işe alım onayı en üstte**, sonra en yeni sonuç |
| Rozet | Cevap bekleyen onay varsa alt menüde **(!)** |
| Süre | Son **90 gün** |
| Sohbet | Sekizinde de kapalı · geçmiş görünür |
| Belgeler | Sadece **işe alınanda** işverenin erişimi sürer |
| 60 gün | Adaya kalan süre hiç gösterilmez · bitince nötr cümle |
| Tekrar | Sonuçlanan sürecin ilanına tekrar başvurulamaz |
| Hiç yoksa | *"Henüz sonuçlanan sürecin yok."* |

### 12.11 Sonuçlanan — akış

```
Süreçteki → Devam eden
   │
   ├─ İşveren "İşe alındı" ──────────────────────────> 1  + onay sorusu
   ├─ İşveren "Devam etmiyoruz" ─────────────────────> 2
   ├─ 60 gün doldu ──────────────────────────────────> 3
   ├─ Aday [Süreçten çekil] ─────────────────────────> 4
   ├─ İşveren "Sonuç bildir", bu adayı seçmedi ──────> 5
   ├─ İlan süresi doldu + 75 gün sonuç yok ──────────> 6
   ├─ Şirket hesabını sildi ─────────────────────────> 7
   └─ Admin ilanı kaldırdı ──────────────────────────> 8
                                                        │
Süreçteki → Sonuçlanan                                  v
   ├─ [Evet, işe alındım] ──> Tebrikler + arama durumu sorusu
   ├─ [Hayır] ──────────────> Emin misin? ──> [Eminim]
   ├─ [Cevabımı değiştir] / [Cevap ver] ──> Onay kutusu yeniden
   ├─ [Detay →] ────────────> Süreç detayı (sonuç yazar)
   ├─ [Sohbeti gör] ────────> Sohbet, salt okunur
   └─ [Listeden kaldır] ────> Kart kalkar
```

---

## 13. Özgeçmiş Sekmesi

Özgeçmiş'in içi — on iki adım, önizleme, tamamlanma yüzdesi, arama özeti, eşleşme gücü, yayınla, yayın eşiği — **`aday-profili-final.md` Bölüm 3–18**'de eksiksiz yazılı. Burada sadece aday panelinde değişenler var:

| Değişiklik | Ne demek |
|---|---|
| **Sekmenin adı "Özgeçmiş"** | Alt menüde "Profilim" yerine **Özgeçmiş** · yayında değilse **(!)** |
| **Hesap bilgileri Profilim'e taşındı** | Ad soyad, telefon (SMS doğrulama), e-posta, doğum tarihi artık sağ üstteki **Profilim**'de düzenleniyor *(Bölüm 3)* |
| **Özgeçmiş'te görünür ama düzenlenmez** | Ad ve fotoğraf özgeçmişin en üstünde görünür; dokununca Profilim açılır |
| **Özgeçmiş Adım 1'de kalanlar** | Cinsiyet · yaşadığı konum · çalışmak istediği konumlar · her konumda hazırım · çalışma şekli · çalışma tipi · kısa tanıtım |
| **Yayın eşiği değişmedi** | Telefon doğrulanmadan özgeçmiş yayınlanamaz · Özgeçmiş'te *"Telefonunu doğrula"* uyarısı → **[Doğrula]** Profilim'i açar |
| **Arama durumu** | Üst barda her sekmede *(2.3)* — Özgeçmiş'in yayınla adımında da seçilir |

---

## 14. Bildirimler

### 14.1 Üç kanal

| Kanal | Nedir | Nerede görünür | Kapatılabilir mi |
|---|---|---|---|
| **Telefon bildirimi** *(push)* | Uygulama **kapalıyken** telefonun ekranına düşen kısa mesaj | Kilit ekranı, bildirim çubuğu | ✅ Ayarlar'dan türlere göre |
| **Zil** *(uygulama içi)* | Uygulamanın içindeki bildirim listesi | Sağ üstteki zil → liste | ❌ **Her zaman açık** |
| **E-posta** | Adayın e-posta adresine giden mesaj | E-posta kutusu | ✅ Ayarlar'dan — güvenlik e-postaları hariç |

| | Telefon | Zil | E-posta |
|---|---|---|---|
| Uygulama kapalıyken görünür | ✅ | ❌ | ✅ |
| Kalıcı mı | Kaydırınca kaybolur | **Kalır** (6 ay) | Kutuda kalır |
| Ne için | Acil ve önemli | **Her şeyin kaydı** | Uygulamaya girmeyeni geri çekmek |

> **Kural:** Her bildirim **zile** düşer. Telefona ve e-postaya sadece önemli olanlar gider. Aday telefon bildirimlerini kapatsa bile zilde hiçbir şeyi kaçırmaz.

### 14.2 Zil ekranı

**Üst bar → zil**

```
┌──────────────────────────────────────────────────────────┐
│ ← Geri               Bildirimler         [Tümünü okundu] │
├──────────────────────────────────────────────────────────┤
│ Bugün                                                    │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ ● ABC Teknoloji başvurunu kabul etti.        10:32   │ │
│ │   Senior Backend Developer · Görüşmeye               │ │
│ │   başlayabilirsin.                                   │ │
│ └──────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ ● PRS Teknoloji seni davet etti.             09:15   │ │
│ │   Backend Developer · 10 gün içinde cevap ver        │ │
│ └──────────────────────────────────────────────────────┘ │
│                                                          │
│ Dün                                                      │
│ ┌──────────────────────────────────────────────────────┐ │
│ │   Mehmet Kaya'dan 3 yeni mesaj               18:40   │ │
│ │   ABC Teknoloji · Senior Backend Developer           │ │
│ └──────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────┐ │
│ │   Sana uygun 6 yeni ilan var                 08:00   │ │
│ └──────────────────────────────────────────────────────┘ │
│                                                          │
│ Bu hafta                                                 │
│ ┌──────────────────────────────────────────────────────┐ │
│ │   DEF Yazılım, Frontend Developer pozisyonu  Sal     │ │
│ │   için başka adaylarla ilerlemeye karar verdi.       │ │
│ └──────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────┘
```

| Parça | Ne |
|---|---|
| **●** | Okunmamış — dokununca kaybolur |
| İlk satır | Ne oldu |
| İkinci satır | Hangi ilan + varsa yapılacak şey |
| Saat | Bugün saat · eskilerde gün / tarih |
| Gruplar | Bugün · Dün · Bu hafta · Daha önce |

| Buton | Ne olur |
|---|---|
| **Bildirime dokunmak** | Okundu olur + **ilgili ekran** açılır (katalogdaki "Dokununca" sütunu) |
| **[Tümünü okundu]** | Bütün ● kaybolur · zildeki sayı sıfırlanır |
| **← Geri** | Aday hangi sekmedeyse oraya |

| Kural | |
|---|---|
| Zildeki sayı | Okunmamış sayısı · 0 ise sayı yok · 99'dan fazlaysa **99+** |
| Sıra | En yeni üstte |
| Saklama | **6 ay** — sonra kendiliğinden silinir |
| Silme | Yok |
| Toplama | Aynı sohbetten kısa sürede çok mesaj → *"Mehmet Kaya'dan 3 yeni mesaj"* |
| İlgili şey artık yoksa | Dokununca *"Bu ilan artık mevcut değil"* |
| Hiç yoksa | *"Henüz bildirimin yok."* |

### 14.3 Bildirim kataloğu

**Kim kullanır:** Geliştirici (hangi olayda ne gideceğini buradan kodlar), tasarımcı (metinleri buradan alır), test eden. **Aday bu tabloyu görmez** — sonucunu telefonunda, zil ekranında ve e-postasında görür.

```
Olay olur  ──>  Katalogda o satıra bakılır  ──>  Bildirim gönderilir
(davet geldi)    Telefon ✓ · Zil ✓ · E-posta ✓      Adayın telefonuna, ziline,
                 Metin: "PRS Teknoloji seni..."       e-postasına düşer
                 Dokununca: Davet detayı
                 Ayarlar: "Davetler"
```

**1. Başvuru**

| # | Olay | Tel | Zil | E-posta | Metin | Dokununca | Ayarlar anahtarı |
|:---:|---|:---:|:---:|:---:|---|---|---|
| 1.1 | Başvurun kabul edildi | ✅ | ✅ | ✅ | *"ABC Teknoloji başvurunu kabul etti. Görüşmeye başlayabilirsin."* | Süreç detayı | Başvuru sonuçları |
| 1.2 | İşveren kabul etmedi | ✅ | ✅ | — | *"ABC Teknoloji, Senior Backend Developer pozisyonu için başka adaylarla ilerlemeye karar verdi."* | Başvuru detayı | Başvuru sonuçları |
| 1.3 | 30 günde yanıtlanmadı | — | ✅ | — | *"Senior Backend Developer başvurun yanıtlanmadan sona erdi."* | Başvuru detayı | — |

**2. Davet**

| # | Olay | Tel | Zil | E-posta | Metin | Dokununca | Ayarlar anahtarı |
|:---:|---|:---:|:---:|:---:|---|---|---|
| 2.1 | Davet geldi | ✅ | ✅ | ✅ | *"PRS Teknoloji seni Backend Developer pozisyonuna davet etti."* | Davet detayı | Davetler |
| 2.2 | Cevap için 2 gün kaldı | ✅ | ✅ | — | *"PRS Teknoloji'nin davetine cevap vermek için 2 gün kaldı."* | Davet detayı | Davetler |
| 2.3 | 10 günde cevap verilmedi | — | ✅ | — | *"PRS Teknoloji'nin davetinin süresi doldu."* | Davet detayı | — |
| 2.4 | İşveren davetini geri çekti | — | ✅ | — | *"PRS Teknoloji davetini geri çekti."* | Davet detayı | — |

**3. Süreç**

| # | Olay | Tel | Zil | E-posta | Metin | Dokununca | Ayarlar anahtarı |
|:---:|---|:---:|:---:|:---:|---|---|---|
| 3.1 | Yeni mesaj | ✅ | ✅ | — | *"Mehmet Kaya: Salı 14:00 uygun mudur?"* | Sohbet | Yeni mesaj |
| 3.2 | İletişim bilgisi istendi | ✅ | ✅ | — | *"ABC Teknoloji telefon ve e-posta bilgini görmek istiyor."* | Sohbet | Yeni mesaj |
| 3.3 | İşveren devam etmedi | ✅ | ✅ | — | *"ABC Teknoloji, Senior Backend Developer pozisyonu için başka adaylarla ilerlemeye karar verdi."* | Süreç detayı | Süreç ve işe alım |
| 3.4 | 60 gün doldu | — | ✅ | — | *"ABC Teknoloji ile Senior Backend Developer sürecin sona erdi."* | Süreç detayı | — |

**4. İşe alım onayı**

| # | Olay | Tel | Zil | E-posta | Metin | Dokununca | Ayarlar anahtarı |
|:---:|---|:---:|:---:|:---:|---|---|---|
| 4.1 | İşveren işe aldığını bildirdi | ✅ | ✅ | ✅ | *"ABC Teknoloji seni işe aldığını bildirdi. Doğru mu?"* | Süreç detayı | Süreç ve işe alım |
| 4.2 | İşveren tekrar sordu | ✅ | ✅ | — | *"ABC Teknoloji işe alındığını tekrar bildirdi. Cevabını kontrol eder misin?"* | Süreç detayı | Süreç ve işe alım |

**5. İlanın sonucu** *(bekleyen başvuru, davet veya süreçteki adaya)*

| # | Olay | Tel | Zil | E-posta | Metin | Dokununca | Ayarlar anahtarı |
|:---:|---|:---:|:---:|:---:|---|---|---|
| 5.1 | Pozisyon doldu | ✅ | ✅ | — | *"ABC Teknoloji'nin Senior Backend Developer pozisyonu doldurulduğu için süreç sona erdi."* | İlgili detay | Başvuru sonuçları |
| 5.2 | Pozisyon iptal edildi | ✅ | ✅ | — | *"ABC Teknoloji, Senior Backend Developer pozisyonunu iptal etti. Süreç sona erdi."* | İlgili detay | Başvuru sonuçları |
| 5.3 | Uygun aday bulunamadı | ✅ | ✅ | — | *"ABC Teknoloji, Senior Backend Developer pozisyonu için değerlendirme sürecini sonlandırdı."* | İlgili detay | Başvuru sonuçları |
| 5.4 | İlan 75 günde sonuçlanmadı | — | ✅ | — | *"ABC Teknoloji ile Senior Backend Developer sürecin sona erdi."* | Süreç detayı | — |
| 5.5 | Şirket ayrıldı | ✅ | ✅ | — | *"ABC Teknoloji platformdan ayrıldı. Sürecin sona erdi."* | İlgili detay | — |
| 5.6 | Admin ilanı kaldırdı | ✅ | ✅ | ✅ | *"Görüştüğün XYZ Ticaret ilanı platform kurallarına aykırı bulunduğu için kaldırıldı. Senden para veya kişisel bilgi istenirse paylaşma."* | İlgili detay | **Kapatılamaz** |

**6. Keşfet**

| # | Olay | Tel | Zil | E-posta | Metin | Dokununca | Ayarlar anahtarı |
|:---:|---|:---:|:---:|:---:|---|---|---|
| 6.1 | Özgeçmiş yayınlandı, ilanlar hazır | — | ✅ | — | *"Sana uygun 86 ilan hazır."* | Keşfet | — |
| 6.2 | Günlük özet — yeni uygun ilanlar | ✅ | ✅ | — | *"Sana uygun 6 yeni ilan var."* | Keşfet | Sana uygun yeni ilanlar |
| 6.3 | Liste bitmişti, yeni ilan geldi | ✅ | ✅ | — | *"Sana uygun yeni bir ilan geldi."* | Keşfet | Sana uygun yeni ilanlar |

**7. Arama durumu**

| # | Olay | Tel | Zil | E-posta | Metin | Dokununca | Ayarlar anahtarı |
|:---:|---|:---:|:---:|:---:|---|---|---|
| 7.1 | 23. gün — 7 gün sonra düşecek | ✅ | ✅ | ✅ | *"Bir süredir giriş yapmadın. Arama durumun 7 gün sonra 'Fırsatlara açığım' olacak."* | Arama durumu penceresi | **Kapatılamaz** |
| 7.2 | 83. gün — 7 gün sonra gizlenecek | ✅ | ✅ | ✅ | *"Özgeçmişin 7 gün sonra işverenlerden gizlenecek."* | Arama durumu penceresi | **Kapatılamaz** |

**8. Hesap ve güvenlik**

| # | Olay | Tel | Zil | E-posta | Metin | Dokununca | Ayarlar anahtarı |
|:---:|---|:---:|:---:|:---:|---|---|---|
| 8.1 | Telefon numarası değişti | — | ✅ | ✅ | *"Telefon numaran değişti. Sen yapmadıysan bize ulaş."* | Profilim | **Kapatılamaz** |
| 8.2 | Telefon doğrulanmadı, özgeçmiş yayından çıktı | ✅ | ✅ | — | *"Özgeçmişin işverenlerden gizlendi. Devam etmek için telefonunu doğrula."* | Profilim | **Kapatılamaz** |
| 8.3 | Yeni cihazdan giriş | — | ✅ | ✅ | *"Hesabına yeni bir cihazdan giriş yapıldı."* | Ayarlar → Açık oturumlar | **Kapatılamaz** |
| 8.4 | Hesap silindi | — | — | ✅ | *"Hesabın silindi."* | — | **Kapatılamaz** |

**9. Bildirdiğin ilan**

| # | Olay | Tel | Zil | E-posta | Metin | Dokununca | Ayarlar anahtarı |
|:---:|---|:---:|:---:|:---:|---|---|---|
| 9.1 | Bildirdiğin ilan kaldırıldı | — | ✅ | — | *"Bildirdiğin ilan incelendi ve kaldırıldı. Teşekkürler."* | — | — |

### 14.4 Ayarlar anahtarları — neyi kapatır

| Ayarlar'daki anahtar | Kapattığı telefon bildirimleri |
|---|---|
| **Davetler** | 2.1 · 2.2 |
| **Yeni mesaj** | 3.1 · 3.2 |
| **Başvuru sonuçları** | 1.1 · 1.2 · 5.1 · 5.2 · 5.3 |
| **Süreç ve işe alım** | 3.3 · 4.1 · 4.2 |
| **Sana uygun yeni ilanlar** | 6.2 · 6.3 |
| **E-posta bildirimleri** | E-postaların hepsi — *kapatılamazlar hariç* |

Anahtar kapalıyken o bildirim **telefona gitmez, zile düşmeye devam eder.**

### 14.5 Bildirim kuralları

| Kural | |
|---|---|
| **Zil** | Her olay zile düşer · kapatılamaz |
| **Telefon** | Sadece adayın bir şey yapması gereken veya önemli olaylar |
| **E-posta** | Sadece en önemliler: davet, kabul, işe alım onayı, kaldırılan ilan, arama durumu uyarıları, güvenlik |
| **Kapatılamayanlar** | 5.6 · 7.1 · 7.2 · 8.1 · 8.2 · 8.3 · 8.4 |
| **Saat** | Mesajlar ve kararlar **hemen** · uyarılar ve günlük özet adayın saat diliminde **sabah** |
| **Toplama** | Aynı sohbetten kısa sürede çok mesaj → tek bildirim |
| **Dil** | Adayın uygulama dilinde |
| **Kırıcı ifade yok** | *"Reddedildin"* hiçbir yerde yazmaz |

---

## 15. Hesabımı Sil

**Nereden:** Ayarlar → Hesap → **[Hesabımı sil]**

Şirket hesabında olduğu gibi **beklemesiz ve kalıcı.** Aday ne olacağını açıkça görmeli, yanlışlıkla silememeli.

### 15.1 Adım 1 — Önce bir alternatif

```
┌──────────────────────────────────────────────────────────┐
│ ← Geri               Hesabımı sil                        │
├──────────────────────────────────────────────────────────┤
│ Sadece bir süre iş aramayacaksan hesabını silmene        │
│ gerek yok.                                               │
│                                                          │
│ Arama durumunu "Şu an iş aramıyorum" yaparsan            │
│ özgeçmişin işverenlerden gizlenir, ama bilgilerin        │
│ kaybolmaz. İstediğin zaman tekrar açabilirsin.           │
│                                                          │
│ [Şu an iş aramıyorum yap]                                │
│                                                          │
│ ──────────────────────────────────────────────────────── │
│ Yine de hesabımı silmek istiyorum           [Devam et →] │
└──────────────────────────────────────────────────────────┘
```

**Neden:** Çoğu aday hesabını *"artık iş aramıyorum"* diye silmek ister; daha yumuşak yol bilgilerini korur.

### 15.2 Adım 2 — Ne olacağını gör

```
┌──────────────────────────────────────────────────────────┐
│ ← Geri               Hesabımı sil                        │
├──────────────────────────────────────────────────────────┤
│ Bu işlem GERİ ALINAMAZ. Hesabın hemen ve kalıcı          │
│ olarak silinecek.                                        │
│                                                          │
│ ── Silinecekler ──────────────────────────────────────── │
│ Özgeçmişin, fotoğrafın, belgelerin ve sertifika          │
│ dosyaların                                               │
│ Hesap bilgilerin: ad, e-posta, telefon, doğum tarihi     │
│ Başvuru, davet ve süreç geçmişin                         │
│ Bildirimlerin ve ayarların                               │
│                                                          │
│ ── Devam edenler sonlanacak ──────────────────────────── │
│ 3 bekleyen başvuru         geri çekilecek                │
│ 2 cevap bekleyen davet     kabul edilmemiş sayılacak     │
│ 1 süreçteki ilan           süreçten çekileceksin         │
│ 1 işe alım onayı           cevapsız kalacak              │
│                                                          │
│ ── Bilmen gerekenler ─────────────────────────────────── │
│ İşverenlerin daha önce indirdiği belgeler onlarda        │
│ kalabilir.                                               │
│ İşverenlerle yazışmalarında adın "Silinmiş kullanıcı"    │
│ olarak görünecek.                                        │
│                                                          │
│ Neden ayrılıyorsun? (opsiyonel)                          │
│ ○ İş buldum                                              │
│ ○ Uygun ilan bulamadım                                   │
│ ○ Başka bir uygulama kullanacağım                        │
│ ○ Çok fazla bildirim geliyor                             │
│ ○ Diğer → [________________________]                     │
│                                                          │
│ [Vazgeç]                          [Hesabımı sil]         │
└──────────────────────────────────────────────────────────┘
```

"Devam edenler" adayın **gerçek sayılarıyla** dolar; hiçbir şey yoksa bölüm görünmez.

### 15.3 Adım 3 — SMS doğrulama

```
┌──────────────────────────────────────────────┐
│ Güvenliğin için telefonuna gelen 6 haneli    │
│ kodu gir.                                    │
│ +90 532 *** ** 67                            │
│ [______]                                     │
│                                              │
│ [Vazgeç]                     [Sil]           │
└──────────────────────────────────────────────┘
```

**Neden:** Geri dönüş yok — telefonu veya şifresi ele geçirilmiş biri hesabı silemesin.

### 15.4 Adım 4 — Silindi

```
┌──────────────────────────────────────────────────────────┐
│                                                          │
│                  Hesabın silindi                         │
│                                                          │
│  Bilgilerin kalıcı olarak silindi.                       │
│  İstediğin zaman yeniden kaydolabilirsin.                │
│                                                          │
│                     [Tamam]                              │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

**[Tamam]** → giriş ekranı · ✉️ *"Hesabın silindi."* (bildirim 8.4)

### 15.5 Arkada ne olur

| | |
|---|---|
| Özgeçmiş, hesap bilgileri, fotoğraf | 🗑 **Hemen** silinir |
| Belgeler ve sertifika dosyaları | 🗑 Silinir |
| Eşleşme satırları, embedding, "bir daha gösterme" kayıtları | 🗑 Silinir |
| Bildirimler, ayarlar, oturumlar | 🗑 Silinir · tüm cihazlardan çıkış |
| **İşverenin listelerindeki başvuru / davet / süreç kayıtları** | **İsimsiz kalır** — ad yerine *"Silinmiş kullanıcı"*, kişisel bilgi yok · listelerde 90 gün, tamamı ilan sonuçlandıktan 2 yıl sonra silinir |
| Şirket ve platform istatistikleri | **Değişmez** — isimsiz toplu sayılar |
| Aynı e-posta / telefonla tekrar kayıt | ✅ Sıfırdan yeni hesap · eski hiçbir şey gelmez |

**Neden işverende kayıt tamamen yok olmuyor:** İşverenin kendi geçmişi (*"bu ilana kim başvurmuştu, kimi işe aldık"*) bir anda kaybolursa ne olduğunu anlamaz. Kayıt kalır ama **kim olduğu artık bilinmez.**

### 15.6 İşveren ne görür — bütün ihtimaller

**Temel kural:** Adayın kişisel bilgileri hemen silinir. İşverenin ekranında kayıt **kalır ama isimsiz olur.**

| # | Aday silindiğinde durum | İşveren ne görür | İşverene bildirim |
|:---:|---|---|---|
| A1 | Aday sadece işverenin **Adaylar** listesinde | Listeden **kaybolur** | — |
| A2 | İşveren adayı **"Bir daha gösterme"** demişti | Hiçbir şey | — |
| A3 | Aday işverenin **karşılaştırma listesinde** | Listeden otomatik düşer | — |
| B1 | **Bekleyen başvuru** | Başvuranlar → **Sonuçlanan**: *"Silinmiş kullanıcı · Aday hesabını kapattı"* | 🔔 sorumluya |
| B2 | Başvuru **zaten sonuçlanmış** | Kart kalır, sonuç aynı, ad *"Silinmiş kullanıcı"* | — |
| C1 | **Cevap bekleyen davet** | Davet Edilenler → **Sonuçlanan**: *"Silinmiş kullanıcı · Aday hesabını kapattı"* | 🔔 sorumluya |
| C2 | Davet **zaten sonuçlanmış** | Kart kalır, sonuç aynı, ad *"Silinmiş kullanıcı"* | — |
| D1 | **Süreç devam ediyor** | Süreçteki → **Sonuçlanan**: *"Silinmiş kullanıcı · Aday hesabını kapattı"* · sohbet kapanır | 📱 🔔 sorumluya · 🔔 sahip / yönetici |
| D2 | Süreç **zaten sonuçlanmış** | Kart kalır, sonuç aynı, ad *"Silinmiş kullanıcı"* | — |
| E1 | İşe alındı, **onay bekleniyordu** | İşe alındı kalır · onay *"Aday cevap vermedi"* olarak kesinleşir · [Adaya tekrar sor] ve [Yanlış işaretledim] kalkar | — |
| E2 | İşe alındı, **aday onaylamıştı** | ✓ Onaylı kalır · ad *"Silinmiş kullanıcı"* | — |
| E3 | İşe alındı, **aday "işe alınmadım" demişti** | **Tartışmalı** olarak kesinleşir · admin listesinde kalır | — |
| F1 | Aday, sonuçlanmış bir ilanın **İlan özeti**'nde | Kart kalır, sonuç aynı, ad *"Silinmiş kullanıcı"* | — |

**İşveren ekranında örnekler:**

```
Süreçteki Adaylar → Sonuçlanan
┌──────────────────────────────────────────────────────────┐
│ [?]  Silinmiş kullanıcı · Backend Geliştirici            │
│ ■ Aday hesabını kapattı · 28 Eylül         [Sohbeti gör] │
└──────────────────────────────────────────────────────────┘

Sonuçlananlar → ilan kartı → İşe alınanlar
│ İşe alınanlar                                            │
│    Ayşe Kaya             ✓ Aday onayladı                 │
│    Silinmiş kullanıcı    ✓ Aday onayladı                 │
│    Silinmiş kullanıcı    - Aday cevap vermedi            │

Sohbetin sonu
│        Bu kullanıcı hesabını sildi · 28 Eylül            │
│        Bu sohbet kapandı.                                │
├──────────────────────────────────────────────────────────┤
│ Bu süreç sona erdi, yeni mesaj gönderilemez.             │
```

**Her durumda ortak:**

| | Aday silindikten sonra işverende |
|---|---|
| Ad, fotoğraf | **"Silinmiş kullanıcı"** · gri simge |
| Unvan | Kalır — hangi pozisyondaki aday olduğu anlaşılsın |
| Özgeçmiş, profil detayı | ❌ Açılmaz |
| Belgeler, PDF | ❌ Yok — işe alınanda bile |
| Telefon, e-posta | ❌ Gizlenir |
| Maaş beklentisi, not | ❌ Gösterilmez |
| Sohbet geçmişi | ✅ Salt okunur |
| Önceden indirilmiş PDF | Platform silemez, işverende kalır |

**Şirket raporuna etkisi:**

| Durum | Raporda |
|---|---|
| Bekleyen başvuru silindi | **Cevapsız sayılmaz** |
| Bekleyen davet silindi | Davet sayısında kalır, cevapsız sayılmaz |
| Süreç silindi | "Sürece alınan"da kalır |
| İşe alımlar | Değişmez |

Rapor günlük istatistik defterinden okunduğu için geriye dönük sayılar değişmez.

### 15.7 Akış

```
Ayarlar → [Hesabımı sil]
   │
   v
Adım 1: Alternatif
   ├─ [Şu an iş aramıyorum yap] ──> Arama durumu değişir · Ayarlar'a dönüş
   └─ [Devam et →]
        │
        v
Adım 2: Ne olacağını gör
   ├─ [Vazgeç] ──> Ayarlar
   └─ [Hesabımı sil]
        │
        v
Adım 3: SMS kodu
   ├─ [Vazgeç] ──> Ayarlar
   └─ [Sil]
        │
        v
Adım 4: "Hesabın silindi" ──> [Tamam] ──> Giriş ekranı
   + bekleyen başvurular, davetler, süreçler kapanır
   + işverenlere bildirim (B1, C1, D1)
   + e-posta (8.4)
```

---

## 16. Süreler — Aday Açısından

| Ne | Süre | Adaya gösterilir mi | Süre dolunca |
|---|---|:---:|---|
| Başvuru — işverenin cevabı | **30 gün** | ❌ | ~ Yanıtlanmadan sona erdi |
| Davet — adayın cevabı | **10 gün** | ✅ *"X gün içinde cevap ver"* · 2 gün kala uyarı | ~ Cevap vermediğin için sona erdi |
| Süreç | **60 gün** | ❌ | ~ Süreç sona erdi |
| İşe alım onayı — cevap | **14 gün** | — | *"Cevap vermedin"* · [Cevap ver] açık kalır |
| İşe alım onayı — değiştirme | **30 gün** | — | Kayıt kesinleşir |
| Sonuçlanan kartlar listede | **90 gün** | ✅ *"Son 90 gün"* yazısı | Listeden kalkar |
| Zil bildirimleri | **6 ay** | — | Silinir |
| Arama durumu — Aktif → Fırsatlara açık | **30 gün** giriş yok | ✅ 23. gün uyarı | Düşer |
| Arama durumu — → Şu an iş aramıyorum | **90 gün** giriş yok | ✅ 83. gün uyarı | Özgeçmiş gizlenir |
| Başvuru ayrıntıları (maaş, not) | Sonuçlandıktan **1 yıl** | — | Silinir |

Tüm süreler veritabanında — gerçek veriyle ayarlanabilir. Zamanla çalışan işler → `bakim-isleri.md`.

---

## 17. Diğer Dokümanlara Etkisi ve Açık Konular

### 17.1 Güncellenecek dokümanlar

| Doküman | Değişecek |
|---|---|
| **`aday-profili-final.md`** | Sekmenin adı **Özgeçmiş** · ad, telefon, e-posta, doğum tarihi **Profilim**'e taşındı · Adım 1'de kalanlar · aday ekranları özeti bu dokümana yönlendirilir |
| **`eslestirme-motoru.md`** | Aday hesabını silince: kişisel bilgiler ve aday tarafındaki kayıtlar hemen silinir; **işverenin listelerindeki başvuru / davet / süreç kayıtları isimsiz kalır**, 2 yıl kuralıyla silinir |
| **`bakim-isleri.md`** | Aynı kural · zil bildirimleri 6 ay *(zaten yazılı)* · davet 2 gün kala uyarısı |
| **`isveren-paneli-final.md`** | Aday silinince *"Silinmiş kullanıcı"* gösterimi · admin ilanı kaldırınca süreçteki adaylara uyarı |

### 17.2 Yazılacak dokümanlar

| Doküman | Bu panelden bağlantılar |
|---|---|
| **Sohbet** | Mesajlaşma · iletişim bilgisi isteme ve paylaşma · sohbetin [⋮] menüsü (İlan detayı · Süreçten çekil · Bildir) · kapanan sohbet · sistem mesajları |
| **Admin paneli** | Bildirilen ilanlar · ilan kaldırma ve süreçteki adaylara uyarı · tartışmalı işe alımlar · paylaşım istatistikleri |
| **Veri koruma** | Aydınlatma metni · belge indirme · silinen adayın isimsiz kayıtları |

### 17.3 Açık konular

| # | Konu | Ne zaman |
|:---:|---|---|
| 1 | Aday silinince işverende **isimsiz kayıt ve sohbet geçmişinin** kalması — "silinme hakkı" açısından sohbet mesajlarının da silinmesi gerekebilir | Hukuki danışmanla |
| 2 | Silinen hesapla ilgili yasal saklama gereken kayıtlar (şikâyet, bildirim) | Hukuki danışmanla |
| 3 | Yurtdışı ilanlarda maaşın yanına **tahmini çeviri** ("≈ 1.950 EUR") | İleride, talep gelirse |
| 4 | Davetin 2 gün kala uyarısının günü | İlk gerçek veriyle |
