# İş Eşleştirme Projesi — Aday Profili

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak
> **Sürüm: final** — ekip toplantısı, eşleştirme motoru ve işveren paneli kararları sonrası
> İlgili dokümanlar: `auth-kararlari.md`, `is-ilani-v3.md`, `staj-ilani.md`, `eslestirme-motoru.md`, `isveren-paneli-final.md`

---

## 0. Bu Sürümde Değişenler

| Konu | Önceki | Şimdi |
|---|---|---|
| **Telefon** | Doğrulanmıyordu | **SMS doğrulama — doğrulanmadan profil yayınlanamaz** |
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
| **Otomatik düşme** | 30-45 gün → fırsatlara açığım | **İki kademe:** 30 gün → fırsatlara açığım, 90 gün → şu an bakmıyorum |
| **Skor adı** | "Yetenek bileşeni" | **"Nitelik uyumu"** — yetenek + sertifika + bölüm |
| **Başvuru belge listesi** | Örnek sabit belgeler | **Profilden otomatik** — "Belgeler" (adayın verdiği adlarla) + "Sertifikalar" |
| **"Bir daha gösterme"** | Gizlediklerim listesi + [Geri al] | **Kalıcı** — ayrı liste yok, geri alınmaz |
| **Aday ekranları** | — | Alt menü, Başvurularım / Davetlerim (Bekleyen / Sonuçlanan), geri çekme, "Neden önerildi? ▾" — Bölüm 16 |
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
| Fotoğraf | 1 | ❌ | Yükleme / avatar | ❌ | ❌ | Kalite | ✅ |
| Ad, soyad | 1 | ✅ | Serbest | ❌ | ❌ | ❌ | ✅ |
| Doğum tarihi | 1 | ✅ | Tarih | ❌ | ❌ | ❌ | ❌ |
| **Cinsiyet** | 1 | ✅ | Kadın / Erkek | ❌ | ❌ **asla** | ❌ | ✅ *sadece detayda* |
| **Cep telefonu + SMS doğrulama** | 1 | ✅ | Ülke kodu + numara | ❌ | ❌ | ❌ | *İletişim isteği onaylanınca* |
| **E-posta** | 1 | ✅ | Giriş hesabından, değişmez | ❌ | ❌ | ❌ | *İletişim isteği onaylanınca* |
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
| Arama durumu | 12 | ✅ | 3 seçenek | ❌ | ✅ *"bakmıyorum" görünmez* | ❌ | ✅ *rozet* |

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
Arama durumu ("şu an bakmıyorum" görünmez)
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

### Fotoğraf

| | |
|---|---|
| Zorunlu | ❌ |
| Yüklenmezse | Baş harflerden renkli avatar |
| İşverene | ✅ |

### Ad, soyad

Zorunlu, işverene görünür.

### Doğum tarihi

| | |
|---|---|
| Zorunlu | ✅ |
| İşverene | ❌ **Gösterilmez** |
| Kullanım | **18 yaş kontrolü** + istatistik |

**Her ülkede 18+.** Kayıtta 18 yaşından küçükse:
> *"Platformu kullanabilmek için 18 yaşını doldurmuş olmalısınız."*

> **Neden işverene gösterilmez:** Kartta yaş yazması gereksiz eleme sebebi yaratır.

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
| **Doğrulama** | ✅ **SMS — doğrulanmadan profil yayınlanamaz** |
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
┌──────────────────────────────────────────────────┐
│ Örn: 5 yıldır e-ticaret alanında backend         │
│ geliştiriyorum. Ödeme sistemleri üzerine          │
│ çalıştım, mimari kararlara dahil olmak istiyorum. │
└──────────────────────────────────────────────────┘
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
> *"Bu şirketi gizli listenize eklemişsiniz. Başvurursanız şirket profilinizi görebilecek. Devam etmek istiyor musunuz?"*

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

Aday tarafında **profilde olmayan, başvuruya özel** bilgiler burada sorulur.

```
Başvuruyu onayla

Senior Backend Developer · ABC Teknoloji

Net minimum maaş beklentiniz (opsiyonel):  [______] [TRY ▾]
Not (opsiyonel):                            [______________]

Bu başvuruyla paylaşılacak belgeler:

Belgeler
☑ Özgeçmiş
☑ Portfolyo

Sertifikalar
☑ SRC Belgesi
☑ İSG Eğitimi

ⓘ Profiliniz ve paylaştığınız belgeler işveren tarafından
  görüntülenebilir ve indirilebilir.

[Başvur]
```

### Maaş beklentisi

