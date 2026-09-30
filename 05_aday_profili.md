# İş Eşleştirme Projesi — Aday Profili

> Son güncelleme: 20 Eylül 2026
> Durum: Kararlar tamamlandı, ekip tartışmasına hazır
> İlgili dokümanlar: `giris-kayit-kararlari.md`, `sirket-dogrulama-ekrani.md`, `sirket-profili.md`, `ilan-olusturma.md`

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

---

## 2. Genel Tasarım İlkeleri

Bölüm boyunca tekrar eden dört desen:

### 2.1 Serbest yazma + öneri (kullanımdan öğrenen liste)

Yetenek, unvan, şirket adı, lise adı, sertifika, bölüm — hepsi böyle. Aday yazmaya başlar, daha önce yazılanlar kullanım sayısına göre sıralı düşer, seçer veya kendi yazar. Yeni kayıt hemen öneri havuzuna girer.

**Neden çalışıyor:** Embedding anlamsal yakınlığı yakalıyor, bu yüzden isim dağınıklığı ("ReactJS" / "React.js" / "Reakt") eşleştirmeyi bozmuyor.

**Nerede çalışmıyor:** Katı filtreye giren alanlarda. Orada kapalı liste zorunlu.

```sql
-- Desen tüm öneri tabloları için aynı
<tablo> (id, ad, normalize_ad, kaynak, kullanim_sayisi)
-- kaynak: resmi_liste | kullanici_girdi
```

`normalize_ad` (küçük harf, noktalama temizliği) + `pg_trgm` ile yazım hataları yakalanır.

> ⚠️ **Kritik risk:** Öneri havuzları başlangıçta boş. İlk kullanıcılar herkes farklı yazar, veri dağılır ve sistem hiç kendine gelemez. **Başlangıç verisi girilmeden yayına çıkılamaz:** ~300-500 unvan, ~200-300 yetenek, üniversite listesi (YÖK), meslek alanları (15-20).

### 2.2 Kapalı liste — sadece katı filtre alanlarında

| Alan | Neden kapalı |
|---|---|
| Meslek alanı | Kaba filtre; "Bilişim"/"IT"/"Yazılım" dağınıklığı filtreyi öldürür |
| Şehir (il/ilçe) | Konum filtresi |
| Diller | Kesişim kontrolü |
| Üniversite | İşveren filtreleyebilir, standart gerekli |
| Eğitim derecesi | Seviye karşılaştırması |

### 2.3 "+ Ekle" deseni

Eğitim, deneyim, yetenek, dil, sertifika, proje — hepsi liste. Sabit alan yok, "+ Ekle" → satır açılır → kaydet → listeye düşer.

**Neden:** Sabit alanlar hiç kimsenin durumuna tam oturmuyor. İki lisans yapan, üç sertifikası olan, hiç deneyimi olmayan — hepsi aynı yapıda çalışmalı.

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
| Yaşadığı il/ilçe | ✅ | ✅ | ❌ | ❌ |
| Çalışmak istediği şehirler | ✅¹ | ✅ | **✅** | ❌ |
| Her konumda çalışmaya hazırım | ❌ | ✅ | Filtreyi atlatır | ❌ |
| Çalışma şekli tercihi | ✅ | ✅ | **✅** | ❌ |
| Kısa tanıtım | ❌ | ✅ | ❌ | **✅** |

¹ *"Her konumda çalışmaya hazırım" işaretliyse zorunlu değil*

**Fotoğraf** — yüklenmezse baş harflerden renkli avatar üretilir. Zorunlu değil: hem bazı kişiler koymak istemez, hem fotoğrafa göre seçim ayrımcılık riski taşır.

**Doğum tarihi** — zorunlu ama **işverene gösterilmez**. Sistemde 18 yaş kontrolü ve istatistik için durur. Kartta yaş yazması gereksiz eleme sebebi yaratır.

**Cinsiyet** — opsiyonel, "belirtmek istemiyorum" seçenekli. İşverene gösterilmez, filtreye ve skora hiç girmez.

