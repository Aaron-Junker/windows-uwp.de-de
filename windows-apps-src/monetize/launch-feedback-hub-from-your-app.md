---
Description: Sie können Ihre Kunden dazu ermutigen, Feedback zu geben, indem Sie den Feedback-Hub über Ihre App starten.
title: Starten des Feedback-Hubs über Ihre App
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Feedback-Hub, Starten
ms.localizationpriority: medium
ms.openlocfilehash: 4190c8af5c8cb7db6b80b1149dff631a8454015b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371069"
---
# <a name="launch-feedback-hub-from-your-app"></a>Starten des Feedback-Hubs über Ihre App

Sie können Ihre Kunden dazu ermutigen, Feedback zu geben, indem Sie ein Steuerelement (wie beispielsweise eine Schaltfläche) zu Ihrer UWP-App (Universelle Windows-Plattform) hinzufügen, durch das der Feedback-Hub gestartet wird. Beim Feedback-Hub handelt es sich um eine vorinstallierte App, in der an einem zentralen Ort Feedback zu Windows und den installierten Apps gesammelt werden kann. Alle Feedback von Kunden, die gesendet wird, für Ihre app über Feedback Hub-Instanz gesammelt und, die Sie in angezeigt der [Feedback Bericht](../publish/feedback-report.md) im Partner Center daher sehen Sie die Probleme, Vorschläge und Upvotes, die Ihre Kunden in einem Bericht übermittelt wird.

