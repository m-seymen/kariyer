# İş Eşleştirme Projesi — Aday Profili

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak
> **Sürüm: final** — ekip toplantısı, eşleştirme motoru, işveren paneli ve aday paneli kararları sonrası
> İlgili dokümanlar: `auth-kararlari.md`, `is-ilani-v3.md`, `staj-ilani.md`, `eslestirme-motoru.md`, `isveren-paneli-final.md`, `aday-paneli-final.md`
> **Bu dokümanın kapsamı:** Özgeçmiş'in içi — alanlar, adımlar, kurallar, eşleştirmeye etkisi. Aday ekranlarının tamamı (Keşfet, Başvurularım, Davetlerim, Süreçteki, Profilim, Ayarlar, bildirimler, hesap silme) → `aday-paneli-final.md`

---

## 0. Bu Sürümde Değişenler

| Konu | Önceki | Şimdi |
|---|---|---|
| **Telefon** | Doğrulanmıyordu | **SMS doğrulama — doğrulanmadan özgeçmiş yayınlanamaz** |
| **İletişim görünürlüğü** | Eşleşince otomatik | **Eşleşme sonrası işveren ister, aday onaylarsa** görünür |
| **E-posta** | Profilde alan | **Giriş hesabından gelir, değiştirilemez** |
| **Doğum tarihi** | Ülkeye göre çalışma yaşı | **Her ülkede 18+** |
| **Cinsiyet** | Opsiyonel, işverene gizli | **Zorunlu (kadın / erkek)**, işverene **sadece profil detayında** görünür, **hiçbir filtreye girmez** |
| **Yaşadığı konum** | Konum ağacı | Aynı — **açık adres alınmaz** |
| **Çalışmak istediği konumlar** | Sınırsız | **En fazla 10** |
| **Kısa tanıtım** | Opsiyonel | **Zorunlu**, min 50 / max 500, basit yer tutucu |
| **Çalışma tipi tercihi** | Yoktu — ilandaki çalışma tipi filtresi karşılıksız kalıyordu | **Yeni alan:** tam / yarı zamanlı / proje bazlı / dönemsel — **çoklu, zorunlu**, katı filtre |
| **İkincil meslek alanı** | Opsiyonel | **Kaldırıldı** — tek meslek alanı |
| **Eğitim** | Üniversite kapalı liste, not ortalaması, başlangıç yılı, "devam ediyorum" kutusu | Üniversite **öneri + serbest**; not ortalaması ve başlangıç yılı **kalktı**; **Durum: Devam ediyor / Mezun** |
| **Eğitim filtresi** | En yüksek ISCED ≥ ilan | **Tüm kayıtlar** — seviye + durum (ilandaki "kimler başvurabilir") |
| **Deneyim — şirket adı** | Onaylılar rozetli | Onaylılar **önce** — rozet yok |
| **Yetenekler** | Etiket + yıl | **Satır satır** ad + yıl, **opsiyonel**; unvana göre öneri yok |
| **Sertifikalar** | Ad + kurum + tarihler | **Belge dosyası zorunlu**, alınma tarihi zorunlu, geçerlilik veya **süresiz** |
| **CV** | Tek PDF | **Belge listesi** — ad + dosya (PDF/JPG/PNG), en fazla 10 |
| **Belge paylaşımı** | Tüm CV gider | **Başvuru / davet kabulünde aday seçer** (belgeler + sertifika dosyaları) |
| **Diğer bilgiler** | Opsiyonel | **Dört zorunlu seçim** — boş durum yok |
| **Askerlik** | Sadece bilgi, işverene görünür | **"Yükümlü değil"** eklendi; ilan şartlıysa **tecilli elenir**; işverene görünür, **süzülemez** |
| **Aktif araç sahibi** | Bilgi | **Kaldırıldı** |
| **Başlama zamanı** | Opsiyonel | **Zorunlu** |
| **Gizlenecek şirketler** | Şirketler | **Sadece onaylı şirketler**, otomatik öneri yok |
| **Otomatik düşme** | 30-45 gün → fırsatlara açığım | **İki kademe:** 30 gün → fırsatlara açığım, 90 gün → şu an iş aramıyorum |
| **Skor adı** | "Yetenek bileşeni" | **"Nitelik uyumu"** — yetenek + sertifika + bölüm |
| **Başvuru belge listesi** | Örnek sabit belgeler | **Profilden otomatik** — "Belgeler" (adayın verdiği adlarla) + "Sertifikalar" |
| **"Bir daha gösterme"** | Gizlediklerim listesi + [Geri al] | **Kalıcı** — ayrı liste yok, geri alınmaz |
| **Aday ekranları** | — | Ayrı doküman: `aday-paneli-final.md` — Bölüm 16'da özet |
| **Sekmenin adı** | Profilim | **Özgeçmiş** — işverenin gördüğü profil |
| **Hesap bilgileri** | Adım 1'de | **Profilim**'e taşındı — ad soyad, e-posta, telefon (SMS doğrulama), doğum tarihi *(kilitli)*, şifre · üst bardaki profil fotoğrafından açılır |
| **Arama durumu adları** | Aktif arıyorum / Şu an bakmıyorum | **Aktif iş arıyorum** · **Fırsatlara açığım** · **Şu an iş aramıyorum** — her sekmede üst barda |
| **Hesap silme** | — | Ayarlar → Hesabımı sil · anında · işverende *"Silinmiş kullanıcı"* olarak isimsiz kalır — `aday-paneli-final.md` Bölüm 15 |
| **Süreç** | Süresiz | **60 gün** — işveren sonuçlandırmazsa biter · aday **süreçten çekilebilir** |
| **İşe alım onayı** | — | İşveren *"işe alındı"* derse adaya *"Doğru mu?"* sorusu — Bölüm 16 |
| **İlanın sonucu** | İlan kapanınca *"ilan kapandı"* | İlanın yayından kalkması başvuruyu bitirmez · işveren **sonuç bildirince** biter, mesaj sonuca göre |
| **Profil PDF** | — | İşveren profilini ve paylaştığı belgeleri **PDF olarak indirebilir** — başvuru ekranında yazar |

---

## 1. Temel İlkeler

### 1.1 Aday profili eşleştirmenin tek aday kaynağı

| Kaynak | Eşleştirmeye giriyor mu? |
|---|:---:|
| **Aday profili alanları** | **✅ tek kaynak** |
| CV ve belgeler | ❌ |
| Sertifika dosyaları | ❌ |
| Şirket profili | ❌ |

> **CV neden girmiyor:** Herkes aynı kurallarla değerlendirilsin. *"Neye göre sıraladınız?"* → *"Adayın kendi girdiği yapılandırılmış alanlara göre."* İşe alımda otomatik karar düzenlemeleri (KVKK, GDPR md. 22, AB Yapay Zeka Yasası, NYC Local Law 144) karşısında savunması kolay.

### 1.2 Aday tarafında yapay zeka yok

İlan tarafında işverenin yazdığı cümleler yapay zekayla ayrıştırılıyor. **Aday tarafında yok** — aday yetenek, yıl, sertifikayı **kendisi yapılandırılmış girer.**

> **Neden asimetri:** İşveren bir ilan yazar, cümle kurması doğal. Aday kendini tanımlıyor; "React, 5 yıl" demek ona kolay. Ayrıca aday sayısı ilan sayısının katları — her aday kaydında yapay zeka maliyeti ve hata riski katlanırdı. Aday kendisi seçtiği için "yanlış anlaşılma" olmaz.

### 1.3 Serbest yazma + öneri

Unvan, okul adı, bölüm, şirket adı, pozisyon, yetenek, sertifika — yazmaya başlayınca öneri gelir; listede varsa seçer, yoksa yazdığı kaydedilir ve öneri havuzuna girer.

- Öneriler kullanıcının **diline göre** süzülür
- `normalize_ad` + metin benzerliği (`pg_trgm`) ile yazım farkları yakalanır
- Embedding çok dilli — "Buchhaltung" ile "muhasebe" yakın çıkar

**Kapalı liste sadece katı filtre alanlarında:** meslek alanı, konum, diller, eğitim seviyesi, çalışma şekli, çalışma tipi, diğer bilgiler.

### 1.4 "+ Ekle" deseni

Eğitim, deneyim, yetenek, dil, sertifika, proje, belge — hepsi liste. "+ Ekle" → satır / form → kaydet → listeye düşer.

### 1.5 Görmek ≠ süzmek

Bazı bilgiler işverene **görünür** ama işveren onlara göre **listeyi süzemez:**

| Bilgi | Görünür | Süzülebilir |
|---|:---:|:---:|
| Cinsiyet | ✅ Profil detayında | ❌ **Asla** |
| Askerlik durumu | ✅ Profil detayında | ❌ — sadece ilan şartı varsa tecilli elenir |
| Doğum tarihi | ❌ | ❌ |

> **Neden:** İşe alımda cinsiyete dayalı ayrımcılık Türkiye (İş Kanunu md. 5), AB ve ABD'de yasak. Platformun kendi filtresi bunu kolaylaştırırsa sorumluluk platforma da gelir.

---

## 2. Tüm Alanlar — Tek Tablo

