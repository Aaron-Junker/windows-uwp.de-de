---
title: Neuigkeiten in der Windows-Dokumentation im September 2017
description: Neue Features, Videos und Entwicklerleitfäden in der Entwicklerdokumentation für Windows 10 im September 2017
keywords: Neuigkeiten, Update, Features, Entwicklerleitfäden, Windows 10, 1709
ms.date: 09/06/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fb63a5f7cefbccbedc09b0c74d9daea9d26b63a8
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "74734955"
---
# <a name="whats-new-in-the-windows-developer-docs-in-september-2017"></a>Neuigkeiten in der Windows-Entwicklerdokumentation im September 2017

Die Entwicklerdokumentation für die Windows-Plattform wird kontinuierlich mit Informationen zu neuen Features für Entwickler aktualisiert. Die folgenden Featureübersichten, Entwicklerleitfäden und Beispiele wurden erst kürzlich bereitgestellt und enthalten neue oder aktualisierte Informationen für Windows-Entwickler.

Natürlich steht das Fall Creators Update direkt vor der Tür. Weitere umfassende Dokumentation folgt in den kommenden Monaten!

Nach der [Installation der Tools und des SDKs](https://developer.microsoft.com/windows/downloads#_blank) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/your-first-app.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

## <a name="features"></a>Features

### <a name="xbox-live-creators-program"></a>Xbox Live Creators-Programm

Das Xbox Live Creators-Programm ist jetzt live. Damit kannst du ganz einfach UWP-Spiele erstellen und veröffentlichen, die sowohl auf Windows 10-PCs als auch auf Xbox One-Konsolen ausgeführt werden können. Weitere Informationen findest du unter [Erste Schritte mit dem Xbox Live Creators-Programm](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="developer-guidance"></a>Erläuterungen für Entwickler

### <a name="xaml-basics-tutorials"></a>Tutorials zu XAML-Grundlagen

Wir haben vier [Tutorials zu den XAML-Grundlagen](https://docs.microsoft.com/windows/uwp/design/basics/xaml-basics-ui) erstellt, die eine Ergänzung des neuen [PhotoLab-Beispiels](https://github.com/Microsoft/Windows-appsample-photo-lab) darstellen. Hiermit werden vier Hauptaspekte der XAML-Programmierung abgedeckt: Benutzeroberflächen, Datenbindung, benutzerdefinierte Stile und adaptive Layouts. Jedes Tutorial beginnt mit einer halbfertigen Version eines PhotoLab-Beispiels, und dann werden Schritt für Schritt die fehlenden Komponenten der endgültigen App erstellt. 

![Screenshot: PhotoLab-Beispiel auf der Seite mit der Fotogalerie](images/PhotoLab-gallery-page.png)  

Hier ist eine kurze Übersicht über die neuen Artikel angegeben:

+ [**Erstellen einer Benutzeroberfläche**](https://docs.microsoft.com/windows/uwp/design/basics/xaml-basics-ui) veranschaulicht, wie eine grundlegende Fotogalerie-Benutzeroberfläche erstellt wird.
+ [**Erstellen von Datenbindungen**](https://docs.microsoft.com/windows/uwp/data-binding/xaml-basics-data-binding) veranschaulicht, wie Datenbindungen der Fotogalerie hinzugefügt werden und wie diese mit realen Bilddaten gefüllt wird.
+ [**Erstellen von benutzerdefinierten Stilen**](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-basics-style) veranschaulicht, wie dem Menü für die Fotobearbeitung interessante benutzerdefinierte Stile hinzugefügt werden.
+ [**Erstellen von adaptiven Layouts**](https://docs.microsoft.com/windows/uwp/design/basics/xaml-basics-adaptive-layout) veranschaulicht, wie das Galerielayout adaptiv gestaltet werden kann, damit es auf jedem Gerät und bei jeder Bildschirmgröße gut aussieht.

### <a name="get-started-tutorials"></a>Tutorials zu den ersten Schritten

Der Abschnitt „Erste Schritte” der UWP-Dokumente wurde mit einer [neuen Startseite für den Abschnitt „Tutorials“](https://docs.microsoft.com/windows/uwp/get-started/create-uwp-apps) aktualisiert. Dieser Abschnitt enthält eine neue und verbesserte Struktur für „Erste Schritte“, damit die Benutzer die passenden Tutorials leicht finden können, z. B. die oben erwähnten Tutorials zu den XAML-Grundlagen.

### <a name="voice-and-tone"></a>Sprache und Tonfall

Wir haben neue [Hinweise zur Sprache und dem Tonfall in UWP-Apps](https://docs.microsoft.com/windows/uwp/in-app-help/voice-and-tone) mit Ratschlägen zum Verfassen von Text in Ihrer App hinzugefügt. Unabhängig davon, was du genau erstellst, ist Folgendes wichtig: Die Sprache muss zugänglich, benutzerfreundlich und informativ sein.

## <a name="samples"></a>Beispiele

### <a name="photolab-sample"></a>PhotoLab-Beispiel

Das [PhotoLab-Beispiel](https://github.com/Microsoft/windows-appsample-photo-lab) umfasst eine einfache Fotogalerie und Benutzeroberfläche für die Fotobearbeitung.

![Screenshot: PhotoLab-Beispiel mit Seite für die Fotobearbeitung](images/PhotoLab-editing-page.png)  

### <a name="customer-orders"></a>Kundenbestellungen

Das Beispiel [Customer Orders Database](https://github.com/Microsoft/Windows-appsample-customers-orders-database) (Datenbank mit Kundenbestellungen) wurde aktualisiert, damit das neue .NET Core 2.0 und Entity Framework verwendet werden.