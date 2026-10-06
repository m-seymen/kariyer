# İş Eşleştirme Projesi — İş İlanı

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak
> **Sürüm 2 — final**
> Durum: Ekip toplantısı sonrası nihai karar
> İlgili dokümanlar: `isveren-paneli-v2.md`, `eslestirme-motoru.md`, `aday-profili.md`
> Kapsam dışı: **Staj ilanı** → ayrı dokümanda

---

## 0. Bu Dokümanı Nasıl Okumalı

Her alan için beş soru cevaplanıyor:

| Soru | Anlamı |
|---|---|
| **Zorunlu mu?** | İşveren boş bırakabilir mi |
| **Nasıl girilir?** | Kapalı liste, serbest metin, seçim |
| **Yapay zeka (LLM) ayrıştırır mı?** | Kayıtta metinden yapılandırılmış bilgi çıkarılıyor mu |
| **Embedding'e girer mi?** | İlanın vektörüne (anlamsal eşleşme) giriyor mu |
| **Katı filtre / skor?** | Adayı eliyor mu, yoksa sadece sıralamayı mı etkiliyor |

**Tüm alanların tek tablodaki özeti → Bölüm 2.**

---

## 1. Temel Kararlar

### 1.1 Yapay zeka sadece kayıt anında, sadece ayrıştırma için

> ~~LLM yok~~ → **LLM sadece ilan kaydında, metinden yapılandırılmış bilgi çıkarmak için.**
> **Sıralamada, skorlamada, eşleştirmede LLM yok.**

**Neden gerekli oldu:** İşveren yetenekleri cümle olarak yazmak istiyor — *"5 yıl React bilen, 8 yıl .NET tecrübesi olan"*. Eşleştirmenin "React 5 yıl isteyen ilan, React 3 yıl bilen ile 1 yıl bileni ayırt edebilmesi" için bu cümleden **"React → 5 yıl"** bilgisinin sayı olarak çıkarılması şart. Kural yazarak (desen arama) bunu her dilde, her yazım şeklinde güvenilir yapmak mümkün değil.

**Neden açıklanabilirlik bozulmuyor:** Yapay zekanın çıkardığı sonuç **işverene onaylatılıyor.** Skor, yapay zekanın yorumu üzerinden değil, **işverenin onayladığı etiketler** üzerinden çalışıyor.

**Maliyet:** Küçük, hızlı bir model ile ilan başına bir sentin çok altında. Bir SMS doğrulamasından ucuz. *(Güncel fiyat model seçiminde kontrol edilmeli.)*

### 1.2 Embedding'e sadece metin girer

> **Sayısal alanlar embedding'e girmez.** "React 5 yıl" ile "React 1 yıl" vektör uzayında neredeyse aynı görünür. Yıl, maaş gibi sayılar aritmetikle karşılaştırılır.
>
> **Katı filtre alanları embedding'e girmez.** Konum, dil, çalışma şekli zaten kesin olarak filtreleniyor.

### 1.3 "Olmazsa olmaz" ve "tercih edilen" her zaman ayrı kutuda

Yetenekler ve sertifikalar için işveren **iki ayrı kutuya** yazar. Bir öğenin zorunlu mu tercihen mi olduğunu **işveren belirler — hangi kutuya yazdığıyla.** Yapay zeka bunu tahmin etmez.

**Yapay zekanın işi sadece:** her kutudaki cümleyi etiketlere ayırmak ve (yeteneklerde) yılı çıkarmak.

> **Neden:** Tek kutu olsaydı yapay zeka "şart", "artı", "tercih sebebi" gibi kelimelerden tahmin yapmak zorunda kalırdı — işveren bu kelimeleri kullanmazsa yanlış sınıflandırırdı. İki kutu tahmini ortadan kaldırıyor ve her yerde aynı mantık oluyor.

### 1.4 Katı filtre sadece gerçekten esnemeyen şeyler için

Yetenek, tecrübe, bölüm, sertifika **kimseyi elemez** — sadece sıralamayı etkiler. Eleyen alanlar: meslek alanı, konum, çalışma şekli, çalışma tipi, iş dili, eğitim seviyesi (seçildiyse), diğer şartlar (gerekliyse).

### 1.5 Şirket bilgisi ile ilan bilgisi ayrı

- **Şirket profili** eşleştirmeye girmez — sadece adaya gösterim
- **İlanın konumu**, şirketin lokasyonundan bağımsız — ilan kendi konumunu tutar *(İK ajansları müşteri lokasyonları için ilan açabilsin diye)*

---

## 2. Tüm Alanlar — Tek Tablo

