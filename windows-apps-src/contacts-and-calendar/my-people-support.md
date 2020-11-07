---
title: Hinzufügen von Unterstützung für „Meine Kontakte“ zu einer Anwendung
description: Erläutert das Hinzufügen von Support für meine Personen zu einer Anwendung und das Anheften und lösen von Kontakten.
ms.date: 06/28/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8c0e174613357ad9e4e45d2776f3fbc618535b30
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339458"
---
# <a name="adding-my-people-support-to-an-application"></a>Hinzufügen von Unterstützung für „Meine Kontakte“ zu einer Anwendung

> [!Note]
> Ab dem Windows 10-Update von Mai 2019 (Version 1903) werden in neuen Windows 10-Installationen standardmäßig "Benutzer in der Taskleiste" nicht mehr angezeigt. Kunden können die Funktion aktivieren, indem Sie mit der rechten Maustaste auf die Taskleiste klicken und "Benutzer auf der Taskleiste anzeigen" drücken. Entwickler werden davon abgeraten, meine Personen Unterstützung zu Ihren Anwendungen hinzuzufügen. Weitere Informationen zum Optimieren von Apps für Windows 10 finden Sie im [Windows Developer-Blog](https://blogs.windows.com/windowsdeveloper/) .

Mit der Funktion "meine Personen" können Benutzer Kontakte aus einer Anwendung direkt an die Taskleiste anheften, wodurch ein neues Kontakt Objekt erstellt wird, mit dem Sie auf verschiedene Weise interagieren können. In diesem Artikel wird gezeigt, wie Sie Unterstützung für dieses Feature hinzufügen können, sodass Benutzer Kontakte direkt aus Ihrer APP anheften können. Wenn Kontakte fixiert werden, sind neue Arten von Benutzerinteraktionen verfügbar, wie z. b. die [Freigabe von Personen](my-people-sharing.md) und [Benachrichtigungen](my-people-notifications.md).

![Meine Leute chatten](images/my-people-chat.png)

## <a name="requirements"></a>Anforderungen

+ Windows 10 und Microsoft Visual Studio 2019. Weitere Informationen zur Installation finden Sie unter [Einrichten von Visual Studio](/windows/apps/get-started/get-set-up).
+ Grundkenntnisse in C# oder einer ähnlichen objektorientierten Programmiersprache. Informationen zu den ersten Schritten mit c# finden Sie unter [Erstellen einer "Hello, World"-App](../get-started/create-a-hello-world-app-xaml-universal.md).

## <a name="overview"></a>Übersicht

Es gibt drei Dinge, die Sie tun müssen, um Ihre Anwendung für die Verwendung der Funktion "meine Personen" zu aktivieren:

1. [Deklarieren Sie die Unterstützung für den sharetarget-Aktivierungs Vertrag in Ihrem Anwendungs Manifest.](./my-people-sharing.md#declaring-support-for-the-share-contract)
2. [Kommentieren Sie die Kontakte, die die Benutzer für die Verwendung Ihrer APP freigeben können.](./my-people-sharing.md#annotating-contacts)
3.  Unterstützung mehrerer Instanzen der Anwendung, die gleichzeitig ausgeführt werden. Benutzer müssen in der Lage sein, mit einer Vollversion der Anwendung zu interagieren, während Sie in einem Kontaktbereich verwendet wird.  Sie können Sie sogar in mehreren Kontakt Panels gleichzeitig verwenden.  Um dies zu unterstützen, muss Ihre Anwendung in der Lage sein, mehrere Sichten gleichzeitig auszuführen. Weitere Informationen hierzu finden Sie im Artikel ["Anzeigen mehrerer Ansichten für eine App"](../design/layout/show-multiple-views.md).

Wenn Sie dies abgeschlossen haben, wird die Anwendung im Kontaktbereich für mit Anmerkungen versehene Kontakte angezeigt.

## <a name="declaring-support-for-the-contract"></a>Deklarieren der Unterstützung für den Vertrag

Um die Unterstützung für den Vertrag "meine Personen" zu deklarieren, öffnen Sie die Anwendung in Visual Studio. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Package. appxmanifest** , und wählen Sie **Öffnen mit** aus. Wählen Sie im Menü die Option **XML (Text)-Editor)** aus, und klicken Sie auf **OK**. Nehmen Sie die folgenden Änderungen am Manifest vor:

**Vorher**

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10">

    <Applications>
        <Application Id="MyApp"
          Executable="$targetnametoken$.exe"
          EntryPoint="My.App">
        </Application>
    </Applications>

```

**Nachher**

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4">

    <Applications>
        <Application Id="MyApp"
          Executable="$targetnametoken$.exe"
          EntryPoint="My.App">
          <Extensions>
            <uap4:Extension Category="windows.contactPanel" />
          </Extensions>
        </Application>
    </Applications>

```

Mit dieser Addition kann die Anwendung jetzt über das Fenster gestartet werden **. Contactpanel** -Vertrag, der Ihnen die Interaktion mit Kontakt Panels ermöglicht.

## <a name="annotating-contacts"></a>Kommentieren von Kontakten

Damit Kontakte aus Ihrer Anwendung in der Taskleiste über den Bereich meine Personen angezeigt werden, müssen Sie Sie in den Windows-Kontakt Speicher schreiben.  Informationen zum Schreiben von Kontakten finden Sie im Beispiel für eine [Kontaktkarte](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration).

Die Anwendung muss auch eine Anmerkung für jeden Kontakt schreiben. Anmerkungen sind Daten aus Ihrer Anwendung, die mit einem Kontakt verknüpft sind. Die-Anmerkung muss die aktivierbare Klasse enthalten, die der gewünschten Ansicht in Ihrem **providerproperties** -Member entspricht, und die Unterstützung für den **contactprofile** -Vorgang deklarieren.

Sie können Kontakte jederzeit mit Anmerkungen versehen, während Ihre APP ausgeführt wird. in der Regel sollten Sie Kontakte kommentieren, sobald Sie dem Windows-Kontakt Speicher hinzugefügt werden.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and contact panel support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactPanelAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations.ContactProfile;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation));
}
```

Die "AppID" ist der Paket Familienname, gefolgt von "!". und die aktivierbare Klassen-ID. Um den Paket Familiennamen zu ermitteln, öffnen Sie **Package. appxmanifest** mit dem Standard-Editor, und suchen Sie auf der Registerkarte "Packaging" (Verpacken). Hier ist "App" die aktivierbare Klasse, die der Anwendungsstart Ansicht entspricht.

## <a name="allow-contacts-to-invite-new-potential-users"></a>Kontakte zum Einladen neuer potenzieller Benutzer zulassen

Standardmäßig wird Ihre Anwendung nur im Kontaktbereich für Kontakte angezeigt, die Sie speziell mit Anmerkungen versehen haben.  Dies soll Verwirrung mit Kontakten vermeiden, mit denen Sie über Ihre APP interagieren können.  Wenn Sie möchten, dass Ihre Anwendung für Kontakte angezeigt wird, die Ihre Anwendung nicht kennt (zum Beispiel, um Benutzer zum Hinzufügen dieses Kontakts zu Ihrem Konto einzuladen), können Sie dem Manifest Folgendes hinzufügen:

**Vorher**

```Csharp
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
      <Extensions>
        <uap4:Extension Category="windows.contactPanel" />
      </Extensions>
    </Application>
