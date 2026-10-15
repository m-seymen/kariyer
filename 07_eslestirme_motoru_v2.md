# Eşleştirme Motoru

> **Kapsam:** Sadece motorun **nasıl çalıştığı** — arka plan.
> Kullanıcının ne gördüğü (kartlar, menüler, başvuru / davet akışı, ilan kapatma ekranı, bildirimler) → `kesfet-ve-surec.md`
> Zamanla çalışan işler (süreler, temizlik, otomatik düşme) → `bakim-isleri.md`
> **Kapsam: global** — uygulama tüm dünyaya açık · **Mimari: ilk günden 1 milyon+ kullanıcı**

---

## İçindekiler

1. Motor hangi sorunu çözüyor?
2. Motor hangi bilgileri kullanıyor?
3. İki araç: kesin kurallar ve anlam benzerliği
4. Motorun aşamaları
5. Embedding
6. İlan metinleri — iki yol ve LLM
7. Katı filtre
8. Vektör aşaması
9. Puan
10. Sonuç tablosu
11. Aksiyon tablosu
12. Hesaplama — ne zaman, nasıl
13. Kuyruk ve işçiler
14. Mimari ve ölçek
15. Yayından önce iki test
16. Veriyle ayarlanacaklar

---

## 1. Motor Hangi Sorunu Çözüyor?

Platformda iki taraf var:

| Taraf | Elinde ne var | Ne istiyor |
|---|---|---|
| **Aday** | Profili | *"Bana uygun ilanları göster"* |
| **İşveren** | İlanı | *"İlanıma uygun adayları göster"* |

Motor bu ikisini buluşturur:

```
GİRDİ                          MOTOR                         ÇIKTI
Aday profilleri  ─┐                                   ┌─▶ Adaya: kendisine uygun ilanlar, sıralı
                  ├──▶  eşleştir + sırala  ──────────┤
İlanlar          ─┘                                   └─▶ İşverene: ilanına uygun adaylar, sıralı
```

İki soruyu cevaplar:
1. **Uygun mu?** — Bu aday bu ilana hiç uyar mı? *(konum, meslek, dil…)*
2. **Ne kadar uygun?** — Uyanlar arasında kim daha iyi?

> **Neden önemli:** Kullanıcı ilan aramıyor, filtre kurmuyor — **Keşfet'i açıyor ve hazır liste görüyor.** Listenin kalitesi = ürünün kalitesi.

Bir işe alım uzmanının kafasındaki sürecin aynısı:

| Uzman ne yapar | Motor ne yapar |
|---|---|
| *"İstanbul'da değil, eleyelim"* | **Katı filtre** — kesin şartlara uymayanı eler |
| *"Bu profiller bu işe benziyor"* | **Vektör** — anlam olarak en yakın 200'ü seçer |
| *"Bu ikisinden hangisi daha uygun?"* | **Puan** — kalanları sıralar |
| Listeyi masaya hazır koyar | **Sonuç tablosu** — kullanıcı açınca anında hazır |

---

## 2. Motor Hangi Bilgileri Kullanıyor?

Motor **sadece formdaki alanlara** bakar:

| Kaynak | Eşleştirmeye giriyor mu | Neden |
|---|:---:|---|
| **Aday profilinin alanları** | ✅ | Herkes aynı formu doldurdu — eşit şartlar |
| **İlanın alanları** | ✅ | İşverenin ne istediği burada |
| CV ve yüklenen belgeler | ❌ | Biri 10 sayfa yüklüyor, biri hiç — eşit değerlendirilmez. *"Neye göre sıraladınız?"* sorusunun cevabı net: *"Adayın kendi girdiği alanlara göre"* |
| Sertifika dosyaları | ❌ | Sertifikanın **adı** profilde var, o kullanılıyor; dosya işverenin görmesi için |
| Şirket profili | ❌ | Eşleştirme **ilan bazlı** — aynı şirketin muhasebe ilanı ile yazılım ilanı birbirine benzemesin |

> Dosyalar ve şirket tanıtımı **insanın görmesi** için var, motorun değil.

---

## 3. İki Araç: Kesin Kurallar ve Anlam Benzerliği

| Araç | Soru | Örnek |
|---|---|---|
| **Kesin kurallar** | *"Uygun mu?"* — evet / hayır | İstanbul'da mı? Bilişim alanında mı? Türkçe biliyor mu? |
| **Anlam benzerliği** | *"Ne kadar benziyor?"* — derece | *"Ödeme sistemleri geliştirdim"* ↔ *"Ödeme altyapımızda çalışacaksınız"* |

**Sadece kesin kurallar olsaydı:** İlan "React" istiyor, aday "React.js" ya da "frontend geliştirme" yazmış → kaçar. Kelimeler tutmayınca iyi aday kaybolur.

**Sadece anlam benzerliği olsaydı:** İstanbul ilanına Ankara'daki aday düşer; muhasebe ilanına "bütçe yönetimi" yazan yazılımcı düşer. Kesin şartlar sessizce çiğnenir.

**İkisi birlikte:**
```
Kesin kurallar   → "Bu 800 kişi şartlara uyuyor"
Anlam benzerliği → "Bu 800 kişiden şu 200'ü işe en yakın"
```

---

## 4. Motorun Aşamaları

```
HAZIRLIK — sadece metin değişince, bir kez
  1. EMBEDDING     Metni "anlam koordinatına" çevirir         (aday + ilan)
  2. AYRIŞTIRMA    İlan cümlesini etiket listesine çevirir     (sadece ilan, LLM)

ÇALIŞMA — her eşleştirmede
  3. KATI FİLTRE   5.000.000 aday → ~500        "Uygun mu?"       → eler
  4. VEKTÖR        ~500 → en benzer 200          "En yakınlar"
  5. PUAN          200 kişiyi puanlar (iki yön)  "Ne kadar uygun?" → sıralar
  6. SONUÇ TABLOSU Hazır liste — ekran buradan okur, hesap yapmaz
```

**Neden bu sıra — ucuzdan pahalıya:**

| Aşama | Havuz | Maliyeti |
|---|---|---|
| Katı filtre | Milyonlar | Çok ucuz — basit evet / hayır |
| Vektör | Yüzler | Orta — anlam karşılaştırması |
| Puan | 200 | Ucuz ama detaylı — 4 parçalı hesap |

Milyonlarca aday için detaylı puan hesaplamak imkânsız olurdu. Önce kaba eleme, sonra ince ayar.

**Örnek:**
```
İlan: İstanbul, Bilişim, Backend Developer, Türkçe

5.000.000 aday
  → Katı filtre: İstanbul'da çalışabilen, Bilişim alanında, Türkçe bilen → 800
  → Vektör: anlam olarak "backend geliştirme"ye en yakın 200
  → Puan: React, .NET var mı? Tecrübe uygun mu? → 0.87, 0.84, 0.79 …
  → Sonuç tablosu: işveren "Adaylar"ı açınca sıralı liste anında gelir
```

---

## 5. Embedding

### 5.1 Nedir

**Embedding, bir metni "anlam koordinatına" çevirmek.** Her metin bir anlam haritasında bir noktaya yerleşir; **anlamı yakın metinler birbirine yakın düşer.**

```
                      🗺 Anlam haritası

   "Ödeme altyapısında          "Ödeme sistemlerimizde
    mikroservis geliştirdim" ●──● dağıtık mimari"
                                    ↑ yakın — kelimeler farklı, anlam aynı

   "Restoranda garsonluk" ●

                                         ● "Muhasebe ve bordro"
```

Teknik olarak bu koordinat **1024 sayılık bir liste** (vektör). İki liste arasındaki yakınlık → **0 ile 1 arası benzerlik.**

| Durum | Kelime eşleşmesi | Embedding |
|---|:---:|:---:|
| "Mikroservis" ↔ "dağıtık mimari" | ❌ | ✅ |
| "Yazılım Mühendisi" ↔ "Backend Developer" | ❌ | ✅ |
| "Muhasebe" ↔ "Buchhaltung" (Almanca) | ❌ | ✅ |

**Çok dilli model** — Türkçe profil Almanca ilanla karşılaştırılabilir, çeviri gerekmez.

**Ne zaman üretilir:** Sadece **metin değiştiğinde, bir kez.** Her eşleştirmede yeniden hesaplanmaz.

### 5.2 Adaydan girenler

| # | Alan | Ne girer | Zorunlu mu |
|:---:|---|---|:---:|
| 1 | **Unvan** | *"Backend Geliştirici"* — **ilk satır** | ✅ |
| 2 | **Kısa tanıtım** | Tamamı | ✅ |
| 3 | **Deneyim** | Her kayıtta **pozisyon + "bu işte ne yaptın"** | Varsa |
| 4 | **Eğitim** | Tüm kayıtların **bölüm adları** | Ön lisans ve üstünde ✅ |
| 5 | **Yetenekler** | Sadece **adları** | Varsa |
| 6 | **Sertifikalar** | Sadece **adları** | Varsa |
| 7 | **Projeler** | **Adı + açıklaması** | Varsa |

**Kural:** Ne doluysa girer, boş olan girmez. Deneyimi olmayan yeni mezunun vektörü unvan + tanıtım + bölüm + (varsa) proje ile oluşur — ek telafi kuralı yok.

**Örnek — adayın embedding'e giden metni:**
```
Backend Geliştirici
5 yıldır e-ticaret alanında backend geliştiriyorum, ödeme sistemleri üzerine çalıştım.
Backend Geliştirici — Ödeme altyapısında mikroservis geliştirdim, günde 2 milyon işlemi taşıyan sistemi iyileştirdim.
Bilgisayar Mühendisliği
React, PostgreSQL, Docker
SRC Belgesi
Kargo takip uygulaması — Küçük kargo firmaları için Flutter ile takip uygulaması.
```

| Neden | |
|---|---|
| **Unvan ilk satır** | Modeller metnin başına biraz daha fazla ağırlık verme eğiliminde; unvan "ne iş yapıyor"u en kısa söyleyen alan |
| **Kısa tanıtım zorunlu** | Deneyimsiz adayın vektörü eskiden sadece *"Unvan + Bölüm"* idi — çok kaba. Artık her profilde adayın kendi cümleleri var |
| **"Ne yaptın" en değerli** | CV ve belgeler eşleştirmeye girmiyor; adayın gerçekte ne yaptığı sadece orada yazıyor |

### 5.3 Adaydan bilerek girmeyenler

| Girmeyen | Neden |
|---|---|
| **Yetenek yılı** | Embedding sayıları okuyamaz — "React 5 yıl" ile "React 1 yıl" neredeyse aynı nokta. Yıl **puanda** kullanılır |
| **Şirket adı** | Aynı şirketten gelenler birbirine yakın çıkar — iş içeriği değil şirket benzerliği eşleşir. "Şirket adını gizle" de korunur |
| **Okul adı** | Aynı okul mezunları birbirine yakın çıkar — okul bazlı önyargı; bazı ülkelerin üniversiteleri sistematik avantaj kazanır |
| **Tarihler, tecrübe yılı** | Sayı — puanda hesaplanır |
| **Konum, meslek alanı, diller, çalışma şekli / tipi, eğitim seviyesi** | Zaten **filtrede** kesin kontrol ediliyor — iki kez sayılmasın |
| **Ad, doğum tarihi, cinsiyet, askerlik, fotoğraf** | Kişisel — ayrımcılık riski |
| **Projede "kullanılan teknolojiler"** | Çoğu zaten yetenek listesinde — aynı kelimeler iki kez girip fazla ağırlık kazanmasın |
| **CV, belgeler, sertifika dosyaları** | Bölüm 2 |

