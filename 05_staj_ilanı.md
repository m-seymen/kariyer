# İş Eşleştirme Projesi — Staj İlanı

> Son güncelleme: 20 Eylül 2026
> **Kapsam: global** — uygulama tüm dünyaya açık olacak
> Durum: Ekip toplantısı sonrası nihai karar
> İlgili dokümanlar: `is-ilani-v3.md`, `isveren-paneli-v2.md`, `eslestirme-motoru.md`

---

## 0. Bu Dokümanı Nasıl Okumalı

Staj ilanı, iş ilanıyla **aynı altyapıyı** kullanır — aynı tablo, aynı eşleştirme motoru, aynı yapay zeka ayrıştırması, aynı onay ekranı, aynı yayın kuralları.

Bu doküman **staja özgü kararları** detaylı anlatır. İş ilanıyla birebir aynı olan kısımlar kısa tutulmuş ve `is-ilani-v3.md`'ye referans verilmiştir.

**Her ülkede aynı form.** Ülkeye göre gizlenen veya değişen alan yok — ücret, staj türü ve diğer tüm alanlar her ülkede aynı şekilde sorulur. İşveren kendi ülkesinin mevzuatına uygun seçimi yapar.

---

## 1. İş İlanından Farkları — Tek Bakışta

| Konu | İş ilanı | Staj ilanı |
|---|---|---|
| **Çalışma tipi** | Tam / yarı zamanlı / proje bazlı / dönemsel — katı filtre | ❌ **Yok** |
| **Çalışma düzeni** | — | ✅ Tam / yarı zamanlı — **sadece gösterim** |
| **Toplam tecrübe** | Min–max yıl — skor | ❌ **Yok** — skorda nötr (0.5) |
| **Eğitim — kimler başvurabilir** | Öğrenci / mezun / ikisi / **şart yok** | Öğrenci / mezun / ikisi — **şart yok seçeneği yok** |
| **Eğitim — seviyeler** | İlkokul → doktora | **Lise → doktora** |
| **Askerlik** | ✅ Zorunlu seçim — şartlıysa katı filtre | ❌ **Yok** |
| **Maaş** | Aralık (min–max), opsiyonel | **Ücret:** ücretli (tek tutar, aylık) / ücretsiz — zorunlu |
| **Staj süresi** | — | ✅ Zorunlu — gösterim |
| **Başlangıç** | Opsiyonel | ✅ Zorunlu — gösterim |
| **Staj türü** | — | ✅ Zorunlu — gösterim |
| **İş tanımı** | "İş tanımı" | **"Staj tanımı"** — aynı yapı |
| Diğer her şey | | **Aynı** |

---

## 2. Tüm Alanlar — Tek Tablo

