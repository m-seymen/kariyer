# İş Eşleştirme Projesi — İş İlanı

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak
> **Sürüm 3 — final** (eğitim şartı yeni yapıya geçti · işveren paneli kararlarıyla güncellendi)
> Durum: Ekip toplantısı sonrası nihai karar
> İlgili dokümanlar: `isveren-paneli-final.md`, `eslestirme-motoru.md`, `aday-profili-final.md`
> Kapsam dışı: **Staj ilanı** → `staj-ilani.md`

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

**Neden açıklanabilirlik bozulmuyor:** Yapay zekanın çıkardığı sonuç **gizli değil** — her kutunun altında *"Algılanan"* satırında görünür ve işveren **[Düzenle]** ile düzeltebilir. Onay ekranı yok; işveren bir şey yapmak zorunda değil. İşverenin düzelttiği etiket, metin sonradan değişse bile korunur (Bölüm 8).

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

Yetenek, tecrübe, bölüm, sertifika **kimseyi elemez** — sadece sıralamayı etkiler. Eleyen alanlar: meslek alanı, konum, çalışma şekli, çalışma tipi, iş dili, eğitim şartı ("şart yok" seçilmediyse), diğer şartlar (gerekliyse).

### 1.5 Şirket bilgisi ile ilan bilgisi ayrı

- **Şirket profili** eşleştirmeye girmez — sadece adaya gösterim
- **İlanın konumu**, şirketin lokasyonundan bağımsız — ilan kendi konumunu tutar *(İK ajansları müşteri lokasyonları için ilan açabilsin diye)*

---

## 2. Tüm Alanlar — Tek Tablo

| Alan | Adım | Zorunlu | Giriş | LLM ayrıştırır | Embedding | Katı filtre | Skor | Adaya görünür |
|---|:---:|:---:|---|:---:|:---:|:---:|:---:|:---:|
| **Meslek alanı** | 1 | ✅ *yayından sonra kilitli* | Kapalı liste | ❌ | ❌ | ✅ | — | ✅ |
| **Unvan** | 1 | ✅ *yayından sonra kilitli* | Serbest + öneri | ❌ | ✅ | ❌ | — | ✅ |
| **İş yeri konumu** | 1 | ✅ | Konum ağacı + şirket lokasyonu kısayolu | ❌ | ❌ | ✅ | — | ✅ |
| **Çalışma şekli** | 1 | ✅ | Ofis / hibrit / uzaktan — çoklu | ❌ | ❌ | ✅ | — | ✅ |
| **Çalışma tipi** | 1 | ✅ | Tam / yarı zamanlı / proje bazlı / dönemsel — **çoklu** | ❌ | ❌ | ✅ | — | ✅ |
| **Olmazsa olmaz yetenekler** | 2 | ❌ | Serbest metin | ✅ ad + yıl | ✅ ham metin | ❌ | ✅ | ✅ |
| **Tercih edilen yetenekler** | 2 | ❌ | Serbest metin | ✅ ad | ✅ ham metin | ❌ | ✅ | ✅ |
| **Toplam tecrübe** | 2 | ❌ | Min – max yıl | ❌ | ❌ | ❌ | ✅ | ✅ |
| **Kimler başvurabilir** | 2 | ✅ | Öğrenci / mezun / ikisi / şart yok | ❌ | ❌ | ✅ *şart yok değilse* | — | ✅ |
| **Hangi seviyede** | 2 | ✅ *şart yoksa gizli* | İlkokul → doktora — çoklu | ❌ | ❌ | ✅ | — | ✅ |
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
Kimler başvurabilir + seviye ("Eğitim şartı yok" seçilmediyse)
Sürücü belgesi · Seyahat · Vardiya · Askerlik (gerekliyse)
```

**Sadece skor (sıralamayı etkiler, kimseyi elemez):**
```
Yetenekler · Sertifikalar · Bölümler → nitelik uyumu
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
| **Devretme** | Sahip ve yönetici sorumluyu değiştirebilir (bkz. `isveren-paneli-final.md` Bölüm 5.3) |
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

> 🔒 **Yayından sonra değiştirilemez** — Bölüm 4.6.

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

> 🔒 **Yayından sonra değiştirilemez** — Bölüm 4.6.

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

```
Çalışma tipi *
☑ Tam zamanlı   ☑ Yarı zamanlı   ☐ Proje bazlı   ☐ Dönemsel
```