| | |
|---|---|
| Zorunlu | ❌ |
| Biçim | **Net minimum** — tek rakam |
| Para birimi | Varsayılan: ilanın para birimi |
| Not | Opsiyonel — *"esnek olabilirim"*, *"yan haklara göre değişir"* |
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

> *"Bu şirketi gizli listenize eklemişsiniz. Başvurursanız şirket profilinizi görebilecek. Devam etmek istiyor musunuz?"*

### Davet kabulü

Aday daveti kabul ederken **aynı ekran** — maaş beklentisi + belge seçimi.

### Başvuru kaydına giren

Maaş beklentisi, para birimi, not, paylaşılan belgeler, başvuru tarihi ve **başvuru anındaki skor anlık görüntüsü** (uyum skoru, alt skorlar, eksik olmazsa olmazlar) — Başvurularım kartındaki "Neden önerildi? ▾" bu görüntüden gösterilir, sonuç tablosu temizlense bile doğru kalır.

### Telefon kontrolü

Yayındaki her profilin telefonu doğrulanmış (yayın eşiği). Başvuru ve davet kabulünde sunucu yine kontrol eder — numara değişmiş ve doğrulanmamışsa:
```
"Devam etmek için telefonunuzu doğrulayın"
SMS → doğrulandı → başvuru KALDIĞI YERDEN tamamlanır
```

---

## 16. Aday Ekranları — Özet

> Aday ekranlarının tamamı **aday dokümanında** *(yazılacak)* ayrıntılanacak. İşverenin karşı taraftaki ekranları ve kuralları → `isveren-paneli-final.md`. Burada sadece adayın göreceği yapı özetleniyor.

### Alt menü

```
🔍 Keşfet · 📨 Başvurularım · ✉️ Davetlerim · 🤝 Süreçteki İlanlar · 👤 Profilim
```

| Sekme | İçerik |
|---|---|
| **Keşfet** | Uygun ilanlar — uyum seviyesine göre; kartta **[Bir daha gösterme] [İncele →]**, detayda **[Başvur]** |
| **Başvurularım** | **Bekleyen / Sonuçlanan** — her kartta o anki durum |
| **Davetlerim** | **Bekleyen / Sonuçlanan** — detayda **[Kabul et] [Kabul etme]** |
| **Süreçteki İlanlar** | **Devam eden / Sonuçlanan** — sohbet, iletişim bilgisi paylaşımı, **[Süreçten çekil]** |
| **Profilim** | Profil adımları, arama durumu · **Ayarlar** ayrı bölüm (dil, saat dilimi, görünüm, bildirimler, güvenlik, gizlilik) |

### "Neden önerildi? ▾"

Tüm ilan kartlarında ve detaylarda açılır-kapanır bölüm — kapalı başlar. Başvuru ve davet kartlarında **başvuru / davet anındaki** skor gösterilir.

### "Bir daha gösterme"

| | |
|---|---|
| Ne yapar | O ilan adayın Keşfet'inden **kalıcı olarak** çıkar |
| Onay | *"Bu ilanı bir daha görmek istemediğinize emin misiniz? Bu işlem geri alınamaz."* |
| Liste | **Yok** — "Gizlediklerim" listesi ve [Geri al] kaldırıldı |
| İşveren | Etkilenmez — adayı görmeye ve davet etmeye devam edebilir |

> "Gizlenecek şirketler" (Adım 11) ile karıştırılmasın: o, adayın bütün bir şirketten saklanmasıdır.

### Başvuru durumları

| Durum | Adaya görünen |
|---|---|
| Bekliyor | ⏳ Değerlendiriliyor *(ilan duraklatıldıysa / süresi dolduysa notu: "İlan yayından kalktı")* |
| Kabul edildi | → Süreçteki İlanlar |
| Reddedildi | ✕ Başka adaylarla ilerlendi |
| İşveren sonucu bildirdi — **pozisyon doldu** | ⏹ Pozisyon doldu |
| İşveren sonucu bildirdi — **alımdan vazgeçti** | ⏹ Pozisyon iptal edildi |
| İşveren sonucu bildirdi — **uygun aday bulamadı** | ⏹ Süreç sona erdi *(nötr — "sen uygun değildin" diye okunmasın)* |
| Süresi doldu (30 gün) | ⌛ Yanıtlanmadan sona erdi |
| Geri çekildi | ↩ Başvurunu geri çektin |

> **İlanın yayından kalkması başvuruyu bitirmez** — duraklatılan veya süresi dolan ilanda başvuru işverenin kararına veya 30 günlük süreye kadar geçerlidir. Başvuru ancak işveren **ilanın sonucunu bildirince** toplu olarak biter.

