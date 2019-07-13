---
title: 'Neuigkeiten in der Windows-Dokumentation im Dezember 2017: Entwicklung von UWP-Apps'
description: Neue Features, Videos und Entwicklerleitfäden in der Entwicklerdokumentation für Windows 10 im Dezember 2017
keywords: Neuigkeiten, Update, Features, Entwicklerleitfäden, Windows 10, Dezember
ms.date: 12/14/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c715a48e0f9d6dea5939e6363441b0300544c252
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321907"
---
# <a name="whats-new-in-the-windows-developer-docs-in-december-2017"></a>Neuigkeiten in der Windows-Entwicklerdokumentation im Dezember 2017

Die Windows-Entwicklerdokumentation wird ständig mit Informationen zu neuen Features aktualisiert, die Entwickler für die Windows-Plattform nutzen können. Die folgenden Featureübersichten, Entwicklerleitfäden und Beispiele wurden nach der Veröffentlichung des Fall Creators Update bereitgestellt und enthalten neue oder aktualisierte Informationen für Windows-Entwickler.

Nach der [Installation der Tools und des SDKs](https://go.microsoft.com/fwlink/?LinkId=821431) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/create-uwp-apps.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

## <a name="features"></a>Features

### <a name="windows-mixed-reality-enthusiasts-guide"></a>Windows Mixed Reality: Handbuch für Fans

Für Mixed Reality-Interessierte werden im Handbuch für Fans ([Enthusiast's Guide](https://docs.microsoft.com/en-us/windows/mixed-reality/enthusiast-guide/)) die wichtigsten Fragen zu Windows Mixed Reality beantwortet. 

Das Handbuch enthält Folgendes: 
- Häufig gestellte Fragen vor dem Kauf 
- Überprüfen der Kompatibilität des PC 
- Installationsanleitungen 
- Verwenden von Headset und Controllern 
- Informationen zum Herunterladen und Spielen von immersiven Games, 360-Videos, 2D-Apps, WebVR und SteamVR 
- Behandeln von Problemen (und vieles mehr)

![Windows Mixed Reality-Headset und -Motion-Controller](images/BeforeYouBegin-tile.jpg)

### <a name="keyboard-interactions"></a>Tastaturinteraktionen

Du kannst deine UWP-Apps so entwerfen und optimieren, dass sowohl eine barrierefreie Benutzeroberfläche als auch Features für erfahrene Benutzer mit aktualisierten [Tastaturinteraktionen](../design/input/keyboard-interactions.md) bereitgestellt werden. Wir haben unsere Empfehlungen und Richtlinien gemäß den neuen Verbesserungen für diese Interaktionen aktualisiert, die mit dem Fall Creators Update hinzugefügt wurden.

Weitere Informationen findest du unter [Zugriffstasten](../design/input/keyboard-accelerators.md) und im Artikel zu [benutzerdefinierten Tastaturinteraktionen](../design/input/custom-keyboard-interactions.md), um die Tastaturfunktionalität deiner Apps zu erweitern.

Füge auf Geräten, die Interaktionen per Toucheingabe unterstützen, die Tastaturfunktionen mithilfe der Artikel [Reagieren auf die Anzeige der Bildschirmtastatur](../design/input/respond-to-the-presence-of-the-touch-keyboard.md) und [Verwenden des Eingabeumfangs zum Ändern der Bildschirmtastatur](../design/input/use-input-scope-to-change-the-touch-keyboard.md) hinzu.

### <a name="microsoft-collaborate"></a>Microsoft Collaborate

Das „Microsoft Collaborate”-Portal enthält Tools und Dienste, um die Zusammenarbeit der Entwickler im Microsoft-Ökosystem durch die Freigabe von Engineering Systems-Arbeitsaufgaben (Fehler, Features usw.) und die Verteilung von Inhalten (Builds, Dokumente, Spezifikationen) zu optimieren. [Weitere Informationen](https://docs.microsoft.com/collaborate/).

![Microsoft Collaborate in Partner Center](images/microsoft_collaborate_screenshot.PNG)

### <a name="package-desktop-applications-with-uwp-projects"></a>Verpacken von Desktopanwendungen mit UWP-Projekten

In Visual Studio 2017, Version 15.5, wurde die Vorlage für das **Paketerstellungsprojekt für Windows-Anwendungen** aktualisiert, damit das Integrieren eines UWP-Projekts deutlich vereinfacht wird. Du musst nicht mehr ein JavaScript-basiertes Paketerstellungsprojekt verwenden und dann das Paketmanifest manuell optimieren.  

Weitere Hinweise zur Verwendung mit dieser neuen Vorlage zum Verpacken deiner Desktopanwendung findest du unter [Packen einer App mit Visual Studio](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net).

Weitere Hinweise dazu, wie du dein Paket einem UWP-Projekt hinzufügst, findest du im Artikel zum [Erweitern deiner Desktopanwendung mit modernen UWP-Komponenten](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend).

### <a name="subscription-add-ons-are-now-available-to-developers-in-the-windows-dev-center-insider-program"></a>Abonnement-Add-Ons sind jetzt für Entwickler im Windows Dev Center-Insider-Programm verfügbar

Alle Entwickler, die dem Dev Center-Insider-Programm beigetreten sind, können nun Abonnement-Add-Ons verwenden (z. B. App-Features oder digitale Inhalte), um in ihren Apps digitale Produkte mit einer automatisierten wiederkehrenden Abrechnung zu verkaufen. Weitere Details findest du unter [Aktivieren von Abonnement-Add-Ons für die App](../monetize/enable-subscription-add-ons-for-your-app.md).

## <a name="developer-guidance"></a>Entwicklerleitfäden

### <a name="color"></a>Farbe

Wir haben einige neue Richtlinien zur Verwendung von Farben Apps hinzugefügt, um für die bestmögliche Benutzerumgebung zu sorgen. Hierzu zählen API-Verwendungsszenarien sowie allgemeine Hinweise zum Design und zur Barrierefreiheit der Benutzeroberfläche. Wir haben auch die Liste mit den Benutzer-Akzentfarben aktualisiert, die für Xbox verfügbar sind. [Sieh dir hier den aktualisierten Artikel zu Farben an.](../design/style/color.md)

![Universelle Windows-Farbpalette](../design/basics/images/colors.png)

### <a name="data-access-guides"></a>Leitfäden zum Datenzugriff

Wir haben einen [SQL Server-Leitfaden](../data-access/sql-server-databases.md) hinzugefügt, um zu veranschaulichen, wie deine App direkt auf eine SQL Server-Datenbank zugreifen kann. Es ist keine Dienstebene erforderlich.

Darüber hinaus haben wir unseren [SQLite-Leitfaden](../data-access/sqlite-databases.md) vollständig umgestaltet und mit einem übersichtlicheren Erscheinungsbild versehen und unsere neuesten bewährten Methoden zum Speichern und Abrufen von Daten in einer einfachen Datenbank auf dem Gerät des Benutzers hinzugefügt.

### <a name="forms"></a>Formulare

Wir haben einen neuen Artikel über das [Erstellen von Formularen in Apps](../design/controls-and-patterns/forms.md) hinzugefügt, um Daten von Benutzern zu sammeln und zu übermitteln. Hierzu gehören bestimmte Informationen zur Implementierung von Formularen und allgemeine Hinweise zu deren Verwendung.

### <a name="intro-to-app-design"></a>Einführung in das App-Design

Der Designleitfaden für die universelle Windows-Plattform (UWP) ist eine Ressource, die Sie beim Entwerfen und Erstellen ansprechender und optimierter Apps unterstützen kann. Unsere [neue Einführung](../design/basics/design-and-ui-intro.md) enthält eine Übersicht über die universellen Entwurfsfunktionen, die in jeder UWP-App enthalten sind, und Informationen zur Verwendung der Dokumente zum Erstellen von Benutzeroberflächen (UIs), die für unterschiedliche Geräte problemlos skaliert werden können.


### <a name="request-ratings-and-reviews"></a>Anfordern von Bewertungen und Prüfungen

Wir haben einen neuen Artikel hinzugefügt, in dem veranschaulicht wird, wie du [Bewertungen und Prüfungen für deine App anfordern](../monetize/request-ratings-and-reviews.md) kannst. Du kannst Bewertungen und Prüfungen im Kontext deiner App anzeigen oder die Seite „Bewertung und Prüfung” für deine App im Store öffnen.

## <a name="samples"></a>Beispiele

### <a name="customer-orders"></a>Kundenbestellungen

Das Beispiel [Customer Orders Database](https://github.com/Microsoft/Windows-appsample-customers-orders-database) (Datenbank für Kundenbestellungen) wurde aktualisiert, damit bewährte Methoden im Hinblick auf den Datenzugriff veranschaulicht werden, z. B. Verwendung des Repositorymusters und Herstellen einer Verbindung mit mehreren Datenquellen (z. B. Sqlite, SQL Azure und REST-Dienst).

## <a name="videos"></a>Videos

### <a name="package-a-net-app-in-visual-studio"></a>Verpacken einer .NET-App in Visual Studio

Es ist einfacher denn je, eine Desktop-App auf die Universelle Windows-Plattform zu übertragen. [Sieh dir das Video an](https://www.youtube.com/watch?v=fJkbYPyd08w), um zu erfahren, wie du deine .NET-App für die Verteilung verpacken kannst. Navigiere anschließend auf [diese Seite](../porting/desktop-to-uwp-packaging-dot-net.md), um weitere Informationen zu erhalten.