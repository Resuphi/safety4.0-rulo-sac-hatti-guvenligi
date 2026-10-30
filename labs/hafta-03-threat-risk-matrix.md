# Hafta 3: Tehdit, Risk Analizi ve Risk Matrisi (Threat & Risk Matrix)

**İncelenen Sistem:** EAE Makina - Otomatik Rulo Sac Doğrultma, Sürme, Roll Form ve Boy Kesme Hattı  
**Laboratuvar Çıktısı:** Risk Register (Siber-Fiziksel Risk Kayıt Tablosu)

---

## 1. Metodoloji ve Puanlama Kuralları

Geleneksel BT (Bilgi Teknolojileri) risk analizlerinde bir saldırı zinciri genellikle *"veri çalındı"* veya *"hizmet kesintiye uğradı"* noktasında sonlandırılır. Ancak **Safety 4.0 (OT + Fonksiyonel Emniyet)** yaklaşımında her siber tehdit senaryosu, makine üzerindeki **fiziksel bir sonuca (yaralanma, uzuv kaybı, mekanik yıkım)** bağlanmak zorundadır.

### Risk Formülü ve Parametreler
$$\text{Risk Skoru} = L \text{ (Olasılık)} \times S \text{ (Şiddet)} \times E \text{ (Maruz Kalma)}$$

- **Olasılık ($L$ - Likelihood):** `1` (Düşük / Zor), `2` (Orta), `3` (Yüksek / Kolay istismar edilebilir)
- **Şiddet ($S$ - Severity):** `1` (Hafif / İlk yardım gerektiren), `2` (Orta / Geçici iş göremezlik), `3` (Yüksek / Uzuv kaybı, kalıcı sakatlık veya ölüm)
- **Maruz Kalma ($E$ - Exposure):** `1` (Nadir / Kısa süreli), `2` (Ara sıra / Periyodik), `3` (Sık / Sürekli tehlikeli bölgede çalışma)

> **Altın Kural (Safety Önceliklendirmesi):** Bir senaryoda **Şiddet ($S = 3$)** ise, olasılık veya maruz kalma düşük olduğu için toplam risk skoru düşük çıksa dahi bu senaryo insan hayatına/uzuv bütünlüğüne kastettiği için göz ardı edilemez ve öncelikli müdahale listesine (Öncelikli İzleme / Aksiyon) alınır.

---

## 2. Siber-Fiziksel Risk Kayıt Tablosu (Risk Register)

Aşağıdaki tabloda rulo sac doğrultma, roll form ve boy kesme hattı için kurgulanan 3 temel tehdit senaryosu ve fiziksel etkileri analiz edilmiştir:

| Senaryo No | Tehdit Kaynağı | Giriş Noktası (Zafiyet) | Siber / Dijital Manipülasyon | Fiziksel Süreç Sapması | Fiziksel Sonuç (İnsan Üzerindeki Etki) | $L$ | $S$ | $E$ | Skor ($L \times S \times E$) | Risk Seviyesi ve Öncelik Durumu |
| :-: | :--- | :--- | :--- | :--- | :--- | :-: | :-: | :-: | :-: | :--- |
| **Senaryo 1** | Dış Saldırgan | İnternete açık uzak bakım modülü (`admin/admin` varsayılan hesabı) | Üretim PLC ve servo sürücü parametrelerine sızarak servo sürme hızını ve giyotin makas kesim zamanlamasını bozar | Operatör merdane kalıp ayarı (*jog modu*) yaparken sac beklenmedik şekilde yüksek hızla sürülür ve makas tetiklenir | **Merdanelerde ezilme / giyotin makasta uzuv kaybı** | 3 | 3 | 3 | **27** | **KRİTİK (1. Öncelik)** |
| **Senaryo 2** | Operatör / Bakımcı (İstemeden) | HMI üzerindeki kilitsiz USB portu | Zararlı yazılım (malware) bulaşmış USB bellek ile profil kesim reçetesi yüklenir; HMI ekranı donar (ekran makası "Duruyor" gösterir) | Sıkışan sacı düzeltmek için makas/pres bölgesine giren çalışanın haberi olmadan arka planda PLC kesim döngüsünü sürdürür | **Çalışanın üzerine giyotin makas / pres inmesi sonucu uzuv kaybı** | 2 | 3 | 2 | **12** | **YÜKSEK (2. Öncelik)** |
| **Senaryo 3** | Tesisteki Yetkisiz Kişi | Wi-Fi AP (Pano kapağındaki kartonda yazılı parola) | Kablosuz ağa bağlanarak loop çukuru mesafe sensörü ayarlarını veya 25 tonluk rulo açıcı fren tork parametrelerini bozar | Doğrultucu ile pres arasındaki kalın çelik sac aşırı gerilir veya rulo kontrolsüz boşalarak sac kamçı gibi fırlar | **Hat yanından geçen çalışana yüksek enerjili sac çarpması / ağır yaralanma** | 2 | 3 | 1 | **6** | **ORTA [⚠️ S=3 ÖNCELİKLİ BAYRAK]** (3. Öncelik) |

