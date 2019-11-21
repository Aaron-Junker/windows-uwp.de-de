---
Description: You can help customers discover your app by linking to your app's listing in the Microsoft Store.
title: Erstellen eines Links zu Ihrer App
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Links, Windows Store-Protokoll, mit einer App verknüpfen, App verknüpfen
ms.localizationpriority: medium
ms.openlocfilehash: de22505cf42193932a5bbd951c983e02eea37bd7
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259986"
---
# <a name="link-to-your-app"></a>Erstellen eines Links zu Ihrer App


You can help customers discover your app by linking to your app's listing in the Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Abrufen des Links zum Store-Eintrag Ihrer App

Um die URL für den Store-Eintrag Ihrer App zu erhalten, wechseln Sie zur Seite der App [App-Identität](view-app-identity-details.md) im Abschnitt **App-Verwaltung**. Das URL-Format heißt **`https://www.microsoft.com/store/apps/<your app's Store ID>`** .

Wenn ein Kunde auf diesen Link klickt, wird die webbasierte Eintragsseite für Ihre App geöffnet. Auf Windows-Geräten wird die Store-App auch ebenfalls Ihren App-Eintrag starten und anzeigen.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Linking to your app's Store listing with the Microsoft Store badge

You can link directly to your app's listing with a custom badge to let customers know your app is in the Microsoft Store.

To create your badge, visit the [Microsoft Store badges](https://developer.microsoft.com/store/badges) page. Sie benötigen die zwölfstellige **Store-ID** Ihrer App, um dieses Formular zum Generieren von Badge und Link verwenden zu können. Die **Store ID** Ihrer App finden Sie auf der Seite [App-Identität](view-app-identity-details.md) unter **App-Verwaltung**.

> [!NOTE]
> See [App marketing guidelines](app-marketing-guidelines.md) for info and requirements related to your use of the Microsoft Store badge.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Linking directly to your app in the Microsoft Store

You can create a link that launches the Microsoft Store and goes directly to your app's listing page without opening a browser by using the **ms-windows-store:** URI scheme.

Diese Links sind hilfreich, wenn Sie wissen, dass Benutzer Windows-Geräte verwenden, und möchten, dass sie direkt zur Eintragsseite im Store gelangen. Sie sollten z. B. diesen Link verwenden, nachdem Sie die Zeichenfolge des Benutzer-Agent in einem Browser bestätigt haben, um zu überprüfen, dass das Betriebssystem des Benutzers den Store unterstützt, oder wenn Sie bereits über eine UWP-App kommunizieren.

To use this URI scheme to link directly to your app's Store listing, append your app's Store ID to this link:

`ms-windows-store://pdp/?ProductId=`

For more about using the Microsoft Store protocol, see [Launch the Microsoft app](../launch-resume/launch-store-app.md).

 

 




