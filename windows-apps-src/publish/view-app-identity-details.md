---
Description: Anzeigen von Details im Zusammenhang mit der eindeutigen Identität, die ihrer app durch das Microsoft Store zugewiesen ist, und erhalten eines Links zur Store-Auflistung Ihrer APP.
title: Anzeigen von Details zur App-Identität
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 07c2d3308d204d37e246a9a56c0a7203a1340dc0
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853257"
---
# <a name="view-app-identity-details"></a>Anzeigen von Details zur App-Identität


Sie können Details im Zusammenhang mit der eindeutigen Identität, die Ihrer APP zugewiesen ist, anzeigen, indem Sie die Microsoft Store auf den zugehörigen **App-Identitäts** Seiten Sie können auch einen Link zur Store-Auflistung Ihrer APP auf dieser Seite erhalten.

Um diese Informationen zu suchen, navigieren Sie zu einer Ihrer Apps und erweitern im linken Navigationsmenü **App-Verwaltung**. Wählen Sie **App-Identität** aus, um diese Details anzuzeigen.


## <a name="values-to-include-in-your-app-package-manifest"></a>In das App-Paketmanifest einzuschließende Werte

Die folgenden Werte müssen im Paket Manifest enthalten sein. Wenn Sie Ihre [Pakete mit Microsoft Visual Studio erstellen](/windows/msix/package/packaging-uwp-apps) und mit demselben Microsoft-Konto angemeldet sind, das Sie mit Ihrem Entwicklerkonto verknüpft haben, werden diese Details automatisch eingefügt. Wenn Sie Ihr Paket manuell erstellen, müssen Sie folgende Details selbst hinzufügen:

-   **Package/Identity/Name**
-   **Paket/Identität/Verleger**
-   **Package/Properties/publisherdisplayname**

Weitere Informationen finden Sie unter [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) in der [Paketmanifestschema-Referenz](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Zusammen werden durch diese Elemente die Identität Ihrer App deklariert und die „Paketfamilie“ gebildet, der alle zugehörigen Pakete angehören. Einzelne Pakete verfügen über zusätzliche Details, z. B. die Architektur und Version.


## <a name="additional-values-for-package-family"></a>Zusätzliche Werte für die Paketfamilie

Die folgenden zusätzlichen Werte beziehen sich auf die Paketfamilie der App, werden aber nicht in Ihr Manifest eingeschlossen.

-   **Paketfamilienname (PFN)** : Dieser Wert wird bei bestimmten Windows-APIs verwendet.
-   **Paket-SID**: Sie benötigen diesen Wert, um WNS-Benachrichtigungen an Ihre App zu senden. Weitere Informationen finden Sie unter [Übersicht über den Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).


## <a name="link-to-your-apps-listing"></a>Erstellen eines Links zum Eintrag Ihrer App

Der direkte Link zu Ihrer App-Seite kann geteilt werden, um Kunden das Auffinden der App im Store zu erleichtern. Dieser Link hat das Format **`https://www.microsoft.com/store/apps/<your app's Store ID>`** . Wenn ein Kunde auf diesen Link klickt, wird die webbasierte Eintragsseite für Ihre App geöffnet. Auf Windows-Geräten wird die Store-App auch ebenfalls Ihren App-Eintrag starten und anzeigen.

Die **Store-ID** Ihrer App wird in diesem Abschnitt auch angezeigt. Diese Store-ID kann dazu verwendet werden, [Store-Badges zu generieren](https://developer.microsoft.com/store/badges) oder Ihre App anderweitig zu identifizieren.

Das **Store-Protokolllink** kann verwendet werden, um Ihre App im Store direkt und ohne einen neuen Browser zu öffnen, als ob Sie es innerhalb der App verknüpfen. Weitere Informationen finden Sie unter [Erstellen eines Links zu Ihrer App](link-to-your-app.md).



 

 