| Alan | Adım | Zorunlu | Giriş | LLM ayrıştırır | Embedding | Katı filtre | Skor | Adaya görünür |
|---|:---:|:---:|---|:---:|:---:|:---:|:---:|:---:|
| **Meslek alanı** | 1 | ✅ | Kapalı liste | ❌ | ❌ | ✅ | — | ✅ |
| **Unvan** | 1 | ✅ | Serbest + öneri | ❌ | ✅ | ❌ | — | ✅ |
| **İş yeri konumu** | 1 | ✅ | Konum ağacı + şirket lokasyonu kısayolu | ❌ | ❌ | ✅ | — | ✅ |
| **Çalışma şekli** | 1 | ✅ | Ofis / hibrit / uzaktan — çoklu | ❌ | ❌ | ✅ | — | ✅ |
| **Çalışma düzeni** | 1 | ✅ | Tam zamanlı / yarı zamanlı | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Olmazsa olmaz yetenekler** | 2 | ❌ | Serbest metin | ✅ ad + yıl | ✅ ham metin | ❌ | ✅ | ✅ |
| **Tercih edilen yetenekler** | 2 | ❌ | Serbest metin | ✅ ad | ✅ ham metin | ❌ | ✅ | ✅ |
| **Kimler başvurabilir** | 2 | ✅ | Öğrenci / mezun / ikisi | ❌ | ❌ | ✅ | — | ✅ |
| **Hangi seviyede** | 2 | ✅ | Lise → doktora — çoklu | ❌ | ❌ | ✅ | — | ✅ |
| **Tercih edilen bölümler** | 2 | ❌ | Serbest metin | ✅ bölüm adları | ✅ ham metin | ❌ | ✅ | ✅ |
| **İş dili** | 2 | ✅ | Kapalı liste, çoklu | ❌ | ❌ | ✅ | — | ✅ |
| **Sürücü belgesi** | 2 | ✅ *seçim* | Gerekli değil / Gerekli + sınıf | ❌ | ❌ | ✅ *gerekliyse* | — | ✅ |
| **Seyahat** | 2 | ✅ *seçim* | Gerekli değil / Engeli olmamalı | ❌ | ❌ | ✅ *gerekliyse* | — | ✅ |
| **Vardiya** | 2 | ✅ *seçim* | Gerekli değil / Vardiyalı çalışabilmeli | ❌ | ❌ | ✅ *gerekliyse* | — | ✅ |
| **Olmazsa olmaz sertifika ve belgeler** | 2 | ❌ | Serbest metin | ✅ ad | ✅ ham metin | ❌ | ✅ | ✅ |
| **Tercih edilen sertifika ve belgeler** | 2 | ❌ | Serbest metin | ✅ ad | ✅ ham metin | ❌ | ✅ | ✅ |
| **Serbest şartlar** | 2 | ❌ | Serbest metin | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Staj tanımı** | 3 | ✅ *min 100* | Serbest metin, max 3.000 | ❌ | ✅ | ❌ | — | ✅ |
| **Staj süresi** | 4 | ✅ | Kapalı liste | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Başlangıç** | 4 | ✅ | Kapalı liste | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Ücret** | 4 | ✅ | Ücretli (+ tutar) / ücretsiz | ❌ | ❌ | ❌ | ✅ *kalite* | ✅ |
| **Staj türü** | 4 | ✅ | Zorunlu / gönüllü / ikisi | ❌ | ❌ | ❌ | ❌ | ✅ |

### Özet — hangi alan nereye gidiyor

**Embedding'e girenler (ilan vektörü):**
```
Unvan
Olmazsa olmaz yetenekler              (ham metin)
Tercih edilen yetenekler              (ham metin)
Tercih edilen bölümler                (ham metin)
Olmazsa olmaz sertifika ve belgeler   (ham metin)
Tercih edilen sertifika ve belgeler   (ham metin)
Staj tanımı
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
Ne arıyor (aday "staj" veya "ikisi de" seçmiş olmalı)
Meslek alanı · İş yeri konumu · Çalışma şekli · İş dili
Kimler başvurabilir · Hangi seviyede
Sürücü belgesi · Seyahat · Vardiya (gerekliyse)
```

**Sadece skor:**
```
Yetenekler · Sertifikalar · Bölümler  → yetenek bileşeni
Ücret belirtilmiş mi                  → kalite bileşeni
(Tecrübe bileşeni stajda nötr — 0.5)
```

**Sadece adaya gösterim:**
```
Çalışma düzeni · Staj süresi · Başlangıç · Ücret tutarı · Staj türü · Serbest şartlar
```

---

## 3. Adım 0 — Staj İlanı Açma

```
📋 İlanlarım                         [+ İş ilanı aç]  [+ Staj ilanı aç]
```

| | |
|---|---|
| **Kim açabilir** | Sahip, yönetici, uzman |
| **İlan tipi** | Butona göre otomatik `staj` — kullanıcı ayrıca seçmez |
| **Sorumlu** | İlanı açan kişi; sahip ve yönetici devredebilir |
| **Taslak** | Her adımda otomatik kayıt |

