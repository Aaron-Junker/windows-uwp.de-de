---
Description: Informationen Sie zum Registrieren Ihrer UWP-app, um Pushbenachrichtigungen empfangen, die Sie vom Partner Center zu senden.
title: Konfigurieren der App für benutzerorientierte Pushbenachrichtigungen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, Uwp, Microsoft Store Services SDK, Ziel, Pushbenachrichtigungen, Partner Center
ms.assetid: 30c832b7-5fbe-4852-957f-7941df8eb85a
ms.localizationpriority: medium
ms.openlocfilehash: a23da0bf740abfeece0047b8afab2ebff987f9d1
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58335048"
---
# <a name="configure-your-app-for-targeted-push-notifications"></a>Konfigurieren der App für benutzerorientierte Pushbenachrichtigungen

Sie können die **Pushbenachrichtigungen** Seite im Partner Center zu direkt Kontakt zu Kunden durch Senden von aufzunehmen Ziel von Pushbenachrichtigungen an die Geräte, auf denen Ihre app für die universelle Windows-Plattform (UWP) installiert ist. So können Sie beispielsweise Ihre Kunden mithilfe von benutzerorientierten Pushbenachrichtigungen auffordern, aktiv zu werden, etwa Ihre App zu bewerten oder ein neues Feature auszuprobieren. Sie können verschiedene Arten von Pushbenachrichtigungen verwenden, darunter Popupbenachrichtigungen, Kachelbenachrichtigungen und reine XML-Benachrichtigungen. Sie können auch die Rate der App-Starts nachverfolgen, die durch Ihre Pushbenachrichtigungen ausgelöst wurden. Weitere Informationen zu diesem Feature finden Sie unter [Senden von Pushbenachrichtigungen an den Kunden Ihrer App](../publish/send-push-notifications-to-your-apps-customers.md).

Bevor Sie gezielte Pushbenachrichtigungen an Ihre Kunden von Partner Center senden können, müssen Sie eine Methode zum Verwenden der [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) Klasse in der Microsoft Store Services SDK zum Registrieren Ihrer app empfangen Benachrichtigungen. Sie können zusätzliche Methoden dieser Klasse verwenden, um Partner Center zu benachrichtigen, dass Ihre app als Reaktion auf einer gezielten Pushbenachrichtigung gestartet wurde (Wenn Sie möchten die Rate der app-Startvorgänge zu verfolgen, die bei Ihrer Benachrichtigungen) und den Empfang von Benachrichtigungen beenden.

## <a name="configure-your-project"></a>Konfigurieren des Projekts

Gehen Sie vor dem Schreiben von Code wie folgt vor, um in Ihrem Projekt einen Verweis auf das Microsoft Store Services SDK hinzuzufügen:

1. Falls noch nicht geschehen, [installieren Sie das Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) auf Ihrem Entwicklungscomputer. 
2. Öffnen Sie das Projekt in Visual Studio.
3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Knoten **Verweise** für Ihr Projekt, und wählen Sie anschließend **Verweis hinzufügen** aus.
4. Erweitern Sie im **Verweis-Manager** die Option **Universelle Windows-App**, und klicken Sie auf **Erweiterungen**.
5. Klicken Sie in der Liste der SDKs auf das Kontrollkästchen neben **Microsoft Engagement Framework** und anschließend auf **OK**.

## <a name="register-for-push-notifications"></a>Registrierung für Pushbenachrichtigungen

So registrieren Sie Ihre app, um gezielte Pushbenachrichtigungen von Partner Center zu erhalten:

1. Suchen Sie in Ihrem Projekt einen Codeabschnitt, der während des Starts ausgeführt wird und in dem Sie Ihre App zum Empfangen von Benachrichtigungen registrieren können.
2. Fügen Sie zu Beginn der Codedatei die folgende Anweisung ein:

    [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#EngagementNamespace)]

3. Rufen Sie ein [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager)-Objekt ab, und rufen Sie eine der [RegisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)-Überladungen in dem zuvor identifizierten Startcode auf. Diese Methode sollte bei jedem Start Ihrer App aufgerufen werden.

  * Wenn Sie Partner Center einen eigenen Kanal-URI für die Benachrichtigungen erstellen möchten, rufen Sie die [RegisterNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) überladen.

      [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync1)]
      > [!IMPORTANT]
      > Wenn Ihre App auch [CreatePushNotificationChannelForApplicationAsync](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) zur Erstellung eines Benachrichtigungskanals für WNS aufruft, achten Sie darauf, dass Ihr Code nicht gleichzeitig die Überladungen [CreatePushNotificationChannelForApplicationAsync](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) und [RegisterNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) aufruft. Wenn Sie beide Methoden aufrufen müssen, stellen Sie sicher, dass Sie sie nacheinander aufrufen und dass die Rückgabe einer der Methoden abgewartet wird, bevor die andere aufgerufen wird.

  * Wenn Sie den Kanal-URI für die Verwendung für gezielte Pushbenachrichtigungen von Partner Center Aufruf angeben möchten, die [RegisterNotificationChannelAsync(StoreServicesNotificationChannelParameters)](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) überladen. Sie sollten dies beispielsweise tun, wenn Ihre App den Windows-Pushbenachrichtigungsdienst (WNS) bereits verwendet und Sie dieselbe Kanal-URI verwenden möchten. Erstellen Sie zuerst ein [StoreServicesNotificationChannelParameters](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters)-Objekt, und weisen Sie die [CustomNotificationChannelUri](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri)-Eigenschaft dem Kanal-URI zu.

      [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync2)]

