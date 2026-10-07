# İş Eşleştirme Projesi — Eşleştirme Motoru

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak
> **Sürüm: final v2** — ekip toplantısı sonrası; ilk günden 1M+ mimari
> İlgili dokümanlar: `aday-profili-v3.md`, `is-ilani-v3.md`, `staj-ilani.md`, `isveren-paneli-v2.md`, `auth-kararlari.md`
> Kapsam dışı: **Bakım işleri** (süreler, temizlik, otomatik düşme, sayaç düzeltme, silmeler) → `bakim-isleri.md`

---

## 0. Yönetici Özeti

Eşleştirme motoru **ürünün kalbi.** Aday profili, ilan formu, şirket doğrulama — hepsi bu motora doğru veri hazırlamak için var.

**Tek cümleyle:**

> Katı filtreyle uygun olmayanları eler, kalanları embedding ile anlamsal yakınlığa göre daraltır, dört parçalı bir skorla **iki tarafın da çıkarına göre ayrı ayrı** sıralar ve sonucu önceden hazırlar.

### Aşamalar

```
1. EMBEDDING       Metin → vektör                  (metin değişince, bir kez)
2. AYRIŞTIRMA      İlan metni → etiket listesi      (sadece ilan, LLM, bir kez)
       ↓
3. KATI FİLTRE     5.000.000 → ~500                 "Uygun mu?"   — eler
4. VEKTÖR          ~500 → en benzer 200             "En yakınlar"
5. SKOR            200 çift × 2 yön                 "Ne kadar uygun?" — sıralar
       ↓
6. SONUÇ TABLOSU   Önbellek — Keşfet buradan okur, hesap yapmaz
```

### Kritik tasarım kararları

| Karar | Gerekçe |
|---|---|
| **LLM sadece ilan kaydında, metni listeye çevirmek için** | İşveren yetenekleri cümleyle yazıyor; "React 5 yıl" bilgisini **sayı olarak** çıkarmak için gerekli. Sıralama ve skorlamada LLM **yok** — maliyet, hız, açıklanabilirlik |
| **Aday tarafında LLM yok** | Aday her şeyi yapılandırılmış giriyor (yetenek + yıl). Aday sayısı ilan sayısının katları — maliyet ve hata riski katlanırdı |
| **Embedding + etiket — iki yol** | Embedding genel benzerliği, etiket karşılaştırması kesin şartları ölçer. Biri tek başına yetmez (Bölüm 2) |
| **CV, belgeler, sertifika dosyaları eşleştirmeye girmez** | Herkes aynı kurallarla, kendi girdiği yapılandırılmış alanlara göre değerlendirilir — adil ve savunulabilir |
| **Şirket profili eşleştirmeye girmez** | Eşleştirme ilan bazlı; aynı şirketin ilanları birbirine benzemesin |
| **İlan konumu şirketten bağımsız** | İş, ofisten farklı yerde olabilir (saha, şantiye, İK ajansının müşterisi) |
| **Konum skorda değil, katı filtrede** | Aday konumlarını kendisi seçti; aralarında sıralama yapmak kararını ikinci kez yorumlamak olur |
| **Koşullu şartlar iki tarafta zorunlu seçim** | Sürücü belgesi, seyahat, vardiya, askerlik — "aday boş bıraktı, sessizce elendi" durumu yok |
| **Görmek ≠ süzmek** | Cinsiyet ve askerlik işverene görünür ama **hiçbir filtrede kullanılamaz** |
| **Eşik değil limit** | Kıtlıkta boş ekran olmasın — her zaman en iyi 200 |
| **İki yön, iki skor** | İşveren ve adayın çıkarı farklı (fazla nitelikli aday örneği — Bölüm 5.5) |
| **Tek sonuç tablosu** | Hangi taraf tetiklerse diğerinin listesi de otomatik güncellenir |
| **Karma hesaplama** | Aktif kullanıcı önceden, pasif kullanıcı açınca — gereksiz hesap ~%85 azalır |
| **Her şey kuyrukla, asenkron** | Kullanıcı beklemez; ani yük emilir; hiçbir iş kaybolmaz |
| **İlk günden 1M+ mimari** | Bölümleme, okuma kopyası, önbellek, ayrı işçiler ilk günden — büyüme anında taşıma ve kesinti olmasın |
| **Kartta ayıkla, detayda karar ver** | Başvuru ve davet karşı tarafın tamamı görüldükten sonra — uyumsuz başvuru yığılmasın |
| **Tüm ağırlıklar veritabanında** | Kod değişmeden gerçek veriyle ayarlanır |

### Bu sürümde değişenler

| Konu | Önceki | Şimdi |
|---|---|---|
| LLM | Hiç yok | İlan kaydında ayrıştırma — **onay ekranı yok**, "Algılanan" satırı + [Düzenle] |
| İlan vektörü | Yetenek adları + iş tanımının 4 sorusu | **7 metin, ham haliyle** — unvan, 5 kutu, iş tanımı |
| Yetenek skoru | Sadece yetenekler | **"Nitelik uyumu"** — yetenek + sertifika + bölüm |
| İlan konumu | Şirket lokasyonundan kopya | **İlanın kendi konumu** |
| Eğitim filtresi | "En az seviye" | **Kimler başvurabilir + seviye**, tüm kayıtlar |
| Meslek alanı | Ana + ikincil | **Tek** |
| Çalışma tipi | İlanda tek seçim, adayda yok | **İki tarafta çoklu**, katı filtre |
| Diğer şartlar | Opsiyonel, boş bırakılabiliyordu | **İki tarafta zorunlu seçim** |
| Askerlik | Filtrede yok | **Sadece tecilli elenir**; stajda yok |
| Tecrübe — staj | Açık soru | **0.5 nötr** |
| Kalite kriterleri | Eski alanlara göre | Zorunlu hale gelen alanlar çıkarıldı, yeniden dağıtıldı |
| Aksiyonlar | "Geç" | **"Bir daha gösterme"** (onaylı) + Gizlediklerim |
| Cevapsız başvuru / davet | Belirsiz | **30 gün / 10 gün** → süresi doldu |
| Liste | Sabit 200 | 200 + **[Daha fazla getir]** |
| Arka plan | `pg_cron` | **Kuyruk + işçiler** (Supabase Queues + Edge Functions) |
| Bildirim | Herkese | Push **FCM**, işverende **sadece ilanın sorumlusuna** · 24 maddelik katalog |
| Bölümleme | "Şimdi değil" | **İlk günden** — sonuç 32, aksiyon 16, mesajlar 16 (hash); zaman bölümleri |
| İşçiler | `pg_cron` işleri | **Her biri ayrı Edge Function**, gerekirse container'a taşınabilir |
| Kartlar | Butonlar kartta | **Kartta ayıklama + incele, detayda başvur / davet et**; her yerde **"Neden önerildi? ▾"** |
| Listeler | — | İki tarafta **Bekleyen / Sonuçlanan**; her kartta o anki durum |
| Geri çekme | Yok | Aday başvuruyu, işveren daveti geri çekebilir; tekrar yok |
| Başvuru kaydı | — | Tarihler + **skor anlık görüntüsü**; belge erişimi sonuçlanınca kapanır |

---

## 1. Embedding

### 1.1 Embedding nedir, neden var

Embedding, bir metni **anlamını taşıyan sayı dizisine** (vektör) çevirir. Anlamca yakın metinlerin vektörleri birbirine yakın düşer:

```
"Ödeme altyapısında mikroservis geliştirdim"
"Ödeme sistemlerimizde dağıtık mimari üzerinde çalışacaksınız"
→ kelimeler farklı, vektörler yakın
```

Bu sayede adayın kendi cümleleriyle yazdığı deneyim, işverenin farklı kelimelerle yazdığı iş tanımıyla eşleşebiliyor. **Çok dilli model** kullanıldığı için "Buchhaltung" ile "muhasebe" de yakın çıkıyor — çeviri gerekmiyor.

### 1.2 Aday vektörüne girenler

| # | Alan | Profil adımı | Zorunlu mu | Ne girer | Ne girmez |
|:---:|---|:---:|:---:|---|---|
| 1 | **Unvan** | 2 | ✅ | Unvan — **metnin ilk satırı** | — |
| 2 | **Kısa tanıtım** | 1 | ✅ (min 50) | Tamamı | — |
| 3 | **Deneyim** | 4 | Varsa | Her kayıtta **pozisyon + "bu işte ne yaptın"** | Şirket adı, tarihler, çalışma türü |
| 4 | **Eğitim** | 3 | Ön lisans+ zorunlu | **Tüm kayıtların bölüm adları** | Okul adı, seviye, durum, yıl |
| 5 | **Yetenekler** | 5 | Varsa | **Yetenek adları** | Yıl |
| 6 | **Sertifikalar** | 7 | Varsa | **Sertifika adları** (süresi geçmiş olanlar dahil) | Kurum, tarihler, belge dosyası |
| 7 | **Projeler** | 8 | Varsa | **Proje adı + açıklama** | Link, teknolojiler, tarih |

**Kural:** Ne doluysa girer, boş olan girmez. Deneyimi olmayan adayın vektörü unvan + tanıtım + bölüm + (varsa) yetenek, sertifika, proje ile oluşur — ek bir telafi kuralı yok.

**Neden unvan ilk satır:** Embedding modelleri metnin başına biraz daha fazla ağırlık verme eğiliminde. Unvan adayın ne iş yaptığını en kısa ve net söyleyen alan.

**Neden kısa tanıtımın zorunlu olması önemli:** Eskiden deneyimsiz ve tanıtımsız bir adayın vektörü sadece *"Unvan + Bölüm"* idi — çok kaba. Artık en minimal profilde bile adayın kendi cümleleriyle yazdığı bir metin var.

**Neden "ne yaptın" en zengin kaynak:** CV, şirket profili, belgeler eşleştirmeye girmiyor. Adayın gerçekte ne iş yaptığı — *"ödeme altyapısı, mikroservis, performans"* — sadece bu metinlerde yazıyor.

**Neden şirket adı girmez:** Girerse aynı şirketten gelenler birbirine yakın çıkar — iş içeriği değil şirket benzerliği eşleşir. Ayrıca "şirket adını gizle" vektörde de korunmuş olur.

**Neden okul adı girmez:** Aynı okuldan mezunlar birbirine yakın çıkar — okul bazlı önyargı. Global ölçekte belirli ülkelerin üniversiteleri sistematik avantaj kazanırdı.

**Neden yetenek yılı girmez:** Sayılar vektörde anlamsız — "React 5 yıl" ile "React 1 yıl" neredeyse aynı görünür. Yıl, skorun **nitelik uyumu** parçasında aritmetikle kullanılır.

**Neden projede "kullanılan teknolojiler" girmez:** Bunlar çoğu zaman adayın yetenek listesinde zaten var; ikinci kez girerse aynı kelimeler vektörde fazla ağırlık kazanır.

### 1.3 İlan vektörüne girenler

| # | Alan | İlan adımı | Zorunlu mu | Embedding (YOL 1) | Etiket → skor (YOL 2) |
|:---:|---|:---:|:---:|:---:|:---:|
| 1 | **Unvan** | 1 | ✅ | ✅ İlk satır | ❌ |
| 2 | **Olmazsa olmaz yetenekler** | 2 | ❌ | ✅ Ham metin | ✅ Ad + yıl |
| 3 | **Tercih edilen yetenekler** | 2 | ❌ | ✅ Ham metin | ✅ Ad |
| 4 | **Tercih edilen bölümler** | 2 | ❌ | ✅ Ham metin | ✅ Bölüm adları |
| 5 | **Olmazsa olmaz sertifikalar** | 2 | ❌ | ✅ Ham metin | ✅ Ad |
| 6 | **Tercih edilen sertifikalar** | 2 | ❌ | ✅ Ham metin | ✅ Ad |
| 7 | **İş tanımı / staj tanımı** | 3 | ✅ (min 100) | ✅ Tamamı | ❌ |

İş ilanı ve staj ilanı **aynı yapıyı** kullanır.

**Neden iş tanımı sadece embedding'e:** İş tanımı "ne iş yapılacak"ı anlatıyor — "var/yok" diye sayılacak bir şeyi yok. Sayılacak şeyler (yetenek, sertifika, bölüm) kendi kutularında.

**Neden kutuların ham metni, etiketler değil:** Ham metin bağlamı taşır — *"PostgreSQL'e hakim, yüksek trafikli sistemlerde"* gibi ifadeler etiket listesinde kaybolurdu. **Etiketler skor için, ham metin anlamsal eşleşme için.** Ham metindeki "5 yıl" gibi sayılar vektöre girer ama zararsızdır — vektör onları anlamlı okumaz, yıl karşılaştırmasını skor yapar.

### 1.4 Hiçbir zaman girmeyenler

| Kategori | Örnekler | Neden |
|---|---|---|
| **Sayısal alanlar** | Tecrübe yılı, maaş, tarihler | Vektörde anlamsız — aritmetikle hesaplanır |
| **Katı filtre alanları** | Konum, meslek alanı, dil, çalışma şekli/tipi, eğitim seviyesi | Zaten kesin olarak filtreleniyor; çift sayılmasın, sinyali seyreltmesin |
| **Kişisel / hassas** | Ad, doğum tarihi, cinsiyet, askerlik, fotoğraf | Ayrımcılık riski |
| **Şirket profili** | Tanıtım, imkanlar, sektör | Aynı şirketin farklı pozisyonları birbirine benzemesin |
| **CV, belgeler, sertifika dosyaları** | | Herkes aynı kurallarla değerlendirilsin |

> **İki altın kural:**
> 1. Sayılar embedding'e değil, aritmetiğe.
> 2. Katı filtre alanları embedding'e değil, filtreye.

### 1.5 Saklama

```sql
CREATE TABLE embeddings (
  varlik_tipi  text   NOT NULL,         -- 'aday' | 'ilan'
  varlik_id    bigint NOT NULL,
  model_adi    text   NOT NULL,         -- hangi modelle üretildi
  boyut        int    NOT NULL,         -- 1024
  metin_hash   text   NOT NULL,         -- 7 metnin özeti
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
| **Ayrı tablo** (profil / ilan tablosunda değil) | Ana tablolar hafif kalır (vektör ~4 KB); model değişimi tek tabloda; 1M+ ölçekte vektörler ayrı bir vektör veritabanına tek parça taşınır |
| **Her varlık için tek satır** | Metin değişince üstüne yazılır, eski vektör tutulmaz |
| **İki kısmi indeks** | İlan için aday ararken ilan vektörleri arasında dolaşılmaz |
| **`metin_hash`** | Kullanıcı sık kaydeder ama metin çoğu zaman aynıdır (sadece konum/maaş değişmiştir). Hash aynıysa API'ye gidilmez |
| **`model_adi`, `boyut`** | Model değişince eski vektörler bulunup yeniden üretilir |
| **Staj ayrı tür değil** | `ilanlar.ilan_tipi = 'staj'`, `varlik_tipi = 'ilan'` |

### 1.6 Embedding servisi

| Karar | Gerekçe |
|---|---|
| **API ile başla** | Başlangıçta kendi sunucu kurmak gereksiz yük |
| **Tek çok dilli model** | Tüm diller aynı vektör uzayında — çeviri gerekmez |
| **1024 boyut** | `pgvector` HNSW indeksi 2000 boyuta kadar destekliyor; kalite kaybı çok az |
| **`embed()` tek fonksiyon arkasında** | Sağlayıcı değişimi tek yerde |
| **İstemciden asla çağrılmaz** | API anahtarı Flutter / Next.js'e gömülürse çalınır — sadece sunucuda |

**Adaylar:** OpenAI `text-embedding-3-large` (`dimensions=1024`) veya Cohere `embed-multilingual-v3`.

> 💬 **Seçim masa başında yapılmamalı:** 20-30 gerçek profil ve 5-10 gerçek ilanla iki modeli yan yana test edin — hangisi doğru adayı üste çıkarıyor, gözle görülür.

> 💡 **Kendi servise geçiş sinyali:** Aylık fatura belirli bir eşiği aşınca veya veri yerleşimi gereksinimi doğunca. Açık kaynak çok dilli modeller (BGE-M3, multilingual-E5) aday.

---

## 2. İlan Metinleri — İki Yol

### 2.1 Neden iki yol

Embedding **bütün metni karıştırıp tek bir genel anlam** çıkarıyor. İlanın metninde yüzlerce kelime var; tek tek yeteneklerin varlığı ya da yokluğu bu genel anlamı çok az değiştiriyor.

**Örnek — olmazsa olmaz: React, .NET, Kubernetes**

```
Aday A: React
Aday B: React, .NET, Kubernetes
(geri kalan her şeyleri benzer — ikisi de backend geliştirici)
```

**Sadece embedding olsaydı:**
```
Aday A:  anlamsal = 0.84
Aday B:  anlamsal = 0.87      ← sadece 3 puan fark
```
İkisi de "backend geliştirici" anlamı taşıyor. A'nın tanıtımı biraz daha detaylıysa **A, B'nin önüne bile geçebilir** — işveren üç zorunlu yetenekten ikisi eksik adayı üstte görür.

**İki yol olunca:**
```
Aday A:  React ✅  .NET ❌  Kubernetes ❌   → olmazsa olmaz = 1/3 = 0.33
Aday B:  React ✅  .NET ✅  Kubernetes ✅   → olmazsa olmaz = 3/3 = 1.00