| | |
|---|---|
| Zorunlu | ✅ En az bir |
| Giriş | Tam zamanlı / yarı zamanlı / proje bazlı / dönemsel — **çoklu seçim** |
| Eşleştirme | **Katı filtre** — adayın çalışma tipi tercihiyle **en az bir ortak** seçenek olmalı |

| İlan | Aday | Sonuç |
|---|---|:---:|
| Tam, Yarı | Tam zamanlı | ✅ |
| Tam | Yarı zamanlı | ❌ |
| Proje bazlı | Tam, Proje bazlı | ✅ |

> **Neden çoklu:** Kafe, mağaza, çağrı merkezi *"tam veya yarı zamanlı"* der — tek seçim olsa işveren adayların yarısını kaybederdi. Aday tarafında da aynı alan çoklu.

> "Staj" bu listede **yok** — staj ayrı ilan tipi.

### 4.6 Yayından sonra kilitli alanlar

| Alan | Taslakken | Yayından sonra |
|---|:---:|:---:|
| **Meslek alanı** | ✅ Değiştirilebilir | 🔒 **Değiştirilemez** |
| **Unvan** | ✅ Değiştirilebilir | 🔒 **Değiştirilemez** |
| Diğer tüm alanlar | ✅ | ✅ |

> *"Meslek alanı ve unvan yayından sonra değiştirilemez. Farklı bir pozisyon için yeni ilan açın."*

**Neden:** İkisi pozisyonun kimliği. Muhasebe ilanını yazılım ilanına çevirmek; eski başvuranların, davetlerin, "bir daha gösterme" kayıtlarının geçmişini alakasız bir pozisyona taşır. Benzer bir pozisyon için **[⋮] → Kopyala** ile yeni ilan açılır.

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
| **Skor** | ✅ Nitelik uyumu — ağırlık 0.75 | ✅ Nitelik uyumu — ağırlık 0.25 |

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

İşveren yönü:  0.4   → "fazla nitelikli — teklifi kabul etmeyebilir"
Aday yönü:     0.2   → "geriye adım"
```
İyi eşleşme **iki tarafın da "evet" diyeceği** eşleşme: 8 yıllık aday junior ilanın maaşına çoğunlukla "hayır" der. Bu yüzden iki listede de **altta** — ama görünür. İşverende düşüş daha yumuşak (fazla nitelikli adayı gerçekten isteyen işveren için).

> Formüllerin tamamı → `eslestirme-motoru.md`

### 5.3 Eğitim şartı

```
Kimler başvurabilir? *
○ Sadece devam eden öğrenciler
○ Sadece mezunlar
○ İkisi de
○ Eğitim şartı yok

