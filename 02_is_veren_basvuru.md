# İş Eşleştirme Projesi — İşveren Başvurusu

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak
> Durum: Ekip toplantısı sonrası nihai karar
> İlgili dokümanlar: `auth-kararlari.md`, `sirket-profili-v3.md`

---

## 0. Özet

İşveren uygulamada **kayıt olmaz.** Herkese açık bir web formuyla **başvurur**, biz belgelerini kontrol edip **onaylarız**, onayla birlikte hesabı açılır ve giriş bilgileri e-postasına gider.

```
Başvuru formu  →  Admin kontrolü  →  Onay  →  Hesap açılır  →  E-posta + şifre gönderilir
 (giriş yok)                                    (şirket onaylı,
                                                 telefon doğrulanmış)
```

**Bu yaklaşımın sonucu:** Sistemde **onaysız işveren hesabı hiç yok.** Sahte şirket hesapları oluşmaz, "onaysız hesap" kilitlerine gerek kalmaz.

**Formda verilen bilgiler**, onayla birlikte şirket profiline **hazır ve doğrulanmış** olarak aktarılır. İşverenin onaydan sonra yapması gereken tek zorunlu iş **şirket tanıtımı** yazmak.

---

## 1. Form Nerede

| | |
|---|---|
| **Platform** | Web (Next.js) — herkese açık sayfa, giriş gerektirmez |
| **Mobil** | Uygulamadaki "İşveren olarak başvur" bağlantısı web formunu açar |

**Giriş noktaları:**
- İşveren giriş ekranındaki **[İşveren olarak başvur]** bağlantısı *(her zaman görünür)*
- Web sitesinde doğrudan bağlantı

> **Neden sadece web:** İşveren uygulama indirmeden başvurabilmeli. Belge yükleme de masaüstünde daha rahat.

---

## 2. Form Yapısı — 7 Bölüm

| # | Bölüm | Amaç |
|:---:|---|---|
| 1 | **Ülke** | Formun geri kalanını belirler |
| 2 | **Şirket bilgileri** | Kimlik — onaydan sonra şirket profiline aktarılır |
| 3 | **Merkez adres** | Resmi adres — kilitli merkez lokasyon olur |
| 4 | **Şirket iletişim** | Şirket telefonu ve e-postası |
| 5 | **Yetkili** | Hesabın sahibi olacak kişi |
| 6 | **Belgeler** | Doğrulama için |
| 7 | **Onaylar** | Yasal metinler |

---

## 3. Bölüm 1 — Ülke

| Alan | Zorunlu | Not |
|---|:---:|---|
| Ülke | ✅ | **İlk soru** |

Seçilen ülkeye göre değişenler:
- Tescil / vergi numarası alanının **etiketi ve formatı**
- Tescil makamı alanının **etiketi**
- **Kabul edilen belge listesi**
- Konum ağacının **başlangıç noktası**
- Telefon alanlarının **varsayılan ülke kodu**

> Ülkeye özel kurallar veritabanında tutulur. Yeni ülke açmak uygulama güncellemesi gerektirmez.

> SMS sadece **lansman ülkelerine** açık olduğu için, ülke listesinde sadece lansman ülkeleri gösterilir.

---

## 4. Bölüm 2 — Şirket Bilgileri

| Alan | Zorunlu | Giriş | Not |
|---|:---:|---|---|
| Şirket resmi unvanı | ✅ | Serbest | Belgedeki tam unvan — **adaya gösterilmez**, admin karşılaştırması için |
| Şirket adı (görünen) | ✅ | Serbest | Adaya görünen kısa ad |
| Tescil / vergi numarası | ✅ | Serbest + format kontrolü | Etiketi ülkeye göre |
| Tescil makamı / vergi dairesi | ✅ | Serbest | Etiketi ülkeye göre |
| Üst sektör | ✅ | Kapalı liste | |
| Alt sektör | ✅ | Kapalı liste | Üst sektör seçilince açılır |
| Çalışan sayısı | ✅ | Kapalı liste | `1-10` / `11-50` / `51-200` / `201-500` / `500+` |
| Kuruluş yılı | ✅ | Yıl | 1800 ile bu yıl arası |
| Web sitesi | ❌ | Serbest | Başında `https://` yoksa eklenir |
| Sosyal medya | ❌ | Platform + kullanıcı adı, çoklu | LinkedIn, Instagram, X, Facebook, YouTube |

