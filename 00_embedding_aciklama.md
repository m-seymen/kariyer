İş veren yazar : "SRC belgesi ve forklift ehliyeti"
  |
  |
  |
  ---- Embedding'e ham metin girer
      -> Genel benzerlik (lojistik, sürücülük işi) gibi
  |
  |
  |
  ---- LLM listeye çevirir -> iş veren onayla
    [SRC belgesi] [Forklift ehliyeti]
    -> adayın sertifikalarıyla tek tek karşılaştırılır

Neden ikisi -> Embedding "bu aday sürücü/lojistik profiline benziyor" der ama SRC belgesinin adayda olmadığını fark etmez.

Liste karşılaştırması bunu yakalar.

Aday A: SRC ✅ geçerli,  Forklift ✅ geçerli  → 2/2
Aday B: SRC ❌ yok,      Forklift ✅ geçerli  → 1/2


Tam olarak bunu gösteren örnek. Diyelim iki adayın geri kalan her şeyi (unvan, tanıtım, deneyim) çok benzer — ikisi de backend geliştirici.

İlan zorunlu: React, .NET, Kubernetes

Aday A: React
Aday B: React, .NET, Kubernetes


Sadece ham metin embedding'e girseydi

Embedding iki adayın bütün metnini ilanın bütün metniyle karşılaştırır. İkisi de "backend geliştirici, web, API, veritabanı" anlatıyor. Aradaki fark birkaç kelime:

Aday A:  anlamsal = 0.84
Aday B:  anlamsal = 0.87

Sadece 3 puan fark. Diğer şeylere göre A, B'nin önüne bile geçebilir — örneğin A'nın tanıtımı biraz daha uzun ve detaylıysa. 
İşveren üç zorunlu yetenekten ikisi eksik olan adayı üstte görür.


İki yol olunca
Liste karşılaştırması eklenince:

Aday A:  React ✅  .NET ❌  Kubernetes ❌   → zorunlu = 1/3 = 0.33
Aday B:  React ✅  .NET ✅  Kubernetes ✅   → zorunlu = 3/3 = 1.00

Fark 3 puandan 12 puana çıktı. B açıkça üstte.

Özet
	Sadece           embedding	              İki yol
A ile B farkı	      ~3 puan	                ~12 puan
B açıkça üstte mi	  ❌ Belirsiz	              ✅ Evet
Sebep	Embedding bütün metni karıştırır, eksik yeteneği fark etmez	Liste her yeteneği tek tek sayar



Tercihen Sertifikalarda aynı buna benzer -> Sadece ağırlığı daha düşük
Tercih edilen sertifikalar

İlan tercih ediyor: İSG eğitimi, İlk yardım belgesi

İki yol:

Ham metin → embedding
LLM listeye çevirir → [İSG eğitimi] [İlk yardım belgesi] → adayla karşılaştırılır

Puanlama:
Adayda var ve geçerli  → 1
Yok veya süresi geçmiş → 0

Tam hesap — zorunlu ve tercihen birlikte

İLAN:
  Zorunlu küme:     React · 5 yıl,  SRC belgesi
  Tercih edilen:    Docker,  İSG eğitimi,  Bölüm: [Bilgisayar Müh.]

ADAY:
  React 3 yıl, Docker
  SRC Belgesi (geçerli)
  İSG Eğitimi (süresi geçmiş)
  Bölüm: Bilgisayar Mühendisliği


Zorunlu Küme:
React        var, 3 < 5     → 0.5 + 0.5 × 3/5 = 0.80
SRC belgesi  var, geçerli   → 1.00
zorunlu_puan = (0.80 + 1.00) / 2 = 0.90


Tercih edilen küme: 
Docker       var                → 1
İSG eğitimi  süresi geçmiş      → 0
Bölüm        eşleşti            → 1
tercihen_puan = 2 / 3 = 0.67

Birleştir
yetenek = 0.75 × 0.90 + 0.25 × 0.67 = 0.675 + 0.167 = 0.84


Fark tablosu

                                  Zorunlu	                              Tercih edilen
Yetenekte yıl	                    ✅ Kısmi puan	                          ❌ Sadece var/yok
Sertifikada geçerlilik	            ✅	                                    ✅
Ağırlık	                              0.75	                                0.25


