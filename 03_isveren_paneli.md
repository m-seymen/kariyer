# İş Eşleştirme Projesi — İşveren Paneli

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak
> **Sürüm 2** — ilan konumu şirket lokasyonlarından ayrıldı
> Durum: Ekip toplantısı sonrası nihai karar
> İlgili dokümanlar: `auth-kararlari.md`, `isveren-basvuru.md`, `sirket-profili-v3.md`, `eslestirme-motoru.md`
> Kapsam dışı: Veritabanı tabloları → ayrı dokümanda

---

## 0. Özet

İşveren paneli, onaylanmış bir şirketin **tek bir şirket profili altında birden fazla kişiyle** çalıştığı alan.

```
ABC Teknoloji (tek şirket profili)
├── Ayşe   — Sahip       (başvuruyu yapan, her şeyi yönetir)
├── Mehmet — Yönetici    (şirket profili + tüm ilanlar)
├── Can    — Uzman       (sadece kendi ilanları)
└── Zeynep — Uzman       (sadece kendi ilanları)
```

**Dört temel kural:**

1. **Bir şirket = bir profil.** Kaç İK çalışanı olursa olsun şirket profili tektir.
2. **Ekibi sadece sahip yönetir.** Ekleme, rol değiştirme, pasife alma — hepsi sahipte.
3. **Uzman sadece kendi ilanlarını görür.** Birbirinin ilanını, başvurusunu, sohbetini göremez.
4. **Şirketin lokasyonu ile ilanın konumu ayrı şeylerdir.** İlan kendi konumunu tutar.

### Bu sürümde değişenler

| Konu | Önceki | Şimdi |
|---|---|---|
| İlan konumu | Şirket lokasyon listesinden seçiliyordu, ona bağlıydı | **İlanın kendi konumu** — konum ağacından serbest seçim; şirket lokasyonları sadece kısayol |
| Ek lokasyonlar | İlanı olan lokasyon silinemez / konumu değiştirilemezdi | **Serbest** — ilan kontrolü yok |
| Yeni senaryo | — | **İK / işe alım ajansları** müşterileri adına ilan açabilir *(detaylar açık — Bölüm 11)* |

---

## 1. Neden Tek Şirket Profili

Büyük bir şirkette bilişim için bir İK uzmanı, usta alımı için başka bir uzman çalışıyor olabilir. **Her biri ayrı şirket hesabı açsaydı:**

| Sorun | Sonuç |
|---|---|
| **Gizlenen şirketler bozulur** | Aday "ABC Teknoloji"yi gizledi. ABC'nin üç ayrı hesabı var — biri gizlenir, diğer ikisi adayı görmeye devam eder. **En hassas özellik çalışmaz** |
| Doğrulama tekrarı | Aynı belgeler üç kez yüklenir, admin aynı şirketi üç kez onaylar |
| Aday kafası karışır | Aynı şirketin üç farklı profili, üç farklı tanıtım |
| Bilgi dağılır | Logo, tanıtım, adres her hesapta farklı olabilir |

**Karar:** Tek şirket profili, sahibin eklediği ekip üyeleri.

---

## 2. Panel Yapısı

### Menü

```
🏢 Şirket Profili     Kimlik · Konum · Tanıtım · İmkanlar
👤 Profilim           Kişinin kendi bilgileri
👥 Ekip               Ekip listesi
📋 İlanlar            İlanlar ve ilan içi sekmeler
🔐 Şirket Yönetimi    Belgeler · Değişiklik talepleri · Hesabı kapat
```

### Üst bar — her sayfada sabit

```
🔔 12    💬 5    [Ayşe Yılmaz ▾]
                  ├── Profilim
                  ├── Bize yazın
                  └── Çıkış yap
```

- **🔔** Bildirim sayısı (başvuru, davet kabulü, ekip olayları)
- **💬** Okunmamış mesaj sayısı
- **Bize yazın** — destek

### Yetki matrisi

| Sayfa | Sahip | Yönetici | Uzman |
|---|:---:|:---:|:---:|
| **Şirket Profili** | ✅ Düzenler | ✅ Düzenler | 👁 Görür |
| **Profilim** | ✅ Kendi | ✅ Kendi | ✅ Kendi |
| **Ekip** | ✅ **Yönetir** | 👁 Görür | 👁 Görür |
| **İlanlar** | ✅ Tümü | ✅ Tümü | ✅ **Sadece kendi** |
| **Şirket Yönetimi** | ✅ | ❌ Görmez | ❌ Görmez |

### Rollerin özeti

| Rol | Kim | Ne yapar |
|---|---|---|
| **Sahip** | Başvuruyu yapan kişi — her şirkette **tam olarak bir** sahip | Her şey: ekip, belgeler, değişiklik talepleri, hesabı kapatma, tüm ilanlar |
| **Yönetici** | Sahibin eklediği | Şirket profilini düzenler, tüm ilanları görür ve yönetir, ilanları uzmanlar arasında devreder. **Ekibi yönetemez** |
| **Uzman** | Sahibin eklediği | Kendi ilanlarını açar ve yönetir. Şirket profilini ve ekibi sadece görür |

> **Yetkiler RLS ile veritabanı seviyesinde.** Arayüzde butonu gizlemek yetmez — uygulamayı inceleyen biri API'yi doğrudan çağırabilir. Her istek, kişinin rolü veritabanından kontrol edilerek karşılanır.

---

## 3. Şirket Profili

Tek sayfa, dört bölüm:

```
🏢 Şirket Profili

[ Kimlik ] [ Konum ] [ Tanıtım ] [ İmkanlar ]
```