> 💬 **Ekip tartışması için:** Cinsiyet alanını hiç tutmama seçeneği de var. Tutmanın tek gerekçesi ileride demografik istatistik. Tutmamanın gerekçesi: veri yoksa sızma ve kötüye kullanım riski de yok. İkisi de savunulabilir.

**İletişim — üç kademeli görünürlük:**

| Aşama | Telefon / e-posta |
|---|:---:|
| Keşfet kartı | ❌ |
| Aday başvurduktan sonra | ❌ |
| **Eşleşmeden sonra** | ✅ |

> Bu kural olmazsa işveren Keşfet'te 200 adayın telefonunu toplar, uygulamayı hiç kullanmadan dışarıdan arar. **Platform dışına kaçış, bu tür pazar yerlerini öldüren bir numaralı sebeptir.**

**Yaşadığı il/ilçe** — sadece gösterim, filtrede kullanılmaz.

**Çalışmak istediği şehirler** — konum filtresinin dayandığı alan. İstanbul'da oturan ama Ankara'da çalışmaya hazır aday, Ankara ilanlarına düşebilmeli.

**"Her konumda çalışmaya hazırım"** — önceki "taşınabilirim" alanının yerine geldi.

> **Neden değişti:** "Taşınabilirim" ile "çalışmak istediğim şehirler" çelişiyordu. Aday hem "Ankara'da çalışırım" hem "taşınabilirim" derse hangisi geçerli belirsiz kalıyordu. Yeni ifade net: işaretlenirse şehir listesi zorunluluğu kalkar ve filtre tamamen atlanır. Arayüzde de kutu işaretlenince şehir alanı gri düşer.

**Kısa tanıtım** — 2-4 cümle, 300-500 karakter. **Embedding'e giren üç metinden biri** (diğerleri: deneyim açıklamaları, proje açıklamaları).

Yer tutucu örnek metin konulmalı:

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

> **Neden ayrı isim:** Sektör = şirket ne iş yapıyor (Gıda, İnşaat). Meslek alanı = kişi ne iş yapıyor (Bilişim, Muhasebe). Bir inşaat şirketi muhasebeci arayabilir — eşleşen şey meslek, sektör değil. Aynı ismi kullanmak kullanıcıyı karıştırır.

**Meslek alanı neden kapalı liste ve neden katı filtre:**

Embedding tek başına bunu güvenilir yapmıyor. "Proje yönetimi, ekip koordinasyonu, bütçe takibi" yazan bir inşaat yöneticisi ile aynı şeyleri yazan bir yazılım proje yöneticisi vektör uzayında çok yakın çıkar. Meslek alanı olmasa ikisi birbirine düşer.

Ayrıca performans: 50 bin adaylık havuzu tek sorguda ~3 bine indiriyor, embedding hesabı ondan sonra çalışıyor.

**İkincil meslek alanı** — opsiyonel, en fazla bir tane. Arayüzde katlanmış dursun ki çoğu kullanıcı hiç dokunmasın.

```sql
aday_profilleri (
  ana_meslek_alani_id      NOT NULL,
  ikincil_meslek_alani_id  NULL
)
-- filtre: ilan.meslek_alani_id IN (ana, ikincil)
```

**İki fayda:** (a) grafik tasarımcı hem Tasarım hem Pazarlama'ya bakabilir, kariyer değiştiren iki alanda arayabilir; (b) ana alan skorda daha ağır sayılır, unvan önerileri ana alandan gelir.

> 💬 **Ekip tartışması için:** Aday tarafında ilan sayısı adaydan az olacak. Tek meslek alanı Keşfet'i boşaltma riski taşıyor. İkincil alan bu riski azaltıyor ama filtre gücünü de bir miktar düşürüyor. İlk veriyle ölçüp ayarlanacak bir parametre.

**Unvan** — serbest + öneri, ana meslek alanına bağlı liste. Katı filtreye girmez.

> **Neden serbest:** "Yazılım Mühendisi" ile "Backend Developer" farklı isim, aynı iş. Katı filtre yapılırsa doğru adaylar elenir. Unvan sadece gösterimde ve embedding'de kullanılır — vektörlenen metnin ilk satırı.

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
okullar (id, ad, normalize_ad, tip, kaynak)
-- tip: universite | lise
-- kaynak: resmi_liste | kullanici_girdi