Aday A:  0.65 × 0.84 + 0.20 × (0.75 × 0.33) + …  ≈ 0.596
Aday B:  0.65 × 0.87 + 0.20 × (0.75 × 1.00) + …  ≈ 0.716
```
**Fark 3 puandan 12 puana çıktı.** B açıkça üstte.

| | Embedding | Etiket karşılaştırması |
|---|---|---|
| Neyi görür | "Ödeme altyapısı" ↔ "ödeme sistemleri" yakın | ".NET var mı?" → yok, kesin |
| Neyi göremez | Bir yeteneğin **olmadığını**, sayıları | Eş anlamlıları ("mikroservis" ↔ "dağıtık mimari") |

> **Embedding "genel olarak benziyor mu" der. Etiket "şart koşulan şeyler tek tek var mı" der.** Birlikte, embedding'in "neredeyse aynı" gördüğü iki adayı işverenin şartlarına göre ayırıyoruz.

### 2.2 Akış

```
İşveren yazar:
"5 yıl React bilen, 8 yıl .NET tecrübesi olan, PostgreSQL'e hakim"
        │
        ├── YOL 1 — Embedding
        │   Ham metin OLDUĞU GİBİ, LLM'e uğramadan ilan vektörüne girer
        │   → ANLAMSAL skor (%65)
        │
        └── YOL 2 — Etiket
            LLM cümleyi listeye çevirir
            [React · 5 yıl] [.NET · 8 yıl] [PostgreSQL]
            → adayın listesiyle TEK TEK karşılaştırılır
            → NİTELİK UYUMU (%20)
```

**LLM eşleştirme yapmıyor.** Sadece işverenin cümlesini, adayla karşılaştırılabilecek bir **listeye** çeviriyor. Karşılaştırmayı sistem basit aritmetikle yapıyor.

> LLM olmasaydı işverenden yetenekleri tek tek liste halinde girmesini istemek zorunda kalırdık. İşveren cümle yazmak istediği için LLM bu çeviriyi yapıyor.

### 2.3 Beş kutu

| Kutu | YOL 1 | LLM ne çıkarır | Hangi kümeye gider |
|---|:---:|---|---|
| Olmazsa olmaz yetenekler | Ham metin | Ad **+ yıl** | Olmazsa olmaz (0.75) |
| Tercih edilen yetenekler | Ham metin | Ad | Tercih edilen (0.25) |
| Tercih edilen bölümler | Ham metin | Bölüm adları | Tercih edilen (0.25) — **tek öğe** |
| Olmazsa olmaz sertifikalar | Ham metin | Ad | Olmazsa olmaz (0.75) |
| Tercih edilen sertifikalar | Ham metin | Ad | Tercih edilen (0.25) |

**Olmazsa olmaz / tercih edilen ayrımını kutu belirler** — LLM tahmin etmez. Tek kutu olsaydı LLM "şart", "artı" gibi kelimelerden tahmin yapmak zorunda kalırdı.

**Tercih edilenlerde de neden iki yol:** Yıl olmasa da "var mı, yok mu" sorusunu embedding cevaplayamıyor. Docker ve Kubernetes'i bilen aday ile bilmeyen, embedding'de neredeyse aynı puanı alır (~2 puan fark); etiket karşılaştırması farkı net koyar (1.0 vs 0.0). Ek maliyet yok — aynı LLM isteğinde ayrıştırılıyor.

### 2.4 Onay ekranı yok — "Algılanan" satırı

LLM sonucu **işverene onaylatılmaz.** Her kutunun altında algılanan etiketler görünür:

```
Olmazsa olmaz yetenekler
┌──────────────────────────────────────────────────┐
│ 5 yıl React bilen, 8 yıl .NET tecrübesi olan     │
└──────────────────────────────────────────────────┘
Algılanan:  React · 5 yıl   .NET · 8 yıl                [Düzenle]
```

| | |
|---|---|
| Nerede görünür | İlan düzenleme ekranında, önizlemede, adayın gördüğü ilan detayında |
| Zorunlu aksiyon | **Yok** — işveren hiçbir şeye basmak zorunda değil |
| [Düzenle] | Etiket ekle / sil / adı düzelt / yılı değiştir |

**Neden onay yok:** İşveren için bir ekran ve bir karar daha demekti; "onay bekliyor" durumları, hatırlatmalar, otomatik kabul gibi yan kurallar doğuruyordu. Etiketlerin görünür ve düzenlenebilir olması, kaliteyi zorlamadan koruyor.

### 2.5 Etiket tablosu

```sql
CREATE TABLE ilan_etiketleri (
  ilan_id       bigint NOT NULL,
  tur           text   NOT NULL,   -- yetenek | sertifika | bolum
  oncelik       text   NOT NULL,   -- olmazsa_olmaz | tercih_edilen
  ad            text   NOT NULL,   -- 'React'
  normalize_ad  text   NOT NULL,   -- 'react'  (karşılaştırma için)
  yil           int,               -- sadece olmazsa olmaz yetenekte, varsa
  kaynak        text   NOT NULL    -- llm | isveren_duzeltti
);
```

| Tablo | Hangi skor parçası |
|---|---|
| `embeddings` | Anlamsal |
| `ilan_etiketleri` | Nitelik uyumu |

**`kaynak` neden var:**
- **Açıklanabilirlik:** Bir etiketin LLM'den mi, işverenin düzeltmesinden mi geldiği bilinir
- **Koruma:** Metin sonradan değişip LLM yeniden çalışırsa, **işverenin düzelttiği etiketler korunur** — LLM üstüne yazmaz
- **Kalite izleme:** İşverenlerin ne sıklıkla düzelttiği ölçülür

Aday tarafında ayrıca etiket tablosu yok — adayın yetenek, sertifika ve eğitim tabloları zaten yapılandırılmış.

### 2.6 LLM kalitesi

Onay olmadığı için kalite sistem tarafında korunur:

| Önlem | Ne yapar |
|---|---|
| **Kesin çıktı formatı** | LLM serbest metin değil, belirli yapıda liste döndürür |
| **Örnekli talimat** | Farklı dillerde ve yazım şekillerinde örneklerle yönlendirilir |
| **Mantık kontrolü** | Yıl 0-40 dışında mı, aynı etiket iki kez mi — hatalı çıktı reddedilir, tekrar denenir |
| **Test seti** | 100-200 gerçek ilan cümlesiyle doğruluk ölçülür; model veya talimat değişince tekrar ölçülür |
| **İzleme** | [Düzenle] kullanım oranı takip edilir — artarsa LLM sorunu var |
| **Küçük, hızlı model** | Görev basit; ilan başına maliyet bir sentin çok altında |
| **Sadece metin değişince** | 5 kutunun hash'i tutulur; maaş veya konum değişince LLM çağrılmaz |

### 2.7 LLM cevap vermezse

İşveren hiçbir zaman takılmaz:

```
İşveren [Kaydet] → LLM cevap vermedi
  → İlan YİNE kaydedilir, yayınlanabilir
  → Kuyruğa: "İlan #5 → ayrıştır"
  → Geçici: adayın yetenek adları ilan metninde aranır (yılsız, var/yok)

Ayrıştırma işçisi tekrar dener → başarılı
  → Etiketler SESSİZCE uygulanır (bildirim yok)
  → Eşleştirme yenilenir
```

| Skor parçası | Ayrıştırma bitmeden |
|---|---|
| Anlamsal (%65) | ✅ Normal — ham metin LLM'e muhtaç değil |
| Nitelik uyumu (%20) | ⚠️ Geçici yöntem |
| Tecrübe, kalite | ✅ Normal |

---

## 3. Katı Filtre

### 3.1 Amacı

Vektör ve skor hesabından **önce** uygun olmayanları eler. **Kimseyi sıralamaz, sadece eler.**

```
Katı filtre:  "Uygun mu?"      → evet / hayır
Skor:         "Ne kadar uygun?" → 0-1 arası puan
```

React bilmeyen aday React isteyen ilandan **elenmez** (yetenek katı filtre değil) — ama skoru düşük olduğu için listenin altında kalır.

### 3.2 Filtre sırası

En çok eleyen önce — sonraki filtreler daha küçük havuzda çalışır:

| # | Filtre | Karşılaştırma |
|:---:|---|---|
| 1 | **Sistem kontrolleri** | İlan yayında · aday yayında · arama durumu |
| 2 | **Konum** | Adayın istediği konumlar ↔ ilanın konumu |
| 3 | **Meslek alanı** | Tek eşitlik |
| 4 | **Ne arıyor** | İş / staj |
| 5 | **Çalışma şekli** | Ortak seçenek |
| 6 | **Çalışma tipi** | Ortak seçenek *(sadece iş ilanı)* |
| 7 | **İş dili** | Ortak dil |
| 8 | **Eğitim** | Kimler başvurabilir + seviye |
| 9 | **Diğer şartlar** | Sürücü belgesi, seyahat, vardiya, askerlik |
| 10 | **Dışlamalar** | Gizlenen şirketler, önceki aksiyonlar |

### 3.3 Filtre 1 — Sistem kontrolleri

| İlan | Aday |
|---|---|
| Durum = **yayında** | Profil **yayında** — zorunlu adımlar tamam **ve telefon doğrulanmış** |
| | Arama durumu **"şu an bakmıyorum" değil** |

> Aday numarasını değiştirip doğrulamazsa profil yayından çıkar → bu filtrede elenir.

### 3.4 Filtre 2 — Konum

**Karşılaştırılan:** Adayın **çalışmak istediği konumlar** (en fazla 10) ↔ **İlanın kendi konumu**

> İlanın konumu = ilan oluşturulurken seçilen yer. Şirketin merkezi veya lokasyonları değil.

**Kural — üst seviye seçilirse altı dahil:**

| Aday seçimi | İlan konumu | Sonuç |
|---|---|:---:|
| Türkiye | Kadıköy, İstanbul | ✅ |
| İstanbul | Kadıköy, İstanbul | ✅ |
| İstanbul | Çankaya, Ankara | ❌ |
| Almanya | München, Bayern | ✅ |

```sql
i.konum_yolu LIKE ak.konum_yolu || '%'
-- 'TR/İstanbul/Kadıköy' LIKE 'TR/İstanbul%'  → eşleşir
```

**Konum filtresi atlanan iki durum:**

| Durum | Sonuç |
|---|---|
| Aday **"Her konumda çalışmaya hazırım"** işaretlemiş | Konum atlanır |
| Adayın ve ilanın çalışma şeklinde **ortak "uzaktan"** var | Konum atlanır |

| Aday | İlan | Ortak | Konum |
|---|---|---|---|
| Uzaktan | Uzaktan | Uzaktan | **Atlanır** — dünyanın her yerinden |
| Uzaktan | Ofis | — | Zaten elenir (Filtre 5) |
| Ofis, Uzaktan | Ofis | Ofis | **Uygulanır** — ofise gidecek |
| Hibrit | Hibrit | Hibrit | **Uygulanır** — hibrit konum gerektirir |

> **Global sonuç:** Uzaktan ilanlarda konum atlandığı için havuz tüm dünya oluyor — o durumda **meslek alanı ve iş dili** en güçlü filtreler.

### 3.5 Filtre 3 — Meslek alanı

```
ilan.meslek_alani = aday.meslek_alani
```

**Tek eşitlik** — ikincil meslek alanı kaldırıldı.

**Neden gerekli:** Embedding aynı kelimeleri kullanan farklı meslekleri karıştırabiliyor — *"proje yönetimi, ekip koordinasyonu, bütçe takibi"* yazan inşaat proje yöneticisi ile yazılım proje yöneticisi vektörde yakın çıkar.

**Neden ikincil alan kaldırıldı:** Aday "Yazılım Geliştirici" olup ikincil "Tarım" seçerse tarım ilanlarına düşer ama profili yazılım anlatır — alakasız ilanlar görür. Kariyer değiştiren aday meslek alanını değiştirir, profilini ona göre günceller; filtre ve vektör aynı yönü gösterir.

### 3.6 Filtre 4 — Ne arıyor

| İlan | Aday | Sonuç |
|---|---|:---:|
| İş | İş | ✅ |
| İş | Staj | ❌ |
| İş | İş + Staj | ✅ |
| Staj | Staj | ✅ |
| Staj | İş + Staj | ✅ |

### 3.7 Filtre 5 — Çalışma şekli

İki taraf çoklu (ofis / hibrit / uzaktan); **en az bir ortak.**

| Aday | İlan | Sonuç |
|---|---|:---:|
| Ofis, Hibrit | Hibrit | ✅ |
| Ofis, Hibrit | Uzaktan | ❌ |
| Uzaktan | Ofis, Uzaktan | ✅ *(konum atlanır)* |

### 3.8 Filtre 6 — Çalışma tipi *(sadece iş ilanı)*

İki taraf çoklu (tam zamanlı / yarı zamanlı / proje bazlı / dönemsel); **en az bir ortak.**

| Aday | İlan | Sonuç |
|---|---|:---:|
| Tam zamanlı | Tam, Yarı | ✅ |
| Yarı zamanlı | Tam | ❌ |
| Tam, Proje bazlı | Proje bazlı | ✅ |

**Neden iki tarafta çoklu:** Kafe, mağaza, çağrı merkezi *"tam veya yarı zamanlı"* der — tek seçim olsa işveren adayların yarısını kaybederdi. Yarı zamanlı arayan öğrenciye tam zamanlı ilan, tam zamanlı arayana yarı zamanlı ilan düşmemeli.

> **Staj ilanında yok** — onun yerine "çalışma düzeni" (tam / yarı zamanlı) var, **sadece gösterim** (öğrencinin müsaitliği dönemden döneme değişiyor).

### 3.9 Filtre 7 — İş dili

İki taraf çoklu, kapalı liste; **en az bir ortak dil.**

| Aday | İlan | Sonuç |
|---|---|:---:|
| Türkçe, İngilizce | İngilizce | ✅ |
| Türkçe | İngilizce | ❌ |

**Neden kritik:** Embedding çok dilli — Türkçe profil İngilizce ilanla anlamsal eşleşebilir, ama aday o ilanı okuyamayabilir. Dil filtresi bunu önler.

> Adayın "bildiğim diller + seviye" alanı filtreye **girmez** — sadece iş dilleri.

### 3.10 Filtre 8 — Eğitim

**İlan tarafı:**
```
Kimler başvurabilir *    ← tek seçim
○ Sadece devam eden öğrenciler
○ Sadece mezunlar
○ İkisi de
○ Eğitim şartı yok        ← sadece iş ilanında

