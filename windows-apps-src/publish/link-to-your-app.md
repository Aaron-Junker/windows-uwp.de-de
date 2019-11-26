---
Description: Sie können Kunden dabei unterstützen, Ihre APP zu ermitteln, indem Sie im Microsoft Store eine Verknüpfung mit der Auflistung Ihrer APP herstellen.
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


Sie können Kunden dabei unterstützen, Ihre APP zu ermitteln, indem Sie im Microsoft Store eine Verknüpfung mit der Auflistung Ihrer APP herstellen.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Abrufen des Links zum Store-Eintrag Ihrer App

Um die URL für den Store-Eintrag Ihrer App zu erhalten, wechseln Sie zur Seite der App [App-Identität](view-app-identity-details.md) im Abschnitt **App-Verwaltung**. Das URL-Format heißt **`https://www.microsoft.com/store/apps/<your app's Store ID>`** .

Wenn ein Kunde auf diesen Link klickt, wird die webbasierte Eintragsseite für Ihre App geöffnet. Auf Windows-Geräten wird die Store-App auch ebenfalls Ihren App-Eintrag starten und anzeigen.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Verknüpfen mit der Store-Auflistung Ihrer APP mit dem Microsoft Store Badge

Sie können direkt mit der Auflistung Ihrer APP mit einem benutzerdefinierten Badge verknüpfen, um Kunden zu informieren, dass sich Ihre APP im Microsoft Store befindet.

Um Ihr Badge zu erstellen, besuchen Sie die Seite [Microsoft Store](https://developer.microsoft.com/store/badges) -Badge. Sie benötigen die zwölfstellige **Store-ID** Ihrer App, um dieses Formular zum Generieren von Badge und Link verwenden zu können. Die **Store ID** Ihrer App finden Sie auf der Seite [App-Identität](view-app-identity-details.md) unter **App-Verwaltung**.

> [!NOTE]
> Informationen und Anforderungen im Zusammenhang mit der Verwendung des Microsoft Store Badge finden Sie unter [Richtlinien](app-marketing-guidelines.md) für die App-Marketing.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Direkte Verknüpfung mit Ihrer APP im Microsoft Store

Sie können einen Link erstellen, der die Microsoft Store öffnet und direkt zur Auflistungs Seite Ihrer APP wechselt, ohne einen Browser mit dem Schema " **MS-Windows-Store:** URI" zu öffnen.

Diese Links sind hilfreich, wenn Sie wissen, dass Benutzer Windows-Geräte verwenden, und möchten, dass sie direkt zur Eintragsseite im Store gelangen. Sie sollten z. B. diesen Link verwenden, nachdem Sie die Zeichenfolge des Benutzer-Agent in einem Browser bestätigt haben, um zu überprüfen, dass das Betriebssystem des Benutzers den Store unterstützt, oder wenn Sie bereits über eine UWP-App kommunizieren.

Um dieses URI-Schema für die direkte Verknüpfung mit der Store-Auflistung Ihrer APP zu verwenden, fügen Sie die Store-ID Ihrer APP an diesen Link an:

`ms-windows-store://pdp/?ProductId=`

Weitere Informationen zur Verwendung des Microsoft Store Protokolls finden Sie unter [Starten der Microsoft-App](../launch-resume/launch-store-app.md).

 

 




