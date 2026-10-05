# İş Eşleştirme Projesi — Kimlik Doğrulama (Auth) Kararları

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak
> Durum: Ekip toplantısı sonrası nihai karar
> Kapsam dışı: Ekip (sahip / yönetici / uzman) yönetimi → **Şirket Menüleri** dokümanında konuşulacak
> Kapsam dışı: Veritabanı tabloları → ayrı dokümanda hazırlanacak

---

## 0. Önceki Sürümden Değişenler

| Konu | Önceki | Şimdi |
|---|---|---|
| Giriş ekranı | Tek giriş, rol kayıtta seçiliyordu | **İki kapı: "İş arayan girişi" / "İşveren girişi"** |
| İşveren kaydı | Uygulamada kayıt ol → panelde belge yükle | **Giriş yapmadan başvuru formu** → admin onayı → hesap açılır |
| Onaysız işveren hesabı | Vardı (panel açık, yayın kilitli) | **Hiç yok** — sistemdeki her işveren hesabı onaylı |
| İşveren giriş yöntemi | E-posta + şifre (kendi belirliyordu) | **Sadece e-posta + şifre** — bilgiler onay mailiyle gelir |
| Şifremi unuttum | Belirtilmemişti | **Sıfırlama linki** (yeni şifre gönderilmez) |
| Telefon doğrulama | Yoktu | **SMS ile** — aday: başvuru ve davet kabulü; işveren: ilan yayını |
| Şirket telefonu | Tek alan | **Admin onayıyla doğrulanmış sayılır** |
| Kişisel profil | Yoktu | **"Profilim"** — şirket profilinden ayrı |
| Başvuru e-postaları | Tek e-posta | **Hesap e-postası + şirket e-postası** ("aynı" seçeneğiyle) |
| Rozet | Kurumsal e-posta + belge → rozetli | **Kaldırıldı** — tüm şirketler "✓ Doğrulanmış şirket" |

---

## 1. Temel İlkeler

**1 — Giriş sürtünmesiz, değer yaratan aksiyon kontrollü.**
Kimse kapıda takılmasın. Ama eşleşmeye götüren aksiyonlar — başvuru, davet kabulü, ilan yayını — telefon doğrulamasından geçer.

> Amaç sahte hesabı engellemek değil (bu her zaman kolaydır), **sahte hesapla bir şey yapmayı** zorlaştırmak.

**2 — Sistemde onaysız işveren yok.**
İşveren hesabı ancak admin onayından sonra oluşur. Bu tek karar; sahte şirket hesaplarını, onaysız-hesap kilitlerini ve bu kilitleri her ekranda kontrol etme yükünü ortadan kaldırıyor.

**3 — İki taraf iki ayrı kapıdan girer, iki ayrı uygulama deneyimi görür.**

**4 — Hiçbir ekran, bir e-postanın sistemde kayıtlı olup olmadığını söylemez.**
Yoksa biri e-posta deneyerek hangi kişilerin ve şirketlerin platformda olduğunu öğrenir.

---

## 2. Giriş Ekranı

Uygulama açıldığında kullanıcı önce kapısını seçer:

```
┌─────────────────────────────────┐
│                                 │
│   [ İş arayan girişi ]          │
│                                 │
│   [ İşveren girişi ]            │
│                                 │
└─────────────────────────────────┘
```

### İş arayan girişi — üç yöntem

```
┌─────────────────────────────────┐
│  [ Google ile devam et ]        │
│  [ Apple ile devam et ]         │
│  [ E-posta ve şifre ]           │
│                                 │
│  Şifremi unuttum                │
└─────────────────────────────────┘
```

- Hesap yoksa → **kayıt olur**
- Hesap varsa → giriş yapar

*iOS zorunluluğu: Google girişi sunulduğu için Apple girişi de olmak zorunda, yoksa App Store reddeder.*

### İşveren girişi — tek yöntem

```
┌──────────────────────────────┐
│  E-posta: [______________]   │
│  Şifre:   [______________]   │
│  [Giriş yap]                 │
│  Şifremi unuttum             │
│  ──────────────────────────  │
│  Hesabınız yok mu?           │
│  [İşveren olarak başvur]     │
└──────────────────────────────┘
```

