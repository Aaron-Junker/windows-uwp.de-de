---
title: Seitenlayout für UWP-Apps
description: Beim Entwurf deiner App musst du zuerst die Layoutstruktur berücksichtigen. In diesem Artikel wird die allgemeine Struktur von grundlegenden Seitenlayouts behandelt, einschließlich der erforderlichen Benutzeroberflächenelemente und der Positionen, an denen diese sich auf einer Seite befinden sollten. In UWP-Apps weist jede Seite in der Regel Navigations-, Befehls- und Inhaltselemente auf.
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP
localizationpriority: medium
ms.openlocfilehash: 7333cebc945715412e3ff1140ca26e1ed5368704
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684539"
---
# <a name="page-layout"></a>Seitenlayout

In UWP-Apps weist jede [**Seite**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) in der Regel Navigations-, Befehls- und Inhaltselemente auf. 

Deine App kann mehrere Seiten umfassen: Wenn ein Benutzer eine UWP-App startet, erstellt der Anwendungscode einen [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame), der im [**Fenster**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) der Anwendung platziert wird. Der Frame kann dann zwischen den [**Seiteninstanzen**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) der Anwendung [navigieren](../basics/navigate-between-two-pages.md). 

Die meisten Seiten folgen einer allgemeinen Layoutstruktur, und in diesem Artikel werden die erforderlichen Benutzeroberflächenelemente und deren Positionen auf einer Seite behandelt. 

![Seitenstruktur](images/page-components.svg)

## <a name="navigation"></a>Navigation
Dein App-Layout beginnt mit dem ausgewählten Navigationsmodell. Dieses definiert, wie die Benutzer in der App zwischen Seiten navigieren. In diesem Artikel werden zwei gebräuchliche Navigationsmuster erläutert: der linke Navigationsbereich und der obere Navigationsbereich. Anleitungen zum Auswählen anderer Navigationsoptionen findest du unter [Navigationsdesigngrundlagen für UWP-Apps](../basics/navigation-basics.md).

![Navigationsmuster oben und links](images/top-left-nav.svg)

### <a name="left-nav"></a>Linker Navigationsbereich
Der linke Navigationsbereich oder das Muster [Navigationsbereich](../controls-and-patterns/navigationview.md) ist im Allgemeinen für die Navigation auf App-Ebene reserviert und liegt auf der obersten Ebene innerhalb der App, sodass er immer sichtbar und verfügbar sein sollte. Wir empfehlen den linken Navigationsbereich, wenn deine App mehr als fünf Navigationselemente oder mehr als fünf Seiten enthält. Das Muster „Navigationsbereich“ enthält normalerweise Folgendes:
- Navigationselemente
- Einstiegspunkt in App-Einstellungen
- Einstiegspunkt in Kontoeinstellungen

Das Steuerelement [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview) implementiert das Muster des linken Navigationsbereichs für UWP.

Wenn ein Navigationselement ausgewählt ist, sollte der Frame zur Seite des ausgewählten Elements navigieren.

![Navigationsbereich erweitert](images/navview-expanded.svg)

Die Schaltfläche „Menü“ ermöglicht Benutzern, den Navigationsbereich zu erweitern und zu reduzieren. Wenn die Bildschirmgröße 640 px übersteigt, wird durch Klicken auf die Schaltfläche „Menü“ der Navigationsbereich auf eine Leiste reduziert.

![Navigationsbereich kompakt](images/navview-compact.svg)

Wenn die Bildschirmgröße kleiner als 640 px ist, wird der Navigationsbereich vollständig reduziert.

![Navigationsbereich minimiert](images/navview-minimal.svg)

### <a name="top-nav"></a>Oberer Navigationsbereich

Der obere Navigationsbereich kann ebenfalls als Navigation auf oberster Ebene dienen. Während der linke Navigationsbereich reduziert werden kann, ist der obere Navigationsbereich immer sichtbar. Das Steuerelement [NavigationView](../controls-and-patterns/navigationview.md) implementiert die Muster des oberen Navigationsbereichs und der Registerkarten für UWP.

![Oberer Navigationsbereich](images/pivot-large.svg)

## <a name="command-bar"></a>Befehlsleiste

Als Nächstes solltest du Benutzern einen einfachen Zugriff auf die am häufigsten verwendete Aufgaben in deiner App ermöglichen. Eine [Befehlsleiste](../controls-and-patterns/app-bars.md) ermöglicht den Zugriff auf Befehle auf App-Ebene oder Seitenebene und kann bei jedem Navigationsmuster verwendet werden.

![Platzierung der Befehlsleiste im oberen Bereich ](images/app-bar-desktop.svg)

Die Befehlsleiste kann oben oder unten auf der Seite platziert werden, je nachdem, welche Position für deine App am besten geeignet ist.

![Platzierung der Befehlsleiste im unteren Bereich](images/app-bar-mobile.svg)

## <a name="content"></a>Inhalt

Außerdem variiert der Inhalt von App zu App stark, sodass du Inhalte auf viele verschiedene Arten präsentieren kannst. Hier werden einige gängige Seitenmuster beschrieben, die du für deine App in Erwägung ziehen kannst. Viele Apps verwenden einige oder alle dieser gängigen Seitenmuster, um verschiedene Arten von Inhalten anzuzeigen. Ebenso kannst du diese Muster auch kombinieren, um sie für deine App zu optimieren.

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
Ein Beispiel für die Implementierung dieser Muster findest du in unseren [UWP-Beispiel-Apps](https://developer.microsoft.com/windows/samples):
- [Videoplayer BuildCast](https://github.com/Microsoft/BuildCast)
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
- [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Kundenauftragsdatenbank](https://github.com/Microsoft/Windows-appsample-customers-orders-database)