Hangi seviyede? *            ← "Eğitim şartı yok" seçilirse gizlenir
☐ İlkokul   ☐ Ortaokul   ☐ Lise   ☐ Ön lisans   ☑ Lisans   ☑ Yüksek lisans   ☑ Doktora
```

| | Kimler başvurabilir | Hangi seviyede |
|---|---|---|
| Zorunlu | ✅ Tek seçim | ✅ En az bir *(şart yoksa gizli)* |
| Eşleştirme | **Katı filtre** *("şart yok" hariç)* | **Katı filtre** |

> **Staj ilanıyla aynı yapı.** İki ilan tipinde tek bir eğitim mantığı var — tek kod, tek filtre. Staj ilanından farkı: iş ilanında **"Eğitim şartı yok"** seçeneği ve **ilkokul / ortaokul** seviyeleri var.

**Filtre kuralı:**

| İşveren seçimi | Aday geçer, eğer… |
|---|---|
| **Sadece devam eden öğrenciler** | Seçilen seviyelerden birinde **"devam ediyorum"** işaretli eğitim kaydı var |
| **Sadece mezunlar** | Seçilen seviyelerden birinde **tamamlanmış** eğitim kaydı var |
| **İkisi de** | Yukarıdakilerden herhangi biri |
| **Eğitim şartı yok** | Filtre çalışmaz |

**Örnek — "Sadece mezunlar, Lisans ve üstü":** Lisans, yüksek lisans veya doktora mezunları geçer; lisans öğrencisi geçmez.

**Örnek — "İkisi de, Lisans ve üstü":** Yarı zamanlı iş için lisans öğrencisi de, mezunu da geçer.

### Neden bu yapı

> **Eski hali** "En az [Lisans]" idi — sadece mezunları kapsıyordu. **Öğrenci almak isteyen işveren** (kafe, mağaza, çağrı merkezi, yarı zamanlı yazılım) için yolu yoktu; lisans 3. sınıftaki öğrenci elenirdi. Yeni yapı bu eksiği kapatıyor.

> **"Eğitim şartı yok" neden var:** İş ilanlarının önemli kısmında (garson, kurye, satış danışmanı, depo) eğitim şartı yok. Bu seçenek olmasa işveren bir şey işaretlemek zorunda kalır ve gereksiz eleme olur.

### Üst seviyeler otomatik işaretlenir

Bir seviye işaretlenince **üstündekiler de otomatik işaretlenir**; işveren isterse kaldırır.

```
Lisans'ı işaretledi  →  Lisans ☑  Yüksek lisans ☑  Doktora ☑
```

> Yoksa işveren sadece "Lisans"ı işaretler, yüksek lisanslı aday yanlışlıkla elenir. Eski "en az lisans" davranışı varsayılan olarak korunuyor.

### İlkokul ve ortaokul — çocuk işçiliği kuralı

"En az ilkokul mezunu" isteyen işveren (örn. bazı üretim işleri) ilkokulu işaretleyebilir. Ama:

| Seçim | İzin |
|---|:---:|
| İlkokul / ortaokul **mezunu** | ✅ |
| İlkokul / ortaokul **öğrencisi** | ❌ **Engellenir** |

**Arayüzde:**
- "Sadece devam eden öğrenciler" seçilirse ilkokul ve ortaokul kutuları **gri düşer**
- "İkisi de" seçilirse bu iki seviye için **sadece mezunlar** filtreden geçer

> İlkokul ve ortaokul öğrencileri çocuktur; onlara iş ilanı açılması hemen her ülkede yasaktır ve platform için ciddi risktir.

### Global

Seviyeler arkada **ISCED** (BM standardı) ile karşılaştırılır; kullanıcı kendi ülkesinin derece adını görür. Eğitim dereceleri ülkeler arasında birebir örtüşmüyor (Almanya'da *Ausbildung*, ABD'de *Associate Degree*) — ortak ölçek olmadan seviye filtresi ülkeler arası çalışmaz.

### Yeni mezun ayrımı

İş ilanında ayrı bir "mezuniyet süresi" filtresi **yok.** "Yeni mezun" isteyen işveren bunu **toplam tecrübe** ile ifade eder:

```
Toplam tecrübe:  [ 0 ] – [ 2 ] yıl
```

Yeni mezunlar üst sıralarda çıkar, 10 yıllık deneyimli aday alt sıralarda kalır (aday yönünde de bu ilan onun için "geriye adım" olarak düşük skor alır). Ayrı bir filtre, iş değiştiren deneyimli birini gereksiz yere elerdi.

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
| **Skor** | ✅ Nitelik uyumu içinde, tercih edilen olarak |

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

**Adayın askerlik durumu işverene profil detayında görünür, ama işveren listeyi askerliğe göre süzemez** — filtre sadece ilan şartı varsa ve sadece tecillileri eler.

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
| **Skor** | ✅ Nitelik uyumu — olmazsa olmaz küme | ✅ Nitelik uyumu — tercih edilen küme |

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
| İlk yayın | **60 gün** — seçim yok |
| Uzatma | 15 / 30 / 60 gün — kesintisiz en fazla **180 gün** |
| Süre dolunca | ⏱ **Süresi doldu** → ilk **30 gün** tekrar yayınlanabilir · **75 gün** içinde sonuç bildirilmeli |

> Ayrıntı → Bölüm 10 ve `isveren-paneli-final.md` Bölüm 4, 11

---

## 8. Kaydetme Akışı — Yapay Zeka Ayrıştırma ve "Algılanan" Satırı

### Akış

```
İşveren [Kaydet] veya [Devam]
        ↓
"Yetenekler analiz ediliyor..."   (1-3 saniye)
        ↓
Her kutunun altında "Algılanan" satırı belirir — onay ekranı YOK
```

```
Olmazsa olmaz yetenekler
┌──────────────────────────────────────────────────┐
│ 5 yıl React bilen, 8 yıl .NET tecrübesi olan,    │
│ PostgreSQL'e hakim                               │
└──────────────────────────────────────────────────┘
Algılanan: React · 5 yıl   .NET · 8 yıl   PostgreSQL   [Düzenle]

Tercih edilen yetenekler
┌──────────────────────────────────────────────────┐
│ Docker ve Kubernetes bilgisi artı, AWS deneyimi  │
└──────────────────────────────────────────────────┘
Algılanan: Docker   Kubernetes   AWS                   [Düzenle]

