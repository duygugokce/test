# KTP — ASKI TİPİ GAZ PEDALI (APS) CAN / J1939 TESTİ

| Alan | Değer |
|---|---|
| Doküman No | KTP-001 |
| Revizyon | R00 |
| Tarih | 17.09.2026 |
| Test Nesnesi | Askı tipi gaz pedalı (Accelerator Pedal Sensor — APS) |
| Yazılım | 965845M004K-SW01 |
| Konnektör | AMP 1-967616-1 (6 yol) · Terminal: AMP 0-0962885-5 · Wire seal: AMP 0-0967067-1 · Blind plug: AMP 0-0967056-1 |
| Referans | Pedal teknik resmi · APS CAN 2110201 – Specifications – Rev1 · SAE J1939-21 / -71 / -73 |

**ÖN KOŞUL:** Pedal, test kablo demeti ile CAN analizörüne bağlanır; hat iki uçtan 120 Ω ile sonlandırılır ve besleme 24 VDC uygulanır. Pedalın source address'i 0 (00h) olduğundan hatta SA = 0 kullanan başka bir düğüm bulunmamalıdır. Test boyunca CAN trace kaydı alınır.

## TEST MADDELERİ

| | | |
|---|---|---|
| **MADDE 1** | **PROSEDÜR** | Konnektör pin-out'u süreklilik ve gerilim ölçümü ile kontrol edilir. |
| | **KABUL KRİTERİ** | Pin 1 = CAN HIGH, Pin 2 = CAN SHIELD, Pin 3 = 0V GROUND, Pin 4 = CAN LOW, Pin 5 = boş, Pin 6 = +8..32 VDC'dir. |
| **MADDE 2** | **PROSEDÜR** | Besleme gerilimi 8 V ve 32 V seviyelerinde uygulanır. |
| | **KABUL KRİTERİ** | Her iki seviyede de pedal çalışır, CAN yayını kesilmez ve hata kaydı oluşmaz. |
| **MADDE 3** | **PROSEDÜR** | CAN analizörü 250 kb/s'ye ayarlanarak hat dinlenir. |
| | **KABUL KRİTERİ** | Mesajlar hata çerçevesi olmadan okunur; baudrate 250 kb/s'dir. |
| **MADDE 4** | **PROSEDÜR** | Besleme verildiği andan ilk CAN mesajına kadar geçen süre ölçülür. |
| | **KABUL KRİTERİ** | İlk mesaj 2 s sonra yayınlanır (start delay = 2 s). |
| **MADDE 5** | **PROSEDÜR** | Pedalın yayınladığı CAN ID okunur. |
| | **KABUL KRİTERİ** | CAN ID 0CF00300h'dir (PGN 61443 – EEC2, source address 0 / 00h). |
| **MADDE 6** | **PROSEDÜR** | EEC2 mesajının periyodu ardışık 100 çerçeve üzerinden ölçülür. |
| | **KABUL KRİTERİ** | Cycle rate 50 ms'dir (tolerans ± %10). |
| **MADDE 7** | **PROSEDÜR** | CAN üzerinden commanded address mesajı gönderilir. |
| | **KABUL KRİTERİ** | Adres değişmez, source address 0 (00h) olarak kalır (commanded address disabled). |
| **MADDE 8** | **PROSEDÜR** | Pedal hiç basılmamış konumdayken SPN 91 değeri okunur. |
| | **KABUL KRİTERİ** | CAN hattında pedal pozisyonu %0 okunur. |
| **MADDE 9** | **PROSEDÜR** | Pedal yarım (%50) basılarak SPN 91 değeri okunur. |
| | **KABUL KRİTERİ** | CAN hattında pedal pozisyonu %50 ± %5 okunur. |
| **MADDE 10** | **PROSEDÜR** | Pedal tam basılarak SPN 91 değeri okunur. |
| | **KABUL KRİTERİ** | CAN hattında pedal pozisyonu %100 okunur. |
| **MADDE 11** | **PROSEDÜR** | Pedal %0'dan %100'e yavaşça basılıp bırakılır. |
| | **KABUL KRİTERİ** | Pedal pozisyonu sıçrama ve takılma olmadan monoton artar ve azalır. |
| **MADDE 12** | **PROSEDÜR** | Pedal yavaş basılırken SPN 558 (IVS) izlenir. |
| | **KABUL KRİTERİ** | SPN 558 pedal serbestken 1'dir, pedal pozisyonu %7'ye ulaştığında 0 olur. |
| **MADDE 13** | **PROSEDÜR** | Pedal tam basılarak SPN 559 (kickdown) değeri okunur. |
| | **KABUL KRİTERİ** | SPN 559 normalde 0'dır, pedal %100'e basıldığında 1 olur. |
| **MADDE 14** | **PROSEDÜR** | EEC2 içindeki SPN 4206 ve SPN 4207 değerleri okunur. |
| | **KABUL KRİTERİ** | Her iki sinyal de Active'dir. |
| **MADDE 15** | **PROSEDÜR** | Sağlıklı pedal ile DM1 mesajı kontrol edilir. |
| | **KABUL KRİTERİ** | DM1 yayını mevcuttur (DTC enabled) ve aktif hata kaydı yoktur. |
| **MADDE 16** | **PROSEDÜR** | SPN 558 / FMI 19 arızası tetiklenir (Signal2 tolerans dışı). |
| | **KABUL KRİTERİ** | DM1'de SPN 558 / FMI 19 okunur, M.I.L. yanar ve sinyal değeri 0x02'dir. |
| **MADDE 17** | **PROSEDÜR** | SPN 91 / FMI 0 arızası tetiklenir (IVS idle iken APP1 switch noktasının üstünde). |
| | **KABUL KRİTERİ** | DM1'de SPN 91 / FMI 0 okunur, M.I.L. yanar ve sinyal değeri 0xFE'dir. |
| **MADDE 18** | **PROSEDÜR** | SPN 91 / FMI 1 arızası tetiklenir (IVS idle değilken APP1 switch noktasının altında). |
| | **KABUL KRİTERİ** | DM1'de SPN 91 / FMI 1 okunur, M.I.L. yanar ve sinyal değeri 0xFE'dir. |
| **MADDE 19** | **PROSEDÜR** | SPN 91 / FMI 3 arızası tetiklenir (Signal1 yüksek tolerans dışı). |
| | **KABUL KRİTERİ** | DM1'de SPN 91 / FMI 3 okunur, M.I.L. yanar ve sinyal değeri 0xFE'dir. |
| **MADDE 20** | **PROSEDÜR** | SPN 91 / FMI 4 arızası tetiklenir (Signal1 düşük tolerans dışı). |
| | **KABUL KRİTERİ** | DM1'de SPN 91 / FMI 4 okunur, M.I.L. yanar ve sinyal değeri 0xFE'dir. |
| **MADDE 21** | **PROSEDÜR** | SPN 559 / FMI 19 arızası tetiklenir (kickdown ile Signal1 uyumsuzluğu). |
| | **KABUL KRİTERİ** | DM1'de SPN 559 / FMI 19 okunur, M.I.L. yanar ve sinyal değeri 0x02'dir; kickdown'sız uygulamada occurrence count 0'dır. |
| **MADDE 22** | **PROSEDÜR** | Arızalar fiziksel olarak oluşturulamıyorsa CAN hattı üzerinden sanal olarak basılır (EK-A). |
| | **KABUL KRİTERİ** | Araç tarafı her arızayı SPN / FMI, M.I.L. ve sinyal değeri (0xFE / 0x02) ile doğru okur. |
| **MADDE 23** | **PROSEDÜR** | Arıza koşulu ortadan kaldırılarak power cycle yapılır. |
| | **KABUL KRİTERİ** | Aktif hata kaydı düşer, M.I.L. söner ve sinyaller normal değerlerine döner. |
| **MADDE 24** | **PROSEDÜR** | Pedalın yazılım versiyonu okunur. |
| | **KABUL KRİTERİ** | Yazılım versiyonu 965845M004K-SW01'dir. |

