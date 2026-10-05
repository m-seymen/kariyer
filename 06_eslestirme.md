# İş Eşleştirme Projesi — Eşleştirme Motoru

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak
> Durum: Kararlar tamamlandı, ekip tartışmasına hazır
> İlgili dokümanlar: `ilan-olusturma.md`, `aday-profili.md`, `sirket-profili.md`

---

## 0. Yönetici Özeti

Eşleştirme motoru, **ürünün kalbi**. Diğer tüm bölümler (profil, ilan, doğrulama) bu motora veri hazırlamak için var.

**Motorun tek cümlelik tanımı:**

> Katı filtreyle havuzu daraltır, vektör benzerliğiyle anlamsal yakınlığı ölçer, ağırlıklı skorla iki tarafın da çıkarına göre ayrı ayrı sıralar ve sonucu önceden hazırlar.

**Altı aşama:**

```
1. EMBEDDING       Metin → vektör (bir kez, metin değişince)
       ↓
2. KATI FİLTRE     5.000.000 → 500   (SQL, milisaniyeler)
       ↓
3. VEKTÖR          500 → 200         (pgvector, milisaniyeler)
       ↓
4. SKOR            Her çift için İKİ skor (aritmetik)
       ↓
5. SONUÇ TABLOSU   Önbellek — Keşfet buradan okur
       ↓
6. TETİKLEYİCİLER  Ne zaman çalışır, bakım işleri
```

**Kritik tasarım kararları — bir bakışta:**

| Karar | Gerekçe |
|---|---|
| **LLM yok** | Maliyet, hız, açıklanabilirlik. Embedding + aritmetik yeterli |
| **CV eşleştirmeye girmez** | Adil ve savunulabilir sıralama; düzenleyici uyum |
| **Şirket profili eşleştirmeye girmez** | Eşleştirme ilan bazlı; aynı şirketin ilanları birbirine benzemesin |
| **Konum skorda değil, katı filtrede** | Aday zaten kendi seçti; ikinci kez yorumlamak gereksiz |
| **Eşik değil limit** | Kıtlık döneminde boş ekran olmasın |
| **İki yön, iki skor** | İşveren ve adayın çıkarı farklı (fazla nitelikli aday örneği) |
| **Tek tablo, iki skor kolonu** | Hangi taraf tetiklerse diğerinin listesi de güncellenir |
| **Karma hesaplama** | Aktif kullanıcı önceden, pasif kullanıcı açınca — israf %85 azalır |
| **Tüm ağırlıklar veritabanında** | Kod değişmeden ayarlanabilir |

---

## 1. Embedding Üretimi

### Ne zaman çalışır

Aday profili veya ilan **kaydedildiğinde / güncellendiğinde**.

```
Metin alanları birleştirilir
  → metin_hash hesaplanır
  → eski hash ile karşılaştırılır
       değişmişse  → embedding API'ye gider → vektör saklanır
       değişmemişse → hiçbir şey yapılmaz
```

> **`metin_hash` neden kritik:** Kullanıcılar profillerini sık kaydeder ama metin çoğu zaman aynıdır (sadece maaş veya konum değişmiştir). Hash kontrolü olmasa her kayıtta API'ye gidilir — gereksiz maliyet ve gecikme.

### Aday vektörüne giren metinler

| Alan | Kaynak adım | Not |
|---|:---:|---|
| Unvan | Profil 2 | Metnin ilk satırı |
| Kısa tanıtım | Profil 1 | Tamamı |
| Yetenek adları | Profil 5 | **Yıl HARİÇ** |
| Deneyim: pozisyon | Profil 4 | Her kayıt |
| Deneyim: "bu işte ne yaptın" | Profil 4 | Her kayıt — **en zengin kaynak** |
| Eğitim: bölüm adı | Profil 3 | **Okul adı ve not HARİÇ** |
| Sertifika adları | Profil 7 | **Veren kurum ve tarih HARİÇ** |
| Proje adı + açıklaması | Profil 8 | **Link ve tarih HARİÇ** |

### İlan vektörüne giren metinler

| Alan | Kaynak adım | Not |
|---|:---:|---|
| İlan başlığı / unvan | İlan 1 | Metnin ilk satırı |
| Yetenek adları | İlan 2 | **Yıl ve zorunlu/tercihen HARİÇ** |
| Tercih edilen bölüm | İlan 2 | |
| Sertifika adları | İlan 2 | |
| İş tanımının 4 soru cevabı | İlan 3 | **En zengin kaynak** |

### Hiç girmeyenler — ve nedenleri

| Kategori | Örnekler | Neden |
|---|---|---|
| **Sayısal alanlar** | Yıl, maaş, not, tarih, çalışan sayısı | Vektör uzayında anlamlı çalışmıyor — "3 yıl" ile "5 yıl" neredeyse aynı görünür |
| **Katı filtre alanları** | Konum, dil, çalışma şekli, meslek alanı, ilan tipi | Zaten kesin olarak filtreleniyor; vektöre karışırsa sinyali seyreltir |
| **Kişisel / hassas** | Ad, doğum tarihi, cinsiyet, fotoğraf | Ayrımcılık riski |
| **Şirket profili** | Tanıtım, yan haklar, sektör | Aynı şirketin farklı pozisyonları birbirine benzemesin |
| **Okul adı** | | Okul bazlı önyargı riski |
| **CV** | | Herkes aynı kurallarla değerlendirilsin |

> **İki altın kural:**
> 1. Sayısal alanlar embedding'e girmez — aritmetikle hesaplanır.
> 2. Katı filtreye giren alanlar embedding'e girmez — çift sayılmasın.

### Saklama

```sql
CREATE TABLE embeddings (
  varlik_tipi  text   NOT NULL,   -- 'aday' | 'ilan' | (ileride başka türler)
  varlik_id    bigint NOT NULL,
  model_adi    text   NOT NULL,
  boyut        int    NOT NULL,
  metin_hash   text   NOT NULL,
  vektor       vector(1024) NOT NULL,
  guncelleme   timestamptz DEFAULT now(),
  PRIMARY KEY (varlik_tipi, varlik_id)
);
```

**`varlik_tipi` genişletilebilir.** Yarın yeni bir eşleşme türü (örneğin özel ders ilanı) eklenirse şema değişmez, sadece yeni bir değer eklenir. Ancak o tür için eşleştirme tablosu ve filtre fonksiyonu ayrı kurulur.

> Not: **Staj ayrı bir varlık türü değildir.** `ilanlar` tablosunda `ilan_tipi = 'staj'` olarak durur, `varlik_tipi = 'ilan'` kullanır.

**`model_adi` ve `boyut` neden var:** Model değiştiğinde vektör boyutu da değişir ve **tüm kayıtların yeniden üretilmesi** gerekir. Bu kolonlar olmadan hangi kaydın eski olduğunu bilemezsiniz. "Yeniden vektörle" işi baştan yazılmalı.

### Embedding servisi

| Karar | Gerekçe |
|---|---|
| **API ile başla** | Hız; MVP'de kendi sunucu kurmak gereksiz yük |
| **Tek çok dilli model** | Tüm diller aynı vektör uzayında — "Buchhaltung" ile "muhasebe" yakın çıkar, çeviri gerekmez |
| **1024 boyut** | pgvector HNSW indeksi 2000 boyuta kadar destekliyor; 3072 indekslenemez. Kalite kaybı çok az |
| **`embed()` tek fonksiyon** | Sağlayıcı değişimi tek dosya değişikliği |
| **Flutter'dan asla çağrılmaz** | API anahtarı istemciye gömülürse çalınır |

**Adaylar:** OpenAI `text-embedding-3-large` (`dimensions=1024`) veya Cohere `embed-multilingual-v3`.

> 💬 **Ekip tartışması için:** Sağlayıcı seçimi masabaşında yapılmamalı. **20-30 gerçek profil ve 5-6 gerçek ilan** ile iki modeli yan yana test edin — hangisi doğru adayı üste çıkarıyor, gözle görülür. Yarım günlük iş.