- **[Başvurumu geri çek]** — Bekleyen kartında, onaylı; aynı ilana **tekrar başvurulamaz**
- Adaya **kalan gün gösterilmez** — sadece kaç gün önce başvurduğu
- Sonuçlananlar listede **90 gün** görünür; kartta **[Listeden kaldır]**
- Her başvurunun **ulaştığı en ileri aşama** kaydedilir (rapor için) — "işveren görüntüledi" bilgisi adaya **gösterilmez**

### Süreçteki İlanlar

| | |
|---|---|
| Süre | Süreç başladıktan **60 gün** içinde işveren sonuçlandırmalı — **kalan gün adaya gösterilmez** |
| **[Süreçten çekil]** | Aday istediği zaman çekilebilir — sohbet kapanır |
| Sonuçlanınca | Sohbet **kapanır** — geçmiş görünür, yeni mesaj yazılamaz |

| Sonuç | Adaya görünen |
|---|---|
| İşe alındı | 🎉 İşe alındın — **onay sorusu** *(aşağıda)* |
| Devam edilmedi | ✕ Başka adaylarla ilerlendi |
| Aday çekildi | ↩ Süreçten çekildin |
| 60 gün doldu | ⌛ Süreç sona erdi *(nötr)* |
| İlanın sonucu bildirildi | ⏹ Sonuca göre mesaj |
| Şirket hesabını sildi | ⏹ Şirket platformdan ayrıldı |

### İşe alım onayı

İşveren adayı *"işe alındı"* olarak işaretlerse:

```
🎉 ABC Teknoloji sizi Senior Backend Developer
   pozisyonu için işe aldığını bildirdi.
   Doğru mu?
   [Evet, işe alındım]        [Hayır]
```

| | |
|---|---|
| Cevap süresi | **14 gün** |
| Cevabı değiştirmek | **30 gün** içinde **[Cevabımı değiştir]** |
| *"Evet"* deyince | *"Arama durumunu 'Şu an bakmıyorum' yapmak ister misin?"* önerisi |
| Neden soruluyor | İşe alım platform dışında olur — iki tarafın da onaylaması, şirket raporunun ve platformun başarısının **gerçek ölçüsü** |

### Şirket profili ve işe alım raporu

Aday ilan detayından şirket profiline gider; profilde şirketin **son 12 aylık işe alım raporu** görünür — kaç ilan açtığı ve nasıl sonuçlandığı, başvuruların kaçına cevap verdiği, kaç kişiyi işe aldığı. En az 20 başvurusu olmayan şirkette *"🆕 Platformda yeni"*. Ayrıntı → `isveren-paneli-final.md` Bölüm 14.

---

## 17. Adım 12 — Önizleme, Arama Durumu, Yayınla

### 1 — Profil önizlemesi

İşverenin **izin almadan** görebildiği her şey — birebir onun ekranı:

```
Önizleme — İşveren seni böyle görür

┌──────────────────────────────────────────────┐
│ [Foto]  Ayşe Kaya                    🟢 Aktif │
│ Backend Geliştirici · Bilişim                 │
│ 📍 Kadıköy, İstanbul                           │
│ Çalışmak istediği: İstanbul, Almanya          │
│ Ofis · Hibrit · Tam / Yarı zamanlı            │
│ Başlayabilir: 1 ay içinde                     │
│                                               │
│ Kadın                                         │
│ "5 yıldır e-ticaret alanında backend..."      │
│                                               │
│ 🎓 Lisans · Bilgisayar Müh. · ODTÜ · Mezun     │
│ 💼 Backend Geliştirici · ABC Teknoloji        │
│    2022 – Halen · "Ödeme altyapısında..."     │
│ 🛠 React (5 yıl) · PostgreSQL (4 yıl)          │
│ 🌐 İş dilleri: Türkçe, İngilizce               │
│    Bildiği: İngilizce (İleri)                  │
│ 📜 SRC Belgesi (2023, süresiz)                 │
│ 📁 Projeler: Kargo takip uygulaması            │
│ 🚗 Sürücü belgesi: B · Seyahat engeli yok ·    │
│    Vardiyalı çalışabilir                      │
│ 🎖 Askerlik: Yapıldı                           │
└──────────────────────────────────────────────┘

🔒 İşverenin göremediği:
   Doğum tarihi · Telefon · E-posta
   CV ve belgeler (sadece başvuruda / süreçte, seçtiklerin)
   Sertifika dosyaları (sadece başvuruda / süreçte, seçtiklerin)
```

> Cinsiyet ve askerlik **Keşfet kartında değil, bu detay görünümünde.**

### 2 — Profil tamamlanma yüzdesi

```
Profilin %75 dolu
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
🟢 Aktif arıyorum   🟡 Fırsatlara açığım   ⚪ Şu an bakmıyorum
```

