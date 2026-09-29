# İş Eşleştirme Projesi — İlan Oluşturma

> Son güncelleme: 20 Eylül 2026

---

## İki İlan Tipi

İşveren panelinde **iki ayrı buton**: "İş İlanı Aç" ve "Staj İlanı Aç".

Tıklanan butona göre `ilan_tipi` otomatik set edilir (`'is'` | `'staj'`) ve o tipin formu açılır. Kullanıcı bu kolonu hiç görmez, sadece iki farklı ekran görür.

**Tek tablo, tek eşleştirme motoru.** Ortak alanlar (kategori, unvan, lokasyon, yetenekler, iş tanımı, yayın) ikisinde de aynıdır; yalnızca birkaç alan farklılaşır.

```sql
ilanlar (
  id, sirket_id, ilan_tipi,        -- 'is' | 'staj'
  kategori_id, unvan_id, lokasyon_id, calisma_sekli, ...

  -- sadece iş ilanında dolu:
  calisma_tipi, min_tecrube, max_tecrube,
  maas_min, maas_max,

  -- sadece staj ilanında dolu:
  staj_suresi, staj_calisma_sekli, staj_ucret_tipi, zorunlu_staj_mi
)
```

### Farklılaşan alanlar

| | İş ilanı | Staj ilanı |
|---|---|---|
| Çalışma tipi | Tam zamanlı / yarı zamanlı / proje bazlı / dönemsel | **Sorulmaz** |
| Staj çalışma şekli | — | Tam zamanlı / yarı zamanlı |
| Tecrübe | Min-max yıl | Sorulmaz |
| Ücret | Maaş aralığı | Ücretli / harçlık / ücretsiz |
| Süre | — | 1 ay / 2 ay / 3 ay / yaz dönemi / dönem boyunca |
| Zorunlu staj | — | Zorunlu / gönüllü (okul evrakı gerekiyor mu) |
| Eğitim | Opsiyonel seviye | Devam eden öğrenci şartı |
| Adım 3 başlığı | İş Tanımı | Staj Tanımı |

> **"Staj" çalışma tipi listesinden çıkarıldı.** `ilan_tipi` zaten staj olduğunu söylüyor; tekrar sormak çelişki riski yaratır.

**Aday tarafında karşılığı:** Profilde tercih — "iş arıyorum" / "staj arıyorum" / "ikisi de" (çoklu seçim). Son sınıf öğrencisi ikisine birden bakmak isteyebilir.

`ilan_tipi` katı filtreye girer.

---

## Her Alan Nereye Gidiyor

| Alan | Katı filtre | Embedding | Skor |
|---|:---:|:---:|:---:|
| İlan tipi (iş/staj) | ✅ | — | — |
| Kategori | ✅ | — | — |
| Unvan | ❌ | ✅ | — |
| Lokasyon | ✅ | ❌ | ✅ |
| Çalışma şekli | ✅ | ❌ | — |
| Çalışma tipi | ✅ | ❌ | — |
| Yetenekler (+ yıl, zorunlu/tercihen) | ❌ | ✅ | ✅ |
| Toplam tecrübe (min-max) | ❌ | ❌ | ✅ |
| Eğitim seviyesi | ✅ *(doluysa)* | ❌ | — |
| Tercih edilen bölüm | ❌ | ✅ | ✅ |
| İş dili | ✅ | ❌ | — |
| Diğer şartlar — hazır kutular | ✅ *(doluysa)* | ❌ | ❌ |
| Diğer şartlar — sertifikalar | ❌ | ✅ | ✅ |
| Diğer şartlar — serbest metin | ❌ | ❌ | ❌ |
| **İş tanımı (Adım 3)** | ❌ | ✅ | — |
| Maaş aralığı | ✅ *(doluysa)* | ❌ | ✅ *(boşsa hafif ceza)* |

**İki kural:**

1. **Sayısal alanlar embedding'e girmez.** "3 yıl" ile "5 yıl" vektör uzayında neredeyse aynı görünür. Tecrübe, maaş, yıl: hepsi aritmetikle hesaplanır.
2. **Katı filtre sadece gerçekten esnemeyen şeyler için.** Yetenek, tecrübe, bölüm ve sertifika skorda ağırlık taşır — kapıda bekçilik yapmaz.

---

## Adım 1 — Pozisyon ve Temel Bilgiler