aday_egitimleri (aday_id, derece, okul_id, serbest_okul_adi,
                 bolum, baslangic_yil, mezuniyet_yil,
                 devam_mi, not_ortalamasi)
```

**Neden üniversite kapalı, lise serbest:**

| | Gerekçe |
|---|---|
| Üniversite | Türkiye'de ~200 tane, YÖK listesi hazır. İşveren "şu okullardan mezun" diye filtreleyebilir → standart gerekli. |
| Lise | On binlerce tane, listeye almak imkansız. Kimse liseye göre filtrelemiyor → serbest kalması zarar vermez. |

**Kurallar:**
- Aynı tür birden fazla eklenebilir (iki lisans yapan var, engellenmemeli)
- Liste en yeni üstte (CV mantığı)
- "Devam ediyorum" işaretliyse mezuniyet yılı gri düşer, yerine "beklenen mezuniyet yılı" gelir
- **En yüksek derece otomatik hesaplanır** → ilanın eğitim seviyesi filtresi bunu kullanır
- **Bölüm adı embedding'e girer**; okul adı ve not girmez

> **Okul adı neden embedding'e girmiyor:** Girerse aynı okuldan mezunlar birbirine yakın çıkar. Bu istediğimiz bir sinyal değil — hatta okul bazlı önyargı üretme riski taşıyor.

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
sirketler (id, ad, normalize_ad, dogrulanmis_mi, isveren_hesabi_var_mi)

aday_deneyimleri (aday_id, sirket_id, serbest_sirket_adi, pozisyon,
                  baslangic, bitis, halen_mi, calisma_turu,
                  ne_yaptin, gizli_mi)
```

`sirket_id` doluysa kayıtlı şirkete bağlı; boşsa `serbest_sirket_adi` kullanılır.

> 💡 **Öneri — sonradan eşleştirme:** Aday serbest yazdığı bir şirket sonradan sisteme kaydolursa, gece çalışan bir iş `normalize_ad` üzerinden eşleşenleri bulup bağlayabilir. Bunun bir yan faydası var: şirket kaydolduğunda *"sizde çalışmış 12 aday var"* bilgisi doğal olarak çıkar — işveren tarafında güçlü bir ilgi çekici.

> **`normalize_ad` neden kritik:** Gizlenen şirketler özelliği ancak şirket adları standartsa çalışır. "ABC A.Ş." ile "ABC AŞ" iki ayrı kayıt olursa gizleme boşa gider.

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
| Yetenek adı | ✅ | Serbest + öneri (kullanım sayısına göre sıralı) |
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

> İkinci kol sayesinde serbest yazmanın dağınıklığı eşleştirmeyi bozmuyor. **Bu, tüm serbest yazma kararlarının dayandığı temel.**

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

#### B — Çalışabileceğim İş Dilleri *(katı filtre)*

Çoklu seçim, kapalı liste, **zorunlu**. Seviye sorulmaz.

**Otomatik kural:** Bildiğim dillerde **"Anadil"** seçilen dil, iş dillerine **otomatik işaretli** düşer. Aday görür, isterse kaldırır.

> **Neden sadece anadil, ileri değil:** Anadil tartışmasız. Ama "İngilizce - İleri" diyen biri İngilizce ortamda çalışmak istemeyebilir — o kararı kendisi vermeli.

> **Neden otomatik ekleme, uyarı değil:** Uyarı kullanıcıya *soru* sorar; otomatik ekleme *doğru şeyi yapar*. Aday "İngilizce - Anadil" yazarken zihninde zaten "İngilizce biliyorum" var. Ona "ama iş dili olarak seçmedin" demek, sistemin iç mantığını ona anlatmak olur. Ayrıca uyarı diyalogları okunmadan kapatılıyor. **Otomatik eklenen işaret görünür ve değiştirilebilir olmalı** — sessizce arkada set edilmemeli, yoksa kullanıcı kontrolü kaybeder.

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
> 1. **Türkçe varsayılan işaretli gelsin** (hem adayda hem ilanda)
> 2. Alan boş bırakılamaz

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