> 💡 **Kendi servise geçiş sinyali:** Aylık fatura belirli bir eşiği aşınca veya kurumsal müşteri "verilerimiz yurt dışına çıkmasın" deyince. Açık kaynak çok dilli modeller (BGE-M3, multilingual-E5) aday.

---

## 2. Katı Filtre

### Amacı

Vektör hesabından **önce** havuzu daraltmak. Pahalı işlemi sadece küçük bir küme üzerinde yapmak.

```
5.000.000 aday
  → katı filtre
  → ~500 aday
  → vektör hesabı sadece bunlar üzerinde
```

### Filtre sırası — en çok eleyen önce

```
1. Sistem kontrolleri   ilan yayında mı, aday aktif mi, profil eşiği
2. Ülke / konum         en çok eleyen (uzaktan istisnası var)
3. Meslek alanı         kaba filtre
4. İlan tipi            iş / staj
5. Çalışma şekli        dizi kesişimi
6. İş dili              dizi kesişimi
7. Koşullu filtreler    eğitim, ehliyet, seyahat, vardiya
8. Dışlamalar           gizlenen şirketler, önceki aksiyonlar
```

### Konum — üç yollu kural

```
Konum filtresi UYGULANIR, şu durumlar HARİÇ:

  1. Aday "her konumda çalışmaya hazırım" işaretlemişse
  2. "uzaktan" ∈ (aday.calisma_sekli ∩ ilan.calisma_sekli)
```

**Dikkat — uzaktan için iki taraf da uzaktan demeli:**

| İlan | Aday | Ortak | Konum |
|---|---|---|---|
| [uzaktan] | [uzaktan] | uzaktan | **Atlanır** |
| [ofis, uzaktan] | [uzaktan] | uzaktan | **Atlanır** |
| [ofis, uzaktan] | [ofis] | ofis | **Uygulanır** — aday ofise gidecek |
| [uzaktan] | [ofis] | — | Zaten elenir (kesişim yok) |
| [hibrit] | [hibrit] | hibrit | **Uygulanır** — hibrit konum gerektirir |

**Konum uygulandığında — ağaç yolu:**

```sql
i.konum_yolu LIKE ak.konum_yolu || '%'
```

| Aday seçimi | İlan konumu | Eşleşir |
|---|---|:---:|
| `TR` | `TR/İstanbul/Kadıköy` | ✅ |
| `TR/İstanbul` | `TR/Ankara/Çankaya` | ❌ |
| `US/California` | `US/California/San Francisco` | ✅ |
| `DE` | `DE/Bayern/München` | ✅ |

Üst seviye seçilirse altı dahil.

> ⚠️ **Global ölçek için önemli sonuç:** Uzaktan ilanlarda ülke filtresi atlandığı için havuz **tüm dünya** oluyor. O durumda **meslek alanı ve iş dili** en değerli filtreler haline geliyor. Bu, "iş dili boş kalırsa hiç eşleşme olmaz" riskinin neden bu kadar kritik olduğunu gösteriyor.

### Dizi kesişimleri

Çalışma şekli, iş dili, sürücü sınıfı — hepsi çoklu, Postgres dizi tipi ve `&&` (overlap) operatörü:

```sql
a.calisma_sekli && i.calisma_sekli
a.is_dilleri    && i.is_dilleri
```

> Ayrı tablo + `EXISTS` yazmaktan kat kat hızlı. **GIN indeksi zorunlu.**

### Koşullu filtreler

Yalnızca **ilan o şartı istemişse** devreye girer:

```sql
(i.min_egitim_isced IS NULL OR a.en_yuksek_isced >= i.min_egitim_isced)
(NOT i.surucu_belgesi_gerekli OR a.surucu_siniflari && i.surucu_siniflari)
(NOT i.vardiya_gerekli OR a.vardiya_uygun)
(NOT i.seyahat_gerekli OR a.seyahat_uygun)
```

İlan dokunmadıysa filtre hiç çalışmaz. **Askerlik filtrede yok** — sadece bilgi.

### Dışlamalar

**Gizlenen şirketler — sadece işveren yönünde:**

```sql
NOT EXISTS (SELECT 1 FROM aday_gizlenen_sirketler
            WHERE aday_id = a.id AND sirket_id = i.sirket_id)
```

Aday o şirketin ilanlarını görmeye devam eder.

**Önceki aksiyonlar — taraf bazlı:**

