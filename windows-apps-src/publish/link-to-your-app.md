---
description: Sie können Kunden dabei unterstützen, Ihre APP zu ermitteln, indem Sie im Microsoft Store eine Verknüpfung mit der Auflistung Ihrer APP herstellen.
title: Erstellen eines Links zu Ihrer App
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Link, Windows Store-Protokoll, Verknüpfen mit einer APP, Verknüpfen mit der APP
ms.localizationpriority: medium
ms.openlocfilehash: 916f82714feb65e3d9d4db48703c831b8128021c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033913"
---
# <a name="link-to-your-app"></a>Erstellen eines Links zu Ihrer App


Sie können Kunden dabei unterstützen, Ihre APP zu ermitteln, indem Sie im Microsoft Store eine Verknüpfung mit der Auflistung Ihrer APP herstellen.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Abrufen des Links zum Store-Eintrag Ihrer App

Um die URL für die Store-Auflistung Ihrer APP zu erhalten, navigieren Sie im Abschnitt **App-Verwaltung** zur Seite [App-Identität](view-app-identity-details.md) der app. Die URL weist das Format auf **`https://www.microsoft.com/store/apps/<your app's Store ID>`** .

Wenn ein Kunde auf diesen Link klickt, wird die webbasierte Auflistungs Seite für Ihre APP geöffnet. Auf Windows-Geräten wird die Store-App auch gestartet und die Liste der apps angezeigt.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Verknüpfen mit der Store-Auflistung Ihrer APP mit dem Microsoft Store Badge

Sie können direkt mit der Auflistung Ihrer APP mit einem benutzerdefinierten Badge verknüpfen, um Kunden zu informieren, dass sich Ihre APP im Microsoft Store befindet.

Um Ihr Badge zu erstellen, besuchen Sie die Seite [Microsoft Store](https://developer.microsoft.com/store/badges) -Badge. Sie benötigen die 12-Zeichen- **Speicher-ID** Ihrer APP, um das Badge und den Link zu generieren. Sie finden die **Store-ID** Ihrer APP auf der Seite " [App-Identität](view-app-identity-details.md) " im Abschnitt "App- **Verwaltung** ".

> [!NOTE]
> Informationen und Anforderungen im Zusammenhang mit der Verwendung des Microsoft Store Badge finden Sie unter [Richtlinien](app-marketing-guidelines.md) für die App-Marketing.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Direkte Verknüpfung mit Ihrer APP im Microsoft Store

Sie können einen Link erstellen, der die Microsoft Store öffnet und direkt zur Auflistungs Seite Ihrer APP wechselt, ohne einen Browser mit dem Schema " **MS-Windows-Store:** URI" zu öffnen.

Diese Links sind nützlich, wenn Sie wissen, dass sich Ihre Benutzer auf einem Windows-Gerät befinden, und Sie möchten, dass Sie direkt auf der Listenseite im Store eintreffen. Beispielsweise können Sie diesen Link verwenden, nachdem Sie die Benutzer-Agent-Zeichen folgen in einem Browser überprüft haben, um zu bestätigen, dass der Speicher vom Betriebssystem des Benutzers unterstützt wird oder wenn Sie bereits über eine UWP-App kommunizieren.

Um dieses URI-Schema für die direkte Verknüpfung mit der Store-Auflistung Ihrer APP zu verwenden, fügen Sie die Store-ID Ihrer APP an diesen Link an:

`ms-windows-store://pdp/?ProductId=`

Weitere Informationen zur Verwendung des Microsoft Store Protokolls finden Sie unter [Starten der Microsoft-App](../launch-resume/launch-store-app.md).

 

 