| Alan | Adım | Zorunlu | Giriş | LLM ayrıştırır | Embedding | Katı filtre | Skor | Adaya görünür |
|---|:---:|:---:|---|:---:|:---:|:---:|:---:|:---:|
| **Meslek alanı** | 1 | ✅ | Kapalı liste | ❌ | ❌ | ✅ | — | ✅ |
| **Unvan** | 1 | ✅ | Serbest + öneri | ❌ | ✅ | ❌ | — | ✅ |
| **İş yeri konumu** | 1 | ✅ | Konum ağacı + şirket lokasyonu kısayolu | ❌ | ❌ | ✅ | — | ✅ |
| **Çalışma şekli** | 1 | ✅ | Ofis / hibrit / uzaktan — çoklu | ❌ | ❌ | ✅ | — | ✅ |
| **Çalışma tipi** | 1 | ✅ | Tam / yarı zamanlı / proje bazlı / dönemsel | ❌ | ❌ | ✅ | — | ✅ |
| **Olmazsa olmaz yetenekler** | 2 | ❌ | Serbest metin | ✅ ad + yıl | ✅ ham metin | ❌ | ✅ | ✅ |
| **Tercih edilen yetenekler** | 2 | ❌ | Serbest metin | ✅ ad | ✅ ham metin | ❌ | ✅ | ✅ |
| **Toplam tecrübe** | 2 | ❌ | Min – max yıl | ❌ | ❌ | ❌ | ✅ | ✅ |
| **Eğitim seviyesi** | 2 | ❌ | Kapalı liste, varsayılan "Şart yok" | ❌ | ❌ | ✅ *seçildiyse* | — | ✅ |
| **Tercih edilen bölümler** | 2 | ❌ | Serbest metin | ✅ bölüm adları | ✅ ham metin | ❌ | ✅ | ✅ |
| **İş dili** | 2 | ✅ | Kapalı liste, çoklu | ❌ | ❌ | ✅ | — | ✅ |
| **Sürücü belgesi** | 2 | ✅ *seçim* | Gerekli değil / Gerekli + sınıf | ❌ | ❌ | ✅ *gerekliyse* | — | ✅ |
| **Seyahat** | 2 | ✅ *seçim* | Gerekli değil / Engeli olmamalı | ❌ | ❌ | ✅ *gerekliyse* | — | ✅ |
| **Vardiya** | 2 | ✅ *seçim* | Gerekli değil / Vardiyalı çalışabilmeli | ❌ | ❌ | ✅ *gerekliyse* | — | ✅ |
| **Askerlik** | 2 | ✅ *seçim* | Şart yok / Engeli olmamalı | ❌ | ❌ | ✅ *şartlıysa* | — | ✅ |
| **Olmazsa olmaz sertifika ve belgeler** | 2 | ❌ | Serbest metin | ✅ ad | ✅ ham metin | ❌ | ✅ | ✅ |
| **Tercih edilen sertifika ve belgeler** | 2 | ❌ | Serbest metin | ✅ ad | ✅ ham metin | ❌ | ✅ | ✅ |
| **Serbest şartlar** | 2 | ❌ | Serbest metin | ❌ | ❌ | ❌ | ❌ | ✅ |
| **İş tanımı** | 3 | ✅ *min 100* | Serbest metin, max 3.000 | ❌ | ✅ | ❌ | — | ✅ |
| **Maaş aralığı** | 4 | ❌ | Min – max + para birimi + net/brüt | ❌ | ❌ | ❌ | ✅ *kalite* | ✅ |
| **Başlama zamanı** | 4 | ❌ | Kapalı liste | ❌ | ❌ | ❌ | ❌ | ✅ |

### Özet — hangi alan nereye gidiyor

**Embedding'e girenler (ilan vektörü):**
```
Unvan
Olmazsa olmaz yetenekler              (ham metin)
Tercih edilen yetenekler              (ham metin)
Tercih edilen bölümler                (ham metin)
Olmazsa olmaz sertifika ve belgeler   (ham metin)
Tercih edilen sertifika ve belgeler   (ham metin)
İş tanımı
```

**Yapay zekanın ayrıştırdıkları (tek istekte, beş kutu):**
```
Olmazsa olmaz yetenekler             →  [ad + yıl]
Tercih edilen yetenekler             →  [ad]
Tercih edilen bölümler               →  [bölüm adı]
Olmazsa olmaz sertifika ve belgeler  →  [ad]
Tercih edilen sertifika ve belgeler  →  [ad]
```
*Zorunlu / tercihen ayrımını yapay zeka değil, kutu belirler.*

**Katı filtreler (adayı eler):**
```
Meslek alanı · İş yeri konumu · Çalışma şekli · Çalışma tipi · İş dili
Eğitim seviyesi (seçildiyse)
Sürücü belgesi · Seyahat · Vardiya · Askerlik (gerekliyse)
```

**Sadece skor (sıralamayı etkiler, kimseyi elemez):**
```
Yetenekler · Sertifikalar · Bölümler → yetenek bileşeni
Toplam tecrübe                       → tecrübe bileşeni
Maaş belirtilmiş mi                  → kalite bileşeni
```

**Hiçbir yere girmeyenler (sadece adaya gösterim):**
```
Serbest şartlar · Başlama zamanı · Maaş tutarı
```

---

## 3. Adım 0 — İlan Açma

İlanlar sayfasında:

```
📋 İlanlarım                         [+ İş ilanı aç]  [+ Staj ilanı aç]
```

| | |
|---|---|
| **Kim açabilir** | Sahip, yönetici, uzman — hepsi |
| **İlan tipi** | Butona göre otomatik — kullanıcı ayrıca seçmez |
| **Sorumlu kişi** | **İlanı açan kişi otomatik sorumlu olur** |
| **Devretme** | Sahip ve yönetici sorumluyu değiştirebilir (bkz. `isveren-paneli-v2.md`) |
| **Taslak** | Her adımda otomatik kaydedilir, yarıda bırakılıp dönülebilir |

> Staj ilanı aynı tabloyu ve eşleştirme motorunu kullanır, ama formu farklıdır → ayrı doküman.

---

## 4. Adım 1 — Pozisyon ve Temel Bilgiler

### 4.1 Meslek alanı

| | |
|---|---|
| Zorunlu | ✅ |
| Giriş | Kapalı liste — 15-20 kalem |
| Eşleştirme | **Katı filtre** |

> **Neden kapalı liste:** Kaba filtre buna bağlı. İnşaat adayı bilişim ilanına düşmemeli. Serbest olsa "Bilişim", "IT", "Yazılım" ayrı kayıtlar olur ve filtre çalışmaz.

> **Terminoloji:** Şirket profilinde **"Sektör"** (şirket ne iş yapıyor), ilanda **"Meslek alanı"** (pozisyon hangi meslek). Bir inşaat şirketi muhasebeci arayabilir — ilanın meslek alanı "Finans ve Muhasebe" olur.

Meslek alanı adları çevrilebilir (kod sabit, ad kullanıcının diline göre). **Tek liste tüm ülkelerde.** Kaynak: ISCO-08, sadeleştirilerek.

### 4.2 Unvan

| | |
|---|---|
| Zorunlu | ✅ |
| Giriş | Serbest yazma + öneri |
| Eşleştirme | **Embedding** — vektör metninin ilk satırı |

```
Meslek alanı:  [Bilişim ▾]
Unvan:         [Backend Dev|                ]
               ┌──────────────────────────┐
               │ Backend Developer        │
               │ Backend Geliştirici      │
               └──────────────────────────┘
```

- Öneriler seçilen meslek alanından gelir, kullanım sayısına göre sıralı
- Listede yoksa elle yazar → kaydedilir, sonraki kullanıcılara öneri olur

