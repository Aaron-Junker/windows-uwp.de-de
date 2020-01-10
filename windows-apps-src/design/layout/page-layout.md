---
title: Seitenlayout für UWP-apps
description: Wenn Sie Ihre APP entwerfen, müssen Sie zuerst die Layoutstruktur in Erwägung gezogen. In diesem Artikel wird die allgemeine Struktur von grundlegenden Seitenlayouts behandelt, einschließlich der Benutzeroberflächen Elemente, die Sie benötigen, und des Orts, an dem Sie sich auf einer Seite befinden sollten. In UWP-apps enthält jede Seite in der Regel Navigations-, Befehls-und Inhaltselemente.
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP
localizationpriority: medium
ms.openlocfilehash: 7333cebc945715412e3ff1140ca26e1ed5368704
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684539"
---
# <a name="page-layout"></a>Seitenlayout

In UWP-apps enthält jede [**Seite**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) in der Regel Navigations-, Befehls-und Inhaltselemente. 

Ihre APP kann mehrere Seiten umfassen: Wenn ein Benutzer eine UWP-App aufruft, erstellt der Anwendungscode einen [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) , der innerhalb des [**Fensters**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window)der Anwendung platziert werden soll. Der Frame kann dann zwischen den [**Seiten**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) Instanzen der Anwendung [Navigieren](../basics/navigate-between-two-pages.md) . 

Die meisten Seiten folgen einer gemeinsamen Layoutstruktur, und in diesem Artikel wird erläutert, welche Benutzeroberflächen Elemente Sie benötigen und wo Sie sich auf einer Seite befinden sollten. 

![Seitenstruktur](images/page-components.svg)

## <a name="navigation"></a>Navigation
Ihr App-Layout beginnt mit dem von Ihnen gewählten Navigations Modell, das definiert, wie die Benutzer zwischen den Seiten in der APP navigieren. In diesem Artikel werden zwei allgemeine Navigationsmuster erläutert: Linker Navigationsbereich und oberster Navigationsbereich. Anleitungen zum Auswählen von anderen Navigationsoptionen finden Sie unter [Grundlagen des Navigations Entwurfs für UWP-apps](../basics/navigation-basics.md).

![obere und linke Navigationsmuster](images/top-left-nav.svg)

### <a name="left-nav"></a>Linker Navigationsbereich
Der linke Navigationsbereich oder [das Navigations](../controls-and-patterns/navigationview.md) Bereichs Muster ist im Allgemeinen für die Navigation auf App-Ebene reserviert und ist auf der höchsten Ebene innerhalb der app vorhanden. Dies bedeutet, dass es immer sichtbar und verfügbar sein sollte. Wir empfehlen den linken Navigationsbereich, wenn mehr als fünf Navigationselemente oder mehr als fünf Seiten in ihrer app vorhanden sind. Das Navigationsbereichs Muster enthält normalerweise Folgendes:
- Navigationselemente
- Einstiegspunkt in App-Einstellungen
- Einstiegspunkt in Kontoeinstellungen

Das [navigationview](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) -Steuerelement implementiert das linke Navigationsmuster für UWP.

Wenn ein Navigationselement ausgewählt ist, sollte der Frame zur Seite des ausgewählten Elements navigieren.

![Navigationsbereich erweitert](images/navview-expanded.svg)

Die Menü Schaltfläche ermöglicht es Benutzern, den Navigationsbereich zu erweitern und zu reduzieren. Wenn die Bildschirmgröße größer als 640 PX ist, wird durch Klicken auf die Menü Schaltfläche der Navigationsbereich auf einen Balken reduziert.

![Navigationsbereich Compact](images/navview-compact.svg)

Wenn die Bildschirmgröße kleiner als 640 PX ist, wird der Navigationsbereich vollständig reduziert.

![minimaler Navigationsbereich](images/navview-minimal.svg)

### <a name="top-nav"></a>Obere Navigationsleiste

