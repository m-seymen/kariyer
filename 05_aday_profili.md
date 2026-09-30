# İş Eşleştirme Projesi — Aday Profili

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak
> Durum: Kararlar tamamlandı, ekip tartışmasına hazır
> İlgili dokümanlar: `giris-kayit-kararlari.md`, `sirket-dogrulama-ekrani.md`, `sirket-profili.md`, `ilan-olusturma.md`

---

## 0. Bu Sürümde Düzeltilenler

| Konu | Eski | Yeni |
|---|---|---|
| Konum | Yaşadığı il/ilçe, çalışmak istediği şehirler | **`konumlar` ağacı** — ülke → eyalet/il → şehir/ilçe |
| Konum filtresi | Şehir listesi eşleşmesi | **`konum_yolu LIKE`** — üst seviye seçilirse altı dahil |
| Eğitim dereceleri | 7 sabit kademe | **ISCED eşlemesi** — ülkeler arası karşılaştırma için |
| Üniversite listesi | YÖK (~200) | **Ülke bazlı** — her lansman ülkesi için ayrı |
| İş dili varsayılanı | "Türkçe işaretli gelsin" | **Kullanıcının ülkesinin ana dili** |
| Maaş beklentisi | `net_min_maas` (TL varsayılan) | **Para birimi alanı** eklendi |
| Öneri havuzları | Tek dil | **Dil bazlı süzme** (`dil_kodu`) |
| Sürücü belgesi sınıfları | B/C/D/E sabit | **Ülkeye göre** (`ulke_surucu_siniflari`) |

---

## 1. Bu Bölümün Stratejik Önemi

Aday profili bu üründe **tek veri kaynağıdır**. Bunu bir kez netleştirmek, aşağıdaki her kararın gerekçesini açıklıyor:

| Kaynak | Eşleştirmeye giriyor mu? |
|---|:---:|
| Şirket profili | ❌ |
| CV | ❌ |
| Yorumlar / referanslar | Yok |
| **Aday profili alanları** | **✅ tek kaynak** |

CV'yi eşleştirmeden çıkarma kararı (adil ve savunulabilir sıralama için) şu bedeli getirdi: **profil alanları zayıfsa eşleştirme zayıf.** Bu yüzden bu bölümde iki şeyi aynı anda optimize etmemiz gerekiyor:

1. **Veri zenginliği** — embedding'in besleneceği metin
2. **Tamamlama oranı** — 12 adımlı bir formu insanların gerçekten bitirmesi

Bu ikisi birbirine ters çalışır. Aşağıdaki kararların çoğu bu gerilimi yönetmek üzerine kurulu.

> **Global ölçekte üçüncü bir gerilim eklendi:** aynı formun 50 ülkede, 20 dilde, farklı eğitim ve belge sistemlerinde çalışması. Çözüm deseni her yerde aynı: **sabit değerleri tabloya taşı, koda gömme.**

---

## 2. Genel Tasarım İlkeleri

Bölüm boyunca tekrar eden dört desen:

### 2.1 Serbest yazma + öneri (kullanımdan öğrenen liste)

Yetenek, unvan, şirket adı, lise adı, sertifika, bölüm — hepsi böyle. Aday yazmaya başlar, daha önce yazılanlar kullanım sayısına göre sıralı düşer, seçer veya kendi yazar. Yeni kayıt hemen öneri havuzuna girer.

**Neden çalışıyor:** Embedding anlamsal yakınlığı yakalıyor, bu yüzden isim dağınıklığı ("ReactJS" / "React.js" / "Reakt") eşleştirmeyi bozmuyor.

> **Global ölçekte bu daha da değerli:** çok dilli embedding sayesinde "Buchhaltung" ile "muhasebe" aynı vektör uzayında yakın çıkıyor — çeviri tablosu gerekmiyor.

**Nerede çalışmıyor:** Katı filtreye giren alanlarda. Orada kapalı liste zorunlu.

```sql
-- Desen tüm öneri tabloları için aynı
<tablo> (id, ad, normalize_ad, dil_kodu, kaynak, kullanim_sayisi)
-- kaynak: resmi_liste | kullanici_girdi
```

`normalize_ad` (küçük harf, noktalama temizliği) + `pg_trgm` ile yazım hataları yakalanır.

> **`dil_kodu` neden gerekli:** Öneri havuzu kullanıcının diline göre süzülmeli. Türk adaya Japonca yetenek önerisi düşmemeli. **Ama eşleştirme dilden bağımsız çalışmaya devam eder** — dil ayrımı sadece öneri kalitesi için.

> ⚠️ **Kritik risk:** Öneri havuzları başlangıçta boş. İlk kullanıcılar herkes farklı yazar, veri dağılır ve sistem hiç kendine gelemez. **Başlangıç verisi girilmeden yayına çıkılamaz** — ve bu veri **her lansman dili için ayrı** hazırlanmalı.

### 2.2 Kapalı liste — sadece katı filtre alanlarında

| Alan | Neden kapalı |
|---|---|
| Meslek alanı | Kaba filtre; "Bilişim"/"IT"/"Yazılım" dağınıklığı filtreyi öldürür |
| **Konum (`konumlar` ağacı)** | Konum filtresi; ağaç yapısı kesin eşleşme gerektirir |
| Diller | Kesişim kontrolü |
| Üniversite | İşveren filtreleyebilir, standart gerekli |
| Eğitim derecesi | Seviye karşılaştırması (ISCED üzerinden) |

### 2.3 "+ Ekle" deseni

Eğitim, deneyim, yetenek, dil, sertifika, proje — hepsi liste. Sabit alan yok, "+ Ekle" → satır açılır → kaydet → listeye düşer.

**Neden:** Sabit alanlar hiç kimsenin durumuna tam oturmuyor. İki lisans yapan, üç sertifikası olan, hiç deneyimi olmayan — hepsi aynı yapıda çalışmalı.

> Global ölçekte bu desenin değeri artıyor: ülkelerin eğitim ve kariyer yapıları farklı, esnek liste hepsini kaldırıyor.

### 2.4 Zorlamak yerine göstermek

Zorunlu alan sayısı minimumda. Eksik alanın bedeli adaya **açıkça gösterilir**, ama engel konulmaz:

- Eşleşme gücü göstergesi
- Eksik filtre uyarısı
- Profil tamamlanma skorda %5 ağırlık

---

## 3. Adım Adım Alanlar

### Adım 1 — Temel Bilgiler

| Alan | Zorunlu | İşverene görünür | Katı filtre | Embedding |
|---|:---:|:---:|:---:|:---:|
| Fotoğraf | ❌ | ✅ | ❌ | ❌ |
| Ad, soyad | ✅ | ✅ | ❌ | ❌ |
| Doğum tarihi | ✅ | ❌ | ❌ | ❌ |
| Cinsiyet | ❌ | ❌ | ❌ | ❌ |
| Telefon | ✅ | Eşleşmeden sonra | ❌ | ❌ |
| E-posta | ✅ | Eşleşmeden sonra | ❌ | ❌ |
| **Yaşadığı konum** | ✅ | ✅ | ❌ | ❌ |
| **Çalışmak istediği konumlar** | ✅¹ | ✅ | **✅** | ❌ |
| Her konumda çalışmaya hazırım | ❌ | ✅ | Filtreyi atlatır | ❌ |
| Çalışma şekli tercihi | ✅ | ✅ | **✅** | ❌ |
| Kısa tanıtım | ❌ | ✅ | ❌ | **✅** |

