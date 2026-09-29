# İş Eşleştirme Projesi — Şirket Profili

> Son güncelleme: 20 Eylül 2026
> İlgili dokümanlar: `giris-kayit-kararlari.md`, `sirket-dogrulama-ekrani.md`

---

## Temel İlke

**Şirket profili ne embedding'e ne filtreye girer.**

Eşleştirmenin tamamı **ilan bazlı** yapılır. Şirket profili yalnızca adaya gösterim içindir — aday kararını verirken okuyacağı bilgidir.

### Bu kararın iki sonucu

**1. İlan metinleri kritik hale gelir.**
Eşleştirmenin dayanacağı tek metin ilandır. İşveren *"Backend Developer aranıyor, Java bilen"* diye üç satır yazarsa vektör zayıf kalır ve eşleştirme kötü çalışır. Bu yüzden ilan oluşturmada **yönlendirilmiş sorular zorunlu** olmalıdır.

**2. Konum ve çalışma şekli ilana taşınır.**
Konum katı filtredir ve şirket profili filtreye girmediğine göre, filtrenin kullanacağı konum `ilanlar.lokasyon_id` olmalıdır. Şirket profilindeki lokasyon listesi bir **kaynaktır**; filtreye giren şey ilanın seçtiği lokasyondur.

Aynısı çalışma şekli için de geçerlidir — aynı şirketin bir ilanı uzaktan, diğeri ofis olabilir.

### Doğrulama bilgileri ayrıdır

Vergi numarası, vergi levhası ve diğer belgeler **yalnızca sizde durur**, adaya gösterilmez. Şirket profili adaya görünen yüzdür.

---

## Profil 4 Adımda Oluşur

| Adım | İçerik | Süre | Durum |
|:---:|---|:---:|---|
| **1** | Kimlik | ~2 dk | Zorunlu |
| **2** | Konum | ~1 dk | Zorunlu |
| **3** | Tanıtım | ~5 dk | Kısmen zorunlu |
| **4** | İmkanlar ve görseller | ~3 dk | Opsiyonel |

### Yayına girme eşiği

> **Adım 1 + Adım 2 + Adım 3'ün ilk sorusu**

Bu kadarı tamamlanınca ilan açılabilir. Gerisi profil puanını yükseltir ama zorunlu değildir.

**Neden bu eşik:** İşveren zaten belge yükleyip onay bekledi, sabrı sınırlı. Dört adımı da zorunlu yaparsanız yarısı yarıda bırakır. Ama logosuz ve tanıtımsız bir şirket kartı da adayı ikna etmez.

### Kaydetme davranışı

- Her adım **ayrı kaydedilir**, yarıda bırakılıp dönülebilir
- Üstte **ilerleme çubuğu** ve **profil tamamlanma yüzdesi** görünür

---

## Adım 1 — Kimlik

| Alan | Zorunlu | Not |
|---|:---:|---|
| Şirket adı (görünen) | ✅ | Resmi unvandan farklı olabilir — "ABC Teknoloji A.Ş." yerine "ABC Teknoloji" |
| Logo | ✅ | **Varsayılan avatar** üretilir, isterse değiştirir |
| Üst sektör | ✅ | Kapalı liste |
| Alt sektör | ✅ | Kapalı liste, "Diğer" kaçışlı |
| Çalışan sayısı | ✅ | Aralık seçimi |
| Kuruluş yılı | ❌ | Sadece yıl |
| Web sitesi | ❌ | |
| Sosyal medya | ❌ | Çoklu |

### Logo

Şirket adının baş harflerinden üretilen renkli avatar varsayılan olarak atanır. İşveren isterse kendi logosunu yükler.

> Zorunlu tutmanın sürtünmesi yok, ama kart da boş görünmüyor.

### Sektör yapısı

**İki kademeli, tek tabloda:**

```sql
sektorler (id, ad, ust_sektor_id, sira, aktif_mi)
```

`ust_sektor_id` boşsa üst sektör, doluysa alt sektördür.

**Örnek:**

```
Bilişim
├── Yazılım Geliştirme
├── Siber Güvenlik
├── Veri ve Yapay Zeka
├── Bulut ve Altyapı
├── E-ticaret
├── Oyun
├── Donanım
└── Diğer
```

**Kurallar:**
- **15-20 üst sektör**, her birinin altında **5-10 alt kalem**
- Üst sektör seçilince alt liste **otomatik açılır** — ayrı ekran veya tıklama yok
- Her üst sektörün altında **"Diğer"** bulunur; seçilirse serbest yazma kutusu açılır
- Alt liste kısa tutulur — 30 seçenekli liste rastgele tıklanan bir şeye dönüşür

**Neden kapalı liste:** Aday sektöre göre filtreleyecek. Serbest bırakılırsa "Yazılım", "Bilişim", "Teknoloji", "IT" dört ayrı kayıt olur ve filtre çalışmaz.