- **Kayıt yok** — hesap sadece admin onayıyla açılır
- Giriş bilgileri onay mailiyle gelir
- **"İşveren olarak başvur"** bağlantısı her zaman görünür — sistem, e-posta yazılmadan kişinin hesabı olup olmadığını bilemez

**Neden işverende Google / Apple yok:**

Hesabı biz e-posta + şifre olarak açıyoruz. Google butonu sadece başvuruda verilen e-posta bir Google hesabıysa çalışırdı (Gmail veya Google Workspace); Outlook, Yandex veya şirket sunucusu e-postalarında çalışmazdı. Apple ise çoğu zaman gizli adres verdiği için neredeyse hiç eşleşmezdi.

Bazen çalışan, bazen "hesap bulunamadı" diyen bir buton kullanıcının kafasını karıştırır. Hesabı biz açıyoruz, giriş bilgisini biz veriyoruz — tek yöntem tutarlı. İşverenlerin çoğu zaten web panelinden girecek; orada tarayıcının şifre yöneticisi hatırlıyor.

### Özet

| | Giriş yöntemleri | Hesabı kim açar |
|---|---|---|
| **İş arayan** | Google / Apple / e-posta + şifre | Kendisi |
| **İşveren (sahip)** | Sadece e-posta + şifre | Biz — admin onayıyla |
| **İşveren ekip üyesi** | Sadece e-posta + şifre | Sistem — davetle *(Şirket Menüleri dokümanında)* |

### Yanlış kapı

| Durum | Mesaj |
|---|---|
| Aday hesabıyla işveren kapısından giriş | *"Bu hesap bir iş arayan hesabıdır."* **[İş arayan girişine git]** |
| İşveren hesabıyla iş arayan kapısından giriş | *"Bu hesap bir işveren hesabıdır."* **[İşveren girişine git]** |

### Hatalı giriş

Her durumda aynı mesaj:

> *"E-posta veya şifre hatalı."*

"Bu e-posta kayıtlı değil" **denmez** — İlke 4.

### Bir hesap = bir rol

Aynı e-posta hem aday hem işveren hesabında kullanılamaz.

> Mimari ileride "tek hesap, iki mod" desteğine açık bırakılıyor. MVP'de gerek yok — çoğu kişi iş için kurumsal, iş aramak için kişisel e-posta kullanıyor.

### Giriş sonrası

| İş arayan | İşveren |
|---|---|
| Aday Profili | İlanlarım |
| Keşfet | Adaylar / Keşfet |
| Başvurularım / Davetler | Eşleşmeler |
| Eşleşmeler | Şirket Profili |
| | Ekip *(sahip ve yönetici)* |
| | Profilim |

---

## 3. İş Arayan Kaydı

```
İş arayan girişi
  → Google / Apple / e-posta + şifre
  → Hesap oluşturulur
  → Kayıtta: ülke, arayüz dili, saat dilimi
  → Aday profili akışına geçer
```

### E-posta doğrulaması — zorunlu değil

E-posta + şifre ile kaydolan adaya doğrulama linki gider, ama **tıklamadan da profil doldurabilir.**

> **Neden zorunlu değil:** Asıl kontrol kapısı telefon (başvuru için zorunlu). E-posta doğrulamasını da zorunlu tutmak kayıtta gereksiz bir sürtünme olur.

Doğrulanmamışsa ekranın üstünde hatırlatma bandı:

> *"E-postanızı doğrulayın, yoksa bildirimleri alamayabilirsiniz."* **[Tekrar gönder]**

Google ve Apple ile kaydolanların e-postası zaten doğrulanmış sayılır.

### Global kapsam için kayıtta alınanlar

| Alan | Neden şimdi |
|---|---|
| **Ülke** | Katı filtrenin kökü, konum ağacının başlangıcı |
| **Arayüz dili** | Çok dilli arayüz |
| **Saat dilimi** | Bildirim zamanlaması |

Para birimi profilde sorulmaz — başvuru anında maaş beklentisiyle birlikte alınır.

### Telefon

Kayıtta zorunlu değil. Aday profilinde cep telefonu + **[Doğrula]** butonu. **Başvuru ve davet kabulü için zorunlu** (Bölüm 7).

---

## 4. Şifremi Unuttum

**İki kapıda da aynı.**