</Applications>
```

**Nachher**

```Csharp
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
      <Extensions>
        <uap4:Extension Category="windows.contactPanel">
            <uap4:ContactPanel SupportsUnknownContacts="true" />
        </uap4:Extension>
      </Extensions>
    </Application>
</Applications>
```

Mit dieser Änderung wird Ihre Anwendung als verfügbare Option im Kontakt Panel für alle Kontakte angezeigt, die der Benutzer angeheftet hat.  Wenn Ihre Anwendung mit dem Kontakt Bereichs Vertrag aktiviert ist, sollten Sie überprüfen, ob der Kontakt von Ihrer Anwendung bekannt ist.  Wenn nicht, sollten Sie die neue Benutzerfunktion ihrer App anzeigen.

![„Meine Kontakte”-Kontaktliste](images/my-people.png)

## <a name="support-for-email-apps"></a>Unterstützung für e-Mail-apps

Wenn Sie eine e-Mail-app schreiben, müssen Sie nicht jeden Kontakt manuell mit einer Anmerkung versehen.  Wenn Sie die Unterstützung für den Kontaktbereich und für das mailto:-Protokoll deklarieren, wird Ihre Anwendung automatisch für Benutzer mit einer e-Mail-Adresse angezeigt.

## <a name="running-in-the-contact-panel"></a>Ausführen im Kontakt Panel

Nun, da Ihre Anwendung für einige oder alle Benutzer im Kontaktbereich angezeigt wird, müssen Sie die Aktivierung mit dem Kontakt Bereichs Vertrag verarbeiten.

```Csharp
override protected void OnActivated(IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.ContactPanel)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        var rootFrame = new Frame();

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        // Navigate to the page that shows the Contact UI.
        rootFrame.Navigate(typeof(ContactPage), e);

        // Ensure the current window is active
        Window.Current.Activate();
    }
}
```

Wenn Ihre Anwendung mit diesem Vertrag aktiviert wird, empfängt Sie ein [contactpanelactivatedeventargs-Objekt](/uwp/api/windows.applicationmodel.activation.contactpanelactivatedeventargs).  Diese enthält die ID des Kontakts, mit dem Ihre Anwendung beim Start zu interagieren versucht, und ein [contactpanel](/uwp/api/windows.applicationmodel.contacts.contactpanel) -Objekt. Sie sollten einen Verweis auf dieses contactpanel-Objekt behalten, das Ihnen die Interaktion mit dem Panel ermöglicht.

Das contactpanel-Objekt verfügt über zwei Ereignisse, auf die Ihre Anwendung lauschen sollte:
+ Das **launchfullapprequ-** Ereignis wird gesendet, wenn der Benutzer das UI-Element aufgerufen hat, das anfordert, dass Ihre vollständige Anwendung in einem eigenen Fenster gestartet wird.  Ihre Anwendung ist dafür verantwortlich, sich selbst zu starten und dabei den gesamten erforderlichen Kontext zu übergeben.  Sie können das tun, wenn Sie dies wünschen (z. b. über das Protokoll Start).
+ Das **schließende Ereignis** wird gesendet, wenn die Anwendung geschlossen wird, sodass Sie jeden Kontext speichern können.

Das contactpanel-Objekt ermöglicht Ihnen außerdem die Festlegung der Hintergrundfarbe für den Header des Kontakt Bereichs (wenn nicht festgelegt, wird standardmäßig das Systemdesign verwendet) und, um den Kontaktbereich Programm gesteuert zu schließen.

## <a name="supporting-notification-badging"></a>Unterstützen von Benachrichtigungs Badging

Wenn Sie möchten, dass Kontakte, die an die Taskleiste angeheftet werden, beim Eintreffen neuer Benachrichtigungen von Ihrer APP, die mit dieser Person verknüpft sind, gebadelt werden sollen, müssen Sie den **Hint-People-** Parameter in ihre Popup [Benachrichtigungen](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md) einschließen und die [Benachrichtigungen zu meinen Personen](./my-people-notifications.md)Ausdrücken.

![Personen Benachrichtigungs Badging](images/my-people-badging.png)

Um einen Kontakt zu übermitteln, muss der Popup Knoten der obersten Ebene den Hint-People-Parameter enthalten, um den Sende-oder verknüpften Kontakt anzugeben. Dieser Parameter kann einen der folgenden Werte aufweisen:
+ **E-Mail-Adresse** 
    + Beispiel: mailto:johndoe@mydomain.com
+ **Telefonnummer** 
    + Beispiel: Tel: 888-888-8888
+ **Remote-ID** 
    + Beispiel: RemoteID: 1234

Im folgenden finden Sie ein Beispiel dafür, wie Sie eine Popup Benachrichtigung mit einer bestimmten Person identifizieren können:
```XML
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastText01">
            <text>John Doe posted a comment.</text>
        </binding>
    </visual>
