---
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Sehen Sie sich die Anmerkungen zu dieser Version der Microsoft-Werbe Bibliotheken an, die XAML-und JavaScript/HTML-Apps für Windows 10, Windows 8.1 Windows Phone 8,1 und Windows Phone 8 unterstützen.
title: Versions Hinweise für die Werbe Bibliotheken
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Werbung, Werbung, Anmerkungen zu dieser Version
ms.localizationpriority: medium
ms.openlocfilehash: 8faf352040b9d7bdc9fc8bc79804d5903573d41d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174964"
---
# <a name="release-notes-for-the-advertising-libraries"></a>Versions Hinweise für die Werbe Bibliotheken

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Dieser Abschnitt enthält Versions Hinweise für die aktuelle Version der Microsoft-Werbe Bibliotheken. Diese Bibliotheken unterstützt XAML- und JavaScript/HTML-Apps für Windows 10, Windows 8.1, Windows Phone 8.1 und Windows Phone 8.

## <a name="installation"></a>Installation


Die Microsoft-Werbe Bibliotheken sind als Teil des [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)verfügbar. Weitere Informationen zum Installieren des SDK finden Sie unter [Installieren des Microsoft Advertising SDK](install-the-microsoft-advertising-libraries.md).

## <a name="uninstall-previous-versions"></a>Deinstallieren früherer Versionen

Vor der Installation des neuesten Microsoft Advertising SDK wird dringend empfohlen, alle vorherigen Instanzen des SDK zu deinstallieren. Weitere Informationen finden Sie unter [Installieren des Microsoft Advertising SDK](install-the-microsoft-advertising-libraries.md).

## <a name="target-architecture-specific-build-outputs"></a>Zielarchitekturspezifische Buildausgabe

Wenn Sie die Microsoft Advertising-Bibliotheken verwenden, können Sie in Ihrem Projekt nicht das Ziel **Any CPU** angeben. Sollte in Ihrem Projekt die Zielplattform **Any CPU** definiert sein, wird in Ihrem Projekt eine Warnung ausgegeben, sobald Sie einen Verweis auf die Microsoft Advertising-Bibliotheken hinzufügen. Um diese Warnung zu entfernen, müssen Sie eine architekturspezifische Buildausgabe verwenden (beispielsweise **x86**) und das Projekt entsprechend aktualisieren. Weitere Informationen finden Sie unter [bekannte Probleme](known-issues-for-the-advertising-libraries.md).

## <a name="c-support"></a>C++-Unterstützung

Die Microsoft Advertising-Bibliotheken (in den die Klassen **AdControl** und **InterstitialAd** enthalten sind) unterstützen in C++ geschriebene Apps sowie DirectX mit Windows-Runtime-Interoperabilität (*interop*). Weitere Informationen und Beispiele zum Programmieren mit XAML und C++ finden Sie unter [Typsystem](/cpp/cppcx/type-system-c-cx).

## <a name="no-toolbox-control"></a>Kein Toolbox-Steuerelement

In der aktuellen Version der Microsoft-Werbe Bibliotheken im [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)gibt es kein Toolbox-Steuerelement zum Ziehen von **adcontrol** oder **interstitialad** auf eine Entwurfs Oberfläche in der app. Informationen zum Hinzufügen dieser Steuerelemente in Ihrem Markup und Code finden Sie in der [Exemplarische Vorgehensweisen für Entwickler](developer-walkthroughs.md).

## <a name="latitude-and-longitude-properties-no-longer-available"></a>Eigenschaften Latitude und Longitude nicht mehr verfügbar

Die **AdControl**-Klasse verfügt nicht mehr über die Eigenschaften **Latitude** und **Longitude** für UWP-Apps. Stattdessen erkennt der Code im Anzeigensteuerelement diese Werte und sendet sie für die App an die Anzeigenserver.


 

 