**Kaynak:** NACE sınıflandırması (TÜİK kullanıyor, kamuya açık) hazır iskelet verir. Doğrudan kullanmayın — çok resmi ve ayrıntılı; sadeleştirerek kendi listenizi çıkarın.

**"Diğer" seçilip yazılan metinler toplanır**, periyodik olarak gerçek kalem haline getirilir.

### Çalışan sayısı

Aralık seçimi: `1-10` / `11-50` / `51-200` / `201-500` / `500+`

> Kesin sayı sormayın — şirketler vermek istemez ve zaten değişir. Aralık adayın ihtiyacını karşılar (startup mı, kurumsal mı).

### Web sitesi

Format kontrolü yapılır — başında `https://` yoksa eklenir. Yoksa tıklanmayan bağlantılar oluşur.

### Sosyal medya

**Ayrı tabloda tutulur:**

```sql
sirket_sosyal_medya (sirket_id, platform, url)
```

> Tek tek kolon açmayın (`linkedin_url`, `instagram_url`…) — yeni platform ekleyince şema değiştirmek zorunda kalırsınız.

**Platformlar:** LinkedIn, Instagram, X, Facebook, YouTube

**Giriş biçimi:** Tam URL yerine **kullanıcı adı** istenir (`@abcteknoloji`), başına platform adresi sistem tarafından eklenir. İnsanlar yanlış link yapıştırıyor (paylaşım linki, uygulama içi link) — bu hatayı ortadan kaldırır.

---

## Adım 2 — Konum

### Kurallar

| | Durum |
|---|---|
| Merkez lokasyon | ✅ Zorunlu |
| Merkez il/ilçe | ✅ Zorunlu (kapalı liste) |
| Merkez açık adres | ❌ Opsiyonel |
| Ek lokasyonlar | ❌ Opsiyonel |

**Merkez neden zorunlu:**
- Şirket kartında adayın ilk baktığı şeylerden biri "nerede"
- Konumsuz profil, adayın ilana girmeden karar vermesini engeller
- Doğrulama belgesinde adres zaten var — tekrar yazmak yük değil

**Açık adres neden opsiyonel:**
Bazı şirketler tam adresini paylaşmak istemez (küçük ofis, ev ofis, güvenlik). İl/ilçe yeterlidir.

### Uzaktan çalışan şirket muafiyeti

**"Tamamen uzaktan, fiziksel ofisimiz yok"** işareti konur.

İşaretlenirse il/ilçe zorunluluğu kalkar.

> Bu muafiyet olmazsa o şirketler rastgele bir il seçer ve veriyi kirletir.

### Veri modeli

**Merkez de bir lokasyondur, tek tabloda tutulur:**

```sql
sirket_lokasyonlari (
  sirket_id,
  ad,              -- "Genel Merkez", "Ankara Ofisi", "Ar-Ge Merkezi"
  il_id,
  ilce_id,
  acik_adres,
  merkez_mi,       -- bir tanesi true
  sira
)
```

> Ayrı "merkez adresi" kolonu açmayın — "merkez taşındı, şube merkez oldu" durumlarında veri taşımak zorunda kalırsınız.

### Lokasyon adı alanı

`ad` alanı önemlidir. İlan oluştururken işveren lokasyon seçecek; sadece *"Ankara / Çankaya"* yazan bir liste yerine **isimli seçenekler** çok daha anlaşılırdır.

### İlanla bağlantı

```
ilanlar.lokasyon_id → sirket_lokasyonlari.id
```

**Eşleştirmede katı filtre olarak kullanılan konum, şirketin merkezi değil ilanın lokasyonudur.**

Merkezi İstanbul olan şirketin Ankara ilanı, Ankara'daki adaya düşmelidir.

### Harita pini gerekmez

Mekan projesinde pin zorunluydu çünkü mesafe sıralamada %40 ağırlıktaydı. Burada mesafe hesabı yapılmıyor — filtre il/ilçe düzeyinde. Açık adres yeterlidir.

*(İleride ofisi haritada göstermek isterseniz eklenebilir.)*

---

## Adım 3 — Tanıtım

### Neden tek kutu değil

Tek büyük *"şirketinizi tanıtın"* kutusu yerine **dört ayrı soru** sorulur.

> Boş kutuya klişe gelir ("dinamik ve yenilikçi bir ekibiz"), somut soruya gerçek cevap gelir.

### Sorular

| # | Soru | Zorunlu |
|:---:|---|:---:|
| 1 | **Şirket ne iş yapıyor?** Ürün/hizmet ne, müşteri kim. | ✅ |
| 2 | **Çalışma kültürünüz nasıl?** Ekip yapısı, karar alma şekli, iletişim tarzı. | ❌ |
| 3 | **Burada çalışmak nasıl bir şey?** Tipik bir gün, çalışma temposu, hibrit/ofis düzeni. | ❌ |
| 4 | **Neden sizde çalışmalı?** Şirketin farkı, aday için fırsat. | ❌ |