> **Neden katı filtre değil:** "Yazılım Mühendisi" ile "Backend Developer" farklı isim, aynı iş. Katı filtre olsa doğru adaylar elenir. Embedding bu yakınlığı yakalıyor.

> **Başlangıç verisi şart:** Meslek alanı başına 20-40 unvan, her lansman dili için. Boş öneri listesiyle karşılaşan ilk kullanıcılar herkes farklı yazar ve öneri sistemi kendine gelemez.

### 4.3 İş yeri konumu

| | |
|---|---|
| Zorunlu | ✅ |
| Giriş | Konum ağacından serbest arama + şirket lokasyonları kısayol |
| Eşleştirme | **Katı filtre** *(uzaktan istisnası var)* |

```
İş yeri konumu:  [Gebze, Kocaeli              ]   ← konum ağacından arama
Hızlı seçim:     [Genel Merkez] [Ankara Ofisi] [Fabrika]
```

- **Serbest seçim:** Konum ağacından herhangi bir yer — şirket lokasyonu olması gerekmez
- **Hızlı seçim:** Şirket lokasyonuna tıklanırsa konum ilana **kopyalanır**, bağlanmaz
- **Sonradan:** Şirket lokasyonu silinse veya değişse ilan **etkilenmez**

> **Neden şirket lokasyonundan ayrı:** İşin yapılacağı yer, şirketin ofisinden farklı olabilir — saha işi, şantiye, müşteri lokasyonu. İK ajansları müşterileri adına farklı şehirlerde ilan açıyor.

**Global konum yapısı:** Her ülke kendi idari derinliğini taşır (Türkiye'de il → ilçe, ABD'de eyalet → şehir, Singapur tek kademe). Arayüzde kademeli menü yok — tek arama kutusu. Kaynak: Geonames.

**Filtre kuralı:**
```
Aday "TR/İstanbul" seçmişse  →  ilan "TR/İstanbul/Kadıköy"  ✅ eşleşir
Aday "TR/İstanbul" seçmişse  →  ilan "TR/Ankara/Çankaya"    ❌ eşleşmez
Aday "DE" seçmişse           →  ilan "DE/Bayern/München"     ✅ eşleşir
```
Aday üst seviye seçerse alt seviyeler dahil.

### 4.4 Çalışma şekli

| | |
|---|---|
| Zorunlu | ✅ |
| Giriş | Ofis / hibrit / uzaktan — **çoklu seçim** |
| Eşleştirme | **Katı filtre** — adayın tercihiyle en az bir ortak seçenek olmalı |

**Konumla bağlantısı:**
```
ortak = aday.çalışma_şekli ∩ ilan.çalışma_şekli

ortak boş                          → ELENİR
aday "her konumda hazırım" demiş   → konum filtresi ATLANIR
"uzaktan" ortakta var              → konum filtresi ATLANIR
diğer                              → konum filtresi UYGULANIR
```

> **Hibrit konum gerektirir** (haftada birkaç gün ofiste). Sadece tam uzaktanda konum atlanır.

> **Uzaktan ilanlarda havuz tüm dünya olur** — o durumda meslek alanı ve iş dili en güçlü filtreler.

### 4.5 Çalışma tipi

| | |
|---|---|
| Zorunlu | ✅ |
| Giriş | Tam zamanlı / yarı zamanlı / proje bazlı / dönemsel — tek seçim |
| Eşleştirme | **Katı filtre** |

> "Staj" bu listede **yok** — staj ayrı ilan tipi.

---

## 5. Adım 2 — Aranan Nitelikler

### 5.1 Yetenekler — iki serbest alan

```
Olmazsa olmaz yetenekler
┌──────────────────────────────────────────────────┐
│ 5 yıl React bilen, 8 yıl .NET tecrübesi olan,    │
│ PostgreSQL'e hakim                               │
└──────────────────────────────────────────────────┘

Tercih edilen yetenekler
┌──────────────────────────────────────────────────┐
│ Docker ve Kubernetes bilgisi artı, AWS deneyimi  │
└──────────────────────────────────────────────────┘
```

| | Olmazsa olmaz | Tercih edilen |
|---|---|---|
| Zorunlu | ❌ | ❌ |
| Giriş | Serbest metin, cümle yazılabilir | Serbest metin |
| **LLM ayrıştırır** | ✅ **Yetenek adı + yıl** | ✅ **Yetenek adı** |
| **Embedding** | ✅ Ham metin | ✅ Ham metin |
| Katı filtre | ❌ | ❌ |
| **Skor** | ✅ Yetenek bileşeni — ağırlık 0.75 | ✅ Yetenek bileşeni — ağırlık 0.25 |

> **Neden iki alan:** İşverenin "olmazsa olmaz" dediği yetenek, "artı" dediğinden ayrılmalı. **Hangi kutuya yazıldığı** bu ayrımı belirler — yapay zeka tahmin etmez, sadece her kutudaki cümleyi etiketlere ayırır.

> **Neden serbest metin:** İşveren doğal dille yazmak istiyor. Yapay zeka cümleyi ayrıştırıp yapılandırılmış hale getiriyor.

> **Neden katı filtre değil:** "React zorunlu" diyen ilana, React yazmamış ama yıllarca frontend yapmış aday da düşmeli — sadece skoru düşük olur. İşveren kartta görür, kendi karar verir.

**Yıl neden önemli:**
```
İlan:     React · 5 yıl
Aday A:   React · 3 yıl   →  0.5 + 0.5 × 3/5 = 0.80
Aday B:   React · 1 yıl   →  0.5 + 0.5 × 1/5 = 0.60
```
A, B'nin önüne geçer. Düz metin tek başına bu ayrımı yapamazdı — yapay zeka ayrıştırması bunun için.

### 5.2 Toplam tecrübe

```
Toplam tecrübe (opsiyonel):  [ 3 ] – [ 6 ] yıl
```

| | |
|---|---|
| Zorunlu | ❌ — boşsa nötr (0.5), kimse elenmez |
| Eşleştirme | **Sadece skor** — tecrübe bileşeni (%10) |
| Embedding | ❌ (sayı) |

**Yetenek yılından farkı:**

