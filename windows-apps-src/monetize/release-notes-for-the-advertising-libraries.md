---
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Überprüfen Sie die Versionshinweise für die Microsoft Advertising-Bibliotheken.
title: Versionshinweise für die Advertising-Bibliotheken
ms.date: 08/23/2017
ms.topic: article
keywords: Windows10, UWP, Anzeigen, Werbung, Versionshinweise
ms.localizationpriority: medium
ms.openlocfilehash: 1bab822c81cdd5af1e6b00ca8d33ed7f7ea3838f
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "8203544"
---
# <a name="release-notes-for-the-advertising-libraries"></a>Versionshinweise für die Advertising-Bibliotheken




Dieser Abschnittenthält Versionshinweise für die aktuelle Version der Microsoft Advertising-Bibliotheken. Diese Bibliotheken unterstützt XAML- und JavaScript/HTML-apps für Windows 10, Windows8.1, Windows Phone 8.1 und WindowsPhone8.

## <a name="installation"></a>Installation


Diese Microsoft Advertising-Bibliotheken stehen als Teil von [Microsoft Advertising-SDK](http://aka.ms/ads-sdk-uwp) zur Verfügung. Weitere Informationen zur SDK-Installation finden Sie unter [Installieren von Microsoft Advertising-SDK](install-the-microsoft-advertising-libraries.md).

## <a name="uninstall-previous-versions"></a>Deinstallieren früherer Versionen

Vor der Installation der neuesten Microsoft Advertising-SDK wird dringend empfohlen, alle vorherigen Instanzen von SDK zu deinstallieren. Weitere Informationen finden Sie unter [Installieren von Microsoft Advertising-SDK](install-the-microsoft-advertising-libraries.md).

## <a name="target-architecture-specific-build-outputs"></a>Zielarchitekturspezifische Buildausgabe

Wenn Sie die Microsoft Advertising-Bibliotheken verwenden, können Sie in Ihrem Projekt nicht das Ziel **Any CPU** angeben. Sollte in Ihrem Projekt die Zielplattform **Any CPU** definiert sein, wird in Ihrem Projekt eine Warnung ausgegeben, sobald Sie einen Verweis auf die Microsoft Advertising-Bibliotheken hinzufügen. Um diese Warnung zu entfernen, müssen Sie eine architekturspezifische Buildausgabe verwenden (beispielsweise **x86**) und das Projekt entsprechend aktualisieren. Weitere Informationen finden Sie unter [Bekannte Probleme](known-issues-for-the-advertising-libraries.md).

## <a name="c-support"></a>C++-Unterstützung

Die Microsoft Advertising-Bibliotheken (in den die Klassen **AdControl** und **InterstitialAd** enthalten sind) unterstützen in C++ geschriebene Apps sowie DirectX mit Windows-Runtime-Interoperabilität (*interop*). Weitere Informationen und Beispiele zum Programmieren mit XAML und C++ finden Sie unter [Typsystem](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx).

## <a name="no-toolbox-control"></a>Kein Toolbox-Steuerelement

In der aktuellen Version der Microsoft Advertising-Bibliotheken im [Microsoft Advertising-SDK](http://aka.ms/ads-sdk-uwp) gibt es kein Toolbox-Steuerelement zum Ziehen eines **AdControl** oder **InterstitialAd** in die Entwurfsoberfläche Ihrer App. Informationen zum Hinzufügen dieser Steuerelemente in Ihrem Markup und Code finden Sie in der [Exemplarische Vorgehensweisen für Entwickler](developer-walkthroughs.md).

## <a name="latitude-and-longitude-properties-no-longer-available"></a>Eigenschaften Latitude und Longitude nicht mehr verfügbar

Die **AdControl**-Klasse verfügt nicht mehr über die Eigenschaften **Latitude** und **Longitude** für UWP-Apps. Stattdessen erkennt der Code im Anzeigensteuerelement diese Werte und sendet sie für die App an die Anzeigenserver.


 

 
