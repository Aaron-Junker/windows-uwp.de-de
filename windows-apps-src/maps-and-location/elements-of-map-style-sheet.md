---
description: Die Einträge und Eigenschaften eines Karten Stylesheets.
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Karten-Stylesheet-Referenz
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, UWP, Maps, Karten Stylesheet
ms.localizationpriority: medium
ms.openlocfilehash: ec30fd5d63dfa522ef721d2d8a2e4950e6e8e854
ms.sourcegitcommit: c9edb164356c0843d8a9b19ab43707d486e392e1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2020
ms.locfileid: "85377931"
---
# <a name="map-style-sheet-reference"></a>Karten-Stylesheet-Referenz

Microsoft-Zuordnungs Technologien verwenden [Karten Stylesheets](https://docs.microsoft.com/BingMaps/styling/map-style-sheets) zum Definieren der Darstellung von Zuordnungen, einschließlich derjenigen, die in einem [mapcontrol](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)-Element einer Windows Store-Anwendung angezeigt werden.  Ein Karten Stylesheet wird mithilfe von JavaScript Object Notation (JSON) über die [mapstylesheet. Parser-fromjson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) -Methode definiert.

Ein Stylesheet besteht aus [Einträgen](https://docs.microsoft.com/BingMaps/styling/map-style-sheet-entries) , deren Eigenschaften überschrieben werden, um die endgültige Darstellung der Karte zu ändern.

Beispielsweise kann der folgende JSON-Code verwendet werden, um Wasser Bereiche in rot anzuzeigen, Wasser Bezeichnungen werden grün angezeigt, und Länder Bereiche werden in blau angezeigt:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

Stylesheets können interaktiv mithilfe der Editor-Anwendung des [Karten Stylesheets](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) erstellt werden.