| Alan | Adım | Zorunlu | Giriş | Embedding | Katı filtre | Skor | İşverene görünür |
|---|:---:|:---:|---|:---:|:---:|:---:|---|
| Fotoğraf | Profilim | ❌ | Yükleme / avatar | ❌ | ❌ | Kalite | ✅ |
| Ad, soyad | Profilim | ✅ | Serbest | ❌ | ❌ | ❌ | ✅ |
| Doğum tarihi | Kayıt · Profilim *(kilitli)* | ✅ | Tarih | ❌ | ❌ | ❌ | ❌ |
| **Cinsiyet** | 1 | ✅ | Kadın / Erkek | ❌ | ❌ **asla** | ❌ | ✅ *sadece detayda* |
| **Cep telefonu + SMS doğrulama** | Profilim | ✅ | Ülke kodu + numara | ❌ | ❌ | ❌ | *İletişim isteği onaylanınca* |
| **E-posta** | Profilim *(kilitli)* | ✅ | Giriş hesabından, değişmez | ❌ | ❌ | ❌ | *İletişim isteği onaylanınca* |
| Yaşadığı konum | 1 | ✅ | Konum ağacı | ❌ | ❌ | ❌ | ✅ |
| Çalışmak istediği konumlar | 1 | ✅¹ | Konum ağacı, çoklu, max 10 | ❌ | ✅ | ❌ | ✅ |
| Her konumda çalışmaya hazırım | 1 | ❌ | Kutu | ❌ | Konumu atlatır | ❌ | ✅ |
| Çalışma şekli | 1 | ✅ | Ofis / hibrit / uzaktan — çoklu | ❌ | ✅ | ❌ | ✅ |
| **Çalışma tipi tercihi** | 1 | ✅ | Tam / yarı zamanlı / proje bazlı / dönemsel — çoklu | ❌ | ✅ *iş ilanında* | ❌ | ✅ |
| **Kısa tanıtım** | 1 | ✅ | Serbest, min 50 / max 500 | ✅ | ❌ | ❌ | ✅ |
| **Meslek alanı** | 2 | ✅ | Kapalı liste, **tek** | ❌ | ✅ | ❌ | ✅ |
| Unvan | 2 | ✅ | Serbest + öneri | ✅ *ilk satır* | ❌ | ❌ | ✅ |
| Ne arıyor | 2 | ✅ | İş / staj — çoklu | ❌ | ✅ | ❌ | ❌ |
| **Eğitim — seviye + durum** | 3 | ✅ *en az 1 kayıt* | Kapalı liste | ❌ | ✅ | ❌ | ✅ |
| Eğitim — okul adı | 3 | ✅² | Öneri + serbest / serbest | ❌ | ❌ | ❌ | ✅ |
| **Eğitim — bölüm** | 3 | ✅² | Öneri + serbest / serbest | ✅ | ❌ | ✅ *nitelik* | ✅ |
| Eğitim — yıl | 3 | ✅² | Yıl | ❌ | ❌ | ❌ | ✅ |
| Deneyim — şirket adı | 4 | ✅³ | 3 kaynaklı öneri | ❌ | ❌ | ❌ | ✅ *gizlenmediyse* |
| Deneyim — pozisyon | 4 | ✅³ | Serbest + unvan önerisi | ✅ | ❌ | ❌ | ✅ |
| Deneyim — çalışma türü | 4 | ✅³ | Kapalı liste | ❌ | ❌ | ✅ *tecrübe* | ✅ |
| Deneyim — tarihler | 4 | ✅³ | Ay + yıl | ❌ | ❌ | ✅ *tecrübe* | ✅ |
| **Deneyim — ne yaptın** | 4 | ✅³ | Serbest, min 50 / max 500 | ✅ **en önemli** | ❌ | ❌ | ✅ |
| Yetenekler | 5 | ❌ | Ad (öneri + serbest) + yıl | ✅ *sadece ad* | ❌ | ✅ *nitelik* | ✅ |
| Bildiğim diller + seviye | 6 | ❌ | Kapalı liste | ❌ | ❌ | ❌ | ✅ |
| **İş dilleri** | 6 | ✅ | Kapalı liste, çoklu | ❌ | ✅ | ❌ | ✅ |
| Sertifika adı | 7 | ✅⁴ | Öneri + serbest | ✅ | ❌ | ✅ *nitelik* | ✅ |
| Sertifika — kurum | 7 | ❌ | Serbest | ❌ | ❌ | ❌ | ✅ |
| Sertifika — alınma tarihi | 7 | ✅⁴ | Ay + yıl | ❌ | ❌ | ❌ | ✅ |
| Sertifika — geçerlilik | 7 | ❌ | Tarih / süresiz | ❌ | ❌ | ✅ *geçersizse sayılmaz* | ✅ |
| **Sertifika — belge dosyası** | 7 | ✅⁴ | PDF / JPG / PNG | ❌ | ❌ | ❌ | *Başvuruda / süreçte, seçilirse* |
| Proje adı + açıklama | 8 | ✅⁵ | Serbest | ✅ | ❌ | ❌ | ✅ |
| Proje — link, teknolojiler, tarih | 8 | ❌ | | ❌ | ❌ | ❌ | ✅ |
| **CV ve belgeler** | 9 | ❌ | Ad + dosya, max 10 | ❌ | ❌ | ❌ | *Başvuruda / süreçte, seçilirse* |
| **Sürücü belgesi** | 10 | ✅ | Yok / Var + en az bir sınıf | ❌ | ✅ *ilan gerektiriyorsa* | ❌ | ✅ |
| **Askerlik durumu** | 10 | ✅ | Yapıldı / Muaf / Tecilli / Yükümlü değil | ❌ | ✅ *ilan şartlıysa — tecilli elenir* | ❌ | ✅ *detayda, süzülemez* |
| **Seyahat engeli** | 10 | ✅ | Yok / Var | ❌ | ✅ *ilan gerektiriyorsa* | ❌ | ✅ |
| **Vardiyalı çalışma** | 10 | ✅ | Evet / Hayır | ❌ | ✅ *ilan gerektiriyorsa* | ❌ | ✅ |
| **Başlama zamanı** | 11 | ✅ | Kapalı liste | ❌ | ❌ | ❌ | ✅ |
| Gizlenecek şirketler | 11 | ❌ | **Sadece onaylı şirketler** | ❌ | ✅ *işveren yönünde* | ❌ | ❌ |
| Arama durumu | Üst bar · 12 | ✅ | 3 seçenek | ❌ | ✅ *"Şu an iş aramıyorum" görünmez* | ❌ | ✅ *rozet* |

¹ *"Her konumda hazırım" işaretliyse değil*
² *Seviyeye göre — Bölüm 5'e bakın*
³ *Deneyim eklendiyse*
⁴ *Sertifika eklendiyse*
⁵ *Proje eklendiyse*

### Özet — hangi alan nereye gidiyor

**Embedding'e girenler (aday vektörü):**
```
Unvan                                  ← ilk satır
Kısa tanıtım
Deneyim: pozisyon + "ne yaptın"        ← en zengin kaynak
Eğitim: bölüm adları
Yetenek adları                         (yıl HARİÇ)
Sertifika adları                       (kurum, tarih HARİÇ)
Proje adı + açıklama                   (link, tarih HARİÇ)
```

**Katı filtreler:**
```
Çalışmak istediği konumlar (uzaktan / her konumda istisnası)
Çalışma şekli · Çalışma tipi (iş ilanında) · Meslek alanı · Ne arıyor · İş dilleri
Eğitim (seviye + durum — ilan istediyse)
Sürücü belgesi · Seyahat · Vardiya · Askerlik (ilan istediyse)
Gizlenecek şirketler (işveren yönünde)
Arama durumu ("Şu an iş aramıyorum" görünmez)
```

**Sadece skor:**
```
Yetenekler (ad + yıl) · Sertifikalar (geçerli olanlar) · Bölüm   → nitelik uyumu
Deneyim tarihleri + çalışma türü                                 → tecrübe bileşeni
Profil doluluğu                                                  → kalite bileşeni
```

**Hiçbir hesaba girmeyen:**
```
Fotoğraf (kalite hariç) · Ad · Doğum tarihi · Cinsiyet · Telefon · E-posta
Yaşadığı konum · Okul adı · Bildiğim diller · Başlama zamanı
CV ve belgeler · Sertifika dosyaları · Proje linkleri
```

---

## 3. Adım 1 — Temel Bilgiler

> **Hesap bilgileri Profilim'de:** Fotoğraf, ad soyad, doğum tarihi, cep telefonu ve e-posta **Özgeçmiş'in Adım 1'inde değil**, sağ üstteki profil fotoğrafından açılan **Profilim** ekranında düzenlenir *(`aday-paneli-final.md` Bölüm 3)*. Kuralları aşağıda aynen geçerli. Ad ve fotoğraf özgeçmişin en üstünde **görünür**; dokununca Profilim açılır.
>
> **Özgeçmiş Adım 1'de kalanlar:** Cinsiyet · yaşadığı konum · çalışmak istediği konumlar · her konumda hazırım · çalışma şekli · çalışma tipi · kısa tanıtım.

### Fotoğraf

| | |
|---|---|
| Zorunlu | ❌ |
| Nerede | **Profilim** — Profilim ve Özgeçmiş'te **aynı** fotoğraf |
| Yüklenmezse | Baş harflerden renkli avatar |
| İşverene | ✅ |

### Ad, soyad

Zorunlu, işverene görünür. **Profilim**'de düzenlenir.

### Doğum tarihi

| | |
|---|---|
| Zorunlu | ✅ |
| Nerede | **Kayıtta bir kez** alınır · Profilim'de **kilitli** görünür · yanlışsa Ayarlar → Bize ulaşın |
| İşverene | ❌ **Gösterilmez** |
| Kullanım | **18 yaş kontrolü** + istatistik |

**Her ülkede 18+.** Kayıtta 18 yaşından küçükse:
> *"Platformu kullanabilmek için 18 yaşını doldurmuş olmalısınız."*

> **Neden işverene gösterilmez:** Kartta yaş yazması gereksiz eleme sebebi yaratır.

> **Neden kilitli:** Serbestçe değişirse 18 yaş kontrolü anlamsızlaşır. Aday hakkında hangi bilgiyi tuttuğumuzu görebilsin diye Profilim'de görünür.

### Cinsiyet

```
Cinsiyet *:  ○ Kadın   ○ Erkek
```

| | |
|---|---|
| Zorunlu | ✅ |
| Seçenekler | Kadın / Erkek |
| İşverene | ✅ **Sadece profil detayında** — Keşfet kartında **yok** |
| Filtre | ❌ **Hiçbir yerde süzülemez** — ne işveren filtresi ne katı filtre |
| Embedding / skor | ❌ |
| Ek kullanım | Adalet denetimi — davet ve eşleşme oranları gruplar arasında dengeli mi |