> [!NOTE]
> Beim Aufrufen der **RegisterNotificationChannelAsync**-Methode wird eine Datei mit dem Namen MicrosoftStoreEngagementSDKId.txt im lokalen App-Datenspeicher für Ihre App erstellt. (Dies ist der von der Eigenschaft [ApplicationData.LocalFolder](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData.LocalFolder) zurückgegebene Ordner.) Diese Datei enthält eine ID, die von der Infrastruktur der benutzerorientierten Pushbenachrichtigungen verwendet wird. Stellen Sie sicher, dass Ihre App diese Datei nicht ändert oder löscht. Andernfalls erhalten die Benutzer möglicherweise mehrere Instanzen von Benachrichtigungen, oder die Benachrichtigungen verhalten sich auf andere Weise nicht ordnungsgemäß.

<span id="notification-customers" />

### <a name="how-targeted-push-notifications-are-routed-to-customers"></a>Wie benutzerorientierte Pushbenachrichtigungen an Kunden weitergeleitet werden

Wenn Ihre App **RegisterNotificationChannelAsync** aufruft, erfasst diese Methode das Microsoft-Konto des Kunden, der derzeit bei dem Gerät angemeldet ist. Beim Senden einer gezielten Pushbenachrichtigung an ein Segment, das dieses Kunden enthält, sendet Partner Center die Benachrichtigung später für Geräte, die diese Kunden Microsoft-Konto zugeordnet sind.