¹ *"Her konumda çalışmaya hazırım" işaretliyse zorunlu değil*

**Fotoğraf** — yüklenmezse baş harflerden renkli avatar üretilir. Zorunlu değil: hem bazı kişiler koymak istemez, hem fotoğrafa göre seçim ayrımcılık riski taşır.

**Doğum tarihi** — zorunlu ama **işverene gösterilmez**. Sistemde 18 yaş kontrolü ve istatistik için durur. Kartta yaş yazması gereksiz eleme sebebi yaratır.

> **Global not:** Çalışma yaşı ülkeye göre değişiyor (çoğu yerde 15-18 bandı, bazı işlerde 21). **Öneri:** `ulke_calisma_yasi` tablosu — kayıt sırasında kullanıcının ülkesine göre kontrol edilsin.

**Cinsiyet** — opsiyonel, "belirtmek istemiyorum" seçenekli. İşverene gösterilmez, filtreye ve skora hiç girmez.

> 💬 **Ekip tartışması için:** Cinsiyet alanını hiç tutmama seçeneği de var. Tutmanın tek gerekçesi ileride demografik istatistik. Tutmamanın gerekçesi: veri yoksa sızma ve kötüye kullanım riski de yok. **Global ölçekte tutmamak daha da güçlü bir seçenek** — bazı ülkelerde bu veri özel nitelikli kişisel veri sayılıyor ve ek yükümlülük getiriyor.

**İletişim — üç kademeli görünürlük:**

| Aşama | Telefon / e-posta |
|---|:---:|
| Keşfet kartı | ❌ |
| Aday başvurduktan sonra | ❌ |
| **Eşleşmeden sonra** | ✅ |

> Bu kural olmazsa işveren Keşfet'te 200 adayın telefonunu toplar, uygulamayı hiç kullanmadan dışarıdan arar. **Platform dışına kaçış, bu tür pazar yerlerini öldüren bir numaralı sebeptir.**

**Telefon:** ülke kodu ayrı alan olarak alınsın, numaradan ayrılsın. Yoksa format doğrulaması ve sonraki aramalar bozulur.

#### Konum yapısı — global

**Sabit kademe (ülke → il → ilçe) kurulmaz.** Ülkelerin idari yapıları farklı, esnek ağaç kullanılır:

```sql
konumlar (
  id, ad, normalize_ad,
  ust_konum_id,        -- kendine referans
  tip,                 -- ulke | eyalet | bolge | il | ilce | sehir
  ulke_kodu,           -- TR, US, DE (en hızlı filtre)
  yol,                 -- 'US/California/San Francisco'
  seviye,              -- 0=ülke, 1, 2, 3
  aktif_mi
)
```

```
Türkiye (ulke)          ABD (ulke)                Singapur (ulke)
└── İstanbul (il)       └── California (eyalet)   ← tek kademe
    └── Kadıköy (ilce)      └── San Francisco (sehir)
```

**Aday tarafı iki alan:**

```sql
aday_profilleri (
  yasadigi_konum_id,       -- sadece gösterim
  yasadigi_ulke_kodu
)

aday_konumlari (           -- çoklu, katı filtre
  aday_id, konum_id, konum_yolu, ulke_kodu
)
```

**Arayüzde kademeli menü yok — tek arama kutusu.** Kullanıcı "San Fran" yazar, *"San Francisco, California, ABD"* çıkar, tıklar.

> Kademeli açılır menü 200 ülkede çalışmaz. Arama kutusu global ürünlerde standart çözüm.

