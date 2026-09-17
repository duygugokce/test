# KTP — ASKI TİPİ GAZ PEDALI (APS) CAN / J1939 TESTİ

| Alan | Değer |
|---|---|
| Doküman No | KTP-001 |
| Revizyon | R00 |
| Tarih | 2026-09-17 |
| Test Nesnesi | Askı tipi gaz pedalı (Accelerator Pedal Sensor) |
| Yazılım | 965845M004K-SW01 |
| Konnektör | AMP 1-967616-1 (6 yol) / Terminal: AMP 0-0962885-5 / Wire seal: AMP 0-0967067-1 / Blind plug: AMP 0-0967056-1 |
| Referans | Pedal teknik resmi, APS CAN 2110201 – Specifications – Rev1, SAE J1939-21/-71/-73 |

**Ön koşul:** Pedal test kablo demeti ile CAN analizörüne bağlanır, hat iki uçtan 120 Ω ile sonlandırılır (≈60 Ω), besleme 24 VDC uygulanır. Pedalın source address'i 0 (00h) olduğundan hatta SA = 0 kullanan başka bir düğüm (motor ECU'su veya simülasyonu) bulunmamalıdır. Tüm test boyunca CAN trace kaydı alınır ve rapora eklenir.

---

## TEST MADDELERİ

