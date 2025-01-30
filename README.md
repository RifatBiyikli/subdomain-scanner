Alt Alan Adı Keşif Aracı


İsim: Rıfat Bıyıklı
Öğrenci No: 2320191076

Hedef domain için pasif ve aktif tarama yöntemlerini birleştirerek alt alan adlarını belirleyen güçlü bir araç. Bu araç, güvenlik araştırmacıları, penótrasyon test uzmanları ve geliştiriciler için keşfedilme görevlerini kolaylaştırmak için tasarlanmıştır.

Özellikler

Pasif Tarama

Sertifika Şeffaflık (CT) loglarından alt alan adları çıkarımı (crt.sh gibi).

Aktif Tarama

Kelime listesi tabanlı brute force alt alan adı keşif.

İlgili alt alan adlarını belirlemek için permütasyon tabanlı tarama.

DNS Analizi

DNS kayıtlarını çözümleme (A, CNAME, MX).

Wildcard DNS yapılandırmalarını tespit edin ve sonuçları filtreleyin.

Doğrulama ve Erişilebilirlik Kontrolleri

Keşfedilen alt alan adlarını doğrulama.

Wildcard DNS'ten etkilenen sonuçları hariç tutma.

Kurulum

Gereksinimler

Python 3.7 veya üstü

Gerekli Python Kütüphaneleri

Gerekli bağımlılıkları pip ile yükleyin:

pip install requests dnspython aiohttp

Kullanım

Depoyu klonlayın:

git clone https://github.com/yourusername/subdomain-scanner.git
cd subdomain-scanner

Brute force tarama için bir kelime listesi hazırlayın:

Özel bir kelime listesi kullanabilir veya SecLists'den birini indirebilirsiniz.

Aracı çalıştırın:

python subdomain_scanner.py

Hedef domain ve kelime listesi dosyasının yolunu girin:

Enter the target domain: example.com
Enter the path to the wordlist: ./wordlists/common.txt

Tarama tamamlandığında sonuçları results.txt dosyasında görüntüleyin.

Çıktı

Sonuç Dosyası: Keşfedilen tüm alt alan adları results.txt dosyasına kaydedilir.

Wildcard Filtreleme: Wildcard DNS girişleriyle eşleşen alt alan adları sonuçlardan hariç tutulur.

Proje Yapısı

subdomain-scanner/
├── src/
│   ├── main.py          # Ana çalışma dosyası
│   ├── scanner.py       # Alt alan adı tarama mantığı
│   ├── resolver.py      # DNS çözümleme fonksiyonları
│   ├── utils.py         # Yardımcı fonksiyonlar
├── wordlists/
│   ├── common.txt       # Örnek kelime listesi
├── output/
│   ├── results.txt      # Tarama sonuçları
├── tests/
│   ├── test_scanner.py  # Birim testler
├── README.md            # Proje dokümanı
├── .gitignore           # Git tarafından yok sayılan dosyalar
└── requirements.txt     # Python bağımlılıkları

İleri Düzey Özellikler (Gelecek Geliştirmeler)

API Entegrasyonları:

VirusTotal, Shodan veya Censys gibi API'lerle entegrasyon.

HTTP ve HTTPS Kontrolleri:

Keşfedilen alt alan adların aktif web sunucusu barındırıp barındırmadığını doğrulama.

Permütasyon ve Mutasyonlar:

Uç durumlar için alt alan adı mutasyonlarını uygulama (örn., dev, test, staging).

Raporlama:

Daha iyi veri işleme için sonuçları CSV veya JSON formatında dışa aktarın.


Bu araç, sadece eğitim ve etik amaçlarla kullanılmalıdır. Araç, açık bir izin olmaksızın domainlere karşı yetkisiz kullanım için kesinlikle yasaktır.

İyi Taramalar! 🚀

