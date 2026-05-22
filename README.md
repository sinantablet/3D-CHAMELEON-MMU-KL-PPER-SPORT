
Klipper Tabanlı Akıllı Çoklu Filament Yönetim Sistemi (MMU)
Bu proje; stock chameleon elektronik kontrol kartına ihtiyaç duymadan, yalnızca 1 adet standart 3D yazıcı anakartı, 6 adet mikro-anahtar (switch/sensör) ve 1 adet mekanik filament kesici kullanarak Klipper üzerinde çalışan 4 renkli (T0-T3) akıllı bir filament yönetim sistemi modifikasyonudur.

Sistem; baskı güvenliğini artırmak, tıkanmaları önlemek ve renk geçişleri ile baskı sonunu tamamen otomatikleştirmek için tasarlanmıştır.

🛠 Donanım Mimarisi ve Sensör Yerleşimi
Sistemde kullanılan 6 adet optik veya mekanik siviç, anakart üzerindeki boş endstop (limit anahtarı) pinlerine bağlanır. Sensörlerin fiziksel konumları ve görevleri şu şekildedir:

A. Takım Giriş Sensörleri (4 Adet)
Konum: T0, T1, T2 ve T3 filament hatlarının tam giriş noktaları.

Görevi: Hangi takıma manuel filament yüklendiğini algılar, otomatik motor senkronizasyonunu başlatır ve ilgili hattaki filamentin bitip bitmediğini (Runout) kontrol eder.

B. Extruder Giriş Sensörü (1 Adet)
Konum: Extruder dişlilerinin hemen girişi (Hotend öncesi).

Görevi: Takımlardan gelen filamentin extruder girişine başarıyla ulaştığını doğrular.

C. Extruder Çıkış Sensörü (1 Adet)
Konum: Extruder dişlilerinin hemen çıkışı.

Görevi: Baskı esnasında gerçek zamanlı malzeme akışını izler. Motor döndüğü halde buradan filament geçmiyorsa, makara takılması veya patinaj (sıkışma) durumunu algılayarak sistemi kurtarma moduna alır.

D. Mekanik Filament Kesici (1 Adet)
Konum: Extruder mekanizmasının hemen altı (Hotend girişinin üstü).

Görevi: Baskı bittiğinde veya renk değişimlerinde filament ucunu keserek pürüzsüz bir tahliye sağlar; hotend içinde malzeme donmasını ve tıkanmayı engeller.

🚀 6 Aşamalı Akıllı Çalışma Mantığı
Sistem, Klipper makroları ve bu 6 sensörün anlık durum bilgileriyle tamamen otomatik bir döngü yönetir:

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

Filament Cut: Mekanik kesici aktif hat üzerindeki filamenti keser.

Extruder Unload: Extruder içinde kalan parça eritilerek tahliye edilir.

Tool Park: Aktif olan takım hangisi ise, o hattın filamenti bir sonraki baskıya hazır olmak üzere kendi başlangıç konumuna geri çekilir.



🎛 Dilimleyici (Slicer) Kurulumu

Sistemin baskı ilk başladığında donanımı ve filament yollarını senkronize eden akıllı protokolümüz TOOL_INIT makrosudur.
(OrcaSlicer, PrusaSlicer, Bambu Studio vb.) Printer Settings -> start g-code  ( TOOL_INIT )
Sistemin baskı sonu otomasyonunu tetikleyebilmesi için dilimleyici yazılımınızın (OrcaSlicer, PrusaSlicer, Bambu Studio vb.) Printer Settings -> end g-code  ( TOOL_OUT  TS0 )

yazıcı ayarlarından -> çoklu malzeme -> tek extruder çoklu mazleme kurulumu
çoklu mazleme sekmesinden -> pirme kulesini aktif edin.


