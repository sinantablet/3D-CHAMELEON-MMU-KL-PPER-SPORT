
3D chameleoan için Akıllı Çoklu Filament Yönetim Sistemi (MMU) Yapılandırma ve Güvenlik Protokolleri
Akıllı Çoklu Filament Yönetim Sistemi (MMU) Yapılandırma ve Güvenlik Protokolleri

Bu sistem; çok renkli ve çok malzemeli (Multi-Material) 3D yazıcı ekosistemlerinde, baskı sürecinin kesintisiz sürmesini sağlamak, kullanıcı hatalarını en aza indirmek, donanım güvenliğini korumak ve baskı sonrasını tamamen otomatikleştirmek adına geliştirilmiş altı aşamalı bir akıllı yönetim mimarisine sahiptir.

1. Başlangıç Kalibrasyonu ve Güvenli Tahliye Protokolü

Sistem ilk enerji aktivasyonunu gerçekleştirdiğinde,Eğer besleme yollarında veya extruder gövdesinde önceden kalmış bir filament algılanırsa, olası sıkışmaları ve nozzle tıkanmalarını önlemek adına otomatik boşaltma algoritması devreye girer. Sistem, eriyik bölgedeki malzemeyi 200 derecede ve güvenli bir şekilde geri çekerek sistemi temizler ve mekanik bileşenleri başlangıç (Home) konumuna getirir.

2. Ardışık Takım Kontrolü ve Ön Yükleme Senkronizasyonu

İlk açılış rutinlerinin ardından, sistem bünyesindeki tüm filament sürücü takımları (T0, T1, T2, T3) sırayla elektronik ve mekanik teste tabi tutulur. Doğrulama sürecinde, aktif hatlardaki filamentler hızlandırılarak önceden belirlenmiş referans bekleme konumuna (hazırda bekletme noktası) otomatik olarak sürülür. Bu sayede renkli yazdırma için filament ler hazır konumda bekletiilir.

3. Dinamik Filament Sonu Algılama ve Otomatik Güvenli Park (Runout)

Baskı esnasında herhangi bir takımdaki filament tükendiğinde veya koptuğunda, hat üzerindeki mikro-sensörler durumu anlık olarak ana karta bildirir. Sistem, baskıyı kusursuz şekilde sürdürebilmek adına "Görüntü ve Pozisyon Koruma" moduna geçer: Kafayı güvenli bir koordinata park eder, ısı değerlerini optimize eder ve kullanıcıdan manuel otomotik yeni filament yüklemesi beklemek üzere sistemi askıya (Pause) alır.

4. Akıllı Manuel Besleme Sonrası Otomatik Takım Eşleştirme

Baskı askıdayken veya yazıcı hazır konumdayken, kullanıcının boşalan takımlardan herhangi birine manuel olarak filament yerleştirmesi sistem tarafından dinamik olarak izlenir. Filamentin ilgili giriş sensöründen geçmesiyle birlikte, sistem hangi takıma müdahale edildiğini akıllıca algılar, o kanala ait motor sürücüsünü aktif konuma getirir ve filamenti el gücüne gerek kalmadan otomatik olarak kavrayıp başlangıç pozisyonuna ilerletir.

5. Ekstrüzyon Sonrası Geri Besleme ve Akıllı Sıkışma Döngüsü (Anti-Jam)
Yazdırma işlemi esnasında filament varlığı, sadece giriş anında değil, extruder dişlilerinin hemen sonrasında da anlık olarak kontrol edilir. Eğer motor dönmesine rağmen extruder çıkışındaki sensörden malzeme akışı algılanmazsa, sistem bunu bir "makara takılması" veya "besleme kaçırma" hatası olarak yorumlar. Sistem, baskıyı hemen durdurmak yerine malzemeyi geri çekip tekrar ileri sürmeyi deneyerek 2 kez otomatik yükleme döngüsü başlatır. Hata çözülmezse donanım güvenliği için sistemi korumalı duraklatma moduna alır. sorun giderildikten sonra baskıya devem edilebilir.

6. Otomatik Baskı Sonu Boşaltma ve Sonraki Baskıya Hazırlık Protokolü
Baskı başarıyla tamamlandığı an, sistem eritme potasını ve filament yolunu bir sonraki işe sıfır hata ile hazırlamak için TOOL_OUT makrosunu tetikler. Bu protokol sırasıyla; filament kesme (filament cut) mekanizmasını çalıştırır, extruder içinde kalan erimiş veya kesilmiş malzemeyi tahliye eder (extruder unload) ve son olarak aktif olan takım hangisi ise o takıma ait filamenti güvenli başlangıç (home) konumuna geri çeker. Böylece bir sonraki baskı öncesinde elle müdahaleye gerek kalmaz.

TOOL_OUT Dilimleyici (Slicer) Entegrasyonu
Bu akıllı bitiş protokolünün sorunsuz çalışabilmesi için dilimleyici yazılımınızın (OrcaSlicer, PrusaSlicer veya Bambu Studio) Bitiş G-Kodu (End G-code) alanına eklenmesi gerekir.
