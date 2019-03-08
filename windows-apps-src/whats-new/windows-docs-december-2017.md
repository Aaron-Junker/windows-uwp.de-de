---
title: Neuigkeiten in der Windows-Dokumentation im Dezember 2017 – Entwicklung von UWP-Apps
description: Neue Features, Videos und Entwicklerleitfäden in der Entwicklerdokumentation für Windows 10 im Dezember 2017
keywords: Neuigkeiten, Update, Features, Anleitungen für Entwickler, Windows 10, Dezember
ms.date: 12/14/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a79edb39cc4bfa5794d79d68f4fb8d5b0e976105
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645315"
---
# <a name="whats-new-in-the-windows-developer-docs-in-december-2017"></a>Neuigkeiten in der Windows-Entwicklerdokumentation im Dezember 2017

Die Entwicklerdokumentation für die Windows-Plattform wird ständig mit Informationen über neue Features für Entwickler aktualisiert. Die folgenden Featureübersichten, Entwicklerleitfäden und Beispiele wurden nach der Veröffentlichung des Fall Creators Update bereitgestellt und enthalten neue oder aktualisierte Informationen für Windows-Entwickler.

Nach der [Installation der Tools und des SDKs](https://go.microsoft.com/fwlink/?LinkId=821431) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/create-uwp-apps.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

## <a name="features"></a>Features

### <a name="windows-mixed-reality-enthusiasts-guide"></a>Windows Mixed Reality: Enthusiast-Handbuch

Für Mixed Reality Tech-Fans beantwortet das [Handbuch für Fans](https://docs.microsoft.com/en-us/windows/mixed-reality/enthusiast-guide/) die wichtigsten Fragen über Windows Mixed Reality. 

In der Anleitung finden Sie: 
- Vor dem Kauf häufig gestellte Fragen 
- Überprüfen der Kompatibilität Ihres PCs, 
- Installationsanweisungen, 
- Wie Sie Ihre Kopfhörer und Controller verwenden, 
- Informationen zum Herunterladen und der Wiedergabe von immersiven Spielen, 360-Videos, 2D-Apps, WebVR und SteamVR, 
- Behandeln von Problemen und vieles mehr.

![Benutzer von Windows Mixed Reality-Headsets und Bewegungscontroller](images/BeforeYouBegin-tile.jpg)

### <a name="keyboard-interactions"></a>Tastaturinteraktionen

Entwerfen und Optimieren Sie Ihre UWP-Apps zum Bereitstellen einer barrierefreien Erfahrung und Features für erfahrene Benutzer mit aktualisierten [Tastaturinteraktionen](../design/input/keyboard-interactions.md). Wir haben unsere Empfehlungen und Richtlinien entsprechend der neuen Verbesserungen für diese Interaktionen aktualisiert, die im Fall Creators Update hinzugefügt wurden.

Weitere Informationen finden Sie unter [Zugriffstasten](../design/input/keyboard-accelerators.md) und [benutzerdefinierte Tastaturinteraktionen](../design/input/custom-keyboard-interactions.md), um die Tastaturfunktionalität Ihrer Apps zu erweitern.

Fügen Sie auf Geräten, die Interaktionen per Fingereingabe unterstützen, die Tastaturfunktionen mithilfe der Artikel [Reagieren auf die Anzeige der Bildschirmtastatur](../design/input/respond-to-the-presence-of-the-touch-keyboard.md) und [Verwenden des Eingabeumfangs zum Ändern der Bildschirmtastatur](../design/input/use-input-scope-to-change-the-touch-keyboard.md) hinzu.

### <a name="microsoft-collaborate"></a>Microsoft Collaborate

Das „Microsoft Collaborate”-Portal bietet Tools und Dienste, um die Zusammenarbeit der Entwickler im Microsoft-Ökosystem durch die Freigabe von Engineering Systems-Arbeitsaufgaben (Fehler, Features usw.) und die Verteilung von Inhalten (Builds, Dokumente, Spezifikationen). [Weitere Informationen](https://docs.microsoft.com/en-us/collaborate).

![Microsoft zusammenarbeiten, im Partner Center](images/microsoft_collaborate_screenshot.PNG)

### <a name="package-desktop-applications-with-uwp-projects"></a>Paket-Desktopanwendungen mit UWP-Projekten

In Visual Studio 2017 Version 15.5 wurde die Vorlage für das **Windows Application Packaging Project** aktualisiert, damit es sehr viel einfacher in ein UWP-Projekt integriert werden kann. Sie müssen nicht mehr ein JavaScript-basiertes Paketprojekt verwenden, und dann das Paketmanifest manuell optimieren.  

Weitere Hinweise zur Verwendung mit der neuen Vorlage zum Verpacken Ihrer Desktopanwendung finden Sie unter [Packen einer App mit Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-packaging-dot-net).

Weitere Hinweise dazu, wie Sie Ihr Paket einem UWP-Projekt hinzufügen finden Sie unter [Erweitern Ihrer Desktopanwendung mit modernen UWP-Komponenten](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend) .

### <a name="subscription-add-ons-are-now-available-to-developers-in-the-windows-dev-center-insider-program"></a>Abonnement-Add-Ons sind jetzt für Entwickler im Windows Dev Center-Insider-Programm verfügbar

Alle Entwickler, die das Dev Center-Insider-Programm beigetreten sind jetzt können Abonnement-Add-Ons zum Verkauf von digitaler Produkten in der apps-app (z.B. app-Funktionen oder digitale Inhalte) mit automatisierten wiederkehrenden Abrechnungszeiträume. Weitere Details finden Sie unter [Abonnement-Add-Ons für Ihre App aktivieren](../monetize/enable-subscription-add-ons-for-your-app.md).

## <a name="developer-guidance"></a>Erläuterungen für Entwickler

### <a name="color"></a>Farbe

Wir haben einige neue Richtlinien zur Verwendung von Farben für die bestmögliche Benutzerumgebung in Ihren Apps hinzugefügt. Dazu zählen API-Verwendungsszenarien sowie allgemeine Hinweise über UI-Design und Barrierefreiheit. Wir haben auch die Liste der Benutzer-Akzentfarben auf Xbox aktualisiert. [Sehen Sie sich die aktualisierte Farbe Artikel.](../design/style/color.md)

![Universelle Windows-Farbpalette](../design/basics/images/colors.png)

### <a name="data-access-guides"></a>Handbücher über den Datenzugriff

Wir haben ein [SQL Server-Handbuch](../data-access/sql-server-databases.md) hinzugefügt, um zu zeigen, wie Ihre App direkt auf eine SQL Server-Datenbank zugreifen kann. Es ist keine Dienstebene erforderlich.

Darüber hinaus haben wir unserer [SQLite-Handbuch](../data-access/sqlite-databases.md) mit einem übersichtlicheren Erscheinungsbild vollständig umgestaltet, um unsere neuesten bewährten Methoden zum Speichern und Abrufen von Daten in einer Lightweight-Datenbank auf dem Gerät des Benutzers zu enthalten.

### <a name="forms"></a>Formulare

Wir haben einen neuen Artikel über [das Erstellen von Formularen in Ihren Apps](../design/controls-and-patterns/forms.md) hinzugefügt, um Daten von Benutzern zu sammeln und zu übermitteln. Dazu gehören bestimmte Informationen zur Implementierung von Formularen und allgemeine Informationen wann und wo sie verwendet werden.

### <a name="intro-to-app-design"></a>Einführung in das Design von Apps

Der Designleitfaden für die Universelle Windows-Plattform (UWP) ist eine Ressource, die Sie beim Entwerfen und Erstellen ansprechender und optimierter Apps unterstützen kann. [Unsere neue Einführung](../design/basics/design-and-ui-intro.md) enthält eine Übersicht über das universelle Design und die enthaltenen Features in jeder UWP-App, und wie Sie die Dokumente zum Erstellen von Benutzeroberflächen (UI) verwenden können, die außergewöhnliche Bilder auf einer Vielzahl von Geräten erstellen.


### <a name="request-ratings-and-reviews"></a>Anfordern von Bewertungen und Prüfungen

Wir haben einen neuen Artikel hinzugefügt, der zeigt, wie Sie [Bewertungen und Prüfungen für Ihre App anfordern können](../monetize/request-ratings-and-reviews.md). Sie können Bewertungen und Prüfungen im Kontext Ihrer App anzeigen, oder die Seite „Bewertung und Prüfung” für Ihre App im Store öffnen.

## <a name="samples"></a>Beispiele

### <a name="customer-orders"></a>Kundenaufträge

Das Beispiel [Kundendatenbank für Aufträge](https://github.com/Microsoft/Windows-appsample-customers-orders-database) wurde aktualisiert, um bewährte Methoden in Hinblick auf den Datenzugriff anzuzeigen, z. B. mithilfe des Repository-Musters und dem Herstellen einer Verbindung mit mehreren Datenquellen (einschließlich Sqlite, SQL Azure und REST-Dienst).

## <a name="videos"></a>Videos

### <a name="package-a-net-app-in-visual-studio"></a>Packen einer .NET-App in Visual Studio

Es ist einfacher denn je, eine Desktop-App auf die Universelle Windows-Plattform zu übertragen. [Sehen Sie sich das Video](https://www.youtube.com/watch?v=fJkbYPyd08w) an und erfahren Sie, wie Sie Ihre .NET-App für die Verteilung verpacken und wechseln Sie auf [diese Seite](../porting/desktop-to-uwp-packaging-dot-net.md) für weitere Informationen.