**Sadece sertifika adı embedding'e girer.** Veren kurum ve tarihler girmez — tarih sayısal, veren kurum ayırt edici sinyal taşımıyor.

**Geçerlilik:** Süresi dolmuş sertifika kartta "süresi geçmiş" olarak gösterilir, skorda sayılmaz. Bazı sertifikalar (İSG, ilk yardım) yenilenmezse geçersiz oluyor.

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

**Link doğrulaması yapılmaz** — çalışıp çalışmadığını kontrol etmek gereksiz iş. Sadece format kontrolü (başında `https://` yoksa eklenir).

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

> **Neden:** Tüm adaylar aynı kurallarla değerlendirilsin. CV yükleyen kişi ekstra sinyalle öne geçmesin. Bu hem daha adil, hem KVKK'nın otomatik karar verme maddesi açısından savunması kolay: *"Neye göre sıraladınız?"* → *"Adayın kendi girdiği yapılandırılmış alanlara göre."*

**Aday tarafı:**
- Yükler
- **İçeriğini önizlemede görür** — doğru dosyayı yüklediğinden emin olur
- **Kendi dosyasını indirebilir**

**İşveren tarafı — kademeli erişim:**

| Yer | CV durumu |
|---|---|
| Keşfet | **Hiç görünmez** |
| Davet gönderildi, cevap bekleniyor | Görünmez |
| **Başvuranlar** | Görüntüle + **indir** |
| **Eşleşmeler** | Görüntüle + **indir** |

> **Keşfet'te neden hiç görünmüyor:** İşveren 200 kart kaydırıyor. Hepsinin CV'sini açabiliyorsa bu bir veri toplama aracına dönüşür ve aday kendi isteğiyle paylaşmadığı bir şeyin okunduğunu bilmiyor. Keşfet kartında profil bilgileri (deneyim, yetenekler, eğitim, açıklamalar) karar vermek için yeterli.

> **Başvuranlar'da neden indirme açık:** Aday kendi isteğiyle başvurdu, CV'sini bilerek paylaştı. İşveren başvuruyu değerlendirmek için gerçekten ihtiyaç duyar.

**Şart:** Başvuru onay diyaloğunda aday bunu bilsin — *"CV'niz işverenle paylaşılacak."* Sessizce paylaşılmasın.

**Teknik:** Supabase Storage, özel bucket, RLS ile erişim (aday kendisi + başvurduğu/eşleştiği işverenler).

> 💬 **Ekip tartışması için:** İndirme açık olması KVKK'da "gerçek silme" taahhüdünü zorlaştırıyor — aday hesabını silse bile indirilmiş CV'ler işverenlerin bilgisayarlarında kalır. Alternatif: sadece görüntüleme. Ama işverenin işini ciddi zorlaştırır (ekip içinde paylaşamaz, not alamaz). Mevcut karar indirmeye izin veriyor; aydınlatma metninde bu açıkça belirtilmeli.

---

### Adım 10 — Diğer Bilgiler

İlan tarafındaki "hazır kutular" katı filtresinin karşılığı.

| Alan | Zorunlu | Katı filtre | Giriş |
|---|:---:|:---:|---|
| Sürücü belgesi | ❌ | **✅** | Yok / var → sınıf (B, C, D, E — çoklu) |
| Aktif araç sahibi | ❌ | ❌ | İşaret kutusu |
| Askerlik durumu | ❌ | ❌ | Yapıldı / muaf / tecilli |
| Seyahat engeli yok | ❌ | **✅** | İşaret kutusu |
| Vardiyalı çalışabilirim | ❌ | **✅** | İşaret kutusu |

**Kararların gerekçeleri:**

