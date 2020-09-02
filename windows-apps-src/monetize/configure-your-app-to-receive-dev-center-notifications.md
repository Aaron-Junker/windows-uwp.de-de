---
Description: Erfahren Sie, wie Sie Ihre UWP-App registrieren, um Pushbenachrichtigungen zu erhalten, die Sie aus Partner Center senden.
title: Konfigurieren der App für benutzerorientierte Pushbenachrichtigungen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK, gezielte Pushbenachrichtigungen, Partner Center
ms.assetid: 30c832b7-5fbe-4852-957f-7941df8eb85a
ms.localizationpriority: medium
ms.openlocfilehash: abb901c1b067dcf3609cbfb5c4cf3f81c9dc465c
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364133"
---
# <a name="configure-your-app-for-targeted-push-notifications"></a>Konfigurieren der App für benutzerorientierte Pushbenachrichtigungen

Auf der Seite **Pushbenachrichtigungen** im Partner Center können Sie direkt mit Kunden in Kontakt treten, indem Sie gezielte Pushbenachrichtigungen an die Geräte senden, auf denen ihre universelle Windows-Plattform-app (UWP) installiert ist. So können Sie beispielsweise Ihre Kunden mithilfe von benutzerorientierten Pushbenachrichtigungen auffordern, aktiv zu werden, etwa Ihre App zu bewerten oder ein neues Feature auszuprobieren. Sie können verschiedene Arten von Pushbenachrichtigungen verwenden, darunter Popupbenachrichtigungen, Kachelbenachrichtigungen und reine XML-Benachrichtigungen. Sie können auch die Rate der App-Starts nachverfolgen, die durch Ihre Pushbenachrichtigungen ausgelöst wurden. Weitere Informationen zu diesem Feature finden Sie unter [Senden von Pushbenachrichtigungen an den Kunden Ihrer App](../publish/send-push-notifications-to-your-apps-customers.md).

Bevor Sie gezielte Pushbenachrichtigungen von Partner Center an Ihre Kunden senden können, müssen Sie eine Methode der [storeservicesengagementmanager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) -Klasse im Microsoft Store Services SDK verwenden, um die APP für den Empfang von Benachrichtigungen zu registrieren. Sie können mit zusätzlichen Methoden dieser Klasse Partner Center Benachrichtigen, dass Ihre APP als Reaktion auf eine gezielte Pushbenachrichtigung gestartet wurde (wenn Sie die Rate der App-Starts nachverfolgen möchten, die aus Ihren Benachrichtigungen resultieren), und den Empfang von Benachrichtigungen nicht mehr erreichen möchten.

## <a name="configure-your-project"></a>Konfigurieren des Projekts

Gehen Sie vor dem Schreiben von Code wie folgt vor, um in Ihrem Projekt einen Verweis auf das Microsoft Store Services SDK hinzuzufügen:

1. Falls noch nicht geschehen, [installieren Sie das Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) auf Ihrem Entwicklungscomputer. 
2. Öffnen Sie Ihr Projekt in Visual Studio.
3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Knoten **Verweise** für Ihr Projekt, und wählen Sie anschließend **Verweis hinzufügen** aus.
4. Erweitern Sie im **Verweis-Manager** die Option **Universelle Windows-App**, und klicken Sie auf **Erweiterungen**.
5. Klicken Sie in der Liste der SDKs auf das Kontrollkästchen neben **Microsoft Engagement Framework** und anschließend auf **OK**.

## <a name="register-for-push-notifications"></a>Registrieren für Pushbenachrichtigungen

So registrieren Sie Ihre APP für den Empfang gezielter Pushbenachrichtigungen von Partner Center:

1. Suchen Sie in Ihrem Projekt einen Code Abschnitt, der während des Starts ausgeführt wird, in dem Sie Ihre APP registrieren können, um Benachrichtigungen zu empfangen.
2. Fügen Sie zu Beginn der Codedatei die folgende Anweisung ein:

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="EngagementNamespace":::