Tercih edilen bölümler
┌──────────────────────────────────────────────────┐
│ Bilgisayar, elektronik veya yazılım mühendisliği │
└──────────────────────────────────────────────────┘
Algılanan: Bilgisayar Müh.   Elektronik Müh.   Yazılım Müh.   [Düzenle]
```

**[Düzenle] ile işveren yapabilir:**
- Etiket silmek, eklemek
- Etiket adını düzeltmek
- Yılı değiştirmek veya eklemek

**İşveren hiçbir şey yapmak zorunda değil** — doğru gördüyse devam eder.

### Neden onay ekranı yok

Onay ekranı işveren için bir ekran ve bir karar daha demekti; *"onay bekliyor"*, hatırlatma, otomatik kabul gibi yan kurallar doğuruyordu. Etiketlerin **görünür ve düzeltilebilir** olması kaliteyi zorlamadan koruyor.

### Kurallar

| Kural | Neden |
|---|---|
| **Tek istek** | Beş kutu (2 yetenek, 1 bölüm, 2 sertifika) aynı yapay zeka isteğiyle ayrıştırılır |
| **Kutu sınıfı belirler** | Bir etiketin olmazsa olmaz mı tercihen mi olduğu, yazıldığı kutudan gelir — yapay zeka tahmin etmez |
| **Sadece metin değişince çalışır** | İlgili metinlerin özeti (hash) tutulur. Maaş veya konum değişti diye yeniden ayrıştırılmaz |
| **İşverenin düzeltmesi korunur** | Düzeltilen etiket `isveren_duzeltti` olarak işaretlenir — metin sonradan değişip yapay zeka tekrar çalışsa bile **üstüne yazılmaz** |
| **Küçük, hızlı model** | Görev basit; büyük model gereksiz |
| **Kalite sistemde korunur** | Kesin çıktı formatı, mantık kontrolü, test seti, [Düzenle] kullanım oranı izlenir → `eslestirme-motoru.md` 6.6 |
| **Sunucuda çalışır** | Yapay zeka çağrısı Edge Function'da — anahtar istemcide değil |
| **Taslakta da çalışır** | İşveren yayından önce "Algılanan" satırını kontrol edebilir |

### Yapay zeka cevap vermezse

İşveren hiçbir zaman takılıp kalmaz:

```
1. İlan yine kaydedilir, yayınlanabilir
2. Kutunun altında: "Yetenekler birazdan analiz edilecek. İlanınızı yayınlayabilirsiniz."
3. Geçici olarak: adayın yetenek adları ilan metninde aranır (yılsız, var/yok)
4. Ayrıştırma arka planda tekrar denenir
5. Başarılı olunca etiketler SESSİZCE uygulanır — bildirim yok, "Algılanan" satırı dolar
```

### Sonra ne olur

```
Etiketler kaydedildi
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

> Ekranlar, butonlar ve tüm akışlar → `isveren-paneli-final.md` Bölüm 3, 4, 5, 11, 12, 13

### Dört hal

```
TASLAK ──[Yayınla]──> YAYINDA ──[Sonuç bildir]──────────────> SONUÇLANAN
                        │  ^                                   (kalıcı)
        [Duraklat] /    │  │ [Devam ettir] /                       ^
        süre doldu      v  │ [Tekrar yayınla] (ilk 30 gün)         │
                     YAYINDA OLMAYAN ──[Sonuç bildir]──────────────┘
                     (duraklatıldı / süresi doldu)    75. gün: sistem
```

| Durum | Anlamı | Eşleştirmede | Nasıl olur |
|---|---|:---:|---|
| `taslak` | Henüz yayınlanmamış | ❌ | İlan açıldığında · oluşturulduktan **30 gün** sonra silinir |
| `yayinda` | Aktif | ✅ | [Yayınla] · [Devam ettir] · [Tekrar yayınla] |
| `duraklatildi` | Geçici durduruldu — geri açılabilir | ❌ | İşveren [Duraklat] — *"çok başvuru geldi, önce bunları değerlendireyim"* |
| `suresi_doldu` | Yayın süresi bitti — ilk 30 gün geri açılabilir | ❌ | Otomatik |
| **Sonuçlanan** | Kalıcı — tekrar yayınlanamaz | ❌ | İşveren [Sonuç bildir] · veya sistem 75. günde |