### Resmi unvan ve görünen ad — neden iki ayrı alan

```
Resmi unvan:   ABC Bilgi Teknolojileri Sanayi ve Ticaret Anonim Şirketi
Görünen ad:    ABC Teknoloji
```

Resmi unvan uzun ve adaya bir şey ifade etmiyor. Adaya **görünen ad** gösterilir; resmi unvan sadece admin'in belgeyle karşılaştırması için tutulur.

### Ülkeye göre etiketler

| Ülke | Numara alanı | Makam alanı |
|---|---|---|
| Türkiye | Vergi numarası *(10 hane)* | Vergi dairesi |
| ABD | EIN *(9 hane)* | Eyalet tescil makamı |
| Birleşik Krallık | Company Number *(8 hane)* | Companies House |
| AB ülkeleri | VAT numarası | Ticaret sicili |
| Diğer | Tescil numarası | Tescil makamı |

### Sektör

İki kademeli kapalı liste. Üst sektör seçilince alt sektör listesi otomatik açılır.

```
Bilişim
├── Yazılım Geliştirme
├── Siber Güvenlik
├── Veri ve Yapay Zeka
├── ...
└── Diğer
```

> **Neden kapalı liste:** Aday sektöre göre filtreleyebilecek. Serbest olsa "Yazılım", "Bilişim", "IT" ayrı kayıtlar olur.

### Sosyal medya

Tam bağlantı yerine **kullanıcı adı** istenir:

```
Platform: [LinkedIn ▾]   Kullanıcı adı: [abcteknoloji]   [+ Ekle]
```

Platform adresi sistem tarafından eklenir — insanların yanlış bağlantı yapıştırması (paylaşım linki, uygulama içi link) önlenir.

---

## 5. Bölüm 3 — Merkez Adres

| Alan | Zorunlu | Not |
|---|:---:|---|
| Konum | ✅ | Konum ağacından arama — şehir/ilçe |
| Açık adres | ✅ | Sokak, bina, numara |

> **Her şirketin kesin adresi olur, istisna yok.** Uzaktan çalışan şirketlerin de resmi/tescilli bir adresi var — belgede yazıyor, admin onu kontrol ediyor.

> **"Uzaktan çalışma" şirket düzeyinde değil, ilan düzeyinde.** Her ilanın çalışma şekli (ofis / hibrit / uzaktan) ayrı seçilir. Şirket düzeyinde ayrı bir "uzaktan" işareti ilanla çakışıp kafa karıştırırdı.

### Konum seçimi

Kademeli menü yok — **tek arama kutusu:**

```
Konum: [Kadık                    ]
       ┌────────────────────────────────┐
       │ Kadıköy, İstanbul, Türkiye     │
       │ Kadıköy, Kırklareli, Türkiye   │
       └────────────────────────────────┘
```

> Kademeli menü (ülke → il → ilçe) 200 ülkede çalışmaz; her ülkenin idari yapısı farklı. Arama kutusu global ürünlerde standart çözüm.

### Gösterim

| | Adaya görünür |
|---|---|
| Şehir / ilçe | ✅ Şirket kartında |
| Açık adres | **Sadece eşleşmeden sonra** |

> Bazı şirketler sokak/bina bilgisini herkese açık paylaşmak istemiyor. Eşleşen aday görüşmeye gidecek — o noktada açık adres gerekli.

### Onaydan sonra

Bu adres şirketin **merkez lokasyonu** olur — kilitli, değişiklik talebiyle güncellenir. Şirket sonradan istediği kadar **ek lokasyon** ekleyebilir (bkz. `sirket-profili-v3.md`).

---

## 6. Bölüm 4 — Şirket İletişim

| Alan | Zorunlu | Not |
|---|:---:|---|
| Şirket telefonu | ✅ | Santral olabilir — **SMS ile doğrulanmaz** |
| Şirket e-postası | ✅ | "Hesap e-postasıyla aynı" kutusu |

### Şirket telefonu

SMS sabit hatta gitmediği için formda doğrulanmaz. **Admin onayıyla doğrulanmış sayılır** — admin ilk dönem bu numarayı arayıp teyit ediyor.