## EK-A — ARIZA (DTC) TABLOSU VE SANAL MANİPÜLASYON ÇERÇEVELERİ

### A.1 DM1 for EEC2 (MCS Standard-1): SPN 91 + SPN 558 + SPN 559

| SPN | FMI | Sinyal değeri | Arıza tanımı | Lamba durumu |
|---|---|---|---|---|
| 558 | 19 | 0x02 | Signal2 is Lower or Higher than defined tolerances | M.I.L. |
| 91 | 0 | 0xFE | APP1 is Higher than IVS switch point while IVS signal is Idle | M.I.L. |
| 91 | 1 | 0xFE | APP1 is Lower than IVS switch point while IVS signal is not Idle | M.I.L. |
| 91 | 3 | 0xFE | Signal1 is Higher than defined high tolerance | M.I.L. |
| 91 | 4 | 0xFE | Signal1 is Lower than defined low tolerance | M.I.L. |
| 559 | 19 | 0x02 | Kickdown Switch position (Signal2) and Signal1 values discrepancy | M.I.L. |

**Not:** Kickdown içermeyen uygulamalarda SPN 559 / FMI 19 occurrence count değeri daima 0'dır. SPN sinyalleri, birbirinden ayrı iki SENT protokol sinyaline (Signal1, Signal2) dayanır (APS CAN 2110201 – Rev1). 0xFE: SPN 91 için Error Indicator (0xFF = Not Available değildir). 0x02 (10b): SPN 558 / 559 için Error Indicator.

### A.2 Sanal manipülasyonda basılacak DM1 çerçeveleri (MADDE 22)