> **İki altın kural:**
> 1. **Sayılar embedding'e değil, hesaba.**
> 2. **Filtrede kontrol edilen şey embedding'e değil, filtreye.**

### 5.4 İlandan girenler

| # | Alan | Ne girer | Zorunlu mu |
|:---:|---|---|:---:|
| 1 | **Unvan** | **İlk satır** | ✅ |
| 2 | **Olmazsa olmaz yetenekler** | İşverenin cümlesi, **olduğu gibi** | ❌ |
| 3 | **Tercih edilen yetenekler** | Cümle, olduğu gibi | ❌ |
| 4 | **Tercih edilen bölümler** | Cümle, olduğu gibi | ❌ |
| 5 | **Olmazsa olmaz sertifikalar** | Cümle, olduğu gibi | ❌ |
| 6 | **Tercih edilen sertifikalar** | Cümle, olduğu gibi | ❌ |
| 7 | **İş tanımı / staj tanımı** | Tamamı | ✅ (min 100 karakter) |

İş ilanı ve staj ilanı **aynı yapıda.**

**Örnek — ilanın embedding'e giden metni:**
```
Senior Backend Developer
5 yıl React bilen, 8 yıl .NET tecrübesi olan, PostgreSQL'e hakim
Docker ve Kubernetes bilgisi artı
Bilgisayar veya yazılım mühendisliği
SRC belgesi
Ödeme altyapımızda mikroservisler geliştirecek, milyonlarca işlemi taşıyan
sistemlerin performansından sorumlu olacaksınız...
```

**Neden cümle, liste değil:** Cümle bağlamı taşır — *"PostgreSQL'e hakim, yüksek trafikli sistemlerde"* gibi ifadeler listede kaybolur. Cümledeki "5 yıl" vektöre girer ama zararsızdır — yıl karşılaştırmasını puan yapar.

**Aday tarafından farkı:** Aday yeteneklerini **tek tek** giriyor (React · 5 yıl), işveren **cümle** yazıyor. Cümle embedding için iyi; ama *"adayda React var mı, kaç yıl?"* diye **tek tek kontrol** için işe yaramıyor → **iki yol** (Bölüm 6).

### 5.5 Saklama

```sql
CREATE TABLE embeddings (
  varlik_tipi  text   NOT NULL,         -- 'aday' | 'ilan'
  varlik_id    bigint NOT NULL,
  model_adi    text   NOT NULL,         -- hangi modelle üretildi
  boyut        int    NOT NULL,         -- 1024
  metin_hash   text   NOT NULL,         -- embedding'e giren metinlerin özeti
  vektor       vector(1024) NOT NULL,
  guncelleme   timestamptz DEFAULT now(),
  PRIMARY KEY (varlik_tipi, varlik_id)
);

-- Aramalar hep KARŞI tarafa yapılır → tür bazlı iki indeks
CREATE INDEX emb_aday_hnsw ON embeddings
  USING hnsw (vektor vector_cosine_ops) WHERE varlik_tipi = 'aday';
CREATE INDEX emb_ilan_hnsw ON embeddings
  USING hnsw (vektor vector_cosine_ops) WHERE varlik_tipi = 'ilan';
```

| Karar | Neden |
|---|---|
| **Ayrı tablo** — profil / ilan tablosunda değil | Ana tablolar hafif kalır (vektör ~4 KB); model değişimi tek tabloda; gerekirse ayrı vektör veritabanına tek parça taşınır |
| **Her varlık için tek satır** | Metin değişince üstüne yazılır |
| **İki kısmi indeks** | İlan için aday ararken ilan vektörleri arasında dolaşılmaz |
| **`metin_hash`** | Metin aynıysa embedding API'sine gidilmez — Bölüm 12.5 |
| **`model_adi`, `boyut`** | Model değişince eski vektörler bulunup yeniden üretilir |
| **Staj ayrı tür değil** | `ilan_tipi = 'staj'`, `varlik_tipi = 'ilan'` |

### 5.6 Embedding servisi

| Karar | Neden |
|---|---|
| **API ile başla** | Başlangıçta kendi model sunucusu gereksiz yük |
| **Tek çok dilli model** | Tüm diller aynı anlam haritasında |
| **1024 boyut** | `pgvector` HNSW indeksi 2000 boyuta kadar destekliyor; kalite kaybı çok az |
| **`embed()` tek fonksiyon arkasında** | Sağlayıcı değişimi tek yerde |
| **Uygulamadan asla çağrılmaz** | API anahtarı Flutter / Next.js'e gömülürse çalınır — sadece sunucuda |

**Adaylar:** OpenAI `text-embedding-3-large` (`dimensions=1024`) · Cohere `embed-multilingual-v3`.

> 💬 **Seçim masa başında yapılmaz** — Bölüm 15.1'deki testte iki model yan yana denenir.
> 💡 **Kendi modelimize geçiş sinyali:** Fatura belirli eşiği aşınca veya veri yerleşimi gerekince. Açık kaynak çok dilli modeller (BGE-M3, multilingual-E5) aday.

---

## 6. İlan Metinleri — İki Yol ve LLM

### 6.1 Neden iki yol

Embedding **bütün metni karıştırıp tek bir genel anlam** çıkarıyor. Tek bir yeteneğin olup olmaması bu genel anlamı çok az değiştiriyor.

**Örnek — olmazsa olmaz: React, .NET, Kubernetes**
```
Aday A: sadece React biliyor
Aday B: React, .NET, Kubernetes — üçünü de biliyor
(ikisi de backend geliştirici, geri kalan profilleri benzer)
```

**Sadece embedding olsaydı:**
```
Aday A → 0.84
Aday B → 0.87     ← sadece 3 puan fark
```
A'nın tanıtımı biraz daha uzunsa **A, B'nin önüne bile geçebilir** — işveren üç şarttan ikisi eksik adayı üstte görür.

**Listeyle tek tek kontrol edince:**
```
Aday A:  React ✅  .NET ❌  Kubernetes ❌   → 1/3
Aday B:  React ✅  .NET ✅  Kubernetes ✅   → 3/3

Toplam puana etkisi:
Aday A ≈ 0.596      Aday B ≈ 0.716      → fark 3 puandan 12 puana çıktı
```

| | Embedding | Liste kontrolü |
|---|---|---|
| Neyi görür | *"Ödeme altyapısı"* ↔ *"ödeme sistemleri"* yakın | *".NET var mı?"* → yok |
| Neyi göremez | Bir şeyin **olmadığını**, sayıları | Eş anlamlıları |

> **Embedding "genel olarak benziyor mu" der. Liste "istenenler tek tek var mı" der.** İkisi birlikte doğru sıralamayı verir.

**Tercih edilenlerde de neden iki yol:** Yıl olmasa da *"var mı, yok mu"* sorusunu embedding cevaplayamıyor. Docker ve Kubernetes'i bilen ile bilmeyen embedding'de ~2 puan fark alır; liste farkı net koyar (1.0 vs 0.0). Ek maliyet yok — aynı LLM isteğinde çevriliyor.

### 6.2 Akış

```
İşveren yazar:
"5 yıl React bilen, 8 yıl .NET tecrübesi olan, PostgreSQL'e hakim"
        │
        ├── YOL 1 — Embedding
        │   Cümle OLDUĞU GİBİ ilan vektörüne girer
        │   → ANLAM BENZERLİĞİ puanı (%65)
        │
        └── YOL 2 — Liste
            LLM cümleyi listeye çevirir:
              React · 5 yıl
              .NET  · 8 yıl
              PostgreSQL
            → Adayın listesiyle TEK TEK karşılaştırılır
            → NİTELİK UYUMU puanı (%20)
```

**LLM eşleştirme yapmıyor, puan vermiyor.** Sadece işverenin cümlesini adayın listesiyle karşılaştırılabilecek bir **listeye çeviriyor** — bir çevirmen gibi. Karşılaştırmayı sistem basit aritmetikle yapıyor.

> LLM olmasaydı işverenden yetenekleri tek tek liste halinde girmesini istemek zorunda kalırdık.

### 6.3 Hangi kutular iki yola gider

| Kutu | YOL 1 — Embedding | YOL 2 — LLM listesi | Hangi gruba |
|---|:---:|---|---|
| Unvan | ✅ | ❌ | — |
| Olmazsa olmaz yetenekler | ✅ | ✅ Ad **+ yıl** | Olmazsa olmaz |
| Tercih edilen yetenekler | ✅ | ✅ Ad | Tercih edilen |
| Tercih edilen bölümler | ✅ | ✅ Bölüm adları | Tercih edilen — **tek öğe** |
| Olmazsa olmaz sertifikalar | ✅ | ✅ Ad | Olmazsa olmaz |
| Tercih edilen sertifikalar | ✅ | ✅ Ad | Tercih edilen |
| İş / staj tanımı | ✅ | ❌ — sayılacak bir şey yok | — |

- **Olmazsa olmaz / tercih edilen ayrımını kutu belirler** — LLM tahmin etmez
- **Ne zaman:** Sadece ilan **kaydedilirken**, kutuların metni değiştiyse — tek istekte beş kutu
- **Maliyet:** Küçük, hızlı model — ilan başına bir sentin çok altında

### 6.4 Onay yok — "Algılanan" satırı + [Düzenle]

LLM sonucu işverene **onaylatılmaz**, ama **gizli de değil:** her kutunun altında *"Algılanan: React · 5 yıl   .NET · 8 yıl"* satırı ve **[Düzenle]** var. İşveren bir şey yapmak zorunda değil.

**Neden onay yok:** Onay ekranı işveren için bir ekran ve karar daha demekti; *"onay bekliyor"*, hatırlatma, otomatik kabul gibi yan kurallar doğuruyordu. Görünür ve düzeltilebilir olması yeterli.

> Ekran ayrıntısı → `kesfet-ve-surec.md`

### 6.5 Etiket tablosu

```sql
CREATE TABLE ilan_etiketleri (
  ilan_id       bigint NOT NULL,
  tur           text   NOT NULL,   -- yetenek | sertifika | bolum
  oncelik       text   NOT NULL,   -- olmazsa_olmaz | tercih_edilen
  ad            text   NOT NULL,   -- 'React'
  normalize_ad  text   NOT NULL,   -- 'react' — karşılaştırma için
  yil           int,               -- sadece olmazsa olmaz yetenekte, varsa
  kaynak        text   NOT NULL    -- llm | isveren_duzeltti
);
```

| Tablo | Hangi puan parçası |
|---|---|
| `embeddings` | Anlam benzerliği |
| `ilan_etiketleri` | Nitelik uyumu |

**`kaynak` neden var:**
- **Koruma:** İşveren bir etiketi düzelttiyse, metin sonradan değişip LLM tekrar çalışsa bile **düzeltilen etiketin üstüne yazılmaz**
- **Açıklanabilirlik:** Etiketin LLM'den mi, işverenden mi geldiği bilinir
- **Kalite izleme:** İşverenlerin ne sıklıkla düzelttiği ölçülür

Aday tarafında etiket tablosu yok — adayın yetenek, sertifika, eğitim tabloları zaten yapılandırılmış.

### 6.6 LLM kalitesi

Onay olmadığı için kaliteyi sistem korur:

| Önlem | Ne yapar |
|---|---|
| **Kesin çıktı formatı** | LLM serbest yazı değil, belirli yapıda liste döndürür |
| **Örnekli talimat** | Farklı dillerde, farklı yazım şekillerinde örneklerle yönlendirilir |
| **Mantık kontrolü** | Yıl 0-40 dışındaysa, aynı etiket iki kez varsa → çıktı reddedilir, tekrar denenir |
| **Test seti** | 100-200 gerçek ilan cümlesiyle doğruluk ölçülür; model değişince tekrar ölçülür |
| **İzleme** | [Düzenle] kullanım oranı artarsa LLM'de sorun var |
| **Sadece metin değişince** | Kutuların özeti tutulur; maaş veya konum değişince LLM çağrılmaz |

### 6.7 LLM çalışmazsa — işveren takılmaz

```
İşveren [Kaydet] → LLM cevap vermedi
  → İlan YİNE kaydedilir, yayınlanabilir
  → Arka planda tekrar denenmek üzere sıraya alınır
  → O arada: adayın yetenek adları ilan metninde aranır (basit, yılsız)

Arka planda tekrar denenir → başarılı
  → Etiketler sessizce uygulanır (bildirim yok)
  → Eşleştirme yenilenir
```

| Puan parçası | LLM çalışmazken |
|---|---|
| Anlam benzerliği | ✅ Normal — LLM'e ihtiyacı yok |
| Nitelik uyumu | ⚠️ Geçici basit yöntem |
| Tecrübe, kalite | ✅ Normal |

> LLM arızası işvereni hiçbir zaman durdurmaz; en büyük puan parçası (%65) LLM olmadan zaten çalışıyor.

---

## 7. Katı Filtre

### 7.1 Nedir

Katı filtre **kesin şartlara uymayanları en başta eler.** Kimseyi sıralamaz — sadece *"evet / hayır"* der.

| Filtrede *(eler)* | Puanda *(sıralar, elemez)* |
|---|---|
| Konum, meslek alanı, iş dili, eğitim şartı… | Yetenekler, sertifikalar, tecrübe |
| **Esnemeyen** şeyler — İstanbul ilanı Ankara'daki adaya uymaz | **Derece** meselesi — React bilmeyen aday da düşer, sadece altta kalır |

**Neden yetenek filtrede değil:** Aday *"React"* yazmamış ama *"frontend geliştirme"* yazmış olabilir. Filtrede olsa iyi aday sessizce elenirdi.

### 7.2 On filtre — sırasıyla

| # | Filtre | Ne kontrol ediyor |
|:---:|---|---|
| 1 | Sistem kontrolleri | İlan yayında mı · aday yayında mı · arama durumu |
| 2 | Konum | Aday o konumda çalışmak istiyor mu |
| 3 | Meslek alanı | Aynı meslek alanı mı |
| 4 | Ne arıyor | İş mi, staj mı |
| 5 | Çalışma şekli | Ofis / hibrit / uzaktan — ortak var mı |
| 6 | Çalışma tipi | Tam / yarı zamanlı… — ortak var mı *(sadece iş ilanı)* |
| 7 | İş dili | Ortak dil var mı |
| 8 | Eğitim | İlanın istediği seviye ve durum var mı |
| 9 | Diğer şartlar | Sürücü belgesi, seyahat, vardiya, askerlik |
| 10 | Dışlamalar | Gizlenen şirketler, önceki aksiyonlar |

**Neden bu sıra:** En çok eleyen önce — ilk filtreler milyonları binlere indiriyor, sonrakiler küçük havuzda çalışıyor.

### 7.3 Filtre 1 — Sistem kontrolleri

| İlan tarafı | Aday tarafı |
|---|---|
| Durum = **yayında** | Profil **yayında** — zorunlu adımlar tamam + **telefon doğrulanmış** |
| | Arama durumu **"Şu an bakmıyorum" değil** |

#### Arama durumu

| Durum | Filtrede | Puana etkisi |
|---|:---:|:---:|
| 🟢 **Aktif arıyorum** | ✅ Geçer | **Yok** — sadece kartta rozet |
| 🟡 **Fırsatlara açığım** *(varsayılan)* | ✅ Geçer | **Yok** — sadece kartta rozet |
| ⚪ **Şu an bakmıyorum** | ❌ **Elenir** | — |

> **Neden "aktif" puana katkı vermiyor:** Durumu aday kendisi seçiyor; puana katılsa herkes "aktif" seçer, rozet anlamını kaybeder.

**Aday arama durumunu ne zaman seçer / değiştirir:**

| An | Ne olur |
|---|---|
| Profili ilk yayınlarken | Yayınla butonunun üstünde seçer — varsayılan 🟡 |
| İstediği zaman | Profil ana ekranı · Keşfet'in üst köşesi · Ayarlar — tek dokunuş |
| Giriş yapmazsa | Otomatik düşme: 30 gün → 🟡 · 90 gün → ⚪ *(bakım işi)* |
| Geri dönünce | Otomatik açılmaz — aday kendisi **[Aktif et]** der |

"Şu an bakmıyorum"dayken bekleyen başvurular, davetler ve süreçteki görüşmeler devam eder — sadece yeni işverenler göremez.

**Telefon:** Aday numarasını değiştirip doğrulamazsa profil yayından çıkar → bu filtrede elenir (Bölüm 12.7).

### 7.4 Filtre 2 — Konum

**Karşılaştırılan:** Adayın **çalışmak istediği konumlar** (en fazla 10) ↔ **ilanın konumu**

> İlanın konumu = işverenin ilanda seçtiği yer — şirketin merkezi değil. İş ofisten farklı yerde olabilir (saha, şantiye, İK ajansının müşterisi).

**Üst seviye seçilirse altı dahil:**

| Aday seçimi | İlan konumu | Sonuç |
|---|---|:---:|
| Türkiye | Kadıköy, İstanbul | ✅ |
| İstanbul | Kadıköy, İstanbul | ✅ |
| İstanbul | Çankaya, Ankara | ❌ |
| Almanya | München | ✅ |

```sql
i.konum_yolu LIKE ak.konum_yolu || '%'
-- 'TR/İstanbul/Kadıköy' LIKE 'TR/İstanbul%' → eşleşir
```

**Konumun atlandığı iki durum:**

| Durum | Neden |
|---|---|
| Aday **"Her konumda çalışmaya hazırım"** işaretlemiş | Kendisi "her yer olur" dedi |
| Aday ve ilanın çalışma şeklinde **ortak "uzaktan"** var | Uzaktan işte konum önemsiz |

| Aday | İlan | Konum |
|---|---|---|
| Uzaktan | Uzaktan | **Atlanır** — dünyanın her yerinden |
| Ofis, Uzaktan | Ofis | **Uygulanır** — ortak olan "ofis" |
| Hibrit | Hibrit | **Uygulanır** — hibritte de ofise gidiliyor |

**Neden konum puanda değil:** Aday konumlarını kendisi seçti; aralarında "hangisi daha iyi" diye sıralamak kararını ikinci kez yorumlamak olur.

> **Global sonuç:** Uzaktan ilanlarda havuz tüm dünya oluyor — o zaman **meslek alanı ve iş dili** en güçlü filtreler.

### 7.5 Filtre 3 — Meslek alanı

```
ilan.meslek_alani = aday.meslek_alani      ← tek eşitlik
```

**Neden gerekli:** Embedding aynı kelimeleri kullanan farklı meslekleri karıştırabiliyor:
```
İnşaat proje yöneticisi:  "Proje yönetimi, ekip koordinasyonu, bütçe takibi"
Yazılım proje yöneticisi: "Proje yönetimi, ekip koordinasyonu, bütçe takibi"
→ Anlam haritasında neredeyse aynı nokta
```

**Neden ikincil meslek alanı kaldırıldı:** "Yazılım Geliştirici" olup ikincil "Tarım" seçen aday tarım ilanlarına düşüyordu — ama profili yazılım anlatıyor. Alakasız ilanlar görüyor, tarım işvereni uymayan aday görüyordu. Kariyer değiştiren aday meslek alanını değiştirir ve profilini günceller.

### 7.6 Filtre 4 — Ne arıyor

Aday çoklu (iş / staj), ilan tek tip. **İlanın tipi adayın seçimlerinden biriyse** geçer.

| İlan | Aday | Sonuç |
|---|---|:---:|
| İş | İş · İş + Staj | ✅ |
| İş | Staj | ❌ |
| Staj | Staj · İş + Staj | ✅ |
| Staj | İş | ❌ |

### 7.7 Filtre 5 — Çalışma şekli

İki taraf çoklu (ofis / hibrit / uzaktan) — **en az bir ortak.**

| Aday | İlan | Sonuç |
|---|---|:---:|
| Ofis, Hibrit | Hibrit | ✅ |
| Ofis, Hibrit | Uzaktan | ❌ |
| Uzaktan | Ofis, Uzaktan | ✅ *(konum atlanır)* |

### 7.8 Filtre 6 — Çalışma tipi *(sadece iş ilanı)*

İki taraf çoklu (tam / yarı zamanlı / proje bazlı / dönemsel) — **en az bir ortak.**

| Aday | İlan | Sonuç |
|---|---|:---:|
| Tam zamanlı | Tam, Yarı | ✅ |
| Yarı zamanlı | Tam | ❌ |
| Tam, Proje bazlı | Proje bazlı | ✅ |

| Neden | |
|---|---|
| İlan çoklu | Kafe, mağaza *"tam veya yarı zamanlı"* der — tek seçim olsa adayların yarısını kaybederdi |
| Aday çoklu | Yarı zamanlı arayan öğrenciye tam zamanlı ilan düşmesin |
| Staj ilanında yok | Stajda "çalışma düzeni" var — sadece gösterim; öğrencinin müsaitliği dönemden döneme değişir |

### 7.9 Filtre 7 — İş dili

**"Bu işte hangi dilde çalışılacak?"** İki taraf çoklu, kapalı liste — **en az bir ortak dil.**

| | İlan | Aday — "Çalışabileceğim iş dilleri" |
|---|---|---|
| Zorunlu | ✅ En az bir | ✅ En az bir |
| Seviye | Yok | **Yok** |
| Varsayılan | Şirketin ülkesinin dili | Adayın ülkesinin ana dili |
| Otomatik | — | "Bildiğim diller"de **Anadil** seçilen dil eklenir — kaldırılabilir |

| Aday | İlan | Sonuç |
|---|---|:---:|
| Türkçe, İngilizce | İngilizce | ✅ |
| Türkçe | İngilizce | ❌ |
| İngilizce | Türkçe, İngilizce | ✅ — ilan iki dilden **birini** yeterli sayıyor |

**Neden kritik:** Embedding çok dilli — Türkçe profil İngilizce ilanla **anlam olarak** eşleşebilir, ama aday o ortamda **çalışamayabilir.** Bu filtre onu baştan eler.

**"Bildiğim diller" (dil + seviye) filtreye girmez** — sadece işverene gösterim. *"İngilizce — İleri"* diyen biri İngilizce ortamda çalışmak istemeyebilir; o kararı kendisi verir.

> ⚠️ **En büyük risk:** İş dilleri boş kalırsa aday **hiçbir ilanla eşleşmez.** Bu yüzden zorunlu ve varsayılan dolu.

### 7.10 Filtre 8 — Eğitim

**İlan tarafı:**
```
Kimler başvurabilir? *     ← tek seçim
○ Sadece devam eden öğrenciler
○ Sadece mezunlar
○ İkisi de
○ Eğitim şartı yok          ← sadece iş ilanında

Hangi seviyede? *          ← çoklu
☐ İlkokul ☐ Ortaokul ☐ Lise ☐ Ön lisans ☐ Lisans ☐ Yüksek lisans ☐ Doktora
```

**Aday tarafı:** Her eğitim kaydında **seviye + durum** (devam ediyor / mezun).