> Bilgilerin çoğu **işveren başvurusundan onaylı** gelir (bkz. `isveren-basvuru.md`). İşverenin onaydan sonra doldurması gereken tek zorunlu alan **tanıtım**.

### 3.1 Kimlik

```
Logo:             [AT]  ← varsayılan avatar        [Değiştir]

Şirket adı:       ABC Teknoloji                     🔒 [Değişiklik talebi]
Sektör:           Bilişim › Yazılım Geliştirme       🔒 [Değişiklik talebi]
Kuruluş yılı:     2015                               🔒 [Değişiklik talebi]
Çalışan sayısı:   51-200                             🔒 [Değişiklik talebi]
Web sitesi:       abc.com.tr                         🔒 [Değişiklik talebi]
Şirket telefonu:  +90 216 555 00 00  ✅              🔒 [Değişiklik talebi]
Şirket e-postası: info@abc.com.tr                    🔒 [Değişiklik talebi]

Sosyal medya:     LinkedIn: abcteknoloji             [Düzenle]
                  [+ Ekle]
```

| Alan | Kaynak | Düzenleme | Adaya görünür |
|---|---|---|:---:|
| Logo | Varsayılan avatar | ✅ **Serbest** | ✅ |
| Şirket adı | Başvuru | 🔒 Talep | ✅ |
| Üst / alt sektör | Başvuru | 🔒 Talep | ✅ |
| Kuruluş yılı | Başvuru | 🔒 Talep | ✅ |
| Çalışan sayısı | Başvuru | 🔒 Talep | ✅ |
| Web sitesi | Başvuru *(varsa)* | 🔒 Talep | ✅ |
| Şirket telefonu | Başvuru — admin onaylı | 🔒 Talep | ✅ |
| Şirket e-postası | Başvuru | 🔒 Talep | ❌ |
| Sosyal medya | Başvuru *(varsa)* | ✅ **Serbest** | ✅ |

**Serbest olan sadece iki alan:** logo ve sosyal medya.

> **Neden bu kadar çok alan kilitli:** Bu bilgiler admin tarafından belgeyle doğrulandı. Serbest bırakılsa onaylı bir şirket kimliği sonradan başka bir şeye çevrilebilir — doğrulamanın anlamı kalmaz. Tek bir kilit kuralı ve tek bir talep akışı, hem geliştirmeyi hem admin işini sade tutuyor.

> **Web sitesi başvuruda boşsa:** Sonradan eklemek de değişiklik talebiyle olur — diğer kilitli alanlarla aynı.