**Etiketler dinamik:** İkinci seviyenin adı ülkeye göre değişir (Türkiye'de "İl", ABD'de "Eyalet", Fransa'da "Bölge"). Etiket `tip` alanından okunur.

**Aday istediği seviyede seçebilir:**

| Aday seçimi | Anlamı |
|---|---|
| `DE` | "Almanya'da çalışırım, şehir farketmez" |
| `US/California` | "Kaliforniya'da çalışırım" |
| `TR/İstanbul/Kadıköy` | "Sadece Kadıköy" |

**Filtre — `konum_yolu LIKE`:**

```sql
i.konum_yolu LIKE ak.konum_yolu || '%'
```

| Aday seçimi | İlan konumu | Eşleşir |
|---|---|:---:|
| `TR` | `TR/İstanbul/Kadıköy` | ✅ |
| `TR/İstanbul` | `TR/Ankara/Çankaya` | ❌ |
| `US/California` | `US/California/San Francisco` | ✅ |
| `US/California` | `US/Texas/Austin` | ❌ |
| `DE` | `DE/Bayern/München` | ✅ |

Üst seviye seçilirse alt seviyelerin tamamı dahil olur.

> **`ulke_kodu` neden ayrı kolon:** `LIKE 'TR%'` de çalışır ama `ulke_kodu = 'TR'` eşitlik karşılaştırması ve çok daha hızlı indeksleniyor. Global ölçekte en çok eleyen filtre bu — 5 milyonu 200 bine indiriyor.

**Veri kaynağı: Geonames** — ücretsiz, ülke/eyalet/şehir hiyerarşisi hazır, tüm dillerde isimler var. Nüfusu 15 bin üstü şehirler yeterli (~25 bin kayıt).

**"Her konumda çalışmaya hazırım"** — önceki "taşınabilirim" alanının yerine geldi.

> **Neden değişti:** "Taşınabilirim" ile "çalışmak istediğim konumlar" çelişiyordu. Aday hem "Ankara'da çalışırım" hem "taşınabilirim" derse hangisi geçerli belirsiz kalıyordu. Yeni ifade net: işaretlenirse konum listesi zorunluluğu kalkar ve filtre tamamen atlanır. Arayüzde de kutu işaretlenince konum alanı gri düşer.

> ⚠️ **Global ölçekte bir uyarı:** "Her konumda çalışırım" işaretleyen aday, çalışma izni olmayan ülkelerdeki ilanlara da düşer. Bu gerçek bir sürtünme kaynağı — açık kalanlar bölümündeki **çalışma izni / vize sponsorluğu** sorusu bu yüzden önemli.

**Kısa tanıtım** — 2-4 cümle, 300-500 karakter. **Embedding'e giren üç metinden biri** (diğerleri: deneyim açıklamaları, proje açıklamaları).

Yer tutucu örnek metin konulmalı *(arayüz diline göre çevrilir)*:

> *"5 yıldır e-ticaret alanında backend geliştiriyorum. Ödeme sistemleri ve yüksek trafikli servisler üzerine yoğunlaştım. Şu an mimari kararlara daha çok dahil olabileceğim bir ekipte çalışmak istiyorum."*

**Neden opsiyonel:** Zorunlu yapılırsa klişe yazılır ("çalışkan ve takım ruhuna sahip bir bireyim"). O metin embedding'e girdiğinde sinyal değil gürültü olur. Gönüllü yazan gerçek bir şey yazar.

---

### Adım 2 — Meslek Alanı, Unvan, Ne Arıyor

| Alan | Zorunlu | Giriş şekli | Filtre |
|---|:---:|---|:---:|
| Ana meslek alanı | ✅ | Kapalı liste | **Katı** |
| İkincil meslek alanı | ❌ | Kapalı liste | **Katı** |
| Unvan | ✅ | Serbest + öneri | ❌ (embedding) |
| Ne arıyor | ✅ | İş / staj / ikisi — çoklu | **Katı** |

**Terminoloji:** Şirket profilinde "Sektör", ilan ve adayda "Meslek Alanı" / "Aradığım Meslek Alanı".

> **Neden ayrı isim:** Sektör = şirket ne iş yapıyor (Gıda, İnşaat). Meslek alanı = kişi ne iş yapıyor (Bilişim, Muhasebe). Bir inşaat şirketi muhasebeci arayabilir — eşleşen şey meslek, sektör değil.

**Meslek alanı neden kapalı liste ve neden katı filtre:**

Embedding tek başına bunu güvenilir yapmıyor. "Proje yönetimi, ekip koordinasyonu, bütçe takibi" yazan bir inşaat yöneticisi ile aynı şeyleri yazan bir yazılım proje yöneticisi vektör uzayında çok yakın çıkar. Meslek alanı olmasa ikisi birbirine düşer.

Ayrıca performans: global havuzu ülke filtresinden sonra bir kademe daha daraltır.

```sql
meslek_alanlari (id, kod, sira, aktif_mi)
meslek_alani_cevirileri (meslek_alani_id, dil_kodu, ad)

aday_profilleri (
  ana_meslek_alani_id      NOT NULL,
  ikincil_meslek_alani_id  NULL
)
-- filtre: ilan.meslek_alani_id IN (ana, ikincil)
```

> **Global not:** Kod sabit (`bilisim`), gösterilen ad arayüz diline göre gelir. **Tek liste tüm ülkelerde kullanılır** — ülke bazlı meslek alanı listesi tutulmaz, yoksa aynı iş farklı ülkelerde farklı kategoriye düşer ve filtre bozulur.

**İkincil meslek alanı** — opsiyonel, en fazla bir tane. Arayüzde katlanmış dursun ki çoğu kullanıcı hiç dokunmasın.

**İki fayda:** (a) grafik tasarımcı hem Tasarım hem Pazarlama'ya bakabilir, kariyer değiştiren iki alanda arayabilir; (b) ana alan skorda daha ağır sayılır, unvan önerileri ana alandan gelir.

> 💬 **Ekip tartışması için:** Aday tarafında ilan sayısı adaydan az olacak. Tek meslek alanı Keşfet'i boşaltma riski taşıyor. İkincil alan bu riski azaltıyor ama filtre gücünü de bir miktar düşürüyor. İlk veriyle ölçüp ayarlanacak bir parametre.

**Unvan** — serbest + öneri, ana meslek alanına bağlı liste. Katı filtreye girmez.

> **Neden serbest:** "Yazılım Mühendisi" ile "Backend Developer" farklı isim, aynı iş. Katı filtre yapılırsa doğru adaylar elenir. Unvan sadece gösterimde ve embedding'de kullanılır — vektörlenen metnin ilk satırı.

> **Global not:** Unvan her dilde birikecek. `dil_kodu` öneri havuzunu süzmek için gerekli. Ama **embedding çok dilli olduğu için** "Backend Developer" ile "Backend Geliştirici" yine eşleşir.

**Ne arıyor** — çoklu seçim (iş / staj / ikisi de). İlan tarafında tek değer.

| İlan tipi | Aday tercihi | Sonuç |
|---|---|:---:|
| İş | [iş] | ✅ |
| İş | [staj] | ❌ |
| İş | [iş, staj] | ✅ |
| Staj | [staj] | ✅ |
| Staj | [iş] | ❌ |
| Staj | [iş, staj] | ✅ |

> **Asimetri neden mantıklı:** Son sınıf öğrencisi hem staj hem iş arar. Ama bir ilan tek tip olmak zorunda — formları, ücret yapısı, alanları farklı.

**Kartta rozet:** İlan listesinde tipi büyük punto ve farklı renkle görünsün. Aday tıklamadan ayırt etsin.

---

### Adım 3 — Eğitim

**"+ Okul Ekle" → tür seçimi → türe göre alanlar → kaydet → liste**

| Seçilen tür | Görünen alanlar |
|---|---|
| İlkokul / Ortaokul | Mezuniyet yılı |
| Lise | Okul adı *(serbest + öneri)*, bölüm, mezuniyet yılı |
| Ön lisans / Lisans / Y.Lisans / Doktora | Üniversite *(kapalı liste + "Diğer")*, bölüm, başlangıç–mezuniyet yılı, "devam ediyorum", not ortalaması *(opsiyonel)* |

```sql
okullar (id, ad, normalize_ad, tip, ulke_kodu, kaynak)
-- tip: universite | lise
-- kaynak: resmi_liste | kullanici_girdi

aday_egitimleri (aday_id, derece_isced, okul_id, serbest_okul_adi,
                 bolum, baslangic_yil, mezuniyet_yil,
                 devam_mi, not_ortalamasi, not_sistemi)
```

#### Eğitim dereceleri — ISCED eşlemesi

> ⚠️ **Global ölçekte en sinsi sorun bu.** Eğitim dereceleri ülkeler arasında birebir örtüşmüyor: Almanya'da *Ausbildung*, ABD'de *Associate Degree*, Türkiye'de ön lisans, Fransa'da *BTS*. "En az lisans" filtresi ülkeler arası çalışmaz.

**Çözüm: ISCED (BM standardı, 0-8) üzerinden karşılaştırma.**

```sql
egitim_dereceleri (id, isced_seviye, ulke_kodu, kod)
egitim_derecesi_cevirileri (derece_id, dil_kodu, ad)
```

- Kullanıcıya **kendi ülkesinin adıyla** gösterilir ("Ön lisans", "Associate Degree")
- Karşılaştırma **ISCED numarasıyla** yapılır
- `aday_profilleri.en_yuksek_isced` otomatik hesaplanır → ilanın eğitim filtresi bunu kullanır

| ISCED | Karşılık |
|:---:|---|
| 1-2 | İlkokul / ortaokul |
| 3 | Lise |
| 5 | Ön lisans / Associate |
| 6 | Lisans / Bachelor |
| 7 | Yüksek lisans / Master |
| 8 | Doktora / PhD |

**Neden üniversite kapalı, lise serbest:**

| | Gerekçe |
|---|---|
| Üniversite | Sayıları yönetilebilir, resmi listeler var. İşveren "şu okullardan mezun" diye filtreleyebilir → standart gerekli. **Ülke bazlı liste** (`okullar.ulke_kodu`). |
| Lise | Her ülkede on binlerce, listeye almak imkansız. Kimse liseye göre filtrelemiyor → serbest kalması zarar vermez. |

> **Üniversite listesi kaynağı:** Türkiye'de YÖK, global için açık veri setleri (Hipolabs universities API, UNESCO listeleri) başlangıç iskeleti verir. **Her lansman ülkesi için ayrı hazırlanmalı.** Listede olmayan için "Diğer" + serbest yazma kaçışı her zaman bulunmalı.

**Not ortalaması — sistem farkı:**

Not sistemleri farklı (Türkiye 4'lük, ABD 4'lük ama farklı hesap, Almanya 1-6 ters ölçek, Fransa 20'lik).

> **Öneri:** `not_sistemi` alanı tutulsun ve not **olduğu gibi gösterilsin**, normalize edilmesin. Zaten skora girmiyor — dönüştürme çabası hata üretir, faydası yok.

**Kurallar:**
- Aynı tür birden fazla eklenebilir (iki lisans yapan var, engellenmemeli)
- Liste en yeni üstte (CV mantığı)
- "Devam ediyorum" işaretliyse mezuniyet yılı gri düşer, yerine "beklenen mezuniyet yılı" gelir
- **En yüksek ISCED seviyesi otomatik hesaplanır** → ilanın eğitim filtresi bunu kullanır
- **Bölüm adı embedding'e girer**; okul adı ve not girmez

> **Okul adı neden embedding'e girmiyor:** Girerse aynı okuldan mezunlar birbirine yakın çıkar. Bu istediğimiz bir sinyal değil — hatta okul bazlı önyargı üretme riski taşıyor. **Global ölçekte bu risk daha büyük:** belirli ülkelerin üniversiteleri sistematik avantaj kazanabilir.

> **Not ortalaması neden opsiyonel ve skora girmiyor:** Zorunlu yapılırsa ya uydurulur ya kayıt bırakılır. Skora katmak ayrımcılık tartışması açabilir. Gösterilir, kararı işveren verir.

**"Devam ediyorum" işareti** staj/öğrenci filtresi için kritik.

---

### Adım 4 — Deneyim

**"+ Deneyim Ekle" → alanlar → kaydet → liste**

| Alan | Zorunlu | Giriş şekli |
|---|:---:|---|
| Şirket adı | ✅ | Serbest + öneri **(üç kaynaklı)** |
| Pozisyon | ✅ | Serbest + öneri |
| Başlangıç (ay/yıl) | ✅ | |
| Bitiş (ay/yıl) | ✅¹ | "Halen çalışıyorum" işaretliyse gri düşer |
| Çalışma türü | ✅ | Tam zamanlı / yarı zamanlı / staj / freelance / dönemsel / gönüllü |
| **Bu işte ne yaptın** | ✅ | 2-3 cümle — **embedding'e girer** |
| Şirket adını gizle | ❌ | İşaret kutusu |

¹ *"Halen çalışıyorum" işaretliyse zorunlu değil*

**Şirket adı — üç kaynaklı öneri:**

1. **Onaylı işveren şirketleri** → rozetli, listede üstte
2. Adayların daha önce yazdıkları → rozetsiz
3. Serbest yazma → hiçbiri uymuyorsa

```sql
sirketler (id, ad, normalize_ad, ulke_kodu,
           dogrulanmis_mi, isveren_hesabi_var_mi)

aday_deneyimleri (aday_id, sirket_id, serbest_sirket_adi, pozisyon,
                  baslangic, bitis, halen_mi, calisma_turu,
                  ne_yaptin, gizli_mi)
```

`sirket_id` doluysa kayıtlı şirkete bağlı; boşsa `serbest_sirket_adi` kullanılır.

> 💡 **Öneri — sonradan eşleştirme:** Aday serbest yazdığı bir şirket sonradan sisteme kaydolursa, arka planda çalışan bir iş `normalize_ad` üzerinden eşleşenleri bulup bağlayabilir. Yan faydası: şirket kaydolduğunda *"sizde çalışmış 12 aday var"* bilgisi doğal olarak çıkar — işveren tarafında güçlü bir ilgi çekici.

> **`normalize_ad` neden kritik:** Gizlenen şirketler özelliği ancak şirket adları standartsa çalışır. "ABC A.Ş." ile "ABC AŞ" iki ayrı kayıt olursa gizleme boşa gider.

> **Global not:** Aynı şirket farklı ülkelerde farklı yasal isimle kayıtlı olabilir (ABC GmbH / ABC Ltd / ABC A.Ş.). Öneri listesinde grup şirketlerini bağlamak ileride gerekebilir; şimdi gerekmiyor ama veri modelinde `ust_sirket_id` alanı bırakmak ucuz.

**"Bu işte ne yaptın" — bu bölümün en önemli alanı**

Zorunlu, en az bir-iki cümle. **Embedding'in en çok beslendiği yer.**

CV eşleştirmeye girmiyor, yetenekler sadece etiket, şirket profili hiç girmiyor. Adayın gerçek ayırt ediciliği bu metinlerden çıkıyor.

> 💬 **Ekip tartışması için:** Bu alanı zorunlu tutmak deneyim ekleme oranını düşürebilir. Alternatif: zorunlu tutmayıp "bu alan boşsa eşleşme gücü düşük" uyarısı vermek. Zorunlu tutma kararının arkasındayız çünkü boş deneyim kaydı sisteme hiçbir şey katmıyor — ama ilk verilerle ölçülmeli.

**Çalışma türü — tek liste**

Ayrı bir "staj mı" kutusu yok, hepsi tek listede. Böylece "tam zamanlı + staj işaretli" gibi çelişkili durumlar oluşmaz.

**Sistemde kullanımı:**
- **Tecrübe yılı tarihlerden otomatik hesaplanır** — aday ayrıca "5 yıl tecrübem var" yazmaz
- Çalışma türüne göre ağırlıklandırma: tam zamanlı tam, staj ve yarı zamanlı kısmi, gönüllü tecrübe hesabına girmez (profilde görünür)
- Öğrenci/yeni mezun ayrımı: staj kayıtları + eğitimde "devam ediyorum" birlikte değerlendirilir

**Şirket adını gizle**

İşaretlenirse kartta şöyle görünür:

```
Şirket adı gizli
Backend Developer
2022 – Halen
"Ödeme altyapısında mikroservis geliştirdim..."
```

Pozisyon, tarih ve açıklama görünür; sadece isim gizli. Ek sektör/çalışan sayısı bilgisi sorulmaz.

> ⚠️ **Bilinen sınırlılık:** Bu gizleme yanıltıcı olabilir. Pozisyon + tarih + iş açıklaması yazılı bir profil, o şirkette çalışan biri için genelde tanınabilir. Asıl korumayı **gizlenen şirketler** özelliği (Adım 11) sağlıyor. Bu alan ikincil bir rahatlık.

---

### Adım 5 — Yetenekler

**"+ Yetenek Ekle" → ad + yıl → liste**

| Alan | Zorunlu | Giriş |
|---|:---:|---|
| Yetenek adı | ✅ | Serbest + öneri (kullanım sayısına göre sıralı, dile göre süzülü) |
| Deneyim yılı | ✅ | Sayı |

**Sınır: en fazla 30** (ilan tarafıyla simetrik).

**Yıldız/seviye yok, sadece yıl.**

> **Neden:** Yıldız öz-değerlendirmedir, güvenilmez. Zamanla herkes 4-5 yıldız verir ve ayırt ediciliği kaybolur. "3 yıl React" ise doğrulanabilir bir iddia ve işverenin ilanında yazdığı şeyle doğrudan karşılaştırılabilir.

**Nereye gidiyor:**

| | |
|---|:---:|
| Katı filtre | ❌ |
| Embedding | ✅ **sadece yetenek adı** |
| Skor | ✅ `yetenek_ortusmesi`, %15 |

> **Yıl neden embedding'e girmiyor:** Sayılar vektör uzayında anlamlı çalışmıyor. "React 4 yıl" ile "React 1 yıl" metin olarak neredeyse aynı görünür. Üstelik sayılar metne karışınca asıl sinyali seyreltir. Yıl skorda aritmetikle hesaplanır.

**İki koldan eşleşir:**
1. **Etiket kesişimi** — aynı `yetenek_id` varsa doğrudan
2. **Embedding** — etiket tutmasa bile "React" ile "frontend geliştirme" yakınlığı yakalanır

> İkinci kol sayesinde serbest yazmanın dağınıklığı eşleştirmeyi bozmuyor. **Bu, tüm serbest yazma kararlarının dayandığı temel.** Global ölçekte ayrıca diller arası köprü kuruyor.

**Katı filtre olmaması bilinçli:** "React zorunlu" diyen ilana, React yazmamış ama 5 yıl frontend yapmış aday da düşer, sadece skoru düşük olur. İşveren kartta görür, kendi karar verir.

---

### Adım 6 — Diller

**İki ayrı bölüm.** Bu ayrım kritik:

#### A — Bildiğim Diller *(gösterim)*

Dil (kapalı liste) + seviye (Başlangıç / Orta / İleri / Anadil). İstediği kadar ekler.

| Katı filtre | Embedding | Skor |
|:---:|:---:|:---:|
| ❌ | ❌ | ❌ |

Sadece kartta gösterilir.

> **Neden hiçbir yere girmiyor:** İlan tarafında karşılığı yok — ilanda sadece "iş dili" var. Karşılaştırılacak alan olmadığı için eşleştirmeye giremez. Embedding'e de girmemesi bilinçli: "İngilizce C1, Almanca A2" gibi metin vektöre karışsa, dil bilen tüm adaylar birbirine yakın çıkar ve asıl sinyali seyreltir.

> **Global ölçekte değeri artıyor:** Çok dilli bir pazarda işveren için bu bilgi kartta gerçekten önemli — ama sistem ölçmüyor, insan değerlendiriyor.

#### B — Çalışabileceğim İş Dilleri *(katı filtre)*

Çoklu seçim, kapalı liste, **zorunlu**. Seviye sorulmaz.

**Otomatik kural:** Bildiğim dillerde **"Anadil"** seçilen dil, iş dillerine **otomatik işaretli** düşer. Aday görür, isterse kaldırır.

> **Neden sadece anadil, ileri değil:** Anadil tartışmasız. Ama "İngilizce - İleri" diyen biri İngilizce ortamda çalışmak istemeyebilir — o kararı kendisi vermeli.

> **Neden otomatik ekleme, uyarı değil:** Uyarı kullanıcıya *soru* sorar; otomatik ekleme *doğru şeyi yapar*. Ayrıca uyarı diyalogları okunmadan kapatılıyor. **Otomatik eklenen işaret görünür ve değiştirilebilir olmalı** — sessizce arkada set edilmemeli.

**Filtre — kesişim kontrolü:**

| İlan iş dili | Aday iş dilleri | Sonuç |
|---|---|:---:|
| [Türkçe] | [Türkçe] | ✅ |
| [Türkçe] | [Türkçe, İngilizce] | ✅ |
| [Türkçe] | [İngilizce] | ❌ |
| [İngilizce] | [Türkçe, İngilizce] | ✅ |
| [Türkçe, İngilizce] | [İngilizce] | ✅ |
| [Türkçe, İngilizce] | [Almanca] | ❌ |

> ⚠️ **En büyük tek risk:** Bu alan boş kalırsa aday **hiçbir ilanla eşleşmez** ve sebebini anlamaz. İki koruma zorunlu:
> 1. **Kullanıcının ülkesinin ana dili varsayılan işaretli gelsin** *(Türkiye'de Türkçe, Almanya'da Almanca, ABD'de İngilizce)*
> 2. Alan boş bırakılamaz

> **Global ölçekte ikinci bir işlevi var:** Embedding çok dilli olduğu için Türkçe yazılmış profil, İngilizce yazılmış ilanla **eşleşebiliyor** — ama aday o ilanı okuyamayabilir. Dil filtresi bunu engelliyor: okuyamayacağı ilan adaya hiç gösterilmiyor.

---

### Adım 7 — Sertifikalar

**"+ Sertifika Ekle" → alanlar → liste**

| Alan | Zorunlu |
|---|:---:|
| Sertifika adı | ✅ |
| Veren kurum | ❌ |
| Alınma tarihi | ❌ |
| Geçerlilik bitişi | ❌ |

Serbest + öneri. İlan tarafındaki sertifika alanıyla simetrik.

**Nereye gidiyor:**

| Katı filtre | Embedding | Skor |
|:---:|:---:|:---:|
| ❌ | ✅ *(sadece ad)* | ✅ |

> **Katı filtre neden değil:** İsimler tutmaz. "PMP" / "PMP Sertifikası" / "Project Management Professional" üç ayrı kayıt olur; katı filtre yapılırsa **sertifikası olan aday bile elenir.**

> **Embedding neden gerekli:** İsim dağınıklığını çözen tek şey bu. Aday "PMP" yazmış, ilan "Proje Yönetimi Sertifikası" istemiş — etiket kesişimi tutmuyor ama embedding ikisini yakın görüyor.

**Sadece sertifika adı embedding'e girer.** Veren kurum ve tarihler girmez.

> **Global not:** Bazı sertifikalar uluslararası geçerli (PMP, AWS, CFA, CISSP), bazıları tamamen yerel (Türkiye'de İSG uzmanlığı ve SMMM, Almanya'da Meisterbrief). Serbest yazma bu farkı kendiliğinden çözüyor — ayrı bir yapı gerekmiyor. **Yerel sertifikalar ülke sınırlarını aşmıyor, bu doğal ve doğru.**

**Geçerlilik:** Süresi dolmuş sertifika kartta "süresi geçmiş" olarak gösterilir, skorda sayılmaz.

---

### Adım 8 — Projeler / Portföy

**Tamamen opsiyonel adım.**

| Alan | Zorunlu | Not |
|---|:---:|---|
| Proje adı | ✅¹ | **Embedding'e girer** |
| Kısa açıklama | ✅¹ | 1-3 cümle — **embedding'e girer** |
| Link | ❌ | GitHub, Behance, kişisel site, Dribbble |
| Kullanılan teknolojiler | ❌ | Serbest + öneri (yetenek havuzundan) |
| Tarih | ❌ | Ay/yıl |

¹ *Proje ekliyorsa zorunlu*

**Sadece proje adı ve açıklama embedding'e girer.** Link ve tarih girmez.

**Link doğrulaması yapılmaz** — çalışıp çalışmadığını kontrol etmek gereksiz iş. Sadece format kontrolü.

> 💡 **Kimler için kritik:** Yeni mezun ve staj arayanlar. Deneyim kaydı olmayan adayın embedding'e girecek metni neredeyse yok — unvan, bölüm ve kısa tanıtım kalıyor. Proje açıklaması o boşluğu doldurur.
>
> **Öneri:** Staj/öğrenci akışında bu adımı öne çıkarın: *"Deneyimin yoksa projelerini ekle, daha çok ilanla eşleşirsin."*

---

### Adım 9 — CV ve Belgelerim

| Alan | Zorunlu | Kısıtlar |
|---|:---:|---|
| CV | ❌ | PDF, en fazla 5 MB, tek dosya |
| *(İleride)* Diğer belgeler | ❌ | Diploma, transkript, referans |

**Eşleştirmeye hiç girmez** — ne embedding, ne filtre, ne skor.

> **Neden:** Tüm adaylar aynı kurallarla değerlendirilsin. CV yükleyen kişi ekstra sinyalle öne geçmesin. Bu hem daha adil, hem **otomatik karar verme** düzenlemeleri açısından savunması kolay: *"Neye göre sıraladınız?"* → *"Adayın kendi girdiği yapılandırılmış alanlara göre."*

> ⚠️ **Global ölçekte bu karar daha da değerli:** AB Yapay Zeka Yasası işe alım amaçlı YZ sistemlerini **yüksek riskli** sınıfa alıyor; New York City'de otomatik işe alım araçları için bağımsız önyargı denetimi (Local Law 144) zorunlu. Şeffaf ve açıklanabilir bir skor yapısı, bu rejimlerde uyum maliyetini ciddi düşürüyor. **Alt skorların saklanması zorunlu tutulmalı.**

**Aday tarafı:** Yükler, **içeriğini önizlemede görür**, **kendi dosyasını indirebilir**.

**İşveren tarafı — kademeli erişim:**

| Yer | CV durumu |
|---|---|
| Keşfet | **Hiç görünmez** |
| Davet gönderildi, cevap bekleniyor | Görünmez |
| **Başvuranlar** | Görüntüle + **indir** |
| **Eşleşmeler** | Görüntüle + **indir** |

> **Keşfet'te neden hiç görünmüyor:** İşveren 200 kart kaydırıyor. Hepsinin CV'sini açabiliyorsa bu bir veri toplama aracına dönüşür. Keşfet kartında profil bilgileri karar vermek için yeterli.

**Şart:** Başvuru onay diyaloğunda aday bunu bilsin — *"CV'niz işverenle paylaşılacak."*

**Teknik:** Supabase Storage, özel bucket, RLS ile erişim.

> 💬 **Ekip tartışması için:** İndirme açık olması "gerçek silme" taahhüdünü zorlaştırıyor — aday hesabını silse bile indirilmiş CV'ler işverenlerin bilgisayarlarında kalır. **Global ölçekte bu GDPR'ın silme hakkı (md. 17) açısından daha keskin bir sorun.** Alternatif: sadece görüntüleme. Mevcut karar indirmeye izin veriyor; aydınlatma metninde açıkça belirtilmeli.

---

### Adım 10 — Diğer Bilgiler

İlan tarafındaki "hazır kutular" katı filtresinin karşılığı.

| Alan | Zorunlu | Katı filtre | Giriş |
|---|:---:|:---:|---|
| Sürücü belgesi | ❌ | **✅** | Yok / var → sınıf *(ülkeye göre liste)* |
| Aktif araç sahibi | ❌ | ❌ | İşaret kutusu |
| Askerlik durumu | ❌ | ❌ | Yapıldı / muaf / tecilli |
| Seyahat engeli yok | ❌ | **✅** | İşaret kutusu |
| Vardiyalı çalışabilirim | ❌ | **✅** | İşaret kutusu |

**Kararların gerekçeleri:**

| Alan | Karar | Gerekçe |
|---|---|---|
| Sürücü belgesi | Katı filtre, sınıf eşleşmeli | Şoför ilanına ehliyetsiz aday düşmemeli. |
| Aktif araç sahibi | Sadece bilgi | Araç sahibi olmayan biri araç alabilir veya şirket araç verebilir. |
| **Askerlik** | **Sadece bilgi** | Filtre yapılırsa kadın adaylar için bu alan boş olduğundan sürekli istisna kuralı gerekir; o kural bir yerde unutulursa **tüm kadın adaylar elenir**. Hukuki risk global: Türkiye'de İş Kanunu md. 5, AB'de 2000/78/EC istihdam eşitliği direktifi, ABD'de Title VII. |
| Seyahat engeli | Katı filtre | Saha satış, bölge sorumlusu gibi pozisyonlarda gerçekten esnemez. |
| Vardiyalı çalışma | Katı filtre | Fabrika, hastane, çağrı merkezi, güvenlik — olmazsa olmaz. |

> **Global not — sürücü belgesi sınıfları:** Sınıflandırma ülkeye göre farklı (AB'de A/B/C/D/E ortak, ABD'de eyalet bazlı Class A/B/C + CDL, Japonya kendi sistemi). Liste `ulke_surucu_siniflari` tablosunda tutulmalı.
>
> **Öneri:** Sınıf eşleşmesini **sadece aynı ülke içinde** uygulayın. Ülkeler arası ilanlarda "ehliyet var/yok" düzeyinde kalın — yoksa yanlış eleme yapılır. Ayrıca ehliyetin başka ülkede geçerli olup olmaması ayrı bir konu, sistemin çözemeyeceği bir şey.

**Filtre sadece iki şart bir aradayken devreye girer:**
1. İlan o şartı **istemiş** (kutu işaretli)
2. Adayda o bilgi **yok veya uymuyor**

Yazılımcı ilanı açan işveren ehliyet kutusuna dokunmuyor → ehliyeti olmayan aday rahatça eşleşiyor.

> **Askerliği filtreden çıkarma kararının yan faydası:** "Askerlik alanı boş olanlar otomatik geçer" istisna kuralına gerek kalmadı. Sistem sadeleşti ve unutma riski ortadan kalktı.

---

### Adım 11 — Beklentiler

| Alan | Zorunlu | Giriş | Filtre |
|---|:---:|---|:---:|
| Başlama zamanı | ❌ | Kapalı liste | ❌ gösterim |
| Gizlenecek şirketler | ❌ | Kayıtlı şirketlerden arama + seçim | **✅ katı** |

**Başlama zamanı** — kapalı liste: Hemen / 2 hafta içinde / 1 ay içinde / 2-3 ay içinde / Esnek.

Filtreye girmez, sadece gösterim.

> **Neden filtre değil:** İşveren "hemen başlayacak" diye süzüp, 1 ay sonra başlayabilecek mükemmel adayı kaybetmesin.

> **Neden kapalı liste:** Sınırlı sayıda anlamlı cevap var. Serbest bırakılırsa "2 hafta" / "iki hafta" / "15 gün" birikir, hiçbir faydası olmaz.

> **Global not:** İhbar süreleri ülkeye göre çok farklı (Almanya'da kıdeme göre 1-7 ay, ABD'de genelde 2 hafta, Türkiye'de 2-8 hafta). Seçenekler bu yelpazeyi kapsamalı — "3 ay+" seçeneği eklenmeli.

#### Maaş beklentisi — profilden çıkarıldı

> **Neden çıkarıldı:** Maaş beklentisi **pozisyona özeldir**. Aynı aday farklı pozisyonlar için farklı bekler. Profilde tek rakam istemek adayı en alt sınırını yazmaya zorlar.

**Yeni yer: başvuru anında.**

```
Net minimum maaş beklentiniz (opsiyonel)
[____] [TRY ▾]        ← varsayılan: ilanın para birimi

Not (opsiyonel)
[_________________________]
```

**Kararların gerekçeleri:**

| Karar | Gerekçe |
|---|---|
| Tek rakam, aralık değil | Aralık pazarlıkta işe yaramıyor — işveren her zaman alt sınırı okur. |
| "Net minimum" ifadesi | Net/brüt karışıklığı çoğu ülkede ciddi. "Minimum" aralık belirsizliğini kaldırıyor. |
| **Para birimi alanı** | Varsayılan ilanın para birimi. Aday isterse değiştirir. |
| Opsiyonel | Bazı aday "görüşmede konuşalım" der. Zorunlu yapılırsa rastgele sayı girilir. |
| Not alanı | "Esnek olabilirim", "yan haklara göre değişir" — işveren için değerli. |

**Maaş katı filtre listesinde değil** — karşılaştırılacak iki taraf yok.

**İşveren tarafında:** Başvuranlar listesinde kolon olarak görünür, sıralanabilir/gruplanabilir.

```sql
basvurular (aday_id, ilan_id, net_min_maas, para_birimi,
            not, tarih, durum)
```

> **Kur çevrimi yapılmadan saklanır** — kur değişince geçmiş veriler bozulmasın. Gösterimde yaklaşık karşılık verilebilir *(ileride, kur servisi gerekir)*.

> 💡 **Beklenmeyen fayda:** Aday, maaş yazmayan ilana başvururken ne yazacağını bilemez — bu işvereni maaş yazmaya iter.

**Davet durumunda:** İşveren Keşfet'ten davet ederse maaş konuşulmamış olur. Aday daveti kabul ederken beklentisini yazabilir, ya da sohbette konuşulur.

#### Gizlenecek şirketler

**En hassas özellik.** Aday şu an çalışıyor ve mevcut işvereninin onu görmesini istemiyor.

```sql
aday_gizlenen_sirketler (aday_id, sirket_id)
```

**Tek yönlü çalışır:**

| | |
|---|---|
| **İşveren adayı göremez** | Keşfet'te çıkmaz, davet gönderemez, profiline erişemez |
| **Aday ilanları görebilir** | Hatta başvurabilir |

> **Neden tek yönlü:** Aday mevcut işvereninin onu *bulmasını* istemiyor, ama o şirketin ilanlarını görmekten rahatsız değil — hatta kendi şirketindeki başka bir pozisyona başvurmak isteyebilir.

**Başvuru durumunda uyarı:**

> "Bu şirketi gizli listenize eklemişsiniz. Başvurursanız şirket profilinizi görebilecek. Devam etmek istiyor musunuz?"

**Sorgudaki yeri:**

```sql
-- İşveren tarafı (aday havuzu): gizleme UYGULANIR
-- Aday tarafı (ilan havuzu): gizleme UYGULANMAZ
```

**Üç kritik nokta:**

1. **Sadece kayıtlı şirketler gizlenebilir.** Sistemde olmayan şirket ilan açmıyor.
2. **`normalize_ad` üzerinden arama** + `pg_trgm` ile yazım hatası toleransı.
3. **RLS ile korunmalı.** Gizlenen şirket adayı veritabanı seviyesinde okuyamasın.

**Gizlenen aday istatistiklerde de sayılmaz.**

> **Global not — grup şirketleri:** Aday "ABC Türkiye"yi gizledi ama "ABC GmbH" ayrı kayıt olarak duruyor ve onu görebiliyor. Grup şirketi bağlantısı (`ust_sirket_id`) kurulursa gizleme tüm gruba uygulanabilir. Şimdi gerekmiyor, veri modelinde alan bırakmak yeterli.

> 💡 **En işe yarayacak detay — otomatik öneri:** Aday deneyim kaydında "halen çalışıyorum" işaretli bir şirket varsa ve o şirket sistemde kayıtlıysa, gizleme ekranında öneri olarak düşsün:
>
> *"Şu an ABC Teknoloji'de çalışıyorsunuz. Bu şirketten gizlenmek ister misiniz?"*

---

### Adım 12 — Önizleme, Arama Durumu, Yayınlama

#### 1. Kart önizlemesi

Keşfet kartıyla **aynı bileşen** — işverenin göreceği birebir hal.

Gizli alanlar burada da görünmez: doğum tarihi, cinsiyet, telefon, e-posta.

#### 2. Profil tamamlanma yüzdesi

Yüzde + eksik alan listesi. Her eksiğin yanında **"düzenle" bağlantısı**.

#### 3. Arama özeti

**Bu bölümün en faydalı parçası.** Filtrelerin ne yaptığı düz dille:

> *"Bilişim alanında, Almanya (Bayern) ve Türkiye (İstanbul)'da, Türkçe ve Almanca iş dilinde, tam zamanlı iş ilanları size gösterilecek."*

> **Neden kritik:** Katı filtreler **sessizce** eliyor. Aday yanlışlıkla tek konum seçmiş, iş dilinden bir dili kaldırmış veya sürücü belgesi alanını boş bırakmışsa — *"hiç ilan gelmiyor"* der ve sebebini asla bulamaz.

#### 4. Eşleşme gücü ve uyarılar

Önizleme ekranında **ve** profil ana ekranında:

```
Eşleşme gücün: Zayıf ●○○

Güçlendirmek için:
+ Yeteneklerini ekle    (en çok fark yaratan)
+ Deneyimlerini ekle
+ Kısa tanıtım yaz

⚠️ Sürücü belgesi, seyahat ve vardiya bilgilerini doldurmadın.
   Bu şartları isteyen ilanlarla eşleşemezsin.
```

**İkisi farklı şey söylüyor, o yüzden ikisi de gerekli:**

| | Mesaj | Psikolojik etki |
|---|---|---|
| Eşleşme gücü | "Eşleşeceksin ama zayıf" | Kazanç fırsatı |
| Uyarı | "Bu ilanları hiç göremeyeceksin" | **Somut kayıp** |

> Kayıp, kazançtan daha çok harekete geçirir.

**Uygulama detayları:**
- **Satırlar tıklanabilir** olsun, doğrudan o adıma götürsün
- **Gösterge dolarken anında değişsin** — anlık geri bildirim
- Uyarıda **"bu şartları isteyen"** ifadesi kritik — aday tüm ilanları kaybettiğini sanmasın

#### 5. Arama durumu

Yayınla butonunun **hemen üstünde** sorulur:

| Durum | Anlamı | Sistemde etkisi |
|---|---|---|
| 🟢 **Aktif arıyorum** | Hemen başlayabilirim | Rozet görünür, sıralamada hafif avantaj |
| 🟡 **Fırsatlara açığım** | Çalışıyorum ama doğru teklife bakarım | Normal görünür — **varsayılan** |
| ⚪ **Şu an bakmıyorum** | Pasif | Keşfet'te hiç görünmez, davet gelmez |

**"Şu an bakmıyorum" seçilince:**
- Keşfet'te görünmez, davet gelmez
- **Mevcut eşleşmeler ve sohbetler devam eder**
- Profil ana ekranında uyarı: *"Profilin gizli, işverenler seni göremiyor"*

**İki yerde erişilebilir:**
1. Adım 12'de (ilk kurulum)
2. **Profil ana ekranında, en üstte** — tek tıklamayla

**Otomatik bakım:** 30-45 gün hiç giriş yapmamış "aktif arıyorum" profiline bildirim gitsin. Cevap yoksa **otomatik "fırsatlara açığım"a düşsün**.

> **Neden gerekli:** Havuz ölü profillerle dolarsa işveren davet atar, cevap alamaz ve sisteme güveni gider. **Bu, ürünü sessizce öldüren şeylerden biri.**

> **Global not:** Hatırlatma bildirimi kullanıcının **saat dilimine** göre gönderilmeli.

#### 6. Yayınla butonu

Basınca profil aktif olur, arka planda embedding üretilir ve uygun ilanlar hesaplanır.

---

## 4. Yayına Girme Eşiği

**Zorunlu 4 adım:**

| Adım | Neden zorunlu |
|---|---|
| **1 — Temel Bilgiler** | Konum ve çalışma şekli katı filtreler; olmadan eşleştirme yapılamaz |
| **2 — Meslek Alanı, Unvan, Ne Arıyor** | Kaba filtrenin tamamı burada |
| **3 — Eğitim** | Eğitim seviyesi (ISCED) katı filtre + bölüm adı embedding'e girecek tek garantili metin |
| **6 — İş dili** | Boş kalırsa aday **hiçbir ilanla eşleşmez** |

**Opsiyonel:** Deneyim, yetenekler, sertifikalar, projeler, CV, diğer bilgiler, beklentiler

> **Kapı kapanmıyor:** Eğitim kaydında en düşük tür "İlkokul" ve orada sadece mezuniyet yılı isteniyor.

### Eşiğin sonuçları — bilerek kabul edilen bedeller

| Eksik alan | Sonuç | Adaya söyleniş |
|---|---|---|
| Yetenek, deneyim, tanıtım | **Zayıf eşleşme** (çalışır ama kaba) | "Eşleşme gücün zayıf" |
| Ehliyet, seyahat, vardiya | **Eleme** (sessiz kayıp) | "Bu şartları isteyen ilanlarla eşleşemezsin" |

**Sadece eşik doldurulmuşsa embedding metni şu kadar:**

```
Unvan: Backend Geliştirici
Bölüm: Bilgisayar Mühendisliği
```

> 💬 **Ekip tartışması için — en önemli açık soru:** Yetenekleri (en az 3) eşiğe eklemek adayın kendi yararına olabilir. Karşı argüman: her zorunlu alan tamamlama oranını düşürür.
>
> **Öneri:** İlk sürümde eşiğe eklemeyin, ama **tamamlama oranı ve eşleşme kalitesini ölçün**.

---

## 5. Veri Akışı Özeti

### Embedding'e giren metinler

```
Unvan
Kısa tanıtım
Yetenek adları (yıl HARİÇ)
Deneyim: pozisyon + "ne yaptın" metinleri
Eğitim: bölüm adları (okul adı HARİÇ, not HARİÇ)
Sertifika adları (veren kurum ve tarih HARİÇ)
Proje adı + açıklaması (link ve tarih HARİÇ)
```

**Dil karışık olabilir — sorun değil.** Çok dilli model tüm dilleri aynı vektör uzayına gömüyor.

### Katı filtreye giren alanlar

| Alan | Adım |
|---|:---:|
| **Ülke kodu** | 1 |
| Çalışmak istediği konumlar (`konum_yolu`) | 1 |
| Çalışma şekli | 1 |
| Meslek alanı (ana + ikincil) | 2 |
| Ne arıyor (iş/staj) | 2 |
| Eğitim seviyesi (ISCED) | 3 |
| İş dili | 6 |
| Sürücü belgesi, seyahat, vardiya | 10 |
| Gizlenecek şirketler | 11 |

### Sadece skora giren

| Alan | Bileşen |
|---|---|
| Yetenekler (yıl dahil) | `yetenek_ortusmesi` %15 |
| Toplam tecrübe (otomatik hesaplanan) | `tecrube_uyumu` %10 |
| Konum yakınlığı | `konum_uyumu` %10 |
| Profil tamamlanma | `profil_kalitesi` %5 |

### Hiçbir yere girmeyen (sadece gösterim)

Fotoğraf · Doğum tarihi · Cinsiyet · Yaşadığı konum · Bildiği diller + seviye · Aktif araç sahibi · Askerlik durumu · Başlama zamanı · Not ortalaması · CV · Link'ler

### İki altın kural

1. **Sayısal alanlar embedding'e girmez.** Yıl, maaş, not, tarih — hepsi aritmetikle hesaplanır.
2. **Katı filtre sadece gerçekten esnemeyen şeyler için.** Yetenek, tecrübe, bölüm, sertifika skorda ağırlık taşır.

---

## 6. Yayına Çıkmadan Önce Hazırlanması Gerekenler

| Veri | Miktar | Kaynak |
|---|---|---|
| Konumlar | ~25 bin (nüfus 15K+) | **Geonames** |
| Meslek alanları | 15-20 + **çevirileri** | ISCO-08'den sadeleştirilecek |
| Unvanlar | 300-500 × **her lansman dili** | ISCO-08 (çok dilli) |
| Yetenekler | 200-300 × **her lansman dili** | Elle |
| Üniversiteler | **Ülke başına** | YÖK (TR) + açık veri setleri |
| Eğitim dereceleri | **ISCED eşlemesi + ülke adları** | ISCED (BM) |
| Diller | 20-30 | ISO 639 |
| Sürücü belgesi sınıfları | **Ülke başına** | Elle |
| Para birimleri | ISO 4217 | Standart liste |
| Çalışma yaşı | **Ülke başına** | Elle + hukuki danışma |

> ⚠️ **Boş öneri havuzu, bu bölümün en büyük sessiz riski.** İlk 50 kullanıcı boş liste görürse herkes farklı yazar, veri dağılır ve öneri sistemi bir daha kendine gelemez.

> ⚠️ **Her yeni ülke/dil açmak bir veri hazırlık işidir** — sadece arayüz çevirisi değil: unvan havuzu, yetenek havuzu, üniversite listesi, eğitim eşlemesi, ehliyet sınıfları, çalışma yaşı.

---

## 7. Açık Sorular — Ekip Tartışması İçin

| # | Soru | Öneri |
|:---:|---|---|
| 1 | Yetenekler eşiğe eklenecek mi? | İlk sürümde hayır, ölç ve karar ver |
| 2 | Cinsiyet alanı hiç tutulmasın mı? | Tutmamak daha güvenli; global ölçekte daha da güçlü argüman |
| 3 | İşveren CV indirebilsin mi? | Şu an evet; GDPR silme hakkı açısından tartışmalı |
| 4 | "Ne yaptın" metni zorunlu kalsın mı? | Evet, ama deneyim ekleme oranı ölçülmeli |
| 5 | İkincil meslek alanı filtre gücünü ne kadar düşürüyor? | Gerçek veriyle ölçülecek |
| 6 | Doğum tarihi işverene gösterilsin mi? | Şu an hayır |
| 7 | Profil tamamlanma yüzdesi hangi alanlara kaç puan verecek? | Belirlenmedi |
| 8 | Skor ağırlıklarının nihai değerleri | Veritabanında tutulacak, gerçek veriyle ayarlanacak |
| 9 | **Çalışma izni / vize sponsorluğu alanı eklenecek mi?** | Global ölçekte gerçek bir katı filtre adayı — aday "çalışma iznim var/yok", ilan "vize desteği veriyoruz" |
| 10 | **Hangi ülkelerle lansmana çıkılacak?** | Veri hazırlığı ve hukuki uyum buna bağlı |
| 11 | **Veri yerleşimi (data residency)** | Bazı ülkeler kişisel verinin yurt dışına çıkmasını kısıtlıyor — mimari konu |
| 12 | Eğitim ISCED eşlemesi kim tarafından yapılacak? | Ülke başına elle |

---

## 8. Sırada

| Bölüm | Durum |
|---|---|
| Giriş / kayıt | ✅ |
| Şirket doğrulama | ✅ |
| Şirket profili | ✅ |
| İlan oluşturma | ✅ |
| **Aday profili** | ✅ |
| **Eşleştirme motoru** | ⏭️ Sırada |
| Keşfet, başvuru, davet, eşleşme, sohbet | Bekliyor |
| Bildirimler | Bekliyor |
| Admin paneli | Bekliyor |
| Veri koruma (KVKK / GDPR / diğer) | Bekliyor |

**Eşleştirme motorunda konuşulacaklar:** filtre zincirinin sırası, skor formülleri, iki yön ayrı skor, arka plan işinin tetikleyicileri, `pg_cron` düzeni, boş sonuç senaryoları, **global ölçekte bölümlendirme ve kuyruk yapısı.**