Hangi seviyede *          ← çoklu
☐ İlkokul ☐ Ortaokul ☐ Lise ☐ Ön lisans ☐ Lisans ☐ Yüksek lisans ☐ Doktora
```

**Aday tarafı:** Her eğitim kaydında seviye + durum (devam ediyor / mezun).

**Kural:**

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

"Sadece mezunlar · Lisans"            → ✅ (lisans mezun kaydı var)
"Sadece öğrenciler · Yüksek lisans"   → ✅ (y.lisans devam kaydı var)
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

| Kural | |
|---|---|
| **ISCED ile karşılaştırma** | Ülkeler arası ortak ölçek — Almanya'daki "Bachelor" ile Türkiye'deki "Lisans" aynı seviye (6) |
| **Üst seviyeler otomatik işaretlenir** | Lisans işaretlenince yüksek lisans ve doktora da — işveren kaldırabilir |
| **İlkokul / ortaokul öğrencisi seçilemez** | Çocuk işçiliği — "ikisi de" seçilse bile bu seviyeler için sadece mezunlar geçer |
| **Staj ilanında** | Lise → doktora; "eğitim şartı yok" seçeneği yok |
| **Mezuniyet süresi sınırı** | Yok |

> ⚠️ **Eğitim artık katı filtre** — adayın doğru girmesi kritik. Seviye kapalı listeden, durum zorunlu, ülkesinin derece adıyla ve açıklamayla gösteriliyor; *"Sadece devam ettiğiniz veya mezun olduğunuz eğitimleri ekleyin"* notu var.

### 3.11 Filtre 9 — Diğer şartlar

**İki tarafta zorunlu seçim** — varsayılan seçili gelmez:

| Şart | İlan | Aday | Elenen |
|---|---|---|---|
| **Sürücü belgesi** | Gerekli değil / Gerekli + sınıf | Yok / Var + en az bir sınıf | Belgesi yok veya sınıf tutmuyor |
| **Seyahat** | Gerekli değil / Engeli olmamalı | Engel yok / Var | "Var" |
| **Vardiya** | Gerekli değil / Vardiyalı çalışabilmeli | Evet / Hayır | "Hayır" |
| **Askerlik** *(sadece iş ilanı)* | Şart yok / Engeli olmamalı | Yapıldı / Muaf / Tecilli / Yükümlü değil | Sadece **tecilli** |

```sql
(NOT i.surucu_gerekli  OR a.surucu_siniflari && i.surucu_siniflari)
(NOT i.seyahat_gerekli OR a.seyahat_engeli = false)
(NOT i.vardiya_gerekli OR a.vardiya_uygun = true)
(NOT i.askerlik_sarti  OR a.askerlik_durumu <> 'tecilli')
```

**Filtre sadece ilan şart koyduysa çalışır.** "Gerekli değil" seçildiyse o alana bakılmaz.

**Neden iki tarafta zorunlu:** Eskiden aday bu alanları boş bırakabiliyor, şartlı ilanlardan **sessizce** eleniyordu. Artık boş durum yok — filtre tek satır, "doldurmadın" uyarısına gerek kalmadı.

**Askerlik — ayrımcılık önlemi:**
- Şartın adı **"askerlik engeli olmamalı"** — sadece işe başlayıp askere gitmek zorunda kalacak durumdakileri (tecilli) eler
- Kadın adaylar, askerliği olmayan ülkelerdeki adaylar **"Yükümlü değil"** seçer → geçer
- **Staj ilanında yok** — öğrencilerin çoğu tecilli
- İşverene profil detayında görünür ama **listede süzülemez**

### 3.12 Filtre 10 — Dışlamalar

**A — Gizlenen şirketler** *(sadece işveren yönünde)*

```sql
NOT EXISTS (SELECT 1 FROM aday_gizlenen_sirketler g
            WHERE g.aday_id = a.id AND g.sirket_id = i.sirket_id)
```

| | |
|---|---|
| Kaynak | Sadece **onaylı şirketler** — işveren başvurusunda admin'in onayladığı ad |
| Şirket bazlı | Şirketin tüm kullanıcıları (sahip, yönetici, uzman) ve tüm ilanları için |
| Tek yönlü | Aday o şirketin ilanlarını görmeye devam eder |

**B — Önceki aksiyonlar** → Bölüm 6.3

### 3.13 Tek fonksiyonda

```sql
CREATE FUNCTION uygun_adaylar(p_ilan_id bigint) ...
CREATE FUNCTION uygun_ilanlar(p_aday_id bigint) ...
```

> **Neden:** Kurallar değişecek. Sorgulara dağıtılırsa bir yerde unutulur ve sistem **sessizce** yanlış sonuç üretir.

### 3.14 İndeksler

```sql
-- Sadece yayındaki ilanlar — kısmi indeks
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

> Çoklu alanlar Postgres dizisi + `&&` (kesişim) operatörü ile — ayrı tablo + `EXISTS`'ten kat kat hızlı. **GIN indeksi zorunlu.**

---

## 4. Vektör Aşaması

### 4.1 Hesap

```sql
SELECT a.id, 1 - (ie.vektor <=> ae.vektor) AS anlamsal
FROM [katı filtre sonucu]
ORDER BY ie.vektor <=> ae.vektor
LIMIT 200;
```

`<=>` = kosinüs mesafesi. **Anlamsal benzerlik = 1 − mesafe.**

### 4.2 Eşik değil limit

> ❌ "0.7 üstü olanlar gösterilsin"
> ✅ "En iyi 200 alınsın"

- Kıtlık döneminde hiçbiri eşiği geçmezse **boş ekran** — halbuki içlerinde makul olanlar var
- Eşik değeri modele bağlı ve sezgisel değil

### 4.3 [Daha fazla getir] — tamamlama

Sabit 200 yerine:

```
1. Liste açılır → en iyi 200
2. Kullanıcı bazılarına aksiyon alır (davet, başvuru, bir daha gösterme)
   → bunlar listeden çıkar
3. Listenin sonunda: [Daha fazla getir]
4. Yeniden hesap → aksiyon alınanlar zaten katı filtrede elendiği için
   liste OTOMATİK olarak daha önce görülmemişlerle 200'e tamamlanır
5. Yeni sonuç yoksa → "Tüm uygun ilanları/adayları gördün" (Bölüm 10.10)
```

Ek mantık gerekmiyor — Filtre 10'un doğal sonucu. Maliyet: tek bir eşleştirme hesabı.

### 4.4 Vektör indeksi — HNSW

Vektörleri bir ağ yapısında tutar; birbirine yakın vektörler komşu olarak bağlanır. Arama her adımda hedefe en yakın komşuya atlar — yarım milyon kaydı taramak yerine birkaç yüz adımda sonuca varır. "Yaklaşık" arama: %95+ doğruluk, kat kat hız.

| Havuz (filtreden kalan) | Süre | Ne zaman |
|---|---|---|
| 500 | ~2-5 ms | Normal |
| 2.000 | ~5-20 ms | Geniş filtre |
| 50.000 | ~20-50 ms | Uzaktan ilan |
| 500.000 | ~50-150 ms | Uzaktan, dünya geneli, geniş alan |

> ⚠️ **Bilinen risk — filtre ile indeks çekişmesi:** Filtre çok seçiciyse HNSW'nin getirdiği yakın vektörlerin çoğu filtreden geçmeyebilir; Postgres sıralı taramaya düşebilir. Çözüm: `hnsw.ef_search` artırmak veya küçük havuzlarda indeks kullanmamak. **Gerçek veriyle `EXPLAIN ANALYZE` ile ölçülmeli.** İndeks ayarıyla çözülemezse filtre bilgisini vektörle birlikte saklayan ayrı vektör veritabanına geçilir — `vektor_ara()` soyutlaması sayesinde tek fonksiyon değişikliği (Bölüm 11.3).

---

## 5. Skor

### 5.1 Skor nedir

Katı filtreden geçenlerin **hepsi uygun** — ama hepsi aynı derecede değil. Skor, onları **en uygundan en aza** sıralamak için 0-1 arası puan.

| | Katı filtre | Skor |
|---|---|---|
| Soru | Uygun mu? | Ne kadar uygun? |
| Sonuç | Evet / hayır | 0-1 |
| Etkisi | **Eler** | **Sıralar** — kimseyi elemez |

### 5.2 Formül

```
skor = 0.65 × anlamsal
     + 0.20 × nitelik uyumu
     + 0.10 × tecrübe
     + 0.05 × kalite
```

| Parça | Ağırlık | Ne ölçüyor | Nereden geliyor | İki yönde |
|---|:---:|---|---|---|
| **Anlamsal** | %65 | Profil ile ilan genel olarak ne kadar benziyor | Embedding (YOL 1) | Aynı |
| **Nitelik uyumu** | %20 | İstenen yetenek, sertifika, bölüm adayda tek tek var mı | LLM etiketleri (YOL 2) ↔ adayın listeleri | Aynı |
| **Tecrübe** | %10 | Toplam tecrübe istenen aralıkta mı | Deneyim tarihleri ↔ ilanın min–max | **Farklı** |
| **Kalite** | %5 | Karşı tarafın profili / ilanı ne kadar dolu | Opsiyonel alanların doluluğu | **Farklı** |

### İki yön — iki skor

Aynı aday–ilan çifti için **iki** puan hesaplanır:

| | Kim kullanır | Soru |
|---|---|---|
| **İşveren skoru** | İşverenin "Adaylar" listesi | "Bu aday benim ilanıma ne kadar uygun?" |
| **Aday skoru** | Adayın "Keşfet" listesi | "Bu ilan bana ne kadar uygun?" |

```
skor_isveren = 0.65 × anlamsal + 0.20 × nitelik + 0.10 × tecrübe_isveren + 0.05 × aday_kalitesi
skor_aday    = 0.65 × anlamsal + 0.20 × nitelik + 0.10 × tecrübe_aday    + 0.05 × ilan_kalitesi
```

**Maliyet:** Pahalı kısım (vektör mesafesi) bir kez hesaplanır, iki skorda aynı sayı. Farklılaşan parçalar basit aritmetik.

### 5.3 Anlamsal — %65, iki yönde aynı

```
anlamsal = 1 − (aday.vektor <=> ilan.vektor)
```

| Aday vektörü | İlan vektörü |
|---|---|
| Unvan | Unvan |
| Kısa tanıtım | Olmazsa olmaz yetenekler (ham) |
| Deneyim: pozisyon + ne yaptın | Tercih edilen yetenekler (ham) |
| Bölüm adları | Tercih edilen bölümler (ham) |
| Yetenek adları | Olmazsa olmaz sertifikalar (ham) |
| Sertifika adları | Tercih edilen sertifikalar (ham) |
| Proje adı + açıklama | İş / staj tanımı |

**Neyi iyi yakalıyor:** *"Ödeme altyapısında mikroservis geliştirdim"* ↔ *"Ödeme sistemlerimizde dağıtık mimari"* — kelimeler farklı, anlam yakın.

**Neden en ağır:** "Bu kişi bu işi yapabilir mi" sorusunun en geniş cevabı bu. Diğer parçalar bunu kesin kriterlerle düzeltiyor.

**Neden iki yönde aynı:** Vektör mesafesi simetrik — A'nın B'ye yakınlığı = B'nin A'ya yakınlığı.

### 5.4 Nitelik uyumu — %20, iki yönde aynı

> Eski adı "yetenek skoru". Artık yetenek + sertifika + bölümü kapsadığı için **"nitelik uyumu."**

**Soru:** İlanın istediği yetenek, sertifika ve bölümler adayda **tek tek** var mı?

#### İki küme

| Küme | İçinde | Ağırlık |
|---|---|:---:|
| **Olmazsa olmaz** | Olmazsa olmaz yetenekler + olmazsa olmaz sertifikalar | **0.75** |
| **Tercih edilen** | Tercih edilen yetenekler + tercih edilen sertifikalar + bölüm *(tek öğe)* | **0.25** |

#### Her öğenin puanı

| Öğe | Adaydaki durum | Puan |
|---|---|:---:|
| **Yetenek — olmazsa olmaz, yıl istenmiş** | Var, yıl ≥ istenen | 1.0 |
| | Var, yıl < istenen | **0.5 + 0.5 × (aday yılı / istenen yıl)** |
| | Yok | 0 |
| **Yetenek — yıl istenmemiş / tercih edilen** | Var | 1 |
| | Yok | 0 |
| **Sertifika** | Var ve **geçerli** | 1 |
| | Yok veya süresi geçmiş | 0 |
| **Bölüm** | Adayın bölümlerinden **herhangi biri** listede | 1 |
| | Hiçbiri yok | 0 |

#### Hesap

```
olmazsa_olmaz = olmazsa olmaz kümenin ortalaması
tercih_edilen = tercih edilen kümenin ortalaması

nitelik uyumu = 0.75 × olmazsa_olmaz + 0.25 × tercih_edilen
```

#### Neden böyle

| Karar | Neden |
|---|---|
| **`0.5 +` tabanı** | Yeteneğin **var olması** tek başına değerli — 1 yıl React bilen, hiç bilmeyenden çok daha iyi |
| **Yıl sadece olmazsa olmazda** | Tercih edilen "olursa iyi olur" — kaç yıl bildiğine bakmak gereksiz hassasiyet |
| **Olmazsa olmaz 3 kat ağır** | İşverenin şart koştuğu şeyler |
| **Bölüm tek öğe** | İşveren 5 bölüm yazdı diye 5 öğe olsa, bölüm yeteneklerden ağır basardı |
| **Süresi geçmiş sertifika sayılmaz** | SRC, İSG, ilk yardım gibi belgeler yenilenmezse geçersiz |

#### Tam örnek

```
İLAN
  Olmazsa olmaz:  React · 5 yıl,  .NET,  SRC belgesi
  Tercih edilen:  Docker,  İSG eğitimi,  Bölüm: [Bilgisayar Müh., Yazılım Müh.]

ADAY
  Yetenekler:   React 3 yıl, Docker 2 yıl
  Sertifikalar: SRC Belgesi (süresiz), İSG Eğitimi (süresi geçmiş)
  Bölüm:        Bilgisayar Mühendisliği

Olmazsa olmaz küme:
  React       var, 3 < 5     → 0.5 + 0.5 × 3/5 = 0.80
  .NET        yok            → 0.00
  SRC belgesi var, geçerli   → 1.00
  olmazsa_olmaz = (0.80 + 0 + 1.00) / 3 = 0.60

Tercih edilen küme:
  Docker      var            → 1
  İSG eğitimi süresi geçmiş  → 0
  Bölüm       eşleşti        → 1
  tercih_edilen = 2 / 3 = 0.67

nitelik uyumu = 0.75 × 0.60 + 0.25 × 0.67 = 0.45 + 0.17 = 0.62
```

#### Özel durumlar