Adaya şirket kartında görünür.

### Şirket e-postası

```
Şirket e-postası: [______________]
                  ☑ Hesap e-postasıyla aynı
```

"Aynı" işaretliyse alan gizlenir, hesap e-postası kullanılır — küçük şirkete ek yük yok.

**Ne işe yarar:**
- Belge yenileme hatırlatmaları
- Şirket hesabıyla ilgili kritik bilgilendirmeler
- Hesap sahibi şirketten ayrılır ve ulaşılamazsa, admin şirkete buradan ulaşır

**Adaya gösterilmez.**

---

## 7. Bölüm 5 — Yetkili (Hesap Sahibi)

| Alan | Zorunlu | Not |
|---|:---:|---|
| Ad | ✅ | |
| Soyad | ✅ | |
| Görev / unvan | ✅ | "İK Müdürü", "Genel Müdür" vb. |
| Hesap e-postası | ✅ | **Giriş bilgileri buraya gidecek** — herhangi bir adres olabilir |
| Cep telefonu | ✅ | **Formda SMS ile doğrulanır** |

**Bu kişi şirketin sahibi olur.** Onaydan sonra ekip üyelerini (yönetici, uzman) kendisi ekler.

Formun bu bölümünde not:

> *"Hesabınız açıldıktan sonra İK ekibinizi 'Ekip' menüsünden ekleyebilirsiniz."*

> **Ayrı "İK yönetici e-postası" neden yok:** Bu kişi başvuranla aynı değilse ya formda işlevsiz bir bilgi olarak kalır ya da Ekip özelliğini tekrarlar. İK yöneticisi, hesap açıldıktan sonra Ekip menüsünden eklenir.

### Hesap e-postası

Ayrıca doğrulanmaz — giriş bilgileri o adrese gideceği için, giriş yapabilen kişi adresin sahibidir.

### Cep telefonu — formda SMS doğrulaması

```
Cep telefonu: [+90 ▾] [532 123 45 67]  [Doğrula]
                          ↓
"Telefonunuza gelen 6 haneli kodu girin"
[ _ _ _ _ _ _ ]
                          ↓
Cep telefonu: +90 532 123 45 67  ✅ Doğrulandı
```

**Doğrulanmadan form gönderilemez:**
```
[Başvuruyu gönder]  ← telefon doğrulanmadan pasif
```

**İki faydası:**
1. **Sahte başvuruyu ciddi azaltır** — admin kuyruğu çöple dolmaz
2. **Hesap açıldığında sahibin telefonu zaten doğrulanmış olur** — giriş yapar yapmaz ilan yayınlayabilir

Numara uluslararası standart formata (E.164) çevrilerek saklanır: `+905321234567`

---

## 8. Bölüm 6 — Belgeler

| | |
|---|---|
| **Liste** | Ülkeye göre |
| **En az** | 1 belge |
| **En fazla** | 3 dosya *(bazı belgeler çok sayfalı)* |
| **Format** | PDF, JPG, PNG |
| **Boyut** | Dosya başına 10 MB |

### Ülkeye göre kabul edilen belgeler

**Türkiye**
- Vergi levhası *(en yaygın — üstünde vergi no, unvan, adres yazıyor)*
- Faaliyet belgesi *(ticaret / sanayi odasından)*
- Ticaret sicil gazetesi

**ABD**
- EIN atama mektubu (IRS)
- Articles of Incorporation
- Eyalet iş lisansı

**Birleşik Krallık**
- Companies House tescil belgesi
- VAT kayıt belgesi

**Almanya**
- Handelsregisterauszug
- Gewerbeanmeldung

**AB geneli**
- Ticaret sicil kaydı
- VAT kayıt belgesi

> Liste veritabanında ülke bazlı tutulur. **Yeni ülke açmadan önce o ülkenin belge listesi girilmeli.**

---

## 9. Bölüm 7 — Onaylar

| Alan | Zorunlu |
|---|:---:|
| ☐ Kullanım koşullarını okudum ve kabul ediyorum | ✅ |
| ☐ Kişisel verilerin işlenmesine ilişkin aydınlatma metnini okudum | ✅ |
| ☐ Verdiğim bilgilerin doğru olduğunu beyan ederim | ✅ |