| Alan | Karar | Gerekçe |
|---|---|---|
| Sürücü belgesi | Katı filtre, sınıf eşleşmeli | Şoför ilanına ehliyetsiz aday düşmemeli. Sınıf kontrolü gerekli: ilan B istiyorsa adayda B olmalı (C ile otomobil kullanılmıyor). |
| Aktif araç sahibi | Sadece bilgi | Araç sahibi olmayan biri araç alabilir veya şirket araç verebilir — kapıda eleme sebebi değil. |
| **Askerlik** | **Sadece bilgi** | Filtre yapılırsa kadın adaylar için bu alan boş olduğundan sürekli istisna kuralı gerekir; o kural bir yerde unutulursa **tüm kadın adaylar elenir**. Hukuki risk var (İş Kanunu md. 5). Filtre dışında tutmak hem güvenli hem basit. |
| Seyahat engeli | Katı filtre | Saha satış, bölge sorumlusu gibi pozisyonlarda gerçekten esnemez. |
| Vardiyalı çalışma | Katı filtre | Fabrika, hastane, çağrı merkezi, güvenlik — olmazsa olmaz. "Gece çalışamam" diyen birini gece vardiyasına göndermek iki tarafın zamanını harcar. |

**Filtre sadece iki şart bir aradayken devreye girer:**
1. İlan o şartı **istemiş** (kutu işaretli)
2. Adayda o bilgi **yok veya uymuyor**

Yazılımcı ilanı açan işveren ehliyet kutusuna dokunmuyor → ehliyeti olmayan aday rahatça eşleşiyor.

> **Askerliği filtreden çıkarma kararının yan faydası:** Daha önce planladığımız "askerlik alanı boş olanlar otomatik geçer" istisna kuralına gerek kalmadı. Sistem sadeleşti ve unutma riski ortadan kalktı.

---

### Adım 11 — Beklentiler

| Alan | Zorunlu | Giriş | Filtre |
|---|:---:|---|:---:|
| Başlama zamanı | ❌ | Kapalı liste | ❌ gösterim |
| Gizlenecek şirketler | ❌ | Kayıtlı şirketlerden arama + seçim | **✅ katı** |

**Başlama zamanı** — kapalı liste: Hemen / 2 hafta içinde / 1 ay içinde / 2-3 ay içinde / Esnek.

Filtreye girmez, sadece gösterim.

> **Neden filtre değil:** İşveren "hemen başlayacak" diye süzüp, 1 ay sonra başlayabilecek mükemmel adayı kaybetmesin. Çoğu işveren beklemeye razı olur.

> **Neden kapalı liste:** Sınırlı sayıda anlamlı cevap var. Serbest bırakılırsa "2 hafta" / "iki hafta" / "15 gün" / "yarım ay" birikir, hiçbir faydası olmaz.

#### Maaş beklentisi — profilden çıkarıldı

> **Neden çıkarıldı:** Maaş beklentisi **pozisyona özeldir**. Aynı aday Senior Developer için 50 bin, Team Lead için 70 bin isteyebilir. Profilde tek rakam istemek adayı en alt sınırını yazmaya zorlar, yoksa düşük maaşlı ama ilginç ilanları hiç görmez.

**Yeni yer: başvuru anında.**

```
Net minimum maaş beklentiniz (opsiyonel)
[____] TL

Not (opsiyonel)
[_________________________]
```

**Kararların gerekçeleri:**

| Karar | Gerekçe |
|---|---|
| Tek rakam, aralık değil | Aralık pazarlıkta işe yaramıyor — işveren her zaman alt sınırı okur, aday kendi elini zayıflatır. |
| "Net minimum" ifadesi | Türkiye'de net/brüt karışıklığı ciddi (40 bin net ≠ 40 bin brüt). "Minimum" da aralık belirsizliğini kaldırıyor. |
| Opsiyonel | Bazı aday "görüşmede konuşalım" demek ister. Zorunlu yapılırsa rastgele sayı girilir. |
| Not alanı | "Esnek olabilirim", "yan haklara göre değişir" — işveren için değerli bilgi. |

**Maaş katı filtre listesinden tamamen çıktı** — karşılaştırılacak iki taraf yok artık.

**İşveren tarafında:** Başvuranlar listesinde kolon olarak görünür, sıralanabilir/gruplanabilir. İşveren bütçesine uyanları kendi seçer.

**İlan tarafında da net/brüt belirtilsin** — yoksa karşılaştırma anlamsız olur.