---

## 3. Senaryoların Değerlendirilmesi ve Önceliklendirme Analizi

### 1) En Yüksek Öncelikli Senaryo Neden Senaryo 1'dir?
**Senaryo 1**, toplamda **27 tam puan ($L=3, S=3, E=3$)** alarak sistemdeki en acil ve yıkıcı riski temsil etmektedir:
- **Olasılık ($L=3$):** Uzak bakım modülü doğrudan internete açıktır ve kimlik doğrulaması `admin/admin` gibi Shodan veya otomatik bot taramalarıyla dakikalar içinde kırılabilecek bir yapıdadır. Saldırganın fiziksel olarak fabrikaya girmesine gerek yoktur.
- **Maruz Kalma ($E=3$):** Roll form ve doğrultma hatlarında farklı sac kalınlıkları ve profil tipleri için operatörler merdane boşluklarını (gap) ve kalıpları sürekli **Jog (adım adım sürme) modunda** makinenin dibinde ayarlamaktadır.
- **Şiddet ($S=3$):** Operatör elleri merdane veya makas bölgesine yakınken hız limitinin (SLS) veya sürme komutunun uzaktan bozulması, doğrudan **ezilme veya uzuv kopması** ile sonuçlanır. Bu nedenle ilk kapatılması gereken açık budur.

### 2) $S=3$ Olup Skoru Düşük Kalan Senaryo 3 Tabloda Nasıl Görünür Kılındı?
**Senaryo 3**'te matematiksel çarpım sonucu $2 \times 3 \times 1 = 6$ (Orta Risk) olarak hesaplanmıştır. Bunun sebebi, çalışanın loop çukuru ve rulo açıcı yanındaki tehlike hattında sürekli bulunmaması ($E=1$) ve saldırganın pano üzerindeki Wi-Fi parolasını görüp yerel kablosuz kapsama alanına girmesi gerekliliğidir ($L=2$).

Ancak standart bir risk matrisinde "6 puan" genellikle düşük/orta kabul edilip ertelenme tehlikesi taşır. Bunu engellemek ve mühendislik dikkatini çekmek için tabloda iki mekanizma uygulanmıştır:
1. **"Risk Seviyesi ve Öncelik Durumu" sütununa açık bir `[⚠️ S=3 ÖNCELİKLİ BAYRAK]` (Severity Override Flag)** eklenmiştir. Bu etiket, toplam skor ne olursa olsun sonucun **ölümcül veya uzuv kaybettirici ($S=3$)** olduğunu gösterir.
2. **Öncelik Sıralaması:** $S=1$ veya $S=2$ olup skoru 8-9 olan (örneğin sadece üretim duruşuna veya hafif sıyrığa yol açan) tüm senaryoların önüne geçirilerek doğrudan aksiyon planına dahil edilmiştir. Böylece pano üzerindeki karton parolanın kaldırılması ve açıcı fren/loop sensör limitlerinin yazılımsal değil donanımsal emniyet sınırlarıyla korunması zorunlu hale getirilmiştir.