**Aday tarafı:** Staj ilanlarını görmesi için profilinde **"Ne arıyor"** alanında **staj** veya **ikisi de** seçili olmalı — katı filtre.

---

## 4. Adım 1 — Pozisyon ve Temel Bilgiler

| Alan | Zorunlu | Giriş | Eşleştirmede |
|---|:---:|---|---|
| Meslek alanı | ✅ | Kapalı liste | Katı filtre |
| Unvan | ✅ | Serbest + öneri — *"Yazılım Stajyeri"*, *"Muhasebe Stajyeri"* | Embedding |
| İş yeri konumu | ✅ | Konum ağacından serbest + şirket lokasyonları kısayol | Katı filtre |
| Çalışma şekli | ✅ | Ofis / hibrit / uzaktan — çoklu | Katı filtre |
| **Çalışma düzeni** | ✅ | Tam zamanlı / yarı zamanlı | **Sadece gösterim** |

Meslek alanı, unvan, iş yeri konumu ve çalışma şekli **iş ilanıyla birebir aynı** — konum ağacı, uzaktan istisnası ve filtre kuralları için → `is-ilani-v3.md` Bölüm 4.

### İş yeri konumu

```
İş yeri konumu *:  [Gebze, Kocaeli              ]   ← konum ağacından serbest arama
Hızlı seçim:       [Genel Merkez] [Ankara Ofisi] [Fabrika]
```

- **Şirket lokasyonlarından bağımsız** — işveren istediği herhangi bir konumu seçer
- Şirket lokasyonları sadece kısayol; tıklanırsa konum ilana **kopyalanır**, bağlanmaz
- Şirket lokasyonu sonradan değişse veya silinse ilan **etkilenmez**

### Çalışma düzeni — staja özgü

```
Çalışma düzeni *
○ Tam zamanlı      (haftanın tamamı — genelde yaz stajı)
○ Yarı zamanlı     (haftada birkaç gün — ders dönemiyle birlikte)
```

> **Neden iş ilanındaki "çalışma tipi" yok:** "Proje bazlı", "dönemsel" staj için anlamsız. Staj için asıl soru: öğrenci haftanın her günü mü gelecek, yoksa ders döneminde birkaç gün mü?

> **Neden filtre değil:** Öğrencinin müsaitliği dönemden döneme değişiyor — yazın tam zamanlı, okul döneminde yarı zamanlı. Aday profilinde sabit bir bilgi olarak tutulsa yanlış eleme yapar. Aday ilanı görür, kendi değerlendirir.

---

## 5. Adım 2 — Aranan Nitelikler

### 5.1 Yetenekler — iki serbest alan

**İş ilanıyla birebir aynı.**

```
Olmazsa olmaz yetenekler
┌──────────────────────────────────────────────────┐
│ Python ve SQL bilgisi                            │
└──────────────────────────────────────────────────┘

Tercih edilen yetenekler
┌──────────────────────────────────────────────────┐
│ Git kullanmış olması artı                        │
└──────────────────────────────────────────────────┘
```

| | Olmazsa olmaz | Tercih edilen |
|---|---|---|
| Zorunlu | ❌ | ❌ |
| LLM ayrıştırır | ✅ ad + yıl | ✅ ad |
| Embedding | ✅ ham metin | ✅ ham metin |
| Katı filtre | ❌ | ❌ |
| Skor | ✅ Yetenek bileşeni — 0.75 | ✅ Yetenek bileşeni — 0.25 |

> **Stajda beklenti farkı:** İşveren genelde yıl yazmaz (*"Python bilgisi"*, *"Excel kullanabilen"*). Yıl yazmazsa skor sadece var/yok bakar; yazarsa iş ilanındaki kısmi puan formülü çalışır. Ayrı bir kural gerekmiyor.

### 5.2 Eğitim — staja özgü