Beachten Sie Folgendes: Wenn der Kunde, der Ihre App gestartet hat, das Gerät an eine anderen Person übergibt, jedoch noch mit seinem Microsoft-Konto bei diesem Gerät angemeldet ist, sieht die andere Person möglicherweise die Benachrichtigung, die für den ursprünglichen Kunden bestimmt war. Dies kann unerwünschte Folgen nach sich ziehen, insbesondere im Fall von Apps, die Dienste anbieten, für deren Nutzung Kunden sich anmelden müssen. Um zu verhindern, dass andere Benutzer in diesem Szenario Ihre benutzerorientierte Benachrichtigungen sehen können, rufen Sie die [UnregisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync)-Methode auf, wenn Kunden sich von Ihrer App abmelden. Weitere Informationen finden Sie unter [Aufheben der Registrierung für Pushbenachrichtigungen](#unregister) weiter unten in diesem Artikel.

### <a name="how-your-app-responds-when-the-user-launches-your-app"></a>Reaktionsweise Ihrer App, wenn der Benutzer die App startet

Nach dem Registrieren Ihrer app zum Empfangen von Benachrichtigungen und Sie [Senden einer Pushbenachrichtigung an Ihre app Kunden in Partner Center](../publish/send-push-notifications-to-your-apps-customers.md), eine der folgenden Einstiegspunkte in Ihrer app wird aufgerufen, wenn der Benutzer eine app als Reaktion startet. mit Ihrer Pushbenachrichtigung. Wenn Sie Code haben, der ausgeführt werden soll, wenn der Benutzer die App startet, können Sie ihn einem dieser Einstiegspunkte in Ihrer App hinzufügen.

  * Hat die Pushbenachrichtigung einen Vordergrund-Aktivierungstyp, übergehen Sie die [OnActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated)-Methode der **App**-Klasse in Ihrem Projekt, und fügen Sie Ihren Code dieser Methode hinzu.

  * Hat die Pushbenachrichtigung einen Hintergrund-Aktivierungstyp, fügen Sie Ihren Code der [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run)-Methode für Ihre [Hintergrundaufgabe](../launch-resume/support-your-app-with-background-tasks.md) hinzu.

So können Sie beispielsweise die Benutzer Ihrer App, die kostenpflichtige Add-Ons gekauft haben, mit einem kostenlosen Add-On belohnen. In diesem Fall können Sie eine Pushbenachrichtigung an ein [Kundensegment](../publish/create-customer-segments.md) senden, die auf diese Benutzer ausgerichtet ist. Dann können Sie in einem der oben aufgeführten Einstiegspunkte Code hinzufügen, um diesen Kunden einen kostenlosen [In-App-Kauf](in-app-purchases-and-trials.md) zu gewähren.

## <a name="notify-partner-center-of-your-app-launch"></a>Benachrichtigen von Partner Center von Ihrer app starten

Bei Auswahl der **nachverfolgen app-Start-Rate** Option für Ihre gezielten Pushbenachrichtigung im Partner Center Aufruf der [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) Methode aus den entsprechenden Einstiegspunkt in Ihre app Partner Center zu benachrichtigen, dass Ihre app als Reaktion auf eine Pushbenachrichtigung gestartet wurde.

Diese Methode gibt auch die ursprünglichen Startargumente für Ihre App zurück. Bei der Auswahl des app-Start-Satzes für Ihrer Pushbenachrichtigung nachverfolgen, starten Sie ein nicht transparenter Überwachung, die ID hinzugefügt wird die Startargumente verfolgen, die app im Partner Center. Sie müssen die Startargumente für Ihre App übergeben die [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) -Methode, und diese Methode sendet die nachverfolgungs-ID zum Partner Center, entfernt die nachverfolgungs-ID aus den Argumenten starten und gibt die ursprüngliche Starten Sie die Argumente, die Ihren Code.

Die Art und Weise des Aufrufs der Methode hängt von dem Aktivierungstyp der Pushbenachrichtigung ab.

* Hat die Pushbenachrichtigung einen Vordergrund-Aktivierungstyp, rufen Sie diese Methode aus der [OnActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated)-Methodenüberschreibung in Ihrer App auf, und übergeben Sie die Argumente, die im [ToastNotificationActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)-Objekt verfügbar sind, an das Objekt, das an diese Methode übergeben wird. Im folgenden Codebeispiel wird davon ausgegangen, dass Ihre Codedatei **using**-Anweisungen für die Namespaces **Microsoft.Services.Store.Engagement** und **Windows.ApplicationModel.Activation** enthält.

  [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/App.xaml.cs#OnActivated)]

* Wenn die Pushbenachrichtigung einen Hintergrundaktivierungstyp hat, rufen Sie diese Methode aus der [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run)-Methode für Ihre [Hintergrundaufgabe](../launch-resume/support-your-app-with-background-tasks.md) auf, und übergeben Sie die Argumente, die im [ToastNotificationActionTriggerDetail](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationActionTriggerDetail)-Objekt verfügbar sind, das an diese Methode übergeben wird. Im folgenden Codebeispiel wird davon ausgegangen, dass Ihre Codedatei **using**-Anweisungen für die Namespaces **Microsoft.Services.Store.Engagement**, **Windows.ApplicationModel.Background** und **Windows.UI.Notifications** enthält.

  [!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#Run)]

<span id="unregister" />

## <a name="unregister-for-push-notifications"></a>Aufheben der Registrierung für Pushbenachrichtigungen

Wenn Ihre app den Empfang von beenden soll als Ziel Pushbenachrichtigungen von Partner Center Aufruf der [UnregisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) Methode.

[!code-csharp[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#UnregisterNotificationChannelAsync)]

Beachten Sie, dass diese Methode den Kanal, der für Benachrichtigungen verwendet wird, ungültig macht, damit die App keine Pushbenachrichtigungen *irgendwelcher* Dienste mehr empfängt. Nachdem es geschlossen wurde, kann nicht der Kanal erneut für alle Dienste, einschließlich gezielte Pushbenachrichtigungen über Partner Center und andere Benachrichtigungen, die Verwendung von WNS verwendet werden. Damit wieder Pushbenachrichtigungen an diese App gesendet werden können, muss die App einen neuen Kanal anfragen.

## <a name="related-topics"></a>Verwandte Themen

* [Senden von Pushbenachrichtigungen an die Kunden Ihrer App](../publish/send-push-notifications-to-your-apps-customers.md)
* [Übersicht über die Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview)
* [Wie Sie anfordern, erstellen und speichern einen Benachrichtigungskanal](https://docs.microsoft.com/previous-versions/windows/apps/hh868221(v=win.10))
* [Microsoft Store Services SDK](https://docs.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
