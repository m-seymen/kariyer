# İş Eşleştirme Projesi — Kayıt ve Giriş Kararları

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak

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

### Global kapsam için kayıtta alınacaklar

Sonradan eklenmesi pahalı olan alanlar baştan bulunmalı:

| Alan | Neden şimdi |
|---|---|
| **Ülke** | Katı filtrenin en çok eleyen kademesi; konum ağacının kökü |
| **Arayüz dili** | Çok dilli arayüz için |
| **Saat dilimi** | Bildirim zamanlaması |
| **Para birimi** | Maaş alanları için (profilde değil, başvuruda kullanılır) |

Ülke kaydı `konumlar` ağacının kök seviyesine (`seviye = 0`) bağlanır.

---

## 2. İşveren Girişi ve Şirket Doğrulama

### Akış

1. İşveren **istediği e-postayla** kayıt olur (Gmail, Hotmail de kabul)
2. Gelen **link ile e-postasını doğrular**
3. Giriş yapar, **panele girer**
4. Panelde **"Şirketinizi doğrulayın"** alanı var → resmi belge, vergi/tescil numarası, şirket unvanı yükler
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

### Kabul edilen belgeler — ülkeye göre değişir

Şirket kaydı **ülke seçimiyle başlar**, kabul edilen belge listesi o ülkeye göre gelir.

```sql
ulke_belge_tipleri (ulke_kodu, belge_tipi, ad, zorunlu_mu, sira)
```

Tabloda tutulur — yeni ülke eklemek uygulama güncellemesi gerektirmez.

**Türkiye:**
- Vergi levhası (en yaygın, herkeste var — üstünde vergi no, unvan, adres yazıyor)
- Faaliyet belgesi
- Ticaret sicil gazetesi

Vergi no GİB sorgu ekranından teyit edilebilir.

**Diğer ülkeler için karşılıkları:** ticaret sicil kaydı, vergi kimlik belgesi (ABD'de EIN mektubu, AB'de VAT kaydı, Birleşik Krallık'ta Companies House kaydı gibi). Her ülkenin kendi tescil sistemi var; liste ülke açıldıkça doldurulur.

**Genel kural:** Admin, belgedeki unvanla başvurudaki şirket adını karşılaştırır. Numara doğrulaması ülkenin resmi sorgu sistemi varsa oradan, yoksa belge üzerinden yapılır.

**Ekstra teyit (elle, ilk dönem):**
Şirketin genel telefonunu arayıp "İK'dan falan bey mi başvurdu" diye sormak — en güçlü doğrulama, ekstra maliyeti yok.

> ⚠️ **Ölçek notu:** Elle onay global ölçekte tıkanır. İlk dönem için doğru, ama ülke sayısı arttıkça (a) ülke bazlı otomatik tescil sorgusu entegrasyonları, (b) bölgesel admin ekipleri gerekecek. Şimdi yapılmayacak, ama mimari elle onayı tek yol olarak varsaymamalı.

### Rozet ayrımı

| Durum | Rozet |
|---|---|
| Kurumsal e-posta + belge | **Doğrulanmış şirket** rozeti |
| Sadece belge (Gmail vb.) | Normal onaylı, rozetsiz |

Aday kartta rozeti görür → güven sinyali. Şirket de rozet almak için kurumsal alan adına teşvik edilir.

### Şirket kaydında alınacak konum bilgisi

Şirket doğrulama formunda **ülke zorunlu** — hem belge listesini hem de sonraki lokasyon girişlerini belirler.

Şirketin fiziksel lokasyonları şirket profilinde (`sirket_lokasyonlari`) tutulur ve `konumlar` ağacına bağlanır.

---

## 3. Özet Tablo

| | Aday | İşveren |
|---|---|---|
| **Giriş** | Google / Apple / e-posta+şifre | E-posta + şifre (herhangi bir adres) |
| **E-posta doğrulama** | Var (link) | Var (link) |
| **Telefon doğrulama** | Yok | Yok |
| **Ek doğrulama** | Yok | **Belge yükleme + admin onayı** |
| **Kilit** | Yok | **Onaysız ilan açamaz** (RLS ile) |
| **Ülke** | Kayıtta alınır (zorunlu) | Doğrulama formunda alınır (zorunlu) |

---

## 4. Açık Kalanlar

- Aday tarafında e-posta doğrulama zorunlu mu, yoksa doğrulamadan da profil doldurulabilsin mi?
- Belge yükleme dosya formatları ve boyut sınırı
- Admin onay süresi hedefi (kullanıcıya "24 saat içinde" gibi bir söz verilecek mi?)
- Başvuru sayısı artınca ülke bazlı otomatik tescil/vergi no sorgulama entegrasyonları
- Hangi ülkelerle lansmana çıkılacak (belge listesi ve admin kapasitesi buna bağlı)
- Çalışma izni / vize sponsorluğu alanı eklenecek mi? *(Global ölçekte gerçek bir katı filtre adayı — aday tarafında "çalışma iznim var/yok", ilan tarafında "vize desteği veriyoruz")*