| Durum | Sonuç |
|---|---|
| İlanda hiç etiket yok | 0.5 (nötr) |
| Sadece olmazsa olmaz küme | `olmazsa_olmaz` tek başına |
| Sadece tercih edilen küme | `tercih_edilen` tek başına |
| LLM ayrıştırması henüz yapılmadı | Geçici: adayın adları ilan metninde aranır (yılsız var/yok) |

#### Eşleşme nasıl yapılır

- Adayın yetenek / sertifika / bölüm adları ilanın etiketleriyle **normalize edilmiş haliyle** karşılaştırılır (küçük harf, noktalama temizliği)
- Yazım farkları (*"ReactJS"* / *"React.js"*) **metin benzerliği** (`pg_trgm`) ile yakalanır

#### Neden iki yönde aynı — eski fark neden kaldırıldı

İlk tasarımda aday yönü *"yeteneklerimin kaçı bu işte kullanılıyor"* (`ilandaki yeteneklerim / tüm yeteneklerim`) diye hesaplanıyordu. **Çok yetenek yazmayı cezalandırıyordu** — 10 yetenekli aday 2 yetenek isteyen ilanla 0.2 alıyordu. Aday bunu fark ederse yeteneklerini eksik yazar, hem etiket hem embedding zayıflardı. "Bu iş bana dar" bilgisi zaten **tecrübe** parçasından geliyor.

### 5.5 Tecrübe — %10, iki yönde FARKLI

#### Adayın tecrübesi — otomatik

Aday ayrıca yazmaz; deneyim kayıtlarından hesaplanır:

| Çalışma türü | Ağırlık |
|---|:---:|
| Tam zamanlı, freelance | × 1.0 |
| Yarı zamanlı, staj, dönemsel | × 0.5 |
| Gönüllü | Sayılmaz |

Örtüşen dönemler iki kez sayılmaz. Deneyimi olmayan aday = **0 yıl.**

#### Neden iki yön farklı

```
İlan:  Junior Developer, 1-2 yıl
Aday:  8 yıl tecrübeli

İşveren: "8 yıllık biri junior ilanıma uygun — harika"   → yüksek
Aday:    "Junior pozisyon mu? Bu geriye adım"             → düşük
```

Tek skor olsa ya işverene iyi aday gösterilmez ya adaya istemediği ilan üstte gösterilir.

#### Puan tablosu

| Aday durumu | İşveren skoru | Aday skoru |
|---|:---:|:---:|
| **Aralıkta** | 1.0 | 1.0 |
| **Aralığın üstünde** | **1.0** — fazla nitelikli, fırsat | Kademeli düşüş ↓ |
| **Aralığın altında** | aday yılı / min yıl | **0.7 + 0.3** × (aday yılı / min yıl) |
| **Hiç tecrübesiz** | 0 | 0.7 |
| **İlan aralık yazmamış** | 0.5 | 0.5 |
| **Staj ilanı** | **0.5** | **0.5** |

**Üstünde — aday skoru kademeleri:**

| Max'ı ne kadar aşıyor | Aday skoru |
|---|:---:|
| 3 yıla kadar | 0.7 |
| 3-6 yıl | 0.4 |
| 6+ yıl | 0.2 |

#### Neden böyle

| Karar | Neden |
|---|---|
| **İşveren — üstünde tam puan** | Fazla nitelikli aday işveren için fırsat; maaşta anlaşamazlarsa sohbette çözülür, sistem baştan uzaklaştırmasın |
| **Aday — altında 0.7 tabanı** | Biraz zorlayıcı ilan adayın **gelişim fırsatı.** Taban olmasaydı (1/3 = 0.33) listenin dibine düşer, aday hiç yukarı bakamazdı |
| **Aday — üstünde kademeli düşüş** | Bir kademe tolere edilir (belki sektör değiştiriyor); üç kademe açıkça yanlış eşleşme |
| **Staj — 0.5 nötr** | Stajda toplam tecrübe sorulmuyor; formül iki ilan tipinde aynı kalsın diye ağırlık dağıtılmıyor, bileşen etkisiz bırakılıyor |

> **"Ceza" değil.** Hiçbir yerde çıkarma yok — her parça 0-1 puan üretir, ağırlıkla çarpılıp toplanır. Düşük puan sadece "tam puan alamıyor" demek.

#### Örnek — ilan 1-2 yıl (junior)

| Aday | İşveren | Aday | Yorum |
|---|:---:|:---:|---|
| 0 yıl | 0.0 | 0.70 | İşveren istemiyor; aday öğrenmek istiyor |
| 1-2 yıl | 1.0 | 1.0 | İkisi de memnun |
| 4 yıl | 1.0 | 0.7 | İşveren sevinir; aday kararsız |
| 8 yıl | 1.0 | 0.2 | İşveren çok ister; aday istemez |

**Son satır:** İşverenin listesinde üstte, adayın listesinde altta. **İkisi de doğru.**

#### Örnek — ilan 3-6 yıl, aday skoru

```
Aday  4 yıl  → 1.0                        aralıkta
Aday  2 yıl  → 0.7 + 0.3 × 2/3 = 0.90     biraz altında
Aday  1 yıl  → 0.7 + 0.3 × 1/3 = 0.80     altında
Aday  8 yıl  → 0.7                        max+2
Aday 11 yıl  → 0.4                        max+5
Aday 15 yıl  → 0.2                        max+9
```

### 5.6 Kalite — %5, iki yönde TAMAMEN FARKLI

#### İşveren skoru → adayın profili ne kadar dolu

Zorunlu alanlar herkeste dolu — sadece **opsiyonel** alanlar:

| Kriter | Puan |
|---|:---:|
| En az bir deneyim kaydı | 0.30 |
| En az 5 yetenek | 0.25 |
| Proje veya sertifika var | 0.15 |
| Fotoğraf var | 0.15 |
| En az bir belge (CV vb.) | 0.15 |
| **Toplam** | **1.00** |

#### Aday skoru → ilan ve şirket ne kadar dolu

| Kriter | Puan |
|---|:---:|
| **Maaş / ücret belirtilmiş** | **0.50** |
| İmkanlar alanı dolu *(şirket profili)* | 0.20 |
| Gerçek logo *(varsayılan avatar değil)* | 0.15 |
| Web sitesi veya sosyal medya | 0.15 |
| **Toplam** | **1.00** |

#### Kaldırılan eski kriterler

| Kriter | Neden |
|---|---|
| Kısa tanıtım yazılmış, eğitim kaydı tam | Artık zorunlu — herkeste dolu |
| İş tanımının 4 sorusu | Tek alan oldu, zorunlu |
| Şirket doğrulanmış | Sistemde onaysız şirket yok |
| Şirket fotoğrafı | Fotoğraf özelliği kaldırıldı |

#### Neden sadece %5

Daha yüksek olsa, profili çok dolu ama işe uymayan aday, uygun ama eksik profilli adayın önüne geçerdi. %5 sadece **eşitliği bozar** — iki aday benzer uygunluktaysa dolu olan üste çıkar.

**Asıl işi teşvik:** Adaya *"profilini doldur"*, işverene *"maaş yaz"* demek — zorlamadan. Maaş en ağır kriter çünkü adayın en çok önemsediği bilgi.

### 5.7 Tam örnek — iki adayın sıralaması

```
İlan: Senior Backend Developer — React 5 yıl ve .NET olmazsa olmaz, 3-6 yıl

                  Aday A           Aday B
Anlamsal          0.86             0.84
Nitelik uyumu     0.85             0.33   (sadece React)
Tecrübe (işv.)    1.00 (4 yıl)     1.00 (5 yıl)
Kalite (işv.)     0.80             0.60

İşveren skoru:
A = 0.65×0.86 + 0.20×0.85 + 0.10×1.00 + 0.05×0.80 = 0.559 + 0.170 + 0.100 + 0.040 = 0.869
B = 0.65×0.84 + 0.20×0.33 + 0.10×1.00 + 0.05×0.60 = 0.546 + 0.066 + 0.100 + 0.030 = 0.742
```

İşverenin listesinde **A üstte.**

### 5.8 Uyum seviyesi — kullanıcıya gösterilen

Toplam skor kullanıcıya sayı olarak değil, **seviye** olarak gösterilir:

| Seviye | Skor *(başlangıç değeri)* |
|---|---|
| 🟢 **Yüksek uyum** | ≥ 0.75 |
| 🟡 **Orta uyum** | 0.55 – 0.75 |
| ⚪ **Düşük uyum** | < 0.55 |

> Eşikler veritabanında — gerçek skor dağılımına bakılarak ayarlanır.

### 5.9 Skor ayarları — veritabanında

**Tüm ağırlıklar ve formül sabitleri** (0.65, 0.75, 0.5 tabanı, 0.7 tabanı, 3-6 yıl kademeleri, kalite puanları, staj nötr değeri, uyum seviyesi eşikleri) veritabanında.

```sql
CREATE TABLE skor_parametreleri (
  yon        text NOT NULL,   -- 'isveren' | 'aday'
  bilesen    text NOT NULL,   -- 'anlamsal' | 'nitelik' | 'tecrube' | 'kalite'
  agirlik    real NOT NULL,
  ayarlar    jsonb,           -- formül içindeki sabitler
  guncelleme timestamptz DEFAULT now(),
  PRIMARY KEY (yon, bilesen)
);
```

| Önlem | Neden |
|---|---|
| **Kod değişmeden ayar** | Gerçek veriyle mutlaka oynanacak — her değişiklik için sürüm çıkarılmasın |
| **Toplam 1.0 kontrolü** | Ağırlıklar yanlışlıkla bozulmasın |
| **Değişiklik günlüğü** | Kim, ne zaman, neyi değiştirdi — sonuçlar bozulursa geri dönülür |
| **Değişiklik sonrası yeniden hesap** | Mevcut skorlar eski ağırlıklarla — büyük değişiklikte toplu yeniden hesap tetiklenir |

### 5.10 Alt skorlar — her eşleşmede saklanır

```json
{
  "isveren": {"anlamsal": 0.86, "nitelik": 0.85, "tecrube": 1.0, "kalite": 0.80},
  "aday":    {"anlamsal": 0.86, "nitelik": 0.85, "tecrube": 0.2, "kalite": 0.65}
}
```

| Neden | |
|---|---|
| **Ayarlama** | Hangi parça yanlış çalışıyor, görülebilsin |
| **Yasal uyum** | "Bu adayı neden üstte gösterdiniz?" sorusuna cevap — KVKK, GDPR md. 22, AB Yapay Zeka Yasası (işe alım yüksek riskli), NYC Local Law 144 |
| **"Neden önerildi?"** | Kullanıcıya açıklama bu puanlardan, LLM'siz üretilir (Bölüm 10.3) |

---

## 6. Sonuç Tablosu, Aksiyonlar, Başvuru ve Davet

### 6.1 Sonuç tablosu — tek tablo, iki yön

Skorlar önceden hesaplanıp burada saklanır. Keşfet açılınca **hesap yapılmaz, okunur** — liste anında gelir.

```sql
CREATE TABLE ilan_aday_eslestirme (
  ilan_id            bigint NOT NULL,
  aday_id            bigint NOT NULL,
  skor_isveren       real   NOT NULL,
  skor_aday          real   NOT NULL,
  alt_skorlar        jsonb,
  model_adi          text,
  hesaplanma_tarihi  timestamptz NOT NULL DEFAULT now(),
  PRIMARY KEY (ilan_id, aday_id)
) PARTITION BY HASH (ilan_id);          -- 32 bölüm, ilk günden (Bölüm 6.9)

CREATE INDEX ON ilan_aday_eslestirme (ilan_id, skor_isveren DESC);
CREATE INDEX ON ilan_aday_eslestirme (aday_id, skor_aday DESC);
CREATE INDEX ON ilan_aday_eslestirme (hesaplanma_tarihi);
```

| İlan | Aday | İşveren skoru | Aday skoru | Hesaplanma |
|---|---|:---:|:---:|---|
| Backend Dev | Ayşe | 0.87 | 0.84 | 2 saat önce |
| Backend Dev | Mehmet | 0.74 | 0.61 | 2 saat önce |

**Okuma:**
```sql
-- İşveren "Adaylar"  → tek bölüme gider
WHERE ilan_id = :ilan ORDER BY skor_isveren DESC LIMIT 200
-- Aday "Keşfet"      → 32 bölüme bakar (indeksli, okuma kopyasından, ilk sayfa önbellekte)
WHERE aday_id = :aday ORDER BY skor_aday DESC LIMIT 200
```

#### Tek tablonun en büyük faydası

```
Pazartesi:  İlan #5 yayınlandı → o anki adaylar için hesaplandı
Salı:       Yeni Aday #999 kaydoldu → onun için hesap
            → İlan #5 uygun → (5, 999) satırı yazıldı — İKİ skorla
Salı akşamı: İşveren İlan #5'i açtı → Aday #999 listede ✅
```

İşveren hiçbir şey yapmadı. Satır iki yönden okunabildiği için yeni aday onun listesine **kendiliğinden** girdi. İki ayrı tablo olsaydı (ilan bazlı / aday bazlı) yeni aday sadece kendi tablosuna yazılır, işveren görmezdi.

#### Yazma — "üstüne yaz"

```sql
INSERT INTO ilan_aday_eslestirme (...) VALUES (...)
ON CONFLICT (ilan_id, aday_id) DO UPDATE SET
  skor_isveren = EXCLUDED.skor_isveren,
  skor_aday    = EXCLUDED.skor_aday,
  alt_skorlar  = EXCLUDED.alt_skorlar,
  model_adi    = EXCLUDED.model_adi,
  hesaplanma_tarihi = now();
```

Aynı iş iki kez çalışsa da sonuç bozulmaz (Bölüm 8.5).

### 6.2 Aksiyon tablosu — ayrı, kalıcı

```sql
CREATE TABLE ilan_aday_aksiyon (
  ilan_id  bigint NOT NULL,
  aday_id  bigint NOT NULL,
  aksiyon  text   NOT NULL,
  taraf    text   NOT NULL,   -- aday | isveren | sistem
  tarih    timestamptz NOT NULL DEFAULT now(),
  PRIMARY KEY (ilan_id, aday_id, aksiyon, taraf)
) PARTITION BY HASH (ilan_id);          -- 16 bölüm, ilk günden

CREATE INDEX ON ilan_aday_aksiyon (aday_id, taraf);
CREATE INDEX ON ilan_aday_aksiyon (ilan_id, taraf);
```

| `aksiyon` | Anlamı |
|---|---|
| `gizlendi` | "Bir daha gösterme" |
| `basvuruldu` | Aday başvurdu |
| `davet_edildi` | İşveren davet etti |
| `eslesti` | İki taraf onayladı — süreçte |
| `reddedildi` | İşveren başvuruyu / aday daveti kabul etmedi |
| `suresi_doldu` | Başvuru 30 gün / davet 10 gün cevapsız |
| `ilan_kapandi` | Karar verilmeden ilan kapandı |
| `geri_cekildi` | Aday başvurusunu / işveren davetini geri çekti |

| | Sonuç tablosu | Aksiyon tablosu |
|---|---|---|
| Ne tutar | Skorlar | Yukarıdaki aksiyonlar |
| Ömrü | **Geçici** — temizlenir, yeniden hesaplanır | **Kalıcı** — hesap silinene kadar |
| Neden ayrı | Skor tablosu temizlenince "bu adayı gizlemiştim / reddetmiştim" bilgisi **kaybolmasın** | |

> Bu ayrım sonradan yapılması zor — **baştan ayrı kurulmalı.**

### 6.3 Aksiyonlar ve Keşfet'e etkisi

