---
id: anleitungen-11-eclipse-setup
title: Eclipse-Setup für ABAP-Entwicklung
sidebar_label: "Eclipse-Setup"
---

## Einführung
Diese Anleitung führt dich Schritt für Schritt durch die Einrichtung von **Eclipse** für die **ABAP-Entwicklung** in der **SAP BTP ABAP Environment**.  

---

## 1. Eclipse installieren
Lade die **aktuellste Version von Eclipse** herunter und installiere sie.  
*(Mit älteren Versionen funktioniert die ABAP-Umgebung nicht!)*

🔗 [Eclipse Download & Installation](https://www.eclipse.org/downloads/packages/installer)

---

## 2. ABAP Development Tools (ADT) in Eclipse installieren
1. In Eclipse: **Help → Install New Software...** auswählen.  
2. Folgende URL eingeben und mit **Enter** bestätigen: https://tools.hana.ondemand.com/latest
3. **ABAP Development Tools** auswählen und **Next** klicken.  
4. Lizenzvereinbarung akzeptieren und Installation abschließen.

---

## 3. SAP BTP Trial Account anlegen
Erstelle einen **SAP BTP Trial Account**:  
🔗 [Anleitung SAP BTP Trial Account](https://developers.sap.com/tutorials/hcp-create-trial-account.html)

---

## 4. ABAP Environment Trial User erstellen
Erstelle einen **Trial User für die ABAP Environment**:  
🔗 [Anleitung ABAP Environment Trial User](https://developers.sap.com/tutorials/abap-environment-trial-onboarding.html)

---

## 5. ABAP Cloud Project in Eclipse anlegen
1. In Eclipse: **File → New → Other → ABAP Cloud Project** auswählen.

![Eclipse-Setup1](/img/Eclipse-Setup1.png)

2. URL aus dem **SAP BTP Cockpit** kopieren, in Eclipse einfügen und **Next** klicken.

![Eclipse-Setup2](/img/Eclipse-Setup2.png)  
![Eclipse-Setup3](/img/Eclipse-Setup3.png)

3. Auf **Open Logon Page in Browser** klicken.  
Wenn du im Cockpit angemeldet bist, wirst du automatisch weitergeleitet.

![Eclipse-Setup4](/img/Eclipse-Setup4.png)

4. Auf **Finish** klicken.

![Eclipse-Setup5](/img/Eclipse-Setup5.png)

---

## 6. ABAP-Entwicklungspaket anlegen
Erstelle ein **ABAP-Entwicklungspaket**:  
🔗 [Anleitung Entwicklungspaket](https://keal57.github.io/DHBWRV-ABAP/additional-material/instructions/setup-abap-environment)

---

## 7. abapGit in Eclipse installieren
Installiere **abapGit** in Eclipse (nur Installation, kein Repository anlegen):  
🔗 [Anleitung abapGit installieren](https://keal57.github.io/DHBWRV-ABAP/additional-material/instructions/use-git-ondemand)

---