| Alan | Zorunlu | Giriş şekli |
|---|:---:|---|
| Kategori | ✅ | Kapalı liste |
| Unvan | ✅ | Serbest yazma + öneri |
| Lokasyon | ✅ | Şirket lokasyonlarından seçim |
| Çalışma şekli | ✅ | Ofis / hibrit / uzaktan — çoklu |
| Çalışma tipi *(iş ilanı)* | ✅ | Tam zamanlı / yarı zamanlı / proje bazlı / dönemsel |
| Açık pozisyon sayısı | ✅ | Varsayılan 1 |

### Kategori ve unvan

**Akış:**
1. Kategori seçilir (kapalı liste) → "Bilişim"
2. Unvan kutusuna yazılmaya başlanır → o kategorideki unvanlar öneri olarak düşer
3. Bulursa tıklar, bulamazsa elle yazar
4. Elle yazılan unvan kaydedilir, sonraki kullanıcılara öneri olarak çıkar

**Öneri listesi kategoriye bağlı** — Bilişim seçene muhasebe unvanları düşmez.

```sql
kategoriler (id, ad, sira, aktif_mi)
unvanlar (id, ad, normalize_ad, kategori_id, kaynak, kullanim_sayisi)
-- kaynak: resmi_liste | kullanici_girdi
```

| | Neden |
|---|---|
| **Kategori kapalı** | Kaba filtre buna bağlı. İnşaat adayı bilişim ilanına düşmemeli. |
| **Unvan serbest** | "Yazılım Mühendisi" ile "Backend Developer" farklı isim, aynı iş. Katı filtre olursa doğru adaylar elenir. |

Unvan sadece **gösterimde** ve **embedding'de** kullanılır.

**Başlangıç verisi şart:** 300-500 unvan (kategori başına 20-40) elle girilmeli. Kaynak: İŞKUR Türk Meslekler Sözlüğü veya ISCO-08, sadeleştirilerek.

> Boş öneri listesiyle karşılaşan ilk kullanıcılar herkes farklı yazar, veri dağılır ve öneri sistemi hiç kendine gelemez.

**Aynı bileşen aday profilinde de kullanılır** — tek kod, iki yer.

### Lokasyon

Sadece şirket profilinde eklenir, ilanda mevcut listeden seçilir.

**Arayüz notu:** Listede yoksa "Şirket profilinden lokasyon ekleyin" bağlantısı koyun. Döndüğünde ilan taslağı kaybolmamalı.

### Çalışma şekli — özel durum

**"Uzaktan" seçiliyse lokasyon filtresi gevşemeli.** Trabzon'daki aday, İstanbul merkezli uzaktan ilana düşebilmeli. Sorguda ayrıca ele alınmalı.

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

> İkinci kol sayesinde serbest yazmanın dağınıklığı (ReactJS / React.js / Reakt) eşleştirmeyi bozmaz.

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

### Tercih edilen bölüm

**Opsiyonel**, serbest yazma + öneri. **Hiçbir zaman katı filtre değil** — doluysa skorda ağırlık.

> "Bilgisayar Mühendisliği mezunu" şartı, bootcamp'ten gelmiş 5 yıl tecrübeli adayı kaybetmemeli.

### İş dili

Çoklu seçim, **katı filtre**. Kesişim kontrolü — en az bir ortak dil varsa geçer.

### Diğer Şartlar

Üç parça, üçü farklı davranır.

#### 1. Hazır kutular — KATI FİLTRE

Kapalı liste, evet/hayır işaretleri:

- **Sürücü belgesi** *(seçilirse sınıf: B, C, D, E)*
- **Askerlik durumu** *(yapılmış / muaf / tecilli kabul — çoklu)*
- Aktif araç sahibi olmak
- Seyahat engeli olmaması
- Vardiyalı çalışabilmek

**Doluysa katı filtre olarak uygulanır**, boşsa hesaba katılmaz.
Embedding'e ve skora girmez.

> "B sınıfı ehliyet zorunlu" yazan kurye ilanına ehliyetsiz aday düşmemeli — bunlar gerçekten esnemeyen şartlardır.

Aday profilinde de karşılığı olmalı, yoksa filtre boşta kalır.