| Ne oldu | Aday Keşfet'i | İşveren Adaylar'ı | Nereye taşınır |
|---|:---:|:---:|---|
| Aday **"Bu ilanı bir daha gösterme"** | ❌ | ✅ | Aday → Profilim → Gizlediklerim |
| İşveren **"Bu adayı bir daha gösterme"** | ✅ | ❌ | İlan → Gizlenenler |
| Aday **başvurdu** | ❌ | ❌ | Aday → Başvurularım · İşveren → Başvuranlar |
| İşveren **davet etti** | ❌ | ❌ | İşveren → Davet Edilenler · Aday → Davetlerim |
| İki taraf **onayladı** | ❌ | ❌ | **Süreçteki İlanlar / Süreçteki Adaylar** |
| Reddedildi · süresi doldu · ilan kapandı · geri çekildi | ❌ | ❌ | İlgili listede **Sonuçlanan** |

| Kural | Neden |
|---|---|
| **"Bir daha gösterme" onay ister** | Onay diyaloğu → kart hafif animasyonla kaybolur; Gizlediklerim / Gizlenenler'den **[Geri al]** |
| **Gizleme tek yönlü** | Aday ilanı gizlediyse işveren hâlâ görüp davet edebilir — aday fikrini değiştirebilir |
| **Başvuru ve davet iki tarafın Keşfet'inden kaldırır** | Süreç başladı — kart ilgili sekmede |
| **Sonuçlanan her şey iki tarafı bağlar** | Konu kapandı; aynı ilan–aday çifti tekrar karşılaşmaz |
| **İlan bazlı** | "Backend" ilanında gizlenen aday, aynı şirketin "Frontend" ilanında görünür |
| **Taşınır, kopyalanmaz** | Kabul edilen kişi Başvuranlar'dan çıkıp Süreçteki Adaylar'a geçer |

### 6.4 Başvuru ve davet — onay ekranı

Aday başvururken veya daveti kabul ederken **profilde olmayan, başvuruya özel** bilgiler sorulur:

```
Başvuruyu onayla

Senior Backend Developer · ABC Teknoloji

Net minimum maaş beklentiniz (opsiyonel)
[______]  [TRY ▾]

Not (opsiyonel)
[_____________________________________]

Bu başvuruyla paylaşılacak belgeler:

Belgeler
☑ Özgeçmiş
☑ Portfolyo

Sertifikalar
☑ SRC Belgesi
☑ İSG Eğitimi

[Başvur]
```

| Alan | Kural |
|---|---|
| **Maaş beklentisi** | Opsiyonel · **net minimum, tek rakam** · para birimi varsayılan ilanınki · **filtreye girmez** · işveren Başvuranlar'da görür |
| **Not** | Opsiyonel — *"Esnek olabilirim"*, *"Yan haklara göre değişir"* |
| **Belgeler** | **Otomatik** — adayın profilde yüklediği belgeler (**kendi verdiği adlarla**) + sertifika dosyaları (sertifika adıyla) |
| | **Varsayılan hepsi işaretli**; aday istediğinin tikini kaldırır; hiç yoksa bölüm görünmez |

**Aynı ekrandaki kontroller:**

| Durum | Ne olur |
|---|---|
| Telefon doğrulanmamış *(numara değişmiş)* | Önce SMS → bitince başvuru **kaldığı yerden** tamamlanır |
| **Gizlediği şirkete** başvuruyor | *"Bu şirketi gizli listenize eklemişsiniz. Başvurursanız şirket profilinizi görebilecek. Devam etmek istiyor musunuz?"* |

**Davet kabulünde aynı ekran** — maaş beklentisi + belge seçimi.

> **Neden maaş profilde değil:** Maaş beklentisi pozisyona özel; profilde tek rakam adayı en alt sınırını yazmaya zorlar.
> **Neden belge seçimi:** Mevcut işvereninden çekinen aday her belgesini her şirketle paylaşmak istemeyebilir; farklı pozisyonlar için farklı özgeçmiş yükleyen aday doğrusunu seçer.

### 6.5 Başvuru / davet kaydı — tarihler ve skor anlık görüntüsü

```sql
CREATE TABLE basvurular (
  id                bigint PRIMARY KEY,
  ilan_id           bigint NOT NULL,
  aday_id           bigint NOT NULL,
  durum             text   NOT NULL,   -- bekliyor | eslesti | reddedildi |
                                       -- suresi_doldu | ilan_kapandi | geri_cekildi
  basvuru_tarihi    timestamptz NOT NULL,
  sonuc_tarihi      timestamptz,
  geri_cekme_tarihi timestamptz,
  maas_beklentisi   numeric,
  para_birimi       text,
  not_metni         text,
  paylasilan_belgeler jsonb,           -- hangi belge / sertifika dosyaları
  uyum_skoru        real,              -- BAŞVURU ANINDAKİ işveren skoru
  alt_skorlar       jsonb,             -- başvuru anındaki anlamsal, nitelik, tecrübe, kalite
  eksik_nitelikler  jsonb              -- başvuru anında eksik olan olmazsa olmazlar
);
-- davetler: aynı yapı (davet_tarihi, cevap_tarihi, geri_cekme_tarihi, davet_eden_id ...)
```

**Neden skor anlık görüntüsü:** Skorlar sonuç tablosunda **geçici** — ilan kapanınca veya 14 günde siliniyor. Ama başvuru kartı 90 gün görünüyor ve "Neden önerildi?" bölümü var. Başvuru / davet **anındaki** skor kayda kopyalanır:

| Fayda | |
|---|---|
| Kart her zaman doğru | Sonuç tablosu temizlense bile |
| Karar anındaki uyum | Aday sonradan profilini değiştirse bile işveren başvurduğu andaki uyumu görür |
| Yasal uyum | "Bu başvuru neden üstteydi?" sorusunun cevabı kalıcı |
| Sıralama | **Başvuranlar** listesi bu skora göre sıralı — işveren en uyumludan başlar |

### 6.6 Bekleyen / Sonuçlanan

İki tarafta, her listede iki alt sekme:

```
┌──────────────┬──────────────┐
│  Bekleyen 4  │ Sonuçlanan 7 │
└──────────────┴──────────────┘
```

**Her kart o anki durumu gösterir:**

| Durum | Aday — Başvurularım | İşveren — Başvuranlar |
|---|---|---|
| Bekliyor | ⏳ Değerlendiriliyor | ⏳ Karar bekliyor · *25 gün kaldı* |
| Bekliyor + ilan duraklatıldı | ⏳ Değerlendiriliyor · İlan şu an duraklatıldı | ⏸ İlan duraklatıldı |
| Kabul edildi | → Süreçteki İlanlar | → Süreçteki Adaylar |
| Reddedildi | ✕ **Başka adaylarla ilerlendi** | ✕ Reddedildi |
| Süresi doldu | ⌛ Yanıtlanmadan sona erdi | ⌛ Yanıtlanmadan sona erdi |
| İlan kapandı | 🔒 İlan kapandı | 🔒 İlan kapandı |
| Geri çekildi | ↩ Başvurunu geri çektin | ↩ Aday başvurusunu geri çekti |

| Durum | Aday — Davetlerim | İşveren — Davet Edilenler |
|---|---|---|
| Bekliyor | ⏳ Cevabın bekleniyor | ⏳ Cevap bekleniyor · *7 gün kaldı* |
| Kabul etti | → Süreçteki İlanlar | → Süreçteki Adaylar |
| Kabul etmedi | ✕ Daveti kabul etmedin | ✕ Kabul etmedi |
| Süresi doldu | ⌛ Yanıtlanmadan sona erdi | ⌛ Yanıtlanmadan sona erdi |
| İlan kapandı | 🔒 İlan kapandı | 🔒 İlan kapandı |
| Geri çekildi | ↩ Davet geri çekildi | ↩ Davet geri çekildi |

> **"Başka adaylarla ilerlendi" = reddedildi.** Aday tarafında kibar ifade — çoğu zaman gerçek sebep de bu.
> **Adaya kalan gün gösterilmez** — baskı yaratır; işverene gösterilir, süresi dolmak üzere olanı görsün.

**"İlan kapandı"nın altına girenler** — aday için hepsi aynı sonuç:

| Arka planda | Adaya görünen |
|---|---|
| Pozisyon doldu · alımdan vazgeçildi · 60 / 180 gün doldu · şirket hesabı kapatıldı | 🔒 İlan kapandı |

> "Pozisyon doldu" ayrıca söylenmez — adaya "başkası alındı" demek gereksiz yere kırıcı.

**Sonuçlandırmayan durumlar:** İlan duraklatıldı *(başvuru bekler, 30 gün işlemeye devam eder)*, ilan başka uzmana devredildi *(aday görmez)*, aday "şu an bakmıyorum" yaptı *(başvurusu devam eder)*.

**İlan tekrar yayınlanırsa:** İşverene *"12 bekleyen başvurunuz vardı. Geri getirilsin mi?"* — **Evet** → başvurular Bekliyor'a döner, adaya 🔔, 30 gün yeniden başlar. **Hayır** → İlan kapandı olarak kalır.

### 6.7 Cevapsız kalma, geri çekme, belge erişimi

#### Cevapsız kalma

| | Süre | Süre dolunca |
|---|:---:|---|
| **Başvuru** — işverenin cevabı | **30 gün** | Süresi doldu — iki tarafın Keşfet'ine bir daha düşmez |
| **Davet** — adayın cevabı | **10 gün** | Süresi doldu — iki tarafın Keşfet'ine bir daha düşmez |

> **Neden geri dönmüyor:** Cevap vermeyen tarafa tekrar tekrar davet / başvuru gitmesi spam'e dönüşür.

#### Geri çekme — iki tarafta, kartta

| | Aday başvuruyu geri çeker | İşveren daveti geri çeker |
|---|---|---|
| Nerede | Başvurularım → Bekleyen kartı **[Başvurumu geri çek]** | Davet Edilenler → Bekleyen kartı **[Daveti geri çek]** |
| Onay | *"Başvurunuzu geri çekmek istediğinize emin misiniz?"* | *"Daveti geri çekmek istediğinize emin misiniz?"* |
| Karşı tarafta | Başvuranlar → Sonuçlanan: *Aday başvurusunu geri çekti* | Davetlerim → Sonuçlanan: *Davet geri çekildi* |
| Bildirim | İlanın sorumlusuna 🔔 | Adaya 🔔 |
| Tekrar | Bu ilana **tekrar başvuramaz** | Bu ilandan bu adaya **tekrar davet gönderilemez** |

> **Neden tekrar yok:** "Başvur – geri çek – tekrar başvur" ile listenin başına çıkma, ya da aynı adayı tekrar tekrar davetle rahatsız etme olmasın.

#### Belge erişimi — sonuçlanınca kapanır

Başvuru **sonuçlandığı anda** (ret, süre dolması, ilan kapanması, geri çekme) işverenin o başvuruda paylaşılan belgelere erişimi **kapanır.** İşveren kaydı görür ama CV'yi açamaz. **Süreçteki adaylarda** erişim devam eder.

#### Listede kalma ve saklama

| | Süre |
|---|---|
| **Sonuçlanan** sekmesinde görünme | **Son 90 gün** — aday / işveren isterse daha erken **[Listeden kaldır]** |
| **Aksiyon kaydı** (kısa satır) | **Kalıcı** — reddedilen aday aynı ilana tekrar düşmesin |
| **Başvuru ayrıntıları** (maaş, not, paylaşılan belgeler) | Sonuçlandıktan **1 yıl** sonra silinir (KVKK / GDPR) |

> Temizlik işleri → `bakim-isleri.md`

### 6.8 Silme kuralları — sonuç tablosu

| Olay | Sonuç satırları | Aksiyonlar | Sohbetler |
|---|:---:|:---:|:---:|
| İlan kapandı / doldu / süresi doldu | Silinir | Kalır | **Devam eder** |
| Aday "şu an bakmıyorum" | Silinir | Kalır | **Devam eder** |
| 14 günden eski hesap | Silinir | Kalır | — |
| Aday telefonu değiştirdi, doğrulamadı | **Silinmez** — profil yayından çıktığı için görünmez | Kalır | Devam eder |
| Aday hesabını sildi | Silinir | Silinir (KVKK/GDPR) | Anonimleşir — işverende *"Aday hesabını kapattı"* |

### 6.9 Bölümleme — ilk günden

Bölümlemenin en kolay olduğu an tablo **boşken.** Dolu tabloyu sonradan bölmek riskli ve yavaş — bu yüzden **ilk günden** kuruluyor.

| Tablo | Anahtar | Bölüm | Neden |
|---|---|:---:|---|
| **`ilan_aday_eslestirme`** (sonuç) | **Hash — `ilan_id`** | **32** | En büyük, en sık yazılan tablo |
| **`ilan_aday_aksiyon`** | **Hash — `ilan_id`** | **16** | Sonuçtan küçük ama kalıcı büyüyor |
| **`mesajlar`** | **Hash — `sohbet_id`** | **16** | Okumalar hep tek sohbete |
| `ilan_goruntulemeleri` | Zaman | Aylık | Sayaca toplandıktan sonra eski aylar tek komutla silinir |
| `bildirimler` | Zaman | Aylık | Eski bildirimler ay ay silinir |
| Kuyruk / iş kayıtları | Zaman | Haftalık | Hızlı dolar, hızlı temizlenir |

> Zaman bölümleri **`pg_partman`** ile otomatik — yeni ay açılır, eski silinir; elle bakım yok.

#### Neden `ilan_id`

| | |
|---|---|
| **Hiç değişmez** | Satır bölümler arasında taşınmaz |
| İşveren sorgusu (`ilan_id = ?`) | **Tek bölüme** gider |
| İlan kapanınca silme | Tek bölümde |

| Reddedilen anahtar | Neden |
|---|---|
| Ülke | **Uzaktan ilanlar ülke sınırını aşıyor** — satır hangi bölüme gidecek belirsiz |
| Durum | Durum değişince satır fiziksel taşınır |
| Tarih | Tüm yazma son bölüme yığılır |

#### Bölüm sayısı

```
5 milyon aktif aday × ~200 eşleşme ≈ 1 milyar satır
32 bölüm → bölüm başına ~31 milyon satır   (rahat; zorlanma ~100 milyonda başlar)
```

- **Sayı sabit** — `hash(ilan_id) % 32`; yeni ilan yeni bölüm yaratmaz
- **Sonradan değiştirmek** = tüm veriyi taşımak; bu yüzden baştan cömert
- **Aday sorguları** tüm bölümlere bakar — her bölümde `aday_id` indeksi, okuma kopyası ve önbellek yükü alıyor. Çok yüksek ölçekte gerekirse aday yönü için ikinci bir tablo eklenir, mevcut yapı bozulmadan

## 7. Hesaplama — Ne Zaman, Nasıl

### 7.1 Karma hesaplama

> **Sorun:** İş bulmuş, aylarca girmeyecek kullanıcılar için her yeni ilanda hesap yapmak saf israf.
> **Ama tamamen "açınca hesapla" da olmaz:** Aktif kullanıcıya anında liste ve "yeni ilan geldi" bildirimi için önceden hesap gerekiyor.

```
Son 14 günde giriş yapmış  → ÖNCEDEN hesaplanır
                             Keşfet açılınca liste anında hazır

14 günden uzun girmemiş    → HİÇ hesaplanmaz
                             Keşfet'i açarsa O AN hesaplanır (~200 ms, "hazırlanıyor")

"Şu an bakmıyorum"         → Hiç — profil kimseye görünmüyor
```