```
Şifremi unuttum
E-posta: [______________]  [Gönder]
        ↓
"Bu e-posta kayıtlıysa sıfırlama bağlantısı gönderildi."
        ↓
Maile link: [Yeni şifre belirle]
  • 1 saat geçerli
  • Tek kullanımlık
        ↓
Kişi yeni şifresini kendisi belirler
```

### Neden yeni şifre göndermiyoruz

"Şifremi unuttum" herkese açık bir buton. Yeni şifre gönderseydik:

```
Kötü niyetli biri: ayse@abc.com yazar → [Gönder]
  → Ayşe'nin şifresi DEĞİŞİR
  → Ayşe hiçbir şey yapmadan hesabına giremez hale gelir
```

Bunu her gün tekrarlayan biri, şifreyi çalmadan kullanıcıyı sürekli hesabından atabilir. İşveren tarafında — ilanları ve adayları bekleyen biri için — bu ciddi bir sorun.

**Link yönteminde eski şifre, link tıklanana kadar çalışmaya devam eder.** Biri kötü niyetle butona basarsa Ayşe sadece bir mail alır, hesabı etkilenmez.

> **Davet ve onay maillerindeki şifreyle farkı:** Onlar yetkili birinin (admin veya şirket yöneticisi) bilinçli işlemi. "Şifremi unuttum" ise herkesin basabileceği bir buton.

**Ek fayda:** Supabase bu akışı hazır veriyor. Sıfırdan şifre üreten bir sistem kurmak yerine sadece "yeni şifre belirle" ekranı tasarlanıyor.

### Özel durumlar

| Durum | Mail |
|---|---|
| Google ile kaydolmuş aday | *"Hesabınız Google ile açılmış. Giriş ekranında 'Google ile devam et'i seçin."* |
| Apple ile kaydolmuş aday | Aynı mesaj, Apple için |
| E-posta kayıtlı değil | **Mail gitmez**, ama ekranda yine aynı mesaj görünür (İlke 4) |

---

## 5. İşveren Başvurusu — Giriş Yapmadan

### Neden bu akış

| | Önceki tasarım | Yeni tasarım |
|---|---|---|
| Sistemde onaysız şirket | Vardı | **Hiç yok** |
| Sahte / boş şirket hesapları | Birikirdi | **Oluşmaz** |
| Onaysız hesap kilitleri | Her ekranda kontrol | **Gerek yok** |
| Onay öncesi ekip ekleme | Mümkündü — risk | **İmkansız** |

**Tek bedeli:** İşveren onay gelene kadar paneli göremez, ilan taslağı hazırlayamaz. Kabul edilebilir — zaten onaysız ilan yayınlayamıyordu.

### Nerede

**Herkese açık web sayfası** (Next.js). İşveren uygulama indirmeden başvurabilir; belge yükleme de masaüstünde daha rahat.

Girişler:
- İşveren giriş ekranındaki **[İşveren olarak başvur]**
- Web sitesinde doğrudan bağlantı

### Akış

```
1. Başvuru formu açılır
2. Ülke seçilir → o ülkenin form alanları ve belge listesi gelir
3. Şirket bilgileri, yetkili bilgileri, belgeler doldurulur
4. Yetkili cep telefonu SMS ile doğrulanır  ← doğrulanmadan gönderilemez
5. [Başvuruyu gönder]
6. "Başvurunuz alındı. Başvuru numarası: ISV-2026-00412
    Sonuç e-posta ile bildirilecek."
7. Admin kuyruğuna düşer
```

### Form alanları

**Şirket bilgileri**