| İlan seçimi | Aday geçer, eğer… |
|---|---|
| Sadece devam eden öğrenciler | Seçilen seviyelerden birinde **"Devam ediyor"** kaydı var |
| Sadece mezunlar | Seçilen seviyelerden birinde **"Mezun"** kaydı var |
| İkisi de | Yukarıdakilerden biri |
| Eğitim şartı yok | Filtre çalışmaz |

**Tüm kayıtlara bakılır, sadece en yükseğe değil:**
```
Aday:  Lisans — Mezun
       Yüksek lisans — Devam ediyor

"Sadece mezunlar · Lisans"            → ✅ lisans mezun kaydı var
"Sadece öğrenciler · Yüksek lisans"   → ✅ y. lisans devam kaydı var
```
Sadece en yükseğe bakılsaydı bu aday "lisans mezunu" isteyen ilanlara düşmezdi.

```sql
i.egitim_sarti = 'yok'
OR EXISTS (
  SELECT 1 FROM aday_egitimleri e
  WHERE e.aday_id = a.id
    AND e.derece_isced = ANY(i.egitim_seviyeleri)
    AND ( (i.egitim_sarti = 'ogrenci' AND e.durum = 'devam')
       OR (i.egitim_sarti = 'mezun'   AND e.durum = 'mezun')
       OR (i.egitim_sarti = 'ikisi') )
)
```

| Kural | Neden |
|---|---|
| Seviyeler **ISCED** numarasıyla karşılaştırılır | Almanya'daki "Bachelor" = Türkiye'deki "Lisans" (6) — ülkeler arası çalışır |
| Seviye işaretlenince **üstündekiler otomatik** işaretlenir | Kolaylık — işveren kaldırabilir |
| **İlkokul / ortaokul öğrencisi** seçilemez | Çocuk işçiliği — "ikisi de" seçilse bile bu seviyelerde sadece mezun |
| **Staj ilanında** | Lise → doktora; "şart yok" seçeneği yok |

### 7.11 Filtre 9 — Diğer şartlar

Dört şart, **iki tarafta zorunlu seçim** — varsayılan seçili gelmez:

| Şart | İlan seçer | Aday seçer | Elenen |
|---|---|---|---|
| **Sürücü belgesi** | Gerekli değil / Gerekli + sınıf | Yok / Var + en az bir sınıf | Belgesi yok veya sınıf tutmuyor |
| **Seyahat** | Gerekli değil / Engeli olmamalı | Engelim yok / Var | "Var" |
| **Vardiya** | Gerekli değil / Vardiyalı çalışabilmeli | Evet / Hayır | "Hayır" |
| **Askerlik** *(sadece iş ilanı)* | Şart yok / Engeli olmamalı | Yapıldı / Muaf / Tecilli / Yükümlü değil | Sadece **tecilli** |

```sql
(NOT i.surucu_gerekli  OR a.surucu_siniflari && i.surucu_siniflari)
(NOT i.seyahat_gerekli OR a.seyahat_engeli = false)
(NOT i.vardiya_gerekli OR a.vardiya_uygun = true)
(NOT i.askerlik_sarti  OR a.askerlik_durumu <> 'tecilli')
```

**Filtre sadece ilan şart koyduysa çalışır.**

**Örnek:**
```
İlan: Saha satış — B ehliyet gerekli, seyahat engeli olmamalı
Aday A: B ehliyet · seyahat engeli yok   → ✅
Aday B: ehliyet yok                      → ❌
Aday C: B ehliyet · seyahat engeli var   → ❌
```

**Neden iki tarafta zorunlu:** Eskiden aday boş bırakabiliyor, şartlı ilanlardan **sessizce** eleniyordu.

**Askerlik — ayrımcılık önlemi:** Şart *"askerlik engeli olmamalı"* — sadece işe başlayıp askere gitmek zorunda kalacak olanı (tecilli) eler. Kadınlar ve askerliği olmayan ülkelerdeki adaylar *"Yükümlü değil"* seçer → geçer. Staj ilanında yok. İşveren profilde görür ama **listede süzemez.**

> **Görmek ≠ süzmek:** Cinsiyet ve askerlik işverene profil detayında görünür ama **hiçbir filtrede kullanılamaz** — işe alımda ayrımcılık yasak; platformun filtresi bunu kolaylaştırırsa sorumluluk platforma da gelir.

### 7.12 Filtre 10 — Dışlamalar

#### A — Gizlenen şirketler *(sadece işveren yönünde)*

Aday profilinde **"Gizlenecek şirketler"** seçtiyse (örneğin mevcut işvereni), o şirket adayı **hiçbir ilanında** göremez.

```sql
NOT EXISTS (SELECT 1 FROM aday_gizlenen_sirketler g
            WHERE g.aday_id = a.id AND g.sirket_id = i.sirket_id)
```

| | |
|---|---|
| Kaynak | Sadece **onaylı şirketler** |
| Kapsam | Şirketin **tüm** ilanları ve kullanıcıları |
| Tek yönlü | Aday o şirketin ilanlarını görmeye devam eder |

#### B — Önceki aksiyonlar

Aksiyon tablosunda kaydı olan **ilan + aday** çifti tekrar listeye düşmez:

| Aksiyon | Aday — Keşfet | İşveren — Adaylar |
|---|:---:|:---:|
| Aday **"Bu ilanı bir daha gösterme"** | ❌ | ✅ görmeye devam eder |
| İşveren **"Bu adayı bir daha gösterme"** | ✅ görmeye devam eder | ❌ |
| Başvuru, davet, eşleşme, herhangi bir sonuç | ❌ | ❌ |

| Kural | Neden |
|---|---|
| **"Bir daha gösterme" kalıcı** | Ayrı bir liste yok, geri alınmaz — onay diyaloğunda söylenir |
| **Tek yönlü** | Aday ilanı gizlediyse işveren hâlâ davet edebilir — aday fikrini değiştirebilir |
| **İlan bazlı** | "Backend" ilanında gizlenen aday aynı şirketin "Frontend" ilanında görünür |
| **Başvuru / davet / sonuç iki tarafı bağlar** | O çift için süreç başladı veya bitti |

> "Bir daha gösterme" ile "Gizlenecek şirketler" **iki ayrı özellik** — biri tek ilan–aday çifti, diğeri adayın bütün bir şirketten saklanması.

### 7.13 Tek fonksiyonda

```sql
CREATE FUNCTION uygun_adaylar(p_ilan_id bigint) ...
CREATE FUNCTION uygun_ilanlar(p_aday_id bigint) ...
```

> **Neden:** Kurallar değişecek. Sorgulara dağıtılırsa bir yerde unutulur, sistem **sessizce** yanlış sonuç üretir.

### 7.14 İndeksler

```sql
CREATE INDEX ON ilanlar (ulke_kodu, meslek_alani_id, ilan_tipi) WHERE durum = 'yayinda';
CREATE INDEX ON ilanlar (konum_yolu text_pattern_ops);
CREATE INDEX ON ilanlar USING gin (calisma_sekli);
CREATE INDEX ON ilanlar USING gin (calisma_tipi);
CREATE INDEX ON ilanlar USING gin (is_dilleri);

CREATE INDEX ON aday_profilleri (meslek_alani_id) WHERE yayinda;
CREATE INDEX ON aday_profilleri USING gin (calisma_sekli);
CREATE INDEX ON aday_profilleri USING gin (calisma_tipi);
CREATE INDEX ON aday_profilleri USING gin (is_dilleri);
CREATE INDEX ON aday_konumlari (konum_yolu text_pattern_ops);
CREATE INDEX ON aday_egitimleri (aday_id, derece_isced, durum);
```

Çoklu alanlar Postgres dizisi + `&&` (kesişim) — **GIN indeksi zorunlu.**

---

## 8. Vektör Aşaması

Katı filtreden geçenler **hepsi uygun** — ama yüzlerce, bazen binlerce. Bu aşama **anlam olarak en yakın 200'ü** seçer; puan sadece onlar için hesaplanır.

```sql
SELECT a.id, 1 - (ie.vektor <=> ae.vektor) AS anlam
FROM [katı filtre sonucu]
ORDER BY ie.vektor <=> ae.vektor
LIMIT 200;
```

### 8.1 Eşik değil, limit

| Yöntem | Ne olur |
|---|---|
| ❌ *"Benzerliği 0.7'nin üstündekileri göster"* | Aday az olan bir alanda kimse geçmezse **ekran boş** — halbuki makul adaylar var |
| ✅ *"En benzer 200'ü göster"* | Her zaman bir liste var; en iyiler üstte |

### 8.2 [Daha fazla getir]

```
1. Liste → en iyi 200
2. Kullanıcı bazılarına aksiyon alır (davet, başvuru, bir daha gösterme)
3. [Daha fazla getir] → yeniden hesap
4. Aksiyon alınanlar Filtre 10'da zaten elendiği için
   liste OTOMATİK olarak daha önce görülmemişlerle 200'e tamamlanır
5. Yeni sonuç yoksa → "Tüm uygun ilanları / adayları gördün"
```

Ek mantık gerekmiyor — Filtre 10'un doğal sonucu.

### 8.3 Hız — HNSW indeksi

Vektörler bir ağ yapısında tutulur; arama her adımda hedefe en yakın komşuya atlar — milyonlarca kaydı taramadan birkaç yüz adımda sonuca varır.

| Filtreden kalan havuz | Süre |
|---|---|
| 500 | ~2-5 ms |
| 2.000 | ~5-20 ms |
| 50.000 | ~20-50 ms |
| 500.000 *(uzaktan, dünya geneli)* | ~50-150 ms |

> ⚠️ **Bilinen risk — filtre ile indeks çekişmesi:** Filtre çok seçiciyse indeksin getirdiği yakın vektörlerin çoğu filtreden geçmeyebilir. Çözüm: `hnsw.ef_search` artırmak veya küçük havuzlarda indeks kullanmamak — **gerçek veriyle ölçülmeli.** Çözülemezse ayrı vektör veritabanına geçilir (Bölüm 14.4).

---

## 9. Puan

### 9.1 Puan nedir

Vektör aşaması *"en benzer 200"*ü seçti. Ama sadece benzerliğe göre sıralamak yetmez — işverenin şart koştuğu yetenekler var mı, tecrübe uygun mu, profil dolu mu? Puan bunları birleştirir.

| | Katı filtre | Puan |
|---|---|---|
| Soru | Uygun mu? | Ne kadar uygun? |
| Sonuç | Evet / hayır | 0-1 |
| Etkisi | **Eler** | **Sıralar** — kimseyi elemez |

### 9.2 Formül

```
puan = 0.65 × anlam benzerliği
     + 0.20 × nitelik uyumu
     + 0.10 × tecrübe
     + 0.05 × kalite
```

| Parça | Ağırlık | Ne soruyor | Nereden | İki yönde |
|---|:---:|---|---|---|
| **Anlam benzerliği** | %65 | Profil ile ilan genel olarak ne kadar benziyor? | Embedding | Aynı |
| **Nitelik uyumu** | %20 | İstenen yetenek, sertifika, bölüm **tek tek** var mı? | LLM listesi ↔ adayın listesi | Aynı |
| **Tecrübe** | %10 | Toplam tecrübe istenen aralıkta mı? | Deneyim tarihleri ↔ ilanın min–max | **Farklı** |
| **Kalite** | %5 | Karşı tarafın profili / ilanı ne kadar dolu? | Opsiyonel alanlar | **Farklı** |

### 9.3 İki yön — iki puan

Aynı aday–ilan çifti için **iki** puan hesaplanır:

| | Kim görür | Soru |
|---|---|---|
| **İşveren puanı** | İşverenin Adaylar listesi | *"Bu aday benim ilanıma ne kadar uygun?"* |
| **Aday puanı** | Adayın Keşfet'i | *"Bu ilan bana ne kadar uygun?"* |

```
işveren_puanı = 0.65 × anlam + 0.20 × nitelik + 0.10 × tecrübe_işveren + 0.05 × adayın_kalitesi
aday_puanı    = 0.65 × anlam + 0.20 × nitelik + 0.10 × tecrübe_aday    + 0.05 × ilanın_kalitesi
```

**Maliyet:** Pahalı kısım (anlam) bir kez hesaplanır, iki puanda aynı sayı kullanılır.

### 9.4 Anlam benzerliği — %65

```
anlam = 1 − (aday vektörü ile ilan vektörü arasındaki mesafe)   → 0 ile 1 arası
```

| Aday tarafı | İlan tarafı |
|---|---|
| Unvan | Unvan |
| Kısa tanıtım | Yetenek kutuları *(cümle)* |
| Deneyim: pozisyon + ne yaptın | Bölüm kutusu *(cümle)* |
| Bölüm adları | Sertifika kutuları *(cümle)* |
| Yetenek adları | İş / staj tanımı |
| Sertifika adları | |
| Proje adı + açıklama | |

```
Aday:  "Ödeme altyapısında mikroservis geliştirdim"
İlan:  "Ödeme sistemlerimizde dağıtık mimari üzerinde çalışacaksınız"
→ Kelimeler farklı, anlam yakın → yüksek puan
```

| Neden | |
|---|---|
| **En ağır** | *"Bu kişi bu işi yapabilir mi?"* sorusunun en geniş cevabı — diğer parçalar bunu kesin kriterlerle düzeltiyor |
| **İki yönde aynı** | Benzerlik simetrik — A'nın B'ye yakınlığı = B'nin A'ya yakınlığı |

### 9.5 Nitelik uyumu — %20

> Eski adı "yetenek skoru" — artık yetenek + sertifika + bölümü kapsadığı için **nitelik uyumu.**

#### İki grup

| Grup | İçinde | Ağırlık |
|---|---|:---:|
| **Olmazsa olmaz** | Olmazsa olmaz yetenekler + olmazsa olmaz sertifikalar | **0.75** |
| **Tercih edilen** | Tercih edilen yetenekler + tercih edilen sertifikalar + bölüm | **0.25** |

#### Her öğenin puanı

| Öğe | Adayda | Puan |
|---|---|:---:|
| **Yetenek — yıl istenmiş** *(olmazsa olmaz)* | Var, yıl yeterli | 1 |
| | Var, yıl az | **0.5 + 0.5 × (aday yılı ÷ istenen yıl)** |
| | Yok | 0 |
| **Yetenek — yılsız / tercih edilen** | Var · Yok | 1 · 0 |
| **Sertifika** | Var ve **geçerli** · Yok veya süresi geçmiş | 1 · 0 |
| **Bölüm** | Adayın bölümlerinden **biri** listede · Hiçbiri | 1 · 0 |

```
olmazsa_olmaz = olmazsa olmaz grubunun ortalaması
tercih_edilen = tercih edilen grubunun ortalaması
nitelik uyumu = 0.75 × olmazsa_olmaz + 0.25 × tercih_edilen
```

#### Örnek

```
İLAN
  Olmazsa olmaz:  React · 5 yıl,  .NET,  SRC belgesi
  Tercih edilen:  Docker,  İSG eğitimi,  Bölüm: Bilgisayar / Yazılım Müh.

ADAY
  React 3 yıl · Docker · SRC (geçerli) · İSG (süresi geçmiş) · Bilgisayar Müh.

Olmazsa olmaz:
  React  3 < 5   → 0.5 + 0.5 × 3/5 = 0.80
  .NET   yok     → 0
  SRC    geçerli → 1
  ortalama = (0.80 + 0 + 1) / 3 = 0.60

Tercih edilen:
  Docker var → 1 · İSG süresi geçmiş → 0 · Bölüm eşleşti → 1
  ortalama = 2/3 = 0.67

nitelik uyumu = 0.75 × 0.60 + 0.25 × 0.67 = 0.62
```

#### Neden böyle

| Karar | Neden |
|---|---|
| **0.5 tabanı** | Yeteneğin **olması** tek başına değerli — 1 yıl React bilen, hiç bilmeyenden çok daha iyi |
| **Yıl sadece olmazsa olmazda** | Tercih edilen "olursa iyi olur" — kaç yıl olduğuna bakmak gereksiz |
| **Olmazsa olmaz 3 kat ağır** | İşverenin şart koştuğu şeyler |
| **Bölüm tek öğe** | İşveren 5 bölüm yazdı diye 5 öğe olsa bölüm yeteneklerden ağır basardı |
| **Süresi geçmiş sertifika sayılmaz** | SRC, İSG, ilk yardım yenilenmezse geçersiz |

#### Özel durumlar

| Durum | Sonuç |
|---|---|
| İlan hiç yetenek / sertifika / bölüm yazmamış | 0.5 — nötr |
| Sadece bir grup var | O grup tek başına |
| Yazım farkı (*ReactJS* / *React.js*) | Normalize + metin benzerliği (`pg_trgm`) ile eşleşir |
| LLM henüz çevirmedi | Geçici: adayın adları ilan metninde aranır (yılsız) |

#### Neden iki yönde aynı

İlk tasarımda aday yönü *"yeteneklerimin kaçı bu işte kullanılıyor"* diye hesaplanıyordu — **çok yetenek yazmayı cezalandırıyordu** (10 yetenekli aday, 2 yetenek isteyen ilanla 0.2 alıyordu). Aday yeteneklerini eksik yazmaya başlar, hem liste hem embedding zayıflardı.

### 9.6 Tecrübe — %10

#### Adayın tecrübesi — otomatik

Aday "5 yıl tecrübem var" yazmaz; deneyim kayıtlarından hesaplanır:

| Çalışma türü | Sayılır |
|---|:---:|
| Tam zamanlı, freelance | × 1 |
| Yarı zamanlı, staj, dönemsel | × 0.5 |
| Gönüllü | Sayılmaz |

Aynı anda iki işte çalışılan dönemler iki kez sayılmaz. Deneyimi yoksa 0 yıl.

#### Puan tablosu

| Aday durumu | İşveren puanı | Aday puanı |
|---|:---:|:---:|
| **Aralıkta** | 1.0 | 1.0 |
| Max'ı **3 yıla kadar** aşıyor | **0.8** | 0.7 |
| Max'ı **3-6 yıl** aşıyor | **0.6** | 0.4 |
| Max'ı **6+ yıl** aşıyor | **0.4** | 0.2 |
| **Aralığın altında** | aday yılı ÷ min yıl | **0.7 + 0.3 ×** (aday yılı ÷ min yıl) |
| **Hiç tecrübesi yok** | 0 | 0.7 |
| **İlan aralık yazmamış** | 0.5 | 0.5 |
| **Staj ilanı** | 0.5 | 0.5 |

#### Neden böyle

| Karar | Neden |
|---|---|
| **İki tarafta da fazla tecrübe düşer** | İyi eşleşme = **iki tarafın da "evet" diyeceği** eşleşme. 8 yıllık aday junior ilanın maaşına çoğunlukla "hayır" der — işverenin listesinin başında olması boşa davet ve ret demek |
| **İşverende daha yumuşak düşüş** | Bazı işverenler fazla nitelikli adayı gerçekten ister — aşağıda ama **görünür** kalsın |
| **Aday — altında 0.7 tabanı** | Biraz zorlayıcı ilan adayın **gelişim fırsatı** — listenin dibine düşmesin |
| **Staj — 0.5 nötr** | Stajda tecrübe sorulmuyor; parça etkisiz kalır, formül değişmez |

> **"Ceza" değil.** Hiçbir yerde çıkarma yok — her parça 0-1 puan üretir. Düşük puan sadece "tam puan alamıyor" demek.

#### Örnek — ilan 1-2 yıl (junior)

| Aday | İşveren puanı | Aday puanı | Ne demek |
|---|:---:|:---:|---|
| 0 yıl | 0.0 | 0.70 | İşveren istemiyor; aday öğrenmek istiyor |
| 1-2 yıl | **1.0** | **1.0** | İkisi de memnun — **iki listede de üstte** |
| 4 yıl | 0.8 | 0.7 | Biraz aşağıda |
| 8 yıl | **0.4** | **0.2** | **İki listede de altta** — ama görünür |

**Toplam puana etkisi:** 8 yıllık aday tecrübeden 0.10 × 0.4 = 0.04 alır; aralıktaki aday 0.10. Diğer her şey eşitse aralıktaki **6 puan** önde. Anlam ve nitelikte çok güçlüyse 8 yıllık aday yine üstlerde çıkabilir — **hiç gizlenmez.**

#### Örnek — ilan 3-6 yıl, aday puanı

```
Aday  4 yıl  → 1.0                        aralıkta
Aday  2 yıl  → 0.7 + 0.3 × 2/3 = 0.90     biraz altında
Aday  1 yıl  → 0.7 + 0.3 × 1/3 = 0.80     altında
Aday  8 yıl  → 0.7                        max'ı 2 yıl aşıyor
Aday 11 yıl  → 0.4                        max'ı 5 yıl aşıyor
Aday 15 yıl  → 0.2                        max'ı 9 yıl aşıyor
```

### 9.7 Kalite — %5

İki tarafta **tamamen farklı** şeye bakar.

#### İşveren puanı → adayın profili ne kadar dolu

Zorunlu alanlar herkeste dolu — sadece **opsiyonel** alanlar:

| Kriter | Puan |
|---|:---:|
| En az bir deneyim kaydı | 0.30 |
| En az 5 yetenek | 0.25 |
| Proje veya sertifika var | 0.15 |
| Fotoğraf var | 0.15 |
| En az bir belge (CV vb.) | 0.15 |
| **Toplam** | **1.00** |

#### Aday puanı → ilan ve şirket ne kadar dolu

| Kriter | Puan |
|---|:---:|
| **Maaş / ücret belirtilmiş** | **0.50** |
| Şirket profilinde "imkanlar" dolu | 0.20 |
| Gerçek logo | 0.15 |
| Web sitesi veya sosyal medya | 0.15 |
| **Toplam** | **1.00** |

#### Örnek

```
Aday A: deneyim ✓ · 8 yetenek ✓ · proje ✓ · fotoğraf ✓ · CV ✗  → 0.85
Aday B: deneyim ✗ · 2 yetenek ✗ · proje ✗ · fotoğraf ✓ · CV ✗  → 0.15

Toplama etkisi:  A → 0.05 × 0.85 ≈ 4.3 puan
                 B → 0.05 × 0.15 ≈ 0.8 puan      → fark ~3.5 puan
```

| Neden | |
|---|---|
| **Sadece %5** | Daha yüksek olsa, profili dolu ama işe **uymayan** aday uygun ama eksik profilli adayın önüne geçerdi. %5 sadece **eşitliği bozar** |
| **Asıl işi teşvik** | Adaya *"profilini doldur"*, işverene *"maaş yaz"* — zorlamadan |
| **Maaş en ağır** | Adayın en çok merak ettiği bilgi |

### 9.8 Tam örnek — iki adayın sıralaması