```
Kimler başvurabilir? *
○ Sadece devam eden öğrenciler
○ Sadece mezunlar
○ İkisi de

Hangi seviyede? *
☐ Lise   ☐ Ön lisans   ☑ Lisans   ☑ Yüksek lisans   ☑ Doktora
```

| | Kimler başvurabilir | Hangi seviyede |
|---|---|---|
| Zorunlu | ✅ | ✅ En az bir |
| Giriş | Tek seçim | Çoklu seçim |
| Eşleştirme | **Katı filtre** | **Katı filtre** |

**Üst seviyeler otomatik işaretlenir:** Lisans işaretlenince yüksek lisans ve doktora da işaretlenir; işveren isterse kaldırır. Yoksa yüksek lisans öğrencisi yanlışlıkla elenir.

**Filtre kuralı:**

| İşveren seçimi | Aday geçer, eğer… |
|---|---|
| **Sadece devam eden öğrenciler** | Seçilen seviyelerden birinde **"devam ediyorum"** işaretli eğitim kaydı var |
| **Sadece mezunlar** | Seçilen seviyelerden birinde **tamamlanmış** eğitim kaydı var |
| **İkisi de** | Yukarıdakilerden herhangi biri |

**Örnek — "Sadece devam eden öğrenciler, Lisans":**
```
Aday A: Lisans 3. sınıf, devam ediyor   → ✅
Aday B: Lisans 2024 mezunu              → ❌ (mezun)
Aday C: Ön lisans 2. sınıf, devam ediyor → ❌ (seviye seçilmemiş)
```

**Örnek — "Sadece mezunlar, Lisans":**
```
Aday A: Lisans 2025 mezunu              → ✅
Aday B: Lisans 2015 mezunu              → ✅  (mezuniyet süresi sınırı yok)
Aday C: Lisans 3. sınıf, devam ediyor    → ❌ (henüz mezun değil)
```

> **Mezuniyet süresi sınırı yok.** Çok yıl önce mezun olmuş birinin staj ilanına düşmesi pratikte nadir — aday tarafında "Ne arıyor: staj" seçmesi gerekiyor ve deneyimli kişiler genelde staj seçmez. İşveren kartta mezuniyet yılını görüp kendisi karar verir.

> **Neden "Eğitim şartı yok" seçeneği yok:** Staj tanımı gereği öğrenci veya yeni mezun odaklı. İş ilanında bu seçenek var.

> **Neden ilkokul ve ortaokul yok:** Staj bu seviyeler için anlamsız; ayrıca bu seviyelerdeki öğrenciler çocuk.

> **Veri zaten var:** Aday profilindeki her eğitim kaydında seviye (ISCED ile), "devam ediyorum" işareti ve mezuniyet yılı bulunuyor. Ek alan gerekmiyor.

> **Global:** Seviyeler arkada **ISCED** ile karşılaştırılır; kullanıcı kendi ülkesinin derece adını görür.

### 5.3 Tercih edilen bölümler

**İş ilanıyla aynı.** Serbest metin, birden fazla bölüm, LLM ayrıştırır, embedding'e girer, skorda tercih edilen kümede, katı filtre değil.

> Stajda bölüm genelde daha belirleyici — ama yine de katı filtre değil. Farklı bölümden ama ilgili yeteneklere sahip öğrenci elenmemeli.

### 5.4 İş dili

**İş ilanıyla aynı.** Zorunlu, kapalı liste, çoklu, varsayılan şirket ülkesinin dili, katı filtre (kesişim).

### 5.5 Diğer şartlar — üç zorunlu seçim

```
Diğer şartlar *

Sürücü belgesi:   ○ Gerekli değil   ○ Gerekli → [B] [C] [D] [E]
Seyahat:          ○ Gerekli değil   ○ Seyahat engeli olmamalı
Vardiya:          ○ Gerekli değil   ○ Vardiyalı çalışabilmeli
```