> 💡 **Beklenmeyen fayda:** İlanda maaş belirtme teşviki güçleniyor. Aday, maaş yazmayan ilana başvururken ne yazacağını bilemez — bu işvereni maaş yazmaya iter.

**Davet durumunda:** İşveren Keşfet'ten davet ederse maaş konuşulmamış olur. Aday daveti kabul ederken beklentisini yazabilir, ya da sohbette konuşulur.

```sql
basvurular (aday_id, ilan_id, net_min_maas, not, tarih, durum)
```

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

> **Neden tek yönlü:** Aday mevcut işvereninin onu *bulmasını* istemiyor, ama o şirketin ilanlarını görmekten rahatsız değil — hatta kendi şirketindeki başka bir pozisyona başvurmak isteyebilir. Tersi olsaydı adayın kendi iradesini kısıtlamış olurduk.

**Başvuru durumunda uyarı:**

> "Bu şirketi gizli listenize eklemişsiniz. Başvurursanız şirket profilinizi görebilecek. Devam etmek istiyor musunuz?"

**Sorgudaki yeri:**

```sql
-- İşveren tarafı (aday havuzu): gizleme UYGULANIR
WHERE aday.id NOT IN (
  SELECT aday_id FROM aday_gizlenen_sirketler
  WHERE sirket_id = :ilan_sirket_id
)

-- Aday tarafı (ilan havuzu): gizleme UYGULANMAZ
```

**Üç kritik nokta:**

1. **Sadece kayıtlı şirketler gizlenebilir.** Sistemde olmayan şirket ilan açmıyor, gizlemenin etkisi olmaz. Arama kayıtlı işveren listesinden.
2. **`normalize_ad` üzerinden arama** + `pg_trgm` ile yazım hatası toleransı.
3. **RLS ile korunmalı.** Gizlenen şirket adayı veritabanı seviyesinde okuyamasın.

**Gizlenen aday istatistiklerde de sayılmaz.** *"İlanınıza uygun 120 aday var"* derken gizlemiş adaylar o sayıya girmemeli.

> 💡 **En işe yarayacak detay — otomatik öneri:** Aday deneyim kaydında "halen çalışıyorum" işaretli bir şirket varsa ve o şirket sistemde kayıtlıysa, gizleme ekranında öneri olarak düşsün:
>
> *"Şu an ABC Teknoloji'de çalışıyorsunuz. Bu şirketten gizlenmek ister misiniz?"*
>
> Çoğu aday bu özelliğin varlığını fark etmeyecek. Ama bu öneri karşısına çıkarsa kullanır.

---

### Adım 12 — Önizleme, Arama Durumu, Yayınlama

#### 1. Kart önizlemesi

Keşfet kartıyla **aynı bileşen** — işverenin göreceği birebir hal.

Gizli alanlar burada da görünmez: doğum tarihi, cinsiyet, telefon, e-posta.

> Aday neyin paylaşıldığını, neyin paylaşılmadığını kendi gözüyle görür. Güven açısından önemli.

#### 2. Profil tamamlanma yüzdesi

Yüzde + eksik alan listesi. Her eksiğin yanında **"düzenle" bağlantısı**, ilgili adıma götürür.

#### 3. Arama özeti

**Bu bölümün en faydalı parçası.** Filtrelerin ne yaptığı düz Türkçeyle:

> *"Bilişim alanında, İstanbul ve Ankara'da, Türkçe iş dilinde, tam zamanlı iş ilanları size gösterilecek."*

> **Neden kritik:** Katı filtreler **sessizce** eliyor. Aday yanlışlıkla tek şehir seçmiş, iş dilinden Türkçe'yi kaldırmış veya sürücü belgesi alanını boş bırakmışsa — *"hiç ilan gelmiyor"* der ve sebebini asla bulamaz. Burada bir bakışta görür.

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

> Kayıp, kazançtan daha çok harekete geçirir. İkinci mesaj daha güçlü motivasyon sağlar.

**Uygulama detayları:**
- **"+ Yeteneklerini ekle" satırları tıklanabilir** olsun, doğrudan o adıma götürsün
- **Gösterge dolarken anında değişsin** — aday yetenek ekledi → "Orta ●●○" oldu. Anlık geri bildirim, oyunlaştırmanın en basit ve etkili hali
- Uyarı metninde **"bu şartları isteyen"** ifadesi kritik — aday tüm ilanları kaybettiğini sanmasın