| | Ne soruyor |
|---|---|
| Yetenek yılı | Belirli bir yetenekte ne kadar — "React'te 5 yıl" |
| Toplam tecrübe | Kariyerin tamamında ne kadar — "3-6 yıllık deneyimli" |

**Adayın tecrübesi** deneyim kayıtlarından otomatik hesaplanır (tam zamanlı ×1, yarı zamanlı ve staj ×0.5, gönüllü sayılmaz).

**İki yönde farklı hesaplanır:**
```
İlan: 1-2 yıl (junior)    Aday: 8 yıl

İşveren yönü:  1.0   → "fazla nitelikli, fırsat"
Aday yönü:     0.2   → "geriye adım"
```
Bu sayede 8 yıllık adayın Keşfet'ine junior ilanlar üst sıralarda düşmez.

> Formüllerin tamamı → `eslestirme-motoru.md`

### 5.3 Eğitim seviyesi

```
En az:  [Şart yok ▾]
         Şart yok
         Lise
         Ön lisans
         Lisans
         Yüksek lisans
         Doktora
```

| | |
|---|---|
| Zorunlu | ❌ — varsayılan **"Şart yok"** |
| Eşleştirme | **Seçildiyse katı filtre** — altındaki adaylar elenir. "Şart yok" ise filtreye girmez |
| Global | Arkada **ISCED** (BM standardı) seviyesiyle karşılaştırılır; kullanıcı kendi ülkesinin derece adını görür |

> **Neden opsiyonel:** Birçok pozisyonda eğitim şartı yok. Zorunlu olsa işveren bir şey seçmek zorunda kalır ve gereksiz eleme olur.

> **Neden ISCED:** Eğitim dereceleri ülkeler arasında birebir örtüşmüyor (Almanya'da *Ausbildung*, ABD'de *Associate Degree*). "En az lisans" filtresi ülkeler arası ancak ortak bir ölçekle çalışır.

### 5.4 Tercih edilen bölümler

```
Tercih edilen bölümler (opsiyonel)
┌──────────────────────────────────────────────────┐
│ Bilgisayar, elektronik veya yazılım mühendisliği │
│ mezunu olabilir                                  │
└──────────────────────────────────────────────────┘
```

| | |
|---|---|
| Zorunlu | ❌ |
| Giriş | Serbest metin — **birden fazla bölüm** yazılabilir |
| **LLM ayrıştırır** | ✅ Bölüm adları |
| **Embedding** | ✅ Ham metin |
| Katı filtre | ❌ |
| **Skor** | ✅ Yetenek bileşeni içinde, tercih edilen olarak |

**Eşleşme:** Adayın bölümü listedekilerden **herhangi biriyle** eşleşirse puan alır.

> **Neden katı filtre değil:** "Bilgisayar Mühendisliği mezunu" şartı, bootcamp'ten gelmiş 5 yıl tecrübeli adayı elerdi.

### 5.5 İş dili

```
İş dili *
[Türkçe ✕] [İngilizce ✕]   [+ Ekle]
```

| | |
|---|---|
| Zorunlu | ✅ En az bir |
| Giriş | Kapalı liste, çoklu |
| Varsayılan | **Şirketin ülkesinin dili** otomatik gelir |
| Eşleştirme | **Katı filtre** — adayın iş dilleriyle en az bir ortak dil |

> **Neden kapalı liste:** Katı filtre. "İngilizce", "English", "İng" ayrı kayıt olursa eşleşme kırılır.

> **Neden varsayılan dolu:** Boş kalırsa ilan **hiçbir adayla eşleşmez.** İşveren isterse ekler, değiştirir.

**Filtre örnekleri:**
```
İlan [Türkçe]              Aday [Türkçe, İngilizce]   ✅
İlan [İngilizce]           Aday [Türkçe]              ❌
İlan [Türkçe, İngilizce]   Aday [İngilizce]           ✅
```

### 5.6 Diğer şartlar — dört zorunlu seçim

```
Diğer şartlar *

Sürücü belgesi:   ○ Gerekli değil   ○ Gerekli → [B] [C] [D] [E]
Seyahat:          ○ Gerekli değil   ○ Seyahat engeli olmamalı
Vardiya:          ○ Gerekli değil   ○ Vardiyalı çalışabilmeli
Askerlik:         ○ Şart yok        ○ Askerlik engeli olmamalı
```

| | |
|---|---|
| Zorunlu | ✅ **Her satırda bir seçim** — varsayılan seçili gelmez |
| Eşleştirme | "Gerekli" seçilenler **katı filtre**; "Gerekli değil" seçilenler hiç çalışmaz |
| Embedding / skor | ❌ |

> **Neden zorunlu seçim:** İşveren her şartı bilinçli olarak işaretlesin. Aday tarafında da aynı dört alan zorunlu — böylece **"aday boş bıraktıysa ne olacak" sorusu tamamen ortadan kalkıyor**, filtre kuralları sade kalıyor.

**Filtre kuralları:**

| İlan | Aday | Sonuç |
|---|---|:---:|
| Gerekli değil | Herhangi | ✅ |
| Sürücü belgesi: B | Var: B | ✅ |
| Sürücü belgesi: B | Yok | ❌ |
| Seyahat engeli olmamalı | Seyahat engelim yok | ✅ |
| Seyahat engeli olmamalı | Seyahat edemem | ❌ |
| Vardiyalı çalışabilmeli | Çalışabilirim | ✅ |
| Vardiyalı çalışabilmeli | Çalışamam | ❌ |
| Askerlik engeli olmamalı | Yapıldı / Muaf / Yükümlü değil | ✅ |
| Askerlik engeli olmamalı | Tecilli | ❌ |

#### Askerlik — ayrımcılık önlemi

**Form her ülkede aynı.** Askerliğin olmadığı ülkede veya kadın aday için aday tarafında **"Yükümlü değil"** seçeneği var.

**Şartın adı bilinçli seçildi:**
- ❌ *"Askerliğini yapmış olmalı"* → kadınları ve yükümlü olmayanları dışlar
- ✅ *"Askerlik engeli olmamalı"* → sadece askerlik yüzünden işe başlayamayacak durumdakileri (tecilli) dışlar