> **Neden Keşfet kartında değil:** Kartta görünürse işveren nitelikleri görmeden cinsiyete göre hızlıca geçebilir — filtre olmasa bile fiilen aynı sonucu üretir. Detayda olunca işveren önce adayın niteliklerini görmüş olur.

> **Neden süzülemez:** İşe alımda cinsiyete dayalı ayrımcılık Türkiye, AB ve ABD'de yasak; platformun filtresi bunu kolaylaştırırsa sorumluluk platforma da gelir.

### Cep telefonu

```
Cep telefonu *:  [+90 ▾] [532 123 45 67]   [Doğrula]
                          ↓
              "Telefonunuza gelen 6 haneli kodu girin"
                          ↓
                 +90 532 123 45 67  ✅ Doğrulandı
```

| | |
|---|---|
| Zorunlu | ✅ |
| Nerede | **Profilim** → [Değiştir] · Özgeçmiş doğrulanmamışsa *"Telefonunu doğrula"* uyarısı gösterir, **[Doğrula]** Profilim'i açar |
| **Doğrulama** | ✅ **SMS — doğrulanmadan özgeçmiş yayınlanamaz** |
| Format | Ülke kodu ayrı; **E.164** ile saklanır (`+905321234567`) |
| İşverene | Sadece **iletişim isteği onaylanınca** (Bölüm 14) |
| Aynı numara | **Tek aday hesabı** |
| SMS sınırları | Numara başına saatte 3, kullanıcı başına günde 5 |

**Numara başka aday hesabında kayıtlıysa:**
```
"Bu numara başka bir hesapta kayıtlı."
[Bu numara benim, doğrula]
  → SMS → kod doğru girilirse:
       • Yeni hesapta numara AKTİF
       • Eski hesapta doğrulama PASİF (hesap silinmez)
       • Eski hesaba e-posta bildirimi
```

**Numara değişirse:**
- Doğrulama **anında kalkar**
- Profil **yayından çıkar** — Keşfet'te görünmez, davet gelmez
- Yeniden doğrulayınca profil geri döner
- Mevcut başvurular, davetler ve süreçteki ilanlar etkilenmez

### E-posta

| | |
|---|---|
| Kaynak | **Giriş hesabından** — Google, Apple veya e-posta + şifre |
| Nerede | **Profilim**'de kilitli görünür |
| Değiştirilebilir | ❌ |
| İşverene | Sadece **iletişim isteği onaylanınca** |

> **Apple "e-postamı gizle":** Adres `xyz@privaterelay.appleid.com` görünür. Adres çalışır — işverenin yazdığı e-posta adayın gerçek kutusuna yönlenir. Bilerek kabul edildi.

> E-posta + şifre ile kaydolanlara doğrulama linki gider ama **zorunlu değil** — ekranın üstünde hatırlatma bandı.

### Yaşadığı konum

```
Yaşadığın konum *:  [Kadıköy, İstanbul, Türkiye    ]   ← konum ağacından arama
```

| | |
|---|---|
| Zorunlu | ✅ |
| Giriş | Konum ağacı — aday istediği seviyede seçer |
| **Açık adres** | ❌ **Alınmaz** |
| İşverene | ✅ |
| Filtre / skor | ❌ — sadece gösterim |

> **Neden açık adres yok:** Hiçbir hesapta kullanılmıyor (mesafe hesabı yok), ev adresi en hassas kişisel verilerden biri, iş başvurusu aşamasında istemek güven kırıcı. İşverenin "aday gelip gidebilir mi" sorusuna ilçe düzeyi yeterli.

### Çalışmak istediği konumlar

```
Çalışmak istediğim konumlar *                         3 / 10
[Kadıköy, İstanbul ✕] [Ankara ✕] [Almanya ✕]
[+ Konum ekle                                        ]

☐ Her konumda çalışmaya hazırım
```

| | |
|---|---|
| Zorunlu | ✅ — "Her konumda hazırım" işaretliyse değil |
| Giriş | Konum ağacı — **global, çoklu** |
| Sınır | **En fazla 10** |
| Seviye | Aday seçer: ülke, il/eyalet, ilçe/şehir |
| Eşleştirme | **Katı filtre** — üst seviye seçilirse altı dahil |

| Aday seçimi | İlan konumu | Eşleşir |
|---|---|:---:|
| Almanya | München, Bayern, Almanya | ✅ |
| İstanbul | Kadıköy, İstanbul | ✅ |
| İstanbul | Çankaya, Ankara | ❌ |

> **Neden 10 sınırı:** 40 şehir ekleyen aday fiilen "her konumda hazırım" demiş olur. 10'dan fazla isteyen o kutuyu işaretlemeli.

### Her konumda çalışmaya hazırım

İşaretlenirse konum listesi gri düşer, **konum filtresi tamamen atlanır**. İşverene *"Her konumda çalışabilir"* olarak görünür.

> Bu aday tüm dünyadaki ilanlara düşer. Çalışma izni / vize alanı şimdilik yok — aday ilanı görür, kendisi değerlendirir.

### Çalışma şekli

```
Çalışma şekli *
☑ Ofis   ☑ Hibrit   ☐ Uzaktan
```

| | |
|---|---|
| Zorunlu | ✅ En az bir |
| Giriş | Çoklu |
| Eşleştirme | **Katı filtre** |

**Kural:**
> **Ortak seçenek yoksa ilan düşmez. Ortak seçeneklerde "uzaktan" varsa konum atlanır; yoksa konum uygulanır.**

| Aday | İlan | Sonuç |
|---|---|---|
| Uzaktan | Uzaktan | ✅ Konum **atlanır** |
| Uzaktan | Ofis | ❌ Düşmez |
| Ofis, Uzaktan | Ofis | ✅ Konum **uygulanır** |
| Ofis, Uzaktan | Ofis, Uzaktan | ✅ Konum **atlanır** |
| Hibrit | Hibrit | ✅ Konum **uygulanır** |

### Çalışma tipi tercihi — yeni alan

```
Çalışma tipi *
☑ Tam zamanlı   ☑ Yarı zamanlı   ☐ Proje bazlı   ☐ Dönemsel
```

| | |
|---|---|
| Zorunlu | ✅ En az bir — **yayın eşiğine girer** |
| Giriş | Çoklu |
| Eşleştirme | **Katı filtre** — ilanın çalışma tipiyle (o da çoklu) **en az bir ortak** |
| Staj ilanında | **Çalışmaz** — stajda "çalışma tipi" yok, "çalışma düzeni" var (sadece gösterim) |

| Aday | İlan | Sonuç |
|---|---|:---:|
| Tam zamanlı | Tam, Yarı | ✅ |
| Yarı zamanlı | Tam | ❌ |
| Tam, Proje bazlı | Proje bazlı | ✅ |

> **Neden eklendi:** İlanda çalışma tipi katı filtre olarak vardı ama aday profilinde karşılığı yoktu — filtre çalışamıyordu. Yarı zamanlı iş arayan öğrenciye tam zamanlı ilan, tam zamanlı arayana yarı zamanlı ilan düşmesin.

### Kısa tanıtım

```
Kendini kısaca tanıt *
┌───────────────────────────────────────────────────┐
│ Örn: 5 yıldır e-ticaret alanında backend          │
│ geliştiriyorum. Ödeme sistemleri üzerine          │
│ çalıştım, mimari kararlara dahil olmak istiyorum. │
└───────────────────────────────────────────────────┘
                                              0 / 500
```

| | |
|---|---|
| Zorunlu | ✅ |
| Uzunluk | **Min 50 / max 500** karakter |
| Yer tutucu | Basit, silik örnek — yazmaya başlayınca kaybolur; arayüz diline göre |
| İşverene | ✅ |
| **Embedding** | ✅ |
| Katı filtre / skor | ❌ |

> **Neden yer tutucu var (şirket profilinde yokken):** Aday tanıtımı **embedding'e giriyor**; iyi yazılması eşleşmeyi doğrudan iyileştiriyor. Şirket tanıtımı ise eşleştirmeye girmiyor.

---

## 4. Adım 2 — Meslek Alanı, Unvan, Ne Arıyor

### Meslek alanı

```
Meslek alanı *:  [Bilişim ve Yazılım ▾]
```

| | |
|---|---|
| Zorunlu | ✅ |
| Sayı | **Tek** |
| Giriş | Kapalı liste — 15-20 kalem, tek liste tüm ülkelerde, çevrilebilir (kaynak: ISCO-08, sadeleştirilmiş) |
| Eşleştirme | **Katı filtre** — `ilan.meslek_alani = aday.meslek_alani` |

**Örnek liste:** Bilişim ve Yazılım · Finans ve Muhasebe · Satış · Pazarlama ve İletişim · Tasarım · İnsan Kaynakları · Mühendislik · Üretim ve İmalat · Lojistik ve Depo · Sağlık · Eğitim · Hukuk · Müşteri Hizmetleri · Turizm, Otel ve Restoran · İnşaat ve Teknik İşler · Perakende ve Mağaza · Tarım · Yönetim …

> **Sektörden farkı:** Sektör (şirket profilinde) = şirket ne iş yapıyor. Meslek alanı = kişi ne iş yapıyor. Bir inşaat şirketi muhasebeci arayabilir — eşleşen şey meslek.

> **Neden embedding yetmiyor:** "Proje yönetimi, ekip koordinasyonu, bütçe takibi" yazan inşaat proje yöneticisi ile yazılım proje yöneticisi vektörde çok yakın çıkar. Meslek alanı olmasa birbirlerinin ilanlarına düşerler.

> **İkincil meslek alanı neden kaldırıldı:** Aday "Yazılım Geliştirici" olup ikincil olarak "Tarım" seçerse tarım ilanlarına düşer ama profili, deneyimi, tanıtımı yazılım anlatıyor — alakasız ilanlar görür, işveren de ona uymayan aday görür. **Kariyer değiştiren aday meslek alanını yeni alanına çevirir** ve profilini ona göre günceller; filtre ve vektör aynı yönü gösterir.

