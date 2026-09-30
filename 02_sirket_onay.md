# İş Eşleştirme Projesi — Şirket Doğrulama Ekranı

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak
> İlgili doküman: `giris-kayit-kararlari.md`

---

## 1. İşveren Tarafı — Ekran Durumları

Panelde **"Şirket Doğrulama"** bölümü. Üç durumda üç farklı görünüm:

### Durum 1 — Henüz yüklenmemiş
- Kısa açıklama: *"İlan yayınlamak için şirketinizi doğrulayın"*
- **Ülke seçimi** *(ilk adım — form alanlarını ve belge listesini bu belirler)*
- Bilgi formu
- Belge yükleme alanı

### Durum 2 — İncelemede
- *"Belgeniz inceleniyor"* + yükleme tarihi
- Yönlendirme: *"Bu arada şirket profilinizi tamamlayabilirsiniz"*
- Yüklenen belgeler görünür durumda

### Durum 3 — Reddedildi
- **Ret sebebi bariz şekilde yazılı**
- Tekrar yükleme butonu
- **Önceki yüklenen belge görünsün** — neyi değiştireceğini anlasın

---

## 2. Formda İstenecek Bilgiler

**Form ülke seçimiyle başlar.** Seçilen ülkeye göre alan etiketleri, format kontrolleri ve belge listesi değişir.

### Her ülkede ortak alanlar

| Alan | Zorunlu | Not |
|---|---|---|
| **Ülke** | ✅ | İlk soru — formun geri kalanını belirler |
| Şirket resmi unvanı | ✅ | Belgedeki unvanla eşleşmeli |
| **Tescil / vergi numarası** | ✅ | Etiketi ve format kontrolü ülkeye göre |
| **Tescil makamı** | ✅ | Etiketi ülkeye göre |
| Adres | ✅ | **`konumlar` ağacından seçim** (ülke → alt seviyeler) + açık adres |
| Yetkili adı ve görevi | ✅ | Örn: "Ayşe Yılmaz — İK Müdürü" |
| Şirket telefonu | ✅ | **Genel santral numarası** — teyit için aranacak, ülke koduyla |
| Web sitesi | ❌ | Varsa güçlü sinyal |

### Ülkeye göre değişen etiketler

| Ülke | Numara alanı | Makam alanı |
|---|---|---|
| Türkiye | Vergi numarası *(10 hane)* | Vergi dairesi |
| ABD | EIN *(9 hane)* | Eyalet tescil makamı |
| Birleşik Krallık | Company Number *(8 hane)* | Companies House |
| AB ülkeleri | VAT numarası | Ticaret sicili |
| Diğer | Tescil numarası | Tescil makamı |

```sql
ulke_form_alanlari (
  ulke_kodu, alan_kodu, etiket, format_regex, zorunlu_mu, sira
)
```

Tabloda tutulur — yeni ülke eklemek uygulama güncellemesi gerektirmez.

### Adres alanı

Serbest metin değil, **`konumlar` ağacından seçim**:

```
Ülke (zaten seçili) → alt seviye(ler) → açık adres (opsiyonel)
```