> İşverenin asıl kaygısı da bu: "Bu kişi işe başlayıp askere gitmek zorunda kalmasın."

**Adayın askerlik durumu işverene gösterilmez** — sadece filtre olarak çalışır. İşveren "kimin tecilli olduğunu" değil, sadece uygun adayları görür.

**Sürücü belgesi sınıfları** ülkeye göre değişir (AB'de B/C/D/E, ABD'de eyalet bazlı); liste şirketin ülkesine göre gelir.

### 5.7 Sertifika ve belgeler — iki serbest alan

Yeteneklerle **aynı mantık:**

```
Olmazsa olmaz sertifika ve belgeler (opsiyonel)
┌──────────────────────────────────────────────────┐
│ SRC belgesi ve forklift ehliyeti                 │
└──────────────────────────────────────────────────┘

Tercih edilen sertifika ve belgeler (opsiyonel)
┌──────────────────────────────────────────────────┐
│ İSG eğitimi almış olması                         │
└──────────────────────────────────────────────────┘
```

| | Olmazsa olmaz | Tercih edilen |
|---|---|---|
| Zorunlu | ❌ | ❌ |
| Giriş | Serbest metin | Serbest metin |
| **LLM ayrıştırır** | ✅ Sertifika adı | ✅ Sertifika adı |
| **Embedding** | ✅ Ham metin | ✅ Ham metin |
| Katı filtre | ❌ | ❌ |
| **Skor** | ✅ Yetenek bileşeni — olmazsa olmaz küme | ✅ Yetenek bileşeni — tercih edilen küme |

> **Neden iki kutu:** Tek kutu olsaydı yapay zeka "şart", "tercih sebebi" gibi kelimelerden zorunlu mu tercihen mi olduğunu tahmin etmek zorunda kalırdı. İşveren bu kelimeleri kullanmazsa yanlış sınıflandırırdı. İki kutu tahmini kaldırıyor ve yeteneklerle aynı deneyimi sunuyor.

> **Neden katı filtre değil:** İsimler tutmuyor — "SRC belgesi" / "SRC 2" / "Mesleki Yeterlilik Belgesi (SRC)". Katı filtre olsa belgesi olan aday yazım farkı yüzünden sessizce elenir.

**Adaydaki süresi dolmuş sertifika yok sayılır** (İSG, ilk yardım, SRC gibi yenilenmesi gereken belgeler).

Olmazsa olmaz sertifika eksikse sadece skor düşer; kartta ayrıca uyarı gösterilmez.

### 5.8 Serbest şartlar

```
Diğer notlar (opsiyonel)
┌──────────────────────────────────────────────────┐
│                                                  │
└──────────────────────────────────────────────────┘
```

Yukarıdakilere girmeyen her şey. **Hiçbir yere girmez — sadece adaya gösterim.**

---

## 6. Adım 3 — İş Tanımı

```
İş tanımı *
┌──────────────────────────────────────────────────┐
│                                                  │
│                                                  │
│                                                  │
└──────────────────────────────────────────────────┘
[✨ Taslak oluştur]                        0 / 3.000
```

| | |
|---|---|
| Zorunlu | ✅ |
| Minimum | **100 karakter** (1-2 cümle) |
| Maksimum | 3.000 karakter |
| Giriş | Tek serbest alan |
| LLM ayrıştırır | ❌ |
| **Embedding** | ✅ **Tamamı** |
| Katı filtre / skor | ❌ — skoru **dolaylı** etkiler (anlamsal bileşen üzerinden) |
| Kalite skoru | ❌ Girmez |

> **Neden en önemli metin:** Şirket profili eşleştirmeye girmiyor. İlanın ne olduğunu anlatan asıl metin burası. Ne kadar net yazılırsa ilan vektörü o kadar doğru olur ve doğru adaylar o kadar yüksek anlamsal puan alır.

> **Minimum neden 100:** "Ekibimize takım arkadaşı arıyoruz" gibi tek satırı engelliyor ama kimseyi uzun yazmaya zorlamıyor.

### İş tanımı kısa kalırsa eşleştirme çöker mi?

**Hayır.** İlan vektörüne sadece iş tanımı girmiyor — unvan, yetenekler, bölümler, sertifikalar da giriyor. İş tanımı vektörü **zenginleştiriyor**, tek başına taşımıyor. Kısa iş tanımı eşleşmeyi zayıflatır ama bozmaz.

### ✨ Taslak oluştur

Yazmak istemeyen işveren için:

```
[✨ Taslak oluştur]
        ↓
"İstanbul Kadıköy'deki ofisimizde, hibrit çalışacak bir Senior
Backend Developer arıyoruz. Ekibimizde React ve .NET ile ürün
geliştirecek, PostgreSQL veritabanlarında çalışacaksınız. En az
5 yıl React ve 8 yıl .NET deneyimi bekliyoruz..."
        ↓
İşveren okur, düzenler, kendi bildiklerini ekler, kaydeder
```

| | |
|---|---|
| Ne kullanır | İşverenin **zaten doldurduğu** alanlar: unvan, konum, çalışma şekli, yetenekler, tecrübe, bölüm, sertifikalar |
| Ne zaman çalışır | Sadece butona basılınca |
| Maliyet | Yetenek ayrıştırmasıyla aynı büyüklükte — ilan başına sentin çok altında |
| Kayıt | Taslak kutuya yazılır; işveren kaydetmeden ilana geçmez |

> **Dürüstlük notu:** Taslak, işverenin girdiği bilgilerden üretildiği için eşleştirmeye **yeni bilgi eklemiyor** — okunabilir hale getiriyor. Asıl kazanç, işverenin taslağın üzerine ekip, günlük iş, çalışma ortamı gibi bildiği şeyleri eklemesi. Her durumda boş bırakmaktan iyi ve adaya okunaklı bir ilan sunuyor.

---

## 7. Adım 4 — Şartlar ve Yayın

### 7.1 Maaş aralığı

```
Maaş aralığı:  [______] – [______]   [TRY ▾]   ○ Net  ○ Brüt
               ☐ Maaş belirtmek istemiyorum
```