3. Rufen Sie ein [StoreServicesEngagementManager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager)-Objekt ab, und rufen Sie eine der [RegisterNotificationChannelAsync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)-Überladungen in dem zuvor identifizierten Startcode auf. Diese Methode sollte bei jedem Start Ihrer App aufgerufen werden.

  * Wenn Partner Center einen eigenen Kanal-URI für die Benachrichtigungen erstellen soll, rufen Sie die Überladung [registernotificationchannelasync ()](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) auf.

      :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="RegisterNotificationChannelAsync1":::
      > [!IMPORTANT]
      > Wenn Ihre APP auch " [foratepushnotificationchannelforapplicationasync](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) " aufruft, um einen Benachrichtigungs Kanal für WNS zu erstellen, müssen Sie sicherstellen, dass der Code nicht " [foratepushnotificationchannelforapplicationasync](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) " aufruft und die [registernotificationchannelasync ()](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) -Überladung gleichzeitig ausgeführt wird. Wenn Sie beide Methoden aufrufen müssen, stellen Sie sicher, dass Sie sie nacheinander aufrufen und dass die Rückgabe einer der Methoden abgewartet wird, bevor die andere aufgerufen wird.

  * Wenn Sie den Kanal-URI angeben möchten, der für gezielte Pushbenachrichtigungen von Partner Center verwendet werden soll, müssen Sie die Überladung [registernotificationchannelasync (storeservicesnotificationchannelparameters)](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) aufrufen. Sie sollten dies beispielsweise tun, wenn Ihre App den Windows-Pushbenachrichtigungsdienst (WNS) bereits verwendet und Sie dieselbe Kanal-URI verwenden möchten. Erstellen Sie zuerst ein [StoreServicesNotificationChannelParameters](/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters)-Objekt, und weisen Sie die [CustomNotificationChannelUri](/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri)-Eigenschaft dem Kanal-URI zu.

      :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="RegisterNotificationChannelAsync2":::

> [!NOTE]
> Wenn Sie die **registernotificationchannelasync** -Methode aufrufen, wird eine Datei mit dem Namen MicrosoftStoreEngagementSDKId.txt im lokalen app-Datenspeicher für Ihre APP erstellt (der Ordner, der von der [ApplicationData. localfolder](/uwp/api/Windows.Storage.ApplicationData.LocalFolder) -Eigenschaft zurückgegeben wird). Diese Datei enthält eine ID, die von der Ziel Infrastruktur für Pushbenachrichtigungen verwendet wird. Stellen Sie sicher, dass diese Datei von Ihrer APP nicht geändert oder gelöscht wird. Andernfalls erhalten die Benutzer möglicherweise mehrere Instanzen von Benachrichtigungen, oder die Benachrichtigungen Verhalten sich auf andere Weise nicht ordnungsgemäß.

<span id="notification-customers" />

### <a name="how-targeted-push-notifications-are-routed-to-customers"></a>Weiterleiten gezielter Pushbenachrichtigungen an Kunden

Wenn Ihre APP **registernotificationchannelasync**aufruft, sammelt diese Methode den Microsoft-Konto des Kunden, der zurzeit am Gerät angemeldet ist. Wenn Sie später eine gezielte Pushbenachrichtigung an ein Segment senden, das diesen Kunden enthält, sendet Partner Center die Benachrichtigung an Geräte, die mit dem Microsoft-Konto dieses Kunden verknüpft sind.