#### 5. Arama durumu

Yayınla butonunun **hemen üstünde** sorulur:

| Durum | Anlamı | Sistemde etkisi |
|---|---|---|
| 🟢 **Aktif arıyorum** | Hemen başlayabilirim | Rozet görünür, sıralamada hafif avantaj |
| 🟡 **Fırsatlara açığım** | Çalışıyorum ama doğru teklife bakarım | Normal görünür — **varsayılan** |
| ⚪ **Şu an bakmıyorum** | Pasif | Keşfet'te hiç görünmez, davet gelmez |

**Varsayılan "Fırsatlara açığım"** — ilk kez profil açan için güvenli orta seçenek. "Aktif arıyorum" bilinçli işaretleme olsun ki rozet değer taşısın.

**"Şu an bakmıyorum" seçilince:**
- Keşfet'te görünmez, davet gelmez
- **Mevcut eşleşmeler ve sohbetler devam eder** — yarıda kesilmesin
- Profil ana ekranında uyarı: *"Profilin gizli, işverenler seni göremiyor"*

> Son madde kritik — aday unutup *"neden kimse bakmıyor"* demesin.

**İki yerde erişilebilir:**

1. **Adım 12'de** (ilk kurulum)
2. **Profil ana ekranında, en üstte** — tek tıklamayla değiştirilebilir

```
🟢 Fırsatlara açık  ›
```

> **Neden iki yer:** Durum en sık değişen şey. İş buldu → "bakmıyorum". İki ay sonra → "aktif arıyorum". Bunu her seferinde 12 adım arasında aratmak kötü tasarım.

**Otomatik bakım:** 30-45 gün hiç giriş yapmamış "aktif arıyorum" profiline bildirim gitsin — *"Hâlâ iş arıyor musun?"* Cevap yoksa **otomatik "fırsatlara açığım"a düşsün**.

> **Neden gerekli:** Havuz ölü profillerle dolarsa işveren davet atar, cevap alamaz ve sisteme güveni gider. **Bu, ürünü sessizce öldüren şeylerden biri.**

#### 6. Yayınla butonu

Basınca profil aktif olur, arka planda embedding üretilir ve uygun ilanlar hesaplanır.

---

## 4. Yayına Girme Eşiği

**Zorunlu 4 adım:**

| Adım | Neden zorunlu |
|---|---|
| **1 — Temel Bilgiler** | Konum ve çalışma şekli katı filtreler; olmadan eşleştirme yapılamaz |
| **2 — Meslek Alanı, Unvan, Ne Arıyor** | Kaba filtrenin tamamı burada |
| **3 — Eğitim** | Eğitim seviyesi katı filtre + bölüm adı embedding'e girecek tek garantili metin |
| **6 — İş dili** | Boş kalırsa aday **hiçbir ilanla eşleşmez** |

**Opsiyonel:** Deneyim, yetenekler, sertifikalar, projeler, CV, diğer bilgiler, beklentiler

> **Eğitimi zorunlu tutmanın yan faydası:** Eğitim seviyesi katı filtre. Zorunlu olmasa, eğitim kaydı olmayan aday *"en az lisans"* isteyen ilanlarda elenir ve sebebini bilmez. Zorunlu olunca o filtre her zaman anlamlı çalışır.

> **Kapı kapanmıyor:** Eğitim kaydında en düşük tür "İlkokul" ve orada sadece mezuniyet yılı isteniyor. Yani her aday bir kayıt girebilir.

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

Eşleşme çalışır ama skorlar birbirine yakın çıkar, sıralama anlamsızlaşır.

> 💬 **Ekip tartışması için — en önemli açık soru:** Yetenekleri (en az 3) eşiğe eklemek, adayın kendi yararına olabilir. Üç yetenek yazmak 30 saniyelik iş, eşleştirme kalitesine katkısı büyük. Karşı argüman: her zorunlu alan tamamlama oranını düşürür.
>
> **Öneri:** İlk sürümde eşiğe eklemeyin, ama **tamamlama oranı ve eşleşme kalitesini ölçün**. Yeteneksiz profillerin aksiyon oranı belirgin şekilde düşükse eşiğe eklenir.

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