> Herkese açık bir formda kişisel veri ve belge toplanıyor — yasal metinlerin onayı zorunlu. Aydınlatma metni ülkenin veri koruma rejimine göre (KVKK, GDPR, diğer) gösterilir.

> **Doğruluk beyanı:** Sahte belge veya bilgi ile başvuru tespit edilirse hesabın kapatılmasına ve gerekirse yasal işleme dayanak olur.

---

## 10. Gönderim

```
[Başvuruyu gönder]
        ↓
┌──────────────────────────────────────────┐
│  ✅ Başvurunuz alındı                     │
│                                          │
│  Başvuru numarası: ISV-2026-00412        │
│                                          │
│  Belgeleriniz incelendikten sonra        │
│  sonuç e-posta ile bildirilecek.         │
└──────────────────────────────────────────┘
```

Aynı bilgiler **hesap e-postasına** da gönderilir:

```
Konu: Başvurunuz alındı — ISV-2026-00412

ABC Teknoloji için işveren başvurunuz alındı.
Belgeleriniz incelendikten sonra size bilgi vereceğiz.
```

> Başvuran kişi henüz giriş yapamadığı için durumunu takip edebileceği tek kanal e-posta. Başvuru numarası destek yazışmalarında kullanılır.

---

## 11. Formun Korunması

Giriş gerektirmeyen bir form botların hedefidir.

| Önlem | Ne yapar |
|---|---|
| **Görünmez CAPTCHA** | Bot gönderimlerini engeller — kullanıcıya bulmaca çözdürmez (örn. Cloudflare Turnstile) |
| **IP başına sınır** | Günde birkaç başvuru |
| **Güvenli belge yükleme** | İmzalı, süreli yükleme bağlantısı — depolama alanı herkese açılmaz |
| **Tekrar başvuru kontrolü** | Aynı tescil/vergi numarasıyla ikinci başvuru engellenir |
| **SMS sınırları** | Numara başına saatte 3, IP başına günde 5 kod; sadece lansman ülkeleri |

### Tekrar başvuru

Aynı ülke + aynı tescil/vergi numarası sistemde varsa:

| Durum | Mesaj |
|---|---|
| Bekleyen başvuru var | *"Bu şirket için incelenmekte olan bir başvuru var."* |
| Onaylı hesap var | *"Bu şirketin hesabı mevcut. Ekibe katılmak için şirket yöneticinizle iletişime geçin."* |
| Önceki başvuru reddedilmiş | Yeni başvuruya izin verilir |

### SMS kötüye kullanımı

> ⚠️ **"SMS pumping":** Botlar "Doğrula" butonunu yüksek ücretli ülke numaralarıyla binlerce kez tetikleyip faturayı şişirir. Herkese açık formda en büyük risk bu.

Sınır aşılınca:

> *"Çok fazla deneme yapıldı. Lütfen 1 saat sonra tekrar deneyin."*

---

## 12. Admin İncelemesi

### Admin kuyruğu

Her başvuru satırında:
- Başvuru numarası, tarih, ülke
- Resmi unvan ve görünen ad
- Tescil numarası, tescil makamı
- Sektör, çalışan sayısı, kuruluş yılı
- Merkez adres
- Şirket telefonu, şirket e-postası
- Yetkili bilgileri, hesap e-postası, cep telefonu (✅ SMS doğrulanmış)
- Yüklenen belgeler — tıklayınca yan panelde açılır

