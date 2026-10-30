# İş Eşleştirme Projesi — İşveren Paneli (Final)

> **Sürüm:** Final — işveren tarafının tüm ekranları, akışları ve kuralları
> **Kapsam:** İşverenin platforma girdiği andan itibaren gördüğü her ekran, bastığı her buton ve sonucunda olan her şey
> **Kapsam dışı:** Eşleştirmenin nasıl hesaplandığı → `eslestirme-motoru.md` · Zamanla çalışan işler → `bakim-isleri.md` · İlan formu alanları → `is-ilani-v3.md`, `staj-ilani.md` · Aday ekranları ve aday bildirim kataloğu → `aday-paneli-final.md` · Sohbetin içi → sohbet dokümanı *(yazılacak)*

---

## İçindekiler

| # | Bölüm | Kısaca |
|:---:|---|---|
| 0 | Bu doküman nasıl okunur | Yapı, semboller, örnek şirket |
| 1 | Temel kavramlar | Roller, sorumlu, ilanın dört hali, dört liste, bekleyen / sonuçlanan, kart ve detay kuralı |
| 2 | Panel menüsü | Sahip, yönetici, uzman menüleri |
| 3 | İlanlarım | Dört sekme, Yayında listesi, ilan kartı, Taslak |
| 4 | İlanın yaşam döngüsü | Tüm geçişler, hangi durumda hangi buton, yayın süreleri |
| 5 | Yayındaki ilanı yönetmek | Uzat, Duraklat, İlanı devret, Kopyala |
| 6 | İlanın içi — Adaylar | Liste, aday detayı, Davet et, Bir daha gösterme |
| 7 | İlanın içi — Başvuranlar | Bekleyen, başvuru detayı, Kabul et / Kabul etme, Sonuçlanan |
| 8 | İlanın içi — Davet Edilenler | Bekleyen, Davetimi geri çek, aday cevapları, Sonuçlanan |
| 9 | İlanın içi — Süreçteki Adaylar | Devam eden, Süreci sonuçlandır, 60 gün, Sonuçlanan, işe alım onayı |
| 10 | Karşılaştırma ve PDF | Karşılaştırma listesi, PDF indirme kuralları |
| 11 | Yayında olmayanlar | Duraklatıldı, Süresi doldu (30 + 45 gün) |
| 12 | Sonuç bildir | Üç sonuç, işe alınanlar, adaya mesajlar |
| 13 | Sonuçlananlar | Dört sonuç, İlan özeti, Sil |
| 14 | Şirket profili ve şirket raporu | Dört görünüm, rapor kuralları, Düzenle, Değişiklik talebi |
| 15 | Ekip | Üç görünüm, üye ekle, roller, rol değiştir, ilan devri, ekipten çıkar |
| 16 | Yetkiler | Tam yetki tablosu |
| 17 | Şirket Yönetimi | Belgeler, değişiklik taleplerim, sahiplik devri, şirket hesabını sil |
| 18 | Profilim | |
| 19 | Ayarlar | |
| 20 | İşverene bildirimler | Tam katalog |
| 21 | Süreler ve zamanlayıcılar | Tek tablo |
| 22 | Veri | Ne zaman ne silinir, istatistik tabloları |
| 23 | Diğer dokümanlara etkisi | |
| 24 | Açık konular | |

---

## 0. Bu Doküman Nasıl Okunur

### 0.1 Yapı

Her ekran aynı düzenle anlatılır:

1. **Ekran** — çizimi
2. **Kim görür** — hangi rol, hangi durumda
3. **Ekranda ne var** — her parçanın anlamı
4. **Butonlar** — her buton: **neden var → basınca ne açılır → sonucunda ne olur → kime ne gider**
5. **Kurallar** — süreler, sınırlar, istisnalar

### 0.2 Semboller

| Sembol | Anlamı |
|---|---|
| `[Buton]` | Dokunulabilir buton |
| `[Seçim ▾]` | Açılır liste |
| `○` / `●` | Tek seçim — boş / seçili |
| `☐` / `☑` | Çoklu seçim — boş / seçili |
| `[⋮]` | Üç nokta menüsü — ek işlemler |
| `▔▔▔` | Seçili sekme |
| `▸` | Seçili alt sekme |
| `📱` | Telefona bildirim (push) |
| `🔔` | Uygulama içi bildirim |
| `✉️` | E-posta |
| `🔒` | Kilitli / erişim yok |
| `⚠️` | Uyarı — işlem bekleyen veya süresi azalan |
| `🔴` | O ilanda işverenin karar vermesi gereken bir şey var |

### 0.3 Çizimlerdeki işaretler

Ekran çizimleri her yazı tipinde düzgün hizalansın diye çizimlerin **içinde** emoji yerine sade işaretler kullanıldı. Gerçek arayüzde bunlar renkli rozet ve simge olarak tasarlanacak:

| Çizimde | Arayüzde | Anlamı |
|---|---|---|
| `● Yayında` | 🟢 yeşil rozet | İlan yayında |
| `‖ Duraklatıldı` | ⏸ | İlan duraklatıldı |
| `● Yüksek uyum` · `○ Orta uyum` · `· Düşük uyum` | 🟢 · 🟡 · ⚪ | Uyum seviyesi |
| `● Aktif iş arıyorum` · `○ Fırsatlara açığım` | 🟢 · 🟡 | Adayın arama durumu |
| `!` | 🔴 / ⚠️ | İşlem bekliyor veya süre azaldı |
| `×` | 🔒 | Kilitli / erişim yok |
| `↻` | ⟳ | Hazırlanıyor |
| `~` | ⏳ ⌛ | Bekliyor / süre doldu |
| `✓` · `✕` · `?` | ✅ · ❌ · ❓ | Olumlu · olumsuz · bilinmiyor |
| `■` | ⏹ | Süreç sonlandırıldı |
| `*` | 🎉 | İşe alındı |
| `[↓ PDF]` | ⬇ PDF | PDF indirme butonu |
| `i` | ⓘ | Bilgi |
| *(boşluk)* | 👤 📍 ⏱ 👁 📨 ✉️ 🤝 💰 📄 | Satır başı simgeleri — sorumlu, konum, süre, görüntülenme, başvuru, davet, süreç, maaş, belge |

Tablolarda ve metinde emojiler olduğu gibi kullanıldı.

### 0.4 Örnek şirket ve kişiler

Tüm ekranlarda aynı örnek kullanılır:

| | |
|---|---|
| Şirket | **ABC Teknoloji** — Bilişim ve Yazılım, Kadıköy / İstanbul |
| 👑 Sahip | **Ali Kaya** |
| 🛡 Yönetici | **Mehmet Kaya** |
| 👤 Uzman | **Zeynep Ak**, **Can Demir** |
| Örnek ilan | **Senior Backend Developer** — sorumlusu Mehmet Kaya |
| Örnek adaylar | Ayşe Kaya, Selin Er, Burak Can, Emre Yıldız |

---

## 1. Temel Kavramlar

Ekranlara geçmeden önce ekibin bilmesi gereken altı kavram.

### 1.1 Üç rol

| Rol | Kim | Kim ekler | Kısaca |
|---|---|---|---|
| 👑 **Sahip** | Şirketi platforma kaydeden kişi | — *(işveren başvurusuyla gelir)* | Her şey + ekip ve şirket yönetimi · **şirkette tek kişi** |
| 🛡 **Yönetici** | İK müdürü, ekip lideri | Sahip | Şirketin **tüm** ilanlarını görür ve yönetir · ekibi yönetemez |
| 👤 **Uzman** | İK uzmanı, işe alım sorumlusu | Sahip | Sadece **kendi** ilanlarını görür ve yönetir |

### 1.2 İlanın sorumlusu

Her ilanın **bir sorumlusu** vardır — ilanı açan kişi ya da ilanın devredildiği kişi.

| | |
|---|---|
| Telefona bildirim (📱) | **Sadece sorumluya** gider — büyük şirkette herkesin telefonu çalmasın |
| Sahip ve yönetici | Başkasının ilanında işlem yapabilir, ama ilan sorumlunun kalır · uygulama içi bildirim (🔔) alır |
| Uzman | Sadece sorumlusu olduğu ilanları görür |

### 1.3 İlanın dört hali

```
┌──────────┐ Yayınla ┌───────────┐  Sonuç bildir   ┌──────────────┐
│  TASLAK  │────────>│  YAYINDA  │────────────────>│  SONUÇLANAN  │
└──────────┘         └──┬─────┬──┘                 └──────┬───────┘
            Duraklat /  │     ^ Devam ettir /             ^
            süre doldu  │     │ Tekrar yayınla            │
                        │     │ (ilk 30 gün)              │
                        v     │                           │
                  ┌─────┴─────┴───────┐                   │
                  │  YAYINDA OLMAYAN  │                   │
                  │    Duraklatıldı   │───────────────────┘
                  │    Süresi doldu   │  Sonuç bildir
                  │ (geri açılabilir) │  (75. günde: sistem)
                  └───────────────────┘

```

| Hal | Adaylar görür mü | Yeni başvuru / davet | İçerideki işler | Geri açılır mı |
|---|:---:|:---:|:---:|:---:|
| 📝 Taslak | ❌ | ❌ | — | — |
| 🟢 Yayında | ✅ | ✅ | ✅ Devam | — |
| ⏸ ⏱ Yayında olmayan | ❌ | ❌ | ✅ Devam | ✅ |
| ✓ ✕ ❓ Sonuçlanan | ❌ | ❌ | ❌ Hepsi bitti | ❌ |

> **Tek cümle:** Yayından kalkan ilan **yeni giriş almaz, içerideki her şey devam eder.** Sonuç bildirilen ilanda ise **her şey biter.**

### 1.4 Bir ilanın içindeki dört liste

Her ilanın içinde adaylar dört listeden birindedir. **Bir aday bir ilan için aynı anda sadece bir listede olur** — taşınır, kopyalanmaz.

```
                  ┌───────────────────┐
                  │      ADAYLAR      │  Uygun adaylar — işverenin Keşfet'i
                  └─────────┬─────────┘
                            │
         ┌──────────────────┴──────────────────┐
         │ İşveren davet etti                  │ Aday başvurdu
         v                                     v
┌─────────────────┐                   ┌─────────────────┐
│ DAVET EDİLENLER │                   │   BAŞVURANLAR   │
└────────┬────────┘                   └────────┬────────┘
         │ Aday kabul etti                     │ İşveren kabul etti
         └──────────────────┬──────────────────┘
                            v
                  ┌───────────────────┐
                  │ SÜREÇTEKİ ADAYLAR │
                  │   (sohbet açık)   │
                  └───────────────────┘
```

| Liste | İçinde kim var | Kim karar verir |
|---|---|---|
| **Adaylar** | İlana uygun, henüz kimsenin bir şey yapmadığı adaylar | İşveren — davet etsin mi |
| **Başvuranlar** | Başvuran adaylar | **İşveren** — kabul / red |
| **Davet Edilenler** | İşverenin davet ettiği adaylar | **Aday** — kabul / red |
| **Süreçteki Adaylar** | İki tarafın da "evet" dediği adaylar | İşveren — işe alındı / devam etmiyoruz · Aday — çekilebilir |

### 1.5 Bekleyen ve Sonuçlanan

Başvuranlar, Davet Edilenler ve Süreçteki Adaylar listelerinin her biri ikiye ayrılır:

| Alt sekme | İçinde |
|---|---|
| **Bekleyen** *(Süreçteki'de: Devam eden)* | Henüz bitmemiş olanlar — iş bekliyor |
| **Sonuçlanan** | Bitmiş olanlar — sadece kayıt, **son 90 gün** |

> **Neden ayrı "Reddedilenler" sekmesi yok:** Reddedilen, süresi dolan, geri çekilen — hepsi aynı şey: **bitmiş.** Bekleyenler önde, bitenler bir dokunuş uzakta.

### 1.6 Kart ve detay — tek kural

> **Kartta incele, detayda karar ver.**

| | Kartta | Detayda |
|---|:---:|:---:|
| Bilgiye hızlı bakış | ✅ | ✅ |
| "Bir daha gösterme" · "Karşılaştır" · "PDF" | ✅ | ✅ |
| **Davet et · Kabul et · Kabul etme · Süreci sonuçlandır** | ❌ | ✅ |

**Neden:** İşveren adayın profilini görmeden karar vermesin. Kabul, red, davet bilinçli olsun.

**Detay ekranlarında karar butonları her zaman üstte ve sabittir** — sayfa aşağı kaydırılsa da görünür.

---

## 2. Panel Menüsü

İşveren giriş yapınca **sol menü** (masaüstü) veya **alt menü** (mobil) görür. Menü **role göre** değişir.

### 2.1 Sahip

```
┌───────────────────────────┐
│ ABC Teknoloji             │
│ Ali Kaya · Sahip          │
├───────────────────────────┤
│ İlanlarım                 │
│ Şirket profili            │
│ Ekip                      │
│ Şirket Yönetimi           │
│ Profilim                  │
│ Ayarlar                   │
│                           │
│ 3                         │
└───────────────────────────┘
```

### 2.2 Yönetici

```
┌───────────────────────────┐
│ ABC Teknoloji             │
│ Mehmet Kaya · Yönetici    │
├───────────────────────────┤
│ İlanlarım                 │
│ Şirket profili            │
│ Ekip                      │
│ Profilim                  │
│ Ayarlar                   │
│                           │
│ 5                         │
└───────────────────────────┘
```

### 2.3 Uzman

```
┌───────────────────────────┐
│ ABC Teknoloji             │
│ Zeynep Ak · Uzman         │
├───────────────────────────┤
│ İlanlarım                 │
│ Şirket profili            │
│ Ekip                      │
│ Profilim                  │
│ Ayarlar                   │
│                           │
│ 1                         │
└───────────────────────────┘
```

### 2.4 Menüler kimde var

| Menü | Sahip | Yönetici | Uzman | Bölüm |
|---|:---:|:---:|:---:|:---:|
| 📋 İlanlarım | ✅ Şirketin tüm ilanları | ✅ Şirketin tüm ilanları | ✅ Sadece kendi ilanları | 3 |
| 🏢 Şirket profili | ✅ Görür + düzenler | ✅ Görür + düzenler | ✅ Sadece görür | 14 |
| 👥 Ekip | ✅ Tam yönetim | ✅ Görür + ilan devreder | ✅ Sadece görür | 15 |
| 🔐 Şirket Yönetimi | ✅ | ❌ Menüde yok | ❌ Menüde yok | 17 |
| 👤 Profilim | ✅ | ✅ | ✅ | 18 |
| ⚙️ Ayarlar | ✅ | ✅ | ✅ | 19 |
| 🔔 Bildirimler | ✅ | ✅ | ✅ | 20 |

### 2.5 Panel açılınca

| Durum | Ne açılır |
|---|---|
| Normal giriş | **📋 İlanlarım → Yayında** |
| Bekleyen bir sahiplik devri isteği varsa | İlanlarım + en üstte **sahiplik bandı** (Bölüm 17.4) |
| Şirkette hiç ilan yoksa | İlanlarım — boş ekran + **[+ Yeni ilan]** |

---
## 3. İlanlarım

### 3.1 Dört sekme

**Menü → 📋 İlanlarım**

```
┌────────────────────────────────────────────────────────────────────┐
│ İlanlarım                                            [+ Yeni ilan] │
│                                                                    │
│ [Yayında 3]  [Yayında olmayanlar 2]  [Sonuçlananlar 4]  [Taslak 2] │
│  ▔▔▔▔▔▔▔▔▔▔                                                        │
└────────────────────────────────────────────────────────────────────┘
```

| Sekme | İçinde | Bölüm |
|---|---|:---:|
| **Yayında** | Adaylara görünen, aktif ilanlar | 3.3 |
| **Yayında olmayanlar** | ⏸ Duraklatılmış · ⏱ Süresi dolmuş — **geri açılabilir** | 11 |
| **Sonuçlananlar** | ✓ Pozisyon doldu · ✕ Uygun aday bulunamadı · ✕ Alımdan vazgeçildi · ❓ Sonuç bildirilmedi — **kalıcı** | 13 |
| **Taslak** | Henüz yayınlanmamış | 3.6 |

| Kural | |
|---|---|
| Sayılar | Her sekmenin yanında o sekmedeki ilan sayısı |
| Açılışta | **Yayında** seçili gelir |
| Sayı 0 ise | Sekme yine görünür · içi boşsa *"Bu durumda ilanınız yok."* |
| **Sahip / yönetici** | Sayılar şirketin **tüm** ilanlarına göre |
| **Uzman** | Sayılar sadece **kendi** ilanlarına göre |

### 3.2 [+ Yeni ilan]

| | |
|---|---|
| Kim görür | Herkes — sahip, yönetici, uzman |
| Basınca | İlan formu açılır *(alanlar → `is-ilani-v3.md`, `staj-ilani.md`)* |
| Form yarıda bırakılırsa | Otomatik olarak **Taslak**'a kaydedilir |
| Sorumlu | İlanı açan kişi |

### 3.3 Yayında — liste ekranı

**Sahip / yönetici görünümü:**

```
┌────────────────────────────────────────────────────────────────────┐
│ İlanlarım                                            [+ Yeni ilan] │
│                                                                    │
│ [Yayında 3]  [Yayında olmayanlar 2]  [Sonuçlananlar 4]  [Taslak 2] │
│  ▔▔▔▔▔▔▔▔▔▔                                                        │
│ Sorumlu: [Tümü ▾]                    Sırala: [Bekleyen başvuru ▾]  │
├────────────────────────────────────────────────────────────────────┤
│                                                                    │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ Bilişim ve Yazılım                              ● Yayında      │ │
│ │ Senior Backend Developer                                  [⋮]  │ │
│ │ Mehmet Kaya · Kadıköy, İstanbul · 12 gün kaldı                 │ │
│ │ 340 görüntülenme                                               │ │
│ │                                                                │ │
│ │    124          8 !           10                7              │ │
│ │ [Adaylar]  [Başvuranlar]  [Davet Edilenler]  [Süreçteki]       │ │
│ │                                                                │ │
│ │ [Uzat]        [Duraklat]        [Sonuç bildir]                 │ │
│ └────────────────────────────────────────────────────────────────┘ │
│                                                                    │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ Finans ve Muhasebe                              ● Yayında      │ │
│ │ Muhasebe Uzmanı                                           [⋮]  │ │
│ │ Zeynep Ak · Çankaya, Ankara · ! 3 gün kaldı                    │ │
│ │ 85 görüntülenme                                                │ │
│ │                                                                │ │
│ │     42          2              0                1              │ │
│ │ [Adaylar]  [Başvuranlar]  [Davet Edilenler]  [Süreçteki]       │ │
│ │                                                                │ │
│ │ [Uzat]        [Duraklat]        [Sonuç bildir]                 │ │
│ └────────────────────────────────────────────────────────────────┘ │
│                                                                    │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ Lojistik ve Depo                                ● Yayında      │ │
│ │ Depo Sorumlusu                                            [⋮]  │ │
│ │ Can Demir · Gebze, Kocaeli · 58 gün kaldı                      │ │
│ │ 3 görüntülenme                                                 │ │
│ │                                                                │ │
│ │     ↻           0              0                0              │ │
│ │ [Adaylar]  [Başvuranlar]  [Davet Edilenler]  [Süreçteki]       │ │
│ │ Uygun adaylar hazırlanıyor...                                  │ │
│ │                                                                │ │
│ │ [Uzat]        [Duraklat]        [Sonuç bildir]                 │ │
│ └────────────────────────────────────────────────────────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

**Uzman görünümü (Zeynep Ak):** Aynı ekran, üç farkla:

```
┌────────────────────────────────────────────────────────────────────┐
│ İlanlarım                                            [+ Yeni ilan] │
│                                                                    │
│ [Yayında 1]  [Yayında olmayanlar 0]  [Sonuçlananlar 1]  [Taslak 0] │
│  ▔▔▔▔▔▔▔▔▔▔                                                        │
│                                      Sırala: [Bekleyen başvuru ▾]  │
├────────────────────────────────────────────────────────────────────┤
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ Finans ve Muhasebe                              ● Yayında      │ │
│ │ Muhasebe Uzmanı                                           [⋮]  │ │
│ │ Zeynep Ak · Çankaya, Ankara · ! 3 gün kaldı                    │ │
│ │ 85 görüntülenme                                                │ │
│ │                                                                │ │
│ │     42          2              0                1              │ │
│ │ [Adaylar]  [Başvuranlar]  [Davet Edilenler]  [Süreçteki]       │ │
│ │                                                                │ │
│ │ [Uzat]        [Duraklat]        [Sonuç bildir]                 │ │
│ └────────────────────────────────────────────────────────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

| Fark | Sahip / yönetici | Uzman |
|---|---|---|
| Görünen ilanlar | Şirketin tümü | Sadece kendisinin |
| **Sorumlu** süzgeci | Var | **Yok** — zaten tek kişi |
| [⋮] menüsü | İlanı devret · Kopyala | Sadece **Kopyala** |

### 3.4 İlan kartında ne var

| Parça | Anlamı |
|---|---|
| Meslek alanı · unvan | |
| Durum | 🟢 Yayında |
| 👤 Sorumlu | İlanın sorumlusu |
| 📍 Konum | İlanın kendi konumu |
| ⏱ Kalan gün | Yayın süresinin bitmesine kaç gün kaldığı · **7 gün ve altında ⚠️** |
| 👁 Görüntülenme | Kaç **farklı** aday ilanın detayını açtı *(dakikada bir güncellenir)* |
| **Dört sayı** | Uygun aday · **bekleyen** başvuru · **cevap bekleyen** davet · **devam eden** süreç |
| 🔴 | Bekleyen başvurusu olan ilan — işverenin karar vermesi gerekiyor |
| ⟳ *Hazırlanıyor* | Eşleştirme henüz bitmedi — birkaç saniye içinde dolar |

**Neden sadece bekleyenler sayılıyor:** Kart **"neye bakmam lazım"** sorusunu cevaplar. Reddedilen, süresi dolan başvurular iş gerektirmiyor — onlar ilanın içinde **Sonuçlanan** alt sekmesinde.

### 3.5 Karttaki dokunma noktaları

| Dokunulan yer | Ne açılır |
|---|---|
| **Kartın boş bir yeri** | İlanın içi — **Adaylar** sekmesi seçili *(Bölüm 6)* |
| **Dört sayıdan birine** | İlanın içi — **o sekme** seçili *(ör. "8 Başvuranlar" → Başvuranlar)* |
| **[Uzat]** | Uzatma penceresi *(Bölüm 5.1)* |
| **[Duraklat]** | Duraklatma penceresi *(Bölüm 5.2)* |
| **[Sonuç bildir]** | Sonuç bildir ekranı *(Bölüm 12)* |
| **[⋮]** | İlanı devret · Kopyala *(Bölüm 5.3, 5.4)* |

### 3.6 Süzgeç ve sıralama — Yayında

| | Seçenekler |
|---|---|
| **Sorumlu** *(sahip / yönetici)* | Tümü · ekipteki her kişi |
| **Sırala** | Bekleyen başvuru *(varsayılan)* · Kalan gün · En yeni · En çok uygun aday |

**Neden varsayılan "bekleyen başvuru":** İşverenin en önemli işi cevap bekleyen adaylar — 30 gün içinde cevap vermezse başvuru düşüyor ve şirket raporunda **cevapsız** görünüyor. En çok bekleyeni olan ilan en üstte.

### 3.7 Taslak

**İlanlarım → Taslak**

```
┌────────────────────────────────────────────────────────────────────┐
│ [Yayında 3]  [Yayında olmayanlar 2]  [Sonuçlananlar 4]  [Taslak 2] │
│                                                         ▔▔▔▔▔▔▔▔   │
│ Sorumlu: [Tümü ▾]                      Sırala: [Oluşturma ▾]       │
├────────────────────────────────────────────────────────────────────┤
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ Bilişim ve Yazılım                              Taslak         │ │
│ │ Frontend Developer                                        [⋮]  │ │
│ │ Mehmet Kaya · Oluşturuldu: 5 Eylül                             │ │
│ │ ~ 18 gün sonra silinecek                                       │ │
│ │                                                                │ │
│ │ ✓ Hazır — yayınlanabilir                                       │ │
│ │                                                                │ │
│ │ [Sil]              [Düzenle]              [Yayınla]            │ │
│ └────────────────────────────────────────────────────────────────┘ │
│                                                                    │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ Finans ve Muhasebe                              Taslak         │ │
│ │ Muhasebe Uzmanı                                           [⋮]  │ │
│ │ Zeynep Ak · Oluşturuldu: 18 Eylül                              │ │
│ │ Kopyalandı: "Muhasebe Uzmanı" (Ağustos)                        │ │
│ │ ! 5 gün sonra silinecek                                        │ │
│ │                                                                │ │
│ │ ! Eksik: İş tanımı · Eğitim şartı                              │ │
│ │                                                                │ │
│ │ [Sil]              [Düzenle]                                   │ │
│ └────────────────────────────────────────────────────────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

**Taslağa nasıl düşer:**

| Yol | |
|---|---|
| [+ Yeni ilan] → form yarıda bırakıldı | Her adımda otomatik kaydedilir |
| Formda **[Taslak kaydet]** | Bilerek kaydetti |
| Başka bir ilanda **[⋮] → Kopyala** | Kopya taslak olarak açılır |

**Kartta:**

| Parça | |
|---|---|
| Unvan | Girilmediyse *"Başlıksız ilan"* |
| Oluşturuldu | Taslağın ilk oluşturulduğu gün |
| ⏳ Kalan gün | Silinmesine kaç gün kaldı · **7 gün ve altında ⚠️** |
| 📋 Kopyalandı | Kopyadan geldiyse hangi ilandan |
| ✅ Hazır / ⚠️ Eksik | Zorunlu alanlar tamam mı — eksikse hangileri |

**Butonlar:**

| Buton | Neden | Basınca |
|---|---|---|
| **[Düzenle]** | Taslağı tamamlamak | İlan formu kaldığı adımdan açılır |
| **[Yayınla]** | Sadece **hazırsa** görünür | Önizleme → **[Yayınla]** → ilan **Yayında**'ya geçer, eşleştirme başlar |
| **[Sil]** | Vazgeçti | Onay: *"Taslak kalıcı olarak silinecek."* → **tamamen** silinir *(hiç yayınlanmadığı için rapora girmez)* |
| **[⋮]** | | İlanı devret · Kopyala |

**Kurallar:**

| Kural | |
|---|---|
| **Süre** | Taslak **oluşturulduğu günden itibaren 30 gün** — düzenlemek süreyi uzatmaz |
| **23. gün** | Sorumluya 🔔 *"Frontend Developer taslağınız 7 gün sonra silinecek. Yayınlamak ister misiniz?"* **[Taslağa git]** |
| **30. gün** | Taslak tamamen silinir |
| Kopyalanan ilan | Yeni taslak sayılır — 30 gün kopyalandığı günden başlar |
| Taslakta eşleştirme | **Çalışmaz** — yayınlanınca başlar |
| Taslakta LLM ("Algılanan" satırı) | **Çalışır** — işveren yayından önce kontrol eder |
| Meslek alanı ve unvan | Değiştirilebilir — **kilit yayından sonra** |
| Şirket raporu | Taslak **sayılmaz** |

**Neden 30 gün:** Bir ayda yayınlanmayan taslak büyük ihtimalle vazgeçilmiş — unutulmuş taslaklar birikmesin.

---

## 4. İlanın Yaşam Döngüsü

### 4.1 Akış

```
┌──────────┐ Yayınla ┌───────────┐  Sonuç bildir   ┌──────────────┐
│  TASLAK  │────────>│  YAYINDA  │────────────────>│  SONUÇLANAN  │
└──────────┘         └──┬─────┬──┘                 └──────┬───────┘
            Duraklat /  │     ^ Devam ettir /             ^
            süre doldu  │     │ Tekrar yayınla            │
                        │     │ (ilk 30 gün)              │
                        v     │                           │
                  ┌─────┴─────┴───────┐                   │
                  │  YAYINDA OLMAYAN  │                   │
                  │    Duraklatıldı   │───────────────────┘
                  │    Süresi doldu   │  Sonuç bildir
                  │ (geri açılabilir) │  (75. günde: sistem)
                  └───────────────────┘

```

### 4.2 Tüm geçişler

| Nereden | Ne olur | Kim | Nereye |
|---|---|---|---|
| 📝 Taslak | [Yayınla] | İşveren | 🟢 Yayında |
| 📝 Taslak | 30 gün doldu | Sistem | 🗑 Silinir |
| 🟢 Yayında | [Duraklat] | İşveren | ⏸ Duraklatıldı |
| 🟢 Yayında | Yayın süresi doldu | Sistem | ⏱ Süresi doldu |
| 🟢 Yayında | [Sonuç bildir] | İşveren | ✓ / ✕ Sonuçlanan |
| ⏸ Duraklatıldı | [Devam ettir] | İşveren | 🟢 Yayında |
| ⏸ Duraklatıldı | Yayın süresi doldu | Sistem | ⏱ Süresi doldu |
| ⏸ Duraklatıldı | [Sonuç bildir] | İşveren | ✓ / ✕ Sonuçlanan |
| ⏱ Süresi doldu | [Tekrar yayınla] — **ilk 30 gün** | İşveren | 🟢 Yayında — yeni 60 gün |
| ⏱ Süresi doldu | [Sonuç bildir] — **75 gün içinde** | İşveren | ✓ / ✕ Sonuçlanan |
| ⏱ Süresi doldu | 75. gün — sonuç bildirilmedi | Sistem | ❓ Sonuç bildirilmedi |
| Sonuçlanan | [Sil] | İşveren | İşverenin listesinden kalkar *(soft delete)* |
| Herhangi biri | [⋮] → Kopyala | İşveren | Yeni 📝 Taslak *(orijinal değişmez)* |

### 4.3 Hangi durumda hangi butonlar

| Durum | Kartın altında | [⋮] — sahip / yönetici | [⋮] — uzman |
|---|---|---|---|
| 📝 Taslak | [Sil] · [Düzenle] · [Yayınla]* | İlanı devret · Kopyala | Kopyala |
| 🟢 Yayında | [Uzat] · [Duraklat] · [Sonuç bildir] | İlanı devret · Kopyala | Kopyala |
| ⏸ Duraklatıldı | [Devam ettir] · [Sonuç bildir] | İlanı devret · Kopyala | Kopyala |
| ⏱ Süresi doldu | [Tekrar yayınla]** · [Sonuç bildir] | İlanı devret · Kopyala | Kopyala |
| ✓ ✕ ❓ Sonuçlanan | [İlan özeti] · [Sil] | İlanı devret · Kopyala | Kopyala |

\* Sadece taslak hazırsa · \*\* Sadece süre dolduktan sonraki ilk 30 gün

> **Kural:** Kartın altında **o duruma özel** işler, [⋮] içinde **her durumda aynı** genel işler.

### 4.4 Yayın süreleri

| Kural | |
|---|---|
| İlk yayın | **60 gün** |
| Uzatma | 15 / 30 / 60 gün — **kesintisiz en fazla 180 gün** |
| Duraklatmak süreyi durdurur mu | **Hayır** — duraklatılmışken de süre işler |
| Tekrar yayınlama | Süre dolduktan sonraki **ilk 30 gün** — yeni 60 gün başlar |

---

## 5. Yayındaki İlanı Yönetmek

### 5.1 [Uzat]

**Neden:** İlan süresi bitmek üzere ama işveren hâlâ aday arıyor.

**Basınca:**

```
┌──────────────────────────────────────────────┐
│ İlanı uzat — Senior Backend Developer        │
│                                              │
│ Şu an: 12 gün kaldı · Bitiş: 30 Eylül        │
│                                              │
│ Ne kadar uzatmak istiyorsunuz?               │
│ ○ 15 gün   → Bitiş: 15 Ekim                  │
│ ● 30 gün   → Bitiş: 30 Ekim                  │
│ ○ 60 gün   → Bitiş: 29 Kasım                 │
│                                              │
│ İlan en fazla 180 gün kesintisiz yayında     │
│ kalabilir. Kalan hakkınız: 108 gün           │
│                                              │
│ [Vazgeç]                          [Uzat]     │
└──────────────────────────────────────────────┘
```

| | |
|---|---|
| Sonuç | Bitiş tarihi ileri gider · kartta yeni kalan gün |
| 180 günü aşan seçenek | Gri ve seçilemez |
| 180 gün dolunca | İlan **⏱ Süresi doldu** olur — devam etmek isterse **[Tekrar yayınla]** |
| Hatırlatma | Süre bitmeden **3 gün önce** sorumluya 🔔 *"İlanınızın süresi 3 gün sonra dolacak."* **[Uzat]** |
| Kim | Sorumlu · sahip · yönetici |

### 5.2 [Duraklat]

**Neden:** *"Çok başvuru geldi, önce bunları değerlendireyim. İlanı kapatmak istemiyorum, gerekirse tekrar açarım."*

**Basınca:**

```
┌──────────────────────────────────────────────┐
│ İlanı duraklat — Senior Backend Developer    │
│                                              │
│ İlan adaylara görünmeyecek, yeni başvuru     │
│ gelmeyecek, davet gönderemeyeceksiniz.       │
│                                              │
│ Bekleyen başvurular, davetler ve süreçteki   │
│ adaylarınız etkilenmez — onlarla çalışmaya   │
│ devam edebilirsiniz.                         │
│                                              │
│ i Yayın süresi işlemeye devam eder.          │
│                                              │
│ [Vazgeç]                      [Duraklat]     │
└──────────────────────────────────────────────┘
```

**Sonuç:** İlan **Yayında olmayanlar** sekmesine geçer, durum **⏸ Duraklatıldı**. Ayrıntılar → Bölüm 11.1.

### 5.3 [⋮] → İlanı devret

**Neden:** Sorumlu izne çıktı, ayrılıyor, ya da iş yükü dağıtılıyor.

**Kim görür:** Sahip, yönetici · Uzmanın [⋮]'sinde **yok**.

**Basınca:**

```
┌──────────────────────────────────────────────┐
│ "Senior Backend Developer" ilanını devret    │
│                                              │
│ Şu anki sorumlu: Mehmet Kaya                 │
│ Yeni sorumlu *:  [Zeynep Ak ▾]               │
│                                              │
│ İlanla birlikte geçecek:                     │
│ 8 bekleyen başvuru                           │
│ 10 cevap bekleyen davet                      │
│ 7 süreçteki aday ve sohbetleri               │
│                                              │
│ [Vazgeç]                       [Devret]      │
└──────────────────────────────────────────────┘
```

| | |
|---|---|
| Yeni sorumlu listesinde | Ekipteki **aktif** üyeler — askıdaki ve henüz giriş yapmamış üyeler yok, şu anki sorumlu yok |
| Ne geçer | İlan · bekleyen başvurular · davetler · süreçteki adaylar · sohbetler |
| Yeni sorumluya | 📱 🔔 *"Senior Backend Developer ilanı size devredildi."* |
| Eski sorumlu | Uzmansa ilanı **artık göremez** · yöneticiyse görmeye devam eder |
| Adaya | **Hiçbir şey gitmez** — aday devri fark etmez |
| İlan özeti → hayat hikâyesi | *"15 Eylül — Mehmet Kaya'dan Zeynep Ak'a devredildi (Ali Kaya)"* |
| Hangi durumda | Her durumda — yayında, yayında olmayan, sonuçlanan, taslak |

**Birden fazla ilanı birden devretmek** → Ekip ekranından (Bölüm 15.6).

### 5.4 [⋮] → Kopyala

**Neden:**

| Durum | Örnek |
|---|---|
| Sonuçlanmış ilanı tekrar açmak | *"Geçen ay Backend Developer aldık, bir tane daha lazım"* — sonuçlanan ilan tekrar yayınlanamaz, kopyalanır |
| Benzer pozisyon | Senior ilanını kopyala → Junior yap |
| Başka şehir | Aynı ilanı Ankara için aç |
| Uygun aday bulunamadı | Şartları değiştirip yeniden dene |

**Kim görür:** Herkes — sahip, yönetici, uzman *(uzman sadece kendi ilanlarını kopyalayabilir)*.

**Basınca:** Onay sorulmaz — kopya doğrudan **📝 Taslak** olarak oluşur ve ilan formu açılır.

| Kopyalanır | Kopyalanmaz |
|---|---|
| Meslek alanı, unvan | Başvurular, davetler |
| Yetenek / bölüm / sertifika kutuları + algılanan etiketler | Süreçteki adaylar, sohbetler |
| İş tanımı | "Bir daha gösterme" kayıtları |
| Konum, çalışma şekli ve tipi, iş dili | Görüntülenme ve tüm sayılar |
| Eğitim, tecrübe, diğer şartlar, maaş | Yayın geçmişi |

| | |
|---|---|
| Yeni ilanın sorumlusu | Kopyalayan kişi |
| Yeni ilan mı | **Evet** — yeni ilan kimliği. Eski ilanda reddedilen veya gizlenen aday yenisinde görünebilir *(yeni pozisyon)* |
| Taslak süresi | Kopyalandığı günden itibaren 30 gün |

### 5.5 [Sonuç bildir]

İlanın tüm işe alım sürecini bitirir. Ayrıntılar → **Bölüm 12.**

---
## 6. İlanın İçi — Adaylar

### 6.1 İlanın içi — dört sekme

İlan kartına dokununca ilanın içi açılır. Üstte ilan başlığı, altında dört sekme:

```
┌──────────────────────────────────────────────────────────────────┐
│ ← İlanlarım                                                      │
│ Senior Backend Developer                           ● Yayında     │
│ Mehmet Kaya · Kadıköy, İstanbul · 12 gün kaldı                   │
│                                                                  │
│ [Adaylar 124] [Başvuranlar 8] [Davet Edilenler 10] [Süreçteki 7] │
│  ▔▔▔▔▔▔▔▔▔▔▔                                                     │
└──────────────────────────────────────────────────────────────────┘
```

| Sekme | Ne gösterir | Bölüm |
|---|---|:---:|
| **Adaylar** | Bu ilana uygun, henüz hiçbir işlem yapılmamış adaylar | 6 |
| **Başvuranlar** | Başvuran adaylar — Bekleyen / Sonuçlanan | 7 |
| **Davet Edilenler** | Davet gönderilen adaylar — Bekleyen / Sonuçlanan | 8 |
| **Süreçteki Adaylar** | Sohbet aşamasındaki adaylar — Devam eden / Sonuçlanan | 9 |

Sekmenin yanındaki sayı: **Adaylar** → uygun aday · diğerleri → **bekleyen / devam eden.**

### 6.2 Adaylar — liste

**Adaylar = işverenin Keşfet'i.** Sistemin bu ilana uygun bulduğu adaylar, **uyum puanına göre** sıralı.

```
┌──────────────────────────────────────────────────────────────────┐
│ Senior Backend Developer                           ● Yayında     │
│ [Adaylar 124] [Başvuranlar 8] [Davet Edilenler 10] [Süreçteki 7] │
│  ▔▔▔▔▔▔▔▔▔▔▔                                                     │
│ 124 uygun aday                                                   │
│ Sırala: [Uyum ▾]                          [⇄ Karşılaştır (0)]    │
├──────────────────────────────────────────────────────────────────┤
│ ┌──────────────────────────────────────────────────────────────┐ │
│ │ [Foto]  Ayşe Kaya                       ● Yüksek uyum        │ │
│ │         Bilişim ve Yazılım                                   │ │
│ │         Backend Geliştirici                                  │ │
│ │         ● Aktif iş arıyorum                                  │ │
│ │                                                              │ │
│ │ Kadıköy, İstanbul                                            │ │
│ │ 4 yıl tecrübe                                                │ │
│ │ Lisans · Bilgisayar Mühendisliği · Mezun                     │ │
│ │ 1 ay içinde başlayabilir                                     │ │
│ │                                                              │ │
│ │ Neden önerildi?                                           ▾  │ │
│ │                                                              │ │
│ │ [Bir daha gösterme]   [+ Karşılaştır]          [İncele →]    │ │
│ └──────────────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────────────┐ │
│ │ [Foto]  Burak Can                       ○ Orta uyum          │ │
│ │         Bilişim ve Yazılım                                   │ │
│ │         Yazılım Mühendisi                                    │ │
│ │         ○ Fırsatlara açığım                                  │ │
│ │                                                              │ │
│ │ Beşiktaş, İstanbul                                           │ │
│ │ 2 yıl tecrübe                                                │ │
│ │ Lisans · Yazılım Mühendisliği · Mezun                        │ │
│ │ Hemen başlayabilir                                           │ │
│ │                                                              │ │
│ │ Neden önerildi?                                           ▾  │ │
│ │                                                              │ │
│ │ [Bir daha gösterme]   [+ Karşılaştır]          [İncele →]    │ │
│ └──────────────────────────────────────────────────────────────┘ │
│                                                                  │
│                     [Daha fazla getir]                           │
└──────────────────────────────────────────────────────────────────┘
```

**Kartta:**

| Parça | Anlamı |
|---|---|
| Fotoğraf · ad soyad · meslek alanı · unvan | |
| Uyum seviyesi | 🟢 Yüksek (≥ 0.75) · 🟡 Orta (0.55–0.75) · ⚪ Düşük (< 0.55) |
| Arama durumu | 🟢 Aktif iş arıyorum · 🟡 Fırsatlara açığım |
| 📍 Yaşadığı konum · 💼 Toplam tecrübe · 🎓 En yüksek eğitim · ⏱ Başlama zamanı | |
| **Neden önerildi? ▾** | Kapalı başlar · açılınca: profil benzerliği, uyan ✅ / eksik ⚠️ nitelikler, tecrübe durumu |

**Kartta bilerek olmayanlar:** Cinsiyet ve askerlik *(sadece detayda — kartta hızlı eleme sebebi olmasın)* · doğum tarihi · telefon · e-posta · belgeler.

**Butonlar:**

| Buton | Neden | Basınca |
|---|---|---|
| **[İncele →]** veya kartın boş yeri | Adayı tanımak | Aday detayı *(6.3)* |
| **[Bir daha gösterme]** | Uymayan adayı listeden çıkarmak | Onay penceresi *(6.5)* |
| **[+ Karşılaştır]** | Birkaç adayı yan yana görmek | Karşılaştırma listesine eklenir *(Bölüm 10.1)* |
| **[Daha fazla getir]** | Liste bitti, yeni adaylar görmek | Yeniden hesaplanır — işlem yapılan adaylar zaten çıktığı için liste görülmemiş adaylarla 200'e tamamlanır |

**Sıralama:** Uyum *(varsayılan)* · Tecrübe · En yeni profil

### 6.3 Aday detayı

```
┌────────────────────────────────────────────────────────────────────┐
│ ← Adaylar                                                          │
├────────────────────────────────────────────────────────────────────┤
│ [Foto]  Ayşe Kaya                            ● Yüksek uyum         │
│         Backend Geliştirici · Bilişim ve Yazılım                   │
│         ● Aktif iş arıyorum · Kadıköy, İstanbul                    │
│                                                                    │
│ [Bir daha gösterme]     [+ Karşılaştır]          [Davet et]        │
├────────────────────────────────────────────────────────────────────┤  ← üst kısım sabit
│ ── Neden önerildi? ───────────────────────────────────────── ▾     │
│                                                                    │
│ ── Hakkında ────────────────────────────────────────────────────   │
│ 5 yıldır e-ticaret alanında backend geliştiriyorum. Ödeme          │
│ sistemleri üzerine çalıştım, mimari kararlara dahil olmak          │
│ istiyorum.                                                         │
│                                                                    │
│ ── Deneyim ─────────────────────────────────────────────────────   │
│ Backend Geliştirici · XYZ Ödeme · Tam zamanlı                      │
│ Mart 2022 – Halen                                                  │
│ Ödeme altyapısında mikroservis geliştirdim, günde 2 milyon         │
│ işlemi taşıyan sistemin performansını iyileştirdim.                │
│                                                                    │
│ ── Eğitim ──────────────────────────────────────────────────────   │
│ Lisans · Bilgisayar Mühendisliği · ODTÜ · Mezun (2021)             │
│                                                                    │
│ ── Yetenekler ──────────────────────────────────────────────────   │
│ React (5 yıl) · .NET (4 yıl) · PostgreSQL (4 yıl) · Docker (2 yıl) │
│                                                                    │
│ ── Sertifikalar ────────────────────────────────────────────────   │
│ SRC Belgesi (2023, süresiz)                                        │
│                                                                    │
│ ── Diller ──────────────────────────────────────────────────────   │
│ İş dilleri: Türkçe, İngilizce · Bildiği: İngilizce (İleri)         │
│                                                                    │
│ ── Projeler ────────────────────────────────────────────────────   │
│ Kargo takip uygulaması — Küçük kargo firmaları için Flutter ile    │
│ takip uygulaması.                                                  │
│                                                                    │
│ ── Diğer bilgiler ──────────────────────────────────────────────   │
│ Sürücü belgesi: B · Seyahat engeli yok · Vardiyalı çalışabilir     │
│ Cinsiyet: Kadın · Askerlik: Yükümlü değil                          │
│ Çalışmak istediği konumlar: İstanbul, Almanya                      │
│ Çalışma şekli: Ofis, Hibrit · Tip: Tam zamanlı                     │
│ Başlayabilir: 1 ay içinde                                          │
│                                                                    │
│ × Telefon, e-posta ve belgeler süreç başlayınca görünür            │
└────────────────────────────────────────────────────────────────────┘
```

| | |
|---|---|
| Üst kısım | Ad, uyum, butonlar — **sabit**, aşağı kaydırınca da görünür |
| Görünmeyen | Doğum tarihi · telefon · e-posta · belgeler |
| Cinsiyet ve askerlik | Görünür ama **hiçbir listede süzülemez** — ayrımcılık yasak |

### 6.4 [Davet et]

**Neden:** İşveren bu adayla görüşmek istiyor.

**Basınca:**

```
┌──────────────────────────────────────────────┐
│ Ayşe Kaya'yı Senior Backend Developer        │
│ pozisyonuna davet et                         │
│                                              │
│ Adaya not (opsiyonel)                        │
│ ┌──────────────────────────────────────────┐ │
│ │ Profilinizi çok beğendik, ödeme          │ │
│ │ altyapısı deneyiminiz tam aradığımız.    │ │
│ │ Görüşmek isteriz.                        │ │
│ └──────────────────────────────────────────┘ │
│                                      95/300  │
│                                              │
│ Aday 10 gün içinde cevap verebilir.          │
│                                              │
│ [Vazgeç]                    [Davet et]       │
└──────────────────────────────────────────────┘
```

| Alan | Kural |
|---|---|
| Not | Opsiyonel · en fazla **300 karakter** |
| İletişim bilgisi | Telefon, e-posta, bağlantı **yazılamaz** — yazılırsa: *"İletişim bilgileri süreç başladıktan sonra paylaşılabilir."* |

**Neden iletişim bilgisi engelleniyor:** Aday daveti kabul etmeden doğrudan aranırsa platform atlanır ve "aday onaylayınca iletişim paylaşılır" kuralı bozulur.

**[Davet et]'e basınca:**

| | |
|---|---|
| Aday | **Adaylar**'dan çıkar → **Davet Edilenler → Bekleyen**'e geçer |
| Adaya | 📱 🔔 ✉️ *"ABC Teknoloji seni Senior Backend Developer pozisyonuna davet etti."* *(aday kataloğu 2.1)* · not, davet detayında görünür |
| Süre | **10 gün** — aday cevap vermezse davet sona erer |
| Kayıt | *"Davet eden: Mehmet Kaya"* |
| Not | Aday kabul ederse sohbetin **ilk mesajı** olur |

### 6.5 [Bir daha gösterme]

**Neden:** Aday bu ilana uygun değil — liste kalabalık olmasın.

**Basınca:**

```
┌──────────────────────────────────────────────┐
│ Bu adayı bu ilan için bir daha görmek        │
│ istemediğinize emin misiniz?                 │
│                                              │
│ Bu işlem geri alınamaz.                      │
│                                              │
│ [Vazgeç]                [Evet, gösterme]     │
└──────────────────────────────────────────────┘
```

| | |
|---|---|
| Sonuç | Kart hafif bir animasyonla kaybolur |
| Kalıcı mı | **Evet** — ayrı bir "gizlenenler" listesi yok, geri alınamaz |
| Sadece bu ilan | Aday aynı şirketin **başka ilanlarında** görünmeye devam eder |
| Adaya | **Hiçbir şey gitmez** — aday fark etmez, ilanı görmeye ve başvurmaya devam edebilir |

> **"Gizlenecek şirketler" ile karıştırılmasın:** O, **adayın** kendi profilinde bir şirketten saklanmasıdır. "Bir daha gösterme" ise işverenin tek bir ilan için tek bir adayı listeden çıkarmasıdır.

### 6.6 Liste durumları

| Durum | Ekranda |
|---|---|
| Eşleştirme hesaplanıyor | ⟳ *"Uygun adaylar hazırlanıyor..."* — bitince liste kendiliğinden dolar |
| Liste var | Kartlar · en altta **[Daha fazla getir]** |
| Liste bitti | *"Bu ilana uygun tüm adayları gördün."* · 🔔 *Yeni aday gelince haber ver* **[Açık]** · **[İlanını güçlendir]** |
| İlan yayında değil | 🔒 *"İlan duraklatıldı — uygun adayları görmek ve davet göndermek için ilanı devam ettirin."* **[Devam ettir]** *(Bölüm 11.1)* |

---

## 7. İlanın İçi — Başvuranlar

### 7.1 Bekleyen — liste

İşverenin **karar vermesi gereken** başvurular.

```
┌───────────────────────────────────────────────────────────────────┐
│ Senior Backend Developer                           ● Yayında      │
│ [Adaylar 124] [Başvuranlar 8] [Davet Edilenler 10] [Süreçteki 7]  │
│                ▔▔▔▔▔▔▔▔▔▔▔▔▔                                      │
│ ┌──────────────┬──────────────┐                                   │
│ │▸ Bekleyen 8  │ Sonuçlanan 15│                                   │
│ └──────────────┴──────────────┘                                   │
│ Sırala: [Uyum ▾]                          [⇄ Karşılaştır (2)]     │
├───────────────────────────────────────────────────────────────────┤
│ ┌───────────────────────────────────────────────────────────────┐ │
│ │ [Foto]  Ayşe Kaya                       ● Yüksek uyum         │ │
│ │         Backend Geliştirici                                   │ │
│ │                                                               │ │
│ │ ~ Karar bekliyor · 25 gün kaldı                               │ │
│ │ Başvuru: 12 Eylül                                             │ │
│ │ 90.000 TRY (net) · 3 belge                                    │ │
│ │                                                               │ │
│ │ Neden önerildi?                                           ▾   │ │
│ │                                                               │ │
│ │ [↓ PDF]   [✓ Karşılaştırmada]   [Profili ve başvuruyu gör →]  │ │
│ └───────────────────────────────────────────────────────────────┘ │
│ ┌───────────────────────────────────────────────────────────────┐ │
│ │ [Foto]  Selin Er                        ○ Orta uyum           │ │
│ │         Yazılım Mühendisi                                     │ │
│ │                                                               │ │
│ │ ! Karar bekliyor · 3 gün kaldı                                │ │
│ │ Başvuru: 20 Ağustos                                           │ │
│ │ 80.000 TRY (net) · 1 belge                                    │ │
│ │                                                               │ │
│ │ Neden önerildi?                                           ▾   │ │
│ │                                                               │ │
│ │ [↓ PDF]   [+ Karşılaştır]       [Profili ve başvuruyu gör →]  │ │
│ └───────────────────────────────────────────────────────────────┘ │
└───────────────────────────────────────────────────────────────────┘
```

**Kartta:**

| Parça | Anlamı |
|---|---|
| Uyum seviyesi | **Başvuru anındaki** puandan — aday sonradan profilini değiştirse de işveren başvurduğu andaki uyumu görür |
| ⏳ Karar bekliyor · X gün kaldı | 30 günlük sürenin kalanı · **7 gün ve altında ⚠️** |
| Başvuru tarihi | |
| 💰 Maaş beklentisi | Aday yazdıysa — net minimum |
| 📄 Belge sayısı | Aday bu başvuruda kaç belge paylaştı |

**Butonlar:**

| Buton | Basınca |
|---|---|
| **[Profili ve başvuruyu gör →]** veya kartın boş yeri | Başvuru detayı *(7.2)* |
| **[⬇ PDF]** | PDF indirme penceresi *(Bölüm 10.2)* |
| **[+ Karşılaştır]** | Karşılaştırma listesine ekler *(Bölüm 10.1)* |

**Sıralama:** Uyum *(varsayılan)* · Başvuru tarihi · Kalan gün

### 7.2 Başvuru detayı

```
┌──────────────────────────────────────────────────────────────────┐
│ ← Başvuranlar                                                    │
├──────────────────────────────────────────────────────────────────┤
│ [Foto]  Ayşe Kaya                            ● Yüksek uyum       │
│         Backend Geliştirici · Bilişim ve Yazılım                 │
│         ● Aktif iş arıyorum · Kadıköy, İstanbul                  │
│         ~ Karar bekliyor · 25 gün kaldı                          │
│                                                                  │
│ [↓ PDF]  [✓ Karşılaştırmada]     [Kabul etme]    [Kabul et]      │
├──────────────────────────────────────────────────────────────────┤  ← üst kısım sabit
│ ── Başvuru bilgileri ─────────────────────────────────────────── │
│ Başvuru tarihi:   12 Eylül 2026                                  │
│ Maaş beklentisi:  90.000 TRY (net)                               │
│ Not:              "Yan haklara göre esnek olabilirim."           │
│                                                                  │
│ Paylaşılan belgeler:                                             │
│   Özgeçmiş                          [Görüntüle]  [İndir]         │
│   Portfolyo                         [Görüntüle]  [İndir]         │
│   SRC Belgesi                       [Görüntüle]  [İndir]         │
│                                                                  │
│ ── Neden önerildi? ───────────────────────────────────────── ▾   │
│                                                                  │
│ ── Profil ────────────────────────────────────────────────────── │
│ Hakkında · Deneyim · Eğitim · Yetenekler · Sertifikalar ·        │
│ Diller · Projeler · Diğer bilgiler                               │
│ (Aday detayındakiyle aynı bölümler — Bölüm 6.3)                  │
│                                                                  │
│ × Telefon ve e-posta süreç başlayınca, aday paylaşırsa görünür   │
└──────────────────────────────────────────────────────────────────┘
```

### 7.3 [Kabul et]

**Neden:** İşveren bu adayla görüşmek istiyor.

```
┌──────────────────────────────────────────────┐
│ Ayşe Kaya'nın başvurusunu kabul etmek        │
│ istiyor musunuz?                             │
│                                              │
│ Aday "Süreçteki Adaylar"a geçecek ve         │
│ sohbet başlayacak.                           │
│                                              │
│ [Vazgeç]                     [Kabul et]      │
└──────────────────────────────────────────────┘
```

| Sonra | |
|---|---|
| Aday | **Başvuranlar**'dan çıkar → **Süreçteki Adaylar → Devam eden**'e geçer |
| Sohbet | Açılır · işveren doğrudan **sohbet ekranına** yönlendirilir |
| Süreç süresi | **60 gün** başlar *(Bölüm 9)* |
| Adaya | 📱 🔔 ✉️ *"ABC Teknoloji başvurunu kabul etti. Görüşmeye başlayabilirsin."* *(aday kataloğu 1.1)* |
| Belgeler | Erişim devam eder |

### 7.4 [Kabul etme]

**Neden:** Aday bu pozisyon için uygun değil.

```
┌──────────────────────────────────────────────┐
│ Ayşe Kaya'nın başvurusunu kabul etmemek      │
│ istediğinize emin misiniz?                   │
│                                              │
│ Adaya gidecek mesaj:                         │
│ "ABC Teknoloji, Senior Backend Developer     │
│  pozisyonu için başka adaylarla ilerlemeye   │
│  karar verdi."                               │
│                                              │
│ Bu aday bu ilana tekrar başvuramaz.          │
│                                              │
│ [Vazgeç]                  [Kabul etme]       │
└──────────────────────────────────────────────┘
```

| Sonra | |
|---|---|
| Aday | **Başvuranlar → Sonuçlanan** · ✕ *Reddedildi* |
| Adaya | 📱 🔔 kibar mesaj — yukarıdaki metin |
| Belgeler | İşverenin erişimi **kapanır** |
| Tekrar | Aday bu ilana **tekrar başvuramaz** |
| İşveren | **Başvuranlar** listesine döner — sıradaki başvuruya geçer |

> **Neden reddetmek iyi bir şey:** Şirket raporunda *"cevapsız kalan başvuru"* görünür. Reddetmek **cevap vermektir** — adayı 30 gün bekletmekten çok daha iyidir.

### 7.5 Cevap verilmezse — 30 gün

| Gün | Ne olur |
|---|---|
| 0 | Aday başvurdu |
| 23 | Sorumluya 🔔 *"3 başvuru 7 gün içinde cevapsız kalacak."* |
| 30 | Başvuru otomatik sona erer → **Sonuçlanan** · ⌛ *Yanıtlanmadan sona erdi* · adaya 🔔 · belge erişimi kapanır · şirket raporunda **cevapsız** sayılır |

### 7.6 Sonuçlanan — liste

```
┌──────────────────────────────────────────────────────────────────┐
│ [Adaylar 124] [Başvuranlar 8] [Davet Edilenler 10] [Süreçteki 7] │
│                ▔▔▔▔▔▔▔▔▔▔▔▔▔                                     │
│ ┌──────────────┬──────────────┐                                  │
│ │  Bekleyen 8  │▸Sonuçlanan 15│                                  │
│ └──────────────┴──────────────┘                                  │
├──────────────────────────────────────────────────────────────────┤
│ [Foto]  Burak Can · Yazılım Mühendisi                    [↓ PDF] │
│ ✕ Reddedildi · 18 Eylül                                          │
│ ──────────────────────────────────────────────────────────────── │
│ [Foto]  Selin Er · Yazılım Mühendisi                     [↓ PDF] │
│ ~ Yanıtlanmadan sona erdi · 10 Eylül                             │
│ ──────────────────────────────────────────────────────────────── │
│ [Foto]  Emre Yıldız · Backend Developer                          │
│ ↩ Aday başvurusunu geri çekti · 8 Eylül                          │
│                                                                  │
│ Son 90 gündeki sonuçlanan başvurular gösteriliyor.               │
└──────────────────────────────────────────────────────────────────┘
```

| Sonuç | Nasıl oldu |
|---|---|
| ✕ Reddedildi | İşveren *[Kabul etme]* dedi |
| ⌛ Yanıtlanmadan sona erdi | İşveren 30 gün cevap vermedi |
| ↩ Aday başvurusunu geri çekti | Aday vazgeçti |
| ⏹ Süreç sonlandırıldı | İşveren ilanın sonucunu bildirdi, başvuru hâlâ bekliyordu — **sadece sonuçlanan ilanın özetinde görünür** |
| 🚫 Aday hesabını kapattı | Aday başvuru beklerken hesabını sildi — kart **"Silinmiş kullanıcı"** olarak görünür *(Bölüm 22.3)* |

| Kural | |
|---|---|
| **Kabul edilenler** | Burada **değil** — Süreçteki Adaylar'da |
| Belgeler | Erişim **kapalı** |
| [⬇ PDF] | **Sadece profil** — belgeler yok · aday geri çektiyse PDF yok |
| Süre | Son **90 gün** — sonra listeden kalkar *(kayıt raporda kalır)* |

---

## 8. İlanın İçi — Davet Edilenler

### 8.1 Bekleyen — liste

Davet gönderilen, **adayın cevabı beklenen** kişiler. Burada karar **adayda** — işveren takip eder, isterse geri çeker.

```
┌──────────────────────────────────────────────────────────────────┐
│ Senior Backend Developer                           ● Yayında     │
│ [Adaylar 124] [Başvuranlar 8] [Davet Edilenler 10] [Süreçteki 7] │
│                               ▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔                │
│ ┌──────────────┬──────────────┐                                  │
│ │▸ Bekleyen 10 │ Sonuçlanan 5 │                                  │
│ └──────────────┴──────────────┘                                  │
├──────────────────────────────────────────────────────────────────┤
│ ┌──────────────────────────────────────────────────────────────┐ │
│ │ [Foto]  Burak Can                       ○ Orta uyum          │ │
│ │         Yazılım Mühendisi                                    │ │
│ │                                                              │ │
│ │ ~ Cevap bekleniyor · 7 gün kaldı                             │ │
│ │ Davet: 15 Eylül · Davet eden: Mehmet Kaya                    │ │
│ │ "Profilinizi çok beğendik, görüşmek isteriz."                │ │
│ │                                                              │ │
│ │ Neden önerildi?                                           ▾  │ │
│ │                                                              │ │
│ │ [Davetimi geri çek]                         [Profili gör →]  │ │
│ └──────────────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────────────┐ │
│ │ [Foto]  Emre Yıldız                     ● Yüksek uyum        │ │
│ │         Backend Developer                                    │ │
│ │                                                              │ │
│ │ ! Cevap bekleniyor · 1 gün kaldı                             │ │
│ │ Davet: 11 Eylül · Davet eden: Zeynep Ak                      │ │
│ │                                                              │ │
│ │ Neden önerildi?                                           ▾  │ │
│ │                                                              │ │
│ │ [Davetimi geri çek]                         [Profili gör →]  │ │
│ └──────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
```

| Parça | Anlamı |
|---|---|
| Kalan gün | 10 günden kalan · **2 gün ve altında ⚠️** |
| Davet eden | Ekipte kim gönderdi |
| 💬 | Davetle gönderilen not — varsa |

**Karşılaştırma ve PDF burada yok** — aday henüz kabul etmedi, şirketle bilgi paylaşma onayı vermedi.

### 8.2 Profil detayı

```
┌──────────────────────────────────────────────────────────────────┐
│ ← Davet Edilenler                                                │
├──────────────────────────────────────────────────────────────────┤
│ [Foto]  Burak Can                            ○ Orta uyum         │
│         Yazılım Mühendisi · Bilişim ve Yazılım                   │
│         Beşiktaş, İstanbul                                       │
│         ~ Cevap bekleniyor · 7 gün kaldı                         │
│         Davet: 15 Eylül · Davet eden: Mehmet Kaya                │
│                                                                  │
│                                          [Davetimi geri çek]     │
├──────────────────────────────────────────────────────────────────┤
│ ── Gönderdiğiniz not ─────────────────────────────────────────── │
│ "Profilinizi çok beğendik, görüşmek isteriz."                    │
│                                                                  │
│ ── Neden önerildi? ───────────────────────────────────────── ▾   │
│ ── Profil ────────────────────────────────────────────────────── │
│ (Aday detayındakiyle aynı bölümler — Bölüm 6.3)                  │
│                                                                  │
│ × Belgeler, aday daveti kabul edince görünür                     │
└──────────────────────────────────────────────────────────────────┘
```

**Belgeler neden kapalı:** Aday hangi belgeleri paylaşacağını **daveti kabul ederken** seçiyor.

### 8.3 [Davetimi geri çek]

**Neden:** Yanlış adaya gönderildi, pozisyon için başka adaylarla ilerleniyor.

```
┌──────────────────────────────────────────────┐
│ Davetinizi geri çekmek istediğinize          │
│ emin misiniz?                                │
│                                              │
│ Adaya gidecek mesaj:                         │
│ "ABC Teknoloji davetini geri çekti."         │
│                                              │
│ Bu adaya bu ilan için tekrar davet           │
│ gönderemezsiniz.                             │
│                                              │
│ [Vazgeç]                     [Geri çek]      │
└──────────────────────────────────────────────┘
```

| Sonra | |
|---|---|
| Aday | **Davet Edilenler → Sonuçlanan** · ↩ *Davet geri çekildi* |
| Adaya | 🔔 *"ABC Teknoloji davetini geri çekti."* |
| Tekrar | Bu ilandan bu adaya **tekrar davet gönderilemez** |

**Neden tekrar yok:** Aynı adayı tekrar tekrar davet edip rahatsız etmek olmasın.

### 8.4 Aday cevap verince

| Aday | İşverende | Sorumluya | Sahip / yöneticiye |
|---|---|---|---|
| **Kabul etti** | → **Süreçteki Adaylar → Devam eden** · sohbet açılır · 60 gün başlar | 📱 🔔 *"Burak Can davetinizi kabul etti. Görüşmeye başlayabilirsiniz."* | 🔔 |
| **Kabul etmedi** | → Sonuçlanan · ✕ *Kabul etmedi* | 📱 🔔 *"Burak Can davetinizi kabul etmedi."* | 🔔 |
| **10 gün cevap yok** | → Sonuçlanan · ⌛ *Yanıtlanmadan sona erdi* | 🔔 *"Burak Can'a gönderdiğiniz davet yanıtlanmadan sona erdi."* | — |

Aday kabul ederken maaş beklentisi, not ve paylaşacağı belgeleri seçer — işveren bunları **Süreçteki Adaylar**'da görür.

### 8.5 Sonuçlanan — liste

```
┌──────────────────────────────────────────────────────────────────┐
│ ┌──────────────┬──────────────┐                                  │
│ │  Bekleyen 10 │▸Sonuçlanan 5 │                                  │
│ └──────────────┴──────────────┘                                  │
├──────────────────────────────────────────────────────────────────┤
│ [Foto]  Ali Kurt · Backend Geliştirici                           │
│ ✕ Kabul etmedi · 20 Eylül                                        │
│ ──────────────────────────────────────────────────────────────── │
│ [Foto]  Deniz Ay · Yazılım Mühendisi                             │
│ ~ Yanıtlanmadan sona erdi · 18 Eylül                             │
│ ──────────────────────────────────────────────────────────────── │
│ [Foto]  Okan Tan · Backend Developer                             │
│ ↩ Davet geri çekildi · 16 Eylül                                  │
│                                                                  │
│ Son 90 gündeki sonuçlanan davetler gösteriliyor.                 │
└──────────────────────────────────────────────────────────────────┘
```

| Sonuç | Nasıl oldu |
|---|---|
| ✕ Kabul etmedi | Aday reddetti |
| ⌛ Yanıtlanmadan sona erdi | Aday 10 gün cevap vermedi |
| ↩ Davet geri çekildi | İşveren geri çekti |
| ⏹ Süreç sonlandırıldı | İşveren ilanın sonucunu bildirdi, davet hâlâ bekliyordu — **sadece sonuçlanan ilanın özetinde görünür** |
| 🚫 Aday hesabını kapattı | Aday davet beklerken hesabını sildi — kart **"Silinmiş kullanıcı"** olarak görünür *(Bölüm 22.3)* |

**Kabul edenler** burada değil — **Süreçteki Adaylar**'da. **PDF yok** — aday hiç onay vermedi.

---
## 9. İlanın İçi — Süreçteki Adaylar

İki tarafın da **"evet"** dediği adaylar — başvurusu kabul edilenler ve daveti kabul edenler. Burada **sohbet** var.

### 9.1 Devam eden — liste

```
┌──────────────────────────────────────────────────────────────────┐
│ Senior Backend Developer                           ● Yayında     │
│ [Adaylar 124] [Başvuranlar 8] [Davet Edilenler 10] [Süreçteki 7] │
│                                                    ▔▔▔▔▔▔▔▔▔▔▔▔▔ │
│ ┌────────────────┬──────────────┐                                │
│ │▸ Devam eden 7  │ Sonuçlanan 3 │                                │
│ └────────────────┴──────────────┘                                │
│ Sırala: [Son mesaj ▾]                     [⇄ Karşılaştır (0)]    │
├──────────────────────────────────────────────────────────────────┤
│ ┌──────────────────────────────────────────────────────────────┐ │
│ │ [Foto]  Ayşe Kaya                   ● Yüksek uyum      2     │ │
│ │         Backend Geliştirici                                  │ │
│ │ "Salı 14:00 uygun mudur?"                            10:32   │ │
│ │ Başvurdu · İletişim paylaşıldı                               │ │
│ │ ~ Sonuçlandırmak için 45 gün kaldı                           │ │
│ │                                                              │ │
│ │ [↓ PDF]  [+ Karşılaştır]  [Profili gör]    [Sohbete git →]   │ │
│ └──────────────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────────────┐ │
│ │ [Foto]  Burak Can                   ○ Orta uyum              │ │
│ │         Yazılım Mühendisi                                    │ │
│ │ Siz: "Merhaba Burak Bey, teknik görüşme için…"         Dün   │ │
│ │ Davet edildi                                                 │ │
│ │ ! Sonuçlandırmak için 5 gün kaldı                            │ │
│ │                                                              │ │
│ │ [↓ PDF]  [+ Karşılaştır]  [Profili gör]    [Sohbete git →]   │ │
│ └──────────────────────────────────────────────────────────────┘ │
│ ┌──────────────────────────────────────────────────────────────┐ │
│ │ [Foto]  Emre Yıldız                 ● Yüksek uyum            │ │
│ │         Backend Developer                                    │ │
│ │ Henüz mesaj yok                                              │ │
│ │ Başvurdu                                                     │ │
│ │ ~ Sonuçlandırmak için 59 gün kaldı                           │ │
│ │                                                              │ │
│ │ [↓ PDF]  [+ Karşılaştır]  [Profili gör]    [Sohbete git →]   │ │
│ └──────────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
```

**Kartta:**

| Parça | Anlamı |
|---|---|
| 💬 2 | Okunmamış mesaj sayısı |
| Son mesaj + saat | *"Siz:"* ile başlıyorsa son yazan işveren |
| 📨 Başvurdu / ✉️ Davet edildi | Aday sürece nereden geldi |
| 📞 İletişim paylaşıldı | Aday telefon ve e-postasını paylaştıysa |
| ⏳ Sonuçlandırmak için X gün kaldı | 60 günlük sürenin kalanı · **7 gün ve altında ⚠️** |

**Butonlar:**

| Buton | Basınca |
|---|---|
| **[Sohbete git →]** veya kartın boş yeri | Sohbet *(içeriği → sohbet dokümanı)* |
| **[Profili gör]** | Süreçteki aday detayı *(9.2)* |
| **[⬇ PDF]** | PDF indirme *(Bölüm 10.2)* |
| **[+ Karşılaştır]** | Karşılaştırma listesine ekler *(Bölüm 10.1)* |

**Sıralama:** Son mesaj *(varsayılan)* · Kalan gün · Uyum

### 9.2 Süreçteki aday detayı

```
┌─────────────────────────────────────────────────────────────────────┐
│ ← Süreçteki Adaylar                                                 │
├─────────────────────────────────────────────────────────────────────┤
│ [Foto]  Ayşe Kaya                            ● Yüksek uyum          │
│         Backend Geliştirici · Bilişim ve Yazılım                    │
│         Süreç başladı: 15 Eylül · ~ 45 gün kaldı                    │
│         Başvurdu                                                    │
│                                                                     │
│ [↓ PDF]  [+ Karşılaştır]   [Süreci sonuçlandır]  [Sohbete git →]    │
├─────────────────────────────────────────────────────────────────────┤  ← üst kısım sabit
│ ── İletişim ────────────────────────────────────────────────────    │
│ +90 532 123 45 67      ayse@mail.com                                │
│ (Aday paylaşmadıysa: "Sohbetten iletişim bilgisi isteyebilirsiniz") │
│                                                                     │
│ ── Başvuru bilgileri ───────────────────────────────────────────    │
│ Maaş beklentisi:  90.000 TRY (net)                                  │
│ Not:              "Yan haklara göre esnek olabilirim."              │
│                                                                     │
│ ── Paylaşılan belgeler ─────────────────────────────────────────    │
│   Özgeçmiş                          [Görüntüle]  [İndir]            │
│   Portfolyo                         [Görüntüle]  [İndir]            │
│   SRC Belgesi                       [Görüntüle]  [İndir]            │
│                                                                     │
│ ── Neden önerildi? ───────────────────────────────────────── ▾      │
│ ── Profil ──────────────────────────────────────────────────────    │
│ (Aday detayındakiyle aynı bölümler — Bölüm 6.3)                     │
└─────────────────────────────────────────────────────────────────────┘
```

Sohbetin içinde de aynı seçenek: üstteki **[⋮] → Süreci sonuçlandır.**

### 9.3 [Süreci sonuçlandır]

**Neden:** Görüşmeler bitti — işe alındı ya da devam edilmiyor. Böylece **işe alım gerçekten kayıt altına girer.**

```
┌──────────────────────────────────────────────┐
│ Ayşe Kaya ile süreç nasıl sonuçlandı?        │
│                                              │
│ ○ İşe alındı *                               │
│ ○ Devam etmiyoruz                            │
│                                              │
│ Adaya gidecek mesaj:                         │
│ (seçime göre burada görünür)                 │
│                                              │
│ [Vazgeç]                  [Sonuçlandır]      │
└──────────────────────────────────────────────┘
```

| Seçim | Adaya gidecek mesaj | Sonra |
|---|---|---|
| **İşe alındı 🎉** | 📱 🔔 ✉️ *"ABC Teknoloji seni işe aldığını bildirdi. Doğru mu?"* **[Evet, işe alındım]** **[Hayır]** *(aday kataloğu 4.1)* | **İşe alım onayı** başlar *(9.6)* · sohbet kapanır |
| **Devam etmiyoruz** | 📱 🔔 *"ABC Teknoloji, Senior Backend Developer pozisyonu için başka adaylarla ilerlemeye karar verdi."* | Sohbet kapanır |

| Kural | |
|---|---|
| **Birden fazla kişi** | Bir ilanda istediği kadar adaya *"İşe alındı"* verilebilir *(ör. "3 kasiyer arıyoruz")* |
| İlan etkilenir mi | **Hayır** — ilan yayında kalır, ilanı kapatma önerisi **gösterilmez** |
| Sohbet | **Kapanır** — geçmiş iki tarafta da görünür, yeni mesaj yazılamaz: *"Bu süreç sonuçlandı."* |
| Belgeler | İşe alınanda erişim **devam eder** · devam edilmeyende **kapanır** |

### 9.4 60 günlük süre

Süreç **başladığı günden itibaren 60 gün** içinde sonuçlandırılmalı.

| Gün | Ne olur |
|---|---|
| 0 | Süreç başladı — kartta *"60 gün kaldı"* |
| 53 | Sorumluya 📱 🔔 *"Ayşe Kaya ile süreç 7 gün sonra sona erecek. Sonuçlandırmak ister misiniz?"* **[İşe alındı]** **[Devam etmiyoruz]** |
| 60 | Sonuçlandırılmadıysa **otomatik biter** |

**60. günde:**

| | İşveren | Aday |
|---|---|---|
| Kartta | ⌛ *Süre doldu — sonuçlandırılmadı* | ⌛ *Süreç sona erdi* |
| Bildirim | 📱 🔔 *"Ayşe Kaya ile süreç 60 gün dolduğu için sona erdi."* — sorumluya · 🔔 sahip / yöneticiye | 🔔 *"ABC Teknoloji ile Senior Backend Developer sürecin sona erdi."* — push yok *(aday kataloğu 3.4)* |
| Sohbet | Kapanır — geçmiş görünür | Kapanır — geçmiş görünür |

| Kural | |
|---|---|
| **Uzatma** | **Yok** — 60 gün kesin |
| Süre neyden sayılır | Sürecin **başladığı** gün — mesajlaşma süreyi uzatmaz |
| Kalan gün | **İşverene gösterilir** · adaya gösterilmez *(baskı hissetmesin)* |
| İlan kapalı olsa da | Süre işler |

**Neden iki tarafa farklı ifade:** İşveren kendi ihmalini açıkça görsün, bir dahakine sonuçlandırsın. Aday için nötr — *"reddedildin"* de değil, *"işveren unuttu"* da değil.

### 9.5 Sonuçlanan — liste

```
┌──────────────────────────────────────────────────────────────────┐
│ ┌────────────────┬──────────────┐                                │
│ │  Devam eden 7  │▸Sonuçlanan 4 │                                │
│ └────────────────┴──────────────┘                                │
├──────────────────────────────────────────────────────────────────┤
│ [Foto]  Ayşe Kaya · Backend Geliştirici                  [↓ PDF] │
│ * İşe alındı · 28 Eylül · ✓ Aday onayladı          [Sohbeti gör] │
│ ──────────────────────────────────────────────────────────────── │
│ [Foto]  Burak Can · Yazılım Mühendisi                    [↓ PDF] │
│ ✕ Devam edilmedi · 25 Eylül                        [Sohbeti gör] │
│ ──────────────────────────────────────────────────────────────── │
│ [Foto]  Emre Yıldız · Backend Developer                          │
│ ↩ Aday süreçten çekildi · 22 Eylül                 [Sohbeti gör] │
│ ──────────────────────────────────────────────────────────────── │
│ [Foto]  Selin Er · Yazılım Mühendisi                     [↓ PDF] │
│ ~ Süre doldu — sonuçlandırılmadı · 20 Eylül        [Sohbeti gör] │
│                                                                  │
│ Son 90 gündeki sonuçlanan süreçler gösteriliyor.                 │
└──────────────────────────────────────────────────────────────────┘
```

| Sonuç | Kim | İşveren kartında | Aday kartında |
|---|---|---|---|
| İşe alındı | İşveren | 🎉 İşe alındı + onay durumu | 🎉 İşe alındın |
| Devam etmiyoruz | İşveren | ✕ Devam edilmedi | ✕ Başka adaylarla ilerlendi |
| Süreçten çekildi | Aday | ↩ Aday süreçten çekildi | ↩ Süreçten çekildin |
| 60 gün doldu | Sistem | ⌛ Süre doldu — sonuçlandırılmadı | ⌛ Süreç sona erdi |
| İlanın sonucu bildirildi | İşveren | ⏹ Süreç sonlandırıldı | ⏹ *(sonuca göre mesaj — Bölüm 12)* |
| Aday hesabını sildi | Sistem | 🚫 **Silinmiş kullanıcı** · Aday hesabını kapattı | — *(aday yok)* |

| | |
|---|---|
| **[Sohbeti gör]** | Sohbet **salt okunur** açılır |
| **[⬇ PDF]** | İşe alınanda: profil + belgeler seçilebilir · diğerlerinde: sadece profil · çekilen ve hesabını silen adayda **yok** |
| Süre | Son **90 gün** |
| Silinmiş kullanıcı kartı | Ad ve fotoğraf yerine *"Silinmiş kullanıcı"* · sadece unvan ve sonuç · profil, belge, PDF, iletişim yok · **[Sohbeti gör]** salt okunur — sohbetin sonunda *"Bu kullanıcı hesabını sildi"* |

### 9.6 İşe alım onayı

**Neden:** İşe alım platform dışında olur — sözleşme bize gelmez. *"Gerçekten işe alındı mı?"* sorusunu **iki tarafa da** sorarak en güvenilir veriyi elde ederiz. Hem şirketin dürüstlüğü hem platformun gerçek başarısı ölçülür.

**Akış:**

```
İşveren "İşe alındı" dedi
(Süreci sonuçlandır veya Sonuç bildir → Pozisyon doldu)
                        │
                        v
┌───────────────────────────────────────────────┐
│  Adaya soru: "ABC Teknoloji seni işe aldığını │
│              bildirdi. Doğru mu?"             │
└──────┬────────────────┬────────────────┬──────┘
       │                │                │
 Evet, işe alındım    Hayır       14 gün cevap yok
       v                v                v
┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│   ✓ Onaylı  │  │ (!) Çelişki │  │  - Cevapsız │
└─────────────┘  └─────────────┘  └─────────────┘
```

**İşveren nerede görür:** Süreçteki → Sonuçlanan kartında ve Sonuçlananlar'daki ilan kartında *(Bölüm 13.2)*.

| Durum | İşaret | Raporda |
|---|---|---|
| Aday onayladı | ✅ | **Onaylı işe alım** |
| Aday onayı bekleniyor | ⏳ | — *(14 gün)* |
| Aday cevap vermedi | ⚪ | İşe alım — onaysız |
| Aday işe alınmadığını bildirdi | ⚠️ | **Tartışmalı** — admin listesine düşer |
| Platform dışından işe alındı | 🏢 | İşe alım — doğrulanamaz |

**Çelişki olursa — işverenin iki seçeneği:**

```
│ * İşe alınanlar                                                │
│    Ayşe Kaya     ✓ Aday onayladı                               │
│    Burak Can     ! Aday işe alınmadığını bildirdi              │
│                  [Yanlış işaretledim]   [Adaya tekrar sor]     │
```

| Buton | Ne olur |
|---|---|
| **[Yanlış işaretledim]** | Aday işe alınanlardan çıkar → sonucu ⏹ *Süreç sonlandırıldı* · adaya bildirim gitmez · çelişki kapanır |
| **[Adaya tekrar sor]** | Adaya 📱 🔔 *"ABC Teknoloji işe alındığını tekrar bildirdi. Cevabını kontrol eder misin?"* *(aday kataloğu 4.2)* — **sadece bir kez** |

**Aday da** kendi kartında 30 gün içinde **[Cevabımı değiştir]** diyebilir.

| Kural | |
|---|---|
| Adayın cevap süresi | **14 gün** |
| Çelişki düzeltme süresi | **30 gün** — sonra kayıt kesinleşir |
| [Adaya tekrar sor] | **Bir kez** — işveren adaya baskı yapamasın |
| Adaya *"Evet"* deyince | *"Arama durumunu 'Şu an iş aramıyorum' yapmak ister misin?"* önerisi |

---

## 10. Karşılaştırma ve PDF

### 10.1 Karşılaştırma

**Neden:** İşverenin en sık sorusu *"bu adaylardan hangisi?"* — ilanın şartlarına göre yan yana görmek.

**Nerede:** Üç sekmede, **her biri kendi içinde bağımsız:**

| Sekme | Var mı |
|---|:---:|
| Adaylar | ✅ |
| Başvuranlar → Bekleyen | ✅ |
| Süreçteki → Devam eden | ✅ |
| Davet Edilenler · tüm Sonuçlanan alt sekmeleri | ❌ |

**Adım 1 — Listeye ekle:** Kartta veya detayda **[+ Karşılaştır]**

```
│ [↓ PDF]   [+ Karşılaştır]       [Profili ve başvuruyu gör →] │
                    ↓ dokununca
│ [↓ PDF]   [✓ Karşılaştırmada]   [Profili ve başvuruyu gör →] │

Ekranın altında 2 saniye: "Karşılaştırmaya eklendi (2/4)"
Sekmenin üstündeki sayaç: [⇄ Karşılaştır (2)]
```

**Ekleyince hiçbir ekran açılmaz** — işveren incelemeye devam eder. **[✓ Karşılaştırmada]**'ya tekrar dokunursa listeden çıkar.

**4 kişi doluyken 5. eklenirse:**

```
┌──────────────────────────────────────────────┐
│ Karşılaştırma listesi dolu (4/4)             │
│                                              │
│ Emre Yıldız'ı eklemek için birini çıkarın:   │
│                                              │
│ [Foto] Ayşe Kaya     ● Yüksek uyum   [Çıkar] │
│ [Foto] Burak Can     ○ Orta uyum     [Çıkar] │
│ [Foto] Selin Er      ○ Orta uyum     [Çıkar] │
│ [Foto] Deniz Ay      ● Yüksek uyum   [Çıkar] │
│                                              │
│ [Vazgeç]                                     │
└──────────────────────────────────────────────┘
```

**[Çıkar]** → o aday çıkar, yeni aday eklenir, pencere kapanır.

**Adım 2 — Karşılaştır:** Sekmenin üstündeki **[⇄ Karşılaştır (n)]**

| Listede | Basınca |
|---|---|
| 0 | *"Karşılaştırmak için adayların kartında [+ Karşılaştır]'a dokunun."* |
| 1 | *"Karşılaştırmak için en az bir aday daha ekleyin."* |
| 2–4 | Karşılaştırma ekranı |

**Karşılaştırma ekranı** *(Başvuranlar'dan açıldığında)*:

```
┌──────────────────────────────────────────────────────────────────────┐
│ ← Başvuranlar        Karşılaştır — Senior Backend Developer          │
├──────────────────────────────────────────────────────────────────────┤
│                     Ayşe Kaya ✕     Burak Can ✕     Selin Er ✕       │
│                     [Foto]          [Foto]          [Foto]           │
│ Uyum                ● Yüksek        ○ Orta          ○ Orta           │
│ Unvan               Backend Gel.    Yazılım Müh.    Yazılım Müh.     │
│ Konum               Kadıköy         Beşiktaş        Çankaya          │
│ Arama durumu        ● Aktif         ○ Açık          ● Aktif          │
│                                                                      │
│ ── Tecrübe ───────────────────────────────────── İstenen: 3–6 yıl ── │
│ Toplam tecrübe      4 yıl ✓         6 yıl ✓         2 yıl !          │
│                                                                      │
│ ── Olmazsa olmaz ─────────────────────────────────────────────────── │
│ React · 5 yıl       ✓ 5 yıl         ! 2 yıl         ! 3 yıl          │
│ .NET                ✓               ✓               ✕                │
│ SRC belgesi         ✓               ✕               ✓                │
│                                                                      │
│ ── Tercih edilen ─────────────────────────────────────────────────── │
│ Docker              ✓               ✓               ✕                │
│ Kubernetes          ✕               ✓               ✕                │
│ Bölüm               ✓ Bilg. Müh.    ✓ Yaz. Müh.     ✕ İşletme        │
│                                                                      │
│ ── Eğitim ────────────────────────────────────────────────────────── │
│ En yüksek           Lisans · Mezun  Y.Lisans · MezunLisans · Mezun   │
│                                                                      │
│ ── Başvuru ───────────────────────────────────────────────────────── │
│ Maaş beklentisi     90.000          110.000         80.000           │
│ Başlayabilir        1 ay            Hemen           2 hafta          │
│ Başvuru tarihi      12 Eylül        14 Eylül        20 Ağustos       │
│ Belge               3               1               1                │
│                                                                      │
│                     [Profil →]      [Profil →]      [Profil →]       │
└──────────────────────────────────────────────────────────────────────┘
```

| Kural | |
|---|---|
| Satırlar | **İlanın kendi şartlarından** kurulur — ilan neyi istediyse o satırlar. Muhasebe ilanında *SAP, Logo, SMMM*; depo ilanında *forklift ehliyeti, vardiya* |
| ✅ ⚠️ ✕ | Var · yıl eksik · yok — eşleştirmedeki nitelik uyumuyla aynı hesap |
| **"Başvuru" bölümü** | Başvuranlar ve Süreçteki'de · Adaylar'da yok *(henüz başvuru yok)* · Süreçteki'de ek: süreçte kaç gün, iletişim paylaşıldı mı |
| **Gösterilmez** | Cinsiyet, askerlik, doğum tarihi, telefon, e-posta — karşılaştırmada ayrımcılık sebebi olmasın |
| ✕ sütun başında | Adayı listeden çıkarır — 1 kişi kalırsa listeye döner |
| Karar | **Burada verilmez** — [Profil →] ile detaya gidilir |
| İndirme | Yok |

| Liste kuralları | |
|---|---|
| En fazla | **4** |
| Her sekmenin kendi listesi | Adaylar · Başvuranlar · Süreçteki karışmaz |
| Her ilanın kendi listesi | |
| Kişiye özel | Mehmet'in listesi Zeynep'inkini etkilemez |
| Ne kadar kalır | Kişi çıkarana kadar |
| Aday sekmeden çıkarsa | **Otomatik düşer** *(ör. başvurusu kabul edilip Süreçteki'ye geçerse)* |

### 10.2 PDF indirme

**Neden:** İşveren adayı bölüm müdürüne iletmek veya kendi dosyasına koymak isteyebilir.

**Nerede:** Kartta ve detayda **[⬇ PDF]**

**Basınca:**

```
┌──────────────────────────────────────────────┐
│ Ayşe Kaya — Profili PDF indir                │
│                                              │
│ ● Sadece profil                              │
│   Profil ve başvuru bilgileri                │
│                                              │
│ ○ Profil + belgeler                          │
│   Paylaşılan 3 belge de eklenir:             │
│   Özgeçmiş · Portfolyo · SRC Belgesi         │
│                                              │
│ [Vazgeç]                        [İndir]      │
└──────────────────────────────────────────────┘
```

| Seçim | İnen dosya |
|---|---|
| **Sadece profil** *(varsayılan)* | 1–2 sayfa — profil + başvuru bilgileri |
| **Profil + belgeler** | **Tek PDF** — profil, ardından paylaşılan belgeler sırayla *(JPG / PNG sayfaya yerleştirilir)* · *"PDF hazırlanıyor..."* |

**PDF'in içi:**

| Bölüm | İçerik |
|---|---|
| Üst | Ad soyad · unvan · meslek alanı · konum · uyum seviyesi |
| Başvuru | Tarih · maaş beklentisi · not |
| Profil | Hakkında · deneyim · eğitim · yetenekler · sertifikalar · diller · projeler · diğer bilgiler |
| İletişim | Sadece aday **paylaştıysa** |
| **Her sayfanın altında** | *"ABC Teknoloji — Mehmet Kaya tarafından 15 Eylül 2026'da indirildi"* |

**Nerede var:**

| Yer | [⬇ PDF] | Ne iner |
|---|:---:|---|
| Başvuranlar → Bekleyen | ✅ | Seçim: sadece profil / profil + belgeler |
| Süreçteki → Devam eden | ✅ | Seçim: sadece profil / profil + belgeler |
| Süreçteki → Sonuçlanan → 🎉 İşe alındı | ✅ | Seçim: sadece profil / profil + belgeler — özlük dosyası için |
| Başvuranlar → Sonuçlanan | ✅ | **Sadece profil** — belgelere erişim kapandı |
| Süreçteki → Sonuçlanan *(işe alınan dışında)* | ✅ | **Sadece profil** |
| Adaylar | ❌ | Aday bu şirkete başvurmadı |
| Davet Edilenler | ❌ | Aday onay vermedi |
| Aday geri çekti / süreçten çekildi / hesabını sildi | ❌ | Adayın tercihine saygı |

Sonuçlanan kartlarda *"Profil + belgeler"* seçeneği **🔒 gri** ve açıklamalı: *"Başvuru sonuçlandığı için belgelere erişim kapandı."*

| Kural | |
|---|---|
| Kim indirebilir | İlanı görebilen herkes — sorumlu, sahip, yönetici |
| Kayıt | Kim, ne zaman, kimin profilini, hangi seçenekle indirdi — admin görür |
| Toplu indirme | **Yok** — tek tek |
| Excel'e aktarma | **Yok** |
| Adaya bilgi | Başvuru ekranında: *"Özgeçmişin ve paylaştığın belgeler işveren tarafından görüntülenebilir ve indirilebilir."* *(`aday-paneli-final.md` 9.1)* |

---

## 11. Yayında Olmayanlar

### 11.1 Liste

```
┌────────────────────────────────────────────────────────────────────┐
│ İlanlarım                                            [+ Yeni ilan] │
│                                                                    │
│ [Yayında 3]  [Yayında olmayanlar 2]  [Sonuçlananlar 4]  [Taslak 2] │
│               ▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔                               │
│ Sorumlu: [Tümü ▾]     Durum: [Tümü ▾]     Sırala: [En yeni ▾]      │
├────────────────────────────────────────────────────────────────────┤
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ Bilişim ve Yazılım                          ‖ Duraklatıldı     │ │
│ │ Senior Backend Developer                                  [⋮]  │ │
│ │ Mehmet Kaya · Kadıköy, İstanbul · 12 gün kaldı                 │ │
│ │                                                                │ │
│ │     ×          8 !           10                7               │ │
│ │ [Adaylar]  [Başvuranlar]  [Davet Edilenler]  [Süreçteki]       │ │
│ │                                                                │ │
│ │ [Devam ettir]                          [Sonuç bildir]          │ │
│ └────────────────────────────────────────────────────────────────┘ │
│                                                                    │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ Finans ve Muhasebe                          Süresi doldu       │ │
│ │ Muhasebe Uzmanı                                           [⋮]  │ │
│ │ Zeynep Ak · Çankaya, Ankara · Bitti: 15 Eylül                  │ │
│ │ Tekrar yayınlamak için 8 gün kaldı                             │ │
│ │                                                                │ │
│ │     ×          2 !           0                1                │ │
│ │ [Adaylar]  [Başvuranlar]  [Davet Edilenler]  [Süreçteki]       │ │
│ │                                                                │ │
│ │ [Tekrar yayınla]                       [Sonuç bildir]          │ │
│ └────────────────────────────────────────────────────────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

**Durum süzgeci:** Tümü · Duraklatıldı · Süresi doldu

**Ortak kural — iki durumda da:**

| | Yayında olmayan ilanda |
|---|---|
| Adayların Keşfet'i | ❌ İlan görünmez |
| Yeni başvuru | ❌ Gelmez |
| Davet göndermek | ❌ Yapılamaz |
| İşverenin **Adaylar** sekmesi | 🔒 Kapalı |
| **Bekleyen başvurular** | ✅ İşveren kabul / red eder — 30 gün işler |
| **Cevap bekleyen davetler** | ✅ Aday kabul / red eder — 10 gün işler |
| **Süreçteki adaylar** | ✅ Sohbet, sonuçlandırma — 60 gün işler |
| Sayılara dokunmak | İlanın içinde o sekme açılır — işlemler yapılabilir |

### 11.2 ⏸ Duraklatıldı

**Nasıl oldu:** İşveren yayındaki ilanda **[Duraklat]**'a bastı *(Bölüm 5.2)*.

**İlanın içi — Adaylar sekmesi kilitli:**

```
┌──────────────────────────────────────────────────────────────────┐
│ ← İlanlarım                                                      │
│ Senior Backend Developer                       ‖ Duraklatıldı    │
│                                                                  │
│ [Adaylar ×] [Başvuranlar 8] [Davet Edilenler 10] [Süreçteki 7]   │
│  ▔▔▔▔▔▔▔▔▔▔▔                                                     │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│                    ‖ İlan duraklatıldı                           │
│                                                                  │
│      Uygun adayları görmek ve davet göndermek için               │
│      ilanı devam ettirin.                                        │
│                                                                  │
│                      [Devam ettir]                               │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
```

**Butonlar:**

| Buton | Basınca | Sonuç |
|---|---|---|
| **[Devam ettir]** | Onay sorulmaz | İlan **Yayında**'ya döner · eşleştirme **yeniden hesaplanır** → *"Uygun adaylar hazırlanıyor..."* → Adaylar sekmesi açılır · kalan süreyle devam eder |
| **[Sonuç bildir]** | Sonuç bildir ekranı | Bölüm 12 |
| **[⋮]** | İlanı devret · Kopyala | |

| Kural | |
|---|---|
| Yayın süresi | **İşlemeye devam eder** — kartta kalan gün görünür |
| Duraklatılmışken süre biterse | Kendiliğinden **⏱ Süresi doldu**'ya geçer · [Devam ettir] yerine [Tekrar yayınla] gelir |
| [Uzat] | Yok — önce devam ettirmeli |
| Arkada | Eşleşme satırları silinir *(kimse görmüyor)* · devam ettirince taze hesaplanır — duraklatma süresince kaydolan adaylar da listeye girer |
| Bekleyen başvuruya adayın gördüğü | İlan başlığında *"İlan geçici olarak durduruldu"* · başvuru *"Değerlendiriliyor"* olarak kalır |

### 11.3 ⏱ Süresi doldu

**Nasıl oldu:** Yayın süresi (60 gün + uzatmalar) bitti. **İşverenin kararı değil** — elindeki işi kesmiyoruz.

**İki süre işler:**

```
Gün 0 — süre doldu         Gün 30                         Gün 75
├──────────────────────────┼──────────────────────────────┤
│ Tekrar yayınla  ✓        │ Tekrar yayınla  (kilitli)    │ Sistem kapatır:
│ Sonuç bildir    ✓        │ Sonuç bildir    ✓            │ ? Sonuç bildirilmedi
                                                            → Sonuçlananlar
```

**Kart — ilk 30 gün:**

```
┌──────────────────────────────────────────────────────────────┐
│ Finans ve Muhasebe                          Süresi doldu     │
│ Muhasebe Uzmanı                                          [⋮] │
│ Zeynep Ak · Çankaya, Ankara · Bitti: 15 Eylül                │
│ Tekrar yayınlamak için 8 gün kaldı                           │
│                                                              │
│     ×          2 !           0                1              │
│ [Adaylar]  [Başvuranlar]  [Davet Edilenler]  [Süreçteki]     │
│                                                              │
│ [Tekrar yayınla]                       [Sonuç bildir]        │
└──────────────────────────────────────────────────────────────┘
```

**Kart — 30. günden sonra:**

```
┌──────────────────────────────────────────────────────────────┐
│ Finans ve Muhasebe                          Süresi doldu     │
│ Muhasebe Uzmanı                                          [⋮] │
│ Zeynep Ak · Çankaya, Ankara · Bitti: 15 Eylül                │
│ Tekrar yayınlama süresi geçti                                │
│ ! Sonucu bildirmek için 12 gün kaldı                         │
│                                                              │
│     ×          0              0                1             │
│ [Adaylar]  [Başvuranlar]  [Davet Edilenler]  [Süreçteki]     │
│                                                              │
│ [Tekrar yayınla ×]                    [Sonuç bildir]         │
└──────────────────────────────────────────────────────────────┘
```

**Butonlar:**

| Buton | Ne zaman | Sonuç |
|---|---|---|
| **[Tekrar yayınla]** | Sadece ilk **30 gün** | İlan **Yayında**'ya döner · **aynı ilan** · yeni 60 gün · eşleştirme sıfırdan hesaplanır · daha önce gizlenen, reddedilen, başvurmuş adaylar **tekrar listeye düşmez** |
| **[Sonuç bildir]** | **75. güne kadar** | Bölüm 12 |
| **[⋮]** | Her zaman | İlanı devret · Kopyala |

**Uyarılar — sorumluya:**

| Gün | Bildirim |
|---|---|
| 0 | 🔔 *"Muhasebe Uzmanı ilanınızın süresi doldu. 2 bekleyen başvuru, 1 süreçteki aday var."* · sahip / yöneticiye 🔔 |
| 23 | 🔔 *"Tekrar yayınlamak için 7 gün kaldı."* |
| 68 | 📱 🔔 *"Sonucu 7 gün içinde bildirin. Bildirmezseniz şirket raporunuzda 'sonuç bildirilmedi' görünecek."* · sahip / yöneticiye 🔔 |
| 75 | 🔔 *"Muhasebe Uzmanı ilanı sonuç bildirilmediği için kapatıldı."* · sahip / yöneticiye 🔔 |

**75. günde ne olur:**

| | |
|---|---|
| İlan | **❓ Sonuç bildirilmedi** → Sonuçlananlar |
| Bekleyen başvuru / davet | Zaten kalmadı — en geç 30. günde süreleri doldu |
| Süreçteki adaylar | Sonlanır · adaya nötr mesaj: *"ABC Teknoloji ile Muhasebe Uzmanı sürecin sona erdi."* *(aday kataloğu 5.4)* · sohbet kapanır |
| Şirket raporu | *"❓ Sonuç bildirilmedi"* satırına eklenir — **adaylar da görür** |
| Sonradan düzeltme | **Yok** |

**30. günde arkada:** Tekrar yayınlanamayacağı için eşleştirmede artık işe yaramayan veriler silinir — ilanın anlam vektörü ve "bir daha gösterme" kayıtları.

**Neden bu süreler:** Hiçbir ilan sonsuza kadar askıda kalmasın. En uzun ömür: 180 gün yayın + 75 gün = **255 gün.**

---
## 12. Sonuç Bildir

### 12.1 Ne demek

**Sonuç bildirmek = "Bu işe alım tamamen bitti."** İlanla ilgili hiçbir şey açık kalmaz — bekleyen başvurular, cevap bekleyen davetler ve süreçteki adaylar sonlanır.

> **Görüşmesi süren işveren henüz sonuç bildirmez** — ilanı **duraklatır** veya süresinin dolmasını bekler. İşi bitince sonucu bildirir.

**Nereden:**

| Durum | Buton |
|---|---|
| 🟢 Yayında | [Sonuç bildir] |
| ⏸ Duraklatıldı | [Sonuç bildir] |
| ⏱ Süresi doldu | [Sonuç bildir] — 75. güne kadar |

**Kim:** İlanın sorumlusu · sahip · yönetici *(yönetici başkasının ilanında da bildirebilir — uzman unutursa toparlar)*

### 12.2 Ekran — seçim yapılmadan

```
┌──────────────────────────────────────────────────────────────────┐
│ Sonuç bildir — Senior Backend Developer                          │
├──────────────────────────────────────────────────────────────────┤
│ Bu işe alım nasıl sonuçlandı? *                                  │
│                                                                  │
│ ○ Pozisyon doldu — işe alım yaptım                               │
│ ○ Uygun aday bulamadık                                           │
│ ○ Alımdan vazgeçtik — pozisyon iptal                             │
│                                                                  │
│ ──────────────────────────────────────────────────────────────── │
│ ■ Devam eden her şey sonlandırılacak                             │
│    3 bekleyen başvuru                                            │
│    2 cevap bekleyen davet                                        │
│    3 süreçteki aday                                              │
│                                                                  │
│ ! Sonuç bildirilen ilan tekrar yayınlanamaz.                     │
│                                                                  │
│ [Vazgeç]                                     [Sonucu kaydet]     │
└──────────────────────────────────────────────────────────────────┘
```

Devam eden hiçbir şey yoksa **⏹** bölümü görünmez. **[Sonucu kaydet]** bir seçenek işaretlenince aktif olur.

### 12.3 "Pozisyon doldu" seçilince

```
┌──────────────────────────────────────────────────────────────────┐
│ Sonuç bildir — Senior Backend Developer                          │
├──────────────────────────────────────────────────────────────────┤
│ Bu işe alım nasıl sonuçlandı? *                                  │
│                                                                  │
│ ● Pozisyon doldu — işe alım yaptım                               │
│ ○ Uygun aday bulamadık                                           │
│ ○ Alımdan vazgeçtik — pozisyon iptal                             │
│                                                                  │
│ ──────────────────────────────────────────────────────────────── │
│ Süreçteki adaylardan kimi işe aldınız?                           │
│    ☑ Ayşe Kaya — Backend Geliştirici                             │
│    ☑ Burak Can — Yazılım Mühendisi                               │
│    ☐ Emre Yıldız — Backend Developer                             │
│    ☐ Platform dışından işe aldım                                 │
│                                                                  │
│ ──────────────────────────────────────────────────────────────── │
│ ■ Diğerlerinin süreci sonlandırılacak                            │
│    3 bekleyen başvuru · 2 davet · 1 süreçteki aday               │
│                                                                  │
│    Adaylara gidecek mesaj:                                       │
│    "ABC Teknoloji'nin Senior Backend Developer pozisyonu         │
│     doldurulduğu için süreç sona erdi."                          │
│                                                                  │
│ ! Sonuç bildirilen ilan tekrar yayınlanamaz.                     │
│                                                                  │
│ [Vazgeç]                                     [Sonucu kaydet]     │
└──────────────────────────────────────────────────────────────────┘
```

| | |
|---|---|
| İşaretlenenler | 🎉 *İşe alındı* · **işe alım onayı** başlar *(Bölüm 9.6)* |
| İşaretlenmeyen süreçtekiler | ⏹ Sonlanır — yukarıdaki mesaj |
| Hiç kimse işaretlenmezse | Kaydedilebilir — *"Platform dışından işe aldım"* kabul edilir |
| Süreçte hiç aday yoksa | *"Kimi işe aldınız?"* bölümü yerine sadece ☐ *Platform dışından işe aldım* |

### 12.4 "Uygun aday bulamadık" seçilince

```
│ ○ Pozisyon doldu — işe alım yaptım                               │
│ ● Uygun aday bulamadık                                           │
│ ○ Alımdan vazgeçtik — pozisyon iptal                             │
│                                                                  │
│ Aynı pozisyonu farklı şartlarla tekrar açmak isterseniz          │
│    ilanı kopyalayabilirsiniz: [⋮] → Kopyala                      │
│                                                                  │
│ ■ Devam eden her şey sonlandırılacak                             │
│    3 bekleyen başvuru · 2 davet · 3 süreçteki aday               │
│                                                                  │
│    Adaylara gidecek mesaj:                                       │
│    "ABC Teknoloji, Senior Backend Developer pozisyonu için       │
│     değerlendirme sürecini sonlandırdı."                         │
```

### 12.5 "Alımdan vazgeçtik" seçilince

```
│ ○ Pozisyon doldu — işe alım yaptım                               │
│ ○ Uygun aday bulamadık                                           │
│ ● Alımdan vazgeçtik — pozisyon iptal                             │
│                                                                  │
│ ■ Devam eden her şey sonlandırılacak                             │
│    3 bekleyen başvuru · 2 davet · 3 süreçteki aday               │
│                                                                  │
│    Adaylara gidecek mesaj:                                       │
│    "ABC Teknoloji, Senior Backend Developer pozisyonunu iptal    │
│     etti. Süreç sona erdi."                                      │
```

### 12.6 Adaya giden mesajlar — neden farklı

| Sonuç | Mesaj | Neden bu mesaj |
|---|---|---|
| **Pozisyon doldu** | *"…pozisyonu doldurulduğu için süreç sona erdi."* | Doğru ve net — kişisel bir ret değil |
| **Uygun aday bulamadık** | *"…değerlendirme sürecini sonlandırdı."* | **Nötr** — *"uygun aday bulunamadı"* demek adaya *"sen uygun değildin"* diye okunur, gereksiz kırar |
| **Alımdan vazgeçtik** | *"…pozisyonunu iptal etti. Süreç sona erdi."* | Doğru ve **rahatlatıcı** — aday *"benimle ilgili değil"* der |
| *İşe alınanlar* | *"…seni işe aldığını bildirdi. Doğru mu?"* | İşe alım onayı |

İşveren ekranda seçim değiştirdikçe mesaj kutusu da değişir — **ne gideceğini önceden bilir.**

### 12.7 Son onay

```
┌──────────────────────────────────────────────┐
│ Senior Backend Developer ilanının sonucu     │
│ "Pozisyon doldu" olarak kaydedilecek.        │
│                                              │
│ 2 adaya işe alım bildirimi,                  │
│ 6 adaya süreç sona erdi mesajı gidecek.      │
│                                              │
│ İlan tekrar yayınlanamaz.                    │
│                                              │
│ [Vazgeç]                    [Onayla]         │
└──────────────────────────────────────────────┘
```

### 12.8 Kaydedince ne olur

| | |
|---|---|
| İlan | **Sonuçlananlar**'a geçer · ✓ Pozisyon doldu / ✕ Uygun aday bulunamadı / ✕ Alımdan vazgeçildi |
| Bekleyen başvurular | ⏹ Süreç sonlandırıldı · adaya 📱 🔔 |
| Cevap bekleyen davetler | ⏹ Süreç sonlandırıldı · adaya 📱 🔔 |
| Süreçteki adaylar | İşe alınanlar 🎉 · diğerleri ⏹ · adaya 📱 🔔 |
| Sohbetler | **Hepsi kapanır** — geçmiş görünür |
| Belgelere erişim | Kapanır — işe alınanlar hariç |
| İçerideki sayılar | Hepsi **0** — [Sil] hemen açılır |
| Arkada | Eşleşme satırları · ilanın anlam vektörü · "bir daha gösterme" kayıtları **silinir** |
| Şirket raporu | Ertesi gün güncellenir |

---

## 13. Sonuçlananlar

### 13.1 Liste

```
┌────────────────────────────────────────────────────────────────────┐
│ İlanlarım                                            [+ Yeni ilan] │
│                                                                    │
│ [Yayında 3]  [Yayında olmayanlar 2]  [Sonuçlananlar 4]  [Taslak 2] │
│                                       ▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔            │
│ Sorumlu: [Tümü ▾]      Sonuç: [Tümü ▾]      Sırala: [En yeni ▾]    │
├────────────────────────────────────────────────────────────────────┤
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ Bilişim ve Yazılım                          ✓ Pozisyon doldu   │ │
│ │ Senior Backend Developer                                  [⋮]  │ │
│ │ Mehmet Kaya · Kadıköy, İstanbul                                │ │
│ │ Yayın: 1 Ağustos – 28 Eylül · Sonuç: 28 Eylül                  │ │
│ │                                                                │ │
│ │ 48 başvuru · 10 davet · 7 sürece alındı                        │ │
│ │                                                                │ │
│ │ * İşe alınanlar                                                │ │
│ │    Ayşe Kaya       ✓ Aday onayladı                             │ │
│ │    Burak Can       ! Aday işe alınmadığını bildirdi            │ │
│ │                    [Yanlış işaretledim]  [Adaya tekrar sor]    │ │
│ │                                                                │ │
│ │ [İlan özeti]                                     [Sil]         │ │
│ └────────────────────────────────────────────────────────────────┘ │
│                                                                    │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ Lojistik ve Depo                            ✓ Pozisyon doldu   │ │
│ │ Depo Sorumlusu                                            [⋮]  │ │
│ │ Can Demir · Gebze, Kocaeli                                     │ │
│ │ Yayın: 5 Temmuz – 2 Eylül · Sonuç: 10 Eylül                    │ │
│ │                                                                │ │
│ │ 22 başvuru · 4 davet · 3 sürece alındı                         │ │
│ │                                                                │ │
│ │ Platform dışından işe alım yapıldı                             │ │
│ │                                                                │ │
│ │ [İlan özeti]                                     [Sil]         │ │
│ └────────────────────────────────────────────────────────────────┘ │
│                                                                    │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ Bilişim ve Yazılım                   ✕ Uygun aday bulunamadı   │ │
│ │ DevOps Mühendisi                                          [⋮]  │ │
│ │ Mehmet Kaya · Uzaktan                                          │ │
│ │ Yayın: 1 Haziran – 15 Ağustos · Sonuç: 20 Ağustos              │ │
│ │                                                                │ │
│ │ 15 başvuru · 12 davet · 4 sürece alındı                        │ │
│ │                                                                │ │
│ │ [İlan özeti]                                     [Sil]         │ │
│ └────────────────────────────────────────────────────────────────┘ │
│                                                                    │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ Satış                                   ✕ Alımdan vazgeçildi   │ │
│ │ Saha Satış Temsilcisi                                     [⋮]  │ │
│ │ Can Demir · Bursa                                              │ │
│ │ Yayın: 10 Mayıs – 5 Haziran · Sonuç: 5 Haziran                 │ │
│ │                                                                │ │
│ │ 9 başvuru · 0 davet · 0 sürece alındı                          │ │
│ │                                                                │ │
│ │ [İlan özeti]                                     [Sil]         │ │
│ └────────────────────────────────────────────────────────────────┘ │
│                                                                    │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ Finans ve Muhasebe                      ? Sonuç bildirilmedi   │ │
│ │ Muhasebe Uzmanı                                           [⋮]  │ │
│ │ Zeynep Ak · Çankaya, Ankara                                    │ │
│ │ Yayın: 15 Temmuz – 15 Eylül · Kapandı: 29 Kasım                │ │
│ │                                                                │ │
│ │ 22 başvuru · 4 davet · 2 sürece alındı                         │ │
│ │                                                                │ │
│ │ [İlan özeti]                                     [Sil]         │ │
│ └────────────────────────────────────────────────────────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

**Süzgeç — Sonuç:** Tümü · Pozisyon doldu · Uygun aday bulunamadı · Alımdan vazgeçildi · Sonuç bildirilmedi

### 13.2 Dört sonuç

| Etiket | Nasıl oldu | Kartta ek bilgi |
|---|---|---|
| ✓ **Pozisyon doldu** | İşveren bildirdi | 🎉 İşe alınanlar + onay durumları · veya 🏢 Platform dışından |
| ✕ **Uygun aday bulunamadı** | İşveren bildirdi | — |
| ✕ **Alımdan vazgeçildi** | İşveren bildirdi | — |
| ❓ **Sonuç bildirilmedi** | Sistem — süresi dolduktan 75 gün sonra | — |

**Kartta neden sayı sekmeleri yok:** İçeride bekleyen hiçbir şey kalmadı. Yerine ilanın **özeti** görünür: kaç başvuru, kaç davet, kaç kişi sürece alındı.

**İşe alınanların onay durumu** ve çelişki butonları → Bölüm 9.6. İşe alınan aday hesabını sildiyse listede **"Silinmiş kullanıcı"** yazar, onay durumu kalır (*"Silinmiş kullanıcı · ✓ Aday onayladı"*); onay bekleniyorsa *"Aday cevap vermedi"* olarak kesinleşir ve [Adaya tekrar sor] / [Yanlış işaretledim] kalkar.

### 13.3 [İlan özeti]

**Neden:** Sonuçlanmış ilana geriye dönük bakmak — *"geçen sefer kimleri sürece almıştık?"*, *"Burak'la ne konuşmuştuk?"*, *"hangi şartlarla açmıştık?"*

**Basınca — salt okunur ekran:**

```
┌──────────────────────────────────────────────────────────────────┐
│ ← Sonuçlananlar                                                  │
│ Senior Backend Developer                       ✓ Pozisyon doldu  │
│ Yayın: 1 Ağustos – 28 Eylül · Sonuç: 28 Eylül                    │
│ × Bu ilan sonuçlandı — sadece görüntüleme                        │
├──────────────────────────────────────────────────────────────────┤
│ ── Bu ilanın özeti ───────────────────────────────────────────── │
│ 340 görüntülenme                                                 │
│                                                                  │
│ 48 başvuru                                                       │
│    ✓ 5 kabul · ✕ 38 red · ~ 2 cevapsız · ↩ 3 aday geri çekti     │
│                                                                  │
│ 10 davet                                                         │
│    ✓ 2 kabul · ✕ 5 red · ~ 3 cevapsız                            │
│                                                                  │
│ 7 sürece alındı                                                  │
│    * 2 işe alındı · ✕ 3 devam edilmedi · ↩ 1 çekildi ·           │
│    ■ 1 süreç sonlandırıldı                                       │
│                                                                  │
│ Sonuç: ✓ Pozisyon doldu — Mehmet Kaya, 28 Eylül                  │
├──────────────────────────────────────────────────────────────────┤
│ [Başvuranlar] [Davet Edilenler] [Süreçteki] [İlan bilgisi]       │
│  ▔▔▔▔▔▔▔▔▔▔▔                                                     │
│ [Foto]  Burak Can · ✕ Reddedildi · 18 Eylül                      │
│ [Foto]  Selin Er · ~ Yanıtlanmadan sona erdi · 10 Eylül          │
│ [Foto]  Deniz Ay · ■ Süreç sonlandırıldı · 28 Eylül              │
│ [Foto]  Emre Yıldız · ↩ Aday başvurusunu geri çekti · 8 Eylül    │
└──────────────────────────────────────────────────────────────────┘
```

**Sekmeler:**

| Sekme | İçinde |
|---|---|
| **Başvuranlar** | Tüm başvurular ve sonuçları |
| **Davet Edilenler** | Tüm davetler ve sonuçları |
| **Süreçteki** | Sürece alınanlar + sonuçları · **[Sohbeti gör]** — salt okunur |
| **İlan bilgisi** | İlanın kendisi — aşağıda |

*(Adaylar sekmesi yok — ilan artık eşleştirmede değil.)*

**İlan bilgisi sekmesi:**

```
┌──────────────────────────────────────────────────────────────────┐
│ [Başvuranlar] [Davet Edilenler] [Süreçteki] [İlan bilgisi]       │
│                                              ▔▔▔▔▔▔▔▔▔▔▔▔▔▔      │
│ ── Adaylar ilanı böyle gördü ─────────────────────────────────── │
│ ┌──────────────────────────────────────────────────────────────┐ │
│ │ [İŞ İLANI]                                                   │ │
│ │ Senior Backend Developer                                     │ │
│ │ ABC Teknoloji  ✓ Doğrulanmış şirket                          │ │
│ │ Kadıköy, İstanbul · Hibrit · Tam zamanlı                     │ │
│ │ 80.000 – 110.000 TRY (net)                                   │ │
│ │                                                              │ │
│ │ Olmazsa olmaz: React (5 yıl) · .NET (8 yıl) · PostgreSQL     │ │
│ │ Tercih edilen: Docker · Kubernetes                           │ │
│ │ Bölüm: Bilgisayar / Yazılım Mühendisliği                     │ │
│ │ Eğitim: Sadece mezunlar · Lisans ve üstü                     │ │
│ │ Tecrübe: 3–6 yıl                                             │ │
│ │ Diğer: Sürücü belgesi gerekli değil · Seyahat engeli         │ │
│ │        olmamalı · Vardiya yok                                │ │
│ │                                                              │ │
│ │ İş tanımı:                                                   │ │
│ │ Ödeme altyapımızda mikroservisler geliştirecek, milyonlarca  │ │
│ │ işlemi taşıyan sistemlerin performansından sorumlu           │ │
│ │ olacaksınız.                                                 │ │
│ └──────────────────────────────────────────────────────────────┘ │
│                                                                  │
│ ── Sadece size görünen ───────────────────────────────────────── │
│ Olmazsa olmaz yetenekler kutusuna yazdığınız:                    │
│   "5 yıl React bilen, 8 yıl .NET tecrübesi olan, PostgreSQL'e    │
│    hakim"                                                        │
│   Algılanan: React · 5 yıl   .NET · 8 yıl   PostgreSQL           │
│                                                                  │
│ ── Hayat hikâyesi ────────────────────────────────────────────── │
│ 1 Ağustos   Yayınlandı — Mehmet Kaya                             │
│ 25 Ağustos  Duraklatıldı — Mehmet Kaya                           │
│ 30 Ağustos  Devam ettirildi — Mehmet Kaya                        │
│ 15 Eylül    30 gün uzatıldı — Ali Kaya                           │
│ 28 Eylül    Sonuç bildirildi: Pozisyon doldu — Mehmet Kaya       │
└──────────────────────────────────────────────────────────────────┘
```

| Parça | Neden |
|---|---|
| **Adaylar ilanı böyle gördü** | Adayın gördüğü ilan detayının **birebir aynısı** — ilanın **sonuçlandığı anki** son hali |
| **Sadece size görünen** | Kutulara yazılan ham cümle + algılanan etiketler — yeni ilan yazarken işe yarar |
| **Hayat hikâyesi** | Kim açtı, ne zaman uzatıldı, duraklatıldı, kime devredildi, kim sonuçlandırdı — ekip içinde şeffaflık |

| Neler yapılamaz | |
|---|---|
| Kabul / red / davet / mesaj | ❌ |
| Belgeleri açmak | ❌ *(işe alınanlar hariç)* |
| Aday profili | ✅ Görülebilir — aday hesabını silmediyse |
| İşe alım çelişkisi düzeltmek | ✅ 30 gün boyunca |
| Kopyalamak | ✅ [⋮] → Kopyala |

| Ne kadar görünür | |
|---|---|
| Özet sayıları | İlan silinene kadar |
| İçindeki aday kartları | 90 gün — sonra listeden kalkar, özetteki sayılar kalır |

### 13.4 [Sil]

**Neden:** Liste kalabalıklaşmasın — eski ilanlar temizlensin.

**Sadece Sonuçlananlar'da** var. Yayındaki veya yayında olmayan ilan silinemez — önce sonuç bildirilmeli.

**Basınca:**

```
┌──────────────────────────────────────────────┐
│ "Senior Backend Developer" ilanını silmek    │
│ istediğinize emin misiniz?                   │
│                                              │
│ İlan listenizden kaldırılacak.               │
│ İlan özeti görüntülenemeyecek.               │
│ Bu işlem geri alınamaz.                      │
│                                              │
│ i Şirket raporunuzdaki sayılar değişmez.     │
│                                              │
│ [Vazgeç]                          [Sil]      │
└──────────────────────────────────────────────┘
```

**Bekleyen bir işe alım onayı veya çelişki varsa ek uyarı:**
```
│ ! 1 işe alım onayı hâlâ bekleniyor.          │
│    Silerseniz düzeltme yapamazsınız.         │
```

| Silince | |
|---|---|
| İşverenin İlanlarım listesi | İlan kalkar |
| İlan özeti, Kopyala | Artık yapılamaz |
| **Şirket raporu** | **Değişmez** — rapor ayrı bir özet tablosundan okunur *(Bölüm 22)* |
| Aday tarafı | Değişmez — adayın sonuçlanan kartı kendi listesinde 90 gün kalır · ilana dokunursa *"Bu ilan artık mevcut değil."* |
| Bekleyen işe alım onayı | Arka planda devam eder — aday cevap verirse rapora işlenir |
| Arkada | **Soft delete** — kayıt işaretlenir, işverene gösterilmez · admin görür · 2 yıl sonra tamamen silinir |

| Kim silebilir | |
|---|---|
| Sahip, yönetici | Tüm sonuçlanan ilanlar |
| Uzman | Kendi sonuçlanan ilanları |

**Neden şirket raporu değişmiyor:** Kötü sonuçlanan ilanlarını silerek raporunu güzelleştirmek mümkün olmasın.

### 13.5 Admin ilanı kaldırırsa

Adaylar bir ilanı **"İlanı bildir"** ile şikâyet edebilir (sahte ilan, para isteme, ayrımcı ifade…). Admin inceleyip ilanın kurallara aykırı olduğuna karar verirse ilanı **kaldırır.**

| | |
|---|---|
| İlan | Hemen Keşfet'ten ve tüm listelerden çıkar · tekrar yayınlanamaz, kopyalanamaz |
| Bekleyen başvuru, davet, süreçteki adaylar | **Hepsi sonlanır** · sohbetler kapanır |
| Adaylara | 📱 🔔 ✉️ *"Görüştüğün XYZ Ticaret ilanı platform kurallarına aykırı bulunduğu için kaldırıldı. Senden para veya kişisel bilgi istenirse paylaşma."* *(aday kataloğu 5.6 — kapatılamaz)* |
| İşverene | Sorumluya 📱 🔔 ✉️, sahibe 🔔 ✉️ *"Senior Backend Developer ilanınız platform kurallarına aykırı bulunduğu için kaldırıldı."* + sebep |
| Kim bildirdi | İşveren **hiçbir zaman göremez** |

> ⚠️ Kaldırılan ilanın İlanlarım'da nasıl göründüğü, itiraz yolu ve şirket raporuna etkisi **admin paneli** konuşulurken netleşecek.

---
## 14. Şirket Profili ve Şirket Raporu

### 14.1 Ne işe yarar

Şirket profili **şirketin vitrini.** Adaylar şirketi buradan tanır; ekip de **adayların şirketi nasıl gördüğünü** buradan görür. Profilin içinde **işe alım raporu** var — şirketin işe alım davranışı herkese açık.

> **Felsefe:** Platform kimseyi zorlamaz — ama her şeyi görünür kılar. İşini düzgün yapan şirket raporunda bunu gösterir; yapmayanı adaylar görür.

**Menü → 🏢 Şirket profili** · Aday ise ilan detayındaki şirket adına dokunarak açar.

### 14.2 Sahip görünümü

```
┌───────────────────────────────────────────────────────────────────┐
│ Şirket profili                                                    │
│ Adaylar şirketinizi böyle görüyor                       [Düzenle] │
├───────────────────────────────────────────────────────────────────┤
│ [Logo]  ABC Teknoloji  ✓ Doğrulanmış şirket                       │
│         Bilişim ve Yazılım · 51-200 çalışan · Kuruluş: 2015       │
│         Merkez: Kadıköy, İstanbul                                 │
│         abcteknoloji.com · LinkedIn · X                           │
│                                                                   │
│ ── Hakkında ────────────────────────────────────────────────────  │
│ 2015'ten beri ödeme sistemleri geliştiriyoruz. Türkiye ve         │
│ Avrupa'da 40'tan fazla bankaya altyapı sağlıyoruz. Ekibimiz 120   │
│ kişiden oluşuyor.                                                 │
│                                                                   │
│ ── İmkanlar ────────────────────────────────────────────────────  │
│ Yemek kartı · Özel sağlık sigortası · Esnek çalışma saatleri ·    │
│ Yıllık eğitim bütçesi                                             │
│                                                                   │
│ ── Lokasyonlar ─────────────────────────────────────────────────  │
│ Kadıköy, İstanbul (Merkez) · Çankaya, Ankara · Berlin             │
│                                                                   │
│ ── İşe alım raporu (son 12 ay) ────────── Güncelleme: bugün 03:00 │
│ İlanlar                                          15               │
│    ● Şu an yayında                                3               │
│    ✓ Pozisyon doldu                               8               │
│    ✕ Uygun aday bulunamadı                        2               │
│    ✕ Alımdan vazgeçildi                           1               │
│    ? Sonuç bildirilmedi                           1               │
│ Başvurular                                      240               │
│    ✓ Kabul edildi                                42  (%18)        │
│    ✕ Reddedildi                                 179  (%74)        │
│    ~ Cevapsız kaldı                              19  (%8)         │
│ Davetler                                         30               │
│    ✓ Aday kabul etti                             18  (%60)        │
│    ✕ Aday reddetti                                8  (%27)        │
│    ~ Aday cevap vermedi                           4  (%13)        │
│ Sürece alınan aday                               60               │
│ * İşe alım                                       12               │
│    ✓ Aday tarafından onaylandı                   10               │
│                                                                   │
│ ── Açık ilanlar (3) ────────────────────────────────────────────  │
│ Senior Backend Developer · Mehmet Kaya              [İlana git →] │
│ Muhasebe Uzmanı · Zeynep Ak                         [İlana git →] │
│ Depo Sorumlusu · Can Demir                          [İlana git →] │
├───────────────────────────────────────────────────────────────────┤
│ ── Sadece size görünen ─────────────────────────────────────────  │
│ ! 8 başvuru 7 gün içinde cevapsız kalacak                         │
│    Senior Backend Developer — 6 başvuru      [Başvuranlara git →] │
│    Muhasebe Uzmanı — 2 başvuru               [Başvuranlara git →] │
│                                                                   │
│ ! Lojistik Uzmanı — sonucu 5 gün içinde bildirin [İlana git →]    │
└───────────────────────────────────────────────────────────────────┘
```

### 14.3 Yönetici görünümü

```
┌───────────────────────────────────────────────────────────────────┐
│ Şirket profili                                                    │
│ Adaylar şirketinizi böyle görüyor                       [Düzenle] │
├───────────────────────────────────────────────────────────────────┤
│ [Logo]  ABC Teknoloji  ✓ Doğrulanmış şirket                       │
│         Bilişim ve Yazılım · 51-200 çalışan · Kuruluş: 2015       │
│         Merkez: Kadıköy, İstanbul                                 │
│         abcteknoloji.com · LinkedIn · X                           │
│                                                                   │
│ ── Hakkında ────────────────────────────────────────────────────  │
│ 2015'ten beri ödeme sistemleri geliştiriyoruz. Türkiye ve         │
│ Avrupa'da 40'tan fazla bankaya altyapı sağlıyoruz. Ekibimiz 120   │
│ kişiden oluşuyor.                                                 │
│                                                                   │
│ ── İmkanlar ────────────────────────────────────────────────────  │
│ Yemek kartı · Özel sağlık sigortası · Esnek çalışma saatleri ·    │
│ Yıllık eğitim bütçesi                                             │
│                                                                   │
│ ── Lokasyonlar ─────────────────────────────────────────────────  │
│ Kadıköy, İstanbul (Merkez) · Çankaya, Ankara · Berlin             │
│                                                                   │
│ ── İşe alım raporu (son 12 ay) ────────── Güncelleme: bugün 03:00 │
│ İlanlar                                          15               │
│    ● Şu an yayında                                3               │
│    ✓ Pozisyon doldu                               8               │
│    ✕ Uygun aday bulunamadı                        2               │
│    ✕ Alımdan vazgeçildi                           1               │
│    ? Sonuç bildirilmedi                           1               │
│ Başvurular                                      240               │
│    ✓ Kabul edildi                                42  (%18)        │
│    ✕ Reddedildi                                 179  (%74)        │
│    ~ Cevapsız kaldı                              19  (%8)         │
│ Davetler                                         30               │
│    ✓ Aday kabul etti                             18  (%60)        │
│    ✕ Aday reddetti                                8  (%27)        │
│    ~ Aday cevap vermedi                           4  (%13)        │
│ Sürece alınan aday                               60               │
│ * İşe alım                                       12               │
│    ✓ Aday tarafından onaylandı                   10               │
│                                                                   │
│ ── Açık ilanlar (3) ────────────────────────────────────────────  │
│ Senior Backend Developer · Mehmet Kaya              [İlana git →] │
│ Muhasebe Uzmanı · Zeynep Ak                         [İlana git →] │
│ Depo Sorumlusu · Can Demir                          [İlana git →] │
├───────────────────────────────────────────────────────────────────┤
│ ── Sadece size görünen ─────────────────────────────────────────  │
│ ! 8 başvuru 7 gün içinde cevapsız kalacak                         │
│    Senior Backend Developer — 6 başvuru      [Başvuranlara git →] │
│    Muhasebe Uzmanı — 2 başvuru               [Başvuranlara git →] │
│                                                                   │
│ ! Lojistik Uzmanı — sonucu 5 gün içinde bildirin [İlana git →]    │
└───────────────────────────────────────────────────────────────────┘
```

**Sahipten farkı:** Ekran aynı · **[Düzenle]** içinde kilitli alanlar için **[Değişiklik talebi]** butonu yok *(Bölüm 14.8)*.

### 14.4 Uzman görünümü (Zeynep Ak)

```
┌───────────────────────────────────────────────────────────────────┐
│ Şirket profili                                                    │
│ Adaylar şirketinizi böyle görüyor                                 │
├───────────────────────────────────────────────────────────────────┤
│ [Logo]  ABC Teknoloji  ✓ Doğrulanmış şirket                       │
│         Bilişim ve Yazılım · 51-200 çalışan · Kuruluş: 2015       │
│         Merkez: Kadıköy, İstanbul                                 │
│         abcteknoloji.com · LinkedIn · X                           │
│                                                                   │
│ ── Hakkında ────────────────────────────────────────────────────  │
│ 2015'ten beri ödeme sistemleri geliştiriyoruz. Türkiye ve         │
│ Avrupa'da 40'tan fazla bankaya altyapı sağlıyoruz. Ekibimiz 120   │
│ kişiden oluşuyor.                                                 │
│                                                                   │
│ ── İmkanlar ────────────────────────────────────────────────────  │
│ Yemek kartı · Özel sağlık sigortası · Esnek çalışma saatleri ·    │
│ Yıllık eğitim bütçesi                                             │
│                                                                   │
│ ── Lokasyonlar ─────────────────────────────────────────────────  │
│ Kadıköy, İstanbul (Merkez) · Çankaya, Ankara · Berlin             │
│                                                                   │
│ ── İşe alım raporu (son 12 ay) ────────── Güncelleme: bugün 03:00 │
│ İlanlar                                          15               │
│    ● Şu an yayında                                3               │
│    ✓ Pozisyon doldu                               8               │
│    ✕ Uygun aday bulunamadı                        2               │
│    ✕ Alımdan vazgeçildi                           1               │
│    ? Sonuç bildirilmedi                           1               │
│ Başvurular                                      240               │
│    ✓ Kabul edildi                                42  (%18)        │
│    ✕ Reddedildi                                 179  (%74)        │
│    ~ Cevapsız kaldı                              19  (%8)         │
│ Davetler                                         30               │
│    ✓ Aday kabul etti                             18  (%60)        │
│    ✕ Aday reddetti                                8  (%27)        │
│    ~ Aday cevap vermedi                           4  (%13)        │
│ Sürece alınan aday                               60               │
│ * İşe alım                                       12               │
│    ✓ Aday tarafından onaylandı                   10               │
│                                                                   │
│ ── Açık ilanlar (3) ────────────────────────────────────────────  │
│ Senior Backend Developer · Mehmet Kaya                 [İncele →] │
│ Muhasebe Uzmanı · Zeynep Ak (siz)                   [İlana git →] │
│ Depo Sorumlusu · Can Demir                             [İncele →] │
├───────────────────────────────────────────────────────────────────┤
│ ── Sadece size görünen ─────────────────────────────────────────  │
│ ! 2 başvuru 7 gün içinde cevapsız kalacak                         │
│    Muhasebe Uzmanı — 2 başvuru               [Başvuranlara git →] │
└───────────────────────────────────────────────────────────────────┘
```

| Uzmanın farkı | |
|---|---|
| [Düzenle] | **Yok** |
| Kendi ilanı | **[İlana git →]** — yönetim ekranı |
| Başkasının ilanı | **[İncele →]** — ilanın **adayın gördüğü hali**, salt okunur · başvuranları, adayları, sohbetleri **göremez** |
| Uyarılar | Sadece **kendi** ilanları |
| Rapor | Şirketin **tamamı** — herkesle aynı |

### 14.5 Aday görünümü

```
┌──────────────────────────────────────────────────────────────────┐
│ ← Geri                                                           │
├──────────────────────────────────────────────────────────────────┤
│ [Logo]  ABC Teknoloji  ✓ Doğrulanmış şirket                      │
│         Bilişim ve Yazılım · 51-200 çalışan · Kuruluş: 2015      │
│         Merkez: Kadıköy, İstanbul                                │
│         abcteknoloji.com · LinkedIn · X                          │
│                                                                  │
│ ── Hakkında ──────────────────────────────────────────────────── │
│ 2015'ten beri ödeme sistemleri geliştiriyoruz. Türkiye ve        │
│ Avrupa'da 40'tan fazla bankaya altyapı sağlıyoruz. Ekibimiz 120  │
│ kişiden oluşuyor.                                                │
│                                                                  │
│ ── İmkanlar ──────────────────────────────────────────────────── │
│ Yemek kartı · Özel sağlık sigortası · Esnek çalışma saatleri ·   │
│ Yıllık eğitim bütçesi                                            │
│                                                                  │
│ ── Lokasyonlar ───────────────────────────────────────────────── │
│ Kadıköy, İstanbul (Merkez) · Çankaya, Ankara · Berlin            │
│                                                                  │
│ ── İşe alım raporu (son 12 ay) ───────────────────────────────── │
│ İlanlar                                          15              │
│    ● Şu an yayında                                3              │
│    ✓ Pozisyon doldu                               8              │
│    ✕ Uygun aday bulunamadı                        2              │
│    ✕ Alımdan vazgeçildi                           1              │
│    ? Sonuç bildirilmedi                           1              │
│ Başvurular                                      240              │
│    ✓ Kabul edildi                                42  (%18)       │
│    ✕ Reddedildi                                 179  (%74)       │
│    ~ Cevapsız kaldı                              19  (%8)        │
│ Davetler                                         30              │
│    ✓ Aday kabul etti                             18  (%60)       │
│    ✕ Aday reddetti                                8  (%27)       │
│    ~ Aday cevap vermedi                           4  (%13)       │
│ Sürece alınan aday                               60              │
│ * İşe alım                                       12              │
│    ✓ Aday tarafından onaylandı                   10              │
│                                                                  │
│ ── Açık ilanlar (3) ──────────────────────────────────────────── │
│ Senior Backend Developer · Kadıköy, İstanbul          [İncele →] │
│ Muhasebe Uzmanı · Çankaya, Ankara                     [İncele →] │
│ Depo Sorumlusu · Gebze, Kocaeli                       [İncele →] │
└──────────────────────────────────────────────────────────────────┘
```

**İlan kartında kısa özet** — aday profile girmeden de görsün:
```
│ ABC Teknoloji  ✓ Doğrulanmış · %92 cevap · * 12 işe alım     │
```

### 14.6 Dört görünüm — farklar

| | Aday | Uzman | Yönetici | Sahip |
|---|:---:|:---:|:---:|:---:|
| Şirket bilgileri, hakkında, imkanlar, lokasyonlar | ✅ | ✅ | ✅ | ✅ |
| İşe alım raporu — tüm şirket | ✅ | ✅ | ✅ | ✅ |
| Açık ilanlarda sorumlu adı | ❌ | ✅ | ✅ | ✅ |
| Açık ilana gitmek | [İncele →] | Kendi: [İlana git →] · diğer: [İncele →] | [İlana git →] | [İlana git →] |
| "Sadece size görünen" uyarılar | ❌ | Kendi ilanları | Tüm ilanlar | Tüm ilanlar |
| [Düzenle] | ❌ | ❌ | ✅ | ✅ |
| Düzenle'de [Değişiklik talebi] | ❌ | ❌ | ❌ | ✅ |

**İşe alınanların isimleri profilde yok** — onlar ilanların içinde *(Sonuçlananlar, İlan özeti)*. Profil vitrin; iç ayrıntılar ilanlarda.

### 14.7 Şirket raporu — kurallar

**Ne gösterir:**

| Kalem | Sayılır | Sayılmaz |
|---|---|---|
| **İlanlar** | Son 12 ayda yayınlanan her ilan · kopyayla açılan yeni ilan · **silinen ilanlar da** | Taslaklar · tekrar yayınlama *(aynı ilan)* |
| **Başvuru — kabul** | Kabul edilen başvurular | |
| **Başvuru — red** | Tek tek reddedilen + ⏹ süreç sonlandırılan *(şirket bildirdi, aday cevap aldı)* | |
| **Başvuru — cevapsız** | ⌛ 30 günde yanıtlanmadan sona eren | ↩ Aday geri çektiyse — şirketin suçu değil |
| **Davet** | Kabul · red · cevap vermedi | ↩ İşveren geri çektiyse |
| **Sürece alınan** | Başvurusu kabul edilen + daveti kabul eden | |
| **İşe alım** | İşverenin *"işe alındı"* dedikleri · ayrıca kaçının **aday tarafından onaylandığı** | ⚠️ Aday *"işe alınmadım"* dediyse onaylıya sayılmaz |

**Kurallar:**

| Kural | Neden |
|---|---|
| **En az 20 başvurudan sonra** görünür | 2 başvurudan *"%50 cevap"* yanıltıcı olur |
| Yeterli veri yoksa | Rapor yerine **🆕 Platformda yeni** — kötü görünmesin |
| **Son 12 ay** | Eski kötü dönem şirketi sonsuza kadar etiketlemesin |
| **Günde bir kez** güncellenir | Gece 03:00 · ekranda *"Güncelleme: bugün 03:00"* |
| **Herkese aynı sayılar** | Adayın gördüğü ile şirketin gördüğü birebir aynı |
| Silinen ilanlar sayılır | Rapor silerek güzelleştirilemesin |

**Neden red sayısı da gösteriliyor:** Cevapsız kalan başvurular da göründüğü için şirket *"red sayım artmasın"* diye adayı cevapsız bırakamaz — **cevapsız bırakmak daha kötü görünür.** Doğru davranış (cevap vermek) ödüllenir.

**"Sadece size görünen" uyarılar** rapordan değil, **canlı kayıtlardan** gelir — anında güncellenir. İşveren başvuruyu cevaplayınca uyarı hemen düşer.

**Admin** ayrıca şirketin **aylık ve günlük dökümünü** görür *(gidişat — ör. "cevapsız oranı 3 aydır artıyor")* — bu döküm adaylara ve şirkete gösterilmez.

### 14.8 [Düzenle]

**Kim:** Sahip, yönetici.

```
┌───────────────────────────────────────────────────────────────────┐
│ ← Şirket profili         Profili düzenle                          │
├───────────────────────────────────────────────────────────────────┤
│ Logo:              [AT]                             [Değiştir]    │
│                                                                   │
│ Şirket adı:        ABC Teknoloji        × [Değişiklik talebi]     │
│ Sektör:            Bilişim › Yazılım    × [Değişiklik talebi]     │
│ Kuruluş yılı:      2015                 × [Değişiklik talebi]     │
│ Çalışan sayısı:    51-200               × [Değişiklik talebi]     │
│ Web sitesi:        abcteknoloji.com     × [Değişiklik talebi]     │
│ Şirket telefonu:   +90 216 555 00 00    × [Değişiklik talebi]     │
│ Şirket e-postası:  info@abc.com.tr      × [Değişiklik talebi]     │
│ Merkez:            Kadıköy, İstanbul    × [Değişiklik talebi]     │
│                                                                   │
│ Sosyal medya:      LinkedIn: abcteknoloji               [Düzenle] │
│                    X: abcteknoloji                      [Düzenle] │
│                    [+ Ekle]                                       │
│                                                                   │
│ Hakkında *:                                                       │
│ ┌───────────────────────────────────────────────────────────────┐ │
│ │ 2015'ten beri ödeme sistemleri geliştiriyoruz. Türkiye ve     │ │
│ │ Avrupa'da 40'tan fazla bankaya altyapı sağlıyoruz. Ekibimiz   │ │
│ │ 120 kişiden oluşuyor.                                         │ │
│ └───────────────────────────────────────────────────────────────┘ │
│                                                                   │
│ İmkanlar:                                                         │
│ ┌───────────────────────────────────────────────────────────────┐ │
│ │ Yemek kartı · Özel sağlık sigortası · Esnek çalışma           │ │
│ │ saatleri · Yıllık eğitim bütçesi                              │ │
│ └───────────────────────────────────────────────────────────────┘ │
│                                                                   │
│ Ek lokasyonlar:    Ankara Ofisi — Çankaya, Ankara  [Düzenle][Sil] │
│                    Berlin Ofisi — Berlin           [Düzenle][Sil] │
│                    [+ Lokasyon ekle]                              │
│                                                                   │
│ [Vazgeç]                                         [Kaydet]         │
└───────────────────────────────────────────────────────────────────┘
```

| Alan | Düzenleme |
|---|---|
| Logo · sosyal medya · hakkında · imkanlar · ek lokasyonlar | ✅ **Serbest** — kaydedince hemen geçerli |
| Şirket adı · sektör · kuruluş yılı · çalışan sayısı · web sitesi · şirket telefonu · şirket e-postası · merkez | 🔒 **Kilitli** — sadece **değişiklik talebiyle** |

**Neden kilitli:** Bu bilgiler admin tarafından **belgeyle onaylandı.** Serbest olsa onaylı bir şirket kimliği başka bir şeye çevrilebilir — doğrulamanın anlamı kalmaz.

**Yönetici** aynı ekranı görür — kilitli alanlarda sadece 🔒, **[Değişiklik talebi] butonu yok.**

### 14.9 [Değişiklik talebi]

**Kim:** Sadece sahip.

**Neden:** Gerçekte şirket bilgileri bazen değişir — şirket adını değiştirir, taşınır, büyür. Yolu olmazsa şirket yeni hesap açmaya çalışır.

**Basınca:**

```
┌──────────────────────────────────────────────┐
│ Değişiklik talebi — Şirket adı               │
│                                              │
│ Mevcut:  ABC Teknoloji                       │
│ Yeni *:  [_____________________________]     │
│ Sebep *: [_____________________________]     │
│ Belge:   [Dosya ekle]   (opsiyonel)          │
│                                              │
│ i Şirket adı veya merkez değişikliğinde      │
│   yeni tescil belgesi eklemeniz süreci       │
│   hızlandırır.                               │
│                                              │
│ [Vazgeç]                [Talebi gönder]      │
└──────────────────────────────────────────────┘
```

**Gönderince:**

| | |
|---|---|
| Admin | Talep kuyruğuna düşer → inceler → onaylar / reddeder |
| Beklerken | **Eski değer geçerli** · alanın yanında *"⏳ Değişiklik talebi inceleniyor"* |
| Onaylanınca | Yeni değer profilde · sahibe 📱 🔔 *"Şirket adı değişiklik talebiniz onaylandı."* |
| Reddedilince | Eski değer kalır · sahibe 📱 🔔 *"Şirket adı değişiklik talebiniz reddedildi: Belge yetersiz."* |
| Aynı alan için | **Bir seferde tek bekleyen talep** |
| Takip | **Şirket Yönetimi → Değişiklik Taleplerim** *(Bölüm 17.3)* |

---
## 15. Ekip

### 15.1 Sahip görünümü

**Menü → 👥 Ekip**

```
┌────────────────────────────────────────────────────────────────────┐
│ Ekip                                                  [+ Üye ekle] │
│ 4 kişi                                           [Roller nedir? i] │
├────────────────────────────────────────────────────────────────────┤
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ [AK]  Ali Kaya                                   Sahip         │ │
│ │       ali@abc.com · Son giriş: bugün                           │ │
│ │       2 ilan                                                   │ │
│ └────────────────────────────────────────────────────────────────┘ │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ [MK]  Mehmet Kaya                                Yönetici      │ │
│ │       mehmet@abc.com · Son giriş: dün                          │ │
│ │       5 ilan                                              [⋮]  │ │
│ └────────────────────────────────────────────────────────────────┘ │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ [ZA]  Zeynep Ak                                  Uzman         │ │
│ │       zeynep@abc.com · Son giriş: 3 gün önce                   │ │
│ │       3 ilan                                              [⋮]  │ │
│ └────────────────────────────────────────────────────────────────┘ │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ [CD]  Can Demir                                  Uzman         │ │
│ │       can@abc.com                                              │ │
│ │       ‖ Askıda — hiç giriş yapmadı                             │ │
│ │                          [Daveti yeniden gönder]         [⋮]   │ │
│ └────────────────────────────────────────────────────────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

**Üye kartında:**

| Parça | Anlamı |
|---|---|
| Baş harfler / fotoğraf · ad soyad · e-posta | |
| Rol | 👑 Sahip · 🛡 Yönetici · 👤 Uzman |
| Son giriş | En son ne zaman uygulamaya girdi |
| **📋 X ilan** | Sorumlu olduğu ilan sayısı — **dokununca İlanlarım açılır, o kişiye süzülmüş** *(Bölüm 15.4)* |
| Durum | Sadece sorun varsa: *⏳ Davet edildi, henüz giriş yapmadı* · *⏸ Askıda — hiç giriş yapmadı* |
| **[⋮]** | Üye işlemleri *(Bölüm 15.5)* · **sahibin kendi kartında yok** |

**Sıralama:** Sahip → yöneticiler → uzmanlar · her grupta ada göre.

### 15.2 Yönetici görünümü

```
┌────────────────────────────────────────────────────────────────────┐
│ Ekip                                                               │
│ 4 kişi                                           [Roller nedir? i] │
├────────────────────────────────────────────────────────────────────┤
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ [AK]  Ali Kaya                                   Sahip         │ │
│ │       ali@abc.com · Son giriş: bugün                           │ │
│ │       2 ilan                                              [⋮]  │ │
│ └────────────────────────────────────────────────────────────────┘ │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ [MK]  Mehmet Kaya (siz)                          Yönetici      │ │
│ │       mehmet@abc.com · Son giriş: bugün                        │ │
│ │       5 ilan                                                   │ │
│ └────────────────────────────────────────────────────────────────┘ │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ [ZA]  Zeynep Ak                                  Uzman         │ │
│ │       zeynep@abc.com · Son giriş: 3 gün önce                   │ │
│ │       3 ilan                                              [⋮]  │ │
│ └────────────────────────────────────────────────────────────────┘ │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ [CD]  Can Demir                                  Uzman         │ │
│ │       can@abc.com                                              │ │
│ │       ‖ Askıda — hiç giriş yapmadı                             │ │
│ └────────────────────────────────────────────────────────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

| Yöneticinin farkı | |
|---|---|
| [+ Üye ekle] | **Yok** |
| [⋮] — diğer üyelerde | Sadece **İlanlarını devret** |
| [⋮] — sahibin kartında | Sadece **Sahiplik talebi** *(Bölüm 17.5 — sahip ulaşılamıyorsa)* |
| [Daveti yeniden gönder] | Yok |
| 📋 X ilan | Dokunulabilir — tüm ilanları görebiliyor |

### 15.3 Uzman görünümü

```
┌────────────────────────────────────────────────────────────────────┐
│ Ekip                                                               │
│ 4 kişi                                           [Roller nedir? i] │
├────────────────────────────────────────────────────────────────────┤
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ [AK]  Ali Kaya                                   Sahip         │ │
│ │       ali@abc.com                                              │ │
│ │       2 ilan                                                   │ │
│ └────────────────────────────────────────────────────────────────┘ │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ [MK]  Mehmet Kaya                                Yönetici      │ │
│ │       mehmet@abc.com                                           │ │
│ │       5 ilan                                                   │ │
│ └────────────────────────────────────────────────────────────────┘ │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ [ZA]  Zeynep Ak (siz)                            Uzman         │ │
│ │       zeynep@abc.com                                           │ │
│ │       3 ilan                                                   │ │
│ └────────────────────────────────────────────────────────────────┘ │
│ ┌────────────────────────────────────────────────────────────────┐ │
│ │ [CD]  Can Demir                                  Uzman         │ │
│ │       can@abc.com                                              │ │
│ │       0 ilan                                                   │ │
│ └────────────────────────────────────────────────────────────────┘ │
└────────────────────────────────────────────────────────────────────┘
```

| Uzmanın farkı | |
|---|---|
| Butonlar | **Hiç yok** — [⋮] yok, [+ Üye ekle] yok |
| Son giriş, askı durumu | Görünmez |
| 📋 X ilan | **Sadece sayı** — dokunulamaz *(başkasının ilanlarını göremez)* · kendi kartındakine dokunursa kendi ilanları açılır |

### 15.4 📋 X ilan — kişinin ilanlarına gitmek

```
Ekip → Zeynep Ak → [3 ilan]
              ↓
┌────────────────────────────────────────────────────────────────────┐
│ ← Ekip                                                             │
│ İlanlarım                                            [+ Yeni ilan] │
│                                                                    │
│ [Yayında 1]  [Yayında olmayanlar 1]  [Sonuçlananlar 1]  [Taslak 0] │
│ Sorumlu: [Zeynep Ak ▾]                Sırala: [Bekleyen başvuru ▾] │
│           ▔▔▔▔▔▔▔▔▔▔▔ ← otomatik seçili                            │
├────────────────────────────────────────────────────────────────────┤
│ (Zeynep'in ilan kartları — Bölüm 3'teki kartların aynısı)          │
└────────────────────────────────────────────────────────────────────┘
```

| | |
|---|---|
| Açılan ekran | **İlanlarım** — ayrı bir ekran değil |
| Süzgeç | **Sorumlu: Zeynep Ak** otomatik seçili |
| Sekme sayıları | Sadece Zeynep'in ilanlarına göre |
| Geri dönmek | **← Ekip** |
| Tek ilanı devretmek | İlan kartında **[⋮] → İlanı devret** *(Bölüm 5.3)* |

**Aynı yere İlanlarım'dan da** gidilir: *Sorumlu: [Zeynep Ak ▾]* seçilerek.

### 15.5 [⋮] menüsü

**Sahip için — üyenin durumuna göre:**

| Üye durumu | [⋮] içinde |
|---|---|
| **Aktif yönetici** | Rolü değiştir · İlanlarını devret · 👑 Sahipliği devret · Ekipten çıkar |
| **Aktif uzman** | Rolü değiştir · İlanlarını devret · Ekipten çıkar |
| **⏳ Davet edildi, henüz giriş yapmadı** | Daveti yeniden gönder · Rolü değiştir · Daveti iptal et |
| **⏸ Askıda** | Daveti yeniden gönder · Rolü değiştir · Ekipten çıkar |

*Henüz giriş yapmamış üyenin ilanı olamaz — "İlanlarını devret" görünmez.*

**Kimde ne görünür:**

| Kim bakıyor | Diğer üyelerin [⋮]'si | Sahibin kartındaki [⋮] |
|---|---|---|
| **Sahip** | Yukarıdaki tam menü | — *(kendi kartı)* |
| **Yönetici** | Sadece **İlanlarını devret** | Sadece **Sahiplik talebi** |
| **Uzman** | [⋮] yok | [⋮] yok |

### 15.6 [+ Üye ekle]

**Kim:** Sadece sahip.

```
┌──────────────────────────────────────────────┐
│ Ekibe üye ekle                               │
│                                              │
│ Ad soyad *        [________________________] │
│ E-posta *         [________________________] │
│                                              │
│ Rol *                                   i    │
│ ○ Yönetici                                   │
│     Tüm ilanları görür ve yönetir,           │
│     ilan devredebilir, şirket profilini      │
│     düzenleyebilir.                          │
│ ○ Uzman                                      │
│     Sadece kendi ilanlarını görür ve         │
│     yönetir.                                 │
│                                              │
│ i Üyeye giriş bilgileri e-postayla           │
│   gönderilir. 7 gün içinde giriş yapmazsa    │
│   hesap askıya alınır.                       │
│                                              │
│ [Vazgeç]                          [Ekle]     │
└──────────────────────────────────────────────┘
```

| Alan | Kural |
|---|---|
| Ad soyad | Zorunlu |
| E-posta | Zorunlu · başka bir hesapta (aday veya başka şirket) kayıtlıysa: *"Bu e-posta başka bir hesapta kayıtlı."* |
| Rol | Zorunlu · **varsayılan seçili gelmez** — sahip bilerek seçsin |
| Telefon | **Sorulmaz** — üye ilk ilanını yayınlarken kendisi doğrular |

**[Ekle]'ye basınca:**

| Gün | Ne olur |
|---|---|
| 0 | Üye oluşur — kartta *⏳ Davet edildi, henüz giriş yapmadı* · üyeye ✉️ giriş adresi + geçici şifre |
| İlk giriş | **Kendi şifresini belirlemesi zorunlu** — e-postadaki şifre bir daha işe yaramaz |
| 5 | Giriş yapmadıysa üyeye ✉️ *"Hesabınız 2 gün içinde giriş yapılmazsa askıya alınacak."* |
| 7 | Giriş yapmadıysa → **⏸ Askıda** · sahibe 🔔 *"Can Demir 7 gün içinde giriş yapmadığı için hesabı askıya alındı."* |
| Askıdayken | Sahip **[Daveti yeniden gönder]** → yeni geçici şifre, yeni 7 gün |

**Neden askıya alma:** E-postayla gönderilen, hiç kullanılmamış şifre risklidir — yanlış adrese gitmiş ya da e-posta ele geçirilmiş olabilir.

### 15.7 [Roller nedir? ⓘ]

**Kim görür:** Herkes. Ayrıca *Üye ekle* ve *Rolü değiştir* pencerelerinde **ⓘ** ile açılır.

```
┌────────────────────────────────────────────────────────────────────┐
│ Roller                                                         [✕] │
├────────────────────────────────────────────────────────────────────┤
│                               Sahip       Yönetici    Uzman        │
│                                                                    │
│ İlan oluşturmak               ✓           ✓           ✓            │
│ Kendi ilanlarını yönetmek     ✓           ✓           ✓            │
│ Tüm ilanları görmek           ✓           ✓           ✕            │
│ Tüm ilanları yönetmek         ✓           ✓           ✕            │
│ İlan devretmek                ✓           ✓           ✕            │
│ Şirket profilini düzenlemek   ✓           ✓           ✕            │
│ Şirket raporunu görmek        ✓           ✓           ✓            │
│ Ekibi yönetmek                ✓           ✕           ✕            │
│ Şirket hesabı                 ✓           ✕           ✕            │
│                                                                    │
│ Sahip — şirketi kaydeden kişi, tek kişidir.                        │
│ Yönetici — tüm işe alımı yönetir, ekibi yönetemez.                 │
│ Uzman — sadece kendi ilanlarıyla çalışır.                          │
└────────────────────────────────────────────────────────────────────┘
```

Ayrıntılı yetki tablosu → **Bölüm 16.**

### 15.8 [⋮] → Rolü değiştir

**Kim:** Sadece sahip.

```
┌──────────────────────────────────────────────┐
│ Mehmet Kaya'nın rolü                    i    │
│                                              │
│ ○ Yönetici   (şu an)                         │
│ ● Uzman                                      │
│                                              │
│ ! Uzman olunca:                              │
│    · Sadece kendi 5 ilanını görecek          │
│    · Diğer ilanlara erişimi kalkacak         │
│    · İlan devredemeyecek                     │
│    · Şirket profilini düzenleyemeyecek       │
│                                              │
│ [Vazgeç]                    [Değiştir]       │
└──────────────────────────────────────────────┘
```

Uyarı metni seçime göre değişir — sahip ne değişeceğini **önceden** görür.

| | |
|---|---|
| Ne zaman geçerli | **Hemen** — üye bir sonraki işleminde yeni yetkiyle devam eder |
| Kendi ilanları | Aynen kalır |
| Üyeye | 🔔 *"Rolünüz 'Uzman' olarak değiştirildi."* |
| İşlem kaydı | *"Ali Kaya, Mehmet Kaya'nın rolünü Yönetici → Uzman yaptı · 15 Eylül"* |
| Sahibin rolü | Değiştirilemez — sahiplik devri ayrı *(Bölüm 17.4)* |

### 15.9 [⋮] → İlanlarını devret

**Kim:** Sahip, yönetici.

**Neden:** Bir üyenin birden fazla ilanını birden devretmek. *(Tek ilan için kısa yol: İlan kartı → [⋮] → İlanı devret.)*

```
┌──────────────────────────────────────────────────────────────────┐
│ Zeynep Ak'ın ilanlarını devret                                   │
├──────────────────────────────────────────────────────────────────┤
│ Devredilecek ilanları seçin:                                     │
│                                                                  │
│ ☑ Muhasebe Uzmanı                     ● Yayında                  │
│   2 başvuru · 0 davet · 1 süreçte          → [Mehmet Kaya ▾]     │
│                                                                  │
│ ☐ Finans Analisti                     ‖ Duraklatıldı             │
│   1 başvuru · 0 davet · 2 süreçte                                │
│                                                                  │
│ ☑ Bordro Uzmanı                       ✓ Pozisyon doldu           │
│                                            → [Ali Kaya ▾]        │
│                                                                  │
│ [Tümünü seç]                                                     │
│                                                                  │
│ 2 ilan devredilecek.                                             │
│                                                                  │
│ [Vazgeç]                                       [Devret]          │
└──────────────────────────────────────────────────────────────────┘
```

| | |
|---|---|
| Seçim | İstediği ilanları işaretler — **hepsini devretmek zorunda değil** |
| Kime | Her ilan için **ayrı kişi** seçilebilir |
| Listede | Üyenin tüm ilanları — her durumda · durum ve içerideki iş miktarı |
| Kime devredilebilir | Ekipteki **aktif** üyeler — askıdaki / giriş yapmamış üye ve şu anki sorumlu listede yok |
| [Devret] | En az bir ilan seçili ve her seçiliye kişi atanınca aktif |
| Yeni sorumlulara | Her ilan için 📱 🔔 *"Muhasebe Uzmanı ilanı size devredildi."* |
| Adaya | **Hiçbir şey gitmez** |

### 15.10 [⋮] → Ekipten çıkar

**Kim:** Sadece sahip.

Aynı buton, **üyenin durumuna göre iki farklı pencere** açar — sistem üyenin ilan sayısına bakıp uygun olanı gösterir.

**Pencere 1 — üyenin hiç ilanı yoksa:**

```
┌──────────────────────────────────────────────┐
│ Can Demir'i ekipten çıkarmak istediğinize    │
│ emin misiniz?                                │
│                                              │
│ Can hesabına bir daha giriş yapamayacak.     │
│ Bu işlem geri alınamaz.                      │
│                                              │
│ [Vazgeç]                       [Çıkar]       │
└──────────────────────────────────────────────┘
```

**Pencere 2 — üyenin ilanı varsa:**

```
┌──────────────────────────────────────────────────────────────────┐
│ Zeynep Ak'ı ekipten çıkar                                        │
├──────────────────────────────────────────────────────────────────┤
│ Zeynep'in 4 ilanı var. Kime devredilsin? *                       │
│                                                                  │
│ ● Hepsi bana (Ali Kaya)                                          │
│ ○ Hepsi şu kişiye: [Mehmet Kaya ▾]                               │
│ ○ İlan ilan seçeceğim                                            │
│                                                                  │
│ Zeynep hesabına bir daha giriş yapamayacak.                      │
│ Bu işlem geri alınamaz.                                          │
│                                                                  │
│ [Vazgeç]                                       [Çıkar]           │
└──────────────────────────────────────────────────────────────────┘
```

**"İlan ilan seçeceğim" seçilip [Çıkar]'a basılınca:**

```
┌──────────────────────────────────────────────────────────────────┐
│ ← Geri         Zeynep Ak'ı ekipten çıkar                         │
├──────────────────────────────────────────────────────────────────┤
│ Her ilan için yeni sorumlu seçin: *                              │
│                                                                  │
│ Muhasebe Uzmanı                       ● Yayında                  │
│ 2 başvuru · 0 davet · 1 süreçte            → [Mehmet Kaya ▾]     │
│ ──────────────────────────────────────────────────────────────── │
│ Finans Analisti                       ‖ Duraklatıldı             │
│ 1 başvuru · 0 davet · 2 süreçte            → [Ali Kaya ▾]        │
│ ──────────────────────────────────────────────────────────────── │
│ Bordro Uzmanı                         ✓ Pozisyon doldu           │
│                                            → [Ali Kaya ▾]        │
│ ──────────────────────────────────────────────────────────────── │
│ Muhasebe Stajyeri                     Taslak                     │
│                                            → [ Seçin ▾ ]  !      │
│                                                                  │
│ Kalanların hepsine: [Ali Kaya ▾]  [Uygula]                       │
│                                                                  │
│ ! 1 ilan için sorumlu seçilmedi.                                 │
│                                                                  │
│ [Vazgeç]                              [Devret ve çıkar]          │
└──────────────────────────────────────────────────────────────────┘
```

| "İlanlarını devret"ten farkı | |
|---|---|
| Onay kutusu | **Yok** — her ilan devredilmek zorunda |
| Kısa yol | **"Kalanların hepsine"** — seçilmemişleri tek seferde bir kişiye |
| Buton | **[Devret ve çıkar]** — tüm ilanlara kişi atanınca aktif |

**Çıkarınca:**

| | |
|---|---|
| Hesap | Silinir — giriş yapamaz |
| İlanları | Seçilen kişilere geçer · yeni sorumlulara 📱 🔔 |
| Sohbetlerdeki mesajları | **Kalır** — adı görünür, ilanı devralan kişi konuşmaya devam eder |
| *"Davet eden: Zeynep Ak"* gibi kayıtlar | Kalır — geçmiş bozulmasın |
| Adaylar | Hiçbir şey görmez |
| Zeynep'e | ✉️ *"ABC Teknoloji ekibinden çıkarıldınız."* |
| İşlem kaydı | *"Ali Kaya, Zeynep Ak'ı ekipten çıkardı · 15 Eylül"* |
| Tekrar eklenebilir mi | ✅ Yeni üye olarak — eski ilanları geri gelmez |

| Kural | |
|---|---|
| Kim çıkarır | Sadece sahip |
| Sahip kendini | Çıkaramaz |
| Hiçbir ilan sahipsiz kalmaz | Devir seçilmeden çıkarılamaz |

---

## 16. Yetkiler — Tam Tablo

### 16.1 İlanlar

| İşlem | 👑 Sahip | 🛡 Yönetici | 👤 Uzman |
|---|:---:|:---:|:---:|
| İlan oluşturmak, taslak kaydetmek | ✅ | ✅ | ✅ |
| İlanları görmek | Tümü | Tümü | **Sadece kendi** |
| Başkasının ilanını aday gibi görmek *(şirket profilinden)* | — | — | ✅ Salt okunur |
| Yayınlamak, düzenlemek | ✅ | ✅ | ✅ kendi |
| Uzatmak, duraklatmak, devam ettirmek, tekrar yayınlamak | ✅ | ✅ | ✅ kendi |
| **Sonuç bildirmek** | ✅ | ✅ **tümü** | ✅ kendi |
| Kopyalamak | ✅ | ✅ | ✅ kendi |
| Silmek *(sonuçlanan / taslak)* | ✅ | ✅ | ✅ kendi |
| **İlanı devretmek** | ✅ | ✅ | ❌ |

### 16.2 Adaylarla işlemler

| İşlem | 👑 Sahip | 🛡 Yönetici | 👤 Uzman |
|---|:---:|:---:|:---:|
| Adayları görmek, davet etmek, "bir daha gösterme" | ✅ | ✅ | ✅ kendi ilanında |
| Başvuru kabul / red | ✅ | ✅ | ✅ kendi ilanında |
| Daveti geri çekmek | ✅ | ✅ | ✅ kendi ilanında |
| Sohbet, süreci sonuçlandırmak | ✅ | ✅ | ✅ kendi ilanında |
| Belgeleri görmek / indirmek, PDF | ✅ | ✅ | ✅ kendi ilanında |
| İletişim bilgisi istemek / görmek | ✅ | ✅ | ✅ kendi ilanında |
| İşe alım çelişkisini düzeltmek | ✅ | ✅ | ✅ kendi ilanında |
| Karşılaştırma | ✅ | ✅ | ✅ kendi ilanında |

**Başkasının ilanında işlem yapılınca:** İlanın sorumlusu **değişmez** · push bildirimleri **sorumluya** gitmeye devam eder · kim yaptığı kayıtta görünür (*"Davet eden: Ali Kaya"*, sohbette mesajın yanında yazan kişinin adı).

### 16.3 Şirket ve ekip

| İşlem | 👑 Sahip | 🛡 Yönetici | 👤 Uzman |
|---|:---:|:---:|:---:|
| Şirket profilini görmek | ✅ | ✅ | ✅ |
| Şirket profilini düzenlemek *(serbest alanlar)* | ✅ | ✅ | ❌ |
| Değişiklik talebi göndermek | ✅ | ❌ | ❌ |
| Şirket raporu | ✅ | ✅ | ✅ |
| Profildeki uyarılar | Tüm ilanlar | Tüm ilanlar | Kendi ilanları |
| Ekibi görmek | ✅ | ✅ | ✅ |
| Üye eklemek, rol değiştirmek, çıkarmak | ✅ | ❌ | ❌ |
| Şirket Yönetimi menüsü | ✅ | ❌ | ❌ |
| Belgeleri görmek | ✅ | ❌ | ❌ |
| Sahipliği devretmek | ✅ | ❌ | ❌ |
| Sahiplik talebi *(sahip ulaşılamıyorsa)* | — | ✅ | ❌ |
| Şirket hesabını silmek | ✅ | ❌ | ❌ |
| Ekipten kendi isteğiyle ayrılmak | ❌ | ✅ | ✅ |

### 16.4 Bildirimler

| | 👑 Sahip | 🛡 Yönetici | 👤 Uzman |
|---|:---:|:---:|:---:|
| Sorumlusu olduğu ilanın olayları | 📱 🔔 | 📱 🔔 | 📱 🔔 |
| Başkasının ilanının önemli olayları | 🔔 | 🔔 | — |

---
## 17. Şirket Yönetimi

### 17.1 Menü

**Sadece sahip görür** — yönetici ve uzmanın menüsünde hiç yok.

**Menü → 🔐 Şirket Yönetimi**

```
┌──────────────────────────────────────────────────────────────────┐
│ Şirket Yönetimi                                                  │
├──────────────────────────────────────────────────────────────────┤
│ Belgeler                                                     [→] │
│    Başvuruda ve değişiklik taleplerinde yüklenen belgeler        │
│                                                                  │
│ Değişiklik Taleplerim                                        [→] │
│    1 talep inceleniyor                                           │
│                                                                  │
│ Sahipliği devret                                             [→] │
│    Şirketin sahipliğini bir yöneticiye verin                     │
│                                                                  │
│ ! Şirket hesabını sil                                        [→] │
│    Şirket ve tüm verileri kalıcı olarak silinir                  │
└──────────────────────────────────────────────────────────────────┘
```

| Bölüm | Neden sadece sahip |
|---|---|
| Belgeler | Resmi, hassas belgeler |
| Değişiklik Taleplerim | Talebi zaten sadece sahip gönderebiliyor |
| Sahipliği devret | En yetkili rol el değiştiriyor |
| Şirket hesabını sil | Geri dönüşü olmayan en büyük karar |

### 17.2 📄 Belgeler

```
┌──────────────────────────────────────────────────────────────────┐
│ ← Şirket Yönetimi                Belgeler                        │
├──────────────────────────────────────────────────────────────────┤
│ Vergi levhası.pdf                                                │
│ İşveren başvurusu — 15 Eylül 2026               ✓ Onaylandı      │
│                                                  [Görüntüle]     │
│ ──────────────────────────────────────────────────────────────── │
│ Ticaret sicil gazetesi.pdf                                       │
│ İşveren başvurusu — 15 Eylül 2026               ✓ Onaylandı      │
│                                                  [Görüntüle]     │
│ ──────────────────────────────────────────────────────────────── │
│ Yeni unvan tescili.pdf                                           │
│ Değişiklik talebi (Şirket adı) — 3 Ekim         ~ İnceleniyor    │
│                                                  [Görüntüle]     │
└──────────────────────────────────────────────────────────────────┘
```

| | |
|---|---|
| İçerik | İşveren başvurusunda ve değişiklik taleplerinde yüklenen belgeler |
| Her belgede | Dosya adı · nereden geldi · tarih · durum |
| Sahip ne yapabilir | **Sadece görüntüler** — silemez, değiştiremez |
| Yeni belge | Buradan değil — ilgili **değişiklik talebi** içinden |
| Kim görür | Sahip ve admin |

**Neden silinemez:** Bu belgeler admin'in şirketi **onaylamasının dayanağı.**

**Ne kadar saklanır:**

| Belge | Süre |
|---|---|
| İşveren başvurusu belgeleri | Şirket **aktif olduğu sürece** |
| Onaylanan değişiklik talebinin belgesi | Şirket aktif olduğu sürece |
| Reddedilen değişiklik talebinin belgesi | Retten **1 yıl** sonra silinir |
| Şirket hesabı silinince | Tüm belgeler **1 yıl** daha saklanır, sonra silinir *(sahtecilik, şikâyet, yasal talep için — sadece admin erişir)* |

> ⚠️ Süreler hukuki danışmanla netleşecek. Aydınlatma metninde: *"Hesap silinse de resmi belgeler 1 yıl saklanır."*

### 17.3 📝 Değişiklik Taleplerim

```
┌───────────────────────────────────────────────────────────────────┐
│ ← Şirket Yönetimi           Değişiklik Taleplerim                 │
├───────────────────────────────────────────────────────────────────┤
│ Şirket adı                                                        │
│ ABC Teknoloji → ABC Yazılım                                       │
│ Gönderildi: 3 Ekim                              ~ İnceleniyor     │
│                                                 [Talebi geri çek] │
│ ───────────────────────────────────────────────────────────────── │
│ Çalışan sayısı                                                    │
│ 11-50 → 51-200                                                    │
│ Gönderildi: 28 Eylül                            ✓ Onaylandı       │
│                                                 2 Ekim            │
│ ───────────────────────────────────────────────────────────────── │
│ Web sitesi                                                        │
│ — → abcteknoloji.com                                              │
│ Gönderildi: 20 Eylül                            ✕ Reddedildi      │
│ Sebep: "Alan adı doğrulanamadı"                 22 Eylül          │
│                                           [Yeniden talep gönder]  │
└───────────────────────────────────────────────────────────────────┘
```

| Durum | Buton | Ne yapar |
|---|---|---|
| ⏳ İnceleniyor | **[Talebi geri çek]** | Talep iptal olur — fikrini değiştirdiyse |
| ✅ Onaylandı | — | |
| ❌ Reddedildi | **[Yeniden talep gönder]** | Talep penceresi aynı alanla açılır — sebebi düzeltip tekrar gönderir |

| Kural | |
|---|---|
| Aynı alan için | Bir seferde **tek** bekleyen talep |
| Sıra | En yeni üstte |
| Talep nasıl oluşturulur | Şirket profili → [Düzenle] → kilitli alanın yanındaki **[Değişiklik talebi]** *(Bölüm 14.9)* |

### 17.4 👑 Sahipliği devret — planlı devir

**Neden:** Sahip şirketten ayrılıyor veya işi başkasına bırakıyor.

**Nereden başlatılır:**
- **Şirket Yönetimi → 👑 Sahipliği devret**
- veya **Ekip → yöneticinin kartı → [⋮] → 👑 Sahipliği devret** *(kısa yol)*

**Adım 1 — Sahip seçer:**

```
┌──────────────────────────────────────────────────────────────────┐
│ Sahipliği devret                                                 │
├──────────────────────────────────────────────────────────────────┤
│ Yeni sahip *:  [Mehmet Kaya — Yönetici ▾]                        │
│                                                                  │
│ Siz ne olacaksınız? *                                            │
│ ● Yönetici olarak ekipte kalayım                                 │
│ ○ Ekipten ayrılayım — ilanlarım yeni sahibe devredilsin          │
│                                                                  │
│ ! Yeni sahip şunları yapabilecek:                                │
│    · Ekibi yönetmek                                              │
│    · Değişiklik talebi göndermek                                 │
│    · Şirket belgelerini görmek                                   │
│    · Şirket hesabını silmek                                      │
│                                                                  │
│ [Vazgeç]                              [Devir isteği gönder]      │
└──────────────────────────────────────────────────────────────────┘
```

| | |
|---|---|
| Kime devredilir | Sadece **aktif yönetici** — telefonu doğrulanmış. Uzmana devretmek için önce rolü yöneticiye çevrilir |
| [Devir isteği gönder] | **SMS doğrulama** → istek yeni sahibe gider |

**Adım 2 — İstek beklerken, sahibin ekranı:**

```
│ Sahipliği devret                                                 │
│    ~ Mehmet Kaya'nın kabulü bekleniyor · 5 gün kaldı             │
│                                              [İsteği geri çek]   │
```

**Adım 3 — Yeni sahip panele girince en üstte bant:**

```
┌──────────────────────────────────────────────────────────────────┐
│ Ali Kaya, ABC Teknoloji'nin sahipliğini size devretmek           │
│    istiyor.                                [İncele ve yanıtla →] │
└──────────────────────────────────────────────────────────────────┘
```

Ayrıca 📱 🔔 ✉️ bildirim.

**[İncele ve yanıtla →]:**

```
┌──────────────────────────────────────────────┐
│ Sahiplik devri — ABC Teknoloji               │
│                                              │
│ Ali Kaya şirketin sahipliğini size           │
│ devretmek istiyor.                           │
│ Ali Kaya: Yönetici olarak ekipte kalacak.    │
│                                              │
│ Sahip olarak:                                │
│ · Ekibi yöneteceksiniz                       │
│ · Değişiklik talebi gönderebileceksiniz      │
│ · Şirket belgelerini göreceksiniz            │
│ · Şirket hesabını silebileceksiniz           │
│                                              │
│ [Reddet]                  [Kabul et]         │
└──────────────────────────────────────────────┘
```

**[Kabul et]** → SMS doğrulama → devir tamamlanır.

| Sonuç | |
|---|---|
| Roller | **Anında** değişir — Mehmet 👑, Ali 🛡 *(veya ekipten çıkar, ilanları Mehmet'e geçer)* |
| Tüm ekibe | 🔔 *"ABC Teknoloji'nin yeni sahibi Mehmet Kaya."* |
| Admin | Kayıt düşer — onay gerekmez |

| Kural | |
|---|---|
| Kabul süresi | **7 gün** — kabul edilmezse istek düşer |
| [Reddet] | İstek düşer · sahibe 🔔 *"Mehmet Kaya sahiplik devrini reddetti."* |
| Güvenlik | **İki taraf da SMS** ile doğrular — ele geçirilmiş bir hesaptan şirket başkasına verilemesin |

### 17.5 Sahiplik talebi — sahip ulaşılamıyorsa

**Neden:** Sahip vefat etti, şirketten ayrılıp hesabını bıraktı, telefonuna erişimi yok — devri **kendisi yapamaz.**

**Kim başlatır:** Şirketteki **yönetici.**

**Nereden:** **Ekip → sahibin kartı → [⋮] → Sahiplik talebi** *(normalde sahibin kartında [⋮] yok; yöneticiye sadece bu tek seçenekle görünür)*

```
┌──────────────────────────────────────────────────────────────────┐
│ Sahiplik talebi — ABC Teknoloji                                  │
├──────────────────────────────────────────────────────────────────┤
│ Mevcut sahip: Ali Kaya                                           │
│                                                                  │
│ Neden? *                                                         │
│ ○ Sahip vefat etti                                               │
│ ○ Sahip şirketten ayrıldı, hesabına erişim yok                   │
│ ○ Diğer: [______________________________]                        │
│                                                                  │
│ Açıklama *:                                                      │
│ [____________________________________________________________]   │
│                                                                  │
│ Yetki belgesi *:  [Dosya ekle]                                   │
│ i Şirketi temsil yetkinizi gösteren belge — imza sirküleri,      │
│   yönetim kurulu kararı, vekaletname vb.                         │
│                                                                  │
│ [Vazgeç]                                  [Talebi gönder]        │
└──────────────────────────────────────────────────────────────────┘
```

**Akış:**

| Adım | Ne olur |
|---|---|
| 1 | Yönetici talep gönderir — **SMS doğrulama + belge** |
| 2 | **Mevcut sahibe** ✉️ 📱 *"Hesabınız için sahiplik talebi yapıldı. Siz onaylamadıysanız 14 gün içinde itiraz edin."* |
| 3 | Admin belgeyi inceler |
| 4 | **14 gün** itiraz yoksa ve belge uygunsa → admin onaylar, devir tamamlanır |
| 5 | Sahip itiraz ederse → talep durur, admin iki tarafla görüşür |

**Mevcut sahip girebiliyorsa — panelde kırmızı bant:**

```
┌──────────────────────────────────────────────────────────────────┐
│ ! Mehmet Kaya hesabınız için sahiplik talebi gönderdi.           │
│    Bu talebi siz onaylamadıysanız 14 gün içinde itiraz edin.     │
│                                                  [İtiraz et]     │
└──────────────────────────────────────────────────────────────────┘
```

**Talep beklerken — Ekip ekranında sahibin kartı:**

```
│ [AK]  Ali Kaya                                   Sahip         │
│       ~ Sahiplik talebi inceleniyor · 9 gün kaldı              │
```

| Sonuç | |
|---|---|
| Eski sahip | Ekipten **çıkarılır** · ilanları yeni sahibe geçer |
| Tüm ekibe | 🔔 *"ABC Teknoloji'nin yeni sahibi Mehmet Kaya."* |
| Şirkette hiç yönetici yoksa | Şirket yetkilisi **destek formu** ile admin'e başvurur — admin yeni sahip hesabını kendisi açar |

**Neden 14 gün bekleme:** Bir yönetici sahibi devre dışı bırakıp şirketi ele geçirmeye çalışabilir. Sahip ulaşılabiliyorsa itiraz eder; ulaşılamıyorsa süre dolar, belgeyle devir olur.

**İki yolun farkı:**

| | Planlı devir | Sahiplik talebi |
|---|---|---|
| Başlatan | Sahip | Yönetici |
| Onaylayan | Yeni sahip | **Admin** |
| Belge | Gerekmez | **Zorunlu** |
| Süre | Yeni sahip kabul edince — hemen | **14 gün** itiraz + admin incelemesi |
| Güvenlik | İki taraf SMS | SMS + belge + sahibe bildirim + admin |

### 17.6 ⚠️ Şirket hesabını sil

**Neden:** Şirket platformdan tamamen ayrılıyor.

**Basınca:**

```
┌──────────────────────────────────────────────────────────────────┐
│ ! Şirket hesabını sil — ABC Teknoloji                            │
├──────────────────────────────────────────────────────────────────┤
│ Bu işlem GERİ ALINAMAZ. Şirket hesabınız hemen ve kalıcı olarak  │
│ silinecek.                                                       │
│                                                                  │
│ İlanlar                                                          │
│    3 yayında · 2 yayında olmayan · 4 sonuçlanan · 2 taslak       │
│    Hepsi silinecek.                                              │
│                                                                  │
│ ■ Devam eden işlemler sonlandırılacak                            │
│    12 bekleyen başvuru                                           │
│    4 cevap bekleyen davet                                        │
│    6 süreçteki aday — sohbetler kapanacak                        │
│                                                                  │
│    Adaylara gidecek mesaj:                                       │
│    "ABC Teknoloji platformdan ayrıldı. Sürecin sona erdi."       │
│                                                                  │
│ Ekip                                                             │
│    4 üyenin hesabı silinecek.                                    │
│                                                                  │
│ Neden ayrılıyorsunuz? (opsiyonel)                                │
│ ○ Aradığımız adayları bulduk                                     │
│ ○ Platform beklentimizi karşılamadı                              │
│ ○ Başka bir platform kullanacağız                                │
│ ○ Şirket kapandı / faaliyet durdu                                │
│ ○ Diğer: [______________________________]                        │
│                                                                  │
│ Onaylamak için şirket adını yazın:                               │
│ [                              ]                                 │
│                                                                  │
│ [Vazgeç]                          [Hesabı kalıcı olarak sil]     │
└──────────────────────────────────────────────────────────────────┘
```

**Son adım — SMS doğrulama:**

```
┌──────────────────────────────────────────────┐
│ Güvenliğiniz için telefonunuza gelen         │
│ 6 haneli kodu girin.                         │
│ +90 532 *** ** 67                            │
│ [______]                                     │
│                                              │
│ [Vazgeç]                          [Sil]      │
└──────────────────────────────────────────────┘
```

| Güvenlik adımı | Neden |
|---|---|
| Etkilenecekler sayılarla gösterilir | Sahip neyi sildiğini tam bilsin |
| Şirket adını yazmak | Yanlışlıkla basılmasın |
| **SMS doğrulama** | Geri dönüş yok — e-posta veya şifre ele geçirilse bile şirket silinemesin |
| Ayrılma sebebi | Opsiyonel — platformu geliştirmek için |

**Silince — hemen:**

| Veri | Ne olur |
|---|---|
| Şirket profili, logo, tanıtım | 🗑 Silinir |
| Ekip üyelerinin hesapları | 🗑 Silinir · üyelere ✉️ *"ABC Teknoloji hesabı silindi."* |
| Tüm ilanlar, etiketleri, eşleşmeleri | 🗑 Silinir |
| "Bir daha gösterme" ve tüm aksiyon kayıtları | 🗑 Silinir |
| Bekleyen başvuru, davet, süreçteki adaylar | ⏹ Sonlanır · adaylara 📱 🔔 |
| Bekleyen işe alım onayları | İptal |
| Adayların "gizlenecek şirketler" listesi | Şirket düşer |
| Şirket raporu ve şirket istatistik satırları | 🗑 Silinir |
| **Sohbetler** | **Adayın tarafında kalır** — kendi geçmişi · şirket adı *"Silinmiş şirket"* |
| **Platform istatistikleri** | Değişmez — zaten isimsiz toplu sayılar *(Bölüm 22)* |
| **Belgeler** | **1 yıl** saklanır, sonra silinir — sadece admin erişir |

**Aday tarafında:**

| Nerede | Ne görür |
|---|---|
| Başvurularım / Davetlerim / Süreçteki → Sonuçlanan | ⏹ *"Şirket platformdan ayrıldı"* |
| İlana dokunursa | *"Bu ilan artık mevcut değil."* |

**Aynı şirket geri gelmek isterse:** Yeni **işveren başvurusu** yapar — admin onayından baştan geçer. Eski ilanlar, ekip, geçmiş gelmez.

**Bekleme süresi yok** — silme anında ve kalıcıdır. Bu yüzden onay adımları (şirket adı + SMS) sıkı tutuldu.

---

## 18. Profilim

**Herkeste aynı** — rol ne olursa olsun. Kişinin **kendi** hesabı; şirketle ilgili hiçbir şey yok.

**Menü → 👤 Profilim**

```
┌──────────────────────────────────────────────────────────────────┐
│ Profilim                                                         │
├──────────────────────────────────────────────────────────────────┤
│ [MK]  Mehmet Kaya                                [Fotoğraf ekle] │
│       Yönetici · ABC Teknoloji                                   │
│                                                                  │
│ Ad soyad:   Mehmet Kaya                                [Düzenle] │
│ E-posta:    mehmet@abc.com                           ×           │
│ Telefon:    +90 532 123 45 67   ✓ Doğrulandı          [Değiştir] │
│                                                                  │
│ [Ekipten ayrıl]                                                  │
└──────────────────────────────────────────────────────────────────┘
```

| Alan | Kural |
|---|---|
| Fotoğraf | Opsiyonel — yoksa baş harfler |
| Rol, şirket | Sadece gösterim — rolü sahip değiştirir |
| Ad soyad | Düzenlenebilir |
| **E-posta** | 🔒 **Değiştirilemez** — giriş adresi *(değişmesi gerekirse sahip yeni üye ekler)* |
| **Telefon** | SMS ile doğrulanır · **ilan yayınlamak için zorunlu** · başkalarına görünmez |

**Telefon:**

| Durum | Ne olur |
|---|---|
| Henüz doğrulanmamış | *"⚠️ İlan yayınlamak için telefonunuzu doğrulayın"* **[Doğrula]** |
| İlk ilanı yayınlarken doğrulanmamışsa | Yayınla akışında SMS istenir — doğrulayınca kaldığı yerden devam |
| **[Değiştir]** | Yeni numara → SMS → doğrulanınca eskisinin yerine geçer |

**[Ekipten ayrıl]:**

```
┌──────────────────────────────────────────────┐
│ ABC Teknoloji ekibinden ayrılmak             │
│ istediğinize emin misiniz?                   │
│                                              │
│ 5 ilanınız şirket sahibine (Ali Kaya)        │
│ devredilecek.                                │
│ Hesabınız silinecek.                         │
│                                              │
│ [Vazgeç]                       [Ayrıl]       │
└──────────────────────────────────────────────┘
```

| Rol | [Ekipten ayrıl] |
|---|---|
| Yönetici, uzman | ✅ İlanları otomatik **sahibe** geçer · sahibe 🔔 *"Mehmet Kaya ekipten ayrıldı, 5 ilanı size devredildi."* |
| **Sahip** | ❌ **Görünmez** — sahip ayrılamaz; ya sahipliği devreder *(17.4)* ya da şirket hesabını siler *(17.6)* |

---

## 19. Ayarlar

**Herkeste aynı** — kişiye özel.

**Menü → ⚙️ Ayarlar**

```
┌──────────────────────────────────────────────────────────────────┐
│ Ayarlar                                                          │
├──────────────────────────────────────────────────────────────────┤
│ ── Genel ─────────────────────────────────────────────────────── │
│ Dil                                                   [Türkçe ▾] │
│ Saat dilimi                                 [İstanbul (UTC+3) ▾] │
│ Görünüm                        [Sistem ▾]   Açık · Koyu · Sistem │
│                                                                  │
│ ── Bildirimler ───────────────────────────────────────────────── │
│ Telefona bildirim (push):                                        │
│   Yeni başvuru                                         [Açık  ●] │
│   Yeni mesaj                                           [Açık  ●] │
│   Davet cevapları                                      [Açık  ●] │
│   Süreç ve ilan uyarıları                              [Açık  ●] │
│   Günlük özet — yeni uygun adaylar                     [Açık  ●] │
│ E-posta bildirimleri                                   [Açık  ●] │
│ i Uygulama içi bildirimler () her zaman açıktır.                 │
│                                                                  │
│ ── Güvenlik ──────────────────────────────────────────────────── │
│ Şifre                                                 [Değiştir] │
│ Açık oturumlar           3 cihaz                         [Gör →] │
│                                       [Tüm cihazlardan çıkış]    │
│                                                                  │
│ ── Gizlilik ──────────────────────────────────────────────────── │
│ Verilerimi indir                                         [İndir] │
│                                                                  │
│ ── Yardım ve yasal ───────────────────────────────────────────── │
│ Yardım merkezi                                               [→] │
│ Bize ulaşın                                                  [→] │
│ Kullanım koşulları                                           [→] │
│ Gizlilik ve aydınlatma metni                                 [→] │
│                                                                  │
│ Sürüm 1.0.0                                                      │
│ [Çıkış yap]                                                      │
└──────────────────────────────────────────────────────────────────┘
```

| Ayar | Ne yapar |
|---|---|
| **Dil** | Uygulamanın dili — menüler, butonlar, bildirimler, e-postalar, tarih ve sayı biçimi, öneri listeleri. İlk açılışta cihazın dili |
| **Saat dilimi** | Bildirim saatleri, tarihler · ilk açılışta cihazdan |
| **Görünüm** | Açık / koyu tema · varsayılan cihazın ayarı |
| **Push bildirimleri** | Türlere göre ayrı ayrı açılıp kapatılır |
| **E-posta bildirimleri** | Push'tan ayrı |
| **🔔 Uygulama içi** | Her zaman açık — önemli bir şey kaçmasın |
| **Şifre** | Mevcut şifre + yeni şifre |
| **Açık oturumlar** | Hangi cihazlarda giriş açık · telefon kaybolursa **tüm cihazlardan çıkış** |
| **Verilerimi indir** | KVKK / GDPR — kişinin kendi verilerini isteme hakkı |

**Dil neyi değiştirmez:** İlan metni, aday profil metinleri, sohbet mesajları, şirket tanıtımı — yazıldıkları dilde kalır.

**Uygulama dili ≠ iş dili:** Uygulama dili ekranların dili; iş dili *"bu işte hangi dilde çalışılacak"* — eşleştirmeye giren ilan alanı.

---
## 20. İşverene Bildirimler

### 20.1 Kurallar

> Adaya giden bildirimlerin tamamı → `aday-paneli-final.md` Bölüm 14.3. Bu bölüm sadece işverene gidenler.

| Kural | |
|---|---|
| **📱 Push** | Sadece **ilanın sorumlusuna** — büyük şirkette herkesin telefonu çalmasın |
| **🔔 Uygulama içi** | Sorumlu + önemli olaylarda sahip ve yönetici |
| **Sessize alma** | Push türlere göre kapatılabilir *(Ayarlar)* · 🔔 her zaman açık |
| **Mesaj gruplama** | Aynı sohbetten kısa sürede çok mesaj → *"Ayşe Kaya'dan 3 yeni mesaj"* |
| **Saat** | Anlık olaylar hemen · uyarılar ve günlük özet kişinin saat diliminde sabah |
| **Dil** | Kişinin uygulama dilinde |

### 20.2 Katalog

**Başvuru**

| Olay | Sorumlu | Sahip / yönetici | Metin |
|---|:---:|:---:|---|
| Yeni başvuru | 📱 🔔 | 🔔 | *"Senior Backend Developer ilanınıza yeni başvuru: Ayşe Kaya"* |
| Aday başvurusunu geri çekti | 🔔 | — | *"Ayşe Kaya başvurusunu geri çekti."* |
| Bekleyen başvurusu olan aday hesabını sildi | 🔔 | — | *"Senior Backend Developer ilanına başvuran bir aday hesabını kapattı."* |
| Başvurular 7 gün içinde cevapsız kalacak | 🔔 | — | *"3 başvuru 7 gün içinde cevapsız kalacak."* |

**Davet**

| Olay | Sorumlu | Sahip / yönetici | Metin |
|---|:---:|:---:|---|
| Aday daveti kabul etti | 📱 🔔 | 🔔 | *"Burak Can davetinizi kabul etti. Görüşmeye başlayabilirsiniz."* |
| Aday daveti reddetti | 📱 🔔 | 🔔 | *"Burak Can davetinizi kabul etmedi."* |
| Davetin süresi doldu (10 gün) | 🔔 | — | *"Burak Can'a gönderdiğiniz davet yanıtlanmadan sona erdi."* |
| Davet edilen aday hesabını sildi | 🔔 | — | *"Davet ettiğiniz bir aday hesabını kapattı."* |

**Süreç**

| Olay | Sorumlu | Sahip / yönetici | Metin |
|---|:---:|:---:|---|
| Yeni mesaj | 📱 🔔 | 🔔 | *"Ayşe Kaya: Salı 14:00 uygun mudur?"* |
| İletişim bilgisi paylaşıldı | 📱 🔔 | — | *"Ayşe Kaya iletişim bilgilerini paylaştı."* |
| Aday süreçten çekildi | 📱 🔔 | 🔔 | *"Emre Yıldız süreçten çekildi."* |
| Süreçteki aday hesabını sildi | 📱 🔔 | 🔔 | *"Senior Backend Developer ilanındaki bir aday hesabını kapattı."* |
| Süreç 7 gün sonra bitecek (53. gün) | 📱 🔔 | — | *"Ayşe Kaya ile süreç 7 gün sonra sona erecek."* |
| Süreç 60 günde bitti | 📱 🔔 | 🔔 | *"Ayşe Kaya ile süreç 60 gün dolduğu için sona erdi."* |

**İşe alım onayı**

| Olay | Sorumlu | Sahip / yönetici | Metin |
|---|:---:|:---:|---|
| Aday işe alındığını onayladı | 🔔 | 🔔 | *"Ayşe Kaya işe alındığını onayladı. 🎉"* |
| Aday işe alınmadığını bildirdi | 📱 🔔 | 🔔 | *"Burak Can işe alınmadığını bildirdi. Kontrol eder misiniz?"* |

**İlan**

| Olay | Sorumlu | Sahip / yönetici | Metin |
|---|:---:|:---:|---|
| Uygun adaylar hazır | 🔔 | — | *"Senior Backend Developer için 124 uygun aday hazır."* |
| Yayın süresi 3 gün sonra dolacak | 🔔 | — | *"İlanınızın süresi 3 gün sonra dolacak."* **[Uzat]** |
| Yayın süresi doldu | 🔔 | 🔔 | *"İlanınızın süresi doldu. 2 bekleyen başvuru, 1 süreçteki aday var."* |
| Tekrar yayınlamak için 7 gün kaldı | 🔔 | — | *"Tekrar yayınlamak için 7 gün kaldı."* |
| Sonuç bildirmek için 7 gün kaldı | 📱 🔔 | 🔔 | *"Sonucu 7 gün içinde bildirin; bildirmezseniz raporunuzda görünecek."* |
| Sonuç bildirilmediği için kapandı | 🔔 | 🔔 | *"İlan sonuç bildirilmediği için kapatıldı."* |
| İlan size devredildi | 📱 🔔 | — | *"Senior Backend Developer ilanı size devredildi."* |
| Taslak 7 gün sonra silinecek | 🔔 | — | *"Frontend Developer taslağınız 7 gün sonra silinecek."* |
| **Admin ilanı kaldırdı** | 📱 🔔 ✉️ | 🔔 ✉️ *(sahip)* | *"Senior Backend Developer ilanınız platform kurallarına aykırı bulunduğu için kaldırıldı."* — **kapatılamaz** |

**Günlük özet**

| Olay | Sorumlu | Metin |
|---|:---:|---|
| İlanın listesinde ilk 50'ye giren yeni adaylar | 📱 | *"Senior Backend Developer ilanına uygun 6 yeni aday var."* — günde bir, sabah |

**Ekip ve şirket** *(ilana bağlı değil)*

| Olay | Kime | Kanal | Metin |
|---|---|:---:|---|
| Üye hesabı askıya alındı | Sahip | 🔔 | *"Can Demir 7 gün içinde giriş yapmadığı için hesabı askıya alındı."* |
| Rolünüz değişti | Üye | 🔔 | *"Rolünüz 'Uzman' olarak değiştirildi."* |
| Üye ekipten ayrıldı | Sahip | 🔔 | *"Mehmet Kaya ekipten ayrıldı, 5 ilanı size devredildi."* |
| Değişiklik talebi sonuçlandı | Sahip | 📱 🔔 | *"Şirket adı değişiklik talebiniz onaylandı."* / *"…reddedildi: sebep"* |
| Sahiplik devri isteği | Yeni sahip | 📱 🔔 ✉️ | *"Ali Kaya, ABC Teknoloji'nin sahipliğini size devretmek istiyor."* |
| Sahiplik talebi yapıldı | Mevcut sahip | 📱 ✉️ | *"Hesabınız için sahiplik talebi yapıldı. 14 gün içinde itiraz edebilirsiniz."* |
| Yeni sahip | Tüm ekip | 🔔 | *"ABC Teknoloji'nin yeni sahibi Mehmet Kaya."* |

---

## 21. Süreler ve Zamanlayıcılar — Tek Tablo

Hepsi **veritabanında** — gerçek veriyle ayarlanabilir. Zamanla çalışan işler → `bakim-isleri.md`.

| Ne | Süre | Uyarı | Süre dolunca |
|---|---|---|---|
| **Taslak** | Oluşturulduğu günden **30 gün** | 23. gün 🔔 | Tamamen silinir |
| **İlan yayını** | **60 gün** · uzatma 15/30/60 · kesintisiz en fazla **180 gün** | 3 gün kala 🔔 | ⏱ Süresi doldu |
| **Tekrar yayınlama hakkı** | Süre dolduktan sonra **30 gün** | 23. gün 🔔 | [Tekrar yayınla] kilitlenir |
| **Sonuç bildirme** | Süre dolduktan sonra **75 gün** *(30 + 45)* | 68. gün 📱 🔔 | ❓ Sonuç bildirilmedi |
| **Başvuru — işverenin cevabı** | **30 gün** | 23. gün 🔔 | ⌛ Yanıtlanmadan sona erdi |
| **Davet — adayın cevabı** | **10 gün** | 8. gün **adaya** 📱 🔔 *(2 gün kala)* | ⌛ Yanıtlanmadan sona erdi |
| **Süreç** | Başladığı günden **60 gün** · uzatma yok | 53. gün 📱 🔔 | ⌛ Süre doldu — sonuçlandırılmadı |
| **İşe alım onayı — adayın cevabı** | **14 gün** | — | ⚪ Aday cevap vermedi |
| **İşe alım çelişkisini düzeltme** | **30 gün** | — | Kayıt kesinleşir |
| **Sonuçlanan kartlar listede** | **90 gün** | — | Listeden kalkar — kayıt raporda kalır |
| **Yeni üye ilk giriş** | **7 gün** | 5. gün ✉️ | ⏸ Askıda |
| **Sahiplik devri — kabul** | **7 gün** | — | İstek düşer |
| **Sahiplik talebi — itiraz** | **14 gün** | — | Admin karar verir |
| **Değişiklik talebi** | Admin incelemesi | — | — |

---

## 22. Veri — Ne Zaman Ne Silinir

### 22.1 İlan sonuçlanınca

| Veri | Ne olur | Neden |
|---|---|---|
| Eşleşme satırları *(uygun aday puanları)* | 🗑 Silinir | İlan bir daha Keşfet'e çıkmayacak |
| İlanın anlam vektörü *(embedding)* | 🗑 Silinir | Eşleştirmede kullanılmayacak |
| "Bir daha gösterme" kayıtları | 🗑 Silinir | İlan tekrar yayınlanamaz |
| Başvuru, davet, süreç, sonuç kayıtları | ✅ Kalır | Aday ve işverenin Sonuçlanan listeleri · İlan özeti · işe alım onayı |
| İlanın kendisi ve etiketleri | ✅ Kalır | İlan özeti · Kopyala |

**Süresi dolan ilanda** aynı temizlik **30. günde** yapılır *(tekrar yayınlama hakkı bitince)*.

### 22.2 Zamanla

| Veri | Ne zaman silinir |
|---|---|
| Başvuru ayrıntıları — maaş beklentisi, not, paylaşılan belgeler listesi | Sonuçlandıktan **1 yıl** sonra |
| Tüm aksiyon kayıtları *(başvuru, davet, süreç, sonuç)* | İlan sonuçlandıktan **2 yıl** sonra |
| Soft delete edilmiş ilan | **2 yıl** sonra tamamen |
| Silinmiş adayların isimsiz kayıtları, sohbet geçmişi | İlan sonuçlandıktan **2 yıl** sonra *(sohbet süresi hukuki danışmanla)* |

### 22.3 Hesap silinince

#### Şirket hesabını sildi

Şirket, ilanlar, ekip, aksiyonlar ve şirket istatistik satırları **tamamen** silinir · belgeler 1 yıl saklanır · adayın tarafındaki kartlar ve sohbetler *"Silinmiş şirket"* olarak kalır *(Bölüm 17.6)*.

#### Aday hesabını sildi

Aday hesabını kendisi siler *(`aday-paneli-final.md` Bölüm 15)*. Silme **anında** olur. Veriler üç gruba ayrılır:

| Grup | Ne | Ne olur |
|---|---|---|
| **Hemen silinir** | Adayın hesap ve özgeçmiş bilgileri, fotoğrafı, belgeleri · maaş beklentisi ve notları · embedding, eşleşmeler, aksiyon kayıtları | Tamamen |
| **İsimsiz kalır, sonra silinir** | İşverenin listelerindeki başvuru, davet, süreç ve işe alım kayıtları · sohbet geçmişi | Ad yerine **"Silinmiş kullanıcı"** · listelerde 90 gün · kayıt ilan sonuçlandıktan **2 yıl** sonra silinir |
| **Kalıcı** | Şirket ve platform istatistikleri | **Değişmez** — kişisel veri içermez |

**Neden işverendeki kayıt bir anda yok olmuyor:** İşverenin kendi geçmişi (*"bu ilana kim başvurmuştu, kimi işe aldık, kiminle yazıştık"*) iz bırakmadan kaybolursa ne olduğunu anlamaz. Kayıt kalır ama **kim olduğu artık bilinmez.**

**İşveren ne görür — bütün ihtimaller:**

| # | Aday silindiğinde durum | İşveren ne görür | Bildirim |
|:---:|---|---|---|
| A1 | Aday sadece **Adaylar** listesinde | Listeden **kaybolur** | — |
| A2 | İşveren adayı **"Bir daha gösterme"** demişti | Hiçbir şey | — |
| A3 | Aday **karşılaştırma listesinde** | Listeden otomatik düşer | — |
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

**Ekranda örnekler:**

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

**Silinmiş kullanıcı kartında:**

| | |
|---|---|
| Ad, fotoğraf | **"Silinmiş kullanıcı"** · gri simge |
| Unvan | Kalır — hangi pozisyondaki aday olduğu anlaşılsın |
| Özgeçmiş, profil detayı | ❌ Açılmaz |
| Belgeler, PDF | ❌ Yok — işe alınanda bile |
| Telefon, e-posta, maaş beklentisi, not | ❌ Gösterilmez |
| Sohbet geçmişi | ✅ Salt okunur |
| Önceden indirilmiş PDF | Platform silemez, işverende kalır |

**Şirket raporuna etkisi:** Bekleyen başvurusu silinen aday **cevapsız sayılmaz** · davet ve süreç sayıları değişmez · işe alımlar değişmez. Rapor istatistik defterinden okunduğu için geriye dönük sayılar değişmez.

> ⚠️ İsimsiz kayıtların ve sohbet geçmişinin işverende kalması hukuki danışmanla netleşecek.

### 22.4 İstatistik tabloları

Raporlar ve istatistikler **canlı kayıtlardan değil**, her gece doldurulan **günlük defterlerden** okunur. Defter bir kez yazılır, **sonradan değişmez** — kaynak kayıtlar silinse bile.

> **Banka hesap özeti gibi:** Her gün ne girdi ne çıktı yazılır; bakiye (rapor) bunların toplamı. Geçmiş işlemler silinse de özet bozulmaz.

**Şirket istatistikleri** — şirket raporunun kaynağı:

```
Tarih      Şirket          İlan  Başvuru  Kabul  Red  Cevapsız  Davet  Süreç  İşe alım
1 Eylül    ABC Teknoloji     1      12      2     8      2        3      2       0
2 Eylül    ABC Teknoloji     0       9      1     7      1        0      1       1
```

| | |
|---|---|
| Her satır | O gün o şirkette **ne olduğu** |
| Rapor | Son 365 satırın toplamı → şirket profilindeki **12 aylık rapor** |
| Aday adı | **Yok** — sadece sayılar |
| Kim görür | Kullanıcılar sadece **12 aylık toplamı** görür · **admin** aylık ve günlük dökümü görür |
| Aday hesabını silerse | Rapor **değişmez** — bekleyen başvurusu *cevapsız* sayılmaz |
| Şirket hesabını silerse | Şirketin satırları da silinir |

**Platform istatistikleri** — platformun genel gidişatı:

```
Tarih         Ülke      Meslek alanı   Açılan ilan   Dolan   Başvuru
Ağustos 2026  Türkiye   Bilişim        1.240         434     52.080
Ağustos 2026  Türkiye   Lojistik         310          98      9.300
```

| | |
|---|---|
| Şirket / ilan / aday adı | **Yok** |
| Kim görür | Sadece admin / platform ekibi |
| Ne için | *"Lojistik ilanlarının sadece %30'u doluyor"* → o alanda aday çekmek, eşleştirmeyi incelemek |
| Hesap silinince | **Değişmez** |

---

## 23. Diğer Dokümanlara Etkisi

| Doküman | Durum |
|---|---|
| **`eslestirme-motoru.md`** | ✅ İşlendi — aksiyon listesi, sonuç bildirilince temizlik, istatistik tabloları, 2 yıl kuralı, aday silinince üç grup |
| **`bakim-isleri.md`** | ✅ İşlendi — taslak, ilan 30 + 45 gün, süreç 60 gün, işe alım onayı, istatistik, 2 yıl silme, aday silinince isimsiz kayıtlar, davetin 2 gün kala uyarısı |
| **`is-ilani-v3.md`, `staj-ilani.md`** | ✅ İşlendi — "Algılanan" satırı, çalışma tipi çoklu, kilitler, ilan durumları |
| **`aday-paneli-final.md`** | ✅ Yazıldı — aday tarafının tüm ekranları, aday bildirim kataloğu, hesap silme |
| **`aday-profili-final.md`** | Güncellenecek — sekme adı "Özgeçmiş", hesap bilgileri Profilim'e |
| **Sohbet dokümanı** *(yazılacak)* | Mesajlaşma · iletişim bilgisi isteme / paylaşma · süreci sohbetten sonuçlandırma · sonuçlanınca salt okunur · devirde sohbetin devamı · silinmiş kullanıcı ve silinmiş şirket gösterimi |
| **Admin paneli** *(yazılacak)* | Şirket onayı · değişiklik talepleri · sahiplik talepleri · tartışmalı işe alımlar · **bildirilen ilanlar ve ilan kaldırma** · PDF indirme kayıtları · şirket aylık / günlük dökümü · platform istatistikleri · hayalet ilan sinyalleri |
| **Veri koruma** *(yazılacak)* | Saklama süreleri · belgelerin 1 yıl saklanması · PDF indirme bilgisi · silinen adayın isimsiz kayıtları · aydınlatma metni |

## 24. Açık Konular

| # | Konu | Ne zaman |
|:---:|---|---|
| 1 | Saklama süreleri (belgeler 1 yıl, ayrıntılar 1 yıl, aksiyonlar 2 yıl) | Hukuki danışmanla |
| 2 | Değişiklik talebinde admin onay süresi — sahibe söz verilecek mi | Admin paneli konuşulurken |
| 3 | Ekip üyesi sayısı sınırı | Gelir modeliyle |
| 4 | Şirket raporu eşiği (20 başvuru) ve süre (12 ay) | İlk gerçek veriyle |
| 5 | Excel'e aktarma | Talep gelirse |
| 6 | İK / işe alım ajansları — müşteri adına ilan | Ayrı konu |
| 7 | Silinen adayın isimsiz kayıtları ve **sohbet geçmişinin** işverende kalması — "silinme hakkı" | Hukuki danışmanla |
| 8 | Admin'in kaldırdığı ilanın işverende görünümü, itiraz yolu, rapora etkisi | Admin paneli konuşulurken |
