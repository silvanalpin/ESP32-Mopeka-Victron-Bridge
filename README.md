# ESP32-Mopeka-Victron-Bridge
Eine Bridge, die Mopeka pro 200B Bluetooth Sensoren ausliest, die Daten filtert und via MQTT direkt in das Victron Energy Venus OS (Cerbo GX) einspeist, damit sie dort als System-Tanks erscheinen.

Features:

        3-Punkt Kalibrierung: Präzise Füllstandsberechnung auch bei ungeraden Tankformen.

        Slosh-Filter: Verhindert springende Werte während der Fahrt durch gleitenden Durchschnitt und Trend-Analyse.

        Dual-Core Nutzung: BLE-Scanning und MQTT-Kommunikation laufen auf getrennten ESP32-Kernen für maximale Stabilität.

        Web-Dashboard: Einfache Kontrolle der Rohwerte über die IP des ESP32 im Browser.

        OTA-Updates: Firmware-Updates bequem über das WLAN.
        
  Hardware-Voraussetzung:

        ESP32 (DevKit v1 o.ä.)

        Mopeka Pro 200B

        Victron Cerbo GX oder Raspberry Pi mit Venus OS

2. Anpassung der Konfiguration

Öffne die .ino Datei und passe den Abschnitt 1. KONFIGURATION an:
C++

const char* WIFI_SSID = "DEIN_WLAN";
const char* WIFI_PASSWORD = "DEIN_PASSWORT";
const char* MQTT_BROKER = "192.168.xx.xx"; // IP deines Cerbo GX
const char* PORTAL_ID = "cxxxxxxxxxx";    // Deine VRM Portal ID

Trage deine Sensor-MAC-Adressen im Array TANKS ein:
C++

const TankConfig TANKS[] = {
  { "Frischwasser", "cc:d9:ce:db:5f:ef", 100, 1 }, // Name, MAC, Instanz, Typ
  { "Grauwasser", "f1:88:b8:51:03:85", 101, 2 }
};

3. Kalibrierung

Messen Sie den Abstand des Sensors zum Boden (oder dem gewünschten 0%-Punkt) in Millimetern und tragen Sie die Werte ein:

    MM_0: Tank leer

    MM_50: Tank halb voll (hilft bei gewölbten Tanks)

    MM_100: Tank voll

    CAPACITY_M3: Gesamtkapazität in Kubikmetern 

Web Interface

Sobald der ESP32 verbunden ist, kannst du seine IP-Adresse im Browser aufrufen. Du erhältst eine Übersicht:

    Verbindungsstatus (WLAN & MQTT)

    Aktueller Füllstand in Prozent und Litern

    Sensor-Diagnose (Signalqualität, Batteriespannung, Temperatur)

MQTT Details

Der ESP32 sendet Daten an das Topic:
W/<PortalID>/tank/<Instanz>/...

Folgende Pfade werden bedient:

    /Level: Füllstand in %

    /Remaining: Restvolumen in m3

    /Capacity: Gesamtkapazität in m3

    /Status: Sensor-Status (0=OK, 4=Fehler)

    /Temperature: Sensortemperatur in °C

    /BatteryVoltage: Spannung der CR2032 Batterie