| | |
|---|---|
| Zorunlu | ❌ — işveren isterse girer |
| Kurallar | Kutu işaretlenmediyse min, max, para birimi ve net/brüt **dolu olmalı** |
| Para birimi | Varsayılan: şirketin ülkesi |
| Katı filtre | ❌ **Hiçbir filtreye girmez** |
| Embedding | ❌ |
| **Kalite skoru** | ✅ **Maaş belirten ilan daha kaliteli sayılır** — aday yönü kalite bileşeninde en ağır kriter |
| Adaya | Doluysa aralık gösterilir; boşsa **"Maaş belirtilmedi"** |

> **Neden filtre değil:** Adayın maaş beklentisi profilde değil, **başvuru anında** soruluyor (pozisyona özel). Karşılaştırılacak iki taraf yok.

> **Neden kalitede ağır:** Adayın en çok önemsediği bilgi. Maaş yazan ilan adayın Keşfet'inde yukarı çıkar — işvereni yazmaya teşvik eder.

> **Global:** Bazı ülkelerde maaş aralığı yazmak **yasal zorunluluk** (AB Maaş Şeffaflığı Direktifi, ABD'de bazı eyaletler). O ülkelerde "Maaş belirtmek istemiyorum" seçeneği gösterilmez — ülke ayarlarından.

> **Kur çevrimi yapılmadan saklanır** — kur değişince geçmiş veriler bozulmasın.

### 7.2 Başlama zamanı

```
Başlama zamanı (opsiyonel):  [Seçiniz ▾]
                              Hemen
                              1 ay içinde
                              Esnek
```

Sadece adaya gösterim.

### 7.3 Yayın süresi

| | |
|---|---|
| Süre | **Sabit 60 gün** — seçim yok |
| Süre dolunca | Otomatik kapanır → `süresi_doldu` |

---

## 8. Kaydetme Akışı — Yapay Zeka Ayrıştırma ve Onay

### Akış

```
İşveren [Kaydet] veya [Devam]
        ↓
"Yetenekler analiz ediliyor..."   (1-3 saniye)
        ↓
┌──────────────────────────────────────────────────────────┐
│ Şunları anladık — doğru mu?                              │
│                                                          │
│ Olmazsa olmaz yetenekler:                                │
│   [React · 5 yıl ✕] [.NET · 8 yıl ✕] [PostgreSQL ✕]      │
│                                                          │
│ Tercih edilen yetenekler:                                │
│   [Docker ✕] [Kubernetes ✕] [AWS ✕]                      │
│                                                          │
│ Tercih edilen bölümler:                                  │
│   [Bilgisayar Müh. ✕] [Elektronik Müh. ✕]                │
│   [Yazılım Müh. ✕]                                       │
│                                                          │
│ Olmazsa olmaz sertifika ve belgeler:                     │
│   [SRC belgesi ✕] [Forklift ehliyeti ✕]                  │
│                                                          │
│ Tercih edilen sertifika ve belgeler:                     │
│   [İSG eğitimi ✕]                                        │
│                                                          │
│ [+ Ekle]                         [Doğru, devam et]       │
└──────────────────────────────────────────────────────────┘
```

**İşveren yapabilir:**
- Etiket silmek, eklemek
- Yılı değiştirmek veya eklemek
- Etiketi olmazsa olmaz ↔ tercih edilen arasında taşımak

### Kurallar

| Kural | Neden |
|---|---|
| **Tek istek** | Beş kutu (2 yetenek, 1 bölüm, 2 sertifika) aynı yapay zeka isteğiyle ayrıştırılır — tek onay ekranı |
| **Kutu sınıfı belirler** | Bir etiketin olmazsa olmaz mı tercihen mi olduğu, yazıldığı kutudan gelir — yapay zeka tahmin etmez |
| **Sadece metin değişince çalışır** | İlgili metinlerin özeti (hash) tutulur. Maaş veya konum değişti diye yeniden ayrıştırılmaz |
| **Küçük, hızlı model** | Görev basit; büyük model gereksiz |
| **İşveren onayı şart** | Skor, **onaylanmış etiketler** üzerinden çalışır — yapay zekanın ham yorumu üzerinden değil |
| **Sunucuda çalışır** | Yapay zeka çağrısı Edge Function'da — anahtar istemcide değil |

### Yapay zeka cevap vermezse

İşveren hiçbir zaman takılıp kalmaz:

```
1. İlan yine kaydedilir
2. Geçici olarak: adayın yetenek adları ilan metninde aranır (yılsız, var/yok)
3. Ayrıştırma arka planda tekrar denenir
4. Başarılı olunca işverene bildirim: "Yetenekleriniz analiz edildi, onaylayın"
```

### Sonra ne olur

```
Onaylandı
  → Etiketler kaydedilir
  → İlan metni birleştirilir → hash değiştiyse embedding üretilir
  → (Yayınlandıysa) eşleştirme arka planda hesaplanır
```

---

## 9. Önizleme ve Yayınlama

```
[Önizle]   → adayın göreceği hali

[Yayınla]
  → Şirket profilinde tanıtım dolu mu?    Hayır → tanıtım alanına yönlendir
  → Cep telefonum doğrulanmış mı?          Hayır → SMS doğrulama
  → İkisi de evet → ilan yayına girer, 60 gün başlar
```

> Şirket zaten onaylı (sistemde onaysız işveren yok). Tanıtım bir kez yazılınca bu kontrol bir daha takılmaz; sonraki ilanlarda tek kontrol kişisel telefon.

---

## 10. İlan Durumları

```
taslak ──→ yayında ──→ süresi_doldu
              │  ↑            │
              │  └── uzat     │
              ↓               ↓
        duraklatıldı    [Tekrar yayınla] ──→ yayında
              │
              ↓
     dolduruldu / kapatıldı
```

| Durum | Anlamı | Eşleştirmede | Nasıl olur |
|---|---|:---:|---|
| `taslak` | Henüz yayınlanmamış | ❌ | İlan açıldığında |
| `yayinda` | Aktif | ✅ | [Yayınla] |
| `duraklatildi` | Geçici durduruldu | ❌ | İşveren [Duraklat] — *"çok başvuru geldi, biraz durdur"* |
| `dolduruldu` | Pozisyon doldu | ❌ | İşveren [Pozisyon doldu] |
| `kapatildi` | İşveren vazgeçti | ❌ | İşveren [Kapat] |
| `suresi_doldu` | 60 gün bitti | ❌ | Otomatik |

> **"Dolduruldu" ile "kapatıldı" neden ayrı:** Dolduruldu **başarı göstergesi** — platformun asıl ölçüsü (işe alım oranı) buradan besleniyor. Vazgeçme farklı bir sinyal: ilan kalitesi veya aday havuzu sorunu olabilir.

> **Duraklat neden var:** Kapatmaktan farkı — tekrar açıldığında ilan sıfırdan kurulmaz, geçmiş korunur.

### Uzatma — süre dolmadan

```
Senior Backend Developer        🟢 Yayında · 12 gün kaldı
[Uzat]
        ↓
Ne kadar uzatmak istersiniz?
○ 15 gün   ○ 30 gün   ○ 60 gün
[Uzat]
```

| | |
|---|---|
| Ne zaman | İlan yayındayken **her zaman** |
| Hatırlatma | **Yok** — işveren isterse kendisi uzatır |
| Seçenekler | 15 / 30 / 60 gün |
| Kesinti | Yok — ilan hiç kapanmaz |
| Sayaç | **Uzatma sayısı +1** |
| Üst sınır | Kesintisiz en fazla **180 gün** — sonra süre dolar |
| Kim yapar | Sorumlu, sahip, yönetici |

> **Neden hazır seçenekler, serbest gün değil:** İşveren "365" yazarsa ilan bir yıl açık kalır — pozisyon çoktan dolmuş olabilir. Platformun en büyük güven sorunlarından biri **ölü ilanlar.**

> **Neden 180 gün sınırı:** İlanın hâlâ gerçekten açık olduğunu işverene ara ara onaylatıyor.

### Tekrar yayınlama — süre dolduktan sonra

```
Senior Backend Developer        ⏱ Süresi doldu
Yayın sayısı: 1
[Tekrar yayınla]
        ↓
Yeni 60 gün başlar · Yayın sayısı +1
```

| Konu | Davranış |
|---|---|
| İlan kaydı | **Aynı ilan** — yeni ilan açılmaz |
| Aksiyon geçmişi | **Korunur** — geçilen ve reddedilen adaylar bir daha görünmez |
| Embedding | Metin değişmediyse **yeniden üretilmez** |
| Eşleştirme sonuçları | Kapanınca silinmişti → **yeniden hesaplanır** |
| Önceki bekleyen başvurular | İşverene sorulur: *"12 bekleyen başvurunuz vardı. Geri getirilsin mi?"* |
| Hangi durumlardan | `süresi_doldu`, `kapatildi`, `dolduruldu` |

> **Neden aynı ilan:** Yeni kayıt açılsaydı geçmiş kaybolurdu — aynı aday aynı ilanı tekrar görür, reddedilen tekrar başvurur.

> **Meslek alanı değiştirilirse:** *"Bu değişiklik ilanın pozisyonunu değiştiriyor. Yeni ilan olarak açın."* yönlendirmesi. Yoksa eski aksiyon geçmişi farklı bir pozisyona yanlış uygulanır.

### İki ayrı sayaç

| Sayaç | Ne zaman artar | Ne anlatır |
|---|---|---|
| **Yayın sayısı** | Kapanıp **tekrar yayınlandığında** | İlanın kaç kez açıldığı |
| **Uzatma sayısı** | **Kapanmadan uzatıldığında** | Pozisyonun ne kadar süredir doldurulamadığı |

> "4 kez uzatıldı" bilgisi işverene ipucu (belki maaş veya şartlar gözden geçirilmeli), admin'e ölü ilan sinyali.

### İlan kapanınca / süresi dolunca ne olur

| | |
|---|---|
| Eşleştirme sonuçları | Silinir *(tablo küçük kalsın — tekrar yayınlanırsa yeniden hesaplanır)* |
| Aksiyon geçmişi | **Kalır** |
| Bekleyen başvurular / davetler | Kapanır, adaylara bildirim gider |
| Aktif sohbetler | **Devam eder** — konuşma yarıda kalmasın |

---

## 11. Eşleştirmeye Etkisi — Skor Bileşenleri

İlandaki alanların skora nasıl yansıdığı:

```
skor = 0.65 × anlamsal       ← ilan vektörü (unvan, yetenek/bölüm/sertifika metinleri, iş tanımı)
     + 0.20 × yetenek        ← onaylanmış etiketler
     + 0.10 × tecrübe        ← toplam tecrübe min–max
     + 0.05 × kalite         ← aday yönünde: ilan kalitesi
```

### Yetenek bileşeni — yeni hali

**Olmazsa olmaz küme** = olmazsa olmaz yetenekler + olmazsa olmaz sertifikalar
**Tercih edilen küme** = tercih edilen yetenekler + tercih edilen sertifikalar + bölüm eşleşmesi

```
Olmazsa olmaz kümedeki her öğe:
  yıl yazılmış:
    adayda var, yıl yeterli   → 1.0
    adayda var, yıl az        → 0.5 + 0.5 × (aday_yıl / istenen_yıl)
    adayda yok                → 0.0
  yıl yazılmamış:
    adayda var → 1.0   yok → 0.0

Tercih edilen kümedeki her öğe:
  adayda var → 1.0   yok → 0.0      (yıl skorda kullanılmaz)

zorunlu_puan  = olmazsa olmaz kümenin ortalaması
tercihen_puan = tercih edilen kümenin ortalaması

yetenek = 0.75 × zorunlu_puan + 0.25 × tercihen_puan
```

| Durum | Sonuç |
|---|---|
| Hiç etiket yok | 0.5 (nötr) |
| Sadece olmazsa olmaz var | `zorunlu_puan` tek başına |
| Sadece tercih edilen var | `tercihen_puan` tek başına |

**Örnek:**
```
Olmazsa olmaz:  React · 5 yıl,  .NET · 8 yıl,  PostgreSQL
Tercih edilen:  Docker, AWS,  Bölüm: [Bilgisayar Müh., Yazılım Müh.]
Aday:           React 3 yıl, PostgreSQL 4 yıl, Docker 2 yıl
                Bölüm: Bilgisayar Müh.

Olmazsa olmaz:
  React       var, 3 < 5   → 0.5 + 0.5 × 3/5 = 0.80
  .NET        yok          → 0.00
  PostgreSQL  var          → 1.00
  zorunlu_puan = (0.80 + 0.00 + 1.00) / 3 = 0.60

Tercih edilen:
  Docker      var → 1
  AWS         yok → 0
  Bölüm       eşleşti → 1
  tercihen_puan = 2/3 = 0.67

yetenek = 0.75 × 0.60 + 0.25 × 0.67 = 0.62
```

**Eşleşme nasıl yapılır:** Adayın yetenek ve sertifika adları, ilanın onaylanmış etiketleriyle karşılaştırılır. Yazım farkları ("ReactJS" / "React.js") benzerlik kontrolüyle yakalanır.

**İki yönde aynı** — yetenek bileşeninde yön farkı yok.

### Kalite bileşeni — aday yönü (ilan kalitesi)

| Kriter | Puan |
|---|:---:|
| **Maaş aralığı belirtilmiş** | **0.50** |
| İmkanlar alanı dolu *(şirket profili)* | 0.20 |
| Gerçek logo yüklenmiş *(varsayılan avatar değil)* | 0.15 |
| Web sitesi veya sosyal medya var *(şirket profili)* | 0.15 |
| **Toplam** | **1.00** |

> İş tanımı kalite skoruna **girmez** — zorunlu ve minimum 100 karakter olduğu için her zaman dolu.

> Tüm ağırlıklar veritabanında — kod değişmeden ayarlanabilir.

---

## 12. İlan Vektörü — Hangi Metinden Oluşur

```
[Unvan]
Olmazsa olmaz yetenekler:             [ham metin]
Tercih edilen yetenekler:             [ham metin]
Tercih edilen bölümler:               [ham metin]
Olmazsa olmaz sertifika ve belgeler:  [ham metin]
Tercih edilen sertifika ve belgeler:  [ham metin]
İş tanımı:                            [tam metin]
```

**Neden ham metin, etiketler değil:** Ham metin bağlamı da taşır — *"PostgreSQL'e hakim, yüksek trafikli sistemlerde"* gibi ifadeler etiket listesinde kaybolurdu. Etiketler skor için, ham metin anlamsal eşleşme için.

**Ne zaman yeniden üretilir:** Bu yedi metinden biri değişince (hash kontrolü). Maaş, konum, durum değişikliği vektörü etkilemez.

**Girmeyenler:** Meslek alanı, konum, çalışma şekli, çalışma tipi, iş dili, eğitim seviyesi, diğer şartlar, toplam tecrübe, maaş, başlama zamanı, serbest şartlar, şirket profilinin tamamı.

---

## 13. Adaya Ne Görünür

### Keşfet kartı

```
┌──────────────────────────────────────────────┐
│ [İŞ İLANI]                                   │
│ Senior Backend Developer                     │
│ ABC Teknoloji  ✓ Doğrulanmış şirket          │
│ 📍 Kadıköy, İstanbul · Hibrit · Tam zamanlı   │
│ 💰 80.000 – 110.000 TRY (net)                 │
│                                              │
│ Olmazsa olmaz: React (5 yıl) ✓  .NET ✗       │
│                PostgreSQL ✓                  │
└──────────────────────────────────────────────┘
```

- İlan tipi rozeti büyük ve renkli — tıklamadan ayırt edilir
- Olmazsa olmaz yeteneklerde adayın sahip oldukları ✓ ile işaretlenir

### İlan detayı

Tüm alanlar — iş tanımı, yetenekler, eğitim, iş dili, diğer şartlar, sertifikalar, serbest şartlar, maaş, başlama zamanı + şirket profili (tanıtım, imkanlar, şirket bilgileri).

**Adaya gösterilmeyenler:**
- İlanın sorumlusunun kim olduğu
- Uzatma ve yayın sayıları
- İlanın sayaçları (görüntülenme, başvuru)

---

## 14. Teknik Notlar

| Konu | Karar |
|---|---|
| **İş kuralları** | Sunucuda — yayın şartları kontrolü, yapay zeka çağrısı, embedding üretimi, durum geçişleri |
| **Yapay zeka çağrıları** | Edge Function içinde; anahtar istemcide değil |
| **Ayrıştırma hash'i** | Beş kutunun (yetenek, bölüm, sertifika) metinlerinin özeti — değişmediyse yapay zeka çağrılmaz |
| **Embedding hash'i** | Yedi metin alanının özeti — değişmediyse API çağrılmaz |
| **Sayaçlar** | İlanın satırında hazır kolonlar; başvuru/davet tetikleyiciyle, görüntülenme dakikalık toplu işle (bkz. `isveren-paneli-v2.md`) |
| **Yetkiler** | RLS — uzman sadece sorumlu olduğu ilanları görür ve düzenler |
| **Taslak** | Her adımda otomatik kayıt |
| **Durum geçişleri** | `süresi_doldu` otomatik (periyodik iş); diğerleri işveren aksiyonuyla |

---

## 15. Diğer Dokümanlara Etkisi

| Doküman | Değişecek |
|---|---|
| **Eşleştirme Motoru** | "LLM yok" → **"LLM sadece ilan kaydında ayrıştırma için"**; yetenek bileşeninin yeni formülü (sertifika + bölüm dahil, yıl opsiyonel); ilan kalitesi kriterlerinin yeni dağılımı; askerlik filtresi (tecilli elenir); diğer şartlarda boş durum kalktı |
| **Aday Profili** | Diğer bilgiler (sürücü belgesi, seyahat, vardiya, askerlik) **zorunlu** — yayın eşiğine giriyor; askerlikte **"Yükümlü değil"** seçeneği; askerlik işverene gösterilmez; boş alan uyarıları kaldırılıyor |
| **İşveren Paneli** | İlan kartında yayın ve uzatma sayıları; [Uzat] ve [Tekrar yayınla] butonları |
| **Staj İlanı** *(yazılacak)* | Bu dokümanın yapısını temel alır; farklılaşan alanlar ayrıca |