> ⚠️ **Askerlik uyarısı:** Filtre olarak kullanılırsa dolaylı cinsiyet ayrımcılığı doğar — sadece erkeklerin durumu var. **Kural:** askerlik alanı boş olan adaylar bu filtreden etkilenmez, otomatik geçer. Yoksa "askerliğini yapmış" şartı tüm kadın adayları eler. Aday tarafında bu alan opsiyonel olmalı.

#### 2. Sertifika ve belgeler — skorda ağırlık

Serbest yazma + öneri. Her biri için zorunlu/tercihen işareti. **Katı filtreye girmez.**

> **Neden:** İsimler tutmaz. "PMP" / "PMP Sertifikası" / "Project Management Professional" üç ayrı kayıt olur; katı filtre yapılırsa sertifikası olan aday bile elenir.

- Zorunlu işaretli → eksikse skor ciddi düşer, aday listede kalır
- Tercihen işaretli → varsa bonus, yoksa ceza yok

**Sertifika adları embedding'e de girer** — "PMP" ile "Proje Yönetimi Sertifikası" vektör uzayında yakın çıkar, isim dağınıklığı sorunu doğal olarak çözülür.

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

**Tamamı embedding'e girer.**

---

## Adım 4 — Şartlar ve Yayın

| Alan | Zorunlu | Not |
|---|:---:|---|
| Maaş aralığı *(iş ilanı)* | ❌ | **Min-max iki rakam**, tek rakam değil |
| Staj ücreti *(staj ilanı)* | ✅ | Ücretli / harçlık / ücretsiz — ücretliyse tutar |
| Başlama zamanı | ❌ | Hemen / 1 ay içinde / esnek |
| Yayın süresi | ✅ | 30 / 60 / 90 gün |
| Günlük davet kotası | — | İşveren girmez, plana göre otomatik |

### Maaş aralığı

**Opsiyonel.** Girilmezse "Belirtilmedi".

| | |
|---|---|
| **Doluysa** | Katı filtre — aralıklar kesişiyorsa geçer |
| **Boşsa** | Filtre atlanır |

**Kesişim örneği:**
```
Aday 25-35 bin bekliyor, ilan 30-40 bin veriyor  → kesişim var, geçer
Aday 40-50 bin bekliyor, ilan 20-25 bin veriyor  → kesişim yok, elenir
```

**Neden zorunlu değil:** Türkiye'de işverenlerin çoğu maaş yazmak istemiyor. Zorunlu yapılırsa ya uydurma aralık girerler (veri çöp olur, filtre yanlış çalışır) ya ilan açmaktan vazgeçerler.

**İki denge unsuru:**
- **Kartta bariz görünür:** "Maaş belirtilmedi" — aday bilsin, sessizce geçilmesin
- **Skorda hafif ceza:** belirtmeyen ilan sıralamada biraz aşağı düşer

> Zorlama yok ama yazan işverene avantaj var — profil tamamlanma mantığının aynısı. Zamanla daha çok işveren yazar.

**Aday tarafında simetrik:** Maaş beklentisi de min-max aralık ve opsiyonel. Biri boşsa filtre atlanır.

### Yayın süresi

30 / 60 / 90 gün. Süre dolunca **ilan otomatik kapanır**. İşverene önceden hatırlatma gider:

> *"İlanınız yarın kapanıyor, uzatmak ister misiniz?"*

**Neden gerekli:** Kapanmayan ilan birikir, aday dolu pozisyona başvurur ve güven gider.

### Günlük davet kotası

İşveren girmez, plana göre otomatik gelir. Panelde görünür:

> *"Bugün 7/10 davet kullanıldı"*

### Önizleme + Yayınla

İlanın **adaya nasıl görüneceği** gösterilir, işveren onaylar.

**Şirket onaysızsa burada kilit devreye girer:**

> *"Şirketiniz doğrulanmadan ilan yayınlanamaz"* + belge yükleme bağlantısı

### Taslak kaydetme

**Her adımda** taslak kaydedilir, yarıda bırakılıp dönülebilir.

---

## Açık Kalanlar

- Kategori listesinin nihai içeriği (15-20 kalem)
- Unvan başlangıç verisi (kategori başına 20-40)
- Skor ağırlıklarının nihai değerleri
- Maaş belirtmeme cezasının ağırlığı
- İlan moderasyonu: her ilan yayından önce admin onayından geçecek mi, yoksa sadece şirket onayı yeterli mi?