Ağaç yapısı her ülkenin kendi derinliğini taşır (ABD'de eyalet → şehir, Türkiye'de il → ilçe, Singapur'da tek kademe). Arayüzde kademeli menü yerine **tek arama kutusu** kullanılır — kullanıcı yazar, "San Francisco, California, ABD" gibi sonuç çıkar, tıklar.

> Bu adres doğrulama kaydı içindir. Şirketin **ilan verilecek fiziksel lokasyonları** ayrıca şirket profilinde (`sirket_lokasyonlari`) tutulur.

---

## 3. Kabul Edilecek Belgeler

**Belge listesi ülkeye göre gelir. En az biri zorunlu.**

```sql
ulke_belge_tipleri (ulke_kodu, belge_tipi, ad, aciklama, sira)
```

### Türkiye

- **Vergi levhası** — en yaygın, herkeste var; üstünde vergi no + unvan + adres yazıyor
- **Faaliyet belgesi** (ticaret/sanayi odasından)
- **Ticaret sicil gazetesi**

### Diğer ülkeler — karşılıkları

| Ülke | Belgeler |
|---|---|
| ABD | EIN atama mektubu (IRS), Articles of Incorporation, eyalet iş lisansı |
| Birleşik Krallık | Companies House tescil belgesi, VAT kayıt belgesi |
| Almanya | Handelsregisterauszug, Gewerbeanmeldung |
| AB geneli | Ticaret sicil kaydı, VAT kayıt belgesi |
| Diğer | Ülkenin resmi ticaret tescil belgesi |

> Liste ülke açıldıkça doldurulur. Yeni ülke açmadan önce o ülkenin kabul edilecek belgeleri tabloya girilmeli.

**Teknik kısıtlar:**

| | |
|---|---|
| Format | PDF, JPG, PNG |
| Boyut | Dosya başına 10 MB |
| Adet | En fazla 3 dosya (belgeler çok sayfalı olabiliyor) |

---

## 4. Admin Tarafı — Doğrulama Kuyruğu

### Her satırda görünecek

- **Ülke bayrağı / kodu** — hangi kurallara göre değerlendirileceğini gösterir
- Şirket unvanı, tescil/vergi no, yetkili, telefon
- **Yüklenen belgeler** — tıklayınca yan panelde açılsın, indirmeye gerek kalmasın
- **Kayıt e-postası** (kurumsal mı Gmail mi — rozet kararı için)
- Başvuru tarihi

**Ülkeye göre filtreleme ve atama** olmalı — ileride bölgesel admin ekipleri kurulduğunda kuyruk ülkeye göre bölünebilsin.

### Üç aksiyon

#### ✅ Onayla
- Şirket doğrulanır
- **Rozet belirlenir:** kurumsal e-posta varsa "Doğrulanmış şirket" rozeti, Gmail vb. ise rozetsiz onay
- İşverene bildirim/e-posta gider *(kullanıcının arayüz dilinde)*
- **İlan açma kilidi kalkar**

#### ❌ Reddet — sebep seçmeli
Hazır sebepler (elle yazma yok — hız ve tutarlılık için):

- Belge okunmuyor / net değil
- Belgedeki unvan girilen şirket adıyla uyuşmuyor
- Belge geçersiz veya eksik
- Tescil / vergi numarası doğrulanamadı
- **Bu ülke için kabul edilen bir belge değil**
- Diğer *(serbest metin)*

Seçilen sebep işverene **aynen** gider — sebep metinleri çevrilebilir olmalı (kod olarak saklanır, arayüzde kullanıcının diline çevrilir).

#### ⏳ Ek bilgi iste
- Reddetmeden *"şu belgeyi de yükleyin"* diyebilme
- Durum **"incelemede"** kalır, işveren ek yükleme yapar
- **Bu buton olmazsa** her eksik için reddetmek zorunda kalırsınız → kötü deneyim

---

## 5. Ekstra Teyit (Elle, İlk Dönem)

Şirketin **genel telefonunu arayıp** *"İK'dan falan bey mi başvurdu"* diye sormak.
En güçlü doğrulama, ekstra maliyeti yok — ilk dönem zaten 20-30 şirketle konuşulacak.

**Resmi sorgu sistemleri** ülkeye göre:

| Ülke | Sorgu kaynağı |
|---|---|
| Türkiye | GİB vergi no sorgulama, MERSİS |
| Birleşik Krallık | Companies House (açık API var) |
| ABD | Eyalet bazlı tescil sorguları (ortak API yok) |
| AB | VIES (VAT numarası doğrulama, açık servis) |

> ⚠️ **Ölçek notu:** Elle onay global ölçekte tıkanır. Ülke sayısı arttıkça (a) resmi API entegrasyonları, (b) bölgesel admin ekipleri gerekecek. Şimdi yapılmayacak, ama **kuyruk yapısı ülke bazlı bölünebilir kurulmalı.**

---

## 6. Teknik Notlar

### Depolama ve erişim
- Belgeler **Supabase Storage'da özel klasörde**
- **RLS:** işveren sadece kendi yüklediğini görür, admin hepsini
- Bu belgeler kişisel/ticari hassas veri → **herkese açık bucket'a konmaz**

### Loglama
Her admin aksiyonu **`admin_log`**'a yazılır: kim, ne zaman, hangi şirketi onayladı/reddetti.

### Saklama süresi — ülkeye göre değişir
- Belgeyi süresiz saklamak hiçbir yerde savunulamaz
- **Onay sonrası 1 yıl** genel varsayılan → sonra belge silinir
- **Onay kaydı kalır** (kim, ne zaman onayladı), belge gider
- **Veri koruma rejimi ülkeye bağlı:** Türkiye'de KVKK, AB/AEA'da GDPR, Birleşik Krallık'ta UK GDPR, Kaliforniya'da CCPA. Saklama süresi ve silme hakları farklılaşabilir.

```sql
ulke_veri_politikalari (ulke_kodu, belge_saklama_ay, notlar)
```

> ⚠️ **Veri yerleşimi (data residency):** Bazı ülkeler kişisel verinin yurt dışına çıkmasını kısıtlıyor. Global ölçekte bu bir mimari konu — veritabanının nerede barındığı sorun olabilir. Lansman ülkeleri seçilirken hukuki danışmanlık alınmalı.

### Tekrar yükleme sınırı
Reddedilen başvuru sınırsız denenebilirse kuyruk dolar.
**3 deneme sonrası** destek yönlendirmesi.

---

## 7. Hatırlatma — İlan Açma Kilidi

Onaysız hesap:

| | |
|---|---|
| Panele girebilir | ✅ |
| Şirket profilini doldurabilir | ✅ |
| İlan taslağı hazırlayabilir | ✅ |
| İlanı yayınlayabilir | ❌ |
| Aday görebilir | ❌ |
| Mesaj atabilir | ❌ |

> Kilit **RLS ile veritabanı seviyesinde** olmalı. Arayüzde gizlemek yetmez.

---

## 8. Açık Kalanlar

- Admin onay süresi hedefi — kullanıcıya "24 saat içinde" gibi bir söz verilecek mi? *(Zaman dilimi farkları bunu zorlaştırıyor)*
- Onay sonrası şirket bilgisi değişirse (unvan, adres) yeniden doğrulama gerekecek mi?
- Başvuru sayısı artınca ülke bazlı otomatik tescil sorgulama entegrasyonları *(önce Companies House ve VIES — açık API'leri var)*
- Aynı tescil numarasıyla ikinci bir hesap açılmak istenirse ne olacak? (şube / farklı İK kullanıcısı senaryosu)
- **Hangi ülkelerle lansmana çıkılacak?** Belge listesi, admin kapasitesi ve hukuki hazırlık buna bağlı
- **Çok uluslu şirketler:** Merkezi Almanya, ilanı Türkiye'de olan şirket hangi ülke kurallarıyla doğrulanacak? *(Öneri: doğrulama merkez ülkesine göre, ilan lokasyonu ayrı)*
- Admin panelinin dili — belgeler yerel dilde gelecek, admin okuyabilecek mi?
