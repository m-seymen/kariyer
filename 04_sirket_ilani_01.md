# İş Eşleştirme Projesi — İlan Oluşturma

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak

---

## Bu Sürümde Düzeltilenler

Sonradan verilen kararlara göre önceki sürümden farklı olan noktalar:

| Konu | Eski | Yeni |
|---|---|---|
| Terminoloji | "Kategori" | **"Meslek Alanı"** *(şirket sektörüyle karışmaması için)* |
| Konum | `lokasyon_id` → il/ilçe | **`konumlar` ağacı** + `konum_yolu` + `ulke_kodu` |
| Askerlik | Katı filtre | **Sadece bilgi** — filtreden çıkarıldı |
| Maaş (aday tarafı) | Profilde min-max | **Profilden kaldırıldı**, başvuru anında tek rakam |
| Maaş (ilan tarafı) | Sadece tutar | **Para birimi + net/brüt** eklendi |
| Unvan kaynağı | İŞKUR TMS | **ISCO-08** *(global standart)* |

---

## İki İlan Tipi

İşveren panelinde **iki ayrı buton**: "İş İlanı Aç" ve "Staj İlanı Aç".

Tıklanan butona göre `ilan_tipi` otomatik set edilir (`'is'` | `'staj'`) ve o tipin formu açılır. Kullanıcı bu kolonu hiç görmez, sadece iki farklı ekran görür.

**Tek tablo, tek eşleştirme motoru.** Ortak alanlar (meslek alanı, unvan, lokasyon, yetenekler, iş tanımı, yayın) ikisinde de aynıdır; yalnızca birkaç alan farklılaşır.

```sql
ilanlar (
  id, sirket_id, ilan_tipi,        -- 'is' | 'staj'
  meslek_alani_id, unvan_id,
  lokasyon_id,                     -- sirket_lokasyonlari referansı
  konum_yolu,                      -- lokasyondan kopyalanır (filtre için)
  ulke_kodu,                       -- lokasyondan kopyalanır (en çok eleyen filtre)
  calisma_sekli text[],            -- dizi, kesişim kontrolü
  is_dilleri text[],               -- dizi, kesişim kontrolü
  ...

  -- sadece iş ilanında dolu:
  calisma_tipi, min_tecrube, max_tecrube,
  maas_min, maas_max, para_birimi, maas_net_mi,

  -- sadece staj ilanında dolu:
  staj_suresi, staj_calisma_sekli, staj_ucret_tipi,
  staj_ucret_tutari, zorunlu_staj_mi
)
```

### Farklılaşan alanlar

| | İş ilanı | Staj ilanı |
|---|---|---|
| Çalışma tipi | Tam zamanlı / yarı zamanlı / proje bazlı / dönemsel | **Sorulmaz** |
| Staj çalışma şekli | — | Tam zamanlı / yarı zamanlı |
| Tecrübe | Min-max yıl | Sorulmaz |
| Ücret | Maaş aralığı + para birimi + net/brüt | Ücretli / harçlık / ücretsiz |
| Süre | — | 1 ay / 2 ay / 3 ay / yaz dönemi / dönem boyunca |
| Zorunlu staj | — | Zorunlu / gönüllü (okul evrakı gerekiyor mu) |
| Eğitim | Opsiyonel seviye | Devam eden öğrenci şartı |
| Adım 3 başlığı | İş Tanımı | Staj Tanımı |

> **"Staj" çalışma tipi listesinden çıkarıldı.** `ilan_tipi` zaten staj olduğunu söylüyor; tekrar sormak çelişki riski yaratır.