</toast>
```

> [!NOTE]
> Wenn Ihre APP die [contactstore-APIs](/uwp/api/windows.applicationmodel.contacts.contactstore) verwendet und die [storedcontact. RemoteID-](/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) Eigenschaft verwendet, um Kontakte zu verknüpfen, die auf dem PC mit den Remote gespeicherten Kontakten gespeichert sind, ist es von entscheidender Bedeutung, dass der Wert für die RemoteID-Eigenschaft stabil und eindeutig ist. Dies bedeutet, dass die Remote-ID ein einzelnes Benutzerkonto einheitlich identifizieren muss und ein eindeutiges Tag enthalten sollte, um sicherzustellen, dass es nicht mit den Remote-IDs anderer Kontakte auf dem PC in Konflikt steht, einschließlich der Kontakte, die sich im Besitz anderer apps befinden.
> Wenn die Remote-IDs, die von Ihrer APP verwendet werden, nicht sicher und eindeutig sind, können Sie die weiter unten in diesem Thema gezeigte remoteidhelper-Klasse verwenden, um allen Remote-IDs ein eindeutiges Tag hinzuzufügen, bevor Sie Sie dem System hinzufügen. Oder Sie können auswählen, dass die RemoteID-Eigenschaft überhaupt nicht verwendet werden soll. stattdessen erstellen Sie eine benutzerdefinierte erweiterte Eigenschaft, in der Remote-IDs für Ihre Kontakte gespeichert werden.

## <a name="the-pinnedcontactmanager-class"></a>Die pinnedcontactmanager-Klasse

Der [pinnedcontactmanager](/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager) wird verwendet, um zu verwalten, welche Kontakte an die Taskleiste angeheftet werden. Mit dieser Klasse können Sie Kontakte anheften und lösen, feststellen, ob ein Kontakt angeheftet ist, und feststellen, ob das Fixieren auf einer bestimmten Oberfläche von dem System unterstützt wird, auf dem die Anwendung zurzeit ausgeführt wird.

Sie können das pinnedcontactmanager-Objekt mit der **GetDefault** -Methode abrufen:

```Csharp
PinnedContactManager pinnedContactManager = PinnedContactManager.GetDefault();
```

## <a name="pinning-and-unpinning-contacts"></a>Anhepten und lösen von Kontakten
Sie können jetzt Kontakte mit dem soeben erstellten pinnedcontactmanager anheften und lösen. Die Methoden " **requestpincontactasync** " und " **requestunpincontactasync** " stellen dem Benutzer Bestätigungs Dialogfelder bereit, sodass Sie von Ihrer Anwendung aufgerufen werden müssen, Single-Threaded Apartment-(ASTA-oder UI-) Thread.

```Csharp
async void PinContact (Contact contact)
{
    await pinnedContactManager.RequestPinContactAsync(contact,
                                                      PinnedContactSurface.Taskbar);
}