```
İlan: Senior Backend Developer — React 5 yıl ve .NET olmazsa olmaz, 3-6 yıl

                  Aday A           Aday B
Anlam benzerliği  0.86             0.84
Nitelik uyumu     0.85             0.33   (sadece React)
Tecrübe (işv.)    1.00 (4 yıl)     1.00 (5 yıl)
Kalite (işv.)     0.80             0.60

İşveren puanı:
A = 0.65×0.86 + 0.20×0.85 + 0.10×1.00 + 0.05×0.80 = 0.869
B = 0.65×0.84 + 0.20×0.33 + 0.10×1.00 + 0.05×0.60 = 0.742
```

İşverenin listesinde **A üstte** — anlam yakın, nitelikler farkı açıyor.

### 9.9 Kullanıcıya puan değil, seviye

| Seviye | Puan *(başlangıç değeri)* |
|---|---|
| 🟢 **Yüksek uyum** | ≥ 0.75 |
| 🟡 **Orta uyum** | 0.55 – 0.75 |
| ⚪ **Düşük uyum** | < 0.55 |

*"0.742"* kullanıcıya bir şey ifade etmiyor; *"🟡 Orta uyum"* ediyor.

### 9.10 Ayarlar veritabanında

Tüm sayılar — 0.65, 0.75, 0.5 tabanı, tecrübe kademeleri, kalite puanları, seviye eşikleri — **veritabanında.**

```sql
CREATE TABLE skor_parametreleri (
  yon        text NOT NULL,   -- 'isveren' | 'aday'
  bilesen    text NOT NULL,   -- 'anlam' | 'nitelik' | 'tecrube' | 'kalite'
  agirlik    real NOT NULL,
  ayarlar    jsonb,           -- formül içindeki sabitler
  guncelleme timestamptz DEFAULT now(),
  PRIMARY KEY (yon, bilesen)
);
```

| Önlem | Neden |
|---|---|
| **Kod değişmeden ayar** | Gerçek veriyle mutlaka oynanacak |
| **Toplam 1.0 kontrolü** | Ağırlıklar yanlışlıkla bozulmasın |
| **Değişiklik günlüğü** | Kim, ne zaman, neyi değiştirdi — bozulursa geri dönülür |
| **Değişiklik sonrası yeniden hesap** | Büyük değişiklikte toplu yeniden hesap tetiklenir |

### 9.11 Alt puanlar saklanır

```
İşveren: anlam 0.86 · nitelik 0.85 · tecrübe 1.0 · kalite 0.80
Aday:    anlam 0.86 · nitelik 0.85 · tecrübe 0.7 · kalite 0.65
```

| Neden | |
|---|---|
| **Ayarlama** | Hangi parça yanlış çalışıyor, görülebilsin |
| **Yasal** | *"Bu adayı neden üstte gösterdiniz?"* — KVKK, GDPR md. 22, AB Yapay Zeka Yasası (işe alım yüksek riskli), NYC Local Law 144 |
| **"Neden önerildi?"** | Kullanıcıya açıklama bu puanlardan, hazır şablonlarla üretilir — LLM'siz |

---

## 10. Sonuç Tablosu

### 10.1 Ne işe yarar

Puanlar **önceden hesaplanıp** bu tabloda saklanır. Kullanıcı Keşfet'i açınca hesap yapılmaz, **okunur** — liste anında gelir.

```sql
CREATE TABLE ilan_aday_eslestirme (
  ilan_id            bigint NOT NULL,
  aday_id            bigint NOT NULL,
  puan_isveren       real   NOT NULL,
  puan_aday          real   NOT NULL,
  alt_puanlar        jsonb,
  model_adi          text,
  hesaplanma_tarihi  timestamptz NOT NULL DEFAULT now(),
  PRIMARY KEY (ilan_id, aday_id)
) PARTITION BY HASH (ilan_id);          -- 32 bölüm, ilk günden

CREATE INDEX ON ilan_aday_eslestirme (ilan_id, puan_isveren DESC);
CREATE INDEX ON ilan_aday_eslestirme (aday_id, puan_aday DESC);
CREATE INDEX ON ilan_aday_eslestirme (hesaplanma_tarihi);
```

| İlan | Aday | İşveren puanı | Aday puanı | Hesaplanma |
|---|---|:---:|:---:|---|
| Backend Dev | Ayşe | 0.87 | 0.84 | 2 saat önce |
| Backend Dev | Mehmet | 0.74 | 0.61 | 2 saat önce |

```sql
-- İşveren "Adaylar" → tek bölüme gider
WHERE ilan_id = :ilan ORDER BY puan_isveren DESC LIMIT 200
-- Aday "Keşfet"     → 32 bölüme bakar (indeksli, okuma kopyasından, ilk sayfa önbellekte)
WHERE aday_id = :aday ORDER BY puan_aday DESC LIMIT 200
```

### 10.2 Tek tablo, iki yön — en büyük faydası

```
Pazartesi:   Backend Dev yayınlandı → o günkü adaylar için hesaplandı
Salı:        Zeynep kaydoldu → onun için hesap → Backend Dev'e uygun
             → (Backend Dev, Zeynep) satırı yazıldı — İKİ puanla
Salı akşamı: İşveren Adaylar'ı açtı → Zeynep listede ✅
```

İşveren hiçbir şey yapmadı — satır iki yönden okunabildiği için yeni aday onun listesine **kendiliğinden** girdi.

**Yazma "üstüne yaz":**
```sql
INSERT ... ON CONFLICT (ilan_id, aday_id) DO UPDATE SET
  puan_isveren = EXCLUDED.puan_isveren, puan_aday = EXCLUDED.puan_aday,
  alt_puanlar = EXCLUDED.alt_puanlar, hesaplanma_tarihi = now();
```
Aynı iş iki kez çalışsa da sonuç bozulmaz.

### 10.3 Neden geçici

Bu tablodaki hiçbir satır **tek kopya bilgi değil** — hepsi başka yerlerden türetiliyor, silinse birkaç milisaniyede yeniden üretilir:

| Satırdaki bilgi | Asıl kaynağı |
|---|---|
| Anlam benzerliği | `embeddings` |
| Nitelik uyumu | İlan etiketleri + adayın listeleri |
| Tecrübe | Deneyim tarihleri + ilanın aralığı |
| Kalite | Profil ve ilan doluluğu |

| Ne zaman silinir | Neden |
|---|---|
| 14 günden eski hesaplananlar | Bayat — açılırsa güncel haliyle yeniden hesaplanır |
| İlan kapandı | Keşfet'te artık görünmüyor |
| Aday "Şu an bakmıyorum" | Kimse görmüyor |
| Pasif kullanıcı (14+ gün girmemiş) | Açarsa anında hesaplanır |
| Aday telefonu değiştirip doğrulamadı | **Silinmez** — profil yayından çıktığı için görünmez; doğrulayınca anında geri gelir |

**Neden siliyoruz:** Sistemin en büyük tablosu — `5 milyon aday × 200 = 1 milyar satır`. Gereksiz satırlar tabloyu yavaşlatır, depolama maliyetini artırır.

> **Kural:** Hesapla **üretilebilen** şey geçici tabloda; **kullanıcının yaptığı** şey kalıcı tabloda (Bölüm 11).

---

## 11. Aksiyon Tablosu

### 11.1 Ne işe yarar

Kullanıcının **yaptığı** her şey burada — ve **kalıcı.** Motor bu tabloyu **Filtre 10**'da kullanır: aksiyon alınmış ilan + aday çifti tekrar listeye düşmez.

```sql
CREATE TABLE ilan_aday_aksiyon (
  ilan_id  bigint NOT NULL,
  aday_id  bigint,            -- hesap silinince NULL (anonim)
  aksiyon  text   NOT NULL,
  taraf    text   NOT NULL,   -- aday | isveren | sistem
  tarih    timestamptz NOT NULL DEFAULT now()
) PARTITION BY HASH (ilan_id);          -- 16 bölüm, ilk günden

CREATE INDEX ON ilan_aday_aksiyon (aday_id, taraf);
CREATE INDEX ON ilan_aday_aksiyon (ilan_id, taraf);
```

| Aksiyon | Anlamı | Kim |
|---|---|---|
| `bir_daha_gosterme` | "Bir daha gösterme" dendi | Aday veya işveren |
| `basvuruldu` | Aday başvurdu | Aday |
| `davet_edildi` | İşveren davet etti | İşveren |
| `eslesti` | İki taraf onayladı — süreçte | — |
| `reddedildi` | İşveren başvuruyu / aday daveti kabul etmedi | İşveren veya aday |
| `suresi_doldu` | Başvuru 30 gün / davet 10 gün cevapsız | Sistem |
| `surec_sonlandi` | İşveren ilanı kapatırken bekleyen başvuru **ve** davetleri toplu bitirdi | İşveren |
| `basvuru_geri_cekildi` | Aday başvurusunu geri çekti | Aday |
| `davet_geri_cekildi` | İşveren davetini geri çekti | İşveren |

> Bu aksiyonların kullanıcıya nasıl göründüğü → `kesfet-ve-surec.md`

### 11.2 Neden sonuç tablosundan ayrı

```
Sonuç tablosu temizlendi → (Backend Dev, Mehmet) satırı silindi
                         → eşleştirme yeniden çalıştı
                         → Mehmet tekrar Backend Dev'in listesine düştü ❌
   …halbuki işveren Mehmet için "bir daha gösterme" demişti!
```

Aksiyon **ayrı ve kalıcı** olduğu için bu olmaz — her hesapta Filtre 10 bu tabloya bakar.

> Bu ayrım sonradan yapılması zor — **baştan ayrı kurulur.**

### 11.3 İlan bazlı

Kayıt **ilan + aday** çiftine yazılır, şirkete değil:
```
İşveren Ayşe için "Backend Dev" ilanında "bir daha gösterme" dedi
  → Backend Dev listesinde görünmez
  → Aynı şirketin "Frontend Dev" ilanında görünmeye devam eder
```
Bir pozisyona uymayan aday başka pozisyona çok uygun olabilir.

### 11.4 Örnek kayıtlar

| İlan | Aday | Aksiyon | Kim | Tarih |
|---|---|---|---|---|
| Backend Dev | Ayşe | basvuruldu | aday | 12 Eylül |
| Backend Dev | Ayşe | eslesti | — | 15 Eylül |
| Backend Dev | Mehmet | bir_daha_gosterme | işveren | 13 Eylül |
| Frontend Dev | Zeynep | davet_edildi | işveren | 14 Eylül |
| Frontend Dev | Zeynep | suresi_doldu | sistem | 24 Eylül |

### 11.5 Ne zaman silinir — ne zaman anonimleşir

> **Kural:** Kim olduğunu söyleyen her şey silinir. Kimseye bağlanamayan sayılar raporlar için kalır.

| Durum | Aksiyon kayıtları |
|---|---|
| İlan **kapandı** — süresi doldu | **Kalır** — tekrar yayınlanırsa "bir daha gösterme" hâlâ geçerli olmalı |
| İlan **elle kapatıldı** | `bir_daha_gosterme` kayıtları **silinir** (ilan tekrar yayınlanamıyor, işe yaramıyor) · diğerleri kalır |
| Süresi dolan ilan **1 yıl** tekrar yayınlanmadı | Kalıcı kapalı sayılır → aynı temizlik |
| İşveren kapalı ilanı **sildi** *(soft delete)* | `bir_daha_gosterme` kayıtları silinir · diğerleri raporlar için kalır |
| **Aday hesabını sildi** | `bir_daha_gosterme` silinir · diğerleri **anonimleşir** (`aday_id` boşaltılır) |
| **Şirket hesabını sildi** *(30 gün sonra)* | `bir_daha_gosterme` silinir · diğerleri **anonimleşir** |

