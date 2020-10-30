---
description: Anzeigen von Details im Zusammenhang mit der eindeutigen Identität, die ihrer app durch das Microsoft Store zugewiesen ist, und erhalten eines Links zur Store-Auflistung Ihrer APP.
title: Anzeigen von Details zur App-Identität
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 11984d1aa5f8da20b529ab485615bd1f760cc2cd
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034903"
---
# <a name="view-app-identity-details"></a>Anzeigen von Details zur App-Identität


Sie können Details im Zusammenhang mit der eindeutigen Identität, die Ihrer APP zugewiesen ist, anzeigen, indem Sie die Microsoft Store auf den zugehörigen **App-Identitäts** Seiten Sie können auch einen Link zur Store-Auflistung Ihrer APP auf dieser Seite erhalten.

Um diese Informationen zu suchen, navigieren Sie zu einer Ihrer Apps und erweitern im linken Navigationsmenü **App-Verwaltung** . Wählen Sie **App-Identität** aus, um diese Details anzuzeigen.


## <a name="values-to-include-in-your-app-package-manifest"></a>Werte, die in das App-Paket Manifest aufgenommen werden sollen

Die folgenden Werte müssen im Paket Manifest enthalten sein. Wenn Sie [Microsoft Visual Studio zum Erstellen der Pakete verwenden](/windows/msix/package/packaging-uwp-apps)und mit demselben Microsoft-Konto angemeldet sind, das Sie mit Ihrem Entwicklerkonto verknüpft haben, werden diese Details automatisch eingeschlossen. Wenn Sie das Paket manuell entwickeln, müssen Sie die folgenden Elemente hinzufügen:

-   **Paket/Identität/Name**
-   **Paket/Identität/Herausgeber**
-   **Package/Properties/publisherdisplayname**

Weitere Informationen finden Sie unter [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) in der [Paketmanifestschema-Referenz](/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Zusammen werden durch diese Elemente die Identität Ihrer App deklariert und die „Paketfamilie“ gebildet, der alle zugehörigen Pakete angehören. Einzelne Pakete verfügen über zusätzliche Details, z. B. die Architektur und Version.


## <a name="additional-values-for-package-family"></a>Zusätzliche Werte für die Paketfamilie

Die folgenden zusätzlichen Werte beziehen sich auf die Paketfamilie der App, werden aber nicht in Ihr Manifest eingeschlossen.

-   **Paketfamilienname (PFN)** : Dieser Wert wird bei bestimmten Windows-APIs verwendet.
-   **Paket-SID** : Sie benötigen diesen Wert, um WNS-Benachrichtigungen an Ihre App zu senden. Weitere Informationen finden Sie unter [Übersicht über den Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).


## <a name="link-to-your-apps-listing"></a>Erstellen eines Links zum Eintrag Ihrer App

Der direkte Link zur Seite Ihrer APP kann freigegeben werden, damit Ihre Kunden die APP im Store finden. Dieser Link weist das Format auf **`https://www.microsoft.com/store/apps/<your app's Store ID>`** . Wenn ein Kunde auf diesen Link klickt, wird die webbasierte Auflistungs Seite für Ihre APP geöffnet. Auf Windows-Geräten wird die Store-App auch gestartet und die Liste der apps angezeigt.

Die **Store-ID** Ihrer App wird in diesem Abschnitt auch angezeigt. Diese Store-ID kann dazu verwendet werden, [Store-Badges zu generieren](https://developer.microsoft.com/store/badges) oder Ihre App anderweitig zu identifizieren.

Der **Link "Store-Protokoll** " kann verwendet werden, um direkt eine Verbindung mit Ihrer APP im Store zu herstellen, ohne einen Browser zu öffnen, z. b. Wenn Sie eine Verknüpfung innerhalb einer App herstellen. Weitere Informationen finden Sie unter [Verknüpfen mit Ihrer APP](link-to-your-app.md).



 

 