| Alan | Zorunlu | Not |
|---|:---:|---|
| Ülke | ✅ | İlk soru — formun geri kalanını belirler |
| Şirket resmi unvanı | ✅ | Belgedeki unvanla eşleşmeli |
| Tescil / vergi numarası | ✅ | Etiketi ve formatı ülkeye göre (Türkiye'de vergi no, ABD'de EIN, AB'de VAT) |
| Tescil makamı / vergi dairesi | ✅ | Etiketi ülkeye göre |
| Adres | ✅ | Konum ağacından arama ile seçilir + açık adres |
| Şirket telefonu | ✅ | Santral olabilir — SMS ile doğrulanmaz, **admin onayıyla doğrulanır** |
| Şirket e-postası | ✅ | Resmi yazışmalar için — **"Hesap e-postasıyla aynı"** kutusu |
| Web sitesi | ❌ | |

**Yetkili bilgileri** *(hesabın sahibi olacak kişi)*

| Alan | Zorunlu | Not |
|---|:---:|---|
| Ad, soyad | ✅ | |
| Görev / unvan | ✅ | |
| **Hesap e-postası** | ✅ | **Giriş bilgileri buraya gidecek** — herhangi bir adres olabilir |
| **Cep telefonu** | ✅ | **Formda SMS ile doğrulanır** |

**Belgeler**

| | |
|---|---|
| Liste | Ülkeye göre (aşağıda) |
| Format | PDF, JPG, PNG |
| Boyut | Dosya başına 10 MB |
| Adet | En fazla 3 |

Formun sonunda not:

> *"Hesabınız açıldıktan sonra İK ekibinizi 'Ekip' menüsünden ekleyebilirsiniz."*

### İki e-posta — neden

```
Hesap e-postası:  [______________]  ← giriş bilgileri buraya gider
                                      (bu kişi şirketin sahibi olur)

Şirket e-postası: [______________]  ← resmi yazışmalar için
                  ☑ Hesap e-postasıyla aynı
```

"Aynı" işaretliyse ikinci alan gizlenir — küçük şirkete ek yük yok.

**Şirket e-postası ne işe yarar:**
- Belge yenileme hatırlatmaları
- Şirket hesabıyla ilgili kritik bilgilendirmeler
- Hesap sahibi şirketten ayrılır ve ulaşılamazsa, admin şirkete buradan ulaşır

**Adaylara gösterilmez** — sadece admin ve sistem kullanır.

> **Ayrı "İK yönetici e-postası" neden yok:** Bu kişi başvuranla aynı değilse, ya formda işlevsiz bir bilgi olarak kalır ya da Ekip özelliğini tekrarlar. İK yöneticisi, hesap açıldıktan sonra **Ekip → Yetkili ekle** ile eklenir.

### Kabul edilen belgeler — ülkeye göre

Liste veritabanında ülke bazlı tutulur; yeni ülke eklemek uygulama güncellemesi gerektirmez.

**Türkiye:** Vergi levhası, faaliyet belgesi, ticaret sicil gazetesi. Vergi no GİB sorgu ekranından teyit edilebilir.

**Diğer ülkeler:** Ticaret sicil kaydı, vergi kimlik belgesi (ABD'de EIN atama mektubu, Birleşik Krallık'ta Companies House kaydı, AB'de VAT kayıt belgesi, Almanya'da Handelsregisterauszug). Liste ülke açıldıkça doldurulur.

### Telefonun formda doğrulanmasının iki faydası

1. **Sahte başvuruyu ciddi azaltır** — admin kuyruğu çöple dolmaz
2. **Hesap açıldığında sahibin telefonu zaten doğrulanmış olur** — giriş yapar yapmaz ilan yayınlayabilir

Hesap e-postası ayrıca doğrulanmaz: giriş bilgileri o adrese gittiği için, giriş yapabilen kişi adresin sahibidir.

### Herkese açık formun korunması

Giriş gerektirmeyen bir form, botların hedefidir.

| Önlem | Ne yapar |
|---|---|
| **Görünmez CAPTCHA** | Bot gönderimlerini engeller (örn. Cloudflare Turnstile — kullanıcıya bulmaca çözdürmez) |
| **IP başına sınır** | Günde birkaç başvuru |
| **Güvenli belge yükleme** | İmzalı, süreli yükleme bağlantısı — depolama alanı herkese açılmaz |
| **Tekrar başvuru kontrolü** | Aynı tescil/vergi numarasıyla ikinci başvuru → *"Bu şirket için zaten bir başvuru veya hesap mevcut."* |
| **SMS sınırları** | Bölüm 7'deki sınırlar formda da geçerli |

---

## 6. Admin Onayı ve Hesap Açılışı

### Admin kuyruğu

Her başvuruda admin şunları görür:
- Ülke, unvan, tescil numarası, tescil makamı
- Yetkili bilgileri, hesap e-postası, şirket e-postası
- Şirket telefonu ve yetkili cep telefonu (✅ SMS doğrulanmış)
- Yüklenen belgeler — tıklayınca yan panelde açılır, indirmeye gerek yok
- Başvuru tarihi

### Üç aksiyon

**✅ Onayla**
- Şirket kaydı oluşturulur — **şirket profili onaylı**
- **Şirket telefonu doğrulanmış** işaretlenir (admin teyidi)
- Sahip hesabı oluşturulur — **cep telefonu doğrulanmış** (formda yapılmıştı)
- Güçlü rastgele şifre üretilir
- Hesap e-postasına onay maili gider

**❌ Reddet**
- Hazır sebeplerden biri seçilir (elle yazma yok — hız ve tutarlılık)
- Hesap e-postasına ret maili + **"Belgeleri tekrar yükle"** bağlantısı

**⏳ Ek bilgi iste**
- Başvuru "incelemede" kalır
- E-postaya ek yükleme bağlantısı gider

**Hazır ret sebepleri:**
- Belge okunmuyor / net değil
- Belgedeki unvan girilen şirket adıyla uyuşmuyor
- Belge geçersiz veya eksik
- Tescil / vergi numarası doğrulanamadı
- Bu ülke için kabul edilen bir belge değil
- Diğer *(serbest metin)*

**Ret ve ek bilgi bağlantıları:** Giriş gerektirmez, o başvuruya özeldir, sürelidir. Kişi bağlantıya tıklar, **sadece eksik belgeyi yükler** — formu baştan doldurmaz.

**Ekstra teyit (elle, ilk dönem):** Admin şirket telefonunu (santral) arayıp *"İK'dan falan bey mi başvurdu"* diye sorar. En güçlü doğrulama, maliyetsiz. Şirket telefonunun onayla birlikte "doğrulanmış" sayılmasının gerekçesi de bu.

> ⚠️ **Ölçek notu:** Elle onay global ölçekte tıkanır. İleride ülke bazlı otomatik tescil sorgusu (Birleşik Krallık Companies House ve AB VIES gibi açık sistemlerle başlanabilir) ve bölgesel admin ekipleri gerekecek. Mimari, elle onayı tek yol olarak varsaymamalı.

### Onay maili

```
Konu: İşveren hesabınız açıldı

ABC Şirketi işveren hesabınız onaylandı.

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
| **7 gün içinde hiç giriş yapılmazsa hesap askıya alınır** | Unutulan veya yanlış adrese giden mail, açık kapı olarak kalmaz. Admin "yeniden gönder" der, yeni şifre üretilir |
| **İlk girişte yumuşak hatırlatma** | *"Şifrenizi değiştirmek ister misiniz?"* **[Değiştir] [Şimdi değil]** — zorunlu değil |

### İlk giriş

```
İşveren girişi → e-posta + şifre
  → "Şifrenizi değiştirmek ister misiniz?" [Değiştir] [Şimdi değil]
  → İlanlarım ekranı
```

**Şirket onaylı, telefon doğrulanmış** — arada hiçbir bekleme veya kilit yok, doğrudan ilan açıp yayınlayabilir.

### Kayıtların saklanması

- **Başvuru kaydı silinmez** — hangi başvurudan hangi şirketin oluştuğu, kimin onayladığı denetim izi olarak kalır
- **Belgeler** onaydan 1 yıl sonra silinir, onay kaydı kalır. Saklama süresi ülkenin veri koruma rejimine göre değişebilir (KVKK, GDPR, diğer)

---

## 7. Telefon Doğrulama

### Hangi telefon, nasıl doğrulanır

| Telefon | Nerede | Nasıl doğrulanır |
|---|---|---|
| Aday cep telefonu | Aday Profili | **SMS** |
| İşveren kullanıcısı cep telefonu | **Profilim** | **SMS** — sahip için başvuru formunda yapıldı |
| Şirket telefonu (santral) | Şirket Profili | **Admin onayıyla** |

### Şirket Profili ve Profilim ayrı

```
🏢 Şirket Profili               👤 Profilim
─────────────────               ─────────────────
Logo, ad, sektör                Ad, soyad
Konum, lokasyonlar              Görev / unvan
Tanıtım, yan haklar             E-posta (giriş)
Şirket telefonu ✅              Cep telefonu ✅ [Doğrula]
Şirket e-postası                Şifre değiştir
Belgeler                        Bildirim tercihleri
```

**Şirket Profili:** Şirkete ait bilgiler. Admin onayıyla birlikte **onaylı** gelir.

**Profilim:** Giriş yapan kişiye ait. **Her kullanıcı** kendi profilini görür ve düzenler — cep telefonunu **aday gibi SMS ile** doğrular.

> **Neden ayrı:** Bir şirkette birden fazla kişi çalışır. Mehmet'in cep doğrulaması Mehmet'e, Can'ınki Can'a ait. Şirketin bilgisi ile kişinin bilgisi karışırsa kim neyi doğruladı belirsizleşir.

> **İsim neden "Profilim":** Bu ekranı sahip, yönetici ve uzman — herkes kullanacak. "Yönetici profili" ismi uzmanlar için yanıltıcı olurdu.

### SMS akışı — aday ve işveren için aynı

```
[ +90 532 123 45 67 ]  [Doğrula]
        ↓
"Telefonunuza gelen 6 haneli kodu girin"
[ _ _ _ _ _ _ ]
        ↓
[ +90 532 123 45 67 ]  ✅ Doğrulandı
```

### Doğrulamanın gerektiği yerler

| Kim | Aksiyon | Telefon doğrulanmış olmalı |
|---|---|:---:|
| Aday | Profil doldurma, Keşfet, ilan geçme | ❌ |
| Aday | **Başvuru** | ✅ |
| Aday | **Davet kabulü** | ✅ |
| İşveren kullanıcısı | Panel, ilan taslağı | ❌ |
| İşveren kullanıcısı | **İlan yayını** | ✅ |

**Aday akışı:**
```
[Başvur] veya [Daveti kabul et]
  → Telefon doğrulanmış mı?
       Evet  → aksiyon tamamlanır
       Hayır → "Devam etmek için telefonunuzu doğrulayın"
               SMS → doğrulandı → aksiyon KALDIĞI YERDEN devam eder
```

> **Davet kabulü neden dahil:** Dahil olmasa kural kolayca delinir — doğrulanmamış hesap, işverenin davetiyle eşleşip sohbete girer.
>
> **"Kaldığı yerden devam":** Doğrulamadan sonra aday tekrar "Başvur"a basmak zorunda kalmamalı. Bu küçük detay, doğrulamanın engel gibi hissettirmesini önler.

**İşveren akışı:**
```
[Yayınla]
  → Cep telefonum doğrulanmış mı?
       Evet  → ilan yayına girer
       Hayır → SMS doğrulama → ilan yayına girer
```

Şirket zaten onaylı olduğu için ilan yayınında **tek kontrol kişisel telefondur.**

### Numara değişikliği — iki tarafta aynı

```
Kullanıcı numarayı değiştirir → [Kaydet]
  → Doğrulama ANINDA kalkar
  → "Yeni numaranızı doğrulayın" [Doğrula]
  → SMS → doğrulanınca tekrar aktif
```

| | Doğrulayana kadar |
|---|---|
| **Aday** | Başvuru ve davet kabulü kilitli. Mevcut başvuru ve eşleşmeleri **etkilenmez** |
| **İşveren kullanıcısı** | Yeni ilan yayınlayamaz. Yayındaki ilanları **yayında kalır** |

**Şirket telefonu değişirse:** Sahip şirket profilinden değiştirirse doğrulama kalkar, değişiklik admin kuyruğuna düşer, admin teyit eder. İlanlar etkilenmez — sadece telefonun yanındaki ✅ kalkar.

### Numara tekilliği

| | Aynı numara birden fazla hesapta |
|---|:---:|
| **Aday** | ❌ Bir numara = bir aday hesabı |
| **İşveren tarafı** | ✅ Birden fazla kullanıcı aynı numarayı doğrulayabilir |
| **Aday + işveren** | ✅ Aynı numara bir aday ve bir işveren hesabında olabilir |

> **İşverende neden serbest:** Şirketlerde tek iş telefonu paylaşılabiliyor, ya da İK müdürü ekibin hesaplarını kendi cebiyle açabiliyor.

> **Sahtecilik sinyali:** Aynı numara **farklı şirketlerde** doğrulanmışsa engellenmez, ama **admin panelinde işaretlenir** — *"Bu numara 3 farklı şirkette doğrulanmış."* Gerçek bir durumsa (danışmanlık firması) admin geçer, değilse inceler.

> **Aday + işveren neden serbest:** Bir İK çalışanı kendi şirketinin hesabını yönetirken başka bir yerde iş arıyor olabilir. Kural rol bazlı: aday tarafında tek, işveren tarafında serbest — birbirini engellemez.

### "Bu numara benim" — aday tarafı

Numara başka bir aday hesabında doğrulanmışsa iki olası durum var: kişi eski hesabını unutup yenisini açmış, ya da operatör numarayı başka birine vermiş.

```
"Bu numara başka bir hesapta kayıtlı."
[Bu numara benim, doğrula]
  → SMS kodu gönderilir
  → Kod doğru girilirse:
       • Yeni hesapta numara AKTİF
       • Eski hesapta doğrulama PASİF (hesap silinmez)
       • Eski hesaba e-posta: "Telefon numaranız başka bir hesapta
         doğrulandı. Siz değilseniz bize ulaşın."
```

> SMS kodunu girebilen kişi numaranın **şu anki sahibidir.** Engel kalkıyor, ama aynı numara asla iki aday hesabında aynı anda aktif olmuyor.

Eski hesap başvuru yapmak isterse yeni bir numara doğrulaması gerekir.

### Numara formatı — standart format zorunlu

`0532 123 45 67`, `+90 532 1234567` ve `5321234567` aynı numara. Hepsi uluslararası standart formata (E.164) çevrilerek saklanır:

```
+905321234567
```

Bu yapılmazsa aynı numara farklı yazımlarla tekrar doğrulanır ve tekillik kuralı atlatılır.

Kullanıcı ülke kodunu ayrı seçer, numarayı yazar; sistem standart formata çevirir. Flutter'da `libphonenumber` tabanlı paket, Next.js'te `libphonenumber-js` bunu yapar.

### SMS kötüye kullanım koruması

> ⚠️ **"SMS pumping" dolandırıcılığı:** Botlar "Doğrula" butonunu yüksek ücretli ülke numaralarıyla binlerce kez tetikleyip SMS faturasını şişirir. Sık atlanan ve pahalı bir risk — özellikle herkese açık başvuru formunda.

| Sınır | Değer |
|---|---|
| Numara başına | Saatte en fazla **3** kod |
| Kullanıcı / IP başına | Günde en fazla **5** kod |
| Ülke | Sadece **lansman ülkelerine** SMS açık |

Sınır aşılınca:

> *"Çok fazla deneme yapıldı. Lütfen 1 saat sonra tekrar deneyin."*

**SMS servisi:** Global ölçekte **Twilio Verify** gibi hazır bir doğrulama servisi önerilir — kendi dolandırıcılık korumasını içerir. Yukarıdaki sınırlar, servisin korumasına **ek olarak** kendi tarafımızda uygulanır.

---

## 8. "Doğrulanmış Şirket"

> **Eski kural:** Kurumsal e-posta + belge → rozetli; sadece belge (Gmail vb.) → rozetsiz.

**Neden kaldırıldı:**

1. **Küçük şirketlerin çoğunun kurumsal alan adı yok.** Mahalle restoranı, küçük atölye, aile şirketi Gmail kullanıyor — cezalandırılmamalı.
2. **Sistemde onaysız işveren yok.** Adayın gördüğü **her** şirket admin tarafından onaylanmış. Herkes rozetliyse rozet bir şey ayırt etmiyor.

**Yeni:** Tüm şirket kartlarında aynı ibare:

```
✓ Doğrulanmış şirket
```

Aday karşılama ekranında:

> *"Platformdaki tüm şirketler belgeleriyle doğrulanmıştır."*

"Bazıları doğrulanmış" demekten çok daha güçlü bir güven mesajı. Bu, ürünün rakiplerden ayrıştığı bir nokta olarak tanıtımda da kullanılabilir.

---

## 9. Platform

| Ekran | Mobil (Flutter) | Web (Next.js) |
|---|:---:|:---:|
| İş arayan girişi ve kaydı | ✅ | ✅ |
| İşveren girişi | ✅ | ✅ |
| **İşveren başvuru formu** | — | ✅ *(sadece web)* |
| Şifremi unuttum / yeni şifre belirle | ✅ | ✅ |
| Telefon doğrulama | ✅ | ✅ |
| **Admin onay kuyruğu** | — | ✅ *(sadece web)* |

> **Başvuru formu neden sadece web:** İşveren uygulama indirmeden başvurabilmeli. Belge yükleme de masaüstünde daha rahat. Mobil uygulamadaki "İşveren olarak başvur" bağlantısı web formunu açar.

> **İş kuralları istemcide değil, sunucuda:** Telefon doğrulama kontrolü, SMS sınırları, başvuru onayı, hesap oluşturma, şifre üretimi — hepsi sunucu tarafında. Flutter ve Next.js sadece ekran çiziyor. Kural değişince tek yerde değişir; iki uygulama otomatik uyar.

---

## 10. Özet

| | İş arayan | İşveren |
|---|---|---|
| **Giriş kapısı** | "İş arayan girişi" | "İşveren girişi" |
| **Kayıt** | Uygulamada, kendisi | **Web başvuru formu → admin onayı → hesap açılır** |
| **Giriş yöntemleri** | Google / Apple / e-posta + şifre | **Sadece e-posta + şifre** |
| **İlk şifre** | Kendisi belirler | Onay mailiyle gelir, isterse değiştirir |
| **Şifremi unuttum** | Sıfırlama linki | Sıfırlama linki |
| **E-posta doğrulama** | Link gider, zorunlu değil | Gerek yok — giriş bilgisi o adrese gidiyor |
| **Cep telefonu** | Aday Profili'nde | Profilim'de *(sahip için formda yapıldı)* |
| **Şirket telefonu** | — | Şirket Profili'nde, admin onayıyla doğrulanmış |
| **Telefon ne zaman gerekir** | Başvuru, davet kabulü | İlan yayını |
| **Numara tekilliği** | Tek hesap | Paylaşılabilir |
| **Numara değişirse** | Doğrulama kalkar, tekrar doğrular | Aynı |
| **Onaysız durum** | — | **Yok** |

---

## 11. Açık Kalanlar

| # | Konu | Not |
|:---:|---|---|
| 1 | **Ekip** (sahip / yönetici / uzman, davet, devretme) | **Şirket Menüleri** dokümanında konuşulacak |
| 2 | **Veritabanı tabloları** | Ayrı dokümanda hazırlanacak |
| 3 | Admin onay süresi hedefi | Başvurana söz verilecek mi? (örn. "2 iş günü içinde") |
| 4 | Lansman ülkeleri | Belge listesi, SMS ülke izinleri ve admin kapasitesi buna bağlı |
| 5 | SMS servisi seçimi | Twilio Verify önerildi; maliyet karşılaştırması yapılmalı |
| 6 | Belge yenileme | Süresi dolan belgeler için periyodik hatırlatma gerekecek mi? |
| 7 | Şirket hesabını kapatma | Şirket Menüleri dokümanında konuşulacak |
| 8 | Çalışma izni / vize sponsorluğu | Global ölçekte gerçek bir katı filtre adayı |
| 9 | Telefonu doğrulanmamış aday, işverenin Keşfet'inde nasıl görünecek? | Keşfet bölümünde konuşulacak |

---

## 12. Diğer Dokümanlara Etkisi

| Doküman | Değişecek |
|---|---|
| **Şirket Doğrulama Ekranı** | **Büyük ölçüde geçersiz** — panel içi doğrulama yok. İçeriği bu dokümanın Bölüm 5-6'sına taşındı. "Admin — İşveren Başvuru Kuyruğu" olarak yeniden yazılabilir |
| **Şirket Profili** | "Doğrulanmış şirket" ibaresi; cep telefonu Profilim'e taşındı; şirket telefonu ve e-postası alanları; belge yenileme bölümü |
| **Aday Profili** | Telefon doğrulama, numara tekilliği, "bu numara benim", numara değişikliği; e-posta doğrulaması hatırlatma bandı |
| **İlan Oluşturma** | Yayın şartı artık sadece kişisel telefon doğrulaması; onaysız şirket kilidinin kaldırılması |
| **Eşleştirme Motoru** | Davet kabulünde telefon kontrolü; `ilan_kalitesi` skorundaki "şirket doğrulanmış" kriterinin kaldırılması (hep dolu olacağı için) |