### Katı filtreye giren alanlar

| Alan | Adım |
|---|:---:|
| Çalışmak istediği şehirler | 1 |
| Çalışma şekli | 1 |
| Meslek alanı (ana + ikincil) | 2 |
| Ne arıyor (iş/staj) | 2 |
| Eğitim seviyesi (en yüksek derece) | 3 |
| İş dili | 6 |
| Sürücü belgesi, seyahat, vardiya | 10 |
| Gizlenecek şirketler | 11 |

### Sadece skora giren

| Alan | Bileşen |
|---|---|
| Yetenekler (yıl dahil) | `yetenek_ortusmesi` %15 |
| Toplam tecrübe (otomatik hesaplanan) | `tecrube_uyumu` %10 |
| Profil tamamlanma | `profil_kalitesi` %5 |

### Hiçbir yere girmeyen (sadece gösterim)

Fotoğraf · Doğum tarihi · Cinsiyet · Yaşadığı şehir · Bildiği diller + seviye · Aktif araç sahibi · Askerlik durumu · Başlama zamanı · Not ortalaması · CV · Link'ler

### İki altın kural

1. **Sayısal alanlar embedding'e girmez.** Yıl, maaş, not, tarih — hepsi aritmetikle hesaplanır.
2. **Katı filtre sadece gerçekten esnemeyen şeyler için.** Yetenek, tecrübe, bölüm, sertifika skorda ağırlık taşır.

---

## 6. Yayına Çıkmadan Önce Hazırlanması Gerekenler

Bu bölüm çalışmaz durumda kalırsa sebebi büyük olasılıkla aşağıdakilerden biri:

| Veri | Miktar | Kaynak |
|---|---|---|
| Meslek alanları | 15-20 | İŞKUR TMS / ISCO-08'den sadeleştirilecek |
| Unvanlar | 300-500 (alan başına 20-40) | Aynı kaynak |
| Yetenekler | 200-300 | Hedef meslek alanlarından elle |
| Üniversiteler | ~200 | YÖK listesi |
| İl / ilçe listesi | Tam | Resmi liste |
| Diller | 20-30 | Elle |
| Eğitim dereceleri | 7 | Sabit |

> ⚠️ **Boş öneri havuzu, bu bölümün en büyük sessiz riski.** İlk 50 kullanıcı boş liste görürse herkes farklı yazar, veri dağılır ve öneri sistemi bir daha kendine gelemez. Bu veriler **kod yazılmadan önce** hazırlanmalı.

---

## 7. Açık Sorular — Ekip Tartışması İçin

| # | Soru | Öneri |
|:---:|---|---|
| 1 | Yetenekler eşiğe eklenecek mi? | İlk sürümde hayır, ölç ve karar ver |
| 2 | Cinsiyet alanı hiç tutulmasın mı? | Tutmamak daha güvenli; tek gerekçesi demografik istatistik |
| 3 | İşveren CV indirebilsin mi? | Şu an evet; KVKK "gerçek silme" açısından tartışmalı |
| 4 | "Ne yaptın" metni zorunlu kalsın mı? | Evet, ama deneyim ekleme oranı ölçülmeli |
| 5 | İkincil meslek alanı filtre gücünü ne kadar düşürüyor? | Gerçek veriyle ölçülecek |
| 6 | Doğum tarihi işverene gösterilsin mi? | Şu an hayır; karar sonraya bırakıldı |
| 7 | Profil tamamlanma yüzdesi hangi alanlara kaç puan verecek? | Belirlenmedi |
| 8 | Skor ağırlıklarının nihai değerleri | Veritabanında tutulacak, gerçek veriyle ayarlanacak |

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
| KVKK | Bekliyor |

**Eşleştirme motorunda konuşulacaklar:** filtre zincirinin sırası, skor formülleri, iki yön ayrı skor, arka plan işinin tetikleyicileri, `pg_cron` düzeni, boş sonuç senaryoları.