**Adaya hiç gösterilmeyenler** *(sadece admin'de)*: resmi unvan, tescil/vergi numarası, tescil makamı.

**Logo:** Şirket adının baş harflerinden renkli avatar varsayılan olarak atanır. Kendi logosunu yükleyebilir, değiştirebilir.

**Sosyal medya:** Tam bağlantı yerine kullanıcı adı girilir (`@abcteknoloji`), platform adresi sistem tarafından eklenir. Platformlar: LinkedIn, Instagram, X, Facebook, YouTube *(liste veritabanında — bölgeye göre genişletilebilir)*.

### 3.2 Konum

```
📍 Genel Merkez   🔒 [Değişiklik talebi]
   Kadıköy, İstanbul, Türkiye
   Caferağa Mah. Moda Cad. No: 12

📍 Ankara Ofisi                Çankaya, Ankara        [Düzenle] [Sil]
📍 Fabrika                     Gebze, Kocaeli         [Düzenle] [Sil]

[+ Lokasyon ekle]
```

**Merkez lokasyon**

| | |
|---|---|
| Kaynak | Başvuru formu |
| İçerik | Konum (ağaçtan) + açık adres — ikisi de zorunluydu |
| Düzenleme | 🔒 Değişiklik talebi |
| Adaya | Sadece şehir/ilçe; **açık adres eşleşmeden sonra** |

**Ek lokasyonlar**

| | |
|---|---|
| Kim ekler / düzenler / siler | Sahip ve yönetici |
| İçerik | Lokasyon adı + **sadece konum ağacı** (açık adres yok) |
| Admin onayı | Gerekmez |
| İlan kontrolü | **Yok** — istediği gibi ekler, düzenler, siler |

> **Neden serbest:** İlanlar kendi konumunu tutuyor (Bölüm 6). Ek lokasyon silinse veya konumu değişse hiçbir ilan etkilenmez. Şirket lokasyonları, şirketin nerede olduğunu adaya göstermek ve ilan açarken **hızlı seçim kısayolu** olmak için var.

### Şirket lokasyonu ≠ ilan konumu

| | Şirket lokasyonu | İlan konumu |
|---|---|---|
| Ne anlatır | Şirketin ofisleri nerede | **İşin nerede yapılacağı** |
| Nerede tutulur | Şirket profilinde | **İlanın kendi kaydında** |
| Eşleştirmeye girer mi | ❌ | ✅ Katı filtre |
| Birbirine bağlı mı | **Hayır** | **Hayır** |

> **Neden ayrı:** Bir şirketin işi, ofislerinden farklı bir yerde olabilir — saha işi, müşteri lokasyonu, şantiye. Özellikle **İK / işe alım ajansları** müşterileri adına ilan açıyor: ajans İstanbul'da, iş Gebze'deki müşteri deposunda. İlan konumu şirket lokasyonuna bağlı olsaydı ajans her müşteri adresini kendi profiline "ek lokasyon" olarak eklemek zorunda kalırdı.

### 3.3 Tanıtım

```
Şirketinizi tanıtın
┌────────────────────────────────────┐
│                                    │
└────────────────────────────────────┘
                              0 / 2.000
```

| | |
|---|---|
| Biçim | Tek serbest alan — soru, örnek, öneri yok |
| Zorunlu | ✅ — **ilan yayınlamak için tek zorunlu eksik** |
| Sınır | En fazla 2.000 karakter, minimum yok |
| Düzenleme | Sahip ve yönetici, **serbest** |

### 3.4 İmkanlar

```
Çalışanlarınıza sunduğunuz imkanlar
┌────────────────────────────────────┐
│                                    │
└────────────────────────────────────┘
                              0 / 1.000
```

| | |
|---|---|
| Biçim | Tek serbest alan, öneri yok |
| Zorunlu | ❌ Opsiyonel |
| Sınır | En fazla 1.000 karakter |
| Düzenleme | Sahip ve yönetici, **serbest** |

> Tanıtım ve imkanlar **eşleştirmeye girmez** — sadece adaya gösterim. Hangi dilde yazılırsa o dilde gösterilir.

### 3.5 Sistem tarafından gösterilen bilgiler

| Bilgi | Adaya görünür |
|---|:---:|
| **✓ Doğrulanmış şirket** *(tüm şirketlerde)* | ✅ |
| Açık ilan sayısı | ✅ |
| Üyelik tarihi | ✅ |

> Sistemde onaysız işveren olmadığı için her şirket kartında aynı ibare. Aday karşılama ekranında: *"Platformdaki tüm şirketler belgeleriyle doğrulanmıştır."*

---

## 4. Profilim

**Giriş yapan kişiye ait.** Şirket profilinden tamamen ayrı. Sahip, yönetici, uzman — herkes kendi profilini görür ve düzenler. **Adaya görünmez.**

```
👤 Profilim

Ad:              [Mehmet      ]
Soyad:           [Kaya        ]
Görev:           [İK Uzmanı   ]
Rol:             Uzman                    ← sadece görüntüleme

Giriş e-postası: mehmet@abc.com.tr       🔒 Sahip tarafından değiştirilir

Cep telefonu:    [+90 532 123 45 67]     ✅ Doğrulandı   [Değiştir]

Şifre:           ••••••••                [Şifre değiştir]

Bildirimler:     ☑ Push bildirimleri (sorumlu olduğum ilanlar)
Arayüz dili:     [Türkçe ▾]
Saat dilimi:     [İstanbul (UTC+3) ▾]
```

| Alan | Düzenleme |
|---|---|
| Ad, soyad, görev | ✅ Kendisi |
| Rol | 👁 Sadece görür — sahip değiştirir |
| **Giriş e-postası** | 🔒 **Kendisi değiştiremez — sahip Ekip'ten değiştirir** |
| Cep telefonu | ✅ Kendisi — değişince yeniden SMS doğrulama |
| Şifre | ✅ Kendisi |
| Bildirim tercihleri, dil, saat dilimi | ✅ Kendisi |

> **Giriş e-postası neden kişide değil:** Hesabı sahip açtı; giriş e-postası şirketin kontrolünde kalmalı. Çalışan ayrılırken e-postayı kendi kişisel adresine çevirip erişimi sürdüremesin.

### Cep telefonu doğrulama

```
[ +90 532 123 45 67 ]  [Doğrula]
  → SMS ile 6 haneli kod → ✅ Doğrulandı
```

- **İlan yayınlamak için zorunlu**
- Sahip için başvuru formunda yapıldı → doğrulanmış gelir
- Yönetici ve uzman ilk girişte yapar
- **Numara değişirse doğrulama anında kalkar** → yeniden doğrular. Doğrulayana kadar yeni ilan yayınlayamaz; yayındaki ilanlar yayında kalır
- İşveren tarafında aynı numara birden fazla kullanıcıda olabilir

> Telefon doğrulamanın tüm kuralları → `auth-kararlari.md`

---

## 5. Ekip

### Kim ne görür

| | Sahip | Yönetici | Uzman |
|---|:---:|:---:|:---:|
| Ekip listesini görür | ✅ | ✅ | ✅ |
| Yetkili ekler | ✅ | ❌ | ❌ |
| Rol değiştirir | ✅ | ❌ | ❌ |
| Giriş e-postasını değiştirir | ✅ | ❌ | ❌ |
| Pasife alır / siler | ✅ | ❌ | ❌ |
| Toplu ilan devreder | ✅ | ❌ | ❌ |

> **Ekibi sadece sahip yönetir.** Yönetici ve uzman listeyi görür ama değiştiremez.

### Ekip sayfası

**Sahibin gördüğü:**
```
👥 Ekip                                           [+ Yetkili ekle]

Ayşe Yılmaz    Sahip      ✅ Telefon   Son giriş: 2 saat önce    4 ilan
Mehmet Kaya    Yönetici   ✅ Telefon   Son giriş: 12 gün önce    6 ilan   [⋯]
Can Demir      Uzman      ✅ Telefon   Son giriş: dün            3 ilan   [⋯]
Zeynep Ak      Uzman      ⏳ Henüz giriş yapmadı                          [⋯]
```

**Yönetici ve uzmanın gördüğü:** Aynı liste — **[+ Yetkili ekle]** ve **[⋯]** olmadan.

> **"Son giriş"** sahibe önemli bir sinyal veriyor: 30 gündür girmeyen bir uzmanın ilanları cevapsız kalıyor olabilir — devretme ihtiyacı.
> **Telefon durumu** "Can neden ilan açamıyor" sorusunun cevabını gösteriyor.

**[⋯] menüsü — sadece sahip:**
- Rolü değiştir *(yönetici ↔ uzman)*
- Giriş e-postasını değiştir
- İlanlarını devret
- Pasife al
- Daveti yeniden gönder *(henüz giriş yapmamışlar için)*
- Sil *(henüz giriş yapmamışlar için)*

### Yetkili ekleme

```
[+ Yetkili ekle]

Ad:       [__________]
Soyad:    [__________]
E-posta:  [__________]     ← herhangi bir adres olabilir
Rol:      ○ Yönetici  ● Uzman

[Ekle]
```

**E-posta başka bir hesapta kayıtlıysa:**
> *"Bu e-posta başka bir hesapta kullanılıyor. Farklı bir e-posta girin."*

*(Bir hesap = bir rol. Aynı e-posta hem aday hem işveren olamaz.)*

### Arka planda ne olur

```
Sahip [Ekle]'ye basar
        ↓
Edge Function (sunucu tarafı)
  1. İsteği yapan bu şirketin sahibi mi?          → veritabanından kontrol
  2. E-posta başka bir hesapta var mı?            → varsa hata
  3. Güçlü rastgele şifre üretilir
  4. Supabase Auth'ta kullanıcı oluşturulur       → e-posta doğrulanmış olarak
  5. Kullanıcı bu şirkete, seçilen rolle bağlanır
  6. Davet maili gönderilir
        ↓
Kişi giriş yapar → sistem şirketini ve rolünü bilir
  → o şirket adına, rolünün izin verdiği kadar işlem yapar
```

**Kullanılan Supabase parçaları:**

| İş | Supabase karşılığı |
|---|---|
| Kullanıcı oluşturma | **Auth Admin API** — sadece sunucuda, gizli anahtarla |
| İş mantığı | **Edge Function** |
| Şirket – kullanıcı – rol ilişkisi | **Postgres tablosu** |
| Kim neyi görür | **RLS** |
| Mail | Edge Function içinden e-posta servisi (Resend, Postmark, SES vb.) |

> ⚠️ **Bu işlem asla uygulamadan (Flutter / Next.js) yapılmaz.** Kullanıcı oluşturma yetkisi veren gizli anahtar istemciye konursa, uygulamayı inceleyen biri herkes adına hesap açabilir.

> **Rol değişikliği anında geçerli:** Sahip birini uzmandan yöneticiye çıkardığında tablodaki tek satır değişir; kişinin yetkileri hemen güncellenir, tekrar giriş yapması gerekmez.

> **Aynı mekanizma:** Admin bir başvuruyu onayladığında sahibin hesabı da bu akışla açılıyor — tek fark, tetikleyen kişi sahip yerine admin.

### Davet maili

```
Konu: ABC Teknoloji sizi ekibine davet etti

Ayşe Yılmaz sizi ABC Teknoloji'ye "Uzman" olarak ekledi.

Giriş bilgileriniz:
E-posta: mehmet@abc.com.tr
Şifre:   R4n!wZ8kLp2q

[İşveren girişine git]

Güvenliğiniz için ilk girişte şifrenizi değiştirmenizi öneririz.
```

| Güvenlik önlemi | |
|---|---|
| Güçlü rastgele şifre (12+ karakter) | Tahmin edilemez |
| Sistemde açık halde saklanmaz | Mail gidince sadece özeti kalır; sahip de göremez |
| **7 gün giriş yapılmazsa hesap askıya alınır** | Sahip "Daveti yeniden gönder" der, yeni şifre üretilir |
| İlk girişte yumuşak hatırlatma | *"Şifrenizi değiştirmek ister misiniz?"* **[Değiştir] [Şimdi değil]** |

### İlk giriş

```
ABC Teknoloji'ye hoş geldiniz.
İlan yayınlayabilmek için cep telefonunuzu doğrulayın.

[+90 ___ ___ __ __]  [Doğrula]
```

Atlayabilir ama ilan yayınlayamaz.

### Silme ve pasife alma

| Ekip üyesi durumu | Sahibin yapabileceği |
|---|---|
| **Henüz giriş yapmamış** | **Silinebilir** — hiçbir işlem yapmadı, iz bırakmadı |
| **Aktif (giriş yapmış)** | **Silinmez, pasife alınır** — önce ilanları devredilir |

> **Aktif kişi neden silinmez:** Açtığı ilanlar, değerlendirdiği başvurular, yazdığı mesajlar var. Silinirse geçmiş kayıtlarda "bu işlemi kim yaptı" bilgisi kaybolur ve adayların sohbetleri sahipsiz kalır. Pasif kişi giriş yapamaz, ama adı geçmişte kalır.

**Pasife alma akışı:**
```
[⋯] → Pasife al
  → Üstünde aktif ilan veya sohbet var mı?
       Evet → "Önce ilanlarını devredin" → devretme ekranı
       Hayır → pasife alınır
```

### Güvenlik kuralları

| Kural | Neden |
|---|---|
| **Sahip pasife alınamaz** | Şirket sahipsiz kalmasın. Sahip ayrılacaksa önce sahipliği bir yöneticiye devreder |
| **Aktif ilanı olan devretmeden pasife alınamaz** | Hiçbir aday cevapsız kalmasın |
| **Silme sadece hiç giriş yapmamışlar için** | Geçmiş kayıtların bütünlüğü |

---

## 6. İlanlar

### Kim neyi görür

| | Sahip | Yönetici | Uzman |
|---|:---:|:---:|:---:|
| Şirketin tüm ilanları | ✅ | ✅ | ❌ |
| Kendi sorumlu olduğu ilanlar | ✅ | ✅ | ✅ |
| İlan açma | ✅ | ✅ | ✅ |
| İlan düzenleme | ✅ Tümü | ✅ Tümü | ✅ Kendi |
| **Tek ilan devretme** | ✅ | ✅ | ❌ |

> **Uzmanlar birbirinin işini görmez.** Can, Zeynep'in ilanlarını, başvurularını, davetlerini, sohbetlerini göremez ve değiştiremez.

> **Eşleştirme motoru bu ayrımı zaten destekliyor:** Aksiyonlar ilan bazlı. Can bir adayı "Backend" ilanında geçti diye, aynı aday Zeynep'in "Usta" ilanında görünmeye devam eder.

### İlan listesi

```
📋 İlanlarım                                        [+ İş ilanı aç] [+ Staj ilanı aç]

┌──────────────────────────────────────────────────────────────┐
│ Senior Backend Developer                    🟢 Yayında       │
│ Mehmet Kaya · İstanbul · 12 gün kaldı                        │
│                                                              │
│ 👁 340 görüntülenme                                          │
│ 📨 8 başvuru   → 4 kabul · 1 red · 3 bekleyen                │
│ ✉️ 10 davet    → 3 kabul · 2 red · 5 bekleyen                │
│ 🤝 7 eşleşme                                                 │
└──────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│ Kaynakçı Usta                               🟢 Yayında       │
│ Can Demir · Gebze · 25 gün kaldı                             │
│ ...                                                          │
└──────────────────────────────────────────────────────────────┘
```

> Sahip ve yönetici tüm ilanları, uzman sadece kendi ilanlarını görür. Sahip/yönetici listesinde her kartta **sorumlu kişinin adı** yazar.

### İlan kartındaki sayılar

| Sayı | Anlamı |
|---|---|
| **Görüntülenme** | Kaç **farklı** aday ilanın detayını açtı |
| **Başvuru** | Toplam başvuru |
| → kabul / red / bekleyen | Başvuruların durumu |
| **Davet** | Gönderilen toplam davet |
| → kabul / red / bekleyen | Davetlerin durumu |
| **Eşleşme** | Kabul edilen başvuru + kabul edilen davet |
| **Kalan gün** | Yayın süresinin bitmesine kalan |

**Görüntülenme kuralları:**

| | |
|---|---|
| Ne sayılır | Aday ilanın **detayını açtı** — Keşfet listesinde görünmesi (gösterim) sayılmaz |
| Tekillik | Aynı aday 5 kez açsa **bir** kez sayılır |
| Gizlilik | **Kimin** görüntülediği gösterilmez — sadece sayı |

> **Gösterim neden sayılmıyor:** Keşfet listesinde görünmek yüksek ama anlamsız bir sayı — işvereni yanıltır.
>
> **Kim görüntüledi neden gizli:** Aday başvurmadıysa ilgisini gizli tutmak onun hakkı.

**Bekleyen neden önemli:**

> "Bekleyen" = **henüz kabul veya red kararı verilmemiş.** Bakılmış ama karar verilmemiş başvuru, aday için bakılmamışla aynı — cevap alamıyor. Kartta en çok dikkat çekmesi gereken sayı bu.
>
> *"Yeni" (henüz açılmamış) sayısı kartta değil, sadece 🔔 bildirim sayacında.*

**Eşleşme hesabı:**

```
Davet gönderildi:   10  → kabul: 3  ─┐
                                      ├─→  Eşleşme: 7
Başvuru geldi:       8  → kabul: 4  ─┘
```

> Teknik not: Eşleşme, iki sayının toplamı olarak değil **o ilandaki tekil eşleşmiş aday sayısı** olarak hesaplanır. Normalde bir aday aynı ilana hem davet alıp hem başvuramaz, ama kenar durumlarda aynı kişi iki kez sayılmaz.

### İlan içi sekmeler

Bir ilana tıklanınca:

```
Senior Backend Developer
Sorumlu: Mehmet Kaya   [Değiştir]      ← sahip ve yönetici görür

[ Adaylar ] [ Başvuranlar ] [ Davet Ettiklerim ] [ Eşleşmeler ]
```

| Sekme | İçerik |
|---|---|
| **Adaylar** | Eşleştirme motorunun bu ilana uygun bulduğu adaylar — skora göre sıralı, filtre opsiyonel |
| **Başvuranlar** | Adayın kendi başvurduğu kişiler — karar sırası işverende |
| **Davet Ettiklerim** | İşverenin davet ettiği, cevap beklenen kişiler |
| **Eşleşmeler** | İki tarafın da onayladığı — **sohbet burada** |

> Bu sekmelerin detayları (kart içeriği, davet akışı, başvuru değerlendirme, sohbet) → **Keşfet ve Eşleşme** dokümanında.

### Tek ilan devretme

```
Sorumlu: Mehmet Kaya   [Değiştir]
            ↓
"Bu ilanı kime devretmek istiyorsunuz?"
○ Can Demir
○ Zeynep Ak
[Devret]
```

| | |
|---|---|
| Kim yapabilir | Sahip ve yönetici |
| Ne geçer | İlan + bekleyen başvurular + davetler + **aktif sohbetler** — birlikte |
| Ne zaman | İş yükü dağıtımı, izin, uzmanlık alanı değişikliği |

**Adaya sohbette sistem mesajı:**
```
─────────────────────────────────
ℹ️ Bu görüşme artık Can Demir
   tarafından yürütülüyor.
─────────────────────────────────
```

> Eski mesajlar olduğu gibi kalır. Aday birden farklı bir isimle muhatap olunca şaşırmaz. Teknik olarak mesajlar tablosuna tek satır eklemek — sisteme yük getirmez. Aynı yapı ileride başka bilgilendirmeler için de kullanılır (*"İlan kapandı"*, *"Eşleşme oluştu"*).

**Toplu devretme** (çalışan ayrılırken tüm ilanları) **sadece sahip** yapar — Ekip sayfasından.

### İlan konumu

İlan oluştururken iş yerinin konumu **ilana özel** seçilir:

```
İş yeri konumu:  [Gebze, Kocaeli              ]   ← konum ağacından serbest arama

Hızlı seçim:     [Genel Merkez] [Ankara Ofisi] [Fabrika]
                  ↑ şirket lokasyonları kısayol olarak
```

| | |
|---|---|
| Serbest seçim | Konum ağacından herhangi bir yer — şirket lokasyonu olması gerekmez |
| Hızlı seçim | Şirket lokasyonuna tıklanırsa konumu ilana **kopyalanır**, bağlanmaz |
| Sonradan | Şirket lokasyonu silinse / değişse ilan **etkilenmez** |
| Eşleştirme | Katı filtrede **ilanın konumu** kullanılır |

> Ayrıntılar (konum ağacı, uzaktan çalışma istisnası, filtre kuralı) → `ilan-olusturma.md` ve `eslestirme-motoru.md`

### İlan yayınlama şartları

```
[Yayınla]
  → Şirket profilinde tanıtım dolu mu?     Hayır → tanıtım alanına yönlendir
  → Cep telefonum doğrulanmış mı?           Hayır → SMS doğrulama
  → İkisi de evet → ilan yayına girer
```

> Şirket zaten onaylı. Tanıtım bir kez yazılınca bu kontrol bir daha takılmaz; sonraki ilanlarda tek kontrol kişisel telefon.

---

## 7. Şirket Yönetimi

**Sadece sahip görür.** Menüde yönetici ve uzman için hiç görünmez.

```
🔐 Şirket Yönetimi

📄 Belgeler
   Vergi levhası.pdf           Başvuru — 15 Eylül 2026      ✅ Onaylandı
   Ticaret sicil gazetesi.pdf  Başvuru — 15 Eylül 2026      ✅ Onaylandı
   Yeni unvan tescili.pdf      Değişiklik talebi — 3 Ekim   ⏳ İnceleniyor

📝 Değişiklik Taleplerim
   Şirket adı       ABC Teknoloji → ABC Yazılım      ⏳ İnceleniyor
   Çalışan sayısı   11-50 → 51-200                   ✅ Onaylandı — 2 Ekim
   Web sitesi       — → abc.com.tr                   ❌ Reddedildi — "Alan adı doğrulanamadı"

⚠️ Şirket Hesabını Kapat
   [Hesabı kapat]
```

### Belgeler

- Başvuruda ve değişiklik taleplerinde yüklenen tüm belgeler
- Her belgenin kaynağı, tarihi, durumu
- Onaydan 1 yıl sonra dosya silinir, kayıt satırı kalır *(süre ülkenin veri koruma rejimine göre değişebilir)*

### Değişiklik talepleri

Şirket profilindeki kilitli alanların yanındaki **[Değişiklik talebi]** butonundan oluşturulur:

```
┌──────────────────────────────────────┐
│ Değişiklik talebi — Şirket adı       │
│                                      │
│ Mevcut:  ABC Teknoloji               │
│ Yeni:    [_____________________]     │
│ Sebep:   [_____________________]     │
│ Belge:   [Dosya ekle]  (opsiyonel)   │
│                                      │
│ [Talebi gönder]                      │
└──────────────────────────────────────┘
```

| | |
|---|---|
| Kim gönderir | Sadece sahip |
| Kapsam | Şirket adı, sektör, kuruluş yılı, çalışan sayısı, web sitesi, şirket telefonu, şirket e-postası, merkez lokasyon |
| Akış | Admin kuyruğuna düşer → onay/ret → sahibe bildirim |
| Beklerken | **Eski değer geçerli kalır** — alanın yanında *"⏳ Değişiklik talebi inceleniyor"* |
| Belge | Opsiyonel ama faydalı — şirket adı veya adres değişikliğinde admin büyük ihtimalle yeni tescil belgesi isteyecek |

> **Admin için not:** Çalışan sayısı gibi sık değişen ve risk taşımayan alanlardaki talepler kuyrukta **"hızlı onay"** olarak işaretlenebilir.

### Şirket hesabını kapatma *(öneri — detaylar onaylanacak)*

```
[Hesabı kapat]
  → "Tüm ilanlar kapanacak, ekip erişimi kesilecek."
  → 30 gün geri alma süresi → sonra kalıcı
```

| Ne | Ne olur |
|---|---|
| İlanlar | Kapanır |
| Bekleyen başvurular / davetler | Düşer, adaylara bildirim gider |
| Aktif sohbetler | Salt okunur kalır — aday geçmişi görebilsin |
| Ekip hesapları | Pasife düşer |
| Şirket kaydı | 30 gün sonra silinir |

---

## 8. Bildirimler ve Sayaçlar

### Üç ayrı mekanizma

| Durum | Ne olur | Teknoloji | Kim için |
|---|---|---|---|
| **Uygulama kapalı** | Telefona bildirim düşer | **Push** (Firebase) | **Sadece ilanın sorumlusu** |
| **Panel açık** | Sayılar güncellenir | **Periyodik yenileme** | Herkes |
| **Açık sohbet** | Mesaj saniyesinde gelir | **Realtime** (Supabase) | Sohbetteki kişiler |

### Push — sadece ilanın sorumlusuna

```
📱 ┌──────────────────────────────────┐
   │ Senior Backend Developer ilanına  │
   │ yeni başvuru                      │
   └──────────────────────────────────┘
```

| Olay | Sorumlu | Sahip / Yönetici |
|---|:---:|:---:|
| Yeni başvuru | 📱 Push + 🔔 sayaç | 🔔 Sayaç |
| Davet kabul / red | 📱 Push + 🔔 sayaç | 🔔 Sayaç |
| Yeni mesaj | 📱 Push + 💬 sayaç | 💬 Sayaç |
| Ekip olayları | — | 🔔 Sayaç |

> **Neden yönetici ve sahibe push yok:** Büyük şirkette yönetici günde yüzlerce bildirim alır ve kapatır — sonra gerçekten önemli olanı kaçırır. Sayaç sayesinde her şeyden haberdar, ama rahatsız edilmiyor.

> **Neden sorumluya push var:** Cevap hızı adayın uygulamada kalması için kritik. Başvurular saatlerce bakılmadan kalırsa aday bekler, uzaklaşır.

Push tercihi **Profilim**'den kapatılabilir.

### Bildirim kutusu

```
🔔 12
┌──────────────────────────────────────────────┐
│ Senior Backend Developer (Mehmet)  3 yeni başvuru │
│ Kaynakçı Usta (Can)                1 davet kabul  │
│ Ekip                               Zeynep katıldı │
└──────────────────────────────────────────────┘
```

**Herkes kendi okuduğunu takip eder.** Mehmet okudu diye yöneticinin sayacı sıfırlanmaz.

> Bir ilan günlerce "3 yeni" olarak kalıyorsa, sahip/yönetici sorumlunun bakmadığını anlar — devretme sinyali.

### Yüksek trafik için tasarım

> **Endişe:** Realtime her açık ekran için bir bağlantı demek — milyonlarca kullanıcıda kaldırmaz.

**Gerçek:** Bağlantı sayısı **kayıtlı kullanıcı** değil, **o an ekranı açık olan** kullanıcı kadar. Yine de bunu daha da azaltmak için realtime sadece gerçekten gereken yerde:

| Ekran | Yöntem | Neden |
|---|---|---|
| **Açık sohbet** | Realtime | Mesaj saniyesinde gelmeli |
| İlan kartı sayıları | Periyodik yenileme | Bir dakika gecikme sorun değil |
| 🔔 💬 sayaçları | Periyodik yenileme | Aynı |
| Aday Keşfet | Ekran açılınca yükle | Anlık değişim gerekmiyor |

```
Uygulamayı açık tutan:          100.000
  → o an sohbet ekranında:        ~5-10.000   ← sadece bunlar realtime
```

Bağlantı sayısı **onda birine** iniyor.

**Periyodik yenileme kuralları:**

| Kural | Neden |
|---|---|
| Ekran açılınca yükle + **60 saniyede bir** yenile + aşağı çekip yenile | Sayı en fazla bir dakika gecikmeli |
| **Tek istekte tüm sayılar** | Ekran beş sayıyı beş ayrı istekle sormasın |
| **Sekme / uygulama arka plandaysa yenileme durur** | Açık bırakılıp unutulan binlerce sekme boşuna istek atmasın |

> **Ölçek büyürse:** Sohbet trafiği çok büyürse realtime kısmı bu işe özel bir servise (Ably, Pusher gibi) taşınabilir. Geri kalanı Supabase'de kalır.

---

## 9. Sayaçlar Nasıl Artar

**İlke:** Ekran sayıları okurken **hiçbir sayım yapılmaz.** Her ilanın satırında hazır sayaç kolonları durur — okumak tek satır. Yüksek trafikte hızlı kalmanın sebebi bu.

Sayaçların artırılması, olayın sıklığına göre iki yöntemle:

### Seyrek olaylar → Tetikleyici (anında)

Başvuru, davet, kabul, red, eşleşme — bir ilana günde belki onlarca.

```
Aday başvurur
  → başvuru kaydı düşer
  → veritabanı tetikleyicisi otomatik çalışır
  → ilanın satırında: başvuru +1, bekleyen +1
```

```
İşveren başvuruyu kabul eder
  → bekleyen -1
  → kabul    +1
  → eşleşme  +1
```

> Uygulamada kod yazılmaz — kayıt düştüğü an sayaç güncellenir. Flutter mı yaptı, web mi yaptı fark etmez; her zaman doğru.

### Sık olaylar → Biriktir, toplu işle

**Görüntülenme** çok sık — popüler bir ilan saatte yüzlerce kez açılabilir. Her açılışta ilanın satırını güncellemek, aynı satıra aynı anda yüzlerce yazma demek: **sıraya girerler, veritabanı yavaşlar.**

```
1. Açılış kaydedilir — aday başına tekil
   (aynı aday ikinci kez açarsa yeni kayıt oluşmaz)

2. Dakikada bir çalışan iş:
   → son dakikadaki yeni görüntülenmeleri ilan bazında sayar
   → ilanın görüntülenme sayacını o kadar artırır
```

İlanın satırı dakikada **bir kez** güncellenir, yüz kez değil. Sayı en fazla bir dakika gecikmeli — ekran da zaten 60 saniyede bir yeniliyor.

### Özet

| Sayaç | Sıklık | Yöntem |
|---|---|---|
| Başvuru, bekleyen, kabul, red | Seyrek | Tetikleyici — anında |
| Davet, davet kabul / red | Seyrek | Tetikleyici — anında |
| Eşleşme | Seyrek | Tetikleyici — anında |
| **Görüntülenme** | **Çok sık** | **Biriktir + dakikada bir topla** |

### Güvenlik ağı

Sayaçlar nadiren de olsa kayabilir (bir hata, yarım kalan işlem). **Gece çalışan bakım işi** sayaçları gerçek kayıtlarla karşılaştırıp düzeltir. *(Eşleştirme motorundaki periyodik bakım işlerine bir madde olarak eklenir.)*

---

## 10. Platform

| | Mobil (Flutter) | Web (Next.js) |
|---|:---:|:---:|
| Şirket Profili | ✅ | ✅ |
| Profilim | ✅ | ✅ |
| Ekip | ✅ | ✅ |
| İlanlar ve ilan içi sekmeler | ✅ | ✅ |
| Şirket Yönetimi | ✅ | ✅ |
| İlan yazma / düzenleme | ✅ | ✅ *(asıl kullanım)* |

> İşverenin ağır işleri (ilan yazma, belge yükleme, ekip yönetimi) web'de daha rahat. Mobil; bildirim, mesaj ve hızlı aday değerlendirme için.

> **İş kuralları sunucuda:** Yetki kontrolü, ekip üyesi oluşturma, devretme, sayaçlar, bildirim gönderimi — hepsi Supabase tarafında (RLS, tetikleyiciler, Edge Function'lar). Flutter ve Next.js sadece ekran çiziyor.

---

## 11. Açık Kalanlar

| # | Konu | Not |
|:---:|---|---|
| 1 | **Şirket hesabını kapatma** — detaylar | Öneri yazıldı, onay bekliyor |
| 2 | **Sahipliği devretme** akışı | Sahip ayrılırsa sahipliği bir yöneticiye nasıl devreder? Onay adımı olacak mı? |
| 3 | Değişiklik talebinde admin onay süresi | Sahibe söz verilecek mi? |
| 4 | Ekip üyesi sayısı sınırı | Gelir modeliyle birlikte — plana göre sınır olacak mı? |
| 5 | Görüntülenme → başvuru oranı | İşverene ipucu olarak ileride gösterilebilir |
| 6 | Ortalama cevap süresi | Şimdilik yok — ileride değerlendirilecek |
| 7 | İlan içi sekmelerin detayı | **Keşfet ve Eşleşme** dokümanında |
| 8 | **"Müşteri adına" ilan** | Aşağıda ↓ |
| 9 | **Başvuruda şirket türü** | Aşağıda ↓ |

### İK / işe alım ajansları — karar bekleyen konular

İlan konumunun ayrılmasıyla ajansların farklı yerlerdeki işler için ilan açması mümkün hale geldi. Ama tam destek için iki karar daha gerekiyor:

**8 — "Müşteri adına" ilan özelliği**

```
Bu ilanı kimin adına açıyorsunuz?
○ Kendi şirketimiz adına
● Müşterimiz adına
    Müşteri adı: [XYZ Lojistik    ]   ☐ Müşteri adını gizle
```

Adaya:
```
Lojistik Depo Sorumlusu
ABC İK Danışmanlık  ✓ Doğrulanmış şirket
Müşteri: XYZ Lojistik            ← veya "Gizli müşteri"
📍 Gebze, Kocaeli
```

**9 — Başvuru formunda şirket türü**

```
Şirket türü:  ○ İşveren   ○ İşe alım ajansı
```

Ajans seçilirse **özel istihdam bürosu izin belgesi** istenir (birçok ülkede yasal zorunluluk — Türkiye'de İŞKUR izni). "Müşteri adına ilan" özelliği sadece **ajans olarak onaylanan** şirketlere açılır.

**Bu özelliğin açtığı riskler:**

| Risk | Açıklama | Öneri |
|---|---|---|
| ⚠️ **Gizlenen şirketler deliniyor** | Aday mevcut işvereni XYZ'yi gizledi. XYZ, ABC İK üzerinden ilan açtı. Gizleme sadece ilanı açan şirkete bakarsa aday XYZ'nin ilanına düşer — XYZ ajans aracılığıyla adayın profilini görebilir | Müşteri adı girildiyse gizleme kontrolü **müşteri şirkete de** uygulansın. **Müşteri gizliyse risk tamamen kapanmaz** — bilerek kabul edilmeli |
| "Doğrulanmış şirket" yanıltabilir | Doğrulanan ajans, müşteri değil | Kartta açıkça ayrılsın: ✓ ajansın yanında, müşterinin yanında değil |
| Lisanssız aracılık | İzinsiz işe alım aracılığı birçok ülkede yasak | Şirket türü + izin belgesi kontrolü |

---

## 12. Diğer Dokümanlara Etkisi

| Doküman | Değişecek |
|---|---|
| **Şirket Profili** | Kilitli alanlar genişledi (çalışan sayısı, web sitesi, şirket telefonu, şirket e-postası da kilitli); sekmeler; Belgeler → Şirket Yönetimi'ne taşındı; **ek lokasyonlardaki iki koruma kuralı kaldırıldı** |
| **Eşleştirme Motoru** | Konum filtresi `ilanlar.konum_yolu` üzerinden — artık şirket lokasyonundan kopyalanmıyor, ilanın kendi seçimi |
| **İlan Oluşturma** | Yayın şartı: tanıtım dolu + kişisel telefon doğrulanmış; `sorumlu_kullanici_id`; ilan kartı sayaçları; **ilan konumu kendi kaydında, konum ağacından serbest seçim — şirket lokasyonuna bağlı değil** (`lokasyon_id` bağlantısı kalkıyor, ilanın kendi `konum_id`, `konum_yolu`, `ulke_kodu` alanları) |
| **İşveren Başvurusu** | *(Karar verilirse)* şirket türü sorusu + ajans izin belgesi |
| **Eşleştirme Motoru** | Periyodik bakıma sayaç düzeltme işi eklenecek; bildirim kuyruğunda push sadece sorumluya |
| **Auth** | Ekip üyesi giriş yöntemi: sadece e-posta + şifre (davet maili ile) |
| **Admin Paneli** *(yazılacak)* | Değişiklik talepleri kuyruğu ("hızlı onay" işareti) |
