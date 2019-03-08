---
Description: Sie können Kunden Ihre app zu ermitteln, indem Sie mit Ihrer app-Auflistung, in dem Microsoft Store verknüpfen.
title: Erstellen eines Links zu Ihrer App
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Links, Windows Store-Protokoll, mit einer App verknüpfen, App verknüpfen
ms.localizationpriority: medium
ms.openlocfilehash: 56bc051c3c5a935f3b6b26e478731fcde9c06902
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661535"
---
# <a name="link-to-your-app"></a>Erstellen eines Links zu Ihrer App


Sie können Kunden Ihre app zu ermitteln, indem Sie mit Ihrer app-Auflistung, in dem Microsoft Store verknüpfen.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Abrufen des Links zum Store-Eintrag Ihrer App

Um die URL für den Store-Eintrag Ihrer App zu erhalten, wechseln Sie zur Seite der App [App-Identität](view-app-identity-details.md) im Abschnitt **App-Verwaltung**. Das URL-Format heißt **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Wenn ein Kunde auf diesen Link klickt, wird die webbasierte Eintragsseite für Ihre App geöffnet. Auf Windows-Geräten wird die Store-App auch ebenfalls Ihren App-Eintrag starten und anzeigen.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Verknüpfen mit Ihrer app-Store-Eintrag mit dem Microsoft Store-PIN

Sie können direkt mit Ihrer app-Liste mit benutzerdefinierten Signal Kunden wissen, dass Ihre app befindet sich in der Microsoft Store verknüpfen.

Um Ihr Badge erstellen zu können, finden Sie auf die [Microsoft Store-Pins](https://go.microsoft.com/fwlink/p/?LinkID=534236) Seite. Sie benötigen die zwölfstellige **Store-ID** Ihrer App, um dieses Formular zum Generieren von Badge und Link verwenden zu können. Die **Store ID** Ihrer App finden Sie auf der Seite [App-Identität](view-app-identity-details.md) unter **App-Verwaltung**.

> [!NOTE]
> Finden Sie unter [App-marketing – Richtlinien](app-marketing-guidelines.md) Informationen und Anforderungen in Bezug auf Ihre Verwendung für den Microsoft Store-Badge.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Direkte Verknüpfung mit Ihrer app in den Microsoft Store

Sie können einen Link erstellen, die startet den Microsoft Store und greift direkt auf Ihrer app-Angebotsseite ohne Öffnen eines Browsers mithilfe der **ms-Windows-Store:** URI-Schema.

Diese Links sind hilfreich, wenn Sie wissen, dass Benutzer Windows-Geräte verwenden, und möchten, dass sie direkt zur Eintragsseite im Store gelangen. Sie sollten z. B. diesen Link verwenden, nachdem Sie die Zeichenfolge des Benutzer-Agent in einem Browser bestätigt haben, um zu überprüfen, dass das Betriebssystem des Benutzers den Store unterstützt, oder wenn Sie bereits über eine UWP-App kommunizieren.

Um dieses URI-Schema verwenden, die um direkt mit Ihrer app-Store auflisten zu verknüpfen, fügen Sie die ID Ihrer app Store auf diesen Link:

`ms-windows-store://pdp/?ProductId=`

Weitere Informationen dazu, mit dem Microsoft Store-Protokoll, finden Sie unter [starten Sie die Microsoft-app](../launch-resume/launch-store-app.md).

 

 