| | Hesap |
|---|---|
| Aktif aday / işveren | Önceden |
| 14-90 gün girmemiş | Sadece açarsa |
| 90+ gün → otomatik "şu an bakmıyorum" | Hiç |

```
Hepsini önceden:   500.000 aday × her yeni ilan
Karma:             ~75.000 aktif aday × her yeni ilan  → ~%85 daha az hesap
```

**Aynı kural işverende:** 14 gündür girmeyen işverenin ilanları için önceden hesap yapılmaz; ilanı açınca hesaplanır.

**Önbellek tazeliği:** Liste 24 saatten eskiyse Keşfet açılınca yeniden hesaplanır.

### 7.2 Durum kolonları

```sql
ilanlar.eslestirme_durumu, ilanlar.son_eslestirme_tarihi
aday_profilleri.eslestirme_durumu, aday_profilleri.son_eslestirme_tarihi
-- beklemede | hesaplaniyor | hazir | hata
```

| Durum | Ekran |
|---|---|
| `hazir` + 24 saatten taze | Tablodan oku |
| `hesaplaniyor` | *"Hazırlanıyor..."* — bitince dolar |
| Eski veya yok | Hesapla, sonra göster |
| `hata` | Tekrar denenir |

> **Neden zorunlu:** Olmazsa "sonuç yok" ile "henüz hesaplanmadı" ayırt edilemez — kullanıcı boş ekran görür, nedenini kimse bilmez.

### 7.3 Aday profili yayınlanınca

```
1. ANINDA (kullanıcı bekler — milisaniyeler)
   ├─ Profil alanları kaydedilir
   ├─ Kontrol: zorunlu adımlar tamam mı? telefon doğrulanmış mı?
   ├─ Profil → "yayında"
   ├─ 7 metnin hash'i değişti mi?
   └─ Değiştiyse: kuyruğa "aday #42 → embedding"   (aynı işlemde)
        ↓
   Kullanıcı: "Profilin yayında. Sana uygun ilanlar hazırlanıyor..."

2. EMBEDDING İŞÇİSİ (1-2 sn)
   ├─ 7 metni birleştirir → embedding API → embeddings tablosu
   └─ Kuyruğa: "aday #42 → eşleştir"

3. EŞLEŞTİRME İŞÇİSİ (milisaniyeler)
   ├─ Katı filtre → vektör (200) → skor (iki yön)
   ├─ ilan_aday_eslestirme tablosuna yazar
   ├─ Profil eşleştirme durumu → "hazır"
   └─ İlk 50'ye giren yeni ilanlar → bildirim notu
```

### 7.4 İlan kaydedilip yayınlanınca

```
1. İşveren [Kaydet]
   ├─ İlan alanları kaydedilir (taslak)
   └─ 5 kutunun hash'i değişti mi?
        Evet → LLM ayrıştırır (1-3 sn) → etiketler kaydedilir
               "Algılanan" satırı görünür — onay yok
        Hata → ilan yine kaydedilir, kuyruğa "ilan #5 → ayrıştır"

2. İşveren [Yayınla]
   ├─ Kontrol: şirket tanıtımı dolu mu? yayınlayanın telefonu doğrulanmış mı?
   ├─ İlan → "yayında"
   └─ Kuyruğa: "ilan #5 → embedding"
        ↓
   İşveren: "İlanınız yayında. Uygun adaylar hazırlanıyor..."

3. EMBEDDING İŞÇİSİ → EŞLEŞTİRME İŞÇİSİ
   İlan #5 × aktif uygun adaylar
```

**Eşleştirme işçisi `varlik_tipi`ne göre iki yönde çalışır:**

| Kuyruktaki iş | Hesaplanan |
|---|---|
| `aday, 42` | Aday 42 × uygun ilanlar |
| `ilan, 5` | İlan 5 × uygun **aktif** adaylar |

İki durumda da satırlar **aynı tabloya, iki skorla** yazılır.

### 7.5 Ne değişince ne tetiklenir

| Ne değişti | Örnek | Embedding | Eşleştirme |
|---|---|:---:|:---:|
| **Metin** | Tanıtım, deneyim, iş tanımı, yetenek / bölüm / sertifika kutuları | ✅ | ✅ *(embedding bitince)* |
| **Sadece katı filtre alanı** | Konum, çalışma şekli/tipi, iş dili, eğitim şartı, diğer şartlar | ❌ Atlanır | ✅ Doğrudan |
| **Sadece skor alanı** | Tecrübe aralığı, maaş | ❌ Atlanır | ✅ Doğrudan |
| **Sadece gösterim alanı** | Başlama zamanı, serbest şartlar, fotoğraf | ❌ | ❌ |

> İlan kutuları değişince ayrıca **LLM yeniden ayrıştırır** — işverenin `isveren_duzeltti` etiketleri korunur.

### 7.6 Yayındaki ilan düzenlenirse

| Alan | Taslakken | Yayından sonra |
|---|:---:|:---:|
| **Meslek alanı** | ✅ | 🔒 **Değişmez** |
| **Unvan** | ✅ | 🔒 **Değişmez** |
| Diğer alanlar | ✅ | ✅ — Bölüm 7.5'e göre tetikler |

> *"Meslek alanı ve unvan yayından sonra değiştirilemez. Farklı bir pozisyon için yeni ilan açın."*

**Neden kilitli:** İkisi pozisyonun kimliği. Muhasebe ilanını yazılım ilanına, "Backend Developer"ı "Satış Temsilcisi"ne çevirmek; eski başvuranların, gizlenenlerin, davetlerin geçmişini alakasız bir pozisyona taşır. Yan faydası: embedding'in ilk satırı sabit kalır.

**Önceden olanlar korunur:** Yeni şartlara artık uymayan bir aday zaten başvurmuşsa başvurusu geçerli kalır — sadece Keşfet'teki yeni adaylar değişir.

### 7.7 Aday telefonu değişirse

```
Numara değişti → doğrulama kalkar → profil yayından çıkar
        ↓
Hemen doğrularsa                    Doğrulamazsa
→ profil yayına döner               → işverenlerin listesinde görünmez (Filtre 1)
→ skorlar hazır, anında geri gelir  → skorlar SİLİNMEZ, bekler
                                    → 14 gün sonra normal temizlikle silinir
```

Her durumda başvurular, davetler, süreçteki ilanlar ve sohbetler **devam eder.**

---

## 8. Kuyruk ve İşçiler

### 8.1 Kuyruk nedir

**Bankadaki sıra numarası.** 100 kişi aynı anda gelse gişe hepsine aynı anda bakamaz — herkes numara alır, sırayla çağrılır. Kimse kapıda bekletilmez, kimse unutulmaz, görevli boğulmaz.

```
Bir şey olur          →   Kuyruğa iş yazılır       →   İşçi sırayla alır, yapar
(ilan yayınlandı)         ("ilan #5'i eşleştir")       (hesaplar, sonucu yazar)
```

| Fayda | |
|---|---|
| **Kullanıcı beklemez** | Ağır iş arka planda; ekran anında cevap verir |
| **Ani yük emilir** | 10.000 iş aynı anda gelse sıraya girer — sistem çökmez, biraz gecikir |
| **Hiçbir iş kaybolmaz** | İşçi çökerse veya dış servis cevap vermezse iş kuyrukta kalır, tekrar denenir |
| **Kolay büyür** | Yük artınca aynı kuyruktan çeken işçi sayısı artar |

### 8.2 Supabase ile

| Parça | İş |
|---|---|
| **Supabase Queues** (`pgmq`) | Kuyruğun kendisi — veritabanı içinde |
| **Veritabanı tetikleyicisi** | Olay olunca kuyruğa iş yazar |
| **Edge Function** | İşçi — kuyruktan alır, yapar |
| **`pg_cron` + `pg_net`** | İşçileri kısa aralıklarla uyandırır |

### 8.3 İşçiler — her biri ayrı Edge Function

| # | İşçi | Kuyruk | Tetikleyen | Ne yapar |
|:---:|---|---|---|---|
| 1 | **Embedding** | `embedding` | Metin değişti | Metni vektöre çevirir → `embeddings` |
| 2 | **Eşleştirme** | `eslestirme` | Embedding bitti · filtre/skor alanı değişti · [Daha fazla getir] · pasif kullanıcı Keşfet'i açtı | Filtre + vektör + skor → `ilan_aday_eslestirme` |
| 3 | **Ayrıştırma** | `ayristirma` | LLM ilan kaydında hata verdi | LLM'i tekrar dener, etiketleri sessizce uygular |
| 4 | **Bildirim** | `bildirim` | Olay oldu · günlük özet zamanı | Push (FCM) + e-posta |
| 5 | **Raporlama ve denetim** | `pg_cron` | Zaman | Adalet denetimi, şirket adı eşleme, LLM kalite izleme, admin metrikleri |

> **Bakım işleri** (süreler, temizlik, otomatik düşme, sayaçlar, silmeler) → `bakim-isleri.md`

**Neden her biri ayrı — tek fonksiyonda değil:**

| Tek fonksiyonda olsaydı | Ayrı oldukları için |
|---|---|
| Biri takılınca hepsi takılır | Embedding servisi yavaşladı → bildirimler etkilenmez |
| Ayrı büyütülemez | Sadece yükü artan işçi çoğaltılır |
| Süre sınırı üç işe bölünür | Her işçi kendi süresini kullanır |

**"İşçi" değil "işçi türü":** Her türden birden fazla kopya aynı kuyruktan iş çekebilir — kuyruk aynı işin iki işçiye gitmesini engeller.

### 8.4 İşçi değil, anında yapılanlar

| İş | Nasıl |
|---|---|
| LLM ayrıştırma (normal durum) | İlan kaydında, Edge Function |
| SMS doğrulama | Twilio Verify, anında |
| Sohbet | Supabase Realtime |
| Dosya kontrolü + virüs taraması | Yüklemede Edge Function → ClamAV *(Altyapı dokümanı)* |
| Ekip üyesi oluşturma | Edge Function, anında |
| Toplu yeniden embedding (model değişimi) | Embedding kuyruğuna **düşük öncelikli** iş |

### 8.5 Yüksek trafik için altı kural

| # | Kural | Ne demek | Neden |
|:---:|---|---|---|
| 1 | **Veri ve iş aynı anda yazılır** | Profil kaydı ile "embedding üret" işi aynı veritabanı işleminde | "Profil yayında ama hiç eşleşmedi" durumu oluşamaz |
| 2 | **Aynı iş bir kez yapılır** | İşin kimliği = *varlık + metin hash'i* | Aday 3 kez art arda kaydederse sadece son hali işlenir |
| 3 | **Toplu embedding** | İşçi 50-100 metni tek API çağrısıyla gönderir | Çağrı sayısı ve maliyet düşer, sağlayıcı sınırına takılma azalır |
| 4 | **Tekrar çalışsa sorun çıkmaz** | Sonuç "üstüne yaz" mantığıyla | İşçi bitirirken çökerse iş tekrar işlenir, sonuç bozulmaz |
| 5 | **Öncelik sırası** | Yeni yayın > güncelleme > toplu yenileme | Yoğun anda yeni kullanıcı beklemez |
| 6 | **Küçük partiler** | Her çağrıda sınırlı iş (örn. 20 eşleştirme); büyük işler 500'lük parçalara bölünür | Edge Function süre sınırına takılmaz |

### 8.6 Kod yapısı — ağır iş SQL'de, işçi ince

| Parça | Nerede | Dil |
|---|---|---|
| Kuyruğa iş yazma | Veritabanı tetikleyicisi | SQL |
| Filtre + vektör + skor | Veritabanı fonksiyonları (`eslestir`) | SQL |
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

| Neden ağır iş veritabanında | |
|---|---|
| **Veri orada** | Binlerce satır okunuyor — dışarı taşımak yavaş |
| **Tek yer** | Kural değişince tek SQL fonksiyonu değişir; Flutter, Next.js, işçiler etkilenmez |

### 8.7 Taşınabilirlik — gerekirse container'a

Edge Function'lar Deno ile yazılıyor; **aynı kod değiştirilmeden bir Deno container'ında da çalışır.** Bunu garanti eden üç kural:

| Kural | Neden |
|---|---|
| **İnce** — kuyruktan al, SQL fonksiyonunu çağır, sil | Taşınacak çok kod olmasın |
| **Durumsuz** — işçi kendi içinde hiçbir şey saklamaz | Kaç kopya çalışırsa çalışsın sorun çıkmaz |
| **Standart bağlantı** — Supabase'e özel çalışma ortamı özelliği kullanmaz | Container'dan da aynı şekilde bağlanır |

**Taşıma sinyalleri** — ölçümle izlenir:

| Sinyal | Karar |
|---|---|
| Kuyruk sürekli birikiyor | O işçi container'a taşınır (Fly.io / Cloud Run), birden çok kopya |
| İş süreleri sınıra yaklaşıyor | Aynı |
| Çağrı maliyeti sürekli açık işçiyi geçiyor | Aynı |

**Bir işçiyi taşımak diğerlerini etkilemez** — örneğin sadece eşleştirme işçisi container'a geçer; kuyruk, işlerin yapısı ve SQL fonksiyonları aynı kalır.

> Supabase'in Edge Function süre ve eşzamanlılık sınırları plana göre değişir — kurulumdan önce güncel değerler kontrol edilmeli.

## 9. Bildirim

### 9.1 Firebase — sadece postacı

| İş | Servis |
|---|---|
| Giriş, veritabanı, dosyalar, kuyruk, işçiler | **Supabase** |
| Push bildirimi | **Sadece Firebase Cloud Messaging (FCM)** |
| E-posta | E-posta servisi (Resend, Postmark, SES vb.) |

FCM; Android, iOS (Apple'ın APNs'i üzerinden) ve web'i (Next.js) tek servisle kapsıyor, gönderim başına ücret yok. Firebase'de kullanıcı hesabı **yok** — Firebase kullanıcıyı değil, **cihazı** tanır.

### 9.2 Kullanıcı ↔ cihaz eşleşmesi

```
1. Kullanıcı Supabase ile giriş yapar
2. Uygulama FCM'den cihaz adresini (token) alır
3. Adres Supabase'e kaydedilir → "Ayşe'nin telefonu = abc123"
```

```sql
CREATE TABLE cihaz_tokenlari (
  kullanici_id uuid NOT NULL,
  token        text NOT NULL,
  platform     text NOT NULL,   -- android | ios | web
  dil          text,
  son_gorulme  timestamptz,
  PRIMARY KEY (kullanici_id, token)
);
```

| An | Ne yapılır |
|---|---|
| Giriş | Adres kaydedilir / güncellenir |
| Adres yenilenir | Uygulama dinler, kaydı günceller |
| **Çıkış** | Adres **silinir** — telefonu başkası kullanırsa eski kullanıcının bildirimleri gelmesin |
| Uygulama silindi | FCM hata döner → kayıt silinir |

Kullanıcı sadece kendi adresini kaydedebilir (RLS). FCM anahtarı sadece Edge Function'da.

### 9.3 Akış

```
Olay (başvuru geldi)
  → tetikleyici iki şey yazar:
     ├─ bildirimler tablosu  ← uygulama içi 🔔 (ilgili herkes)
     └─ bildirim kuyruğu     ← push işi (kurala uyanlar)
  → bildirim işçisi: kime? cihazları? dili? → FCM → telefon
```

### 9.4 Üç kanal

> **📱 Push** — telefona / tarayıcıya, uygulama kapalıyken · **🔔 Uygulama içi** — bildirim kutusunda saklanır, sayaçta görünür · **✉️ E-posta**

