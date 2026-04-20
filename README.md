# Tao Wang | Software Development Learning Portfolio

![Java](https://img.shields.io/badge/Java-Learning-ED8B00?logo=openjdk&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-Labs-FCC624?logo=linux&logoColor=black)
![IHK AP1](https://img.shields.io/badge/IHK-AP1%20Prep-00599C)
![Backend](https://img.shields.io/badge/Backend-Mini%20Projects-0A7EA4)
![Algorithms](https://img.shields.io/badge/Algorithms-Java%20Training-2E8B57)

> DE zuerst, EN second. Structured for IHK preparation and long-term software engineering growth.

## Ueberblick (DE)
Ich bin Umschuelerin zur Fachinformatikerin fuer Anwendungsentwicklung in Deutschland.  
Dieses Profil ist mein zentrales Lern-Dashboard mit klarer Trennung von **Hub**, **Courses (Lernfelder 1-9)**, **Notizen**, **Labs** und **Projekten**.

## Snapshot
| Bereich | Fokus | Link |
|---|---|---|
| Learning Hub | Roadmap, Wochenziele, Lernfortschritt, **Courses (Lernfelder 1-9)** | [IT-Learning-Journey](https://github.com/lilarosa/IT-Learning-Journey) |
| IHK AP1 Prep | Aufgaben, Loesungen, Mock Exams | [ihk-ap1-prep](https://github.com/lilarosa/ihk-ap1-prep) |
| Java Projects | OOP, CLI, Fehlerbehandlung | [java-projects](https://github.com/lilarosa/java-projects) |
| Backend Training | Kleine testbare Java-Projekte | [backend-mini-projects-java](https://github.com/lilarosa/backend-mini-projects-java) |
| Algorithm Training | Java DSA fuer AP1/Interviews | [algorithm-training-java](https://github.com/lilarosa/algorithm-training-java) |
| Linux/DevOps Labs | Linux, Shell, Git, spaeter Docker | [devops-linux-labs](https://github.com/lilarosa/devops-linux-labs) |
| Arduino Projects | CPS, Sensoren, Aktoren, Breadboard-Uebungen | [Arduino-Projects](https://github.com/lilarosa/Arduino-Projects) |
| Frontend Projects | 01 Chinesisch, 02 Hungary, 03 CareU, 04 AP1 App | [frontend-projects](https://github.com/lilarosa/frontend-projects) |

## Featured Projects (Selbst entwickelt)
| Projekt | Kurzbeschreibung | Repo | Demo |
|---|---|---|---|
| Chinesisch Lern Website | Interaktive Lernplattform fuer Kinder mit Schriftzeichen, Pinyin und Spielen | [frontend-projects](https://github.com/lilarosa/frontend-projects) | [Live Demo](https://lilarosa.github.io/frontend-projects/projects/01_chinesisch-lernen/) |
| Hungary Survival | Web-App fuer Expats in Ungarn mit Guides, Tools und PWA | [frontend-projects](https://github.com/lilarosa/frontend-projects) | [Live Demo](https://lilarosa.github.io/frontend-projects/projects/02_hungary-survival/) |
| CareU (EN) | Pflege- und Erinnerungsapp mit Aufgaben, Uhrzeit, Vorlesen und Notruf | [frontend-projects](https://github.com/lilarosa/frontend-projects) | [Live Demo](https://lilarosa.github.io/frontend-projects/projects/03_careU/en/) |
| CPS Anzucht-Gewaechshaus (Arduino) | Cyber-Physisches System: Sensoren (Licht/Bodenfeuchte/IR) steuern Aktoren (LED/Pumpe), plus RTC/LCD und Audio-Durchsagen | [Study-notes](https://github.com/lilarosa/Study-notes/tree/main/projects/CPS) | [Docs](https://github.com/lilarosa/Study-notes/blob/main/projects/CPS/docs/cps-graphic-mapping.md) |
| Temperature Fan Control | Arduino CPS Mini-Projekt mit NTC, LED und Luefter inklusive Hysterese-Regelung | [Arduino-Projects](https://github.com/lilarosa/Arduino-Projects) | [Sketch](https://github.com/lilarosa/Arduino-Projects/tree/main/sketches/05_temperature_fan_control) |

## Arduino Code Sample (DE)
Beispiel aus `Temperature Fan Control`: jede Zeile ist auf Deutsch kommentiert, damit Logik, Verdrahtung und Regelverhalten direkt auf der Startseite nachvollziehbar sind.

```cpp
#include <math.h>  // Bietet mathematische Funktionen wie log() fuer die Temperaturberechnung.

const int analogPin = A0;  // Der NTC-Sensor ist am analogen Eingang A0 angeschlossen.
const int ledPin = 6;  // Die Status-LED ist an Pin 6 angeschlossen.
const int fanPin = 10;  // Der Luefter wird ueber Pin 10 geschaltet.

const float targetTempC = 20.0;  // Gewuenschte Solltemperatur in Grad Celsius.
const float hysteresisC = 0.5;  // Hysterese verhindert zu haeufiges Ein- und Ausschalten.

bool fanOn = false;  // Speichert, ob der Luefter aktuell eingeschaltet ist.
unsigned long lastLogTime = 0;  // Speichert den Zeitpunkt der letzten seriellen Ausgabe.
const unsigned long logIntervalMs = 2000;  // Ausgabeintervall fuer den seriellen Monitor in Millisekunden.

const int fixedResistor = 10000;  // Festwiderstand des Spannungsteilers in Ohm.
const int betaValue = 3950;  // Beta-Wert des NTC fuer die Umrechnung in Temperatur.
const int r0 = 10000;  // Nennwiderstand des NTC bei 25 Grad Celsius.

void setup() {  // Diese Funktion wird beim Start genau einmal ausgefuehrt.
  Serial.begin(9600);  // Startet die serielle Kommunikation mit 9600 Baud.
  pinMode(ledPin, OUTPUT);  // Setzt den LED-Pin als Ausgang.
  pinMode(fanPin, OUTPUT);  // Setzt den Luefter-Pin als Ausgang.
}  // Ende der Setup-Funktion.

void loop() {  // Diese Funktion laeuft danach fortlaufend in einer Schleife.
  int raw = analogRead(analogPin);  // Liest den aktuellen Analogwert des NTC ein.
  if (raw <= 0) {  // Prueft, ob ein ungueltiger oder kritischer Messwert vorliegt.
    delay(500);  // Wartet kurz, bevor erneut gemessen wird.
    return;  // Bricht den aktuellen Schleifendurchlauf ab.
  }  // Ende der Sicherheitsabfrage fuer den Messwert.

  float rNtc = fixedResistor * (1023.0 / raw - 1.0);  // Berechnet den aktuellen Widerstand des NTC.
  float tempK = 1.0 / (1.0 / 298.15 + (1.0 / betaValue) * log(rNtc / r0));  // Rechnet den Widerstand in Kelvin um.
  float tempC = tempK - 273.15;  // Wandelt Kelvin in Grad Celsius um.

  if (millis() - lastLogTime >= logIntervalMs) {  // Prueft, ob das Ausgabeintervall erreicht wurde.
    Serial.print("Temperature: ");  // Gibt einen Text als Einleitung im seriellen Monitor aus.
    Serial.print(tempC);  // Gibt den aktuellen Temperaturwert aus.
    Serial.println(" C");  // Ergaenzt die Einheit und erzeugt einen Zeilenumbruch.
    lastLogTime = millis();  // Speichert den Zeitpunkt der letzten Ausgabe.
  }  // Ende der seriellen Protokollierung.

  if (!fanOn && tempC > targetTempC + hysteresisC) {  // Schaltet ein, wenn der obere Grenzwert ueberschritten wird.
    digitalWrite(fanPin, HIGH);  // Aktiviert den Luefter.
    digitalWrite(ledPin, HIGH);  // Aktiviert die LED als Statusanzeige.
    fanOn = true;  // Merkt sich den neuen Zustand "Luefter an".
  } else if (fanOn && tempC < targetTempC - hysteresisC) {  // Schaltet aus, wenn der untere Grenzwert unterschritten wird.
    digitalWrite(fanPin, LOW);  // Deaktiviert den Luefter.
    digitalWrite(ledPin, LOW);  // Deaktiviert die LED.
    fanOn = false;  // Merkt sich den neuen Zustand "Luefter aus".
  }  // Ende der Zweipunktregelung mit Hysterese.

  delay(200);  // Kleine Pause zur Stabilisierung der Schleife.
}  // Ende der Hauptschleife.
```

## Diese Woche (Status)
| Bereich | Ziel | Status |
|---|---|---|
| IHK AP1 | 1 Mock-Exam + SQL Wiederholung | In Arbeit |
| Java | 5 Aufgaben in `algorithm-training-java` | In Arbeit |
| Backend | `01-student-api` Struktur abschliessen | Geplant |
| Linux/DevOps | Week-01 Labs dokumentieren | In Arbeit |

## Lernstruktur
| Kategorie | Repositories |
|---|---|
| Courses | [IT-Learning-Journey](https://github.com/lilarosa/IT-Learning-Journey) |
| Notes | [Study-notes](https://github.com/lilarosa/Study-notes) |
| Labs | [Linux-Week1-Lab](https://github.com/lilarosa/Linux-Week1-Lab), [DHCP-Apache-Webserver_Week2_Lab](https://github.com/lilarosa/DHCP-Apache-Webserver_Week2_Lab) |
| Projects | [frontend-projects](https://github.com/lilarosa/frontend-projects), [hungary-survival](https://github.com/lilarosa/hungary-survival), [Arduino-Projects](https://github.com/lilarosa/Arduino-Projects) |

## Aktuelle Ziele (2026)
- AP1 systematisch vorbereiten (SQL, Java, UML, Testing, Netzwerk)
- Java von Uebungsniveau zu testbaren Mini-Projekten entwickeln
- Linux/DevOps Grundlagen in reproduzierbaren Labs festigen
- Jedes Repository mit klarer README- und Wochenplan-Struktur pflegen

## Profile (EN)
I am retraining as a software developer in Germany. This profile is my learning dashboard organized into **hub**, **courses (Lernfelder 1-9)**, **notes**, **labs**, and **projects**.

## This Week (Status)
| Area | Goal | Status |
|---|---|---|
| IHK AP1 | 1 mock exam + SQL refresh | In progress |
| Java | 5 tasks in `algorithm-training-java` | In progress |
| Backend | Finish `01-student-api` base structure | Planned |
| Linux/DevOps | Document week-01 labs | In progress |

## 2026 Focus (EN)
- Structured IHK AP1 preparation
- Java progression from exercises to testable mini-projects
- Reproducible Linux/DevOps labs
- Consistent repository documentation and weekly plans

## Contact
- GitHub: [@lilarosa](https://github.com/lilarosa)