Um den Feedback-Hub über Ihre App zu starten, verwenden Sie eine API, die vom [Microsoft Store Services SDK](https://aka.ms/store-em-sdk) bereitgestellt wird. Es wird die Verwendung dieser API zum Starten des Feedback-Hubs über ein Benutzeroberflächenelement in Ihrer App empfohlen, das unseren Designrichtlinien entspricht.

> [!NOTE]
> Der Feedback-Hub ist nur auf Geräten verfügbar, auf denen Version 10.0.14271 oder höher von Windows 10 für die Desktop- und Mobil-[Gerätefamilien](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) ausgeführt wird. Es wird empfohlen, ein Feedback-Steuerelement in Ihrer App nur dann einzublenden, wenn der Feedback-Hub auf dem Gerät des Benutzers verfügbar ist. Mithilfe des Codes in diesem Thema wird die Vorgehensweise veranschaulicht.

## <a name="how-to-launch-feedback-hub-from-your-app"></a>So starten Sie den Feedback-Hub über Ihre App

So starten Sie den Feedback-Hub über Ihre App

1. [Installieren Sie das Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk).
2. Öffnen Sie das Projekt in Visual Studio.
3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Knoten **Verweise** für Ihr Projekt, und wählen Sie anschließend **Verweis hinzufügen** aus.
4. Erweitern Sie im **Verweis-Manager** die Option **Universelle Windows-App**, und klicken Sie auf **Erweiterungen**.
5. Klicken Sie in der Liste der SDKs auf das Kontrollkästchen neben **Microsoft Engagement Framework** und anschließend auf **OK**.
6. Fügen Sie in Ihrem Projekt das Steuerelement hinzu, das Sie für Benutzer anzeigen lassen möchten, um den Feedback-Hub zu starten, z. B. eine Schaltfläche. Wir empfehlen Ihnen folgende Konfiguration für das Steuerelement:
  * Legen Sie die Schriftart der Inhalte im Steuerelement auf **Segoe MDL2 Assets** fest.
  * Legen Sie den Text im Steuerelement auf das hexadezimale Unicode-Zeichen E939 fest. Dabei handelt es sich um den Zeichencode für das empfohlene Feedbacksymbol in der Schriftart **Segoe MDL2 Assets**.
  * Legen Sie bezüglich der Sichtbarkeit des Steuerelements fest, dass es ausgeblendet wird.
    > [!NOTE]
    > Wir empfehlen Ihnen, das Feedback-Steuerelement standardmäßig auszublenden und es in Ihrem Initialisierungscode nur dann anzuzeigen, wenn der Feedback-Hub auf dem Gerät des Benutzers verfügbar ist. Im nächsten Schritt wird die Vorgehensweise erläutert.

    Der folgende Code veranschaulicht die XAML-Definition einer [Schaltfläche](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button), die wie oben beschrieben konfiguriert ist.

    ```XML
    <Button x:Name="feedbackButton" FontFamily="Segoe MDL2 Assets" Content="&#xE939;" HorizontalAlignment="Left" Margin="138,352,0,0" VerticalAlignment="Top" Visibility="Collapsed"  Click="feedbackButton_Click"/>
    ```

7. Verwenden Sie im Initialisierungscode der App-Seite, auf der Ihr Feedback-Steuerelement gehostet wird, die statische [IsSupported](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher.issupported)-Methode der [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher)-Klasse, um zu ermitteln, ob der Feedback-Hub auf dem Gerät des Benutzers verfügbar ist. Der Feedback-Hub ist nur auf Geräten verfügbar, auf denen Version 10.0.14271 oder höher von Windows 10 für die Desktop- und Mobil-[Gerätefamilien](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) ausgeführt wird.

    Wenn diese Eigenschaft **true** zurückgibt, legen Sie fest, dass das Steuerelement sichtbar wird. Der folgende Code veranschaulicht die Vorgehensweise für eine [Schaltfläche](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button).

    [!code-csharp[LaunchFeedback](./code/StoreSDKSamples/cs/FeedbackPage.xaml.cs#ToggleFeedbackVisibility)]
      > [!NOTE]
      > Obwohl der Feedback-Hub zurzeit auf Xbox-Geräten nicht unterstützt wird, gibt die Eigenschaft **IsSupported** zurzeit **true** auf Xbox-Geräten aus, auf denen Windows 10, Version 10.0.14271 oder höher ausgeführt wird. Dies ist ein bekanntes Problem, das in einer zukünftigen Version des Microsoft Store Services SDK behoben sein wird.  

8. Rufen Sie im Ereignishandler, der ausgeführt wird, wenn der Benutzer auf das Steuerelement klickt, ein [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher)-Objekt ab, und rufen Sie die [LaunchAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher.launchasync)-Methode auf, um die Feedback-Hub-App zu starten. Es gibt zwei Überladungen für diese Methode: eine ohne Parameter und eine weitere, die ein Wörterbuch mit Schlüssel-Wert-Paaren akzeptiert, die wiederum Metadaten enthalten, die Sie mit dem Feedback verknüpfen möchten. Im folgenden Beispiel wird gezeigt, wie Sie den Feedback-Hub im [Click](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignishandler für eine [Schaltfläche](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) starten.

    [!code-csharp[LaunchFeedback](./code/StoreSDKSamples/cs/FeedbackPage.xaml.cs#FeedbackButtonClick)]

## <a name="design-recommendations-for-your-feedback-ui"></a>Designempfehlungen für Ihre Feedback-Benutzeroberfläche

Um den Feedback-Hub zu starten, empfehlen wir, in Ihrer App ein Benutzeroberflächenelement (z. B. eine Schaltfläche) hinzuzufügen, in dem das folgende standardmäßige Feedbacksymbol mit der Schriftart „Segoe MDL2 Assets“ und dem Zeichencode E939 angezeigt wird.

![Feedbacksymbol](images/feedback_icon.PNG)

Außerdem wird empfohlen, mindestens eine der folgenden Platzierungsoptionen für die Verknüpfung mit dem Feedback-Hub in Ihrer App zu verwenden.
* **Direkt in der App-Leiste** Je nach Implementierung möchten Sie möglicherweise nur das Symbol verwenden oder Text hinzufügen (siehe unten).

  ![Feedbacksymbol](images/feedback_appbar_placement.png)

* **In den Einstellungen Ihrer App**. Dies ist eine subtilere Art, um Benutzern Zugriff auf den Feedback-Hub zu gewähren. Im folgenden Beispiel wird der Feedbacklink als einer der Links unter der App angezeigt.

  ![Feedbacksymbol](images/feedback_settings_placement.png)

* **In einem ereignisgesteuerten Flyout**. Dies ist hilfreich, wenn Sie vor dem Start des Windows-Feedback-Hubs die Meinung Ihrer Kunden zu einer bestimmten Frage erfahren möchten. Beispiel: Nachdem Ihre App eine bestimmte Funktion verwendet, könnten Sie den Kunden mit einer gezielten Frage zu seiner Zufriedenheit mit diesem Feature dazu auffordern, Ihnen seine Meinung mitzuteilen. Wenn der Kunde auf die Frage reagieren möchte, wird über Ihre App der Feedback-Hub gestartet.


## <a name="related-topics"></a>Verwandte Themen

* [Feedbackbericht](../publish/feedback-report.md)