### Unvan

```
Unvan *:  [Yazılım Geliştirici          ]
           ┌────────────────────────────┐
           │ Yazılım Geliştirici        │
           │ Backend Developer          │
           └────────────────────────────┘
```

| | |
|---|---|
| Zorunlu | ✅ |
| Giriş | Serbest + öneri — *"Yazılımcı"*, *"Komi"*, *"Muhasebeci"* |
| Öneriler | Meslek alanına bağlı, kullanım sayısına göre, kullanıcının dilinde |
| **Embedding** | ✅ — aday vektörünün **ilk satırı** |
| Katı filtre | ❌ |

> **Neden filtre değil:** "Yazılım Mühendisi" ile "Backend Developer" farklı isim, aynı iş. Embedding yakalıyor.

### Ne arıyor

```
Ne arıyorsun? *
☑ İş   ☐ Staj
```

| | |
|---|---|
| Zorunlu | ✅ En az bir |
| Giriş | Çoklu |
| Eşleştirme | **Katı filtre** — ilan tipi seçimlerden birinde olmalı |

| İlan | Aday | Sonuç |
|---|---|:---:|
| İş | İş | ✅ |
| İş | Staj | ❌ |
| İş | İş + Staj | ✅ |
| Staj | Staj | ✅ |
| Staj | İş + Staj | ✅ |

---

## 5. Adım 3 — Eğitim

### Ekleme akışı

**"+ Eğitim ekle" → 1. Seviye seç → 2. Seviyeye göre form → 3. Kaydet**

```
Eğitim ekle

Sadece devam ettiğiniz veya mezun olduğunuz eğitimleri ekleyin.

Seviye *
○ İlkokul
○ Ortaokul
○ Lise
○ Ön lisans
○ Lisans
○ Yüksek lisans
○ Doktora
```

### Seviyeye göre alanlar

| Seviye | Okul adı | Bölüm | Durum | Yıl |
|---|---|---|:---:|:---:|
| **İlkokul / Ortaokul** | Serbest | — | — *(mezun sayılır)* | — |
| **Lise** | Serbest, öneri yok | Serbest, öneri yok — **opsiyonel** | ✅ | ✅ |
| **Ön lisans → Doktora** | **Öneri** — listede yoksa serbest | **Öneri** — listede yoksa serbest — **zorunlu** | ✅ | ✅ |

**Ön lisans ve üstü:**
```
Okul adı *:   [Orta Doğu Tek|                      ]
              ┌────────────────────────────────────┐
              │ Orta Doğu Teknik Üniversitesi       │
              │ Middle East Technical University    │
              └────────────────────────────────────┘
              Listede yoksa yazdığınız gibi kaydedilir

Bölüm *:      [Bilgisayar Mühendisliği            ]

Durum *:      ○ Devam ediyor   ● Mezun

Mezuniyet yılı *:  [2023 ▾]

[Kaydet]
```

**Lise:**
```
Okul adı *:      [Kadıköy Anadolu Lisesi        ]
Alan / bölüm:    [Bilişim Teknolojileri         ]   (opsiyonel)
Durum *:         ○ Devam ediyor   ○ Mezun
Yıl *:           [2018 ▾]
```

**İlkokul / Ortaokul:**
```
Okul adı *:      [_______________________]
```

**Yıl alanının adı duruma göre:**

| Durum | Yıl |
|---|---|
| Devam ediyor | Beklenen mezuniyet yılı |
| Mezun | Mezuniyet yılı |

> **Lisede bölüm neden opsiyonel:** Genel liselerde anlamlı bir bölüm yok. Meslek liselerinde ise alan önemli (Elektrik-Elektronik, Muhasebe, Bilişim) — yazılırsa embedding'e girer.

> **Lise / ortaokulda "devam ediyor" neden var:** Platform 18+ ama açık öğretim veya akşam lisesine devam eden yetişkinler var.

### Liste

```
Eğitim                                         [+ Eğitim ekle]

🎓 Yüksek lisans · Bilgisayar Mühendisliği
   Boğaziçi Üniversitesi · Devam ediyor (2026)          [Düzenle]

🎓 Lisans · Bilgisayar Mühendisliği
   ODTÜ · Mezun (2023)                                  [Düzenle]
```

En yeni üstte. **En az bir kayıt zorunlu** — "örgün eğitim kaydım yok" seçeneği **yok**. En kolay kayıt (ilkokul) sadece okul adı istiyor.

### Öğrenim durumu

**Öğrenim durumu = en yüksek kayıt.** Kartta işverene bu özet görünür:
```
🎓 Lisans · Bilgisayar Mühendisliği · Mezun
```

**Etiket ve seviye adları her dilde/ülkede o yerin kullandığı adla:**

| Dil | Etiket | Örnek |
|---|---|---|
| Türkçe | Öğrenim durumu | Lisans |
| İngilizce | Education level | Bachelor's degree |
| Almanca | Bildungsabschluss | Bachelor |

**Türkiye'deki 7 seviye ve ISCED karşılıkları:**

| Resmi kademe | Seviye | ISCED |
|---|---|:---:|
| Temel eğitim | İlkokul | 1 |
| | Ortaokul | 2 |
| Ortaöğretim | Lise | 3 |
| Yükseköğretim | Ön lisans | 5 |
| | Lisans | 6 |
| | Yüksek lisans | 7 |
| | Doktora *(tıpta uzmanlık, sanatta yeterlik dahil)* | 8 |

> Eski sistem mezunları ("ilköğretim", 5+3) ilkokul/ortaokul seviyesine denk gelir.

### Filtre — tüm kayıtlara bakar

İlanda "Kimler başvurabilir" (öğrenci / mezun / ikisi / şart yok) + "Hangi seviyede" var. Filtre **en yüksek kayda değil, tüm kayıtlara** bakar:

```
Aday:  Lisans — Mezun (2023)
       Yüksek lisans — Devam ediyor
```

| İlan | Sonuç |
|---|:---:|
| Sadece mezunlar · Lisans | ✅ *(lisans mezun kaydı var)* |
| Sadece devam eden öğrenciler · Yüksek lisans | ✅ *(Y.lisans devam kaydı var)* |
| Sadece devam eden öğrenciler · Lisans *(sadece lisans işaretli)* | ❌ |

> Sadece en yükseğe bakılsaydı bu aday "lisans mezunu" isteyen ilanlara düşmezdi.

| | Katı filtre | Embedding | Skor |
|---|:---:|:---:|:---:|
| Seviye + durum | ✅ | ❌ | ❌ |
| Bölüm | ❌ | ✅ | ✅ *(ilanın tercih ettiği bölümlerle)* |
| Okul adı | ❌ | ❌ | ❌ |

> **Okul adı neden hiçbir yere girmiyor:** Girerse aynı okuldan mezunlar birbirine yakın çıkar — okul bazlı önyargı. Global ölçekte belirli ülkelerin üniversiteleri sistematik avantaj kazanırdı.

### Kaldırılanlar

| Alan | Neden |
|---|---|
| Başlangıç yılı | Filtre sadece mezuniyet / beklenen yılı kullanıyor |
| Not ortalaması | Hiçbir yere girmiyor; ülkeler arası karşılaştırılamaz (4'lük, 20'lik, ters ölçek) |
| "Ayrıldı" durumu | Ekip kararı — iki durum yeterli; bırakılan eğitim eklenmez |
| Üniversite kapalı listesi | Okul adı hiçbir hesaba girmiyor; kapalı liste her ülke için büyük hazırlık ve "listede yok" sorunu demek |
| Mezuniyet hatırlatması | Ekip kararı — yok |

### Okul önerileri — kaynak

| Seviye | Öneri kaynağı |
|---|---|
| Ön lisans → Doktora | **ROR** (Research Organization Registry — açık, CC0, çok dilli isimler) + diğer açık listeler + adayların yazdıkları |
| Lise / ilkokul / ortaokul | Öneri yok — serbest yazma |

- Adayın ülkesindeki okullar önce gelir; yurt dışı okullar da aranabilir
- Seçilen okulun **ROR kimliği** saklanır — ileride gerekirse veri standart
- Alabildiğimiz kadar okul yüklenir; liste ne kadar geniş olursa olsun **serbest yazma her zaman açık**

### Global doğruluk — bilinen sınırlar

**Türkiye için %100'e yakın, global için %90+ doğru.** Kayabilecek noktalar:

| Sorun | Örnek |
|---|---|
| Birebir oturmayan dereceler | Almanya *Ausbildung*, İngiltere *A-levels / HND*, Fransa *Bac+2/+3/+5*, ABD *Associate / GED* |
| Ara seviye | ISCED 4 (lise sonrası mesleki eğitim) — mesleki eğitimin güçlü olduğu ülkelerde yaygın |
| Beyana dayalı | Platform eğitim belgesi doğrulamıyor |

**Sağlamlaştırma:**

| Önlem | |
|---|---|
| **Seviye adları ülkeye göre** | Aday kendi sisteminin adını görür, arkada doğru ISCED'e eşlenir |
| **Her seviyenin altında kısa açıklama** | *"Lisans: 3-4 yıllık üniversite diploması (Bachelor)"* |
| **Eşleme tablosu veritabanında** | Ülke × yerel derece → ISCED; hata bulunca tablo düzeltilir |
| **Her lansman ülkesinde yerel kontrol** | Ülke açılmadan eşleme yerel biriyle gözden geçirilir |

> **Form yapısı her yerde aynı** (seviye → okul → bölüm → durum → yıl). Sadece **seviye seçeneklerinin adları** ülkeye göre — aynı form, kendi dilinde/sisteminde.

---

## 6. Adım 4 — Deneyim

**Adım zorunlu değil** — yeni mezun, staj arayan için. **Eklenen her kayıtta alanlar zorunlu.**

### Ekleme formu