> **Global not:** Staj kavramı ülkeye göre farklı hukuki statüde (Almanya'da *Praktikum*, ABD'de *internship*/*co-op*, Fransa'da *stage* + zorunlu *convention de stage*). "Zorunlu staj / okul evrakı" alanı bazı ülkelerde anlamsız, bazılarında yasal zorunluluk. **Öneri:** alan kalsın ama ülkeye göre gösterilsin/gizlensin (`ulke_form_alanlari` tablosu).

**Aday tarafında karşılığı:** Profilde tercih — "iş arıyorum" / "staj arıyorum" / "ikisi de" (çoklu seçim). Son sınıf öğrencisi ikisine birden bakmak isteyebilir.

`ilan_tipi` katı filtreye girer.

---

## Her Alan Nereye Gidiyor

| Alan | Katı filtre | Embedding | Skor |
|---|:---:|:---:|:---:|
| **Ülke kodu** | ✅ | — | — |
| İlan tipi (iş/staj) | ✅ | — | — |
| Meslek alanı | ✅ | — | — |
| Unvan | ❌ | ✅ | — |
| Lokasyon (`konum_yolu`) | ✅ | ❌ | ✅ |
| Çalışma şekli | ✅ | ❌ | — |
| Çalışma tipi | ✅ | ❌ | — |
| Yetenekler (+ yıl, zorunlu/tercihen) | ❌ | ✅ *(sadece ad)* | ✅ |
| Toplam tecrübe (min-max) | ❌ | ❌ | ✅ |
| Eğitim seviyesi | ✅ *(doluysa)* | ❌ | — |
| Tercih edilen bölüm | ❌ | ✅ | ✅ |
| İş dili | ✅ | ❌ | — |
| Diğer şartlar — hazır kutular | ✅ *(doluysa)* | ❌ | ❌ |
| Diğer şartlar — sertifikalar | ❌ | ✅ *(sadece ad)* | ✅ |
| Diğer şartlar — serbest metin | ❌ | ❌ | ❌ |
| **İş tanımı (Adım 3)** | ❌ | ✅ | — |
| Maaş aralığı | ❌ | ❌ | ✅ *(boşsa hafif ceza)* |

**Üç kural:**

1. **Sayısal alanlar embedding'e girmez.** "3 yıl" ile "5 yıl" vektör uzayında neredeyse aynı görünür. Tecrübe, maaş, yıl: hepsi aritmetikle hesaplanır.
2. **Katı filtre sadece gerçekten esnemeyen şeyler için.** Yetenek, tecrübe, bölüm ve sertifika skorda ağırlık taşır — kapıda bekçilik yapmaz.
3. **Ülke kodu ilk filtredir.** Global ölçekte en çok eleyen kademe; sorgu buradan başlar.

### Filtre sırası — global ölçek için kritik

```
1. ulke_kodu eşitliği      → 5M → 200K
2. meslek_alani_id         → 200K → 8K
3. ilan_tipi, dil, şekil   → 8K → 3K
4. konum_yolu LIKE         → 3K → 800
5. koşullu filtreler       → 800 → 500
6. vektör karşılaştırması  → 500 → 200
```

Vektör hesabı en sona kalır ve sadece 500 kayıt üzerinde çalışır.

---

## Adım 1 — Pozisyon ve Temel Bilgiler

| Alan | Zorunlu | Giriş şekli |
|---|:---:|---|
| Meslek alanı | ✅ | Kapalı liste |
| Unvan | ✅ | Serbest yazma + öneri |
| Lokasyon | ✅ | Şirket lokasyonlarından seçim |
| Çalışma şekli | ✅ | Ofis / hibrit / uzaktan — çoklu |
| Çalışma tipi *(iş ilanı)* | ✅ | Tam zamanlı / yarı zamanlı / proje bazlı / dönemsel |
| Açık pozisyon sayısı | ✅ | Varsayılan 1 |

### Meslek alanı ve unvan

**Terminoloji:** Şirket profilinde "Sektör", ilan ve adayda **"Meslek Alanı"**.

> **Neden ayrı isim:** Sektör = şirket ne iş yapıyor (Gıda, İnşaat). Meslek alanı = kişi ne iş yapıyor (Bilişim, Muhasebe). Bir inşaat şirketi muhasebeci arayabilir — eşleşen şey meslek, sektör değil. Aynı ismi kullanmak kullanıcıyı karıştırır.

**Akış:**
1. Meslek alanı seçilir (kapalı liste) → "Bilişim"
2. Unvan kutusuna yazılmaya başlanır → o alandaki unvanlar öneri olarak düşer
3. Bulursa tıklar, bulamazsa elle yazar
4. Elle yazılan unvan kaydedilir, sonraki kullanıcılara öneri olarak çıkar

**Öneri listesi meslek alanına bağlı** — Bilişim seçene muhasebe unvanları düşmez.

```sql
meslek_alanlari (id, kod, sira, aktif_mi)
meslek_alani_cevirileri (meslek_alani_id, dil_kodu, ad)

unvanlar (id, ad, normalize_ad, meslek_alani_id,
          dil_kodu, kaynak, kullanim_sayisi)
-- kaynak: resmi_liste | kullanici_girdi
```

| | Neden |
|---|---|
| **Meslek alanı kapalı** | Kaba filtre buna bağlı. İnşaat adayı bilişim ilanına düşmemeli. |
| **Unvan serbest** | "Yazılım Mühendisi" ile "Backend Developer" farklı isim, aynı iş. Katı filtre olursa doğru adaylar elenir. |

Unvan sadece **gösterimde** ve **embedding'de** kullanılır.

> **Global not — meslek alanı çevirisi:** Kod sabit (`bilisim`), gösterilen ad kullanıcının arayüz diline göre gelir. **Tek liste tüm ülkelerde kullanılır** — ülke bazlı meslek alanı listesi tutulmaz, yoksa aynı iş farklı ülkelerde farklı kategoriye düşer ve filtre bozulur.

> **Global not — unvan dili:** Unvan serbest yazıldığı için her dilde kayıt birikecek. `dil_kodu` kolonu öneri havuzunu süzmek için gerekli — Türk işverene Japonca unvan önerisi düşmemeli. Ama **embedding çok dilli olduğu için** "Backend Developer" ile "Backend Geliştirici" yine eşleşir; dil ayrımı sadece öneri kalitesi için.

**Başlangıç verisi şart:** 300-500 unvan (alan başına 20-40), **lansman yapılacak her dil için**. Kaynak: **ISCO-08** (BM'nin uluslararası meslek sınıflandırması, çok dilli), sadeleştirilerek.

> Boş öneri listesiyle karşılaşan ilk kullanıcılar herkes farklı yazar, veri dağılır ve öneri sistemi hiç kendine gelemez.

**Aynı bileşen aday profilinde de kullanılır** — tek kod, iki yer.

### Lokasyon

Sadece şirket profilinde eklenir, ilanda mevcut listeden seçilir.

Seçim yapıldığında `konum_yolu` ve `ulke_kodu` şirket lokasyonundan **kopyalanır** — her filtrede JOIN yapmak yerine tek `LIKE` ve tek eşitlik karşılaştırması yapılır.

**Konum yapısı:** `konumlar` ağacı, her ülke kendi derinliğini taşır (ABD'de eyalet → şehir, Türkiye'de il → ilçe, Singapur'da tek kademe). Detay: `sirket-profili.md`.

**Filtre:**
```sql
i.konum_yolu LIKE ak.konum_yolu || '%'
```

| Aday seçimi | İlan konumu | Eşleşir |
|---|---|:---:|
| `TR` | `TR/İstanbul/Kadıköy` | ✅ |
| `TR/İstanbul` | `TR/Ankara/Çankaya` | ❌ |
| `US/California` | `US/California/San Francisco` | ✅ |
| `US/California` | `US/Texas/Austin` | ❌ |

**Arayüz notu:** Listede yoksa "Şirket profilinden lokasyon ekleyin" bağlantısı koyun. Döndüğünde ilan taslağı kaybolmamalı.

### Çalışma şekli — özel durum

**Dizi tipi, kesişim kontrolü** (`&&` operatörü + GIN indeksi).

**"Uzaktan" kesişimde varsa konum filtresi atlanır.**

```
ortak = aday.calisma_sekli ∩ ilan.calisma_sekli

if ortak boş                      → ELENİR
if aday."her konumda hazır"       → konum ATLANIR
if "uzaktan" ∈ ortak              → konum ATLANIR
else                              → konum UYGULANIR
```

> Trabzon'daki aday, İstanbul merkezli uzaktan ilana düşebilmeli. Global ölçekte bu daha da önemli: uzaktan ilanlar coğrafyadan bağımsız, en değerli eşleşmeler orada.

> **Hibrit konum gerektirir** (haftada 2 gün ofiste). Sadece tam uzaktanda konum atlanır.

### Açık pozisyon sayısı

Zorunlu, varsayılan 1.

**İleriye dönük:** Kota buna bağlanabilir — 5 kişi arayan ilanın günlük davet kotası daha yüksek olabilir. Gelir modeli geldiğinde doğal fiyatlama kriteri.

---

## Adım 2 — Aranan Nitelikler

### Yetenekler

Her yetenek için üç bilgi:

| | |
|---|---|
| Yetenek adı | Serbest yazma + öneri (kullanım sayısına göre sıralı) |
| İstenen yıl | Sayı |
| Zorunlu / tercihen | İşaret |

**Sınırlar:**

| | |
|---|---|
| Toplam yetenek | **En fazla 30** |
| Zorunlu yetenek | **Sınır yok** — 6'yı aşınca yumuşak uyarı |

Uyarı metni:

> *"Çok fazla zorunlu yetenek eşleşmeyi zorlaştırır. Gerçekten olmazsa olmazları zorunlu, diğerlerini tercihen işaretleyin."*

Engellemez, sadece bilgilendirir.

> **Neden uyarı gerekiyor:** Katı filtre olmadığı için kimse elenmez, ama 20 zorunlu yetenek `yetenek_ortusmesi` hesabını anlamsızlaştırır — hiçbir aday hepsine sahip olmaz, hepsi benzer düşük puan alır ve o bileşen adayları ayıramaz hale gelir. Sınırsızlık en çok işverenin kendi eşleştirmesini bozar.

**Aynı 30 sınırı aday profilinde de geçerli** — simetri sağlar, profil metni şişmez.

**Skorda nasıl çalışır** — `yetenek_ortusmesi`, %15 ağırlık:

- Zorunlu yetenek eksikse → skor ciddi düşer, **aday listeden atılmaz**
- Tercihen yetenek varsa → bonus, yoksa ceza yok
- Yıl farkı: istenen 3 / adayda 4 = tam, 1 = kısmi

**İki koldan eşleşir:**
1. **Etiket kesişimi** — aynı `yetenek_id` varsa doğrudan
2. **Embedding** — etiket tutmasa bile "React" ile "frontend geliştirme" yakınlığı yakalanır

> İkinci kol sayesinde serbest yazmanın dağınıklığı (ReactJS / React.js / Reakt) eşleştirmeyi bozmaz. **Global ölçekte bu daha da değerli:** "Buchhaltung" ile "muhasebe" aynı vektör uzayında yakın çıkıyor, çeviri gerekmiyor.

**Sadece yetenek adı embedding'e girer, yıl girmez.**

**Adaya gösterim:** Kartta "Zorunlu: React (var), Node.js (yok)" özeti dursun. LLM olmadan gerekçe gösterebildiğiniz tek yer burası.

### Toplam tecrübe *(sadece iş ilanı)*

Min-max yıl. Katı filtre değil, embedding'e girmez, sadece skorda (`tecrube_uyumu`, %10).

```
İlan: 3-6 yıl
Aday 4 yıl   → tam puan
Aday 2 yıl   → kısmi
Aday 10 yıl  → kısmi (fazla nitelikli)
```

### Eğitim seviyesi

**Opsiyonel.** Doluysa katı filtre, boşsa hesaba katılmaz.

> Birçok pozisyonda eğitim şartı yok. Zorunlu yapılırsa gereksiz eleme olur.

*Staj ilanında bunun yerine "devam eden öğrenci" şartı gelir.*

> **Global not:** Eğitim dereceleri ülkeler arasında birebir örtüşmüyor (Almanya'da *Ausbildung*, ABD'de *Associate Degree*, Türkiye'de ön lisans). **Öneri:** ISCED seviyelerine (BM standardı, 0-8) eşleyin; kullanıcıya kendi ülkesinin adıyla gösterin, karşılaştırmayı ISCED numarasıyla yapın. Yoksa "en az lisans" filtresi ülkeler arası çalışmaz.

### Tercih edilen bölüm

**Opsiyonel**, serbest yazma + öneri. **Hiçbir zaman katı filtre değil** — doluysa skorda ağırlık.

> "Bilgisayar Mühendisliği mezunu" şartı, bootcamp'ten gelmiş 5 yıl tecrübeli adayı kaybetmemeli.

### İş dili

Çoklu seçim (dizi tipi), **katı filtre**. Kesişim kontrolü — en az bir ortak dil varsa geçer.

> ⚠️ **Global ölçekte en büyük tek risk:** Bu alan yanlış doldurulursa eşleşme tamamen ölür. Varsayılan olarak **şirketin ülkesinin ana dili** işaretli gelsin. Çok uluslu ekiplerde İngilizce de eklenir.

### Diğer Şartlar

Üç parça, üçü farklı davranır.

#### 1. Hazır kutular — KATI FİLTRE

Kapalı liste, evet/hayır işaretleri:

- **Sürücü belgesi** *(seçilirse sınıf)*
- Aktif araç sahibi olmak
- Seyahat engeli olmaması
- Vardiyalı çalışabilmek

**Doluysa katı filtre olarak uygulanır**, boşsa hesaba katılmaz.
Embedding'e ve skora girmez.

> "B sınıfı ehliyet zorunlu" yazan kurye ilanına ehliyetsiz aday düşmemeli — bunlar gerçekten esnemeyen şartlardır.

Aday profilinde de karşılığı olmalı, yoksa filtre boşta kalır.

> ✅ **Askerlik durumu filtreden çıkarıldı.** Artık **sadece bilgi amaçlı** — işveren kartta görür, süzemez.
>
> **Neden:** Filtre yapılırsa kadın adaylar için bu alan boş olduğundan sürekli istisna kuralı gerekir; o kural bir yerde unutulursa tüm kadın adaylar elenir. Hukuki risk var (Türkiye'de İş Kanunu md. 5, AB'de istihdam eşitliği direktifleri, ABD'de Title VII). Filtre dışında tutmak hem güvenli hem basit — istisna kuralına gerek kalmadı, sistem sadeleşti.

> **Global not — sürücü belgesi sınıfları:** Sınıflandırma ülkeye göre farklı (AB'de A/B/C/D/E, ABD'de eyalet bazlı Class A/B/C + CDL). Sınıf listesi `ulke_surucu_siniflari` tablosunda tutulmalı, koda gömülmemeli. Ülkeler arası karşılaştırma zor — **öneri:** sınıf eşleşmesini sadece aynı ülke içinde uygulayın, ülkeler arası ilanlarda "ehliyet var/yok" düzeyinde kalın.

#### 2. Sertifika ve belgeler — skorda ağırlık

Serbest yazma + öneri. Her biri için zorunlu/tercihen işareti. **Katı filtreye girmez.**

> **Neden:** İsimler tutmaz. "PMP" / "PMP Sertifikası" / "Project Management Professional" üç ayrı kayıt olur; katı filtre yapılırsa sertifikası olan aday bile elenir.

- Zorunlu işaretli → eksikse skor ciddi düşer, aday listede kalır
- Tercihen işaretli → varsa bonus, yoksa ceza yok

**Sadece sertifika adı embedding'e girer** (veren kurum ve tarih girmez) — "PMP" ile "Proje Yönetimi Sertifikası" vektör uzayında yakın çıkar, isim dağınıklığı doğal olarak çözülür.

> **Global not:** Bazı sertifikalar uluslararası geçerli (PMP, AWS, CFA), bazıları tamamen yerel (Türkiye'de İSG uzmanlığı, SMMM). Serbest yazma bu farkı kendiliğinden çözüyor — ayrı bir yapı gerekmiyor.

#### 3. Serbest şartlar — sadece gösterim

Diğer ikisine girmeyen şeyler için metin alanı. Filtreye de embedding'e de skora da girmez. Aday okur, o kadar.

---

## Adım 3 — İş Tanımı

*(Staj ilanında başlık: **Staj Tanımı**)*

### Neden bu adım en kritik

Şirket profili embedding'e girmiyor. Sayısal alanlar girmiyor. Filtreler girmiyor.

Geriye eşleştirmenin dayanacağı **iki şey** kalıyor: **yetenek adları** ve **bu metin**. Yetenekler sadece etiket — birkaç kelime. Asıl zengin metin burada üretiliyor.

> İşveren üç satır yazarsa vektör zayıf olur, eşleştirme kötü çalışır ve "sistem işe yaramıyor" der. Halbuki sorun sistemde değil, girdide.

### Neden tek kutu değil

Boş bir "iş tanımını yazın" kutusuna klişe gelir *("dinamik ekibimize takım arkadaşı arıyoruz")*. Somut soruya gerçek cevap gelir.

### İş ilanı soruları

| # | Soru | Zorunlu |
|:---:|---|:---:|
| 1 | **Bu pozisyonda ne yapılacak?** Günlük iş, projeler, kullanılan araçlar | ✅ |
| 2 | **Ekip yapısı nasıl?** Kaç kişi, kime bağlı, kimlerle çalışacak | ✅ |
| 3 | **Aradığınız kişi nasıl biri?** Teknik olmayan beklentiler, çalışma tarzı | ✅ |
| 4 | **İlk 3 ayda ne başarmış olmalı?** | ❌ |

> Şirket profilinde sadece ilk soru zorunluydu. **Burada ilk üçü zorunlu**, çünkü eşleştirme buna dayanıyor.

4. soru opsiyonel ama çok güçlü — hem aday için netlik verir hem embedding için en zengin metni üretir.

### Staj tanımı soruları

| # | Soru | Zorunlu |
|:---:|---|:---:|
| 1 | **Stajyer ne yapacak, hangi projelerde olacak?** | ✅ |
| 2 | **Kim mentorluk yapacak, ekip yapısı nasıl?** | ✅ |
| 3 | **Nasıl bir stajyer arıyorsunuz?** | ✅ |
| 4 | **Staj sonunda ne öğrenmiş olacak?** | ❌ |

### Uygulama detayları

- **Yer tutucu örnek metin** her soruda — ne yazılacağını gösterir, cevap kalitesini gözle görülür yükseltir
- **Karakter sınırı:** soru başına 500-800
- **Minimum uzunluk uyarısı:** birkaç kelime yazıp geçilmesin
- **Soru başlıkları ve yer tutucular çevrilebilir** — arayüz diline göre gelir

**Tamamı embedding'e girer.**

### İlan metninin dili

İşveren hangi dilde yazarsa o dilde saklanır ve adaya **olduğu gibi** gösterilir.

> **Çok dilli embedding sayesinde eşleştirme dilden bağımsız çalışıyor** — Türkçe yazılmış bir profil, İngilizce yazılmış bir ilanla eşleşir. Ama **aday o ilanı okuyamayabilir.** Bu yüzden **iş dili filtresi** (Adım 2) kritik: dil filtresi geçmeyen ilan adaya hiç gösterilmez, böylece okuyamayacağı ilanla karşılaşmaz.

> 💡 **İleriye dönük öneri:** İlan metnine makine çevirisi eklemek, uzaktan çalışma pazarında ciddi bir avantaj olur. MVP'de gerekmez.

---

## Adım 4 — Şartlar ve Yayın

| Alan | Zorunlu | Not |
|---|:---:|---|
| Maaş aralığı *(iş ilanı)* | ❌ | **Min-max iki rakam** + para birimi + net/brüt |
| Staj ücreti *(staj ilanı)* | ✅ | Ücretli / harçlık / ücretsiz — ücretliyse tutar + para birimi |
| Başlama zamanı | ❌ | Hemen / 1 ay içinde / esnek |
| Yayın süresi | ✅ | 30 / 60 / 90 gün |
| Günlük davet kotası | — | İşveren girmez, plana göre otomatik |

### Maaş aralığı

**Opsiyonel.** Girilmezse "Belirtilmedi".

**Üç bileşen birlikte alınır:**

```
Maaş aralığı: [____] – [____]
Para birimi:  [TRY ▾]        ← şirketin ülkesine göre varsayılan
              ○ Net  ● Brüt
```

| | |
|---|---|
| **Doluysa** | Kartta gösterilir, skorda avantaj |
| **Boşsa** | "Belirtilmedi" + skorda hafif ceza |

> **Maaş katı filtreden çıkarıldı.** Aday tarafında maaş beklentisi profilde sorulmuyor — başvuru anında soruluyor. Karşılaştırılacak iki taraf olmadığı için filtre de yok.

**Neden aday profilinde sorulmuyor:** Maaş beklentisi **pozisyona özeldir**. Aynı aday Senior Developer için farklı, Team Lead için farklı bekler. Profilde tek rakam istemek adayı en alt sınırını yazmaya zorlar.

**Başvuru anında sorulan:**

```
Net minimum maaş beklentiniz (opsiyonel)
[____] [TRY ▾]

Not (opsiyonel)
[_________________________]
```

Tek rakam, net üzerinden, minimum olduğu açık. İşveren başvuranlar listesinde kolon olarak görür, sıralayabilir.

**Neden ilan tarafında zorunlu değil:** Çoğu ülkede işverenler maaş yazmak istemiyor. Zorunlu yapılırsa ya uydurma aralık girerler ya ilan açmaktan vazgeçerler.

> ⚠️ **Global uyarı — maaş şeffaflığı yasaları:** Bazı yerlerde maaş aralığı yazmak **yasal zorunluluk**: AB Maaş Şeffaflığı Direktifi (2026'ya kadar üye ülkelerde yürürlüğe giriyor), ABD'de Kaliforniya, Colorado, New York gibi eyaletler. Bu ülkelerde maaş alanı **zorunlu** olmalı.
>
> **Öneri:** `ulke_form_alanlari` tablosunda maaşın zorunluluğu ülke bazlı tutulsun. Şirketin ülkesi maaş şeffaflığı gerektiriyorsa alan zorunlu gelsin. Bu hem hukuki uyum hem ürün kalitesi getirir.

> 💡 **Beklenmeyen fayda:** Aday, maaş yazmayan ilana başvururken ne yazacağını bilemez — bu işvereni maaş yazmaya iter.

**Para birimi kuralları:**
- Varsayılan: şirketin ülkesinin para birimi
- Adaya **kendi tercih ettiği para biriminde yaklaşık karşılığı** gösterilebilir *(ileride, kur servisi gerekir)*
- **Maaş değerleri kur çevrimi yapılmadan saklanır** — kur değişince geçmiş veriler bozulmasın

### Yayın süresi

30 / 60 / 90 gün. Süre dolunca **ilan otomatik kapanır**. İşverene önceden hatırlatma gider:

> *"İlanınız yarın kapanıyor, uzatmak ister misiniz?"*

**Neden gerekli:** Kapanmayan ilan birikir, aday dolu pozisyona başvurur ve güven gider.

> **Global not:** Hatırlatma bildirimi kullanıcının **saat dilimine** göre gönderilmeli — gece 3'te bildirim gitmesin.

### Günlük davet kotası

İşveren girmez, plana göre otomatik gelir. Panelde görünür:

> *"Bugün 7/10 davet kullanıldı"*

Kota sıfırlaması kullanıcının saat dilimine göre değil, **UTC gün başına** göre olsun — yoksa saat dilimi değiştirerek kota istismarı mümkün olur.

### Önizleme + Yayınla

İlanın **adaya nasıl görüneceği** gösterilir, işveren onaylar.

**Şirket onaysızsa burada kilit devreye girer:**

> *"Şirketiniz doğrulanmadan ilan yayınlanamaz"* + belge yükleme bağlantısı

### Taslak kaydetme

**Her adımda** taslak kaydedilir, yarıda bırakılıp dönülebilir.

---

## Yayına Çıkmadan Önce Hazırlanması Gerekenler

| Veri | Miktar | Kaynak |
|---|---|---|
| Meslek alanları | 15-20 + çevirileri | ISCO-08'den sadeleştirilecek |
| Unvanlar | 300-500 × **her lansman dili** | ISCO-08 (çok dilli) |
| Yetenekler | 200-300 × **her lansman dili** | Elle |
| Eğitim seviyeleri | ISCED eşlemesi + ülke adları | ISCED (BM) |
| Sürücü belgesi sınıfları | Ülke başına | Elle |
| Para birimleri | ISO 4217 | Standart liste |
| Ülke form kuralları | Lansman ülkeleri için | Elle + hukuki danışma |

> ⚠️ **Her yeni ülke/dil açmak bir veri hazırlık işidir** — sadece arayüz çevirisi değil: unvan havuzu, yetenek havuzu, belge listesi, eğitim eşlemesi, maaş zorunluluğu kuralı.

---

## Açık Kalanlar

- Meslek alanı listesinin nihai içeriği (15-20 kalem) ve çevirileri
- Unvan başlangıç verisi (alan başına 20-40, her dil için)
- Skor ağırlıklarının nihai değerleri
- Maaş belirtmeme cezasının ağırlığı
- İlan moderasyonu: her ilan yayından önce admin onayından geçecek mi, yoksa sadece şirket onayı yeterli mi?
- **Hangi ülkelerle lansmana çıkılacak?** Form kuralları, veri hazırlığı ve hukuki uyum buna bağlı
- **Çalışma izni / vize sponsorluğu alanı** eklenecek mi? *(Aday tarafında "çalışma iznim var/yok", ilan tarafında "vize desteği veriyoruz" — global ölçekte gerçek bir katı filtre adayı)*
- Eğitim seviyesi ISCED eşlemesi kim tarafından yapılacak?
- Maaş şeffaflığı zorunluluğu olan ülkelerin listesi ve güncel tutulması