Kurallar iş ilanıyla aynı: her satırda bir seçim zorunlu, varsayılan seçili gelmez; "Gerekli" seçilenler katı filtre.

> **Askerlik neden yok:** Öğrencilerin büyük kısmı **tecilli**. İş ilanındaki "askerlik engeli olmamalı" şartı stajda neredeyse tüm erkek öğrencileri eler — anlamsız ve zararlı. Staj formunda askerlik satırı gösterilmez.

### 5.6 Sertifika ve belgeler — iki serbest alan

**İş ilanıyla aynı.**

```
Olmazsa olmaz sertifika ve belgeler (opsiyonel)
Tercih edilen sertifika ve belgeler (opsiyonel)
```

LLM ayrıştırır (ad), embedding'e girer, skorda yetenek bileşeni içinde, katı filtre değil. Adaydaki süresi dolmuş sertifika yok sayılır.

### 5.7 Serbest şartlar

**İş ilanıyla aynı.** Opsiyonel, hiçbir yere girmez, sadece adaya gösterim.

### 5.8 Toplam tecrübe — yok

> **Neden yok:** Stajyerin tecrübesi genelde sıfır veya çok az. Bu alanın skordaki işi "fazla nitelikli / az nitelikli" ayrımıydı — stajda anlamsız.
>
> **Skorda:** Tecrübe bileşeni staj ilanlarında **her aday için 0.5 (nötr)** alır.

---

## 6. Adım 3 — Staj Tanımı

```
Staj tanımı *
┌──────────────────────────────────────────────────┐
│                                                  │
│                                                  │
└──────────────────────────────────────────────────┘
[✨ Taslak oluştur]                        0 / 3.000
```

| | |
|---|---|
| Zorunlu | ✅ |
| Minimum / maksimum | 100 / 3.000 karakter |
| Giriş | Tek serbest alan — soru, yer tutucu, öneri yok |
| LLM ayrıştırır | ❌ |
| **Embedding** | ✅ Tamamı |
| Katı filtre / skor | ❌ — skoru dolaylı etkiler (anlamsal bileşen üzerinden) |
| Kalite skoru | ❌ |

**İş tanımıyla birebir aynı yapı**, sadece adı farklı.

### ✨ Taslak oluştur

İşverenin doldurduğu alanlardan bir taslak üretir: unvan, konum, çalışma şekli ve düzeni, yetenekler, kimler başvurabilir, seviye, staj süresi, başlangıç, ücret, staj türü.

> *Örnek:* "Yaz döneminde İstanbul ofisimizde tam zamanlı, 2 aylık yazılım stajı. Lisans öğrencilerini bekliyoruz. Python ve SQL bilgisi olan, gerçek projelerde ekip içinde çalışmak isteyen adaylar arıyoruz. Staj ücretlidir; zorunlu ve gönüllü stajyer kabul ediyoruz."

İşveren düzenler, kendi eklemelerini yapar, kaydeder. Sadece butona basılınca çalışır.

---

## 7. Adım 4 — Staj Şartları

| Alan | Zorunlu | Giriş | Eşleştirmede |
|---|:---:|---|---|
| **Staj süresi** | ✅ | Kapalı liste | Sadece gösterim |
| **Başlangıç** | ✅ | Kapalı liste | Sadece gösterim |
| **Ücret** | ✅ | Ücretli (+ tutar) / ücretsiz | Kalite skoru |
| **Staj türü** | ✅ | Zorunlu / gönüllü / ikisi | Sadece gösterim |

### 7.1 Staj süresi

```
Staj süresi *
○ 1 ay   ○ 2 ay   ○ 3 ay   ○ 6 ay   ○ Dönem boyunca   ○ Esnek
```

### 7.2 Başlangıç

```
Başlangıç *
○ Hemen
○ Yaz dönemi
○ Belirli tarih → [Ay ▾] [Yıl ▾]
○ Esnek
```

