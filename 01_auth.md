# İş Eşleştirme Projesi — Kayıt ve Giriş Kararları

> Son güncelleme: 20 Eylül 2026

---

## 1. Aday (İş Arayan) Girişi

**Üç yöntem, hepsi açık — kullanıcı seçer:**

- Google ile giriş
- Apple ile giriş *(iOS zorunluluğu: Google varsa Apple da olmak zorunda, yoksa App Store reddediyor)*
- E-posta + şifre

**Telefon doğrulaması YOK.**
Telefon alanı profilde iletişim bilgisi olarak durur, doğrulanmaz.

**Gerekenler:**
- E-posta + şifre seçeneği için **şifre sıfırlama akışı** (Supabase hazır veriyor, ekranı yazılacak)
- E-posta ile kayıtta **doğrulama linki** gönderilmeli (yanlış yazılan adrese ulaşılamaz)

**Not (ileriye):**
Telefon doğrulaması olmadığı için toplu sahte profil açma riski var. Veri modelinde `telefon_dogrulandi_mi` kolonu bırakılacak — suistimal başlarsa açmak kolay olsun.

---

## 2. İşveren Girişi ve Şirket Doğrulama

### Akış

1. İşveren **istediği e-postayla** kayıt olur (Gmail, Hotmail de kabul)
2. Gelen **link ile e-postasını doğrular**
3. Giriş yapar, **panele girer**
4. Panelde **"Şirketinizi doğrulayın"** alanı var → vergi levhası, vergi no, şirket unvanı yükler
5. **Biz kontrol edip onaylarız** (admin panelinden)
6. Onaylanınca **ilan açabilir**

### Neden bu akış

Kapıda belge istemek caydırıcı olurdu. Bu modelde kişi önce sisteme giriyor, paneli görüyor, ne yapacağını anlıyor — sonra belge yüklüyor.

### Kritik kural: İlan açma kilidi onayda

**Onaysız hesap:**
- ✅ Panele girebilir
- ✅ Şirket profilini doldurabilir
- ✅ İlan taslağı hazırlayabilir
- ❌ İlanı **yayınlayamaz**
- ❌ **Aday göremez**
- ❌ **Mesaj atamaz**

> Bu kilit **RLS ile veritabanı seviyesinde** olmalı. Arayüzde gizlemek yetmez — uygulamayı inceleyen biri API'yi doğrudan çağırır.

### Arayüz detayları

**Durum şeridi (panelin üstünde, bariz):**
> "Şirketiniz henüz doğrulanmadı — ilan yayınlamak için belge yükleyin."

Kullanıcı neden ilan açamadığını anlamadan kalmasın.

**Bekleme süresini değerlendirme:**
> "Belgeniz incelenirken şirket profilinizi tamamlayabilirsiniz."

Onaylandığı anda ilan açmaya hazır olur.

**Ret durumunda:**
- Ret sebebi yazılmalı
- Tekrar yükleme hakkı verilmeli
- (Belge okunmuyordu, yanlış dosya yüklendi gibi durumlar sık olacak)

### Kabul edilen belgeler

- **Vergi levhası** (en yaygın, herkeste var — üstünde vergi no, unvan, adres yazıyor)
- Faaliyet belgesi
- Ticaret sicil gazetesi

Admin, belgedeki unvanla başvurudaki şirket adını karşılaştırır. Vergi no GİB sorgu ekranından teyit edilebilir.

**Ekstra teyit (elle, ilk dönem):**
Şirketin genel telefonunu arayıp "İK'dan falan bey mi başvurdu" diye sormak — en güçlü doğrulama, ekstra maliyeti yok.

### Rozet ayrımı

| Durum | Rozet |
|---|---|
| Kurumsal e-posta + belge | **Doğrulanmış şirket** rozeti |
| Sadece belge (Gmail vb.) | Normal onaylı, rozetsiz |

Aday kartta rozeti görür → güven sinyali. Şirket de rozet almak için kurumsal alan adına teşvik edilir.

---

## 3. Özet Tablo

| | Aday | İşveren |
|---|---|---|
| **Giriş** | Google / Apple / e-posta+şifre | E-posta + şifre (herhangi bir adres) |
| **E-posta doğrulama** | Var (link) | Var (link) |
| **Telefon doğrulama** | Yok | Yok |
| **Ek doğrulama** | Yok | **Belge yükleme + admin onayı** |
| **Kilit** | Yok | **Onaysız ilan açamaz** (RLS ile) |

---

## 4. Açık Kalanlar

- Aday tarafında e-posta doğrulama zorunlu mu, yoksa doğrulamadan da profil doldurulabilsin mi?
- Belge yükleme dosya formatları ve boyut sınırı
- Admin onay süresi hedefi (kullanıcıya "24 saat içinde" gibi bir söz verilecek mi?)
- Başvuru sayısı artınca vergi no otomatik sorgulama entegrasyonu