# Safety 4.0: Fonksiyonel Makine Emniyeti ve OT Siber Güvenliği Laboratuvar Çalışmaları

## 1. Projenin Amacı
Bu proje, endüstriyel otomasyon sistemlerinde **Fonksiyonel Makine Emniyeti (Functional Safety - ISO 13849 / IEC 62061)** ile **Operasyonel Teknoloji Siber Güvenliği (OT Cybersecurity - IEC 62443)** disiplinlerinin kesişimini ("Safety 4.0") incelemek amacıyla hazırlanmış 5 haftalık laboratuvar çalışmalarını içerir. 

Modern endüstriyel tesislerde makinelerin uzaktan bakım, kestirimci bakım telemetrisi ve üretim takibi amacıyla dış ağlara bağlanması, geleneksel olarak "izole" kabul edilen emniyet kritik kontrol sistemlerini siber tehditlere açık hale getirmiştir. Bu çalışmada, ağır sanayi tipi bir metal işleme hattı üzerinde siber saldırıların veya dijital hataların nasıl fiziksel yaralanmalara yol açabileceği analiz edilmiş ve hem ağ mimarisi hem de fonksiyonel emniyet düzeyinde alınması gereken mühendislik önlemleri adım adım modellenmiştir.

---

## 2. Seçilen Örnek Vaka ve Makine Tanıtımı
Çalışma kapsamında gerçek bir endüstriyel uygulama senaryosu oluşturmak adına **EAE Makina** (*eaemachinery.com*) tarafından üretilen **"Otomatik Rulo Sac Doğrultma, Sürme, Roll Form ve Boy Kesme Hattı"** baz alınmıştır.

### Hattın Temel Bileşenleri
1. **Hidrolik Rulo Açıcı (Decoiler):** 25 tona kadar ağırlıktaki çelik sac rulolarını taşıyan, hidrolik genleşme ve frenleme sistemine sahip ünite.
2. **Merdaneli Doğrultucu (Straightener / Leveler):** Rulodan açılan sacın iç gerilimlerini alan ve sacı düzleştiren yüksek basınçlı merdane grubu.
3. **Loop Çukuru (Akümülatör / Pit):** Doğrultucu ile kesikli çalışan pres/kesim üniteleri arasında sacın gerginliğini ve hız senkronizasyonunu dengeleyen sensör kontrollü boşluk alanı.
4. **Servo Sürme ve Punch Delme Presi:** Sacı milimetrik hassasiyetle süren servo sürücüler ve yüksek tonajlı delme işlemi yapan pres ünitesi.
5. **Roll Form ve Giyotin Makas (Shear):** Sacı istenen profil kesitine getiren istasyonlar ve boy kesme işlemini gerçekleştiren hidrolik/eksantrik giyotin makas.

### Neden Bu Makine Seçildi?
- **Yüksek Fiziksel Tehlike Potansiyeli:** 25 tonluk potansiyel enerjiye sahip sac rulosu, yüksek sıkıştırma kuvvetine sahip doğrultma merdaneleri ve yüksek hızlı giyotin makas gibi bileşenler; olası bir kontrol kaybında **geri döndürülemez uzuv kaybı veya ölümle** sonuçlanabilecek tehlikeler barındırmaktadır (PL e / SIL 3 seviyesi gereksinimler).
- **Karmaşık OT Mimarisi:** Hat üzerinde yüksek hızlı senkronizasyon gerektiren servo sürücüler, standart üretim PLC'si, Safety PLC, operatör panelleri (HMI) ve uzaktan destek için kullanılan ağ bileşenleri bir arada bulunmaktadır.
- **Siber-Fiziksel Etkileşim:** Kalıp ayarı (jog modu), reçete yükleme ve uzaktan arıza teşhisi gibi operasyonlar sırasında siber güvenlik zafiyetlerinin doğrudan fiziksel emniyeti nasıl tehlikeye atabileceğini göstermek için ideal bir örnektir.

---

## 3. İlk 5 Haftanın Özet Akışı

| Hafta | Konu Başlığı | Laboratuvar Çıktısı | Özet İçerik |
| :--- | :--- | :--- | :--- |
| **Hafta 1** | [Safety Architecture](./labs/hafta-01-safety-architecture.md) | *Safety Function Map* | Normal kontrol yolu ile emniyet (Safety) kontrol yolunun ayrıştırılması, PLr/SIL kavramları, güvenli durum (Safe State) tanımı ve emniyet fonksiyonunun dijital bağımlılıklarının analizi. |
| **Hafta 2** | [IEC 62443 Mini Workshop](./labs/hafta-02-iec62443-zone-conduit.md) | *Zone / Conduit Diagram* | Makine üzerindeki OT varlıklarının IEC 62443 standardına göre bölgelere (Zone) ayrılması, geçiş yollarının (Conduit) tanımlanması ve güvenli uzak erişim karar tablosunun oluşturulması. |
| **Hafta 3** | [Threat & Risk + Risk Matrix](./labs/hafta-03-threat-risk-matrix.md) | *Risk Register* | Siber tehditlerin fiziksel sonuçlara (ezilme, uzuv kaybı, sac fırlaması) bağlandığı senaryoların kurgulanması, $L \times S \times E$ formülü ile risk puanlaması ve şiddet ($S=3$) önceliklendirmesi. |
| **Hafta 4** | [Secure Remote Access](./labs/hafta-04-secure-remote-access.md) | *Secure Architecture* | Doğrudan internete açık güvensiz bakım mimarisinden; VPN, MFA, Jump Server ve fiziksel onay anahtarı içeren hedef mimariye geçiş ve Fonksiyonel Emniyet bağımsızlık gereksiniminin yazılması. |
| **Hafta 5** | [SBOM / CVE / Patch](./labs/hafta-05-sbom-cve-patch.md) | *Vulnerability Decision* | PLC haberleşme kütüphanesinde çıkan kritik bir zafiyetin (CVSS 9.8) analizi, üretimi tehlikeye atmadan müdahale adımları, yama testi, Safety Validation (CRC) kontrolü ve telafi edici kontroller. |

---

## 4. Dosya Yapısı

```text
.
├── README.md
└── labs/
    ├── hafta-01-safety-architecture.md
    ├── hafta-02-iec62443-zone-conduit.md
    ├── hafta-03-threat-risk-matrix.md
    ├── hafta-04-secure-remote-access.md
    └── hafta-05-sbom-cve-patch.md
```