| Arıza | SPN (hex) | FMI (hex) | Basılacak veri (8 byte) | Occurrence |
|---|---|---|---|---|
| SPN 558 / FMI 19 | 22Eh | 13h | 7F FF 2E 02 13 01 FF FF | 1 |
| SPN 91 / FMI 0 | 05Bh | 00h | 7F FF 5B 00 00 01 FF FF | 1 |
| SPN 91 / FMI 1 | 05Bh | 01h | 7F FF 5B 00 01 01 FF FF | 1 |
| SPN 91 / FMI 3 | 05Bh | 03h | 7F FF 5B 00 03 01 FF FF | 1 |
| SPN 91 / FMI 4 | 05Bh | 04h | 7F FF 5B 00 04 01 FF FF | 1 |
| SPN 559 / FMI 19 | 22Fh | 13h | 7F FF 2F 02 13 00 FF FF | 0 |

DM1: CAN ID 18FECA00h (PGN 65226, SA 00h), DLC = 8, periyot 1000 ms. Byte 1 = lamba durumu (M.I.L. = 01b → 7Fh), Byte 2 = FFh, Byte 3–4 = SPN (LSB önce), Byte 5 = SPN yüksek 3 bit + FMI, Byte 6 = CM + occurrence count, Byte 7–8 = FFh. Kodlama SPN conversion method version 4 içindir.

### A.3 Arıza anında eş zamanlı basılacak EEC2 çerçevesi

| Senaryo | EEC2 byte 1 | EEC2 byte 2 (SPN 91) |
|---|---|---|
| SPN 558 error | byte 1 bit 1–2 = 10b (örn. FEh) | normal değer |
| SPN 91 error | normal değer | FEh |
| SPN 559 error | byte 1 bit 3–4 = 10b (örn. FBh) | normal değer |

EEC2: CAN ID 0CF00300h, DLC = 8, periyot 50 ms. SPN 558 = byte 1 bit 1–2, SPN 559 = byte 1 bit 3–4, SPN 91 = byte 2 (0,4 %/bit).

## EK-B — TEST SONUÇ TABLOSU

| Madde | Konu | Ölçülen / Gözlenen Değer | Sonuç (U / K) | Açıklama — Bulgu No |
|---|---|---|---|---|
| 1 | Pin-out | | | |
| 2 | Besleme aralığı (8 – 32 VDC) | | | |
| 3 | Baudrate (250 kb/s) | | | |
| 4 | Start delay (2 s) | | | |
| 5 | CAN ID (0CF00300h) / source address (00h) | | | |
| 6 | Cycle rate (50 ms) | | | |
| 7 | Commanded address (disabled) | | | |
| 8 | SPN 91 — pedal basılı değil (%0) | | | |
| 9 | SPN 91 — pedal yarım basılı (%50) | | | |
| 10 | SPN 91 — pedal tam basılı (%100) | | | |
| 11 | SPN 91 — doğrusallık | | | |
| 12 | SPN 558 — 1 → 0 (%7) | | | |
| 13 | SPN 559 — 0 → 1 (%100) | | | |
| 14 | SPN 4206 / 4207 — Active | | | |
| 15 | DM1 — enabled / arızasız durum | | | |
| 16 | DTC — SPN 558 / FMI 19 | | | |
| 17 | DTC — SPN 91 / FMI 0 | | | |
| 18 | DTC — SPN 91 / FMI 1 | | | |
| 19 | DTC — SPN 91 / FMI 3 | | | |
| 20 | DTC — SPN 91 / FMI 4 | | | |
| 21 | DTC — SPN 559 / FMI 19 | | | |
| 22 | Sanal (CAN) manipülasyon ile DTC doğrulama | | | |
| 23 | Arıza temizleme | | | |
| 24 | Yazılım versiyonu (965845M004K-SW01) | | | |

| | Ad Soyad | Tarih | İmza |
|---|---|---|---|
| Testi Yapan | | | |
| Kontrol Eden | | | |
| Onaylayan | | | |

## EK-C — AÇIK MADDELER

| No | Konu | Aksiyon |
|---|---|---|
| A1 | SPN 4206 / 4207'nin EEC2 içindeki byte.bit konumu ve "Active" kodlaması | APS CAN 2110201 – Rev1 ve tedarikçi J1939 matrisinden teyit edilip MADDE 14'e işlenecek. |
| A2 | SPN 558 geçiş noktası (%7) toleransı | Spesifikasyondan alınacak; tanımlı değilse ± %2 kabul edilecek. |
| A3 | Start delay toleransı | Spesifikasyondan alınacak; tanımlı değilse ± 200 ms kabul edilecek. |
| A4 | Uygulamada kickdown var mı? | Varsa MADDE 13 zorunlu; yoksa SPN 559 = 0 ve occurrence count = 0 olarak kayıt edilecek. |
| A5 | Arıza enjeksiyon fikstürü | Tedarikçiden talep edilecek; temin edilemezse MADDE 16 – 21 doğrudan MADDE 22 ile yapılacak. |
| A6 | DM1 SPN conversion method versiyonu | Sağlıklı pedalın DM1 yayınından teyit edilecek. |