| Aksiyon | Aday Keşfet'i | İşveren Keşfet'i |
|---|:---:|:---:|
| Aday ilanı **geçti** | ❌ gizlenir | ✅ görünür |
| İşveren adayı **geçti** | ✅ görünür | ❌ gizlenir |
| Aday **başvurdu** | ❌ (Başvurularım'a taşındı) | — |
| İşveren **davet etti** | — | ❌ (Davet Ettiklerim'e taşındı) |
| Başvuru/davet **reddedildi** | ❌ | ❌ |

> **Geçme tek yönlü:** Aday bir ilanı geçtiyse, işveren o adayı hâlâ görür ve davet edebilir — aday fikrini değiştirebilir. Tersi de geçerli.
>
> **Reddedilme iki tarafı bağlar:** Konu kapanmıştır, tekrar karşılaşılmaz.
>
> **İlan bazlı:** İşveren bir adayı "Backend" ilanında geçti diye, aynı aday "Frontend" ilanında görünmeye devam eder. Aksiyon `(ilan_id, aday_id)` çiftine yazılır, şirkete değil.

### Tek fonksiyonda toplanır

```sql
CREATE FUNCTION uygun_adaylar(p_ilan_id bigint) ...
CREATE FUNCTION uygun_ilanlar(p_aday_id bigint) ...
```

> **Neden:** Filtre kuralları değişecek (yeni alan, gevşetme kuralı). Sorgulara dağıtılırsa bir yerde unutulur ve sistem **sessizce** yanlış sonuç üretir.

### Gerekli indeksler

```sql
-- Yayında olan ilanlar için kısmi indeks
CREATE INDEX ON ilanlar (ulke_kodu, meslek_alani_id, ilan_tipi)
  WHERE durum = 'yayinda';

CREATE INDEX ON ilanlar (konum_yolu text_pattern_ops);
CREATE INDEX ON ilanlar USING gin (calisma_sekli);
CREATE INDEX ON ilanlar USING gin (is_dilleri);

CREATE INDEX ON aday_profilleri (ana_meslek_alani_id);
CREATE INDEX ON aday_profilleri USING gin (calisma_sekli);
CREATE INDEX ON aday_profilleri USING gin (is_dilleri);
CREATE INDEX ON aday_konumlari (konum_yolu text_pattern_ops);
```

> **Kısmi indeks neden:** 500 bin ilanın 50 bini yayındaysa indeks 10 kat küçük ve hızlı. İlan durumunu bölümlendirme anahtarı yapmak yerine bu yöntem kullanılıyor.

---

## 3. Vektör Karşılaştırması

### Hesap

```sql
SELECT a.id, (ie.vektor <=> ae.vektor) AS mesafe
FROM ... [katı filtre]
ORDER BY mesafe
LIMIT 200;
```

`<=>` pgvector'ün kosinüs mesafe operatörü.

```
anlamsal_benzerlik = 1 - mesafe
```

### Eşik değil, limit

> ❌ "0.7 üstü olanlar gösterilsin"
> ✅ "En iyi 200 kayıt alınsın"

**Neden eşik kullanılmıyor:**
- Kıtlık döneminde hiçbiri eşiği geçmezse **boş ekran**. Halbuki içlerinde makul olanlar vardır.
- Eşik değeri modele bağlı ve sezgisel değil — bir modelde 0.7 iyi, diğerinde 0.85.

> 💡 **Yumuşak eşik (opsiyonel):** Eleme için değil, işaretleme için kullanılabilir. Çok düşük skorlu kartlar "düşük uyum" etiketiyle gösterilir veya listenin sonunda ayrı başlık altında durur.

### Performans

Süreler **katı filtreden kalan havuz** üzerinde:

| Havuz | Süre (HNSW indeksli) | Ne zaman görülür |
|---|---|---|
| 500 | ~2-5 ms | Normal durum |
| 2.000 | ~5-20 ms | Geniş filtre |
| 50.000 | ~20-50 ms | Uzaktan ilan, geniş meslek alanı |
| 500.000 | ~50-150 ms | Uzaktan, dünya geneli, çok geniş alan |

Arka planda çalıştığı için kullanıcı fark etmez.

### Vektör indeksi

```sql
CREATE INDEX ON embeddings USING hnsw (vektor vector_cosine_ops);
```

**Ne indeksleniyor:** `embeddings.vektor` kolonu.

**İndeks olmadan:** Postgres havuzdaki her vektörü açıp mesafe hesaplar. 500 bin kayıt × 1024 sayı = saniyeler.

**HNSW nasıl çalışır:** Vektörleri bir ağ yapısında tutar — birbirine yakın vektörler komşu olarak bağlanır. Arama rastgele bir noktadan başlar, her adımda hedefe en yakın komşuya atlar, ilerleyemeyince durur. Yarım milyon kaydı taramak yerine birkaç yüz adımda sonuca varır. "Yaklaşık" arama — %95+ doğrulukla ve kat kat hızlı.

### ⚠️ Bilinen risk — filtre ile indeks çekişmesi

Katı filtre çok seçiciyse (havuz 500'e iniyor) HNSW "en yakın 200"ü getirir ama onların çoğu filtreden geçmeyebilir. Postgres daha çok çeker, yine yetmez, sonunda sıralı taramaya düşer. Halbuki 500 kaydı doğrudan taramak 2 ms sürerdi.

**Çözümler:**
- `SET hnsw.ef_search = 200` (varsayılan 40) — aramayı genişletir
- Küçük havuzlarda planlayıcıyı indeks kullanmaktan vazgeçirmek

> 💬 **Ekip için:** Bu gerçek veriyle `EXPLAIN ANALYZE` ile ölçülmeden ayarlanamaz. İlk yük testinde mutlaka bakılmalı.

---

## 4. Skor Hesabı

### Neden embedding tek başına yetmiyor

Embedding tüm metni **ortalayarak** bakıyor. ~200 kelimelik bir profilde "React" tek bir kelime; varlığı veya yokluğu skoru belki %2 oynatıyor. Ama işveren için "React zorunlu" kritik bir şart.

```
İlan: React zorunlu, Node.js zorunlu

Aday A: React 5 yıl, Node.js 4 yıl
Aday B: Vue 5 yıl, Django 4 yıl

Sadece embedding:   A = 0.87,  B = 0.84    ← neredeyse aynı!
```

İkisi de "web geliştirici" anlamı taşıyor. Embedding aradaki farkı 3 puan görüyor.

**Diğer bileşenler devreye girince:**

```
A: 0.65 × 0.87 + 0.20 × 1.0 + ... = 0.766
B: 0.65 × 0.84 + 0.20 × 0.0 + ... = 0.546
```

Fark 3 puandan **22 puana** çıktı. A açıkça üstte.

> **Bileşenlerin işi:** Embedding'in "aynı gibi" gördüğü iki adayı, işverenin gerçekten önemsediği kesin kriterlerle ayırmak. Embedding "genel olarak ne kadar benziyor" der; diğer bileşenler "şart koşulan şeyler var mı" der.

### İki yön — neden iki skor

**Aynı çift, iki taraf için farklı değerde.** En net örnek:

```
İlan:  Junior Developer, 1-2 yıl tecrübe
Aday:  8 yıl tecrübeli

İşveren: "8 yıllık biri junior ilanıma uygun, harika"  → yüksek
Aday:    "Junior pozisyon mu? Bu geriye adım"           → düşük
```

Tek skor kullanılırsa ya işverene iyi aday gösterilmez ya adaya istemediği ilan üstte gösterilir.

**İki taraf da kendi Keşfet'ini açıyor ve kendi sıralamasını görüyor:**
- İşveren: ilanı seçer → "bu ilana uyan adaylar" → `skor_isveren` ile sıralı
- Aday: Keşfet'i açar → "bana uyan ilanlar" → `skor_aday` ile sıralı

### Formül

```
skor_isveren = 0.65 × anlamsal          (ortak)
             + 0.20 × yetenek           (ortak)
             + 0.10 × tecrube_isveren   (farklı)
             + 0.05 × aday_kalitesi     (farklı)

skor_aday    = 0.65 × anlamsal          (aynı sayı)
             + 0.20 × yetenek           (aynı sayı)
             + 0.10 × tecrube_aday      (farklı)
             + 0.05 × ilan_kalitesi     (farklı)
```

| Bileşen | Ağırlık | İki yönde | Embedding'de var mı |
|---|:---:|---|:---:|
| Anlamsal | %65 | Aynı | — |
| Yetenek | %20 | Aynı | ✅ (eriyerek) |
| Tecrübe | %10 | **Farklı** | ❌ |
| Kalite | %5 | **Farklı** | ❌ |

> **Konum skorda yok.** Katı filtrede kaldı. Gerekçe: filtreyi geçen tüm ilanlar adayın **kendi seçtiği** konumlarda. Aralarında sıralama yapmak adayın kararını ikinci kez yorumlamak olur. Konumun %10'u anlamsal ve yeteneğe dağıtıldı.

> **Maliyet notu:** Pahalı kısım (vektör mesafesi) bir kez hesaplanıyor, iki skorda da aynı sayı kullanılıyor. Farklılaşan bileşenler basit aritmetik. İkinci skorun maliyeti sıfıra yakın.

---

### 4.1 `anlamsal` — %65, iki yönde aynı

```
anlamsal = 1 - (aday.vektor <=> ilan.vektor)
```

Vektör mesafesi simetrik — "A'dan B'ye" ile "B'den A'ya" farkı yok. Tek hesap, iki skorda aynı değer.

**Bu tek sayının içinde:** Unvan, tanıtım, yetenek adları, deneyim açıklamaları, bölüm, sertifikalar, projeler — hepsi **anlam olarak** karşılaştırılmış. "Ödeme altyapısı" ile "ödeme sistemleri", "mikroservis" ile "dağıtık mimari" arasındaki yakınlıkları yakalıyor.

**Kritik nokta:** Bu sayı girdinin zenginliğine bağlı. Aday sadece "Backend Geliştirici / Bilgisayar Mühendisliği" yazmışsa vektör de o kadar bilgi taşıyor ve benzerlik kaba çıkıyor. Bu yüzden deneyim açıklamaları zorunlu tutuldu.

---

### 4.2 `yetenek` — %20, iki yönde aynı

**Soru:** "İlanın istediği yetenekler adayda var mı?"

**Yeteneklerin iki kez sayılması kasıtlı:** Embedding'de "eriyerek" var (%65'in içinde küçük bir pay), burada **kesin olarak** sayılıyor. Embedding kör noktasını kapatıyor:

| | Ne ölçüyor | Güçlü yanı | Kör noktası |
|---|---|---|---|
| Embedding | Bütünsel anlam | Farklı isimleri köprüler ("React" ↔ "frontend") | Kesin varlık/yokluk, sayılar |
| Yetenek bileşeni | Etiket + yıl | Kesin, ölçülebilir | Eş anlamlıları kaçırır |

#### Formül

**Adım 1 — Her zorunlu yeteneği puanla:**

```
Adayda var, yıl ≥ istenen   → 1.0
Adayda var, yıl < istenen   → 0.5 + 0.5 × (aday_yıl / istenen_yıl)
Adayda yok                  → 0.0
```

> **Neden `0.5 +` ile başlıyor:** Yeteneğin **var olması** tek başına değerli. 1 yıl React bilen, hiç bilmeyenden çok daha iyi. Sıfırdan başlatmak haksızlık olur.

```
İstenen 3 yıl, adayda 1 yıl → 0.5 + 0.5 × (1/3) = 0.67
İstenen 3 yıl, adayda 2 yıl → 0.5 + 0.5 × (2/3) = 0.83
İstenen 3 yıl, adayda 5 yıl → 1.0  (fazlası tam puan)
```

**Adım 2 — Zorunluların ortalaması:**

```
zorunlu_puan = ortalama(zorunlu yetenek puanları)
```

**Adım 3 — Tercihen yetenekler:**

```
Adayda var → 1.0     Yok → 0.0     (yıl kontrolü yok)

tercihen_puan = ortalama(tercihen yetenek puanları)
```

**Adım 4 — Birleştir:**

```
yetenek = 0.75 × zorunlu_puan + 0.25 × tercihen_puan
```

Zorunlular 3 kat ağır — işverenin gerçekten şart koştuğu şeyler.

#### Tam örnek

```
İLAN istiyor:                      ADAYDA var:
  React    3 yıl  zorunlu            React       5 yıl
  Node.js  2 yıl  zorunlu            Docker      1 yıl
  Docker   —      tercihen            Kubernetes  2 yıl
                                      Go          3 yıl

Zorunlu:
  React:   var, 5 ≥ 3      → 1.0
  Node.js: yok             → 0.0
  zorunlu_puan = (1.0 + 0.0) / 2 = 0.5

Tercihen:
  Docker:  var             → 1.0
  tercihen_puan = 1.0

yetenek = 0.75 × 0.5 + 0.25 × 1.0
        = 0.375 + 0.25
        = 0.625
```

#### Özel durumlar

| Durum | Sonuç |
|---|---|
| İlan hiç yetenek yazmamış | `0.5` (nötr) |
| Sadece zorunlu var, tercihen yok | `zorunlu_puan` tek başına |
| Sadece tercihen var | `tercihen_puan` tek başına |

#### ⚠️ Yön farkı neden kaldırıldı

İlk tasarımda aday yönü farklıydı: *"yeteneklerimin kaçı bu işte kullanılıyor"*

```
yetenek_aday = (ilanda geçen yeteneklerim) / (toplam yeteneklerim)
```

**Sorun:** Bu formül **çok yetenek yazmayı cezalandırıyordu.** 10 yetenek yazan aday, 2 yetenek isteyen ilanla eşleşince 2/10 = 0.2 alıyordu.

**Ters teşvik:** Aday bunu fark ederse yeteneklerini eksik yazar → hem etiket kesişimi hem embedding zayıflar → kendi ölçtüğümüz şeyi bozmuş oluruz.

**Karar:** Yetenek bileşeni iki yönde aynı. "Bu iş bana dar geliyor" bilgisi zaten **tecrübe bileşeninden** geliyor.

---

### 4.3 `tecrube` — %10, iki yönde FARKLI

**Tecrübe yılı nereden geliyor:** Aday ayrıca girmiyor. Deneyim kayıtlarındaki tarihlerden otomatik:

| Çalışma türü | Ağırlık |
|---|:---:|
| Tam zamanlı | × 1.0 |
| Freelance | × 1.0 |
| Yarı zamanlı | × 0.5 |
| Staj | × 0.5 |
| Dönemsel | × 0.5 |
| Gönüllü | Hesaba girmez |

**Örtüşen dönemler iki kez sayılmaz** — aynı anda iki işte çalışılmışsa tarih aralıkları birleştirilir.

#### İşveren yönü — fazla tecrübe tam puan

```
Aralıkta (min ≤ aday ≤ max)   → 1.0
Aralığın ÜSTÜNDE              → 1.0     ← tam puan
Aralığın ALTINDA              → aday_yıl / min_yıl
Hiç tecrübesiz (0 yıl)        → 0.0
İlan aralık yazmamış          → 0.5     (nötr)
```

> **Neden üstünde tam puan:** İşveren için fazla nitelikli aday bir fırsat. Maaşta anlaşamazlarsa sohbette çözülür; sistem baştan uzaklaştırmasın.

**Örnekler — ilan 3-6 yıl istiyor:**

```
Aday  4 yıl  → 1.0         aralıkta
Aday  6 yıl  → 1.0         aralıkta
Aday  9 yıl  → 1.0         üstünde, tam puan
Aday  2 yıl  → 2/3 = 0.67
Aday  1 yıl  → 1/3 = 0.33
Aday  0 yıl  → 0.0
```

#### Aday yönü — iki uçta da tam puan alamaz

```
Aralıkta                       → 1.0

Aralığın ALTINDA               → 0.7 + 0.3 × (aday_yıl / min_yıl)

Aralığın ÜSTÜNDE:
  max'ı 3 yıla kadar aşıyor    → 0.7
  max'ı 3-6 yıl aşıyor         → 0.4
  max'ı 6+ yıl aşıyor          → 0.2
```

> **"Ceza" değil, "destek".** Hiçbir yerde çıkarma işlemi yok. Her bileşen 0-1 arası puan üretir, ağırlıkla çarpılıp toplanır. Düşük puan sadece "tam puan alamıyor" demek.

**`0.7` taban neden var:**

Aralığın altındaki ilan, aday için **gelişim fırsatı** — biraz zorlayıcı ama ulaşılabilir. Aday bunu görmek ister.

Taban olmasaydı:
```
İlan 3-6 yıl, aday 1 yıl → 1/3 = 0.33  → listenin dibine düşer
```

Aday sürekli kendi seviyesindeki ilanları görür, hiç yukarı bakamaz. Kariyer uygulamasında kötü bir davranış.

Taban ile:
```
0.7  → taban (aralığın altında bile olsa listede kalır)
0.3  → aralığa yaklaştıkça kazanılan ek puan

Aday 1 yıl → 0.7 + 0.3 × (1/3) = 0.80
Aday 2 yıl → 0.7 + 0.3 × (2/3) = 0.90
Aday 3 yıl → 1.0  (aralığa girdi)
```

**Üstünde neden kademeli düşüş:** Geriye adım kimsenin istediği şey değil. Bir kademe (0.7) tolere edilebilir — belki sektör değiştiriyordur. Üç kademe (0.2) açıkça yanlış eşleşme.

**Örnekler — ilan 3-6 yıl istiyor:**

```
Aday  4 yıl  → 1.0                       aralıkta
Aday  2 yıl  → 0.7 + 0.3 × (2/3) = 0.90  biraz altında
Aday  1 yıl  → 0.7 + 0.3 × (1/3) = 0.80  altında
Aday  8 yıl  → 0.7                       max+2, bir kademe
Aday 11 yıl  → 0.4                       max+5, iki kademe
Aday 15 yıl  → 0.2                       max+9, üç kademe
```

#### İki yön yan yana — ilan 1-2 yıl (junior)

| Aday | İşveren yönü | Aday yönü | Yorum |
|---|:---:|:---:|---|
| 0 yıl | 0.0 | 0.70 | İşveren istemiyor; aday öğrenmek istiyor |
| 1 yıl | 1.0 | 1.0 | İkisi de memnun |
| 2 yıl | 1.0 | 1.0 | İkisi de memnun |
| 4 yıl | 1.0 | 0.7 | İşveren sevinir; aday kararsız |
| 8 yıl | 1.0 | 0.2 | İşveren çok ister; aday hiç istemez |

**Son satır:** İşverenin listesinde 3. sırada, adayın listesinde 47. sırada. **İkisi de doğru.**

#### Toplam skora etkisi

```
skor_aday = 0.65 × anlamsal  (0.87) → 0.566
          + 0.20 × yetenek   (0.75) → 0.150
          + 0.10 × tecrube   (0.90) → 0.090
          + 0.05 × kalite    (0.80) → 0.040
          = 0.846

Tecrübe tam puan olsaydı: 0.856
Fark: 0.01 → 1 puanlık eksik kazanç
```

#### Staj ilanlarında

Staj ilanında tecrübe sorulmuyor. Bu bileşen **0.5 (nötr)** alır veya ağırlığı diğer bileşenlere dağıtılır.

---

### 4.4 `kalite` — %5, iki yönde TAMAMEN FARKLI şeye bakıyor

#### İşveren yönü = adayın profil doluluğu

| Kriter | Puan |
|---|:---:|
| Deneyim kaydı var, "ne yaptın" dolu | 0.25 |
| En az 5 yetenek | 0.20 |
| Kısa tanıtım yazılmış | 0.15 |
| Eğitim kaydı tam (bölüm dolu) | 0.15 |
| Proje veya sertifika var | 0.10 |
| Fotoğraf var | 0.10 |
| CV yüklenmiş | 0.05 |
| **Toplam** | **1.00** |

#### Aday yönü = ilanın ve şirketin doluluğu

| Kriter | Puan |
|---|:---:|
| İş tanımının 4 sorusu dolu | 0.30 |
| Maaş aralığı belirtilmiş | 0.20 |
| Şirket doğrulanmış (rozetli) | 0.15 |
| Şirket profili tam | 0.15 |
| Yan haklar girilmiş | 0.10 |
| Şirket fotoğrafı var | 0.10 |
| **Toplam** | **1.00** |

#### Örnek — dolu profil vs eşiği zor geçen profil

**Aday A:**

| Kriter | Durum | Puan |
|---|---|:---:|
| Deneyim | ✅ 3 kayıt | 0.25 |
| 5+ yetenek | ✅ 12 yetenek | 0.20 |
| Tanıtım | ✅ | 0.15 |
| Eğitim | ✅ | 0.15 |
| Proje/sertifika | ✅ 2 proje | 0.10 |
| Fotoğraf | ✅ | 0.10 |
| CV | ❌ | 0.00 |
| | **Toplam** | **0.95** |

**Aday B:**

| Kriter | Durum | Puan |
|---|---|:---:|
| Deneyim | ❌ | 0.00 |
| 5+ yetenek | ❌ 2 yetenek | 0.00 |
| Tanıtım | ❌ | 0.00 |
| Eğitim | ✅ | 0.15 |
| Proje/sertifika | ❌ | 0.00 |
| Fotoğraf | ✅ | 0.10 |
| CV | ❌ | 0.00 |
| | **Toplam** | **0.25** |

```
A: 0.05 × 0.95 = 0.0475
B: 0.05 × 0.25 = 0.0125
Fark: 3.5 puan (100 üzerinden)
```

#### Ağırlık neden sadece %5

Daha yüksek olsaydı (%20 gibi), profili çok dolu ama işe hiç uygun olmayan aday, profili eksik ama mükemmel uyan adayın önüne geçerdi. Asıl iş **uygunluğu** ölçmek.

%5 sadece **eşitliği bozan** faktör: iki aday benzer uygunluktaysa, profili dolu olan üstte çıkar.

#### Asıl işlevi teşvik

Bu bileşenin gerçek amacı sıralama değil, **davranış değiştirmek**:
- Adaya: *"Profilini doldurursan daha çok görünürsün"* — ve bu doğru oluyor
- İşverene: *"Maaş yazar, iş tanımını doldurursan ilanın üstte çıkar"*

Aday profilindeki "Eşleşme gücü" göstergesi bunu görsel olarak yansıtır.

---

### 4.5 Parametre tablosu — kod değişmeden ayar

**Tüm ağırlıklar ve formül içindeki sabitler veritabanında.** Gerçek veriyle mutlaka oynanacak; her değişiklik için sürüm çıkarmak istemezsiniz.

```sql
CREATE TABLE skor_parametreleri (
  yon        text NOT NULL,   -- 'isveren' | 'aday'
  bilesen    text NOT NULL,   -- 'anlamsal' | 'yetenek' | 'tecrube' | 'kalite'
  agirlik    real NOT NULL,
  ayarlar    jsonb,
  guncelleme timestamptz DEFAULT now(),
  PRIMARY KEY (yon, bilesen)
);
```

**Başlangıç değerleri:**

```sql
INSERT INTO skor_parametreleri VALUES
-- İŞVEREN YÖNÜ
('isveren','anlamsal', 0.65, NULL),
('isveren','yetenek',  0.20, '{"zorunlu_agirlik":0.75, "kismi_taban":0.5,
                               "yetenek_yok_notr":0.5}'),
('isveren','tecrube',  0.10, '{"ustunde_tam_puan":true,
                               "aralik_yok_notr":0.5}'),
('isveren','kalite',   0.05, '{"deneyim":0.25, "yetenek_5":0.20,
                               "tanitim":0.15, "egitim":0.15,
                               "proje_sertifika":0.10, "foto":0.10,
                               "cv":0.05}'),

-- ADAY YÖNÜ
('aday','anlamsal', 0.65, NULL),
('aday','yetenek',  0.20, '{"zorunlu_agirlik":0.75, "kismi_taban":0.5,
                            "yetenek_yok_notr":0.5}'),
('aday','tecrube',  0.10, '{"altinda_taban":0.7,
                            "kademe1_sinir":3, "kademe1_puan":0.7,
                            "kademe2_sinir":6, "kademe2_puan":0.4,
                            "kademe3_puan":0.2}'),
('aday','kalite',   0.05, '{"is_tanimi":0.30, "maas":0.20,
                            "dogrulama":0.15, "sirket_profili":0.15,
                            "yan_haklar":0.10, "foto":0.10}');
```

> "0.7 tabanı" veya "3 yıl kademe sınırı" gibi değerleri değiştirmek için **tek satır `UPDATE`** yeterli.

**Üç güvenlik önlemi:**

| Önlem | Neden |
|---|---|
| **Toplam 1.0 kontrolü** | Yanlışlıkla toplam ≠ 1.0 olursa skorlar bozulur. `CHECK` kısıtı veya hesapta normalizasyon |
| **Değişiklik günlüğü** | `skor_parametreleri_log` — kim, ne zaman, neyi değiştirdi. Sonuçlar bozulursa geri dönülebilsin |
| **Değişiklik sonrası yeniden hesaplama** | Mevcut satırlar eski ağırlıklarla hesaplanmış durumda. Periyodik tazeleme yavaş yavaş düzeltir; büyük değişiklikte toplu tetikleme |

### 4.6 `alt_skorlar` — saklanması zorunlu

Her satırda her bileşenin ayrı değeri saklanır:

```json
{
  "isveren": {"anlamsal": 0.87, "yetenek": 0.625, "tecrube": 1.0, "kalite": 0.75},
  "aday":    {"anlamsal": 0.87, "yetenek": 0.625, "tecrube": 0.2, "kalite": 0.60}
}
```

**İki sebep:**

1. **Ayarlama:** Hangi bileşen yanlış çalışıyor? Alt skorlar olmadan göremezsiniz.
2. **Düzenleyici uyum:** Otomatik karar verme düzenlemelerinde (KVKK, GDPR md. 22, AB Yapay Zeka Yasası — işe alım "yüksek riskli" sınıfta, NYC Local Law 144) **"neden bu sıralama"** sorusuna cevap verebilmek gerekiyor.

> 💡 **Ürün fırsatı:** Alt skorlar kartta gösterilebilir — *"Yetenek uyumu yüksek, tecrübe aralığın biraz altında."* LLM olmadan açıklanabilir eşleşme.

---

## 5. Sonuç Tablosu

Hesaplanan skorlar buraya yazılır. Kullanıcı Keşfet'i açtığında **hesap yapılmaz**, buradan okunur.

### İki tablo — biri geçici, biri kalıcı

#### `ilan_aday_eslestirme` — büyük, geçici, önbellek

```sql
CREATE TABLE ilan_aday_eslestirme (
  ilan_id            bigint NOT NULL,
  aday_id            bigint NOT NULL,
  skor_isveren       real NOT NULL,
  skor_aday          real NOT NULL,
  alt_skorlar        jsonb,
  model_adi          text,
  hesaplanma_tarihi  timestamptz NOT NULL DEFAULT now(),
  gosterildi_isveren boolean NOT NULL DEFAULT false,
  gosterildi_aday    boolean NOT NULL DEFAULT false,
  PRIMARY KEY (ilan_id, aday_id)
);
```

#### `ilan_aday_aksiyon` — küçük, kalıcı

```sql
CREATE TABLE ilan_aday_aksiyon (
  ilan_id bigint NOT NULL,
  aday_id bigint NOT NULL,
  aksiyon text NOT NULL,   -- gecildi | basvuruldu | reddedildi |
                           -- davet_edildi | eslesti
  taraf   text NOT NULL,   -- aday | isveren
  tarih   timestamptz NOT NULL DEFAULT now(),
  PRIMARY KEY (ilan_id, aday_id, aksiyon, taraf)
);
```

> **Neden ayrı:** Eşleştirme tablosu sık temizleniyor. Aksiyon tablosu dokunulmuyor — "daha önce geçilmiş kart" bilgisi kaybolmamalı. Ayrıca bu ayrım sonradan yapılması zor bir iş; **baştan ayrı kurulmalı.**

### Tek tablo, iki yön

```sql
-- İşveren Keşfet
WHERE ilan_id = :x ORDER BY skor_isveren DESC LIMIT 50

-- Aday Keşfet
WHERE aday_id = :y ORDER BY skor_aday DESC LIMIT 50
```

**Tek tablonun en büyük faydası:**

```
Pazartesi:  İlan #5 yayınlandı → o anki adaylar için hesaplandı

Salı:       Yeni Aday #999 kaydoldu
            → Aday #999 için hesap çalıştı
            → İlan #5 uygun çıktı → (5, 999) satırı yazıldı
                                     ↑ aynı tabloya, İKİ skorla

Salı akşamı: İşveren İlan #5'in Keşfet'ini açtı
            → WHERE ilan_id = 5
            → Aday #999 da listede ✅
```

İşverenin ilanı hiç güncellenmedi, hiçbir şey yapmadı. Satır iki yönden okunabildiği için yeni aday onun listesinde de görünüyor.

> ⚠️ **İki ayrı tablo olsaydı** (`ilan_bazli`, `aday_bazli`): Yeni aday sadece kendi tablosuna yazılır, işverenin tablosunda görünmezdi. Sürekli tazeleme gerekirdi. Tek tablo bu sorunu **tasarımdan** çözüyor.

### Yazma — UPSERT

```sql
INSERT INTO ilan_aday_eslestirme (...)
VALUES (...)
ON CONFLICT (ilan_id, aday_id) DO UPDATE SET
  skor_isveren      = EXCLUDED.skor_isveren,
  skor_aday         = EXCLUDED.skor_aday,
  alt_skorlar       = EXCLUDED.alt_skorlar,
  model_adi         = EXCLUDED.model_adi,
  hesaplanma_tarihi = now();
```

`gosterildi_*` kolonları **korunur** — yeniden hesaplama "daha önce gösterildi" bilgisini silmemeli.

> **İki gösterildi kolonu neden ayrı:** Aynı çift işverene gösterilmiş ama adaya henüz gösterilmemiş olabilir.

### İndeksler

```sql
CREATE INDEX ON ilan_aday_eslestirme (ilan_id, skor_isveren DESC);
CREATE INDEX ON ilan_aday_eslestirme (aday_id, skor_aday DESC);
CREATE INDEX ON ilan_aday_eslestirme (hesaplanma_tarihi);

CREATE INDEX ON ilan_aday_aksiyon (aday_id, taraf);
CREATE INDEX ON ilan_aday_aksiyon (ilan_id, taraf);
```

### Silme kuralları

| Olay | Eşleştirme satırları | Aksiyon satırları | Sohbetler |
|---|:---:|:---:|:---:|
| İlan kapandı / doldu / süresi doldu | **Silinir** | Kalır | Devam eder |
| Aday "bakmıyorum" dedi | **Silinir** | Kalır | Devam eder |
| 14 günden eski hesap | **Silinir** | Kalır | — |
| Aday hesabını sildi | Silinir | **Silinir** (KVKK/GDPR) | Anonimleşir |

> **Sohbetler neden devam eder:** Aday ile işveren konuşurken ilan kapanırsa konuşma yarıda kalmasın. Belki başka pozisyon için konuşuyorlar.

### İlan yeniden yayınlandığında

```
durum: suresi_doldu → yayinda
ilan_id AYNI kalır
```

| Konu | Davranış |
|---|---|
| Embedding | **Yeniden üretilmez** — metin aynıysa hash aynı |
| Eşleştirme satırları | Kapanınca silinmişti → **yeniden hesaplanır** |
| Aksiyon geçmişi | **Korunur** — geçilen, reddedilen bir daha görünmez |
| Önceki bekleyen başvurular | İşverene sorulur: *"12 bekleyen başvurunuz vardı. Geri getirilsin mi?"* |

> **Neden aynı `ilan_id`:** Yeni kayıt açılsaydı geçmiş kaybolurdu — aynı aday aynı ilanı tekrar görür, reddedilen tekrar başvurur.

> **Meslek alanı değişirse:** "Yeni ilan olarak aç" yönlendirmesi. Yoksa eski geçmiş farklı bir pozisyona yanlış uygulanır.

### Bölümlendirme

**Karar:** `PARTITION BY HASH (ilan_id)`, **16 bölüm**

```sql
CREATE TABLE ilan_aday_eslestirme (...) PARTITION BY HASH (ilan_id);

CREATE TABLE ilan_aday_eslestirme_p00 PARTITION OF ilan_aday_eslestirme
  FOR VALUES WITH (MODULUS 16, REMAINDER 0);
-- ... p15'e kadar
```

**Bölüm sayısı sabit.** Yeni ilan yeni bölüm yaratmaz — `hash(ilan_id) % 16` ile mevcut 16 bölümden birine düşer. Bakım gerektirmez.

**Değerlendirilen ve reddedilen alternatifler:**

| Seçenek | Neden olmadı |
|---|---|
| **Ülke bazlı (LIST)** | **Uzaktan çalışma bozuyor.** ABD'deki aday, Türkiye'deki uzaktan ilanla eşleşiyor — satır hangi bölüme gidecek? Ülke bölümüne konursa diğer taraf göremez. |
| **İlan durumu** | **Durum değişince satır fiziksel olarak taşınır** — ağır iş. Üstelik o satırlar zaten silinecekti. Ayrıca durum eşleştirme tablosunda değil, kopyalanması gerekir → tutarlılık riski. |
| **Tarih (RANGE)** | Temizlik için ideal (`DROP PARTITION`) ama yazma tek bölüme yığılıyor. |
| **İki tablo, veriyi çoğaltma** | Çok yüksek ölçekte profesyonel çözüm (her yön kendi okuma desenine optimize). Ama tutarlılık riski var; şimdilik gereksiz. |

> **Genel kural:** Bölüm anahtarı **değişmeyen** bir şey olmalı. `ilan_id` hiç değişmez. Durum gibi değişken alanlar **indeksle** yönetilir.

**Okuma davranışı:**
- İşveren sorgusu (`ilan_id = ?`) → **tek bölüme** gider
- Aday sorgusu (`aday_id = ?`) → 16 bölümü tarar; indeksli olduğu için hızlı

> 🚫 **Şimdi yapılmayacak.** Profesyonel sıralama: önce indeksler → sonra tablo ayırma (yapıldı) → sonra veri ömrü kısaltma (yapıldı) → **en son bölümlendirme**. Genelde ~100 milyon satırda gerekli hale gelir.
>
> **Şimdi yapılacak olan:** Şemayı uygun kurmak — `PRIMARY KEY (ilan_id, aday_id)` (bölüm anahtarı PK'nın parçası olmalı) ve aksiyon tablosu ayrı.

**Bölümlendirmenin gizli maliyetleri** *(ekip bilmeli):*
- Her sorgu daha yavaş planlanıyor
- Bölüm anahtarı PK'nın parçası olmak zorunda
- Yabancı anahtar referansları sınırlı
- Bölüm sayısını değiştirmek = veri taşıma

---

## 6. Tetikleyiciler ve Periyodik İşler

### Karma hesaplama modeli

> **Sorun:** İş bulan, aylarca girmeyen binlerce kullanıcı için her yeni ilanda hesap yapıp tablo dolduruyorduk. Kimse bakmıyordu — saf israf.
>
> **Ama tamamen "açınca hesapla" da olmaz:** "Yeni ilan gelince haber ver" özelliği, kullanıcı uygulamayı açmadan eşleşme olduğunu bilmeyi gerektiriyor.

**Çözüm — aktif önceden, pasif açınca:**

```
Vektör                      → her zaman hazır (metin değişince güncellenir)

Eşleşme tablosu:
  Son 14 günde giriş yapmış → ÖNCEDEN hesaplanır (liste anında hazır)
  14 günden uzun girmemiş   → hesaplanmaz
                              Keşfet açınca ANLIK hesaplanır (~200 ms)
```

**Neden 14 gün:** Aktif iş arayan birkaç günde bir giriyor. Onlara anında dolu liste sunmak deneyimin kalbi. Pasifler için 200 ms beklemek sorun değil — zaten haftalardır gelmemişler.

**Yük farkı:**

```
Eski: 500.000 aday × her yeni ilan       → hepsi hesaplanıyor
Yeni: ~75.000 aktif aday × her yeni ilan → %85 azalma
      + pasifler sadece girdiklerinde
```

### Anlık tetikleyiciler — asenkron

**Kullanıcı hiç bekletilmez.** İş kuyruğa girer, arka planda çalışır.

| Olay | Ne hesaplanır |
|---|---|
| İlan yayınlandı / güncellendi / yeniden yayınlandı | O ilan × **aktif** uygun adaylar |
| Aday profili kaydedildi / güncellendi | O aday × uygun ilanlar |
| Pasif kullanıcı Keşfet'i açtı | Anlık hesap |
| Önbellek 24 saatten eski + Keşfet açıldı | Yeniden hesap |

**Her tetiklemede iki skor birden yazılır.** Hangi taraf tetiklerse diğerinin listesine de girer.

### Durum kolonları

```sql
ilanlar.eslestirme_durumu          -- beklemede | hesaplaniyor | hazir | hata
ilanlar.son_eslestirme_tarihi

aday_profilleri.eslestirme_durumu
aday_profilleri.son_eslestirme_tarihi
```

**Keşfet açıldığında:**

| Durum | Ekran |
|---|---|
| `hazir` + 24 saatten taze | Tablodan oku, kartları göster |
| `hesaplaniyor` | *"Hazırlanıyor..."* — bitince otomatik dolar |
| Eski veya hiç yok | Hesapla, sonra göster |
| `hata` | Tekrar dene |

> **Neden bu kolonlar zorunlu:** Olmazsa "sonuç yok" ile "henüz hesaplanmadı" ayırt edilemez. Aday boş ekran görür ve neden olduğunu kimse bilmez.

**Süre beklentisi:** Embedding API çağrısı 1-2 saniye, eşleştirme hesabı milisaniyeler. Kullanıcı "hazırlanıyor" ekranını çoğu zaman hiç görmez.

### Periyodik bakım işleri — `pg_cron`

> **Önemli:** Bunlar eşleştirme için değil. Eşleştirme anlık, tetikleyicilerle. Periyodik işler sadece **bakım ve güvenlik ağı**.

| İş | Sıklık | Ne yapıyor |
|---|---|---|
| **Hata tekrarı** | 15 dakika | `eslestirme_durumu = 'hata'` olanları tekrar dener |
| **Süre kontrolleri** | Saatlik | Süresi dolan ilanları kapatır; cevapsız davetleri ve başvuruları düşürür |
| **Temizlik + sırayla tazeleme** | Günlük | 14 günden eski satırları siler; en bayat aktif ilanları tazeler |
| **Hareketsiz profiller** | Haftalık | 30-45 gün girmemiş "aktif arıyorum" → "fırsatlara açığım" |

```sql
SELECT cron.schedule('hata-tekrar',   '*/15 * * * *', $$ ... $$);
SELECT cron.schedule('sure-kontrol',  '0 * * * *',    $$ ... $$);
SELECT cron.schedule('gunluk-bakim',  '0 3 * * *',    $$ ... $$);
SELECT cron.schedule('hareketsiz',    '0 4 * * 1',    $$ ... $$);
```

**Sırayla tazeleme neden gerekli:** Hesapta limit 200'dü. 201. aday hiç yazılmadı. Havuz değiştikçe o aday artık ilk 200'e girebilir.

```sql
-- Her gün en bayat N aktif ilanı tazele
SELECT id FROM ilanlar
WHERE durum = 'yayinda'
ORDER BY son_eslestirme_tarihi ASC NULLS FIRST
LIMIT 10000;
```

> ⚠️ **Global ölçekte "gece" yok.** Dünyanın bir yerinde her zaman gündüz. Ağır işler UTC'de en düşük trafik saatinde ve **parça parça** çalışmalı — tek seferde bitmek zorunda değil.

### Kartlar tükendiğinde

**Aday tarafı:**

```
Şu an sana uygun tüm ilanları gördün.

[Yeni ilan gelince haber ver]  ← varsayılan açık

Profilini güçlendirirsen daha çok ilanla eşleşirsin:
+ Yetenek ekle
+ Deneyim ekle
```

**İşveren tarafı (simetrik):**

```
Bu ilana uygun tüm adayları gördün.

[Yeni aday gelince haber ver]  ← varsayılan açık

İlanını güçlendirirsen daha çok adayla eşleşirsin:
+ Maaş aralığı ekle
+ İş tanımını detaylandır
```

**Neden bu tasarım:**
- **Hiç ek sorgu gerektirmiyor** — elinizdeki profil tamamlanma verisini gösteriyor
- **Doğru şeye yönlendiriyor** — filtre gevşetmek yerine profili iyileştirmek

> **Kart tükenmesi bir hata değil, doğal bir son.** Aday tüm uygun ilanları gördü demek. Sorun kartların bitmesi değil, havuzun küçük olması — çözümü arayüzde değil, daha çok işveren toplamakta.

**Değerlendirilen ve ertelenen:**

> ❌ **Filtre gevşetme önerisi** *("uzaktan da açarsan 12 ilan daha var")*
>
> **Neden ertelendi:** Her filtreyi tek tek kaldırıp sorguyu yeniden çalıştırmak gerekir — 6 filtre = 6 ek sorgu, her boş ekranda. Ayrıca gevşetince gelen ilanların kalitesi düşük olabilir; aday heveslenip gevşetir, alakasız kartlar görür, güveni gider.
>
> **İleride yapılırsa:** Sadece **konum** için, tek sorgu: *"Başka şehirleri de açarsan 34 ilan daha var."* Diğer filtreler için uğraşılmaz.

### Bildirim kuyruğu

**Neden doğrudan göndermiyoruz:**
1. Gün içinde 6 yeni ilan hesaplanırsa 6 bildirim gider → kullanıcı bildirimleri kapatır
2. FCM çağrısı başarısız olursa bildirim kaybolur

```sql
CREATE TABLE bildirim_kuyrugu (
  id            bigserial PRIMARY KEY,
  kullanici_id  bigint NOT NULL,
  tip           text NOT NULL,   -- yeni_ilan | yeni_aday | davet |
                                 -- basvuru | eslesme | mesaj
  veri          jsonb,
  durum         text DEFAULT 'bekliyor',  -- bekliyor | gonderildi | hata
  olusturma     timestamptz DEFAULT now(),
  gonderim      timestamptz,
  deneme_sayisi int DEFAULT 0
);

CREATE INDEX ON bildirim_kuyrugu (durum, tip, kullanici_id);
```

**İki gönderim politikası:**

| Tip | Politika | Neden |
|---|---|---|
| Eşleşme, davet, başvuru, mesaj | **Anlık** (her dakika işlenir) | Kritik — gecikmemeli |
| Yeni uygun ilan / aday | **Günlük özet** | Bildirim yorgunluğunu önler |

**Günlük özet:**

```sql
SELECT kullanici_id, count(*)
FROM bildirim_kuyrugu
WHERE durum = 'bekliyor' AND tip = 'yeni_ilan'
GROUP BY kullanici_id;
-- → "Sana uygun 4 yeni ilan var"
```

**Kullanıcının saat dilimine göre sabah** gönderilir. Özet işi saatlik çalışır ve "şu an saat diliminde sabah 9 olan kullanıcılar" için gönderir.

**Sadece "haber ver" açık ve aktif kullanıcılar için hesaplanır.** Bu grup tüm adayların küçük bir kısmı — karma modelle uyumlu.

---

## 7. İlan Durumu

```sql
ilanlar.durum  -- taslak | yayinda | duraklatildi |
               -- dolduruldu | kapatildi | suresi_doldu
```

| Durum | Anlamı | Eşleştirmede |
|---|---|:---:|
| `taslak` | Henüz yayınlanmamış | ❌ |
| `yayinda` | Aktif | ✅ |
| `duraklatildi` | İşveren geçici durdurdu | ❌ |
| `dolduruldu` | Pozisyon doldu | ❌ |
| `kapatildi` | İşveren vazgeçti | ❌ |
| `suresi_doldu` | Yayın süresi bitti | ❌ |

> **"Dolduruldu" ile "kapatıldı" neden ayrı:** Dolduruldu **başarı göstergesi** — kuzey yıldızı metriğiniz (işe alım oranı) buradan besleniyor. Vazgeçme farklı bir sinyal: ilan kalitesi veya aday havuzu sorunu olabilir.

> 💡 **Duraklat özelliği:** *"Çok başvuru geldi, biraz durdur."* Kapatmaktan farkı: tekrar açıldığında ilan sıfırdan kurulmaz. Kotayı ve aday akışını yönetmek için değerli.

**Ek kolonlar:**

```sql
ilanlar (
  ...
  kapanma_tarihi    timestamptz,
  kapanma_sebebi    text,
  ilk_yayin_tarihi  timestamptz,
  yayin_bitis       timestamptz,
  yayin_sayisi      int DEFAULT 1
)
```

`ilk_yayin_tarihi` adaya gösterilebilir — *"Bu ilan 4 aydır açık"* bilgisi aday için değerli bir sinyal.

**Arayüz:**

```
Senior Backend Developer          ⏱ Süresi doldu
12 başvuru · 3 eşleşme
[Yeniden yayınla]  [Düzenle ve yayınla]  [Kapat]
```

Süre dolmadan 1 gün önceki hatırlatmada doğrudan **[Uzat]** butonu — uzatma, süre dolup yeniden yayınlamaktan daha temiz: hiçbir şey silinmez, kesinti olmaz.

---

## 8. Ölçek Yol Haritası

| Kullanıcı | Yapılacak | Neden |
|---|---|---|
| **0 – 100K** | Tek tablo, bölümsüz. İyi indeksler. `pg_cron` | Gereksiz karmaşıklık yok |
| **100K – 1M** | Hash bölümlendirme (16). `EXPLAIN ANALYZE` ile ayar | Satır sayısı 100M'a yaklaşıyor |
| **1M+** | `pgmq` veya harici kuyruk. İşçi havuzu. Kendi embedding servisi | `pg_cron` ölçeklenmez; API maliyeti yüksek |
| **Çok yüksek** | İki yön ayrı tablo (veri çoğaltma). Bölgesel veritabanları | Okuma desenine göre optimizasyon; veri yerleşimi yasaları |

> **Profesyonel ilke:** Tahmin üzerine mimari kurmayın. Her aşamada gerçek veriyle darboğazı görüp o zaman çözün. Tek kişilik bir projede en pahalı hata, hiç kullanıcıya ulaşmadan ölçek altyapısı kurmaktır.

**Ama şemayı baştan doğru kurun** — bunlar sonradan pahalı:

| Şimdi yapılacak | Neden |
|---|---|
| `PRIMARY KEY (ilan_id, aday_id)` | Bölümlendirmeye hazır |
| Aksiyon tablosu ayrı | Sonradan ayırmak zor |
| `model_adi`, `boyut` kolonları | Model değişimi |
| `ulke_kodu` kolonları | Filtre ve istatistik |
| Parametre tablosu | Ayarlama |
| `alt_skorlar` | Düzenleyici uyum |

---

## 9. Açık Sorular — Ekip Tartışması İçin

| # | Soru | Öneri |
|:---:|---|---|
| 1 | Embedding sağlayıcısı: OpenAI mı Cohere mı? | 20-30 gerçek profille test edip karar verin |
| 2 | Skor ağırlıkları doğru mu? | İlk tahmin. 20-30 gerçek eşleşmeye gözle bakıp ayarlayın |
| 3 | Aktif/pasif eşiği 14 gün doğru mu? | Kullanıcı davranışı verisiyle ayarlanacak |
| 4 | Önbellek tazeliği 24 saat yeterli mi? | Yoğun pazarlarda 12 saate inebilir |
| 5 | Limit 200 doğru mu? | Kullanıcılar ortalama kaç kart kaydırıyor? 100'e inebilir |
| 6 | Staj ilanında tecrübe bileşeni nasıl? | 0.5 nötr mü, ağırlık dağıtımı mı? |
| 7 | Alt skorlar kartta gösterilsin mi? | Ürün kararı — açıklanabilirlik vs kart sadeliği |
| 8 | Geri alma butonu ("son geçtiğimi geri getir")? | Tek adımlık geri alma önerilir |
| 9 | Geçme kalıcı mı? | Önerilen: kalıcı; ilan ciddi güncellenirse yeniden gösterilebilir |
| 10 | Yumuşak eşik ("düşük uyum" etiketi) kullanılsın mı? | İlk sürümde hayır, veriye bakıp karar |

---

## 10. İlk Sürümden Önce Mutlaka Yapılacak Doğrulama

> ⚠️ **Bu adım atlanırsa sonraki 3 ay boşa gidebilir.**

**Arayüz yazmadan önce:**

1. 20-30 gerçek aday profili ve 5-10 gerçek ilan **elle** Supabase'e girin
2. Eşleştirme fonksiyonunu çalıştırın
3. Sonuçları gerçek kişilere gösterin — ekran görüntüsüyle:
   - Adaya: *"Bu ilanlar senin için uygun mu?"*
   - İşverene: *"Bu adaylar ilanına uygun mu?"*
4. Cevap zayıfsa: ağırlıkları, formülleri, embedding modelini değiştirin

**Neden bu kadar önemli:** Eşleştirme kalitesi ürünün tamamını belirliyor. Arayüzü yazdıktan sonra "eşleştirme kötü çalışıyor" anlaşılırsa, üstüne kurulan her şey sorgulanır. Bu test yarım günlük iş, kazandırdığı süre aylar.

---

## 11. Sırada

| Bölüm | Durum |
|---|---|
| Giriş / kayıt | ✅ |
| Şirket doğrulama | ✅ |
| Şirket profili | ✅ |
| İlan oluşturma | ✅ |
| Aday profili | ✅ |
| **Eşleştirme motoru** | ✅ |
| **Keşfet, başvuru, davet, eşleşme, sohbet** | ⏭️ Sırada |
| Bildirimler (detay) | Bekliyor |
| Admin paneli | Bekliyor |
| Veri koruma (KVKK / GDPR / diğer) | Bekliyor |

**Keşfet bölümünde konuşulacaklar:** 4 sekme yapısı (iki taraf simetrik), kart içerikleri, başvuru onay diyaloğu, davet akışı, eşleşme anı, sohbet, geri alma.