**Sonuçlanan ilanın sonucu:**

| Sonuç | Kim |
|---|---|
| `pozisyon_doldu` | İşveren — işe alım yaptı *(kimi aldığını seçer)* |
| `aday_bulunamadi` | İşveren — uygun aday bulamadı |
| `vazgecildi` | İşveren — pozisyon iptal |
| `sonuc_bildirilmedi` | Sistem — süre dolduktan **75 gün** sonra |

> **Sonuçlar neden ayrı:** *Pozisyon doldu* platformun **asıl başarı ölçüsü.** *Uygun aday bulunamadı* eşleştirme veya aday havuzu sorununa işaret eder. *Vazgeçildi* şirketin iç kararı. Üçü şirket raporunda ayrı görünür.

### Yayında olmayan ilan — ortak kural

> **Yeni giriş durur, içerideki her şey devam eder.**

| | |
|---|---|
| Keşfet, yeni başvuru, davet | ❌ Durur · işverenin Adaylar sekmesi kilitli |
| Bekleyen başvurular | ✅ İşveren kabul / red eder — 30 gün işler |
| Cevap bekleyen davetler | ✅ Aday kabul / red eder — 10 gün işler |
| Süreçteki adaylar | ✅ Sohbet, sonuçlandırma — 60 gün işler |
| Duraklatılmışken süre | **İşlemeye devam eder** — biterse *süresi doldu* olur |

### Sonuç bildirilince — her şey biter

| | |
|---|---|
| Bekleyen başvuru, davet, süreçteki adaylar | **Hepsi sonlanır** — adaya sonuca göre mesaj · işe alınanlara işe alım onayı sorusu |
| Sohbetler | Kapanır — geçmiş görünür |
| İlan | Tekrar yayınlanamaz · **[Kopyala]** ile yeni ilan açılabilir · **[Sil]** ile işverenin listesinden kaldırılabilir *(soft delete — rapor değişmez)* |

> Görüşmesi süren işveren sonuç bildirmez — **duraklatır**, işi bitince bildirir.

### Uzatma — süre dolmadan

| | |
|---|---|
| Ne zaman | İlan yayındayken **her zaman** |
| Hatırlatma | Süre bitmeden **3 gün önce** sorumluya 🔔 |
| Seçenekler | 15 / 30 / 60 gün |
| Kesinti | Yok — ilan hiç kapanmaz |
| Sayaç | **Uzatma sayısı +1** |
| Üst sınır | Kesintisiz en fazla **180 gün** — sonra süre dolar |
| Kim yapar | Sorumlu, sahip, yönetici |

> **Neden hazır seçenekler, serbest gün değil:** İşveren "365" yazarsa ilan bir yıl açık kalır — pozisyon çoktan dolmuş olabilir. Platformun en büyük güven sorunlarından biri **ölü ilanlar.**

> **Neden 180 gün sınırı:** İlanın hâlâ gerçekten açık olduğunu işverene ara ara onaylatıyor.

### Tekrar yayınlama — süre dolduktan sonra

| Konu | Davranış |
|---|---|
| Hangi durumdan | **Sadece `suresi_doldu`** — ve sadece süre dolduktan sonraki **ilk 30 gün** |
| İlan kaydı | **Aynı ilan** — yeni ilan açılmaz · yeni 60 gün · yayın sayısı +1 |
| Aksiyon geçmişi | **Korunur** — gizlenen, reddedilen, başvurmuş adaylar bir daha görünmez |
| Embedding | Metin değişmediyse **yeniden üretilmez** |
| Eşleştirme sonuçları | Süre dolunca silinmişti → **yeniden hesaplanır** |
| Bekleyen başvurular | Zaten **açık kalmıştı** — olduğu gibi devam eder |
| Sonuçlanan ilan | **Tekrar yayınlanamaz** — [Kopyala] ile yeni ilan |

> **Neden aynı ilan:** Yeni kayıt açılsaydı geçmiş kaybolurdu — aynı aday aynı ilanı tekrar görür, reddedilen tekrar başvurur.

### Süresi dolan ilan — 30 + 45 gün

| Gün *(süre dolduktan sonra)* | Ne olur |
|---|---|
| 0 | ⏱ Süresi doldu — bekleyenler ve süreçtekiler devam eder |
| 23 | Sorumluya 🔔 *"Tekrar yayınlamak için 7 gün kaldı."* |
| 30 | **[Tekrar yayınla] kilitlenir** · ilanın embedding'i ve "bir daha gösterme" kayıtları silinir |
| 68 | Sorumluya 📱 🔔 *"Sonucu 7 gün içinde bildirin."* |
| 75 | Sonuç bildirilmediyse → **❓ Sonuç bildirilmedi** · kalan süreçler nötr mesajla sonlanır · şirket raporunda görünür |