| | | |
|---|---|---|
| **MADDE 1** | **PROSEDÜR** | Konnektör (AMP 1-967616-1) wire side görünüşünde pin-out, süreklilik ve gerilim ölçümü ile kontrol edilir. |
| | **KABUL KRİTERİ** | Pin 1 = CAN HIGH, Pin 2 = CAN SHIELD, Pin 3 = 0V GROUND, Pin 4 = CAN LOW, Pin 5 = boş (blind plug), Pin 6 = +8..32 VDC'dir; pin 1–4 arası direnç ≈ 60 Ω ölçülür, pin 6 ile pin 3 arasında kısa devre yoktur. |
| **MADDE 2** | **PROSEDÜR** | Kullanılan terminal, wire seal ve blind plug parça numaraları kontrol edilir. |
| | **KABUL KRİTERİ** | Terminal AMP 0-0962885-5, wire seal AMP 0-0967067-1, blind plug AMP 0-0967056-1'dir. |
| **MADDE 3** | **PROSEDÜR** | Besleme gerilimi 8 V, 12 V, 24 V ve 32 V seviyelerinde uygulanarak haberleşme ve akım çekişi kontrol edilir. |
| | **KABUL KRİTERİ** | Tüm besleme seviyelerinde pedal çalışır, CAN yayını kesilmez, sinyal değerleri değişmez ve aktif hata kaydı oluşmaz. |
| **MADDE 4** | **PROSEDÜR** | CAN analizörü 250 kb/s'ye ayarlanarak hat dinlenir, ardından farklı bir baudrate (500 kb/s) denenir. |
| | **KABUL KRİTERİ** | 250 kb/s'de mesajlar hata çerçevesi olmadan okunur, 500 kb/s'de okunamaz; baudrate 250 kb/s'dir ve programlanabilir değildir. |
| **MADDE 5** | **PROSEDÜR** | Trace kaydı açıkken pedal beslemesi verilir ve ilk CAN mesajının yayınlanma süresi ölçülür; işlem 5 kez tekrarlanır. |
| | **KABUL KRİTERİ** | Besleme anından itibaren 2 s boyunca hatta mesaj çıkmaz, ilk EEC2 mesajı 2000 ms ± 200 ms içinde yayınlanır ve bu süre her tekrarda aynıdır. |
| **MADDE 6** | **PROSEDÜR** | Pedalın yayınladığı CAN ID, extended (29-bit) formatta okunur ve alanlarına ayrılarak çözümlenir. |
| | **KABUL KRİTERİ** | CAN ID 0CF00300h'dir (Priority 3, PGN 61443 / F003h – EEC2, Source Address 0 / 00h), DLC = 8'dir ve pedal bunun dışında beklenmeyen mesaj yayınlamaz. |
| **MADDE 7** | **PROSEDÜR** | 0CF00300h mesajından ardışık en az 100 çerçeve kaydedilerek periyot, jitter ve pedal hareketi sırasındaki davranışı ölçülür. |
| | **KABUL KRİTERİ** | Cycle rate 50 ms ± %10'dur (45–55 ms), tek çerçeve sapması 40–60 ms aralığını aşmaz ve periyot pedal hareketinden ve besleme geriliminden etkilenmez. |
| **MADDE 8** | **PROSEDÜR** | CAN üzerinden Commanded Address (PGN 65240 / FED8h) mesajı yeni bir adres ile gönderilir, ardından power cycle yapılır. |
| | **KABUL KRİTERİ** | Pedal adres değişikliğini kabul etmez; source address 00h ve CAN ID 0CF00300h olarak kalır (commanded address disabled). |
| **MADDE 9** | **PROSEDÜR** | Pedal hiç basılmamış (serbest) konumdayken EEC2 içindeki SPN 91 (Accelerator Pedal Position 1) değeri 10 s boyunca izlenir. |
| | **KABUL KRİTERİ** | CAN hattında pedal pozisyonu **%0** okunur (ham değer 00h, tolerans 0–%2), değer kararlıdır ve pedala basılıp bırakıldığında her seferinde %0'a geri döner. |
| **MADDE 10** | **PROSEDÜR** | Pedal, strok ölçüm aparatı ile toplam stroğun %50'sine basılarak sabit tutulur ve SPN 91 değeri okunur; işlem 3 kez tekrarlanır. |
| | **KABUL KRİTERİ** | CAN hattında pedal pozisyonu **%50 ± %5** (ham değer 7Dh ± 0Dh) okunur, değer basılı tutulduğu sürece kararlıdır ve tekrarlar arası fark ≤ %3'tür. |
| **MADDE 11** | **PROSEDÜR** | Pedal mekanik dayamaya kadar tam basılarak SPN 91 değeri okunur. |
| | **KABUL KRİTERİ** | CAN hattında pedal pozisyonu **%100** (ham değer FAh, tolerans %98–%100) okunur; FEh (error) veya FFh (not available) değeri okunmaz ve aktif hata oluşmaz. |
| **MADDE 12** | **PROSEDÜR** | Pedal %0'dan %100'e ve tekrar %0'a yavaşça (≈5 s) hareket ettirilirken SPN 91 değeri kaydedilir; ardından hızlı basma/bırakma 5 kez tekrarlanır. |
| | **KABUL KRİTERİ** | Pedal pozisyonu monoton olarak artar ve azalır; sıçrama, takılma veya geri dönüş görülmez, ara noktalar (%25/%50/%75) mekanik strok ile ± %5 uyumludur. |
| **MADDE 13** | **PROSEDÜR** | Pedal çok yavaş basılırken SPN 558 (Accelerator Pedal 1 Low Idle Switch / IVS) ile SPN 91 eş zamanlı izlenir ve geçiş noktası kaydedilir. |
| | **KABUL KRİTERİ** | SPN 558 pedal serbestken **1**'dir ve pedal pozisyonu **%7**'ye (± %2) ulaştığında **0**'a döner; pedal bırakıldığında tekrar 1 olur, geçiş noktası tekrarlanabilirdir ve sinyal hiçbir zaman 10b (error) okunmaz. |
| **MADDE 14** | **PROSEDÜR** | Pedal %0, %50 ve %100 (kickdown) konumlarına getirilerek SPN 559 (Accelerator Pedal Kickdown Switch) değeri okunur. |
| | **KABUL KRİTERİ** | SPN 559 pedal %0 ve %50'de **0**, pedal **%100**'e basıldığında **1** okunur, pedal bırakıldığında tekrar 0'a döner ve sinyal hiçbir zaman 10b (error) okunmaz. |
| **MADDE 15** | **PROSEDÜR** | Pedal serbest, %50 ve %100 konumlarında ve power cycle sonrasında EEC2 içindeki SPN 4206 değeri okunur. |
| | **KABUL KRİTERİ** | SPN 4206 tüm pedal konumlarında ve her besleme seviyesinde **Active** okunur. |
| **MADDE 16** | **PROSEDÜR** | Pedal serbest, %50 ve %100 konumlarında ve power cycle sonrasında EEC2 içindeki SPN 4207 değeri okunur. |
| | **KABUL KRİTERİ** | SPN 4207 tüm pedal konumlarında ve her besleme seviyesinde **Active** okunur. |
| **MADDE 17** | **PROSEDÜR** | Sağlıklı pedal ile 30 s trace alınarak DM1 mesajı (PGN 65226 / 18FECA00h) varlığı, periyodu ve içeriği kontrol edilir. |
| | **KABUL KRİTERİ** | DM1 yayını mevcuttur (DTC fonksiyonu **Enabled**), aktif DTC listesi boştur, MIL sönüktür ve pedal tam strok hareket ettirildiğinde istenmeyen hata kaydı oluşmaz. |
| **MADDE 18** | **PROSEDÜR** | SPN 558 / FMI 19 arızası tetiklenir (Signal2 tanımlı toleransların altında veya üstünde) ve DM1 ile EEC2 içeriği okunur. |
| | **KABUL KRİTERİ** | DM1'de **SPN 558 / FMI 19** aktif DTC olarak yayınlanır, lamba durumu **M.I.L.** olur, occurrence count artar ve EEC2'de SPN 558 sinyal değeri **0x02** (error) okunur. |
| **MADDE 19** | **PROSEDÜR** | SPN 91 / FMI 0 arızası tetiklenir (IVS sinyali Idle iken APP1'in switch noktasından yüksek olması) ve DM1 ile EEC2 içeriği okunur. |
| | **KABUL KRİTERİ** | DM1'de **SPN 91 / FMI 0** aktif DTC olarak yayınlanır, lamba durumu **M.I.L.** olur ve EEC2'de SPN 91 sinyal değeri **0xFE** (error) okunur. |
| **MADDE 20** | **PROSEDÜR** | SPN 91 / FMI 1 arızası tetiklenir (IVS sinyali Idle değilken APP1'in switch noktasından düşük olması) ve DM1 ile EEC2 içeriği okunur. |
| | **KABUL KRİTERİ** | DM1'de **SPN 91 / FMI 1** aktif DTC olarak yayınlanır, lamba durumu **M.I.L.** olur ve EEC2'de SPN 91 sinyal değeri **0xFE** (error) okunur. |
| **MADDE 21** | **PROSEDÜR** | SPN 91 / FMI 3 arızası tetiklenir (Signal1 tanımlı yüksek toleransın üstünde) ve DM1 ile EEC2 içeriği okunur. |
| | **KABUL KRİTERİ** | DM1'de **SPN 91 / FMI 3** aktif DTC olarak yayınlanır, lamba durumu **M.I.L.** olur ve EEC2'de SPN 91 sinyal değeri **0xFE** (error) okunur. |
| **MADDE 22** | **PROSEDÜR** | SPN 91 / FMI 4 arızası tetiklenir (Signal1 tanımlı düşük toleransın altında) ve DM1 ile EEC2 içeriği okunur. |
| | **KABUL KRİTERİ** | DM1'de **SPN 91 / FMI 4** aktif DTC olarak yayınlanır, lamba durumu **M.I.L.** olur ve EEC2'de SPN 91 sinyal değeri **0xFE** (error) okunur. |
| **MADDE 23** | **PROSEDÜR** | SPN 559 / FMI 19 arızası tetiklenir (Kickdown anahtarı Signal2 konumu ile Signal1 değerlerinin uyuşmaması) ve DM1 ile EEC2 içeriği okunur. |
| | **KABUL KRİTERİ** | DM1'de **SPN 559 / FMI 19** aktif DTC olarak yayınlanır, lamba durumu **M.I.L.** olur ve EEC2'de SPN 559 sinyal değeri **0x02** (error) okunur; kickdown içermeyen uygulamalarda occurrence count değeri daima **0**'dır. |
| **MADDE 24** | **PROSEDÜR** | MADDE 18–23'teki hatalar fiziksel olarak oluşturulamıyorsa, pedal hattan çıkarılarak araç/pedal tarafı CAN üzerinden sanal olarak manipüle edilir; EK-A'daki DM1 ve EEC2 çerçeveleri hatta basılır. |
| | **KABUL KRİTERİ** | Basılan her arıza için araç tarafı (ECU/gösterge/analizör) ilgili **SPN + FMI** kombinasyonunu doğru çözümler, **M.I.L.** durumunu ve arıza anındaki sinyal değerlerini (**SPN 91 = 0xFE**, **SPN 558/559 = 0x02**) EK-A tablosuna uygun okur. |
| **MADDE 25** | **PROSEDÜR** | İki arıza (örn. SPN 558/FMI 19 + SPN 91/FMI 3) eş zamanlı tetiklenir veya basılır ve DM1 içeriği çözümlenir. |
| | **KABUL KRİTERİ** | DM1 içinde her iki DTC de eksiksiz listelenir; ikiden fazla DTC olması hâlinde çok paketli DM1 (TP.BAM) yapısı doğru şekilde yayınlanır ve tüm DTC'ler okunabilir. |
| **MADDE 26** | **PROSEDÜR** | Arıza koşulu ortadan kaldırılır, pedal sağlıklı hale getirilerek power cycle yapılır ve DM1 ile EEC2 tekrar okunur. |
| | **KABUL KRİTERİ** | Aktif DTC listesi boşalır, M.I.L. söner ve sinyaller normal değerlerine döner (SPN 91 = %0, SPN 558 = 1, SPN 559 = 0); hata kaydı geçmiş arıza olarak saklanır. |
| **MADDE 27** | **PROSEDÜR** | Test edilen pedalın yazılım versiyonu etiket ve/veya diyagnostik okuma ile kontrol edilir. |
| | **KABUL KRİTERİ** | Yazılım versiyonu **965845M004K-SW01**'dir. |

---

## EK-A — ARIZA (DTC) TABLOSU VE SANAL MANİPÜLASYON ÇERÇEVELERİ

### A.1 Doğrulanacak arızalar — DM1 for EEC2 (MCS Standard-1): SPN 91 + SPN 558 + SPN 559

| SPN | FMI | Sinyal değeri | Arıza tanımı | Lamba durumu |
|---|---|---|---|---|
| 558 | 19 | 0x02 | Signal2 is Lower or Higher than defined tolerances | M.I.L. |
| 91 | 0 | 0xFE | APP1 is Higher than IVS switch point while IVS signal is Idle | M.I.L. |
| 91 | 1 | 0xFE | APP1 is Lower than IVS switch point while IVS signal is not Idle | M.I.L. |
| 91 | 3 | 0xFE | Signal1 is Higher than defined high tolerance | M.I.L. |
| 91 | 4 | 0xFE | Signal1 is Lower than defined low tolerance | M.I.L. |
| 559 | 19 | 0x02 | Kickdown Switch position (Signal2) and Signal1 values discrepancy | M.I.L. |

**Not:** Kickdown içermeyen uygulamalarda SPN 559 / FMI 19 occurrence count değeri daima 0'dır. SPN sinyalleri, birbirinden ayrı iki SENT protokol sinyaline (Signal1, Signal2) dayanır (bkz. APS CAN 2110201 – Specifications – Rev1).

**Sinyal değerlerinin anlamı:** 0xFE (254), 1 byte'lık SPN 91 için "Error Indicator"dır (0xFF = Not Available ile karıştırılmamalıdır). 0x02 (10b), 2 bit'lik SPN 558 / SPN 559 için "Error Indicator"dır.

### A.2 Sanal manipülasyonda basılacak DM1 çerçeveleri (MADDE 24)

DM1: CAN ID **18FECA00h** (Priority 6, PGN 65226, SA 00h), DLC = 8, periyot 1000 ms.
Byte 1 = lamba durumu (MIL = 01b → 7Fh), Byte 2 = FFh, Byte 3–4 = SPN (LSB önce), Byte 5 = SPN yüksek 3 bit + FMI, Byte 6 = CM + occurrence count, Byte 7–8 = FFh.

| Arıza | SPN (hex) | FMI | Basılacak veri (8 byte) |
|---|---|---|---|
| SPN 558 / FMI 19 | 22Eh | 13h | `7F FF 2E 02 13 01 FF FF` |
| SPN 91 / FMI 0 | 05Bh | 00h | `7F FF 5B 00 00 01 FF FF` |
| SPN 91 / FMI 1 | 05Bh | 01h | `7F FF 5B 00 01 01 FF FF` |
| SPN 91 / FMI 3 | 05Bh | 03h | `7F FF 5B 00 03 01 FF FF` |
| SPN 91 / FMI 4 | 05Bh | 04h | `7F FF 5B 00 04 01 FF FF` |
| SPN 559 / FMI 19 | 22Fh | 13h | `7F FF 2F 02 13 00 FF FF` |

Yukarıdaki kodlama SPN conversion method **version 4** içindir; farklı versiyon kullanılıyorsa byte 3–5 dizilimi sağlıklı pedalın DM1 yayınından teyit edilerek güncellenir.

### A.3 Arıza anında eş zamanlı basılacak EEC2 çerçevesi

EEC2: CAN ID **0CF00300h**, DLC = 8, periyot 50 ms. SPN 558 = byte 1 bit 1–2, SPN 559 = byte 1 bit 3–4, SPN 91 = byte 2 (0,4 %/bit).

| Senaryo | Byte 1 | Byte 2 (SPN 91) |
|---|---|---|
| SPN 558 error | bit 1–2 = 10b (örn. FEh) | normal değer |
| SPN 91 error | normal değer | **FEh** |
| SPN 559 error | bit 3–4 = 10b (örn. FBh) | normal değer |

---

## EK-B — TEST SONUÇ TABLOSU

| Madde | Konu | Ölçülen / Gözlenen Değer | Sonuç (U / K) | Açıklama |
|---|---|---|---|---|
| 1 | Pin-out | | | |
| 2 | Terminal / seal / plug parça no | | | |
| 3 | Besleme aralığı (8–32 VDC) | | | |
| 4 | Baudrate (250 kb/s) | | | |
| 5 | Start delay (2 s) | | | |
| 6 | CAN ID (0CF00300h) / SA (00h) | | | |
| 7 | Cycle rate (50 ms) | | | |
| 8 | Commanded address (disabled) | | | |
| 9 | SPN 91 — pedal basılı değil (%0) | | | |
| 10 | SPN 91 — pedal yarım basılı (%50) | | | |
| 11 | SPN 91 — pedal tam basılı (%100) | | | |
| 12 | SPN 91 — doğrusallık | | | |
| 13 | SPN 558 — 1 → 0 (%7) | | | |
| 14 | SPN 559 — 0 → 1 (%100) | | | |
| 15 | SPN 4206 — Active | | | |
| 16 | SPN 4207 — Active | | | |
| 17 | DM1 — Enabled / arızasız durum | | | |
| 18 | DTC SPN 558 / FMI 19 | | | |
| 19 | DTC SPN 91 / FMI 0 | | | |
| 20 | DTC SPN 91 / FMI 1 | | | |
| 21 | DTC SPN 91 / FMI 3 | | | |
| 22 | DTC SPN 91 / FMI 4 | | | |
| 23 | DTC SPN 559 / FMI 19 | | | |
| 24 | Sanal (CAN) manipülasyon ile DTC doğrulama | | | |
| 25 | Çoklu arıza | | | |
| 26 | Arıza temizleme | | | |
| 27 | Yazılım versiyonu (965845M004K-SW01) | | | |

| | Ad Soyad | Tarih | İmza |
|---|---|---|---|
| Testi Yapan | | | |
| Kontrol Eden | | | |
| Onaylayan | | | |

---

## EK-C — AÇIK MADDELER

| No | Konu | Aksiyon |
|---|---|---|
| A1 | SPN 4206 / 4207'nin EEC2 içindeki byte.bit konumu ve "Active" kodlaması | APS CAN 2110201 – Rev1 ve tedarikçi J1939 matrisinden teyit edilip MADDE 15–16'ya işlenecek |
| A2 | SPN 558 geçiş noktası (%7) toleransı | Spesifikasyondan alınacak; tanımlı değilse ± %2 kabul edilecek |
| A3 | Start delay toleransı | Spesifikasyondan alınacak; tanımlı değilse ± 200 ms kabul edilecek |
| A4 | Uygulamada kickdown var mı? | Varsa MADDE 14 zorunlu; yoksa SPN 559 = 0 ve SPN 559/FMI 19 occurrence count = 0 olarak kayıt edilecek |
| A5 | Arıza enjeksiyon fikstürü | Tedarikçiden talep edilecek; temin edilemezse MADDE 18–23 doğrudan MADDE 24 (sanal manipülasyon) ile yapılacak |
| A6 | DM1 SPN conversion method versiyonu | Sağlıklı pedalın DM1 yayınından teyit edilecek |
