
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



🎛Yazıcı ayarı
printer.cfg dosyanızın ilk satırna  
[include chameleon.cfg] 
ekleyin


######################################################       ####################################################################

                                                  MAKRO AYAR VE DEĞERLERİ

--------------------------------------------------------------------------------


pin çıkışlarını kendi bord unuza göe ayarlayınız

[manual_stepper selector]
step_pin: nano:PE0
dir_pin: nano:PB9
microsteps: 1
rotation_distance: 2
enable_pin: !nano:PE1
endstop_pin: !nano:PA2  
velocity: 20      
accel: 20      
position_endstop: 15

[manual_stepper chameleon]
step_pin: nano:PB5
dir_pin: nano:PB4
microsteps: 16
rotation_distance: 41
enable_pin: !nano:PB8
endstop_pin: nano:PE6   ( bu pin önemli CHAMELEON_EXTRUDER_SMART_LOAD buradaki pine edn step yaparak filament konumu ayaralıyor)
position_endstop: 0

sensör pinleriniz anakartınıza göre ayarlayınız..


[filament_switch_sensor tool_0_sensor]
switch_pin: nano:PA15
pause_on_runout: True

[filament_switch_sensor tool_1_sensor]
switch_pin: nano:PA12
pause_on_runout: True

[filament_switch_sensor tool_2_sensor]
switch_pin: nano:PA14
pause_on_runout: True

[filament_switch_sensor tool_2_sensor]
switch_pin: nano:PA13
pause_on_runout: True

[filament_switch_sensor mmu_check]
switch_pin: nano:PB2
pause_on_runout: False


--------------------------------------------------------------------------------


[gcode_macro TOOL_INIT]
description: Homes the tool selector. Needed before any tool change.
variable_tool: 0
variable_ysplit_distance: 100
variable_chameleon_speed: 40
variable_chameleon_acc: 40
gcode:
  MANUAL_STEPPER STEPPER=selector SET_POSITION=0 
  MANUAL_STEPPER STEPPER=selector MOVE=-1.40
  MANUAL_STEPPER STEPPER=selector SET_POSITION=0 
  MANUAL_STEPPER STEPPER=selector MOVE=0.12
  MANUAL_STEPPER STEPPER=selector SET_POSITION=0 
  MANUAL_STEPPER STEPPER=chameleon SET_POSITION=0


---->variable_ysplit_distance: 100  dğerini filamnet hub ölçünüze göre ayarlaryın yspit