**İşaretler** *(admin'in dikkatini çeker):*
- Yetkilinin cep numarası başka şirket(ler)de de kayıtlı → *"Bu numara 2 farklı şirket başvurusunda kullanılmış"*
- Hesap e-postası ücretsiz bir servisten → bilgi amaçlı, engel değil

### Kontrol listesi

Admin şunları kontrol eder:
1. Belgedeki unvan, formdaki resmi unvanla uyuşuyor mu?
2. Belgedeki tescil/vergi numarası, formdakiyle aynı mı?
3. Belgedeki adres, formdaki merkez adresle uyumlu mu?
4. Belge bu ülke için kabul edilen türden mi, geçerli mi?
5. *(İlk dönem, elle)* Şirket telefonunu arayıp başvuruyu teyit et — *"İK'dan Ayşe Hanım mı başvurdu?"*

**Resmi sorgu sistemleri** *(varsa):*

| Ülke | Kaynak |
|---|---|
| Türkiye | GİB vergi no sorgulama, MERSİS |
| Birleşik Krallık | Companies House (açık sorgu) |
| AB | VIES (VAT numarası doğrulama) |
| ABD | Eyalet bazlı tescil sorguları |

### Üç aksiyon

#### ✅ Onayla

Sistem otomatik olarak:
1. **Şirket kaydını** oluşturur — formdaki bilgilerle, **onaylı**
2. **Şirket telefonunu** doğrulanmış işaretler (admin teyidi)
3. **Merkez lokasyonu** oluşturur — kilitli
4. **Sahip hesabını** oluşturur — cep telefonu doğrulanmış (formda yapılmıştı)
5. **Güçlü rastgele şifre** üretir
6. Hesap e-postasına **onay maili** gönderir

#### ❌ Reddet

- Hazır sebeplerden biri seçilir *(elle yazma yok — hız ve tutarlılık)*
- Hesap e-postasına ret maili + **"Belgeleri tekrar yükle"** bağlantısı

**Hazır ret sebepleri:**
- Belge okunmuyor / net değil
- Belgedeki unvan, girilen unvanla uyuşmuyor
- Belgedeki numara, girilen numarayla uyuşmuyor
- Belge geçersiz veya süresi dolmuş
- Bu ülke için kabul edilen bir belge değil
- Şirket teyit edilemedi
- Diğer *(serbest metin)*

#### ⏳ Ek bilgi iste

- Başvuru **"incelemede"** kalır
- Admin ne istediğini yazar: *"Faaliyet belgesinin güncel halini yükleyin."*
- Hesap e-postasına ek yükleme bağlantısı gider

> **Bu buton olmazsa** her küçük eksik için reddetmek zorunda kalınır — başvuran için kötü deneyim.

### Ret ve ek bilgi bağlantıları

- **Giriş gerektirmez** — kişinin henüz hesabı yok
- **O başvuruya özel** — başka başvuruya erişim vermez
- **Süreli** — 14 gün
- Kişi bağlantıya tıklar, **sadece eksik belgeyi yükler** veya düzeltmesi gereken alanı düzeltir — **formu baştan doldurmaz**

```
Başvurunuz için ek belge gerekiyor — ISV-2026-00412

Admin notu: "Faaliyet belgesinin güncel halini yükleyin."

[Dosya ekle]
[Gönder]
```

Gönderilince başvuru tekrar admin kuyruğuna düşer.

### Başvuru durumları

```
beklemede  →  incelemede  →  onaylandı
                  ↓    ↑
            ek_bilgi_istendi
                  ↓
              reddedildi  →  (yeniden başvuru mümkün)
```

---

## 13. Onay Maili ve İlk Giriş

### Onay maili

```
Konu: İşveren hesabınız açıldı

ABC Teknoloji işveren hesabınız onaylandı.

Giriş bilgileriniz:
E-posta: ayse@abc.com
Şifre:   K7m#pQ2xVn9w

[İşveren girişine git]

Güvenliğiniz için ilk girişte şifrenizi değiştirmenizi öneririz.
```

### Şifre güvenliği

| Önlem | Neden |
|---|---|
| **Güçlü rastgele şifre** (12+ karakter, harf/rakam/sembol) | Tahmin edilemez |
| **Sistemde açık halde saklanmaz** | Mail gönderildikten sonra sadece özeti kalır; admin de göremez |
| **7 gün içinde giriş yapılmazsa hesap askıya alınır** | Unutulan veya yanlış adrese giden mail açık kapı olarak kalmaz. Admin "yeniden gönder" der, yeni şifre üretilir |
| **İlk girişte yumuşak hatırlatma** | *"Şifrenizi değiştirmek ister misiniz?"* **[Değiştir] [Şimdi değil]** — zorunlu değil |

### İlk giriş

```
İşveren girişi → e-posta + şifre
        ↓
"Şifrenizi değiştirmek ister misiniz?"  [Değiştir] [Şimdi değil]
        ↓
┌──────────────────────────────────────────┐
│  ABC Teknoloji'ye hoş geldiniz.          │
│                                          │
│  İlan yayınlamadan önce:                 │
│  ☐ Şirket tanıtımını yazın               │
│  ☐ Logo yükleyin (opsiyonel)             │
│                                          │
│  [Profili tamamla]                       │
└──────────────────────────────────────────┘
```

**Şirket onaylı, telefon doğrulanmış, bilgilerin çoğu dolu** — ilan yayınlamak için tek zorunlu eksik **şirket tanıtımı**.

---

## 14. Formdan Şirket Profiline Aktarım

| Form alanı | Şirket profilinde | Değiştirilebilir |
|---|---|---|
| Resmi unvan | Sadece admin'de | — |
| Şirket adı (görünen) | Şirket adı | 🔒 Değişiklik talebi |
| Tescil / vergi numarası | Sadece admin'de | — |
| Tescil makamı | Sadece admin'de | — |
| Üst / alt sektör | Sektör | 🔒 Değişiklik talebi |
| Kuruluş yılı | Kuruluş yılı | 🔒 Değişiklik talebi |
| Çalışan sayısı | Çalışan sayısı | ✅ Serbest |
| Web sitesi | Web sitesi | ✅ Serbest |
| Sosyal medya | Sosyal medya | ✅ Serbest |
| Merkez konum + açık adres | Merkez lokasyon | 🔒 Değişiklik talebi |
| Şirket telefonu | Şirket telefonu ✅ | ✅ Değişince admin teyidine düşer |
| Şirket e-postası | Şirket e-postası | ✅ Serbest |
| Yetkili ad, soyad, görev | **Profilim** (sahip) | ✅ Serbest |
| Hesap e-postası | **Profilim** — giriş e-postası | — |
| Cep telefonu ✅ | **Profilim** | ✅ Değişince yeniden SMS doğrulama |
| Belgeler | **Belgeler** bölümü (sadece sahip) | — |

> **Kilitli alanlar:** Şirketin kimliği — admin tarafından belgeyle doğrulandı. Değiştirmek için sahip **değişiklik talebi** gönderir (yeni değer + sebep + isteğe bağlı belge), admin onaylar.

> **Resmi unvan, tescil numarası ve makamı adaya gösterilmez** — sadece doğrulama içindir.

---

## 15. Kayıtların Saklanması

| Kayıt | Saklama |
|---|---|
| **Başvuru kaydı** | **Silinmez** — hangi başvurudan hangi şirketin oluştuğu, kimin onayladığı denetim izi olarak kalır |
| **Belgeler** | Onaydan 1 yıl sonra dosya silinir, kayıt satırı kalır |
| **Reddedilen başvuru** | Belgeler 90 gün sonra silinir, kayıt kalır |

> Saklama süreleri ülkenin veri koruma rejimine göre (KVKK, GDPR, diğer) değişebilir.

---

## 16. Açık Kalanlar

| # | Soru | Not |
|:---:|---|---|
| 1 | Admin onay süresi hedefi | Başvurana söz verilecek mi? *(örn. "2 iş günü içinde")* |
| 2 | Lansman ülkeleri | Belge listesi, SMS izinleri, admin kapasitesi buna bağlı |
| 3 | Bekleyen başvurunun iptali | Başvuran vazgeçerse e-postadaki bağlantıyla iptal edebilsin mi? |
| 4 | Reddedilen başvuruda tekrar deneme sınırı | Sınırsız mı, örn. 3 deneme mi? |
| 5 | Otomatik tescil sorgusu | Ülke sayısı arttıkça gerekecek — Companies House ve VIES ile başlanabilir |
| 6 | Çok uluslu şirketler | Merkezi Almanya, ilanı Türkiye'de olan şirket hangi ülkenin formuyla başvurur? *(Öneri: merkez ülkesi)* |

---

## 17. Diğer Dokümanlara Etkisi

| Doküman | Değişecek |
|---|---|
| **Auth** | Başvuru form alanları bu dokümana taşındı — auth'ta özet + bağlantı yeterli |
| **Şirket Profili** | Çalışan sayısı artık başvurudan geliyor; yayın eşiği **sadece tanıtım**; "Tamamen uzaktan" seçeneği kaldırıldı; resmi unvan ve görünen ad ayrımı |
| **Admin Paneli** *(yazılacak)* | Başvuru kuyruğu, kontrol listesi, üç aksiyon, değişiklik talepleri kuyruğu, şirket telefonu değişikliği teyidi |