**Sadece 1. soru zorunludur.** Diğerleri profil puanını artırır.

> İşveren zaten belge yükleyip onay bekledi; dört zorunlu metin görürse yarıda bırakır.

### Uygulama detayları

**Yer tutucu örnek metin — her soruda**

> *Örn: "Lojistik firmaları için rota optimizasyonu yazılımı geliştiriyoruz. 40 kurumsal müşterimiz var."*

Somut bir örnek, insanların ne yazacağını anlamasını sağlar ve cevap kalitesini gözle görülür yükseltir.

**Karakter sınırı:** Soru başına 500-800 karakter.

> Sınırsız bırakırsanız birileri tüm şirket tarihçesini yapıştırır, aday okumaz.

**Minimum uzunluk uyarısı:** 1. soruya 20 karakter yazıp geçilmesin.

> *"Biraz daha detay verin, adaylar bunu okuyor"* — yumuşak uyarı, sert engel değil.

### Embedding'e girmez

Bu metinler eşleştirmeye **hiç** girmez.

**Neden:** Eşleştirme ilan bazında yapılıyor. Şirket tanıtımı vektöre karışırsa aynı şirketin tüm ilanları birbirine benzer ve pozisyon ayrımı bulanıklaşır — *"backend geliştirici"* ile *"muhasebe uzmanı"* ilanı aynı şirketse birbirine yakın çıkar.

---

## Adım 4 — İmkanlar ve Görseller

Tamamı opsiyoneldir, profil puanını artırır.

### Yan haklar

**Serbest yazma + otomatik tamamlama.** Filtreye girmez, yalnızca bilgi amaçlıdır.

```sql
yan_haklar (id, ad, normalize_ad, kullanim_sayisi)
sirket_yan_haklari (sirket_id, yan_hak_id)
```

**Öneri kullanım sayısına göre sıralı gelir.** "Yemek kartı"nı 200 şirket yazmışsa üstte çıkar — doğal standartlaşma oluşur.

> **Filtreye girmediği için serbest yazmanın hiçbir sakıncası yok.** Yetenek ve şirket adındaki mantığın aynısı.

**Başlangıç verisi — önemli:**
15-20 yaygın kalem elle girilmelidir:

> Özel sağlık sigortası · Yemek kartı / yemekhane · Servis / yol desteği · Uzaktan çalışma · Esnek saat · Eğitim bütçesi · Dil kursu · Spor üyeliği · Kreş desteği · Prim / bonus · Hisse opsiyonu · Ekipman desteği · Doğum izni desteği · Yıllık izin fazlası

İlk şirketler boş öneri listesiyle karşılaşmamalı — yoksa herkes farklı yazar ve öneri sistemi işe yaramaz hale gelir.

### Görseller

| | Adet | Durum |
|---|:---:|:---:|
| Ofis / şirket fotoğrafı | 3-8 | Opsiyonel |
| Tanıtım videosu (link) | 1 | Opsiyonel |

> Fotoğraflar kartta ciddi fark yaratır ama zorunlu tutulmaz — her şirketin gösterilecek ofisi yok.

---

## Otomatik Gelen Bilgiler

Bunlar işveren tarafından girilmez, sistem tarafından hesaplanır:

- Açık ilan sayısı
- Doğrulanmış rozeti *(kurumsal e-posta + belge → rozetli; sadece belge → rozetsiz onay)*
- Üyelik tarihi
- *(İleride)* Ortalama cevap süresi — aday için güçlü güven sinyali

---

## Teşvik Mekanizması

Zorlamak yerine **ödüllendirme**:

- Profili eksik şirketin ilanları aday Keşfet'inde **daha aşağıda** çıkar
- Panelde uyarı: *"Profiliniz %60 dolu — tamamlarsanız ilanlarınız daha çok adaya ulaşır"*

> Bu yaklaşım hem mekan projesindeki kararla hem aday tarafındaki profil eşiğiyle tutarlıdır.

---

## Açık Kalanlar

- **Çoklu kullanıcı:** Bir şirkette birden fazla İK çalışanı olabilir. Profili kim düzenler, ilanı kim açar?
  → Basit çözüm: ilk kayıt olan "sahip", başkalarını davet eder.
  → Şimdi yazılmasa da veri modelinde **`sirket_kullanicilari`** tablosu bırakılmalı.
- Aynı vergi numarasıyla ikinci hesap açılmak istenirse ne olacak? (şube / farklı İK kullanıcısı senaryosu)
- Sektör listesinin nihai içeriği (NACE'den sadeleştirilecek)
- Profil tamamlanma yüzdesinin hangi alanlara kaç puan vereceği