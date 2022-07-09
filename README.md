# tello-talent

## Inleiding
Tello talent bestaat uit twee onderdelen: de educatieve Tello drone en een extra programmeerbaar element, de RMTT (remote Tello Talent o.i.d.). De educatieve Tello is in zichzelf al via telemetrie te besturen. Hiervoor moet je bijv. met een laptop verbinden met het WiFi SSID van de Tello en met een app die UDP packets over poort 8889 kan zenden de berichten versturen. De SDK waarmee de Tello bestuurd kan worden is [hier](https://dl.djicdn.com/downloads/RoboMaster+TT/Tello_SDK_3.0_User_Guide_en.pdf) te vinden. Deze bestanden zijn ook aan dit repository toegevoegd.

De RMTT kan deze commando's ook via de seriële ingang van de Tello verzenden, met toevoeging van het prefix '[TELLO] '. Antwoorden komen terug met het prefix ETT.

## Tello sensors
De Tello heeft een frontfacing camera die een videostream uit uitvoeren via UDP poort 11111.

De Tello heeft ook een downfacing camera voor het herkennen van mission pads. Deze mission pads helpen bij positionering. Je kunt de Tello naar een mission pad sturen en dan een x y z coördinaat van -500 - 500 geven. Onbekend is wat de absolute verplaatsing t.o.v. die missionpads is.

De Tello heeft ook een (infrarood?)sensor voor de afstand tot de grond. Onbekend is nog hoe / of deze uitgevraagd kan worden.

## RMTT programmeren
De RMTT bevat een ESP32 controller die los programmeerbaar is. Op het moment dat de RMTT op de Tello wordt aangesloten, verandert het SSID van 'TELLO.....' naar 'RMTT.....'. Het lijkt erop dat de TELLO zijn WiFi op dat moment uitzet en de RMTT het SSID presenteert. Dit heb ik echter nog niet met zekerheid kunnen terugvinden. Het zou kunnen dat het nog steeds de TELLO is die het WiFi netwerk presenteert, maar met een andere SSID.

De RMTT is te programmeren met het Arduino framework. Hiervoor zijn libraries beschikbaar, zie [hier](https://github.com/RoboMaster/RMTT_Libs). Deze libaries zijn ook aan dit repo toegevoegd. 

De RMTT heeft een RGB-led aan de bovenkant en een drukknop aan de zijkant. Deze zijn los aan te sturen of uit te lezen, evt. via de RMTT-libraries. De RMTT heeft ook een connector waarop een meegeleverd accessoire kan worden aangesloten of andere, zelfgemaakte hardware. Hiervoor is bij de drone een los printje meegeleverd die de connector omzet naar (holes voor)dupont pins met een 2.54 spacing. Het meegeleverde accessoire is een 8x8 matrix met RGB leds met daarbovenop een TOF-afstandsmeter die metingen kan doen tot 2 meter.

## Arduino IDE voorbereiden
Voor het programmeren van de RMTT moeten eerst ESP32 boards worden toegevoegd aan de Arduino IDE:
- Ga naar de voorkeuren van het programma en vul deze URL in bij Additional Board Manager URLs: https://dl.espressif.com/dl/package_esp32_index.json
- Zoek in de Board Manager naar 'esp32' en installeer deze boards.
- Kies 'ESP32 WRover Kit (all versions)
- Kies de volgende instellingen:
  - Upload speed: "921600"
  - CPU Frequency: "240MHz (WiFi/BT)" --> onzeker over noodzaak van deze instelling, is misschien niet helemaal correct
  - Flash Frequency: "40MHz"
  - Flash Mode: "DIO"
  - Flash Size: "2MB (16Mb)"
  - Partition Scheme: "Minimal (1.3MB APP/700KB SPIFFS)"
- Bij het aansluiten is de RMTT onder 'Poort' te vinden als 'usbserialxxx'.
- De default serial port baudrate is 115200. Het is raadzaam dit standaard aan te houden, ook in eigen seriële communicatie omdat fouten ook met die baudrate worden gecommuniceerd.