> Staj genelde **dönemlik** — "yaz stajı" en yaygın kalıp, bu yüzden ayrı seçenek.

### 7.3 Ücret

```
Ücret *
○ Ücretli → [______] [TRY ▾] / ay   ○ Net  ○ Brüt
○ Ücretsiz
```

| | |
|---|---|
| Zorunlu | ✅ Seçim zorunlu |
| Ücretli seçilirse | Tutar, para birimi ve net/brüt **dolu olmalı** |
| Tutar biçimi | **Tek tutar, aylık** *(iş ilanındaki min–max aralık yerine)* |
| Para birimi | Varsayılan: şirketin ülkesi |
| Katı filtre | ❌ |
| **Kalite skoru** | ✅ **Ücretli + tutar belirtilmiş** ilan avantajlı |

> **Neden tek tutar:** Staj ücretleri genelde sabit — aralık vermek gereksiz karmaşıklık.

> **"Ücretsiz" her ülkede seçilebilir.** Ülkeye göre gizlenen seçenek yok — işveren kendi ülkesinin mevzuatına uygun seçimi yapar.

### 7.4 Staj türü

```
Kabul edilen staj türü *
○ Zorunlu staj    (okulun zorunlu tuttuğu staj — okul evrakı imzalanır, sigorta yapılır)
○ Gönüllü staj
○ İkisi de
```

| | |
|---|---|
| Zorunlu | ✅ |
| Eşleştirmede | **Sadece gösterim** |
| Ülke | **Her ülkede gösterilir** — kavram olmayan ülkelerde işveren "İkisi de" veya "Gönüllü" seçer |

> **Neden önemli:** Zorunlu stajyer almak bazı ülkelerde işveren için **ek yükümlülük** (okul formu, staj sigortası). Bazı şirketler bunu yapmak istemez, bazıları sadece zorunlu stajyer alır. Zorunlu stajını yapacak yer arayan öğrenci, "sadece gönüllü" diyen ilanı baştan görüp geçebilir.

> **Neden filtre değil:** Aday profilinde "zorunlu stajım var" bilgisi yok ve dönemden döneme değişiyor. Kartta açıkça görünür, aday kendi değerlendirir.

---

## 8. Kaydetme Akışı

**İş ilanıyla birebir aynı** — ayrıntılar `is-ilani-v3.md` Bölüm 8.

```
[Kaydet] → "Yetenekler analiz ediliyor..."
        ↓
┌──────────────────────────────────────────────────────────┐
│ Şunları anladık — doğru mu?                              │
│                                                          │
│ Olmazsa olmaz yetenekler:     [Python ✕] [SQL ✕]          │
│ Tercih edilen yetenekler:     [Git ✕]                    │
│ Tercih edilen bölümler:       [Bilgisayar Müh. ✕]        │
│                               [Yazılım Müh. ✕]           │
│ Olmazsa olmaz sertifikalar:   —                          │
│ Tercih edilen sertifikalar:   —                          │
│                                                          │
│ [+ Ekle]                         [Doğru, devam et]       │
└──────────────────────────────────────────────────────────┘
```

| Kural | |
|---|---|
| Tek istek | Beş kutu aynı yapay zeka isteğiyle ayrıştırılır |
| Kutu sınıfı belirler | Zorunlu / tercihen ayrımını yapay zeka tahmin etmez |
| Sadece metin değişince | Hash kontrolü |
| İşveren onayı şart | Skor onaylı etiketlerle çalışır |
| Hata durumunda | İlan yine kaydedilir; arka planda tekrar denenir |

---

## 9. Önizleme, Yayın ve Süre

**İş ilanıyla birebir aynı.**