Der obere Navigationsbereich kann auch als Navigation auf oberster Ebene fungieren. Während der linke Navigationsbereich redusible ist, ist der obere Navigationsbereich immer sichtbar. Das [navigationview](../controls-and-patterns/navigationview.md) -Steuerelement implementiert das oberste Navigations-und Registerkarten Muster für UWP.

![obere Navigation](images/pivot-large.svg)

## <a name="command-bar"></a>Befehlsleiste

Als nächstes können Sie Benutzern einen einfachen Zugriff auf die gängigsten Aufgaben Ihrer APP bereitstellen. Eine [Befehlsleiste](../controls-and-patterns/app-bars.md) kann Zugriff auf Befehle auf App-oder Seitenebene bereitstellen, und Sie kann mit jedem beliebigen Navigationsmuster verwendet werden.

![Befehls leisten Platzierung im oberen Bereich ](images/app-bar-desktop.svg)

Die Befehlsleiste kann oben oder unten auf der Seite platziert werden, je nachdem, welcher Wert für Ihre APP am besten geeignet ist.

![Platzierung der Befehlsleiste am unteren Rand](images/app-bar-mobile.svg)

## <a name="content"></a>Inhalt

Schließlich variiert der Inhalt von APP zu app stark, sodass Sie Inhalte auf viele verschiedene Arten präsentieren können. Hier werden einige gängige Seitenmuster beschrieben, die Sie möglicherweise in Ihrer APP verwenden möchten. Viele Apps verwenden einige oder alle dieser gängigen Seitenmuster, um verschiedene Arten von Inhalten anzuzeigen. Gleichermaßen können Sie diese Muster so kombinieren, dass Sie für Ihre APP optimiert werden.

## <a name="landing"></a>Angebotsseite (Landing-Page)

![Angebotsseite (Landing-Page)](images/hero-screen.svg)

Angebotsseiten werden auch Hero-Screens genannt und werden häufig auf der obersten Ebene einer App-Erfahrung angezeigt. Die große Oberfläche dient als Bühne, um Inhalte hervorzuheben, die Benutzer ansehen und nutzen möchten.

## <a name="collections"></a>Sammlungen

![Galerie](images/gridview.svg)

Über Sammlungen können Benutzer Gruppen von Inhalten oder Daten suchen. Die [Rasteransicht](../controls-and-patterns/item-templates-gridview.md) ist eine gute Option für Fotos oder medienzentrierte Inhalte, und die [Listenansicht](../controls-and-patterns/item-templates-listview.md) ist eine gute Option für textlastige Inhalte oder Daten.

## <a name="masterdetail"></a>Master/Detail

![Master Detail](images/master-detail.svg)

Das [Master/Detail](../controls-and-patterns/master-details.md)-Modell besteht aus einer Listenansicht (Master) und einer Inhaltsansicht (Detail). Beide Bereiche sind festgelegt und bieten vertikales Scrollen. Wenn ein Element in der Listenansicht ausgewählt ist, wird die Inhaltsansicht entsprechend aktualisiert. 

## <a name="forms"></a>Formulare
![Formular](images/form.svg)

Ein [Formular](../controls-and-patterns/forms.md) ist eine Gruppe von Steuerelementen, die Daten von Benutzern sammeln und übermitteln. Die meisten, wenn nicht alle Apps, verwenden eine Art Formular für Einstellungsseiten, Anmeldeportale, Feedback-Hubs, Kontoerstellung oder andere Zwecke. 

## <a name="sample-apps"></a>Beispiel-Apps
Wenn Sie sehen möchten, wie diese Muster implementiert werden können, sehen Sie sich unsere [UWP-Beispiel-apps](https://developer.microsoft.com/windows/samples)an:
- [Buildcast-Video Player](https://github.com/Microsoft/BuildCast)
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Kunden bestellen Datenbank](https://github.com/Microsoft/Windows-appsample-customers-orders-database)