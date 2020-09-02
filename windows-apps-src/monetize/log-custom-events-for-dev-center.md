---
Description: Sie können benutzerdefinierte Ereignisse aus ihrer UWP-App protokollieren und diese Ereignisse im Bericht "Verwendung" im Partner Center überprüfen.
title: Protokollieren benutzerdefinierter Ereignisse im Partner Center
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK, Protokollieren von Ereignissen
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: 5a1df08b62199bf1249af8bfbbb00921874a671c
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364103"
---
# <a name="log-custom-events-for-partner-center"></a>Protokollieren benutzerdefinierter Ereignisse im Partner Center

Der [Verwendungs Bericht](../publish/usage-report.md) in Partner Center ermöglicht Ihnen das erhalten von Informationen zu benutzerdefinierten Ereignissen, die Sie in ihrer universelle Windows-Plattform-app (UWP) definiert haben. Ein benutzerdefiniertes Ereignis ist eine beliebige Zeichenfolge, die ein Ereignis oder eine Aktivität in Ihrer App repräsentiert. Beispielsweise kann ein Spiel benutzerdefinierte Ereignisse mit den Bezeichnungen *FirstLevelPassed*, *SecondLevelPassed*usw. definieren, die protokolliert werden, wenn der Benutzer die einzelnen Levels des Spiels durchläuft.

Um ein benutzerdefiniertes Ereignis aus Ihrer App zu protokollieren, übergeben Sie die Zeichenfolge des benutzerdefinierten Ereignisses an die [Log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)-Methode des Microsoft Store Services SDK. Die Gesamtanzahl der Vorkommen für Ihre benutzerdefinierten Ereignisse finden Sie im Abschnitt **benutzerdefinierte Ereignisse** des [Nutzungs Berichts](../publish/usage-report.md) im Partner Center.

> [!NOTE]
> Benutzerdefinierte Ereignisse, die Sie bei Partner Center protokollieren, stehen nicht in Zusammenhang mit [Windows-Ereignissen](/windows/desktop/Events/windows-events)und werden nicht in **Ereignisanzeige**angezeigt.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie benutzerdefinierte Protokollierungs Ereignisse im **Verwendungs Bericht** für Ihre APP im Partner Center überprüfen können, muss Ihre APP im Store veröffentlicht werden.

## <a name="how-to-log-custom-events"></a>Protokollieren von benutzerdefinierten Ereignissen

1. Falls noch nicht geschehen, [installieren Sie das Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) auf Ihrem Entwicklungscomputer.

2. Öffnen Sie Ihr Projekt in Visual Studio.

3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Knoten **Verweise** für Ihr Projekt, und wählen Sie anschließend **Verweis hinzufügen** aus.

4. Erweitern Sie im **Verweis-Manager** die Option **Universelle Windows-App**, und klicken Sie auf **Erweiterungen**.

5. Klicken Sie in der Liste der SDKs auf das Kontrollkästchen neben **Microsoft Engagement Framework** und anschließend auf **OK**.

6. Fügen Sie die folgende Anweisung am Anfang jeder Codedatei hinzu, in der Sie benutzerdefinierte Ereignisse protokollieren möchten.
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/LogEvents.cs" id="EngagementNamespace":::

7. Rufen Sie in jedem Abschnitt des Codes, in dem Sie ein benutzerdefiniertes Ereignis protokollieren möchten, ein [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)-Objekt ab, und rufen Sie dann die [Protokoll](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)-Methode auf. Übergeben Sie die Zeichenfolge für das benutzerdefinierte Ereignis an die Methode.
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/LogEvents.cs" id="Log":::

    > [!NOTE]
    > Der [Verwendungs Bericht](../publish/usage-report.md) kann viel Zeit in Anspruch nehmen, wenn die APP viele benutzerdefinierte Ereignisse mit langen Namen protokolliert. Es wird empfohlen, für Ihre benutzerdefinierten Ereignisse kurze Namen zu verwenden. 

## <a name="related-topics"></a>Verwandte Themen

* [Nutzungsbericht](../publish/usage-report.md)
* [Protokollierungsmethode](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](./microsoft-store-services-sdk.md)