```
[Önizle]   → adayın göreceği hali

[Yayınla]
  → Şirket profilinde tanıtım dolu mu?    Hayır → tanıtım alanına
  → Cep telefonum doğrulanmış mı?          Hayır → SMS doğrulama
  → İkisi de evet → yayına girer, 60 gün başlar
```

| | |
|---|---|
| Yayın süresi | Sabit **60 gün** |
| Uzat | İstediği zaman — **15 / 30 / 60 gün**, hatırlatma yok, uzatma sayısı +1 |
| Üst sınır | Kesintisiz en fazla **180 gün** |
| Süre dolunca | **[Tekrar yayınla]** → yeni 60 gün, yayın sayısı +1, aynı ilan, geçmiş korunur |

> **Staj ilanının "yayın süresi" ile "staj süresi" farklı şeyler:** Yayın süresi ilanın platformda kaç gün açık kaldığı (60 gün). Staj süresi stajyerin şirkette ne kadar çalışacağı (1 ay, 2 ay…).

---

## 10. İlan Durumları

**İş ilanıyla birebir aynı.**

| Durum | Eşleştirmede |
|---|:---:|
| `taslak` | ❌ |
| `yayinda` | ✅ |
| `duraklatildi` | ❌ |
| `dolduruldu` | ❌ |
| `kapatildi` | ❌ |
| `suresi_doldu` | ❌ |

Tekrar yayınlama `süresi_doldu`, `kapatildi` ve `dolduruldu` durumlarından yapılabilir. Meslek alanı değiştirilirse "yeni ilan olarak açın" yönlendirmesi.

> **Dönemlik staj için ipucu:** Her yaz aynı staj programını açan şirket, geçen yılki ilanı **[Tekrar yayınla]** ile açabilir — geçen yıl geçilen veya reddedilen adaylar tekrar karşısına çıkmaz.

---

## 11. Eşleştirmeye Etkisi

### Skor

```
skor = 0.65 × anlamsal       ← ilan vektörü (iş ilanıyla aynı yapı)
     + 0.20 × yetenek        ← iş ilanıyla aynı formül
     + 0.10 × tecrübe        ← STAJDA NÖTR: her aday 0.5
     + 0.05 × kalite         ← aday yönü: "ücret belirtilmiş" en ağır kriter
```

| Bileşen | İş ilanı | Staj ilanı |
|---|---|---|
| Anlamsal | Aynı | Aynı |
| Yetenek | Aynı formül | Aynı formül |
| **Tecrübe** | Min–max ile iki yönlü | **0.5 — nötr** |
| **Kalite — en ağır kriter** | Maaş aralığı belirtilmiş (0.50) | **Ücretli + tutar belirtilmiş (0.50)** |

**Kalite — aday yönü (staj):**

| Kriter | Puan |
|---|:---:|
| **Ücretli + tutar belirtilmiş** | **0.50** |
| İmkanlar alanı dolu *(şirket profili)* | 0.20 |
| Gerçek logo yüklenmiş | 0.15 |
| Web sitesi veya sosyal medya var | 0.15 |

> **"Ücretsiz" seçen ilan** bu kriterden puan alamaz — ücretli staj adayların Keşfet'inde biraz yukarı çıkar. Zorlama yok, teşvik var.

**Tecrübe neden nötr, sıfır değil:** Sıfır olsaydı tüm staj ilanları skor bakımından aynı miktarda geri kalırdı — sıralamada fark yaratmazdı ama mutlak skorları iş ilanlarıyla karşılaştırılamaz hale getirirdi. Nötr değer, bileşeni etkisiz bırakıyor.

### Katı filtre — iş ilanıyla karşılaştırma

| Filtre | İş ilanı | Staj ilanı |
|---|:---:|:---:|
| Ne arıyor (iş / staj) | ✅ | ✅ |
| Meslek alanı | ✅ | ✅ |
| İş yeri konumu | ✅ | ✅ |
| Çalışma şekli | ✅ | ✅ |
| İş dili | ✅ | ✅ |
| Çalışma tipi | ✅ | ❌ |
| Kimler başvurabilir + seviye | ✅ *(şart yok seçilmediyse)* | ✅ |
| Sürücü belgesi, seyahat, vardiya | ✅ *gerekliyse* | ✅ *gerekliyse* |
| Askerlik | ✅ *şartlıysa* | ❌ |