```
Deneyim ekle

Şirket adı *:      [ABC Tekn|                         ]
                   ┌──────────────────────────────────┐
                   │ ABC Teknoloji        (platformda) │  ← 1. onaylı şirketler
                   │ ABC Teknik Servis                 │  ← 2. adayların yazdıkları
                   └──────────────────────────────────┘
                   Listede yoksa yazdığınız gibi kaydedilir   ← 3.

☐ Şirket adını gizle

Pozisyon *:        [Backend Geliştirici            ]   ← unvan havuzundan öneri

Çalışma türü *:    [Tam zamanlı ▾]
                    Tam zamanlı · Yarı zamanlı · Staj ·
                    Freelance · Dönemsel · Gönüllü

Başlangıç *:       [Mart ▾] [2022 ▾]
Bitiş *:           [Ay ▾]   [Yıl ▾]
                   ☑ Halen burada çalışıyorum   → bitiş gri düşer

Bu işte ne yaptın? *
┌──────────────────────────────────────────────────┐
│ Ödeme altyapısında mikroservis geliştirdim,      │
│ günde 2 milyon işlemi taşıyan sistemin           │
│ performansını iyileştirdim.                      │
└──────────────────────────────────────────────────┘
                                            0 / 500
[Kaydet]
```

### Liste

```
Deneyim                                        [+ Deneyim ekle]

💼 Backend Geliştirici · ABC Teknoloji
   Tam zamanlı · Mart 2022 – Halen                       [Düzenle]
   "Ödeme altyapısında mikroservis geliştirdim..."

💼 Yazılım Stajyeri · Şirket adı gizli
   Staj · Haz 2021 – Ağu 2021                            [Düzenle]
```

### Alanlar

| Alan | Zorunlu | Embedding | Skor | İşverene |
|---|:---:|:---:|:---:|---|
| Şirket adı | ✅ | ❌ | ❌ | ✅ *gizlenmediyse* |
| Şirket adını gizle | ❌ | — | — | *"Şirket adı gizli"* |
| Pozisyon | ✅ | ✅ | ❌ | ✅ |
| Çalışma türü | ✅ | ❌ | ✅ *tecrübe ağırlığı* | ✅ |
| Başlangıç / bitiş | ✅ | ❌ | ✅ *tecrübe yılı* | ✅ |
| **Bu işte ne yaptın** | ✅ — **min 50 / max 500** | ✅ **en önemli kaynak** | ❌ | ✅ |

### Şirket adı — üç kaynaklı öneri

```
1. Platformdaki onaylı şirketler     ← önce, en üstte
2. Diğer adayların yazdığı şirketler ← kullanım sayısına göre
3. Yazdığı gibi kaydedilir           ← öneri havuzuna girer
```

- Tek arama kutusunda bu sırayla
- Yazılan ad sonradan diğer adaylara öneri olur
- O şirket ileride platforma onaylanırsa, isim benzerliğiyle **onaylı kayda bağlanır**
- Rozet yok — platformdaki tüm şirketler zaten onaylı

### "Bu işte ne yaptın" — bölümün en önemli alanı

CV eşleştirmeye girmiyor, şirket profili girmiyor. Adayın gerçek ayırt ediciliği bu metinlerden çıkıyor. Boş deneyim kaydı sisteme hiçbir şey katmaz — bu yüzden kayıt eklenince zorunlu.

### Toplam tecrübe — otomatik

Aday ayrıca yazmaz; tarihlerden hesaplanır:

| Çalışma türü | Ağırlık |
|---|:---:|
| Tam zamanlı, freelance | × 1.0 |
| Yarı zamanlı, staj, dönemsel | × 0.5 |
| Gönüllü | Sayılmaz *(profilde görünür)* |

Örtüşen dönemler iki kez sayılmaz.

### Şirket adını gizle

```
Şirket adı gizli
Backend Developer · 2022 – Halen
"Ödeme altyapısında mikroservis geliştirdim..."
```

> ⚠️ **Sınırlılık:** Pozisyon + tarih + açıklama o şirkette çalışan biri için tanınabilir olabilir. Asıl koruma **gizlenecek şirketler** (Adım 11).

---

## 7. Adım 5 — Yetenekler

**Opsiyonel** — hiç eklenmeyebilir.

```
Yeteneklerim                                      2 / 30

┌──────────────────────────────┐  ┌────────┐
│ React                        │  │ 5 yıl ▾│   🗑
└──────────────────────────────┘  └────────┘
┌──────────────────────────────┐  ┌────────┐
│ PostgreSQL                   │  │ 4 yıl ▾│   🗑
└──────────────────────────────┘  └────────┘

[+ Ekle]
```

| | |
|---|---|
| Zorunlu | ❌ |
| En fazla | 30 |
| Giriş | **[+ Ekle]** → yeni satır → yetenek adı + yıl |
| Yetenek adı | **Otomatik tamamlama** — listede varsa seçer, yoksa yazdığı kaydedilir |
| Yıl | Açılır liste: `<1 · 1 · 2 · 3 … 10+` — satır doluysa zorunlu |
| Seviye / yıldız | ❌ **Yok** — sadece yıl |
| Unvana göre öneri | ❌ **Yok** — kişi kendisi ekler |
| **Embedding** | ✅ **Sadece ad** (yıl girmez) |
| **Skor** | ✅ **Nitelik uyumu** (ad + yıl) |
| Katı filtre | ❌ |

> **Neden yıl:** İlan "React 5 yıl" isterse, 3 yıl bilen aday 1 yıl bilenin önüne geçer:
> ```
> React 3 yıl → 0.5 + 0.5 × 3/5 = 0.80
> React 1 yıl → 0.5 + 0.5 × 1/5 = 0.60
> ```

> **Neden yıldız yok:** Öz-değerlendirme güvenilmez; zamanla herkes 4-5 yıldız verir. "3 yıl React" ilanla doğrudan karşılaştırılabilir.

> **Neden yıl embedding'e girmiyor:** Sayılar vektör uzayında anlamsız — "React 4 yıl" ile "React 1 yıl" neredeyse aynı görünür.

> **Neden katı filtre değil:** "React zorunlu" diyen ilana React yazmamış ama yıllarca frontend yapmış aday da düşer — sadece skoru düşük olur.