| Durum | Etkisi |
|---|---|
| 🟢 **Aktif arıyorum** | Rozet görünür |
| 🟡 **Fırsatlara açığım** | **Varsayılan** |
| ⚪ **Şu an bakmıyorum** | Keşfet'te **görünmez**, davet gelmez; **mevcut başvurular, davetler ve süreçteki ilanlar devam eder** |

**Hızlı değiştirilebileceği yerler:**

| Yer | Nasıl |
|---|---|
| Adım 12 | Yayınla butonunun üstünde |
| **Profil ana ekranı** | En üstte, tek dokunuş |
| **Keşfet ekranı** | Üst köşede durum rozeti — dokununca değişir |
| Ayarlar | Genel bölümünde |

"Şu an bakmıyorum" seçiliyse profil ana ekranında:
> *"Profilin gizli, işverenler seni göremiyor."* **[Aktif et]**

### 6 — Otomatik düşme — iki kademe

```
🟢 Aktif arıyorum
      │  30 gün giriş yok
      ↓
🟡 Fırsatlara açığım
      │  toplam 90 gün giriş yok
      ↓
⚪ Şu an bakmıyorum   → Keşfet'te görünmez, davet gelmez
```

| | |
|---|---|
| Kademe 1 | 30 gün giriş yoksa → **Fırsatlara açığım** |
| Kademe 2 | 90 gün giriş yoksa → **Şu an bakmıyorum** |
| Başlangıç durumu fark etmez | Zaten "fırsatlara açığım" olan da 90 günde düşer |
| Önceden haber | Her düşüşten **7 gün önce** bildirim — adayın saat dilimine göre |
| Mevcut başvurular, davetler ve süreçteki ilanlar | **Devam eder** |
| Geri dönünce | *"Profilin gizli. Tekrar iş aramaya başlamak ister misin?"* **[Aktif et]** — **otomatik açılmaz** |
| Süreler | Veritabanında — gerçek veriyle ayarlanabilir |

> **Neden:** Havuz ölü profillerle dolarsa işveren davet atar, cevap alamaz, platforma güveni gider. **Bu, ürünü sessizce öldüren şeylerden biri.**

> **Neden geri dönüşte otomatik açılmıyor:** Belki işe girmiş, sadece mesajlarına bakmak için girmiş. Kendisi karar verir.

### 7 — Yayınla

```
[Yayınla]
  → Zorunlu adımlar tamam mı?          Hayır → eksik adıma götür
  → Telefon doğrulanmış mı?             Hayır → SMS doğrulama
  → Evet → profil yayına girer
           embedding üretilir
           uygun ilanlar hesaplanır
```

---

## 18. Yayın Eşiği

**Adımlar taslak olarak kaydedilir** — aday yarıda bırakıp dönebilir. Ama aşağıdakiler tamamlanmadan **profil yayınlanamaz:**

| Adım | Zorunlu | Neden |
|---|:---:|---|
| **1 — Temel bilgiler** | ✅ | Konum, çalışma şekli ve **çalışma tipi** (filtre), tanıtım (embedding), cinsiyet |
| **Telefon doğrulaması** | ✅ | Sahte profilleri azaltır; başvuru ve davet için güvence |
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

| Doküman | Değişecek |
|---|---|
| **Eşleştirme Motoru** | ✅ İşlendi — `eslestirme-motoru.md` |
| **Bakım işleri** | Güncellenecek — `bakim-isleri.md`: süreç 60 gün, işe alım onayı 14 / 30 gün, ilan 30 + 45 gün kuralları (otomatik düşme, 1 yıl, 90 gün kuralları zaten yazılı) |
| **İş ilanı** | ✅ İşlendi — `is-ilani-v3.md`: çalışma tipi çoklu, "Algılanan" satırı, meslek alanı ve unvan kilidi |
| **Staj ilanı** | ✅ İşlendi — `staj-ilani.md` |
| **İşveren Paneli** | ✅ İşlendi — `isveren-paneli-final.md`: iletişim isteği, başvuru bazlı belgeler, PDF indirme, cinsiyet ve askerlik detayda görünür ama süzülemez, şirket raporu |
| **Aday dokümanı** *(yazılacak)* | Keşfet, Başvurularım, Davetlerim, Süreçteki İlanlar, işe alım onayı, Ayarlar ekranları |
| **Auth** | Aday telefon doğrulaması artık yayın eşiğinde (sadece başvuruda değil) |
| **Veri koruma** *(yazılacak)* | Cinsiyet zorunlu ve işverene görünür; belge indirme açık; iletişim bilgisi erişim kaydı; aydınlatma metninde açık bilgilendirme |