| | 📱 Push | 🔔 Uygulama içi |
|---|---|---|
| Amaç | **Hemen haberdar et** | **Kaybolmasın, sonra bakılsın** |
| Saklanır mı | Hayır — gönderilir, biter | Evet — okundu / okunmadı |
| Kim alır | Sadece **harekete geçmesi gereken** kişi | **İlgili herkes** |

> **İşverende push sadece ilanın sorumlusuna:** Büyük şirkette yönetici günde yüzlerce bildirim alır, kapatır, önemliyi kaçırır. Sayaç ile haberdar, ama rahatsız edilmiyor.

### 9.5 Bildirim kataloğu

#### Başvuru

| # | Olay | Kime | Kanal | Metin | Dokununca |
|:---:|---|---|:---:|---|---|
| 1 | Aday başvurdu | İlanın sorumlusu | 📱 🔔 | *"Senior Backend Developer ilanınıza yeni başvuru: Ayşe Kaya"* | İlan → Başvuranlar |
| 2 | Aday başvurdu | Sahip / yönetici | 🔔 | Aynı | İlan → Başvuranlar |
| 3 | Başvuru **kabul edildi** | Aday | 📱 🔔 | *"ABC Teknoloji başvurunuzu kabul etti. Görüşmeye başlayabilirsiniz."* | Süreçteki İlanlar → sohbet |
| 4 | Başvuru **kabul edilmedi** | Aday | 📱 🔔 | *"ABC Teknoloji, Senior Backend Developer pozisyonu için başka adaylarla ilerlemeye karar verdi."* | Başvurularım → Sonuçlanan |
| 5 | Başvurunun **süresi doldu** | Aday | 🔔 | *"Senior Backend Developer başvurunuz yanıtlanmadan sona erdi."* | Başvurularım → Sonuçlanan |
| 6 | Aday başvurusunu **geri çekti** | İlanın sorumlusu | 🔔 | *"Ayşe Kaya başvurusunu geri çekti."* | İlan → Başvuranlar → Sonuçlanan |

#### Davet

| # | Olay | Kime | Kanal | Metin | Dokununca |
|:---:|---|---|:---:|---|---|
| 7 | İşveren davet etti | Aday | 📱 🔔 | *"ABC Teknoloji sizi Senior Backend Developer pozisyonuna davet etti."* | Davetlerim |
| 8 | Davet **kabul edildi** | İlanın sorumlusu | 📱 🔔 | *"Ayşe Kaya davetinizi kabul etti. Görüşmeye başlayabilirsiniz."* | Süreçteki Adaylar → sohbet |
| 9 | Davet kabul edildi | Sahip / yönetici | 🔔 | Aynı | Süreçteki Adaylar |
| 10 | Davet **kabul edilmedi** | İlanın sorumlusu | 📱 🔔 | *"Ayşe Kaya davetinizi kabul etmedi."* | Davet Edilenler → Sonuçlanan |
| 11 | Davet kabul edilmedi | Sahip / yönetici | 🔔 | Aynı | |
| 12 | Davetin **süresi doldu** | İlanın sorumlusu | 🔔 | *"Ayşe Kaya'ya gönderdiğiniz davet yanıtlanmadan sona erdi."* | Davet Edilenler → Sonuçlanan |
| 13 | İşveren daveti **geri çekti** | Aday | 🔔 | *"ABC Teknoloji davetini geri çekti."* | Davetlerim → Sonuçlanan |

#### Süreç ve sohbet

| # | Olay | Kime | Kanal | Metin | Dokununca |
|:---:|---|---|:---:|---|---|
| 14 | Yeni mesaj | Aday veya ilanın sorumlusu | 📱 🔔 | *"ABC Teknoloji: Merhaba Ayşe Hanım, …"* (ilk satır) | Sohbet |
| 15 | Yeni mesaj | Sahip / yönetici | 🔔 | Aynı | Sohbet |
| 16 | **İletişim bilgisi istendi** | Aday | 📱 🔔 | *"ABC Teknoloji telefon ve e-posta bilginizi görmek istiyor."* | Sohbet → [Paylaş] / [Şimdilik hayır] |
| 17 | İletişim bilgisi **paylaşıldı** | İlanın sorumlusu | 📱 🔔 | *"Ayşe Kaya iletişim bilgilerini paylaştı."* | Sohbet |
| 18 | İlan **devredildi** | Aday | 🔔 | *"Bu görüşme artık Can Demir tarafından yürütülüyor."* *(sohbette sistem mesajı olarak da)* | Sohbet |
| 19 | İlan **devredildi** | Yeni sorumlu | 📱 🔔 | *"Senior Backend Developer ilanı size devredildi."* | İlan |

#### İlan durumu

| # | Olay | Kime | Kanal | Metin | Dokununca |
|:---:|---|---|:---:|---|---|
| 20 | İlan **kapandı** | Bekleyen başvurusu olan adaylar | 🔔 | *"Başvurduğunuz Senior Backend Developer ilanı kapandı."* | Başvurularım → Sonuçlanan |
| 21 | İlan kapandı | Daveti bekleyen adaylar | 🔔 | *"ABC Teknoloji'nin davet ettiği pozisyon kapandı."* | Davetlerim → Sonuçlanan |
| 22 | İlan **yeniden açıldı**, başvurular geri getirildi | İlgili adaylar | 🔔 | *"Başvurduğunuz Senior Backend Developer ilanı yeniden açıldı; başvurunuz değerlendirmeye alındı."* | Başvurularım → Bekleyen |

#### Günlük özet

| # | Olay | Kime | Kanal | Metin | Dokununca |
|:---:|---|---|:---:|---|---|
| 23 | İlk 50'ye giren yeni ilanlar | Aday *("haber ver" açık, aktif)* | 📱 | *"Sana uygun 4 yeni ilan var."* | Keşfet |
| 24 | İlk 50'ye giren yeni adaylar | İlanın sorumlusu | 📱 | *"Senior Backend Developer ilanına uygun 6 yeni aday var."* | İlan → Adaylar |

> **Günlük özet neden sadece 📱:** Bir hatırlatma; kullanıcı uygulamayı açınca yeni ilanları / adayları zaten listede görüyor.

### 9.6 Kurallar

| Kural | |
|---|---|
| **Dil** | Alıcının arayüz dilinde — tüm metinler şablon, her dile çevrilir |
| **Saat** | Anlık bildirimler hemen; günlük özet alıcının saat diliminde sabah |
| **Mesaj gruplama** | Aynı sohbetten kısa sürede çok mesaj → tek push: *"ABC Teknoloji'den 3 yeni mesaj"* |
| **Sessize alma** | Push **türlere göre** kapatılabilir (mesaj, başvuru, günlük özet); 🔔 her zaman kalır |
| **Ret metinleri** | Kibar ve nötr — aday için kırıcı olmasın |
| **İzin verilmezse** | Push gitmez; 🔔 sayaç yine çalışır |

> Bakım kaynaklı bildirimler (otomatik düşme uyarısı, hesap askıya alma) → `bakim-isleri.md` · Hesap / şirket e-postaları (işveren onayı, ekip daveti, değişiklik talebi) → ilgili dokümanlar

### 9.7 Yeni ilan / aday — günlük özet

| | Aday | İşveren |
|---|---|---|
| "Uygun" ne demek | Yeni ilan adayın listesinde **ilk 50'ye** girdi | Yeni aday ilanın listesinde **ilk 50'ye** girdi |
| Kime | "Haber ver" açık + **aktif** aday | İlanın **sorumlusu** |
| Sıklık | Günde en fazla bir, sabah | Aynı |
| Varsayılan | Açık, kapatılabilir | Açık, kapatılabilir |

**Neden ilk 50:** Filtreden geçen her yeni ilan sayılsa geniş profilli adaya *"87 yeni ilan"* gider — anlamsız. **Pasif kullanıcılar** önceden hesaplanmadığı için özet almaz — 14 gündür girmeyene her gün bildirim uygulamayı silmeye iter.

### 9.8 Anlık güncelleme — sadece sohbette

| Ekran | Yöntem | Neden |
|---|---|---|
| **Açık sohbet** | **Supabase Realtime** | Mesaj saniyesinde gelmeli |
| Sayaçlar, bildirim kutusu, ilan kartı sayıları | **60 sn'de bir yenileme** + aşağı çekip yenile; arka plandayken durur; tek istekte tüm sayılar | Bir dakika gecikme sorun değil |
| Keşfet | Açılınca yükle | Anlık değişim gerekmiyor |

> Realtime bağlantı sayısı "o an ekranı açık olan" kadar — sadece sohbette açık tutmak bağlantıları onda birine indiriyor.

## 10. Keşfet, Kartlar ve Menüler

### 10.1 Menüler

**Aday — alt menü (en fazla 5):**

```
┌──────────────────────────────────────────────────────┐
│  Üst bar:   🔔 3      💬 2           🟢 Aktif arıyorum │
├──────────────────────────────────────────────────────┤
│                    (ekran içeriği)                   │
├──────────────────────────────────────────────────────┤
│  🔍        📨           ✉️         🤝         👤      │
│ Keşfet  Başvurularım  Davetlerim  Süreçte   Profilim │
└──────────────────────────────────────────────────────┘
```

| Sekme | İçerik | Rozet |
|---|---|---|
| 🔍 **Keşfet** | Uygun ilanlar | — |
| 📨 **Başvurularım** | Bekleyen / Sonuçlanan | Görülmemiş yeni sonuç |
| ✉️ **Davetlerim** | Bekleyen / Sonuçlanan | Cevap bekleyen davet |
| 🤝 **Süreçteki İlanlar** | Eşleşilen ilanlar — sohbet | Okunmamış mesaj |
| 👤 **Profilim** | Profil, **Gizlediklerim**, ayarlar | — |

**İşveren — her ilanın içinde:**
```
[ Adaylar ] [ Başvuranlar ] [ Davet Edilenler ] [ Süreçteki Adaylar ] [ Gizlenenler ]
```

**İki tarafın simetrisi:**

| Aday | İşveren (ilan bazlı) |
|---|---|
| Keşfet | Adaylar |
| Başvurularım (Bekleyen / Sonuçlanan) | Başvuranlar (Bekleyen / Sonuçlanan) |
| Davetlerim (Bekleyen / Sonuçlanan) | Davet Edilenler (Bekleyen / Sonuçlanan) |
| Süreçteki İlanlar | Süreçteki Adaylar |
| Profilim → Gizlediklerim | Gizlenenler |

> **Neden ayrı "Reddedilenler" sekmesi yok:** Alt menüde yer sınırlı; reddedilen + süresi dolan + kapanan aynı şey — **sonuçlanmış.** Bekleyenler önde, sonuçlananlar bir dokunuş uzakta; aday her açtığında ret listesiyle karşılaşmaz.

### 10.2 Kart standardı — tüm kartlar için tek kural

> **Kartta:** ayıklama + incele · **Detayda:** süreci başlatan aksiyon + ayıklama

| | Kartta | Detayda |
|---|:---:|:---:|
| **[Bir daha gösterme]** | ✅ | ✅ |
| **[İncele →]** | ✅ | — |
| **[Başvur]** *(aday)* | ❌ | ✅ |
| **[Davet et]** *(işveren)* | ❌ | ✅ |
| Uyum rozeti 🟢 🟡 ⚪ | ✅ | ✅ |
| **Neden önerildi? ▾** | ✅ | ✅ |

**Kartın tamamına dokunmak da detayı açar** — [İncele] görünür bir ipucu.

| Neden bu standart | |
|---|---|
| İki taraf aynı mantıkla | Öğrenmesi kolay, ekip için tek kural |
| Başvuru / davet **bilinçli** | Karşı tarafın tamamını gördükten sonra |
| Uyumsuz başvuru / davet yığılmaz | İki tarafın listesi daha kaliteli |
| Ayıklama hızlı | İlgisizi açmadan çıkarabilir |

### 10.3 "Neden önerildi? ▾" — açılır bölüm

**Tüm kartlarda ve tüm detay ekranlarında** — kapalı başlar, dokununca açılır (Flutter `ExpansionTile`).

```
│ Neden önerildi?                           ▾  │      ← kapalı

│ Neden önerildi?                           ▴  │      ← açık
│   ✅ Profilin bu ilanla çok uyumlu            │
│   ✅ React ✓  PostgreSQL ✓                    │
│   ⚠️ .NET sende yok                           │
│   ⚠️ Tecrübe istenenin biraz altında          │
│      (3 yıl, istenen 5)                      │
│   ✅ Maaş belirtilmiş                         │
```

| Kart | Rozet | Neden önerildi? ▾ | Skor kaynağı |
|---|:---:|:---:|---|
| Aday — Keşfet | ✅ | ✅ | Sonuç tablosu (güncel) |
| Aday — Başvurularım | ✅ | ✅ | **Başvuru anındaki** anlık görüntü |
| Aday — Davetlerim | ✅ | ✅ | **Davet anındaki** anlık görüntü |
| İşveren — Adaylar | ✅ | ✅ | Sonuç tablosu (güncel) |
| İşveren — Başvuranlar | ✅ | ✅ | **Başvuru anındaki** anlık görüntü |
| İşveren — Davet Edilenler | ✅ | ✅ | **Davet anındaki** anlık görüntü |

| | |
|---|---|
| Ne zaman hesaplanır | Sadece **açılınca** — açılmayan kart için hesap yok |
| Üretim | Alt skorlar + etiket karşılaştırması → hazır şablonlar, her dile çevrilmiş — **LLM yok** |
| Maliyet | Açılan kart başına milisaniyeler |

> **Neden kapalı başlar:** Kart sade kalır; "eksik / altında" uyarıları ilk bakışta caydırmasın — ama merak eden tek dokunuşla görür, detaya girmeden bilinçli ayıklama yapar.

### 10.4 Aday — Keşfet kartı

```
┌──────────────────────────────────────────────┐
│ [İŞ İLANI]                     🟢 Yüksek uyum │
│ Bilişim ve Yazılım                           │
│ Senior Backend Developer                     │
│ ABC Teknoloji  ✓ Doğrulanmış şirket          │
│ 📍 Kadıköy, İstanbul                          │
│ Hibrit · Tam zamanlı                         │
│ 💰 80.000 – 110.000 TRY (net)                 │
│                                              │
│ Neden önerildi?                           ▾  │
│                                              │
│ [Bir daha gösterme]              [İncele →]  │
└──────────────────────────────────────────────┘
```

| Kartta | |
|---|---|
| İlan tipi | **İŞ İLANI** / **STAJ İLANI** — farklı renk rozet |
| Meslek alanı, unvan | |
| Şirket adı | + ✓ Doğrulanmış şirket |
| Konum | İlanın konumu |
| Çalışma şekli · tipi | Stajda tip yerine çalışma düzeni |
| Maaş / ücret | **Sadece belirtilmişse** |

**Detay:** İlanın tamamı + şirket profili + Neden önerildi? ▾ + alt çubukta **[Bir daha gösterme] [Başvur]**

### 10.5 İşveren — Adaylar kartı

```
┌──────────────────────────────────────────────┐
│ [Foto]  Ayşe Kaya          🟢 Yüksek uyum     │
│         Bilişim ve Yazılım                   │
│         Backend Geliştirici                  │
│         🟢 Aktif arıyorum                     │
│                                              │
│ 📍 Kadıköy, İstanbul                          │
│ 💼 4 yıl tecrübe                              │
│ 🎓 Lisans · Bilgisayar Mühendisliği · Mezun   │
│ ⏱ 1 ay içinde başlayabilir                    │
│                                              │
│ Neden önerildi?                           ▾  │
│                                              │
│ [Bir daha gösterme]              [İncele →]  │
└──────────────────────────────────────────────┘
```

