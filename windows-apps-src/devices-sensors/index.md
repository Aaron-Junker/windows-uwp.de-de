---
ms.assetid: 0b891f63-02fa-4c30-b307-9fbcccac5caa
title: Geräte, Sensoren und Leistung
description: Damit Ihren Benutzern umfangreiche Einsatzmöglichkeiten geboten werden können, ist es möglicherweise erforderlich, externe Geräte oder Sensoren in die App zu integrieren.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 06dd8355cd0afd3889c0db2c337c93b5fbedf0fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57582742"
---
# <a name="devices-sensors-and-power"></a>Geräte, Sensoren und Leistung


Damit Ihren Benutzern umfangreiche Einsatzmöglichkeiten geboten werden können, ist es möglicherweise erforderlich, externe Geräte oder Sensoren in die App zu integrieren. Hier folgen einige Beispiele zu Features, die Sie mithilfe der in diesem Abschnitt beschriebenen Technologie zur App hinzufügen können.

-   Bereitstellen verbesserter Druckfunktionen
-   Integrieren von Bewegungs- und Ausrichtungssensoren in Ihr Spiel
-   Herstellen einer direkten Verbindung oder einer Verbindung über ein Netzwerkprotokoll mit einem Gerät

| Thema | Beschreibung |
|-------|-------------|
| [Aktivieren von Gerätefunktionen](enable-device-capabilities.md) | In diesem Lernprogramm wird beschrieben, wie Gerätefunktionen in Microsoft Visual Studio deklariert werden. Diese Funktionen ermöglichen Ihrer App die Verwendung von Kameras, Mikrofonen, Positionssensoren und anderen Geräten. | 
| [Aktivieren des Benutzermoduszugriffs für Windows IoT](enable-usermode-access.md) | In diesem Lernprogramm wird der Benutzermoduszugriff auf GPIO, I2C, SPI und UART auf Windows 10 IoT Core beschrieben. |
| [Auflisten von Geräten](enumerate-devices.md) | Der Enumeration-Namespace ermöglicht die Suche nach Geräten, die intern mit dem System verbunden, extern verbunden oder über Drahtlos- oder Netzwerkprotokolle erkannt werden können. |
| [Koppeln von Geräten](pair-devices.md) | Einige Geräte müssen gekoppelt werden, bevor sie verwendet werden können. Der [<strong>Windows.Devices.Enumeration</strong>](https://msdn.microsoft.com/library/windows/apps/BR225459)-Namespace unterstützt drei verschiedene Verfahren zum Koppeln von Geräten: |
| [Point of Service (POS)](point-of-service.md) | In diesem Abschnitt wird die Interaktion mit Point Of Service-Peripheriegeräten wie etwa Strichcodescannern, Belegdruckern, Kassenschubladen usw. beschrieben. | 
| [Sensoren](sensors.md) | Mithilfe von Sensoren können Apps die Beziehung zwischen einem Gerät und der physischen Umgebung ermitteln. Sensoren können für die App die Richtung, Ausrichtung und Bewegung des Geräts erfassen. |
| [Bluetooth](bluetooth.md) | Dieser Abschnitt enthält Artikel zur Bluetooth-Integration in UWP-Apps (Universelle Windows-Plattform). Dies umfasst u. a. die Verwendung von RFCOMM-, GATT- und LE (Low Energy)-Ankündigungen. | 
| [Drucken und Scannen](printing-and-scanning.md) | In diesem Abschnitt wird das Drucken und Scannen aus Ihrer Universellen Windows-App beschrieben. | 
| [3D-Druck](3d-printing.md) | Dieser Abschnitt beschreibt die Verwendung der 3D-Druckfunktionen in Ihrer Universellen Windows-App. |
| [Erstellen einer NFC-Smartcard-App](host-card-emulation.md) | Windows Phone 8.1 hat Apps mit NFC-Kartenemulation per SIM-basiertem sicherem Element unterstützt. Für dieses Modell war es aber erforderlich, dass Apps für das sichere Bezahlen eng mit den Betreibern von mobilen Netzwerken gekoppelt waren. Dadurch wurde die Vielfältigkeit möglicher Zahlungslösungen anderer Händler oder Entwickler eingeschränkt, die nicht mit Betreibern von mobilen Netzwerken gekoppelt waren. In Windows 10 Mobile haben wir eine neue Technologie für die Kartenemulation eingeführt, die die Bezeichnung „Host-Kartenemulation“ (Host Card Emulation, HCE) trägt. Mithilfe von HCE-Technologie kann Ihre App direkt mit einem NFC-Kartenleser kommunizieren. In diesem Thema wird veranschaulicht, wie die Host-Kartenemulation (HCE) für Windows 10 Mobile-Geräte funktioniert und wie Sie eine HCE-App entwickeln können, bei der Kunden ohne Zusammenarbeit mit dem Betreiber eines mobilen Netzwerks statt mit einer physischen Karte per Smartphone auf Ihre Dienste zugreifen können. |
| [Abrufen von Akkuinformationen](get-battery-info.md) | Erfahren Sie, wie Sie mithilfe von APIs im [<strong>Windows.Devices.Power</strong>](https://msdn.microsoft.com/library/windows/apps/Dn895017)-Namespace ausführliche Akkuinformationen erhalten |