**Neden tamamen silmiyoruz:** İlan raporu *"120 başvuru → 5 kabul → 1 işe alım"* der. Aday kayıtları tamamen silinse sayılar geriye dönük değişir. Anonim kayıt kimseye bağlanamaz — **KVKK / GDPR açısından kişisel veri sayılmaz.**

**Boyut:** Bir kayıt ~50 bayt; 100 milyon kayıt ~5 GB. Gereksiz olanlar yukarıdaki kurallarla temizlenir.

> Temizlik işleri → `bakim-isleri.md` · Hesap silme ayrıntısı → Veri koruma dokümanı

---

## 12. Hesaplama — Ne Zaman, Nasıl

### 12.1 Karma hesaplama

**İki uç da kötü:**

| Yöntem | Sorun |
|---|---|
| Herkes için hep önceden hesapla | İş bulmuş, aylardır girmeyen binlerce kişi için her yeni ilanda hesap — **israf** |
| Sadece Keşfet açılınca hesapla | Aktif kullanıcı her açılışta bekler; *"sana uygun yeni ilan var"* bildirimi gönderilemez |

**Çözüm:**
```
Son 14 günde giriş yapmış  → ÖNCEDEN hesaplanır
                             Keşfet açılınca liste anında hazır

14 günden uzun girmemiş    → HESAPLANMAZ
                             Keşfet'i açarsa o an hesaplanır (~200 ms, "Hazırlanıyor...")

"Şu an bakmıyorum"         → Hiç — kimse görmüyor
```

```
Hepsi önceden:  500.000 aday × her yeni ilan
Karma:          ~75.000 aktif aday × her yeni ilan   → ~%85 daha az hesap
```

| | |
|---|---|
| **İşveren tarafı** | Aynı kural — 14 gündür girmeyen işverenin ilanları için önceden hesap yapılmaz |
| **Tazelik** | Liste 24 saatten eskiyse açılınca yeniden hesaplanır |

### 12.2 Eşleştirme durumu

Arka plandaki hesabın **şu an nerede olduğunu** söyleyen etiket — her aday profilinde ve her ilanda bir tane.

```sql
aday_profilleri.eslestirme_durumu, aday_profilleri.son_eslestirme_tarihi
ilanlar.eslestirme_durumu,         ilanlar.son_eslestirme_tarihi
```

| Durum | Anlamı | Kullanıcı görür |
|---|---|---|
| `beklemede` | Sıraya alındı, henüz başlamadı | ⏳ *Hazırlanıyor...* |
| `hesaplaniyor` | İşçi şu an hesaplıyor | ⏳ *Hazırlanıyor...* |
| `hazir` | Bitti | Liste — veya gerçekten boşsa *"uygun ilan yok"* |
| `hata` | Ters gitti, tekrar denenecek | ⏳ *Hazırlanıyor...* |

**Neden gerekli — boş liste iki farklı şey olabilir:**
```
Keşfet boş. Neden?
  a) Hesap henüz bitmedi      → birkaç saniye sonra dolacak
  b) Gerçekten uygun ilan yok → bitti, gerçekten boş
```
Etiket olmasa sistem ikisini ayıramaz; aday (a)'da da *"uygun ilan yok"* görüp çıkar.

**Neden dört ayrı durum:** Kullanıcı için iki durum var (*hazır / hazırlanıyor*). Ayrım **sistem için:** `hesaplaniyor`da uzun kalan iş = işçi takılmış → bakım işi yeniden başlatır. `beklemede` uzun = sıra birikmiş → işçi sayısı artırılır. `hata` artışı = bir servis çökmüş → uyarı.

> Diğer "durum"larla karışmasın: **arama durumu** (aday iş arıyor mu) ve **ilan durumu** (ilan yayında mı) ayrı şeyler.

### 12.3 Aday profili yayınlanınca

```
ADIM 1 — ANINDA (kullanıcı bekler, milisaniyeler)
  ├─ Profil kaydedilir
  ├─ Kontrol: zorunlu adımlar tamam mı? telefon doğrulanmış mı?
  ├─ Profil → "yayında"
  └─ Metin değiştiyse → sıraya: "Ayşe → embedding üret"   (aynı işlemde)
        ↓
  Ayşe görür: "Profilin yayında. Sana uygun ilanlar hazırlanıyor..."

ADIM 2 — ARKA PLANDA (1-2 sn)
  ├─ Metinler birleştirilir → embedding üretilir → embeddings tablosu
  └─ Sıraya: "Ayşe → eşleştir"

ADIM 3 — ARKA PLANDA (milisaniyeler)
  ├─ Katı filtre → en benzer 200 → puan (iki yön)
  ├─ Sonuç tablosuna yazılır
  ├─ Durum → "hazır"
  └─ Listesinde ilk 50'ye giren yeni ilanlar → günlük özet bildirimine not
```

### 12.4 İlan kaydedilip yayınlanınca

```
ADIM 1 — İşveren [Kaydet]  (ilan taslak)
  ├─ İlan kaydedilir
  └─ Yetenek / bölüm / sertifika kutuları değişti mi?
        Evet  → LLM listeye çevirir (1-3 sn) → "Algılanan" satırı
        Hayır → önceki liste kullanılır
        LLM cevap vermezse → ilan yine kaydedilir, arka planda tekrar denenir

ADIM 2 — İşveren [Yayınla]  (anında)
  ├─ Kontrol: şirket tanıtımı dolu mu? yayınlayanın telefonu doğrulanmış mı?
  ├─ İlan → "yayında"
  └─ Sıraya: "İlan #5 → embedding üret"
        ↓
  İşveren görür: "İlanınız yayında. Uygun adaylar hazırlanıyor..."

ADIM 3 — ARKA PLANDA → embedding → ADIM 4 → eşleştir (ilan × aktif uygun adaylar)
```

| | Aday profili | İlan |
|---|---|---|
| LLM | ❌ | ✅ Kaydederken, bir kez |
| Kullanıcı ne kadar bekler | Milisaniyeler | Kaydederken 1-3 sn, yayınlarken milisaniyeler |
| Eşleştirme | Aday × uygun ilanlar | İlan × uygun **aktif** adaylar |

Hangi taraf tetiklerse tetiklesin satırlar **aynı tabloya, iki puanla** yazılır.

### 12.5 Ne değişince ne tetiklenir

Düzenlemede her şey baştan hesaplanmaz — sadece **gereken:**

| Ne değişti | Örnek | Embedding | Eşleştirme |
|---|---|:---:|:---:|
| **Metin** | Tanıtım, deneyim, iş tanımı, yetenek / bölüm / sertifika kutuları | ✅ | ✅ *(embedding bitince)* |
| **Sadece filtre alanı** | Konum, çalışma şekli / tipi, iş dili, eğitim şartı, diğer şartlar | ❌ | ✅ |
| **Sadece puan alanı** | Tecrübe aralığı, maaş | ❌ | ✅ |
| **Sadece gösterim alanı** | Başlama zamanı, fotoğraf, serbest şartlar | ❌ | ❌ |

İlan kutuları değişince **LLM de yeniden** çevirir — işverenin düzelttiği etiketler korunur.

**Nasıl anlıyor — metin özeti (hash):**
```
Yeni özet = eski özet?
  Evet  → metin aynı → embedding'e dokunma (API çağrısı yok)
  Hayır → embedding yeniden üret
```

**Örnek:**
```
İşveren sadece konumu değiştirdi  → embedding YOK · eşleştirme VAR
Aday sadece fotoğrafını değiştirdi → hiçbir şey
Aday yeni deneyim ekledi           → embedding VAR → eşleştirme VAR
```

### 12.6 Yayındaki ilan düzenlenirse

| Alan | Taslakken | Yayından sonra |
|---|:---:|:---:|
| **Meslek alanı** | ✅ | 🔒 **Değişmez** |
| **Unvan** | ✅ | 🔒 **Değişmez** |
| Diğer alanlar | ✅ | ✅ — Bölüm 12.5'e göre tetikler |

> *"Meslek alanı ve unvan yayından sonra değiştirilemez. Farklı bir pozisyon için yeni ilan açın."*

**Neden:** İkisi pozisyonun kimliği. Muhasebe ilanını yazılım ilanına çevirmek; eski başvuranların, davetlerin, "bir daha gösterme" kayıtlarının geçmişini alakasız bir pozisyona taşır. Yan faydası: embedding'in ilk satırı sabit kalır.

**Önceden olanlar korunur:** Yeni şartlara artık uymayan aday zaten başvurmuşsa başvurusu geçerli kalır — sadece listedeki yeni adaylar değişir.

### 12.7 Aday telefonu değişirse

```
Numara değişti → doğrulama kalkar → profil yayından çıkar (Filtre 1'de elenir)
        ↓
Hemen doğrularsa                       Doğrulamazsa
→ profil yayına döner                  → işverenlerin listesinde görünmez
→ puanlar zaten hazır, anında geri     → puanlar SİLİNMEZ, bekler
                                       → 14 gün sonra normal temizlikle silinir
```

Her durumda başvurular, davetler ve süreçteki görüşmeler **devam eder.**

---

## 13. Kuyruk ve İşçiler

### 13.1 Kuyruk nedir

**Bankadaki sıra numarası.** 100 kişi aynı anda gelse gişe hepsine aynı anda bakamaz — herkes numara alır, sırayla çağrılır.

```
Bir şey olur          →   Sıraya iş yazılır        →   İşçi sırayla alır, yapar
(ilan yayınlandı)         ("ilan #5'i eşleştir")       (hesaplar, sonucu yazar)
```

| Fayda | |
|---|---|
| **Kullanıcı beklemez** | Ağır iş arka planda |
| **Ani yük emilir** | 10.000 iş aynı anda gelse sıraya girer — sistem çökmez, biraz gecikir |
| **Hiçbir iş kaybolmaz** | İşçi çökerse veya dış servis cevap vermezse iş sırada kalır, tekrar denenir |
| **Kolay büyür** | Yük artınca aynı sıradan çeken işçi sayısı artar |

### 13.2 Supabase ile

| Parça | İş |
|---|---|
| **Supabase Queues** (`pgmq`) | Kuyruğun kendisi — veritabanı içinde |
| **Veritabanı tetikleyicisi** | Olay olunca kuyruğa iş yazar |
| **Edge Function** | İşçi — kuyruktan alır, yapar |
| **`pg_cron` + `pg_net`** | İşçileri kısa aralıklarla uyandırır |

### 13.3 Motorun işçileri — her biri ayrı Edge Function

| İşçi | Kuyruk | Tetikleyen | Ne yapar |
|---|---|---|---|
| **Embedding** | `embedding` | Metin değişti | Metni vektöre çevirir → `embeddings` |
| **Eşleştirme** | `eslestirme` | Embedding bitti · filtre / puan alanı değişti · [Daha fazla getir] · pasif kullanıcı Keşfet'i açtı | Filtre + vektör + puan → sonuç tablosu |
| **Ayrıştırma** | `ayristirma` | LLM ilan kaydında cevap vermedi | LLM'i tekrar dener, etiketleri sessizce uygular |

> Bildirim işçisi → `kesfet-ve-surec.md` · Zamanla çalışan bakım işleri → `bakim-isleri.md`

**Neden her biri ayrı:**

| Tek fonksiyonda olsaydı | Ayrı oldukları için |
|---|---|
| Biri takılınca hepsi takılır | Embedding servisi yavaşladı → metni değişmeyenlerin eşleştirmesi devam eder |
| Ayrı büyütülemez | Sadece yükü artan işçi çoğaltılır |
| Süre sınırı paylaşılır | Her işçi kendi süresini kullanır |

