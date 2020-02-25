---
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Überprüfen Sie die Versionshinweise für die Microsoft Advertising-Bibliotheken.
title: Versionshinweise für die Advertising-Bibliotheken
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Anzeigen, Werbung, Versionshinweise
ms.localizationpriority: medium
ms.openlocfilehash: 377069522c6b31a55028bf35bf9c71ac50c90608
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77506844"
---
# <a name="release-notes-for-the-advertising-libraries"></a>Versionshinweise für die Advertising-Bibliotheken

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Dieser Abschnitt enthält Versionshinweise für die aktuelle Version der Microsoft Advertising-Bibliotheken. Diese Bibliotheken unterstützen XAML-und JavaScript/HTML-Apps für Windows 10, Windows 8.1 Windows Phone 8,1 und Windows Phone 8.

## <a name="installation"></a>Installation


Diese Microsoft Advertising-Bibliotheken stehen als Teil von [Microsoft Advertising-SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) zur Verfügung. Weitere Informationen zur SDK-Installation finden Sie unter [Installieren von Microsoft Advertising-SDK](install-the-microsoft-advertising-libraries.md).

## <a name="uninstall-previous-versions"></a>Deinstallieren früherer Versionen

Vor der Installation der neuesten Microsoft Advertising-SDK wird dringend empfohlen, alle vorherigen Instanzen von SDK zu deinstallieren. Weitere Informationen finden Sie unter [Installieren von Microsoft Advertising-SDK](install-the-microsoft-advertising-libraries.md).

## <a name="target-architecture-specific-build-outputs"></a>Zielarchitekturspezifische Buildausgabe

Wenn Sie die Microsoft Advertising-Bibliotheken verwenden, können Sie in Ihrem Projekt nicht das Ziel **Any CPU** angeben. Sollte in Ihrem Projekt die Zielplattform **Any CPU** definiert sein, wird in Ihrem Projekt eine Warnung ausgegeben, sobald Sie einen Verweis auf die Microsoft Advertising-Bibliotheken hinzufügen. Um diese Warnung zu entfernen, müssen Sie eine architekturspezifische Buildausgabe verwenden (beispielsweise **x86**) und das Projekt entsprechend aktualisieren. Weitere Informationen finden Sie unter [Bekannte Probleme](known-issues-for-the-advertising-libraries.md).

## <a name="c-support"></a>C++-Unterstützung

Die Microsoft Advertising-Bibliotheken (in den die Klassen **AdControl** und **InterstitialAd** enthalten sind) unterstützen in C++ geschriebene Apps sowie DirectX mit Windows-Runtime-Interoperabilität (*interop*). Weitere Informationen und Beispiele zum Programmieren mit XAML und C++ finden Sie unter [Typsystem](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx).

## <a name="no-toolbox-control"></a>Kein Toolbox-Steuerelement

In der aktuellen Version der Microsoft Advertising-Bibliotheken im [Microsoft Advertising-SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) gibt es kein Toolbox-Steuerelement zum Ziehen eines **AdControl** oder **InterstitialAd** in die Entwurfsoberfläche Ihrer App. Informationen zum Hinzufügen dieser Steuerelemente in Ihrem Markup und Code finden Sie in der [Exemplarische Vorgehensweisen für Entwickler](developer-walkthroughs.md).

## <a name="latitude-and-longitude-properties-no-longer-available"></a>Eigenschaften Latitude und Longitude nicht mehr verfügbar

Die **AdControl**-Klasse verfügt nicht mehr über die Eigenschaften **Latitude** und **Longitude** für UWP-Apps. Stattdessen erkennt der Code im Anzeigensteuerelement diese Werte und sendet sie für die App an die Anzeigenserver.


 

 