### İlan vektörü

İş ilanıyla aynı yapı — sadece "İş tanımı" yerine "Staj tanımı":
```
[Unvan]
Olmazsa olmaz yetenekler:             [ham metin]
Tercih edilen yetenekler:             [ham metin]
Tercih edilen bölümler:               [ham metin]
Olmazsa olmaz sertifika ve belgeler:  [ham metin]
Tercih edilen sertifika ve belgeler:  [ham metin]
Staj tanımı:                          [tam metin]
```

---

## 12. Adaya Ne Görünür

### Keşfet kartı

```
┌──────────────────────────────────────────────┐
│ [STAJ İLANI]                                 │
│ Yazılım Stajyeri                             │
│ ABC Teknoloji  ✓ Doğrulanmış şirket          │
│ 📍 Kadıköy, İstanbul · Hibrit · Tam zamanlı   │
│ ⏱ 2 ay · Yaz dönemi                          │
│ 💰 15.000 TRY / ay (net)                      │
│ 📋 Zorunlu ve gönüllü staj                    │
│                                              │
│ Olmazsa olmaz: Python ✓  SQL ✗               │
└──────────────────────────────────────────────┘
```

- **Staj rozeti** iş ilanından farklı renkte — aday tıklamadan ayırt eder
- Ücretsizse: *"💰 Ücretsiz"*
- Olmazsa olmaz yeteneklerde adayın sahip oldukları ✓ ile işaretlenir

### İlan detayı

Tüm alanlar + şirket profili (tanıtım, imkanlar, şirket bilgileri).

**Adaya gösterilmeyenler:** İlanın sorumlusu, uzatma ve yayın sayıları, ilan sayaçları.

---

## 13. Aday Tarafında Gerekenler

Staj ilanının çalışması için aday profilinde **ek alan gerekmiyor** — kullanılan veriler zaten var:

| Staj ilanı alanı | Aday profilindeki karşılığı |
|---|---|
| Ne arıyor | Adım 2 — "iş / staj / ikisi de" |
| Kimler başvurabilir | Adım 3 — eğitim kayıtlarındaki "devam ediyorum" işareti |
| Hangi seviyede | Adım 3 — eğitim kayıtlarının seviyesi (ISCED) |
| Sürücü belgesi, seyahat, vardiya | Adım 10 — zorunlu seçimler |

---

## 14. Genel Kurallar

| Konu | Karar |
|---|---|
| **Platform yaş sınırı** | 18+ *(auth kararıyla aynı)* |
| **Ülke farklılıkları** | Yok — form her ülkede aynı; ücretsiz seçeneği ve staj türü her yerde görünür |
| **Mevzuata uyum** | İşveren, kendi ülkesinin staj mevzuatına uygun seçimleri yapmaktan sorumludur — kullanım koşullarında belirtilir |

---

## 15. Diğer Dokümanlara Etkisi

| Doküman | Değişecek |
|---|---|
| **İş İlanı** | Eğitim alanı aynı yapıya geçti: "Kimler başvurabilir" (öğrenci / mezun / ikisi / **şart yok**) + "Hangi seviyede" (**ilkokul → doktora**) → `is-ilani-v3.md` |
| **Eşleştirme Motoru** | Staj ilanında tecrübe bileşeni 0.5; kalite bileşeninde "ücret belirtilmiş"; "kimler başvurabilir + seviye" filtresi; çalışma tipi ve askerlik filtreleri staj için devre dışı |
| **Aday Profili** | Değişiklik yok — gereken veriler zaten mevcut |