**"İşçi" değil "işçi türü":** Her türden birden fazla kopya aynı kuyruktan iş çekebilir — kuyruk aynı işin iki işçiye gitmesini engeller.

### 13.4 Yüksek trafik için altı kural

| # | Kural | Ne demek | Neden |
|:---:|---|---|---|
| 1 | **Veri ve iş aynı anda yazılır** | Profil kaydı ile "embedding üret" işi aynı veritabanı işleminde | "Profil yayında ama hiç eşleşmedi" oluşamaz |
| 2 | **Aynı iş bir kez yapılır** | İşin kimliği = *varlık + metin özeti* | Aday 3 kez art arda kaydederse sadece son hali işlenir |
| 3 | **Toplu embedding** | 50-100 metin tek API çağrısıyla | Maliyet düşer, sağlayıcı sınırına takılma azalır |
| 4 | **Tekrar çalışsa sorun çıkmaz** | Sonuç "üstüne yaz" | İşçi bitirirken çökerse iş tekrar işlenir, bozulmaz |
| 5 | **Öncelik sırası** | Yeni yayın > güncelleme > toplu yenileme | Yoğun anda yeni kullanıcı beklemez |
| 6 | **Küçük partiler** | Her çağrıda sınırlı iş; büyük işler 500'lük parçalara | Edge Function süre sınırına takılmaz |

### 13.5 Kod yapısı — ağır iş SQL'de, işçi ince

| Parça | Nerede | Dil |
|---|---|---|
| Kuyruğa iş yazma | Veritabanı tetikleyicisi | SQL |
| Filtre + vektör + puan | Veritabanı fonksiyonları (`eslestir`) | SQL |
| İşçiler | Edge Function | TypeScript (Deno) |
| Zamanlama | `pg_cron` | SQL |

```typescript
// Eşleştirme işçisi — mantık bu kadar
const isler = await db.rpc('pgmq_read', { kuyruk: 'eslestirme', adet: 20 })
for (const is of isler) {
  await db.rpc('eslestir', { tip: is.varlik_tipi, id: is.varlik_id })
}
await db.rpc('pgmq_delete', { kuyruk: 'eslestirme', isler })
```

**Neden ağır iş veritabanında:** Veri orada — binlerce satırı dışarı taşımak yavaş. Kural değişince tek SQL fonksiyonu değişir; uygulama ve işçiler etkilenmez.

### 13.6 Taşınabilirlik

Edge Function'lar Deno ile yazılıyor; **aynı kod değiştirilmeden bir container'da da çalışır.** Şartları: işçi **ince**, **durumsuz**, **standart bağlantı** kullanır.

| Taşıma sinyali | Karar |
|---|---|
| Kuyruk sürekli birikiyor · iş süreleri sınıra yaklaşıyor · çağrı maliyeti sürekli açık işçiyi geçiyor | O işçi container'a taşınır (Fly.io / Cloud Run) — diğerleri etkilenmez |

> Supabase'in Edge Function süre ve eşzamanlılık sınırları plana göre değişir — kurulumdan önce kontrol edilmeli.

---

## 14. Mimari ve Ölçek

> **Karar:** Mimari ilk günden **1 milyon+ kullanıcıya** göre kuruluyor — büyüme anında taşıma, yeniden yazma, kesinti olmasın.
> **Bilinçli bedeli:** Kullanıcı azken de bazı parçalar (okuma kopyası gibi) için ödeme yapılır.

### 14.1 Genel şema

```
 Flutter / Next.js
        │
        ▼
 ┌──────────────────────────────────────────┐
 │ SUPABASE                                 │
 │  Auth · RLS · Edge Functions (istek anı) │
 │                                          │
 │  Ana Postgres ── bölümlü tablolar        │
 │       │                                  │
 │       ├─▶ Kuyruklar (Supabase Queues)    │
 │       │      ├─ embedding  ─▶ işçi       │
 │       │      ├─ eslestirme ─▶ işçi       │
 │       │      └─ ayristirma ─▶ işçi       │
 │       │                                  │
 │       └─▶ Okuma kopyası ◀── Keşfet okur  │
 └──────────────────────────────────────────┘
        │
        ├─▶ Önbellek (Keşfet ilk sayfası)
        └─▶ Embedding API · LLM API
```

### 14.2 İlk günden kurulan parçalar

| Parça | Karar | Neden |
|---|---|---|
| **Bölümleme** | Sonuç **32**, aksiyon **16** (hash, `ilan_id`) | Boş tabloyu bölmek kolay, dolu tabloyu bölmek zor |
| **İşçiler** | Her biri ayrı Edge Function, taşınabilir | Ek sunucu yok; gerekirse tek tek container'a |
| **Okuma kopyası** | Keşfet ve listeler buradan okur | Okuma, yazma yüküyle yarışmaz |
| **Önbellek** | Keşfet'in ilk sayfası kısa süreli (örn. Upstash Redis) | Tekrar açılışta veritabanına gidilmez |
| **Vektör arama** | `pgvector` + tüm aramalar **`vektor_ara()`** arkasında | Gerekirse ayrı vektör veritabanına geçiş tek fonksiyon |
| **Servis soyutlamaları** | `embed()`, `ayristir()`, `eslestir()` | Sağlayıcı / yer değişince tek yerde |
| **Bölge** | Tek bölge + bölgeye hazır şema (`ulke_kodu`, kişisel veri ile ilan verisi ayrı) | Çok bölge ancak lansman ülkeleri netleşince |
| **Ölçüm** | Kuyruk uzunluğu, iş süresi, hata oranı, okuma kopyası gecikmesi | Darboğaz tahminle değil veriyle görülür |

### 14.3 Bölümleme

| Tablo | Anahtar | Bölüm |
|---|---|:---:|
| `ilan_aday_eslestirme` (sonuç) | Hash — `ilan_id` | **32** |
| `ilan_aday_aksiyon` | Hash — `ilan_id` | **16** |

**Neden `ilan_id`:**

| | |
|---|---|
| Hiç değişmez | Satır bölümler arasında taşınmaz |
| İşveren sorgusu | **Tek bölüme** gider |
| İlan kapanınca silme | Tek bölümde |

| Reddedilen anahtar | Neden |
|---|---|
| Ülke | Uzaktan ilanlar ülke sınırını aşıyor — satır hangi bölüme gidecek belirsiz |
| Durum | Durum değişince satır fiziksel taşınır |
| Tarih | Tüm yazma son bölüme yığılır |

```
5 milyon aday × ~200 eşleşme ≈ 1 milyar satır
32 bölüm → bölüm başına ~31 milyon     (rahat; zorlanma ~100 milyonda başlar)
```

- **Sayı sabit** — `hash(ilan_id) % 32`; değiştirmek tüm veriyi taşımak demek, bu yüzden baştan cömert
- **Aday sorgusu** 32 bölüme bakar — her bölümde `aday_id` indeksi; okuma kopyası ve önbellek yükü alır

> Mesajlar, görüntülemeler, bildirimler gibi diğer tabloların bölümlemesi → ilgili dokümanlar

### 14.4 Neden ayrı vektör veritabanı ilk günden değil

Vektörleri iki yerde tutmak **senkronizasyon sorunu** — birinde güncellenip diğerinde güncellenmeyen vektör sessizce yanlış eşleşme üretir. `pgvector` milyonlarca vektörde çalışıyor; `vektor_ara()` sayesinde gerekirse geçiş tek fonksiyon. **Geçiş sinyali:** filtre ile indeks çekişmesinin (Bölüm 8.3) ayarla çözülememesi.

### 14.5 Bölgeler — ileride

Yasalar veriyi bölge içinde tutmayı isterse:

| Veri | Nerede |
|---|---|
| Aday verisi (kişisel) | Kendi bölgesinde |
| İlan verisi (kişisel değil) | **Tüm bölgelere kopyalanır** — uzaktan ilanlar her bölgeden görünsün |

Şema hazır; karar lansman planıyla verilir.

### 14.6 Darboğaz beklentisi

| Muhtemel darboğaz | Önlem |
|---|---|
| Vektör arama (uzaktan ilanlarda geniş havuz) | Kısmi HNSW, `ef_search`, `vektor_ara()` |
| Embedding API sınırı | Toplu gönderim, kuyruk, tekrar deneme |
| Popüler ilan × binlerce aday | Parçalı işler, işçi kopyası, gerekirse container |
| Edge Function sınırları | Küçük partiler, taşıma sinyalleri |
| Aday sorgusu 32 bölüme bakıyor | İndeks, okuma kopyası, önbellek; gerekirse aday yönü için ikinci tablo |

---

## 15. Yayından Önce İki Test

> ⚠️ **İkisi de atlanırsa sonraki aylar boşa gidebilir.**

### 15.1 Doğrulama testi — eşleşme kalitesi

**Arayüz yazmadan önce:**
1. 20-30 gerçek aday profili ve 5-10 gerçek ilan **elle** girilir
2. Eşleştirme çalıştırılır
3. Sonuçlar gerçek kişilere gösterilir — *"Bu ilanlar senin için uygun mu?"* / *"Bu adaylar ilanına uygun mu?"*
4. Aynı setle **embedding sağlayıcıları** ve **LLM doğruluğu** karşılaştırılır
5. Zayıfsa: ağırlıklar, formüller, model değiştirilir

**Neden:** Eşleştirme kalitesi ürünün tamamını belirliyor. Arayüzden sonra "eşleştirme kötü" anlaşılırsa üstüne kurulan her şey sorgulanır.

### 15.2 Yük testi — hız ve dayanıklılık

```
1. Sahte veri: 5 milyon aday · 500 bin ilan · 1 milyar sonuç satırı
2. Trafik: binlerce eşzamanlı Keşfet açılışı · saatte binlerce güncelleme ·
           "Pazartesi sabahı" pasif kullanıcı dönüşü · tek popüler ilan × on binlerce aday
3. Ölç:   Keşfet açılış süresi · kuyruk birikmesi · işçi süreleri · okuma kopyası gecikmesi
4. Hedef: Keşfet < 300 ms · kuyruk dakikalar içinde boşalıyor · işçiler süre sınırı altında
5. Zorlanan yeri bul → düzelt → tekrar ölç
```

**Neden:** Hiçbir mimari için "hiç yavaşlamaz" garantisi verilemez — en yakın güvence, gerçek yükten önce ölçmek.

---

## 16. Veriyle Ayarlanacaklar

Tasarım kararları verildi. Kalanlar **ilk gerçek veriyle** ayarlanacak sayılar — hepsi veritabanında, kod değişmeden:

| # | Konu | Başlangıç | Nasıl karar verilecek |
|:---:|---|---|---|
| 1 | Embedding sağlayıcısı | OpenAI veya Cohere | Doğrulama testinde yan yana |
| 2 | Puan ağırlıkları | 0.65 / 0.20 / 0.10 / 0.05 | Gerçek eşleşmelere gözle bakılarak |
| 3 | Tecrübe kademeleri | İşveren 0.8 / 0.6 / 0.4 · Aday 0.7 / 0.4 / 0.2 | Davet kabul oranlarına göre |
| 4 | Uyum seviyesi eşikleri | 0.75 / 0.55 | Puan dağılımına göre |
| 5 | Aktif kullanıcı eşiği | 14 gün | Kullanıcı davranışı |
| 6 | Liste boyutu | 200 | Kullanıcıların kaç kart incelediği |
| 7 | Günlük özet eşiği | İlk 50 | Bildirim açılma ve kapatılma oranı |
