---
Description: Sie können benutzerdefinierte Ereignisse protokollieren, aus der UWP-app und überprüfen diese Ereignisse im Bericht zu im Partner Center.
title: Protokollieren benutzerdefinierter Ereignisse im Partner Center
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Services-SDK, Ereignisse protokollieren
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: 368e2fb7b3c6d78f68235b829e088d79b5673cf2
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334758"
---
# <a name="log-custom-events-for-partner-center"></a>Protokollieren benutzerdefinierter Ereignisse im Partner Center

Die [Nutzungsbericht](https://msdn.microsoft.com/windows/uwp/publish/usage-report) in Partner Center können Sie erhalten Informationen zu benutzerdefinierten Ereignissen, die Sie in Ihrer app (Universelle Windows Plattform) definiert haben. Ein benutzerdefiniertes Ereignis ist eine beliebige Zeichenfolge, die ein Ereignis oder eine Aktivität in Ihrer App repräsentiert. Beispielsweise kann ein Spiel benutzerdefinierte Ereignisse mit den Bezeichnungen *FirstLevelPassed*, *SecondLevelPassed*usw. definieren, die protokolliert werden, wenn der Benutzer die einzelnen Levels des Spiels durchläuft.

Um ein benutzerdefiniertes Ereignis aus Ihrer App zu protokollieren, übergeben Sie die Zeichenfolge des benutzerdefinierten Ereignisses an die [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)-Methode des Microsoft Store Services SDK. Sie können die gesamtvorkommen überprüfen, für Ihre benutzerdefinierten Ereignisse in der **benutzerdefinierte Ereignisse** im Abschnitt der [Nutzungsbericht](https://msdn.microsoft.com/windows/uwp/publish/usage-report) im Partner Center.

> [!NOTE]
> Benutzerdefinierte Ereignisse, die Sie zum Partner Center protokollieren sind unabhängig vom stagingstatus [Windows-Ereignisse](https://msdn.microsoft.com/library/windows/desktop/aa964766.aspx), und sie erscheinen nicht im **Ereignisanzeige**.

## <a name="prerequisites"></a>Vorraussetzungen

Bevor Sie, die benutzerdefinierte Protokollierung von Ereignissen in überprüfen können der **Nutzungsbericht** für Ihre app im Partner Center, muss Ihre app in den Store veröffentlicht werden.

## <a name="how-to-log-custom-events"></a>Protokollieren von benutzerdefinierten Ereignissen

1. Falls noch nicht geschehen, [installieren Sie das Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) auf Ihrem Entwicklungscomputer.

2. Öffnen Sie das Projekt in Visual Studio.

3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Knoten **Verweise** für Ihr Projekt, und wählen Sie anschließend **Verweis hinzufügen** aus.

4. Erweitern Sie im **Verweis-Manager** die Option **Universelle Windows-App**, und klicken Sie auf **Erweiterungen**.

5. Klicken Sie in der Liste der SDKs auf das Kontrollkästchen neben **Microsoft Engagement Framework** und anschließend auf **OK**.

6. Fügen Sie die folgende Anweisung am Anfang jeder Codedatei hinzu, in der Sie benutzerdefinierte Ereignisse protokollieren möchten.
    [!code-csharp[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

7. Rufen Sie in jedem Abschnitt des Codes, in dem Sie ein benutzerdefiniertes Ereignis protokollieren möchten, ein [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)-Objekt ab, und rufen Sie dann die [Protokoll](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)-Methode auf. Übergeben Sie die Zeichenfolge für das benutzerdefinierte Ereignis an die Methode.
    [!code-csharp[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

    > [!NOTE]
    > Möglicherweise dauert das Laden des [Nutzungsberichts](https://msdn.microsoft.com/windows/uwp/publish/usage-report) lange, wenn Ihre App viele benutzerdefinierte Ereignisse mit langen Namen protokolliert. Es wird empfohlen, dass kurze Namen für Ihre benutzerdefinierten Ereignisse zu verwenden. 

## <a name="related-topics"></a>Verwandte Themen

* [Nutzungsbericht](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [Log-Methode](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