| Kartta | |
|---|---|
| Fotoğraf, ad soyad, meslek alanı, unvan | |
| Arama durumu | 🟢 Aktif arıyorum / 🟡 Fırsatlara açığım |
| Konum | Yaşadığı konum |
| Toplam tecrübe | Otomatik hesaplanan yıl |
| En yüksek eğitim | Seviye · bölüm · mezun / devam ediyor |
| Başlama zamanı | |

**Kartta bilerek olmayanlar:** Cinsiyet ve askerlik (**sadece detayda** — kartta hızlı eleme sebebi olmasın), doğum tarihi, telefon, e-posta, belgeler.

**Detay:** Profilin tamamı + Neden önerildi? ▾ + alt çubukta **[Bir daha gösterme] [Davet et]**. Telefon, e-posta, doğum tarihi, belgeler kendi kurallarına göre gizli.

### 10.6 Aday — Başvurularım kartları

**Bekleyen:**
```
┌──────────────────────────────────────────────┐
│ [İŞ İLANI]                     🟢 Yüksek uyum │
│ Senior Backend Developer                     │
│ ABC Teknoloji  ✓ Doğrulanmış şirket          │
│ 📍 Kadıköy, İstanbul · Hibrit                 │
│                                              │
│ ⏳ Değerlendiriliyor                          │
│ Başvuru: 12 Eylül · 5 gün önce               │
│ Gönderdiğin: 💰 90.000 TRY (net) · 📄 3 belge  │
│                                              │
│ Neden önerildi?                           ▾  │
│                                              │
│ [Başvurumu geri çek]          [İlanı gör →]  │
└──────────────────────────────────────────────┘
```

**Sonuçlanan:**
```
┌──────────────────────────────────────────────┐
│ Frontend Developer · DEF Yazılım             │
│ ✕ Başka adaylarla ilerlendi                   │
│ Başvuru: 1 Ağustos · Sonuç: 15 Ağustos       │
└──────────────────────────────────────────────┘
┌──────────────────────────────────────────────┐
│ Backend Developer · JKL Yazılım              │
│ 🔒 İlan kapandı                               │
│ Başvuru: 20 Temmuz · Kapandı: 2 Ağustos      │
└──────────────────────────────────────────────┘
```

| Bekleyen kartında | |
|---|---|
| İlan tipi, unvan, şirket, konum | |
| Durum | ⏳ Değerlendiriliyor *(+ duraklatıldıysa notu)* |
| Başvuru tarihi + kaç gün önce | **Kalan gün gösterilmez** |
| Gönderdiği | Maaş beklentisi (yazdıysa) + kaç belge |
| [Başvurumu geri çek] · [İlanı gör →] | |

**Davetlerim** aynı yapıda — bekleyen kartında *"⏳ Cevabın bekleniyor · Davet: 15 Eylül"*, detayda **[Kabul et] [Kabul etme]**.

### 10.7 İşveren — Başvuranlar ve Davet Edilenler kartları

**Başvuranlar (Bekleyen):**
```
┌──────────────────────────────────────────────┐
│ [Foto]  Ayşe Kaya          🟢 Yüksek uyum     │
│         Backend Geliştirici                  │
│                                              │
│ ⏳ Karar bekliyor · 25 gün kaldı               │
│ Başvuru: 12 Eylül                            │
│ 💰 90.000 TRY (net) · 📄 3 belge               │
│                                              │
│ Neden önerildi?                           ▾  │
│                                              │
│ [Profili ve başvuruyu gör →]                 │
└──────────────────────────────────────────────┘
```
Detayda **[Kabul et] [Kabul etme]**. Başvuranlar **başvuru anındaki skora** göre sıralı.

**Davet Edilenler (Bekleyen):**
```
┌──────────────────────────────────────────────┐
│ [Foto]  Ayşe Kaya          🟢 Yüksek uyum     │
│         Backend Geliştirici                  │
│                                              │
│ ⏳ Cevap bekleniyor · 7 gün kaldı              │
│ Davet: 15 Eylül · Davet eden: Mehmet Kaya    │
│                                              │
│ Neden önerildi?                           ▾  │
│                                              │
│ [Daveti geri çek]            [Profili gör →] │
└──────────────────────────────────────────────┘
```

### 10.8 İşveren — ilan kartı

```
┌──────────────────────────────────────────────────────────────────┐
│ Bilişim ve Yazılım                                🟢 Yayında     │
│ Senior Backend Developer                                         │
│ 👤 Mehmet Kaya · 📍 Kadıköy, İstanbul · ⏱ 12 gün kaldı             │
│ 👁 340 görüntülenme                                               │
│                                                                  │
│   124        8           10                7              3      │
│ [Adaylar] [Başvuranlar] [Davet Edilenler] [Süreçteki Adaylar] [Gizlenenler] │
│                                                                  │
│ [Uzat]  [Duraklat]  [Kapat]                                      │
└──────────────────────────────────────────────────────────────────┘
```

| Kartta | |
|---|---|
| Meslek alanı, unvan, durum, açan kişi, konum, kalan gün, görüntülenme | |
| Sekme sayıları | Uygun · **bekleyen** başvuru · **cevap bekleyen** davet · süreçte · gizlenen *(kabul/red dökümü yok)* |

| Buton | |
|---|---|
| **[Uzat]** | 15 / 30 / 60 gün — kesintisiz en fazla 180 |
| **[Duraklat]** → **[Devam ettir]** | Geçici gizleme, yeni başvuru gelmez |
| **[Kapat]** | *Pozisyon doldu* / *Alımdan vazgeçtik* |
| **[Tekrar yayınla]** | Süresi dolmuş / kapatılmış ilan — aynı ilan, geçmiş korunur |

> Sayaçların nasıl tutulduğu → `isveren-paneli-v2.md`

### 10.9 Gizlediklerim / Gizlenenler

| | Aday — Gizlediklerim | İşveren — Gizlenenler |
|---|---|---|
| Nerede | Profilim altında | **Her ilanın kendi sekmesi** |
| İçerik | Sadece **hâlâ yayında** olan ilanlar | O ilanda gizlenen adaylar |
| [Geri al] | İlan tekrar Keşfet'e düşer *(filtreye uyuyorsa)* | Aday tekrar Adaylar'a düşer |
| İlan kapanırsa | Listeden düşer; tekrar yayınlanırsa gizli olarak geri gelir | Gizleme kaydı korunur |

### 10.10 Kartlar tükendiğinde

[Daha fazla getir] yeni sonuç döndürmediyse:

```
Aday:                                       İşveren (ilan bazlı):
Şu an sana uygun tüm ilanları gördün.       Bu ilana uygun tüm adayları gördün.
🔔 Yeni ilan gelince haber ver [Açık]       🔔 Yeni aday gelince haber ver [Açık]
[Profilini güçlendir]                       [İlanını güçlendir]
```

> **Kart tükenmesi hata değil** — kullanıcı tüm uygunları gördü. Çözüm arayüzde değil, havuzu büyütmekte.

**Ertelenen:** *"Uzaktan da açarsan 12 ilan daha var"* gibi filtre gevşetme önerisi — her filtre için ayrı sorgu gerektirir, gevşetince gelen sonuçlar güveni zedeleyebilir.

## 11. Mimari — İlk Günden 1 Milyon+ İçin

> **Karar:** Mimari ilk günden **1 milyon+ kullanıcıya** göre kuruluyor. Büyüme anında taşıma, yeniden yazma, kesinti olmasın.
>
> **Bilinçli bedeli:** Kullanıcı azken de okuma kopyası gibi parçalar için ödeme yapılır; ilk günden daha çok parça yönetilir.

### 11.1 Genel şema

```
 Flutter / Next.js
        │
        ▼
 ┌──────────────────────────────────────────┐
 │ SUPABASE                                 │
 │  Auth · RLS · Edge Functions (istek anı) │
 │                                          │
 │  Ana Postgres  ── bölümlü tablolar       │
 │       │                                  │
 │       ├─▶ Kuyruklar (Supabase Queues)    │
 │       │      ├─ embedding  ─▶ Edge Function işçisi
 │       │      ├─ eslestirme ─▶ Edge Function işçisi
 │       │      ├─ ayristirma ─▶ Edge Function işçisi
 │       │      └─ bildirim   ─▶ Edge Function işçisi ─▶ FCM / e-posta
 │       │                                  │
 │       └─▶ Okuma kopyası ◀── Keşfet okur  │
 └──────────────────────────────────────────┘
        │
        ├─▶ Önbellek (Keşfet ilk sayfası)
        ├─▶ Embedding API · LLM API
        └─▶ ClamAV container (dosya taraması)
```

### 11.2 İlk günden kurulan parçalar

| Parça | Karar | Neden |
|---|---|---|
| **Bölümleme** | Sonuç **32**, aksiyon **16**, mesajlar **16** (hash); görüntülenme, bildirim, iş kayıtları **zaman** | Boş tabloyu bölmek kolay, dolu tabloyu bölmek zor (Bölüm 6.9) |
| **İşçiler** | Her biri **ayrı Edge Function**, taşınabilir | Ek sunucu yok; gerekirse tek tek container'a (Bölüm 8.7) |
| **Okuma kopyası** | Keşfet ve listeler buradan okur | Okuma, yazma yüküyle yarışmaz |
| **Önbellek** | Keşfet'in ilk sayfası kısa süreli önbellekte (örn. Upstash Redis) | Aynı kullanıcı tekrar açınca veritabanına gidilmez |
| **Vektör arama** | `pgvector` + tüm aramalar **`vektor_ara()`** arkasında | Gerekirse ayrı vektör veritabanına geçiş tek fonksiyon |
| **Servis soyutlamaları** | `embed()`, `ayristir()`, `eslestir()` tek fonksiyon arkasında | Sağlayıcı / yer değişince tek yerde |
| **Bölge** | Tek bölge + **bölgeye hazır şema** (`ulke_kodu`, kişisel veri ile ilan verisi ayrı) | Çok bölge ancak lansman ülkeleri netleşince |
| **Ölçüm** | Kuyruk uzunluğu, iş süresi, hata oranı, okuma kopyası gecikmesi — pano + uyarı | Darboğaz tahminle değil veriyle görülür |

*Supabase okuma kopyaları, Queues ve Edge Function sınırları plana göre değişir — kurulumdan önce güncel koşullar kontrol edilmeli.*

### 11.3 Neden ayrı vektör veritabanı ilk günden değil

Vektörleri iki yerde tutmak (Postgres + ayrı vektör veritabanı) **senkronizasyon sorunu** demek — bir yerde güncellenip diğerinde güncellenmeyen vektör sessizce yanlış eşleşme üretir. `pgvector` milyonlarca vektörde çalışıyor; `vektor_ara()` soyutlaması sayesinde gerçekten gerekirse geçiş tek fonksiyonu değiştirmek olur. Geçiş sinyali: filtre ile indeks çekişmesinin (Bölüm 4.4) indeks ayarıyla çözülememesi.

### 11.4 Bölgeler ve uzaktan ilanlar — ileride

Yasalar veriyi bölge içinde tutmayı isterse (AB, Türkiye…):

| Veri | Nerede |
|---|---|
| **Aday verisi** (kişisel) | Kendi bölgesinde kalır |
| **İlan verisi** (kişisel veri yok) | **Tüm bölgelere kopyalanır** — her bölge global ilan havuzuna kendi içinden bakar |
| Eşleşme olunca | Sadece gereken bilgi karşı tarafa açılır |

**Neden şimdi kurulmuyor:** Hangi ülkeyle başlanacağı belli değil; çok bölgeli veritabanı ve ilan kopyalama ciddi karmaşıklık. Şema hazır, karar lansman planıyla verilir.

### 11.5 Darboğaz nerede çıkar — beklenti

| Muhtemel darboğaz | Önlem |
|---|---|
| **Vektör arama** (uzaktan ilanlarda geniş havuz) | Kısmi HNSW indeksler, `ef_search` ayarı, `vektor_ara()` soyutlaması |
| **Embedding API sınırı** | Toplu gönderim, kuyruk, tekrar deneme |
| **Eşleştirme hesabı** (popüler ilan × binlerce aday) | Parçalı işler, işçi kopyası, gerekirse container |
| **Edge Function sınırları** | Küçük partiler, taşıma sinyalleri |
| **Aday sorgusu 32 bölüme bakıyor** | `aday_id` indeksi, okuma kopyası, önbellek; gerekirse aday yönü için ikinci tablo |

## 12. Yayından Önce — İki Test

> ⚠️ **İkisi de atlanırsa sonraki aylar boşa gidebilir.**

### 12.1 Doğrulama testi — eşleşme kalitesi

**Arayüz yazmadan önce:**

1. 20-30 gerçek aday profili ve 5-10 gerçek ilan **elle** veritabanına girin
2. Eşleştirme fonksiyonunu çalıştırın
3. Sonuçları gerçek kişilere gösterin:
   - Adaya: *"Bu ilanlar senin için uygun mu?"*
   - İşverene: *"Bu adaylar ilanına uygun mu?"*
4. Aynı test setiyle **embedding sağlayıcılarını** ve **LLM ayrıştırma doğruluğunu** karşılaştırın
5. Cevap zayıfsa: ağırlıkları, formülleri, modeli değiştirin

**Neden:** Eşleştirme kalitesi ürünün tamamını belirliyor. Arayüz yazıldıktan sonra "eşleştirme kötü" anlaşılırsa üstüne kurulan her şey sorgulanır.

### 12.2 Yük testi — hız ve dayanıklılık

**Yayından önce, gerçeğe yakın ölçekte:**

```
1. Sahte veri üret
   5 milyon aday · 500 bin ilan · 1 milyar sonuç satırı
2. Gerçekçi trafik simüle et
   - Aynı anda binlerce Keşfet açılışı
   - Saatte binlerce ilan / profil güncellemesi
   - "Pazartesi sabahı" — binlerce pasif kullanıcının aynı anda dönüşü
   - Popüler ilan — tek ilan × on binlerce aday
3. Ölç
   - Keşfet açılış süresi
   - Kuyruk birikmesi ve boşalma süresi
   - İşçi süreleri, hata oranı
   - Okuma kopyası gecikmesi
4. Hedefler
   - Keşfet  < 300 ms
   - Kuyruk dakikalar içinde boşalıyor
   - İşçiler Edge Function süre sınırının altında
5. Zorlanan yeri bul → düzelt → tekrar ölç
```

**Neden:** Hiçbir mimari için "hiç yavaşlamaz" garantisi verilemez. Garantiye en yakın şey, gerçek yükten önce ölçmek.

## 13. Açık Kalanlar — Veriyle Ayarlanacaklar

Tasarım kararlarının hepsi verildi. Kalanlar **ilk gerçek veriyle** ayarlanacak sayılar:

| # | Konu | Başlangıç | Nasıl karar verilecek |
|:---:|---|---|---|
| 1 | Embedding sağlayıcısı | OpenAI veya Cohere | Doğrulama testinde yan yana |
| 2 | Skor ağırlıkları | 0.65 / 0.20 / 0.10 / 0.05 | Gerçek eşleşmelere gözle bakılarak |
| 3 | Uyum seviyesi eşikleri | 0.75 / 0.55 | Skor dağılımına göre |
| 4 | Aktif kullanıcı eşiği | 14 gün | Kullanıcı davranışı |
| 5 | Liste boyutu | 200 | Kullanıcıların kaç kart incelediği |
| 6 | Günlük özet eşiği | İlk 50 | Bildirim açılma ve kapatılma oranı |

Hepsi veritabanında — kod değişmeden ayarlanır.
