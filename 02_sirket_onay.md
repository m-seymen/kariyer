# İş Eşleştirme Projesi — Şirket Doğrulama Ekranı

> Son güncelleme: 20 Eylül 2026
> İlgili doküman: `giris-kayit-kararlari.md`

---

## 1. İşveren Tarafı — Ekran Durumları

Panelde **"Şirket Doğrulama"** bölümü. Üç durumda üç farklı görünüm:

### Durum 1 — Henüz yüklenmemiş
- Kısa açıklama: *"İlan yayınlamak için şirketinizi doğrulayın"*
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

| Alan | Zorunlu | Not |
|---|---|---|
| Şirket resmi unvanı | ✅ | Belgedeki unvanla eşleşmeli |
| Vergi numarası | ✅ | 10 haneli, format kontrolü yapılabilir |
| Vergi dairesi | ✅ | Levhada yazıyor, teyidi kolaylaştırır |
| Adres | ✅ | |
| Yetkili adı ve görevi | ✅ | Örn: "Ayşe Yılmaz — İK Müdürü" |
| Şirket telefonu | ✅ | **Genel santral numarası** — teyit için aranacak |
| Web sitesi | ❌ | Varsa güçlü sinyal |

---

## 3. Kabul Edilecek Belgeler

**En az biri zorunlu:**

- **Vergi levhası** — en yaygın, herkeste var; üstünde vergi no + unvan + adres yazıyor
- **Faaliyet belgesi** (ticaret/sanayi odasından)
- **Ticaret sicil gazetesi**

**Teknik kısıtlar:**

| | |
|---|---|
| Format | PDF, JPG, PNG |
| Boyut | Dosya başına 10 MB |
| Adet | En fazla 3 dosya (levha iki sayfa olabiliyor) |

---

## 4. Admin Tarafı — Doğrulama Kuyruğu

### Her satırda görünecek

- Şirket unvanı, vergi no, yetkili, telefon
- **Yüklenen belgeler** — tıklayınca yan panelde açılsın, indirmeye gerek kalmasın
- **Kayıt e-postası** (kurumsal mı Gmail mi — rozet kararı için)
- Başvuru tarihi

### Üç aksiyon

#### ✅ Onayla
- Şirket doğrulanır
- **Rozet belirlenir:** kurumsal e-posta varsa "Doğrulanmış şirket" rozeti, Gmail vb. ise rozetsiz onay
- İşverene bildirim/e-posta gider
- **İlan açma kilidi kalkar**

#### ❌ Reddet — sebep seçmeli
Hazır sebepler (elle yazma yok — hız ve tutarlılık için):

- Belge okunmuyor / net değil
- Belgedeki unvan girilen şirket adıyla uyuşmuyor
- Belge geçersiz veya eksik
- Vergi numarası doğrulanamadı
- Diğer *(serbest metin)*

Seçilen sebep işverene **aynen** gider.

#### ⏳ Ek bilgi iste
- Reddetmeden *"şu belgeyi de yükleyin"* diyebilme
- Durum **"incelemede"** kalır, işveren ek yükleme yapar
- **Bu buton olmazsa** her eksik için reddetmek zorunda kalırsınız → kötü deneyim

---

## 5. Ekstra Teyit (Elle, İlk Dönem)

Şirketin **genel telefonunu arayıp** *"İK'dan falan bey mi başvurdu"* diye sormak.
En güçlü doğrulama, ekstra maliyeti yok — ilk dönem zaten 20-30 şirketle konuşulacak.

Vergi no ayrıca **GİB sorgu ekranından** teyit edilebilir.

---

## 6. Teknik Notlar

### Depolama ve erişim
- Belgeler **Supabase Storage'da özel klasörde**
- **RLS:** işveren sadece kendi yüklediğini görür, admin hepsini
- Bu belgeler kişisel/ticari hassas veri → **herkese açık bucket'a konmaz**

### Loglama
Her admin aksiyonu **`admin_log`**'a yazılır: kim, ne zaman, hangi şirketi onayladı/reddetti.

### Saklama süresi (KVKK)
- Belgeyi süresiz saklamak savunulamaz
- **Onay sonrası 1 yıl** makul → sonra belge silinir
- **Onay kaydı kalır** (kim, ne zaman onayladı), belge gider

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

- Admin onay süresi hedefi — kullanıcıya "24 saat içinde" gibi bir söz verilecek mi?
- Onay sonrası şirket bilgisi değişirse (unvan, adres) yeniden doğrulama gerekecek mi?
- Başvuru sayısı artınca vergi no otomatik sorgulama entegrasyonu
- Aynı vergi numarasıyla ikinci bir hesap açılmak istenirse ne olacak? (şube / farklı İK kullanıcısı senaryosu)