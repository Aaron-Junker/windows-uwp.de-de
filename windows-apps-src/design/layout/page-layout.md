---
title: Seitenlayout für UWP-apps
description: Beim Entwerfen Ihrer app ist zunächst sollten die Layoutstruktur. Dieser Artikel behandelt die allgemeine Struktur der grundlegenden Seitenlayouts, einschließlich der Elemente der Benutzeroberfläche müssen Sie ein, und sollten, in dem auf einer Seite beginnen. In UWP-apps hat jede Seite in der Regel Navigation, Befehl und Content-Elemente.
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, UWP
localizationpriority: medium
ms.openlocfilehash: edda9948e70b1757ddb46fae45a579bb2fdb8de1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633715"
---
# <a name="page-layout"></a>Seitenlayout

In UWP-apps jede [ **Seite** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) hat im Allgemeinen die Navigation, Befehl und Content-Elemente. 

Ihre app kann über mehrere Seiten verfügen: Wenn ein Benutzer eine UWP-app gestartet wird, erstellt der Anwendungscode eine [ **Frame** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) innerhalb der Anwendung platzieren [ **Fenster** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window). Dann können Sie der Frame [navigieren](../basics/navigate-between-two-pages.md) zwischen der Anwendung [ **Seite** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) Instanzen. 

Die meisten Seiten führen Sie eine gemeinsame Layoutstruktur und diesem Artikel wird beschrieben, welche Benutzeroberfläche Elemente müssen, und sollten, in dem auf einer Seite beginnen. 

![Seitenstruktur](images/page-components.svg)

## <a name="navigation"></a>Navigation
Das Layout der app beginnt mit das Navigationsmodell an, das definiert, wie Ihre Benutzer zwischen den Seiten in Ihrer app navigieren. In diesem Artikel beschäftigen wir uns mit zwei allgemeine navigationsmuster: links, Nav "und" Top NAV. Anleitungen zur Auswahl der übrigen Navigationsoptionen finden Sie unter [Grundlagen des Berichtsentwurfs Navigation für UWP-apps](../basics/navigation-basics.md).

![oberen und linken navigationsmuster](images/top-left-nav.svg)

### <a name="left-nav"></a>Linke Navigationsleiste
Linker Navigationsbereich, oder die [Navigationsbereich](../controls-and-patterns/navigationview.md) Muster ist in der Regel reserviert für die Navigation von app-Ebene und vorhanden ist, auf der obersten Ebene innerhalb der app, was bedeutet, dass es immer sichtbar und verfügbar sein sollte. Linke Navigationsleiste wird empfohlen, wenn mehr als fünf Navigations-Elemente oder mehr als fünf Seiten in der app vorhanden sind. Das Nav Bereich Muster enthält in der Regel:
- Navigationselementen
- Einstiegspunkt in den app-Einstellungen
- Einstiegspunkt in den kontoeinstellungen

Die [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) Steuerelement implementiert das Muster für die linke Navigationsleiste für UWP.

Wenn ein Element ausgewählt ist, sollte der Frame zu des ausgewählten Elements navigieren.

![Navigationsbereich erweitert](images/navview-expanded.svg)

Die Schaltfläche ermöglicht zum Erweitern und reduzieren den Navigationsbereich. Wenn die Größe des Bildschirms ist größer als 640 px, klicken Sie auf die Menüschaltfläche reduziert den Navigationsbereich in einem Balken.

![Navigationsbereich compact](images/navview-compact.svg)

Wenn die Größe des Bildschirms ist kleiner als 640 Pixel, den Navigationsbereich vollständig reduziert ist.

![Minimale Navigationsbereich](images/navview-minimal.svg)

### <a name="top-nav"></a>Top nav

Top Nav kann auch als Navigation auf oberster Ebene fungieren. Linke Navigationsleiste reduzierbare ist, zwar ist Top Nav immer sichtbar. Die [NavigationView](../controls-and-patterns/navigationview.md) Steuerelement implementiert die obere Muster für die Navigation und Registerkarten für UWP.

![oben im Navigationsbereich](images/pivot-large.svg)

## <a name="command-bar"></a>Befehlsleiste

Als Nächstes empfiehlt es sich, Benutzern einfachen Zugriff auf Ihre app am häufigsten ausgeführten Aufgaben bereitzustellen. Ein [Befehlsleiste](../controls-and-patterns/app-bars.md) bieten Zugriff auf app-Ebene oder auf Seitenebene Befehle aus, und kann mit jedem-navigationsmuster verwendet werden.

![Befehl Balken Platzierung oben ](images/app-bar-desktop.svg)

Die Befehlsleiste platziert werden kann, oben oder unteren Rand der Seite, je nachdem, was für Ihre app am besten geeignet ist.

![Platzierung von Befehlsleiste am unteren](images/app-bar-mobile.svg)

## <a name="content"></a>Inhalt

Und schließlich variieren Inhalt weit von app zu app, damit Sie Inhalt auf viele verschiedene Arten darstellen können. Nachfolgend wird erläutert, einige allgemeine Muster für die Seite, die Sie in Ihrer app verwenden möchten. Viele Apps verwenden einige oder alle dieser gängigen Seitenmuster, um verschiedene Arten von Inhalten anzuzeigen. Ebenso können Sie mischen und Zuordnen dieser Muster, um für Ihre app zu optimieren.

## <a name="landing"></a>Angebotsseite (Landing-Page)

![Angebotsseite (Landing-Page)](images/hero-screen.svg)

Angebotsseiten werden auch Hero-Screens genannt und werden häufig auf der obersten Ebene einer App-Erfahrung angezeigt. Die große Oberfläche dient als Bühne, um Inhalte hervorzuheben, die der Nutzer ansehen und nutzen möchte.

## <a name="collections"></a>Sammlungen

![Galerie](images/gridview.svg)

Über Sammlungen können Benutzer Gruppen von Inhalten oder Daten suchen. Die [Rasteransicht](../controls-and-patterns/item-templates-gridview.md) ist eine gute Option für Fotos oder medienzentrierte Inhalte, und die [Listenansicht](../controls-and-patterns/item-templates-listview.md) ist eine gute Option für textlastige Inhalte oder Daten.

## <a name="masterdetail"></a>Master/Detail

![master details](images/master-detail.svg)

Das [Master/Detail](../controls-and-patterns/master-details.md)-Modell besteht aus einer Listenansicht (Master) und einer Inhaltsansicht (Detail). Beide Bereiche sind festgelegt und bieten vertikales Scrollen. Wenn ein Element in der Listenansicht ausgewählt ist, wird die Inhaltsansicht entsprechend aktualisiert. 

## <a name="forms"></a>Formulare
![form](images/form.svg)

Ein [Formular](../controls-and-patterns/forms.md) ist eine Gruppe von Steuerelementen, die Daten von Benutzern sammeln und übermitteln. Die meisten, wenn nicht alle Apps, verwenden eine Art Formular für Einstellungsseiten, Anmeldeportale, Feedback-Hubs, Kontoerstellung oder andere Zwecke. 

## <a name="sample-apps"></a>Beispiel-Apps
Um anzuzeigen, wie diese Muster implementiert werden können, sehen Sie sich unsere [UWP-Beispiel-apps](https://developer.microsoft.com/en-us/windows/samples):
- [BuildCast-Video-Player](https://github.com/Microsoft/BuildCast)
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Kunden Auftragsdatenbank](https://github.com/Microsoft/Windows-appsample-customers-orders-database)