async void UnpinContact (Contact contact)
{
    await pinnedContactManager.RequestUnpinContactAsync(contact,
                                                        PinnedContactSurface.Taskbar);
}
```

Sie können auch mehrere Kontakte gleichzeitig anheften:

```Csharp
async Task PinMultipleContacts(Contact[] contacts)
{
    await pinnedContactManager.RequestPinContactsAsync(
        contacts, PinnedContactSurface.Taskbar);
}
```

> [!Note]
> Zurzeit ist kein Batch Vorgang zum Lösen von Kontakten vorhanden.

**Hinweis:** 

## <a name="see-also"></a>Weitere Informationen
+ [Freigeben von „Meine Kontakte”](my-people-sharing.md)
+ [Meine Personen benachrichtigt](my-people-notifications.md)
+ [Channel 9-Video zum Hinzufügen von Support für meine Personen zu einer Anwendung](https://channel9.msdn.com/Events/Build/2017/P4056)
+ [Beispiel für die Integration von meine Personen](https://github.com/tonyPendolino/MyPeopleBuild2017)
+ [Beispiel für eine Kontaktkarte](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
+ [Dokumentation der pinnedcontactmanager-Klasse](/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager)
+ [Verbinden der App mit Aktionen auf einer Visitenkarte](./integrating-with-contacts.md)