> **Bilerek kabul edilen:** Yetenek girmeyen aday, yetenek isteyen ilanlarda nitelik uyumundan düşük alır (eşleşmenin %20'si). Elenmez, sıralamada geride kalır. **Eşleşme gücü** göstergesi bunu söyler.

**İki koldan eşleşme:**
1. **Skor:** ilanın etiketleriyle karşılaştırma — işverenin cümlesinden LLM ayrıştırır, işveren isterse düzeltir; yazım farkları benzerlik kontrolüyle yakalanır
2. **Embedding:** etiket tutmasa bile "React" ↔ "frontend geliştirme" yakınlığı

---

## 8. Adım 6 — Diller

### A — Bildiğim diller *(sadece gösterim)*

```
Bildiğim diller                                [+ Dil ekle]
İngilizce   [İleri ▾]       🗑
Almanca     [Başlangıç ▾]   🗑
```

| | |
|---|---|
| Zorunlu | ❌ |
| Giriş | Dil (kapalı liste) + seviye: Başlangıç / Orta / İleri / Anadil |
| Filtre / embedding / skor | ❌ |
| İşverene | ✅ |

> İlanda karşılığı yok (ilanda sadece "iş dili" var). Embedding'e girse dil bilen herkes birbirine yakın çıkardı.

### B — Çalışabileceğim iş dilleri *(katı filtre)*

```
İş dilleri *
[Türkçe ✕] [İngilizce ✕]   [+ Ekle]
```

| | |
|---|---|
| Zorunlu | ✅ En az bir |
| Giriş | Kapalı liste, çoklu, seviye yok |
| Varsayılan | **Adayın ülkesinin ana dili** dolu gelir |
| Otomatik | "Bildiğim diller"de **Anadil** seçilen dil otomatik eklenir — görünür, kaldırılabilir |
| Eşleştirme | **Katı filtre** — ilanla en az bir ortak dil |

> ⚠️ **En büyük tek risk:** Bu alan boş kalırsa aday hiçbir ilanla eşleşmez. Bu yüzden varsayılan dolu ve boş bırakılamaz.

> **Global işlevi:** Embedding çok dilli olduğu için Türkçe profil İngilizce ilanla eşleşebilir — ama aday o ilanı okuyamayabilir. Dil filtresi bunu önler.

---

## 9. Adım 7 — Sertifikalar

**Adım opsiyonel.** Eklenen her sertifikada:

```
Sertifika adı *:     [SRC Belgesi                ]   ← öneri + serbest
Veren kurum:         [_______________]                (opsiyonel)
Alınma tarihi *:     [Ay ▾] [Yıl ▾]
Geçerlilik bitişi:   [Ay ▾] [Yıl ▾]   ☐ Süresiz      (opsiyonel)
Belge *:             [📄 Dosya seç]   PDF, JPG, PNG · en fazla 5 MB
                                                      🗑
[+ Ekle]
```

| Alan | Zorunlu | Embedding | Skor |
|---|:---:|:---:|:---:|
| Sertifika adı | ✅ | ✅ | ✅ |
| Veren kurum | ❌ | ❌ | ❌ |
| **Alınma tarihi** | ✅ | ❌ | ❌ |
| Geçerlilik bitişi / süresiz | ❌ | ❌ | ✅ *geçerlilik kontrolü* |
| **Belge dosyası** | ✅ | ❌ | ❌ |

### Geçerlilik

| Aday ne girdi | Sistem |
|---|---|
| Bitiş tarihi, henüz geçmedi | ✅ Geçerli |
| Bitiş tarihi **geçmiş** | ❌ Skorda sayılmaz; kartta *"süresi geçmiş"* |
| "Süresiz" | ✅ Her zaman geçerli |
| Hiçbiri | ✅ Süresiz kabul edilir |

> Ad, geçerlilik fark etmeksizin embedding'e girer (etkisi küçük); **skor geçerliliğe bakar.**

### Skorda

İlanın **olmazsa olmaz** ve **tercih edilen** sertifika kutularıyla eşleşir — **nitelik uyumu** içinde.

```
İlan:   Olmazsa olmaz: [SRC belgesi]    Tercih edilen: [İSG eğitimi]
Aday:   SRC Belgesi — geçerli           İSG Eğitimi — süresi geçmiş

SRC  → olmazsa olmaz kümede → 1.0
İSG  → tercih edilen kümede → 0.0  (süresi geçmiş)
```

### Belge dosyası

| | |
|---|---|
| İşverene — Keşfet | ❌ Sadece sertifika adı |
| İşverene — Başvuranlar / Süreçteki Adaylar | ✅ Görüntüle + indir — **aday paylaşmayı seçtiyse** (Bölüm 15) |
| Şirkette kim görür | İlanın sorumlusu + sahip + yönetici |

> ⚠️ **Dosya yüklenmesi doğrulama değildir.** Platform belgeyi kontrol etmiyor. Kartta **"doğrulanmış" gibi bir ibare olmaz** — yanlış güven verir. İşveren dosyayı görüp kendisi değerlendirir.

> **Neden katı filtre değil:** İsimler tutmuyor — "PMP" / "Project Management Professional". Katı filtre olsa sertifikası olan aday da elenir.

---

## 10. Adım 8 — Projeler / Portföy

**Tamamen opsiyonel.**

```
Proje adı *:              [Kargo takip uygulaması          ]
Açıklama *:               ┌────────────────────────────────┐
                          │ Küçük kargo firmaları için     │
                          │ Flutter ile takip uygulaması.  │
                          └────────────────────────────────┘
Link:                     [https://github.com/...          ]
Kullanılan teknolojiler:  [Flutter] [Firebase]
Tarih:                    [Ay ▾] [Yıl ▾]
                                                           🗑
[+ Ekle]
```

| Alan | Zorunlu | Embedding |
|---|:---:|:---:|
| Proje adı | ✅ *(proje eklendiyse)* | ✅ |
| Açıklama | ✅ *(proje eklendiyse)* — 1-3 cümle | ✅ |
| Link | ❌ — sadece format kontrolü | ❌ |
| Kullanılan teknolojiler | ❌ — yetenek havuzundan otomatik tamamlama | ❌ |
| Tarih | ❌ | ❌ |

Katı filtre / skor: ❌

> **Kimler için kritik:** Deneyimi olmayan yeni mezun ve staj arayanlar. Embedding'e girecek metinleri az — proje açıklaması boşluğu doldurur.

---

## 11. Adım 9 — CV ve Belgelerim

**Opsiyonel.** Tek CV yerine **belge listesi:**

```
CV ve Belgelerim

Belge adı *:  [Özgeçmiş                    ]
Dosya *:      [📄 Dosya seç]   PDF, JPG, PNG · en fazla 5 MB
[Yükle]

📄 Özgeçmiş                     PDF   [Önizle] [İndir] [Sil]
📄 Portfolyo                    PDF   [Önizle] [İndir] [Sil]
📄 Referans Mektubu              JPG   [Önizle] [İndir] [Sil]

[+ Belge ekle]
```

| | |
|---|---|
| Zorunlu | ❌ |
| Belge adı | ✅ Serbest — aday verir |
| Format | PDF, JPG, PNG |
| Boyut | Dosya başına en fazla 5 MB |
| Adet | En fazla **10** |
| **Eşleştirme** | ❌ **Hiçbir yere girmez** |

### İşverene görünürlük

| Yer | Belgeler |
|---|---|
| Keşfet | ❌ |
| Davet gönderildi, cevap bekleniyor | ❌ |
| **Başvuranlar** | ✅ Görüntüle + **indir** — **sadece adayın o başvuruda seçtikleri** |
| **Süreçteki Adaylar** | ✅ Görüntüle + **indir** — **sadece seçilenler** |
| **Başvuru / süreç sonuçlanınca** (ret, süre dolması, geri çekme, işverenin sonuç bildirmesi, süreçten çekilme, 60 gün dolması) | ❌ **Erişim anında kapanır** — *işe alınan adayda devam eder* |

- Şirkette: **ilanın sorumlusu + sahip + yönetici** — diğer uzmanlar görmez
- **İndirme açık** — tek tek belge veya **profil + belgeler tek PDF** *(her sayfada şirket, indiren kişi ve tarih yazar · kim ne indirdi kaydedilir)* — aydınlatma metninde açıkça belirtilir (aday hesabını silse bile indirilmiş kopyalar işverende kalabilir) · ayrıntı → `isveren-paneli-final.md` Bölüm 10.2
- **Teknik:** Supabase Storage, özel bucket, RLS ile erişim

---

## 12. Adım 10 — Diğer Bilgiler

İlandaki "diğer şartlar"ın karşılığı. **Dört zorunlu seçim** — varsayılan seçili gelmez.

```
Diğer bilgiler *

Sürücü belgesi *:     ○ Yok
                      ○ Var → en az bir sınıf: [B] [C] [D] [E] ...

Askerlik durumu *:    ○ Yapıldı  ○ Muaf  ○ Tecilli  ○ Yükümlü değil

Seyahat engeli *:     ○ Yok   ○ Var

Vardiyalı çalışma *:  ○ Evet, çalışabilirim   ○ Hayır
```

| Alan | Zorunlu | Katı filtre — sadece ilan şart koyduysa | İşverene |
|---|:---:|---|:---:|
| Sürücü belgesi | ✅ — "Var" ise **en az bir sınıf** | İlan "gerekli + sınıf" → sınıf eşleşmeli | ✅ |
| Askerlik | ✅ | İlan "askerlik engeli olmamalı" → **tecilli elenir** *(sadece iş ilanında)* | ✅ *detayda, süzülemez* |
| Seyahat engeli | ✅ | İlan "seyahat engeli olmamalı" → "Var" elenir | ✅ |
| Vardiyalı çalışma | ✅ | İlan "vardiyalı çalışabilmeli" → "Hayır" elenir | ✅ |

**Filtre sadece ilanda şart varsa çalışır** — iş ilanında da staj ilanında da. İlan "gerekli değil" dediyse o alana bakılmaz.

> **Neden zorunlu:** Eskiden aday bu alanları boş bırakabiliyor, şartlı ilanlardan **sessizce** eleniyordu. Artık iki tarafta da seçim zorunlu — boş durum yok, filtre tek satır, "doldurmadın" uyarısına gerek kalmadı.

### Askerlik

- **"Yükümlü değil":** Kadın adaylar, askerliği olmayan ülkelerdeki adaylar, yabancı uyruklular — kimse kendini yanlış kategoriye koymak zorunda kalmaz
- Form **her ülkede aynı**
- İlan şartı **"askerlik engeli olmamalı"** — sadece **tecilli** elenir; yapıldı / muaf / yükümlü değil geçer
- **Staj ilanında askerlik şartı yok** — öğrencilerin çoğu tecilli
- İşverene **profil detayında görünür**, işveren **askerliğe göre süzemez**

### Sürücü belgesi sınıfları

Sınıf listesi ülkeye göre (AB'de B/C/D/E, ABD'de eyalet bazlı). Liste adayın ülkesine göre gelir.

### Kaldırılan

**Aktif araç sahibi** — ilanda karşılığı yoktu, hiçbir yere girmiyordu.

---

## 13. Adım 11 — Beklentiler

### Başlama zamanı

```
Ne zaman başlayabilirsin? *
○ Hemen   ○ 2 hafta içinde   ○ 1 ay içinde   ○ 2-3 ay içinde   ○ 3 ay+   ○ Esnek
```

| | |
|---|---|
| Zorunlu | ✅ |
| Giriş | Kapalı liste, tek seçim |
| Filtre / embedding / skor | ❌ |
| İşverene | ✅ — sadece gösterim |

> **Neden filtre değil:** İşveren "hemen başlayacak" diye süzüp, 1 ay sonra başlayabilecek iyi adayı kaybetmesin.

### Maaş beklentisi — profilde yok

Pozisyona özel olduğu için **başvuru anında** sorulur (Bölüm 15).

### Gizlenecek şirketler

```
Gizlemek istediğim şirketler

[ABC Tekn|                              ]
┌─────────────────────────────────────┐
│ ABC Teknoloji                        │  ← sadece onaylı şirketler
└─────────────────────────────────────┘

[ABC Teknoloji ✕]  [XYZ Lojistik ✕]
```

| | |
|---|---|
| Zorunlu | ❌ |
| Kaynak | **Sadece platformdaki onaylı şirketler** |
| Serbest yazma | ❌ — listede yoksa gizlenemez |
| Otomatik öneri | ❌ Yok — aday kendisi arar |
| Eşleştirme | **Katı filtre — işveren yönünde** |

> **Neden sadece onaylı şirketler:** Adayları sadece onaylı şirketler görebiliyor. Elle yazılmış bir şirket adının platformda hesabı yok — gizlemek işe yaramaz, eşleşme de yapılamaz.

**Tek yönlü çalışır:**

| | |
|---|---|
| İşveren adayı **göremez** | Keşfet'te çıkmaz, davet gönderemez, profile erişemez |
| Aday ilanları **görür** | Hatta başvurabilir |

> **Neden tek yönlü:** Aday mevcut işvereninin onu bulmasını istemiyor ama o şirketin ilanlarını görmekten rahatsız değil — kendi şirketinde başka pozisyona başvurmak isteyebilir.

**Gizlediği şirkete başvururken uyarı:**
> *"ABC Teknoloji'yi gizlediğin şirketler arasına eklemişsin. Başvurursan bu şirket özgeçmişini görebilecek."* **[Vazgeç]** **[Yine de başvur]**

- **RLS ile korunur** — gizlenen şirket adayı veritabanı seviyesinde okuyamaz
- Gizlenen aday şirketin **istatistiklerinde de sayılmaz**
- **Şirket bazlı** — şirketin tüm kullanıcıları (sahip, yönetici, uzman) için geçerli

---

## 14. İletişim Bilgilerinin Paylaşılması

Telefon ve e-posta **süreç başladıktan sonra bile otomatik görünmez.**

```
Süreç başladı → sohbet açılır
        ↓
İşveren: [📞 İletişim bilgisi iste]
        ↓
Aday: "ABC Teknoloji telefon ve e-posta bilginizi görmek istiyor."
      [Paylaş]   [Şimdilik hayır]
```

| Kural | |
|---|---|
| Aday kendiliğinden de paylaşabilir | Sohbette **[İletişim bilgimi paylaş]** |
| "Şimdilik hayır" | Sohbet devam eder, işveren sonra tekrar isteyebilir |
| Telefon + e-posta | **Birlikte** paylaşılır |
| **İlan bazlı** | ABC'nin "Backend" ilanında paylaşıldı diye, ABC'nin başka ilanının sorumlusu otomatik görmez |
| Şirkette kim görür | **İlanın sorumlusu + sahip + yönetici** — diğer uzmanlar görmez |
| Devredilirse | Yeni sorumlu görür · eski sorumlu **uzmansa** erişimi kalkar *(yönetici tüm ilanları gördüğü için görmeye devam eder)* · aday devri fark etmez |
| Erişim kaydı | Kim, ne zaman açtı — kayıt tutulur (KVKK/GDPR, kötüye kullanım) |

> **Neden otomatik değil:** Eşleşme zaten iki tarafın onayı, ama aday 10 şirketle eşleşirse 10 şirkete telefonunu otomatik vermiş olurdu. Aday kime vereceğine **kendisi** karar verir. Konuşma önce uygulama içinde kalır — platform dışına erken kaçış azalır.

---

## 15. Başvuru ve Davet Kabulü Anında

Aday tarafında **özgeçmişte olmayan, başvuruya özel** bilgiler burada sorulur. Ekranın tamamı → `aday-paneli-final.md` Bölüm 9 (başvuru) ve 11.5 (daveti kabul et).

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

### Maaş beklentisi

| | |
|---|---|
| Zorunlu | ❌ |
| Biçim | **Net, aylık, en az** — tek rakam |
| Para birimi | Varsayılan: ilanın para birimi, değiştirilebilir |
| İşverene not | Opsiyonel · en fazla **300** karakter · telefon, e-posta, bağlantı **yazılamaz** |
| Filtre | ❌ |
| İşverene | Başvuranlar listesinde kolon |

> **Neden profilde değil:** Maaş beklentisi pozisyona özel. Profilde tek rakam istemek adayı en alt sınırını yazmaya zorlar.

> **Kur çevrimi yapılmaz** — kur değişince geçmiş veriler bozulmasın.

### Belge paylaşımı

| | |
|---|---|
| Liste | **Otomatik** — profilde yüklenen belgeler (**adayın verdiği adlarla**) ve sertifika dosyaları (**sertifika adıyla**), iki grup halinde |
| Sabit belge türü | **Yok** — sistem hiçbir belge adı eklemez |
| Varsayılan | **Hepsi işaretli** — aday dokunmadan "Başvur" diyebilir |
| Aday seçebilir | İstediğinin tikini kaldırır |
| Hiç belgesi yoksa | Bu bölüm görünmez |
| Kayıt | **Başvuru bazlı** — işveren sadece o başvuruda paylaşılanları görür |
| Profilden silinen belge | Sonraki başvurularda çıkmaz; önceki başvuruda paylaşılan, o başvuru sonuçlanana kadar işverende kalır |
| Süresi geçmiş sertifika | Listede çıkar — aday paylaşmak isteyebilir; işveren dosyada tarihi görür |

> **Neden:** Mevcut işvereninden çekinen aday referans mektubunu her şirketle paylaşmak istemeyebilir; farklı pozisyonlar için farklı özgeçmiş yükleyen aday doğrusunu seçer.

### Gizlediği şirkete başvururken

> *"ABC Teknoloji'yi gizlediğin şirketler arasına eklemişsin. Başvurursan bu şirket özgeçmişini görebilecek."* **[Vazgeç]** **[Yine de başvur]**

### Davet kabulü

Aday daveti kabul ederken **aynı ekran** — başlık *"Daveti kabul et"*, en üstte işverenin davet notu, maaş beklentisi + not + belge seçimi. Kabul edilince süreç başlar; sohbette **işverenin notu ilk, adayın notu ikinci mesaj** olur.

### Başvuru kaydına giren

Maaş beklentisi, para birimi, not, paylaşılan belgeler, başvuru tarihi ve **başvuru anındaki skor anlık görüntüsü** (uyum skoru, alt skorlar, eksik olmazsa olmazlar) — Başvuru, davet ve süreç detaylarındaki **[Neden önerildi?]** penceresi bu görüntüden gösterilir, sonuç tablosu temizlense bile doğru kalır.

### Telefon kontrolü

Yayındaki her özgeçmişin telefonu doğrulanmış (yayın eşiği). Başvuru ve davet kabulünde sunucu yine kontrol eder — numara değişmiş ve doğrulanmamışsa:
```
"Devam etmek için telefonunu doğrula"
SMS → doğrulandı → başvuru KALDIĞI YERDEN tamamlanır
```

---

## 16. Aday Ekranları — Özet

> Aday ekranlarının tamamı — her ekran çizimi, her buton, akışlar, bildirim kataloğu — **`aday-paneli-final.md`**'de. Burada sadece yapının özeti var; ayrıntı için ilgili bölüme bakın.

### Ana yapı

```
┌──────────────────────────────────────────────────────────┐
│ [● Aktif iş arıyorum ▾]          (zil 3) (ayarlar) (AK)  │
├──────────────────────────────────────────────────────────┤
│                (seçili sekmenin içeriği)                 │
├──────────────────────────────────────────────────────────┤
│ Keşfet  Başvurularım  Davetlerim  Süreçteki  Özgeçmiş    │
└──────────────────────────────────────────────────────────┘
```

| Yer | İçinde | `aday-paneli-final.md` |
|---|---|:---:|
| Üst bar | Arama durumu · zil · dişli (Ayarlar) · profil fotoğrafı (Profilim) | 2 |
| **Keşfet** | Uygun ilanlar · kartta **[Bir daha gösterme] [İncele] [Başvur]** · Neden önerildi? | 6 |
| İlan detayı | İlanın tüm alanları · [Neden önerildi?] ve [Şirket profili] pencereleri · Paylaş · İlanı bildir | 7, 8 |
| **Başvurularım** | Bekleyen / Sonuçlanan · [Başvurumu geri çek] | 10 |
| **Davetlerim** | Bekleyen / Sonuçlanan · [Kabul et] [Kabul etme] · cevap süresi adaya gösterilir | 11 |
| **Süreçteki İlanlar** | Devam eden / Sonuçlanan · sohbet · [Süreçten çekil] · işe alım onayı | 12 |
| **Özgeçmiş** | Bu dokümandaki adımlar, önizleme, tamamlanma, yayınla | 13 |
| Profilim | Ad soyad, e-posta, telefon, doğum tarihi, şifre, çıkış | 3 |
| Ayarlar | Dil, saat dilimi, görünüm, bildirimler, oturumlar, verilerimi indir, yardım, **hesabımı sil** | 4, 15 |
| Bildirimler | Zil ekranı · aday bildirim kataloğu | 14 |

### Bu dokümanı ilgilendiren kurallar

| Konu | Kural |
|---|---|
| **"Bir daha gösterme"** | Kalıcı · "Gizlediklerim" listesi ve [Geri al] **yok** · sadece o ilan · işveren etkilenmez |
| **Başvurunun sonucu** | İlanın yayından kalkması başvuruyu bitirmez · işveren **sonuç bildirince** biter, adaya sonuca göre mesaj (*pozisyon doldu / iptal / nötr*) |
| **Süreç** | 60 gün · kalan süre adaya gösterilmez · aday **[Süreçten çekil]** diyebilir |
| **İşe alım onayı** | İşveren *"işe alındı"* derse adaya *"Doğru mu?"* · 14 gün cevap · 30 gün değiştirme |
| **Kalan gün** | Başvuruda ve süreçte adaya **gösterilmez** · davette **gösterilir** (karar adayda) |
| **Şirket profili** | Son 12 aylık işe alım raporu · 20 başvurudan az veride *"Platformda yeni"* |
| **Hesap silme** | Anında · kişisel bilgiler silinir · işverende *"Silinmiş kullanıcı"* olarak isimsiz kalır · istatistikler değişmez |

---

## 17. Adım 12 — Önizleme, Arama Durumu, Yayınla

### 1 — Özgeçmiş önizlemesi

İşverenin **izin almadan** görebildiği her şey — birebir onun ekranı:

```
Önizleme — İşveren seni böyle görür

┌─────────────────────────────────────────────────────────┐
│ [Foto]  Ayşe Kaya                 ● Aktif iş arıyorum   │
│ Backend Geliştirici · Bilişim                           │
│ Kadıköy, İstanbul                                       │
│ Çalışmak istediği: İstanbul, Almanya                    │
│ Ofis · Hibrit · Tam / Yarı zamanlı                      │
│ Başlayabilir: 1 ay içinde                               │
│                                                         │
│ Kadın                                                   │
│ "5 yıldır e-ticaret alanında backend..."                │
│                                                         │
│ Eğitim: Lisans · Bilgisayar Müh. · ODTÜ · Mezun         │
│ Deneyim: Backend Geliştirici · ABC Teknoloji            │
│    2022 – Halen · "Ödeme altyapısında..."               │
│ Yetenekler: React (5 yıl) · PostgreSQL (4 yıl)          │
│ İş dilleri: Türkçe, İngilizce                           │
│    Bildiği: İngilizce (İleri)                           │
│ Sertifika: SRC Belgesi (2023, süresiz)                  │
│ Projeler: Kargo takip uygulaması                        │
│ Sürücü belgesi: B · Seyahat engeli yok ·                │
│    Vardiyalı çalışabilir                                │
│ Askerlik: Yapıldı                                       │
└─────────────────────────────────────────────────────────┘

🔒 İşverenin göremediği:
   Doğum tarihi · Telefon · E-posta
   CV ve belgeler (sadece başvuruda / süreçte, seçtiklerin)
   Sertifika dosyaları (sadece başvuruda / süreçte, seçtiklerin)
```

> Cinsiyet ve askerlik **Keşfet kartında değil, bu detay görünümünde.**

### 2 — Profil tamamlanma yüzdesi

```
Özgeçmişin %75 dolu
Eksikler:  + Yetenek ekle   + Proje ekle
```

Her eksiğe tıklayınca ilgili adıma gider.

### 3 — Arama özeti

> *"Bilişim alanında, İstanbul ve Almanya'da, Türkçe ve İngilizce iş dilinde, ofis ve hibrit **iş ilanları** sana gösterilecek."*

> **Neden kritik:** Katı filtreler sessizce eler. Aday yanlışlıkla tek konum seçmiş veya bir iş dilini kaldırmışsa *"hiç ilan gelmiyor"* der ve sebebini bulamaz.

### 4 — Eşleşme gücü

```
Eşleşme gücün: Orta ●●○
```

Sadece gösterge — ek uyarı yok. *(Eski "sürücü belgesi / seyahat / vardiya doldurmadın" uyarısı kaldırıldı — bu alanlar artık zorunlu.)*

### 5 — Arama durumu

```
🟢 Aktif iş arıyorum   🟡 Fırsatlara açığım   ⚪ Şu an iş aramıyorum
```

| Durum | Etkisi |
|---|---|
| 🟢 **Aktif iş arıyorum** | Rozet görünür |
| 🟡 **Fırsatlara açığım** | **Varsayılan** |
| ⚪ **Şu an iş aramıyorum** | İşverenlerin listesinde **görünmez**, davet gelmez; **mevcut başvurular, davetler ve süreçteki ilanlar devam eder** · aday Keşfet'e bakmaya ve başvurmaya devam edebilir |

**Nerede değiştirilir:**

| Yer | Nasıl |
|---|---|
| **Her sekmede üst bar** | Solda arama durumu — dokununca pencere, her seçeneğin altında ne olacağı yazar *(`aday-paneli-final.md` 2.3)* |
| Adım 12 | Yayınla butonunun üstünde |

"Şu an iş aramıyorum" seçiliyse üst barın altında bant:
> *"Özgeçmişin gizli — işverenler seni göremiyor"* **[Aktif et]**

### 6 — Otomatik düşme — iki kademe

```
🟢 Aktif iş arıyorum
      │  30 gün giriş yok
      ↓
🟡 Fırsatlara açığım
      │  toplam 90 gün giriş yok
      ↓
⚪ Şu an iş aramıyorum   → işverenlerin listesinde görünmez, davet gelmez
```

| | |
|---|---|
| Kademe 1 | 30 gün giriş yoksa → **Fırsatlara açığım** |
| Kademe 2 | 90 gün giriş yoksa → **Şu an iş aramıyorum** |
| Başlangıç durumu fark etmez | Zaten "fırsatlara açığım" olan da 90 günde düşer |
| Önceden haber | Her düşüşten **7 gün önce** telefon + zil + e-posta — adayın saat diliminde sabah · **kapatılamaz** *(aday kataloğu 7.1, 7.2)* |
| Mevcut başvurular, davetler ve süreçteki ilanlar | **Devam eder** |
| Geri dönünce | Üst bar altında bant: *"Özgeçmişin gizli — işverenler seni göremiyor"* **[Aktif et]** — **otomatik açılmaz** |
| Süreler | Veritabanında — gerçek veriyle ayarlanabilir |

> **Neden:** Havuz ölü profillerle dolarsa işveren davet atar, cevap alamaz, platforma güveni gider. **Bu, ürünü sessizce öldüren şeylerden biri.**

> **Neden geri dönüşte otomatik açılmıyor:** Belki işe girmiş, sadece mesajlarına bakmak için girmiş. Kendisi karar verir.

### 7 — Yayınla

```
[Yayınla]
  → Zorunlu adımlar tamam mı?          Hayır → eksik adıma götür
  → Telefon doğrulanmış mı?             Hayır → SMS doğrulama (Profilim)
  → Evet → özgeçmiş yayına girer
           embedding üretilir
           uygun ilanlar hesaplanır
```

---

## 18. Yayın Eşiği

**Adımlar taslak olarak kaydedilir** — aday yarıda bırakıp dönebilir. Ama aşağıdakiler tamamlanmadan **özgeçmiş yayınlanamaz:**

| Adım | Zorunlu | Neden |
|---|:---:|---|
| **1 — Temel bilgiler** | ✅ | Konum, çalışma şekli ve **çalışma tipi** (filtre), tanıtım (embedding), cinsiyet |
| **Telefon doğrulaması** *(Profilim'de)* | ✅ | Sahte profilleri azaltır; başvuru ve davet için güvence |
| **2 — Meslek alanı, unvan, ne arıyor** | ✅ | Kaba filtre + embedding |
| **3 — Eğitim** *(en az bir kayıt)* | ✅ | Eğitim filtresi |
| 4 — Deneyim | ❌ | Yeni mezun için |
| 5 — Yetenekler | ❌ | Eşleşme gücüne yansır |
| **6 — İş dilleri** | ✅ | Boş kalırsa hiç eşleşme yok |
| 7 — Sertifikalar | ❌ | |
| 8 — Projeler | ❌ | |
| 9 — CV ve belgeler | ❌ | |
| **10 — Diğer bilgiler** | ✅ | Dört katı filtre — boş durum olmasın |
| **11 — Başlama zamanı** | ✅ | |

**Sadece eşik doldurulmuşsa embedding metni:**
```
Unvan: Backend Geliştirici
Kısa tanıtım: (min 50 karakter)
Bölüm: Bilgisayar Mühendisliği
```

> Kısa tanıtımın zorunlu olmasıyla, en minimal profilde bile embedding'in en az birkaç cümlelik metni oluyor.

---

## 19. Eşleştirmeye Etkisi — Skor

```
skor = 0.65 × anlamsal       ← aday vektörü ↔ ilan vektörü
     + 0.20 × nitelik uyumu  ← yetenek (ad + yıl) + sertifika (geçerli) + bölüm
     + 0.10 × tecrübe        ← deneyim tarihleri + çalışma türü (stajda nötr)
     + 0.05 × kalite         ← işveren yönünde: aday profil doluluğu
```

### Kalite — işveren yönü (aday profil doluluğu)

Zorunlu alanlar her profilde dolu olduğu için kriterler sadece **opsiyonel** alanlar:

| Kriter | Puan |
|---|:---:|
| En az bir deneyim kaydı | 0.30 |
| En az 5 yetenek | 0.25 |
| Proje veya sertifika var | 0.15 |
| Fotoğraf var | 0.15 |
| En az bir belge (CV vb.) yüklenmiş | 0.15 |
| **Toplam** | **1.00** |

> **Kaldırılan kriterler:** "Kısa tanıtım yazılmış" ve "eğitim kaydı tam" — artık zorunlu, her profilde dolu.

> Tüm ağırlıklar veritabanında — kod değişmeden ayarlanabilir. Formüllerin tamamı → `eslestirme-motoru.md`

---

## 20. Yayına Çıkmadan Hazırlanması Gerekenler

| Veri | Miktar | Kaynak |
|---|---|---|
| Konumlar | ~25 bin (nüfus 15K+) | **Geonames** |
| Meslek alanları | 15-20 + çevirileri | ISCO-08'den sadeleştirilmiş |
| Unvanlar | 300-500 × her lansman dili | ISCO-08 (çok dilli) |
| Yetenekler | 200-300 × her lansman dili | Elle |
| **Üniversiteler** | Alabildiğimiz kadar | **ROR** + açık listeler — öneri havuzu |
| Eğitim dereceleri | ISCED eşlemesi + ülke adları + açıklamalar | ISCED (BM) + her lansman ülkesinde yerel kontrol |
| Diller | 20-30 | ISO 639 |
| Sürücü belgesi sınıfları | Ülke başına | Elle |
| Para birimleri | ISO 4217 | Standart liste |

> ⚠️ **Boş öneri havuzu en büyük sessiz risk.** İlk 50 kullanıcı boş liste görürse herkes farklı yazar, veri dağılır.

> ⚠️ **Her yeni ülke / dil bir veri hazırlık işidir** — arayüz çevirisi, unvan ve yetenek havuzu, eğitim eşlemesi, sürücü sınıfları.

---

## 21. Diğer Dokümanlara Etkisi

| Doküman | Durum |
|---|---|
| **Eşleştirme Motoru** | ✅ İşlendi — `eslestirme-motoru.md` |
| **Bakım işleri** | ✅ İşlendi — `bakim-isleri.md`: süreç 60 gün, işe alım onayı 14 / 30 gün, ilan 30 + 45 gün, otomatik düşme uyarıları, aday silinince isimsiz kayıtlar |
| **İş ilanı** | ✅ İşlendi — `is-ilani-v3.md` |
| **Staj ilanı** | ✅ İşlendi — `staj-ilani.md` |
| **İşveren Paneli** | ✅ İşlendi — `isveren-paneli-final.md`: iletişim isteği, başvuru bazlı belgeler, PDF indirme, cinsiyet ve askerlik detayda görünür ama süzülemez, şirket raporu, silinmiş kullanıcı |
| **Aday Paneli** | ✅ Yazıldı — `aday-paneli-final.md`: tüm aday ekranları, Profilim, Ayarlar, bildirim kataloğu, hesap silme |
| **Auth** | Aday telefon doğrulaması artık yayın eşiğinde (sadece başvuruda değil) |
| **Veri koruma** *(yazılacak)* | Cinsiyet zorunlu ve işverene görünür; belge indirme açık; iletişim bilgisi erişim kaydı; aydınlatma metninde açık bilgilendirme |