Wenn der Kunde, der Ihre APP gestartet hat, das Gerät an eine andere Person weitergeben soll, während Sie mit Ihrem Microsoft-Konto weiterhin beim Gerät angemeldet sind, sollten Sie beachten, dass die andere Person die Benachrichtigung erhält, die auf den ursprünglichen Kunden gerichtet war. Dies kann unbeabsichtigte Folgen haben, insbesondere für apps, die Dienste anbieten, die Kunden für die Verwendung von anmelden können. Um zu verhindern, dass andere Benutzer ihre Ziel Benachrichtigungen in diesem Szenario anzeigen, müssen Sie die [unregisternotificationchannelasync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) -Methode aufrufen, wenn sich Kunden von Ihrer APP abmelden. Weitere Informationen finden Sie weiter unten in diesem Artikel unter Aufheben der [Registrierung für Pushbenachrichtigungen](#unregister) .

### <a name="how-your-app-responds-when-the-user-launches-your-app"></a>Wie Ihre APP reagiert, wenn der Benutzer Ihre APP gestartet

Nachdem die APP für den Empfang von Benachrichtigungen registriert wurde und Sie [eine Pushbenachrichtigung an die Kunden der APP über Partner Center gesendet](../publish/send-push-notifications-to-your-apps-customers.md)haben, wird einer der folgenden Einstiegspunkte in ihrer app aufgerufen, wenn der Benutzer die APP als Reaktion auf Ihre Pushbenachrichtigung startet. Wenn Sie Code haben, der ausgeführt werden soll, wenn der Benutzer die App startet, können Sie ihn einem dieser Einstiegspunkte in Ihrer App hinzufügen.

  * Hat die Pushbenachrichtigung einen Vordergrund-Aktivierungstyp, übergehen Sie die [OnActivated](/uwp/api/windows.ui.xaml.application.onactivated)-Methode der **App**-Klasse in Ihrem Projekt, und fügen Sie Ihren Code dieser Methode hinzu.

  * Hat die Pushbenachrichtigung einen Hintergrund-Aktivierungstyp, fügen Sie Ihren Code der [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run)-Methode für Ihre [Hintergrundaufgabe](../launch-resume/support-your-app-with-background-tasks.md) hinzu.

So können Sie beispielsweise die Benutzer Ihrer App, die kostenpflichtige Add-Ons gekauft haben, mit einem kostenlosen Add-On belohnen. In diesem Fall können Sie eine Pushbenachrichtigung an ein [Kundensegment](../publish/create-customer-segments.md) senden, die auf diese Benutzer ausgerichtet ist. Dann können Sie in einem der oben aufgeführten Einstiegspunkte Code hinzufügen, um diesen Kunden einen kostenlosen [In-App-Kauf](in-app-purchases-and-trials.md) zu gewähren.

## <a name="notify-partner-center-of-your-app-launch"></a>Benachrichtigen von Partner Center über Ihren app-Start

Wenn Sie die Option **App-Start Rate nachverfolgen** für Ihre gezielte Pushbenachrichtigung im Partner Center auswählen, rufen Sie die [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) -Methode vom entsprechenden Einstiegspunkt in Ihrer APP auf, um Partner Center zu benachrichtigen, dass Ihre APP als Reaktion auf eine Pushbenachrichtigung gestartet wurde.

Diese Methode gibt auch die ursprünglichen Startargumente für Ihre App zurück. Wenn Sie die APP-Start Rate für Ihre Pushbenachrichtigung nachverfolgen möchten, wird den Start Argumenten eine nicht transparente Überwachungs-ID hinzugefügt, um den App-Start in Partner Center zu verfolgen. Sie müssen die Start Argumente für Ihre APP an die [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) -Methode übergeben, und diese Methode sendet die Überwachungs-ID an Partner Center, entfernt die Überwachungs-ID aus den Launch-Argumenten und gibt die ursprünglichen Start Argumente an Ihren Code zurück.

Die Art und Weise, wie diese Methode aufgerufen wird, hängt vom Aktivierungs Typ der Pushbenachrichtigung ab:

* Hat die Pushbenachrichtigung einen Vordergrund-Aktivierungstyp, rufen Sie diese Methode aus der [OnActivated](/uwp/api/windows.ui.xaml.application.onactivated)-Methodenüberschreibung in Ihrer App auf, und übergeben Sie die Argumente, die im [ToastNotificationActivatedEventArgs](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)-Objekt verfügbar sind, an das Objekt, das an diese Methode übergeben wird. Im folgenden Codebeispiel wird davon ausgegangen, dass Ihre Codedatei **using**-Anweisungen für die Namespaces **Microsoft.Services.Store.Engagement** und **Windows.ApplicationModel.Activation** enthält.

  :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/App.xaml.cs" id="OnActivated":::

* Wenn die Pushbenachrichtigung einen Hintergrundaktivierungstyp hat, rufen Sie diese Methode aus der [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run)-Methode für Ihre [Hintergrundaufgabe](../launch-resume/support-your-app-with-background-tasks.md) auf, und übergeben Sie die Argumente, die im [ToastNotificationActionTriggerDetail](/uwp/api/Windows.UI.Notifications.ToastNotificationActionTriggerDetail)-Objekt verfügbar sind, das an diese Methode übergeben wird. Im folgenden Codebeispiel wird davon ausgegangen, dass Ihre Codedatei **using**-Anweisungen für die Namespaces **Microsoft.Services.Store.Engagement**, **Windows.ApplicationModel.Background** und **Windows.UI.Notifications** enthält.

  :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="Run":::

<span id="unregister" />

## <a name="unregister-for-push-notifications"></a>Aufheben der Registrierung für Pushbenachrichtigungen

Wenn Sie möchten, dass Ihre APP keine gezielten Pushbenachrichtigungen mehr von Partner Center empfängt, können Sie die [unregisternotificationchannelasync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) -Methode aufrufen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="UnregisterNotificationChannelAsync":::

Beachten Sie, dass diese Methode den Kanal, der für Benachrichtigungen verwendet wird, ungültig macht, damit die App keine Pushbenachrichtigungen *irgendwelcher* Dienste mehr empfängt. Nachdem Sie geschlossen wurde, kann der Kanal für keine Dienste erneut verwendet werden, einschließlich gezielter Pushbenachrichtigungen aus Partner Center und anderen Benachrichtigungen mithilfe von WNS. Damit wieder Pushbenachrichtigungen an diese App gesendet werden können, muss die App einen neuen Kanal anfragen.

## <a name="related-topics"></a>Verwandte Themen

* [Senden von Pushbenachrichtigungen an die Kunden Ihrer App](../publish/send-push-notifications-to-your-apps-customers.md)
* [Übersicht über die Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
* [So wird's gemacht: Anfordern, Erstellen und Speichern eines Benachrichtigungskanals](/previous-versions/windows/apps/hh868221(v=win.10))
* [Microsoft Store Services SDK](./microsoft-store-services-sdk.md)