----->MANUAL_STEPPER STEPPER=selector MOVE=-1.40      ( bu değeri elle konsola deneyerek girnizi -1 den  -2 ye kadar deneyin selktör şaftı üzerindeki çıkıntının paralel konumu sizin başlangıç açınız olacakrtır.

------------------------------------------------------------------------------------------

[gcode_macro FILAMENT_CUT]
description: Filament kesme, tamamen otomatik güvenli Z
gcode:
  {% set F = params.F|default(6000)|int %}
  {% set current_z = printer.toolhead.position.z %}
  {% set max_model_z = params.model_z|default(current_z)|float %}
  {% set Z_safe = [current_z + 5, max_model_z + 5]|max %}

  
  G90 
  G1 Z{Z_safe} F3000
  G1 X290 Y290 F{F}

  G92 E0
  G1 E-2 F300
  G92 E0
  G1 E-90 F2500
  G92 E0; 
  G1 X300 Y290 F4000  
  G1 X250 Y299 F4000
  G1 X300 Y299 F4000
  G1 X290 Y290 F4000
  M117 "Filament kesildi"
  G92 E0  

filamnet kesme konumunuzu ve filamnet kesiciye çekme ölçünüzü yazıcınıza yapısına göre ayarlayınız


----->  G1 E-90 F2500 
nuzell ucu ile kesici bıcak arasındaki mesafenizi giriniz bu değer filamet ucundan 2 mm olacak şekilde ayarlanırsa filameet atığınız azalır

kesme konumu ve kesme hareketleri
---->G1 X290 Y290 F{F}

  G1 X300 Y290 F4000  
  G1 X250 Y299 F4000
  G1 X300 Y299 F4000
  G1 X290 Y290 F4000

  -----------------------------------------------------------------------------

[gcode_macro TOOL_SELECT]
gcode:
  {% set TOOL = params.TOOL|int %}
  {% set SELECTOR_SPEED = 20 %}
  M117 "Setting tool to TOOL{TOOL}"
  
 
  {% if TOOL == 0 %}
    MANUAL_STEPPER stepper=selector MOVE=0
  {% elif TOOL == 1 %}
    MANUAL_STEPPER stepper=selector MOVE=-4.5
  {% elif TOOL == 2 %}
    MANUAL_STEPPER stepper=selector MOVE=-9.5
  {% elif TOOL == 3 %}
    MANUAL_STEPPER stepper=selector MOVE=-12.5
  {% endif %}
  SET_GCODE_VARIABLE MACRO=TOOL_INIT VARIABLE=tool VALUE={TOOL}


her takımın konumunu konsoldan  MANUAL_STEPPER stepper=selector MOVE=0  manuel olarakdeğer girerek tespit edin
t0 t1 t2 t3 takım yolu açılarını bulunuz ve sisteme kayıt ediniz. açı değerleinin sağlıklı olduğundan emin olun filamneti yol seçimleri belirlediğiniz bu değerlere göre yapılacaktır.

--->MANUAL_STEPPER stepper=selector MOVE=0
--->MANUAL_STEPPER stepper=selector MOVE=-4.5
--->MANUAL_STEPPER stepper=selector MOVE=-9.5
--->MANUAL_STEPPER stepper=selector MOVE=-12.5

----------------------------------------------------------------------------------------------------------------------------------

[gcode_macro TOOL_FREE]
gcode:
  {% set TOOL = params.TOOL|int %}
   M117 "Free filament path for tool {TOOL}"
  {% if TOOL == 0 %}
    MANUAL_STEPPER stepper=selector MOVE=-12.5
  {% elif TOOL == 1 %}
    MANUAL_STEPPER stepper=selector MOVE=-9.5
  {% elif TOOL == 2 %}
    MANUAL_STEPPER stepper=selector MOVE=0
  {% elif TOOL == 3 %}
    MANUAL_STEPPER stepper=selector MOVE=-4.5
  {% endif %}

TOOL_SELECT makrosune göre  ters örüntülü  olarak değerleri girebilirsiniz.

-----------------------------------------------------------------------------------------------------------------------

[gcode_macro CHAMELEON_EXTRUDER_SMART_LOAD]


    {% if TOOL == 0 or TOOL == 1 %}
        MANUAL_STEPPER stepper=chameleon MOVE=-95 speed={SPEED} accel={ACC}
        MANUAL_STEPPER stepper=chameleon SET_POSITION=0
    {% else %}
        MANUAL_STEPPER stepper=chameleon MOVE=95 speed={SPEED} accel={ACC}
        MANUAL_STEPPER stepper=chameleon SET_POSITION=0
    {% endif %}
    
---->MANUAL_STEPPER stepper=chameleon MOVE=-95 speed={SPEED} accel={ACC}
---->MANUAL_STEPPER stepper=chameleon MOVE=95 speed={SPEED} accel={ACC}

[gcode_macro TOOL_INIT] makrosu içindeki variable_ysplit_distance: 100 değerinden 5 ila 10 puan düşük giriniz

----------------------------------------------------------------------------------------------------------------------------

[gcode_macro EXTRUDER_UNLOAD]

----> G1 E-42 F2000 
filamnet kesici ile extuder dişilileri arasındaki mesafenizi giriniz ölçünüzden 10 puan falza giremniz yaralı olacaktır. örnek kesici ile dişili arasında 40mm ölçüm yaptı iseniz 50 mm giriniz


----------------------------------------------------------------------------------------------------------------------------------
[gcode_macro EXTRUDER_LOAD]

---->G1 E108 F2000  extuder ile nuzel arasındakş  ölçünüzü girniz değerin tam olması filamnet  çapaklanmasını azaltacaktır normal ölçünüzden 1mm düşük girmenizi tavsiye ederim