### İki ayrı sayaç

| Sayaç | Ne zaman artar | Ne anlatır |
|---|---|---|
| **Yayın sayısı** | Süresi dolup **tekrar yayınlandığında** | İlanın kaç kez açıldığı |
| **Uzatma sayısı** | **Kapanmadan uzatıldığında** | Pozisyonun ne kadar süredir doldurulamadığı |

> "4 kez uzatıldı" bilgisi işverene ipucu (belki maaş veya şartlar gözden geçirilmeli), admin'e ölü ilan sinyali.

### Durum değişince eşleştirme ne olur

| Olay | Eşleştirme sonuçları | Embedding | Aksiyon geçmişi |
|---|---|---|---|
| Duraklatıldı / süresi doldu | Silinir | Kalır | Kalır |
| Devam ettirildi / tekrar yayınlandı | **Yeniden hesaplanır** | Kalır | Kalır |
| Süre dolduktan sonra 30. gün | — | Silinir | "Bir daha gösterme" silinir · diğerleri kalır |
| Sonuç bildirildi | Silinir | Silinir | "Bir daha gösterme" silinir · diğerleri kalır |

> Ayrıntı → `eslestirme-motoru.md` 12.6

---

## 11. Eşleştirmeye Etkisi — Skor Bileşenleri

İlandaki alanların skora nasıl yansıdığı:

```
skor = 0.65 × anlamsal       ← ilan vektörü (unvan, yetenek/bölüm/sertifika metinleri, iş tanımı)
     + 0.20 × nitelik uyumu  ← ilan etiketleri
     + 0.10 × tecrübe        ← toplam tecrübe min–max
     + 0.05 × kalite         ← aday yönünde: ilan kalitesi
```

### Nitelik uyumu *(eski adı: yetenek bileşeni)*

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

nitelik uyumu = 0.75 × zorunlu_puan + 0.25 × tercihen_puan
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

nitelik uyumu = 0.75 × 0.60 + 0.25 × 0.67 = 0.62
```

**Eşleşme nasıl yapılır:** Adayın yetenek ve sertifika adları, ilanın etiketleriyle (yapay zekanın algıladığı + işverenin düzelttiği) karşılaştırılır. Yazım farkları ("ReactJS" / "React.js") benzerlik kontrolüyle yakalanır.

**İki yönde aynı** — nitelik uyumunda yön farkı yok.

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

**Girmeyenler:** Meslek alanı, konum, çalışma şekli, çalışma tipi, iş dili, eğitim şartı, diğer şartlar, toplam tecrübe, maaş, başlama zamanı, serbest şartlar, şirket profilinin tamamı.

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
| **Sayaçlar** | İlanın satırında hazır kolonlar; başvuru/davet tetikleyiciyle, görüntülenme dakikalık toplu işle (bkz. `isveren-paneli-final.md`) |
| **Yetkiler** | RLS — uzman sadece sorumlu olduğu ilanları görür ve düzenler |
| **Taslak** | Her adımda otomatik kayıt |
| **Durum geçişleri** | `suresi_doldu`, 30. gün kilidi, 75. gün `sonuc_bildirilmedi` ve taslağın 30. günde silinmesi otomatik (bakım işleri); diğerleri işveren aksiyonuyla |

---

## 15. Diğer Dokümanlara Etkisi

| Doküman | Değişecek |
|---|---|
| **Eşleştirme Motoru** | ✅ İşlendi — LLM sadece ilan kaydında ayrıştırma · nitelik uyumu · ilan kalitesi kriterleri · askerlik filtresi · ilan durumu değişince motorun davranışı |
| **Aday Profili** | ✅ İşlendi — diğer bilgiler zorunlu · askerlikte "Yükümlü değil" · askerlik profil detayında görünür, süzülemez · çalışma tipi çoklu |
| **İşveren Paneli** | ✅ İşlendi — `isveren-paneli-final.md`: ilan yaşam döngüsü, sonuç bildirme, butonlar, sayaçlar |
| **Staj İlanı** | `staj-ilani.md` — bu dokümanın yapısını temel alır; eğitim şartı iki ilan tipinde aynı mantıkla çalışır |
