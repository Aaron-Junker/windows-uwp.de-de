---
title: Support für Meine Kontakte zu einer Anwendung hinzufügen
description: Erläutert, wie Sie einer Anwendung Support für „Meine Kontakte” hinzufügen, und wie Sie Kontakte auf der Startseite anheften und entfernen
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f54cb261f6ef94545d656d5bd4f624622cc6dfff
ms.sourcegitcommit: dafda665fd3d25136194e452e7500b5bab076638
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2019
ms.locfileid: "71982230"
---
# <a name="adding-my-people-support-to-an-application"></a>Support für Meine Kontakte zu einer Anwendung hinzufügen

> [!Note]
> Ab dem Windows 10-Update von Mai 2019 (Version 1903) werden in neuen Windows 10-Installationen standardmäßig "Benutzer in der Taskleiste" nicht mehr angezeigt. Kunden können die Funktion aktivieren, indem Sie mit der rechten Maustaste auf die Taskleiste klicken und "Benutzer auf der Taskleiste anzeigen" drücken. Entwickler werden davon abgeraten, meine Personen Unterstützung zu Ihren Anwendungen hinzuzufügen. Weitere Informationen zum Optimieren von Apps für Windows 10 finden Sie im [Windows Developer-Blog](https://blogs.windows.com/windowsdeveloper/) .

Die Feature „Meine Kontakte” ermöglicht Benutzern das Anheften von Kontakten an die Taskleiste, um neue Kontaktobjekt zu erstellen, die auf unterschiedliche Weise miteinander interagieren können. In diesem Artikel wird gezeigt, wie Sie Support für dieses Feature hinzufügen können, damit Benutzer Kontakte direkt in der App anheften können. Wenn Kontakte angeheftet sind, werden neue Arten an Benutzerinteraktionen verfügbar, z. B. [Meine Kontakte freigeben](my-people-sharing.md) und [Benachrichtigungen ](my-people-notifications.md).

![Chat für Meine Kontakte](images/my-people-chat.png)

## <a name="requirements"></a>Anforderungen

+ Windows 10 und Microsoft Visual Studio 2019. Ausführliche Informationen zur Installation finden Sie unter [Visual Studio einrichten](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ Sie sollten über Grundkenntnisse in C# oder einer ähnlichen objektorientierten Programmiersprache verfügen. Die ersten Schritte mit C# finden Sie unter [Erstellen der App „Hello, world“ (C++)](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="overview"></a>Übersicht

Es gibt drei Dinge, die durchgeführt werden müssen, damit die Anwendung das Feature„ Meine Kontakte” verwenden kann:

1. [Deklarieren Sie die Unterstützung für den sharetarget-Aktivierungs Vertrag in Ihrem Anwendungs Manifest.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#declaring-support-for-the-share-contract)
2. [Kommentieren Sie die Kontakte, die die Benutzer für die Verwendung Ihrer APP freigeben können.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#annotating-contacts)
3.  Unterstützen Sie mehrere Instanzen Ihrer Anwendung, die zur gleichen Zeit ausgeführt werden. Benutzer müssen mit einer vollständigen Version Ihrer Anwendung interagieren, während Sie diese im Kontaktbereich verwenden.  Sie können diese sogar in mehreren Kontaktlisten gleichzeitig verwenden.  Um dies zu unterstützen, muss Ihre Anwendung mehrere Ansichten gleichzeitig ausführen können. Weitere Informationen hierzu finden Sie im Artikel ["Anzeigen mehrerer Ansichten für eine App"](https://docs.microsoft.com/windows/uwp/design/layout/show-multiple-views).

Wenn Sie dies getan haben, wird die Anwendung in der Kontaktliste für Kontakte mit Kommentaren angezeigt.

## <a name="declaring-support-for-the-contract"></a>Deklarieren von Support für den Vertrag

Um Unterstützung für den Vertrag „Meine Kontakte” zu deklarieren, öffnen Sie die Anwendung in Visual Studio. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Datei **Package.appxmanifest** und wählen Sie **Öffnen mit** aus. Wählen Sie aus dem Menü anschließend **XML (Text)-Editor**, und klicken Sie auf **OK**. Nehmen Sie danach folgenden Änderungen am Manifest vor.

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

Durch diesen Zusatz kann Ihre Anwendung jetzt über den Vertrag **Windows. ContactPanel** gestartet werden, die Sie mit mit Kontakten in der Kontaktliste interagieren können.

## <a name="annotating-contacts"></a>Kontakte mit Kommentaren versehen

Um Kontakte von Ihrer Anwendung über den Bereich „Meine Kontakte” in der Taskleiste anzuzueigen, müssen Sie diese auf den Windows-Kontaktspeicher schreiben.  Weitere Informationen zum Schreiben von Kontakten finden Sie unter [Beispiel für die Visitenkarte](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration).

Ihre Anwendung muss ebenfalls eine Anmerkung für jeden Kontakt schreiben. Kommentare sind Datenteile von Ihrer Anwendung, die mit einem Kontakt verknüpft sind. Die Anmerkung muss die aktivierbare Klassen für die gewünschte Ansicht in seinem Element **ProviderProperties** und Support für den **ContactProfile**-Vorgang deklarieren.

Sie können Kontakten jederzeit einen Kommentar hinzufügen, während die App ausgeführt wird, aber in der Regel sollten Sie Kontakte sofort kommentieren, wenn sie dem Windows-Kontaktspeicher hinzugefügt werden.

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

Die “appId” ist der Paketfamilienname, gefolgt von ‘!’ und einer aktivierbaren Klassen-ID. Um den Paketfamiliennamen zu such, öffnen Sie **Package.appxmanifest** mithilfe des Standard-Editors und suchen Sie auf der Registerkarte "Verpacken". Hier ist "App" die aktivierbare Klasse, die der Anwendungsstart-Ansicht entspricht.

## <a name="allow-contacts-to-invite-new-potential-users"></a>Kontakten die Einladung von neuen potenziellen Benutzern ermöglichen

Standardmäßig wird die Anwendung nur für Kontakte in der Kontaktliste angezeigt, die Sie speziell kommentiert haben.  Dies vermeidet eine Verwechslungen mit Kontakten, die nicht mit Ihnen durch die App interagiert können.  Wenn Sie möchten, dass Ihre Anwendung für Kontakte angezeigt wird, deren Ihre Anwendung nicht bekannt ist (z. B. um Benutzer dazu aufzufordern, ihrem Konto Kontakte hinzuzufügen), können Sie Ihrem Manifest Folgendes hinzufügen:

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

Aufgrund dieser Änderung wird die Anwendung als verfügbare Option für alle Kontakte in der Kontaktliste angezeigt, die der Benutzer angeheftet hat.  Wenn Ihre Anwendung durch den Kontaktlisten-Vertrag aktiviert ist, sollten Sie überprüfen, ob der Kontakt Ihrer Anwendung bereits bekannt ist.  Wenn dies nicht der Fall ist, sollten Sie die Benutzerfreundlichkeit Ihrer App anzeigen.

![„Meine Kontakte”-Kontaktliste](images/my-people.png)

## <a name="support-for-email-apps"></a>Unterstützung für E-Mail-Apps

Wenn Sie eine E-Mail-App schreiben, müssen Sie nicht jeden Kontakt manuell kommentieren.  Wenn Sie Unterstützung für die Kontaktliste und das „mailto: protocol” deklarieren, erscheint Ihre Anwendung automatisch für Benutzer mit einer E-Mail-Adresse.

## <a name="running-in-the-contact-panel"></a>Ausführen in der Kontaktliste

Jetzt, wo Ihre Anwendung in der Kontaktliste für einige oder alle Benutzer angezeigt wird, müssen Sie diese mit dem Kontaktlisten-Vertrag aktivieren.

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

Wenn Ihre Anwendung mit diesem Vertrag aktiviert ist, erhält sie ein [ContactPanelActivatedEventArgs-Objekt](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.activation.contactpanelactivatedeventargs).  Dieses enthält die ID des Kontakts, mit dem Ihre Anwendung zu interagieren versucht und ein [ContactPanel](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactpanel)-Objekt. Sie sollten einen Verweis auf dieses ContactPanel-Objekt beibehalten, der die Interaktion mit der Liste ermöglicht.

Das ContactPanel-Objekt verfügt über zwei Ereignisse, auf die Ihre Anwendung hören soll:
+ Das **LaunchFullAppRequested**-Ereignis wird gesendet, wenn der Benutzer das Element der Benutzeroberfläche aufgerufen hat, das anfordert, dass die vollständige Anwendung in einem eigenen Fenster gestartet wird.  Ihre Anwendung ist für ihren Start verantwortlich und übergibt dabei alle erforderlichen Kontexte.  Sie können nach Ihren eigenen Wünschen durchführen (z. B. per Protokollstart).
+ Die **Closing-Ereignis** wird gesendet, wenn Ihre Anwendung im Begriff ist, beendet zu werden, sodass Sie den Kontext speichern können.

Mit dem ContactPanel-Objekt können Sie ebenfalls die Hintergrundfarbe des Headers der Kontaktliste festlegen (wenn er nicht festgelegt ist, wird das Systemdesign standardmäßig verwendet) und die Kontaktliste programmgesteuert schließen.

## <a name="supporting-notification-badging"></a>Unterstützung von Benachrichtigungssignalen

Wenn Sie möchten, dass auf der Taskleiste angeheftete Kontakte ein Signal erhalten, wenn neue Benachrichtigungen von Ihrer App für diese Person eintreffen, müssen die **Hint-Personen**-Parameter in den [Popupbenachrichtigungen](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts) und [Meine Kontakte – Benachrichtigungen](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-notifications) einbezogen werden.

![Benachrichtigung für „Meine Kontakte”](images/my-people-badging.png)

Um einen Kontakt zu benachrichtigen, muss der Knoten der obersten Ebene Popups hint-people-Parameter enthalten, um den Absender-Kontakt oder andere Kontakte anzugeben. Dieser Parameter kann folgende Werte haben:
+ **E-Mail Adresse** 
    + Beispiel: [mailto:johndoe@mydomain.com](mailto:johndoe@mydomain.com)
+ **Telefonnummer** 
    + Beispiel: Tel:888-888-8888
+ **Remote-ID** 
    + Beispiel: remoteid:1234

Hier ist ein Beispiel, wie Sie eine Popupbenachrichtigung für eine bestimmte Person identifizieren:
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
> Falls Ihre App die [ContactStore APIs](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactstore) verwendet und auf dem Smartphone gespeicherte Kontakte mithilfe der [StoredContact.RemoteId](https://docs.microsoft.com/en-us/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId)-Eigenschaft mit remote gespeicherten Kontakten verknüpft, muss der Wert für die RemoteId-Eigenschaft unbedingt stabil und eindeutig sein. Die Remote-ID muss also durchweg ein einzelnes Benutzerkonto identifizieren und ein eindeutiges Tag enthalten, um zu verhindern, dass sich Konflikte mit den Remote-IDs anderer Kontakte auf dem PC ergeben. Hierzu zählen auch Kontakte von anderen Apps.
> Falls die Stabilität und Eindeutigkeit der von Ihrer App verwendeten Remote-IDs nicht gewährleistet ist, können Sie allen Ihren RemoteIdHelper mithilfe der später in diesem Thema beschriebenen -Klasse ein eindeutiges Tag hinzufügen, bevor Sie die Remote-IDs dem System hinzufügen. Alternativ können Sie auch ganz auf die Verwendung der RemoteId-Eigenschaft verzichten und stattdessen eine benutzerdefinierte erweiterte Eigenschaft erstellen, um die Remote-IDs für Ihre Kontakte zu speichern.

## <a name="the-pinnedcontactmanager-class"></a>Die PinnedContactManager-Klasse

Der [PinnedContactManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager) verwaltet, welche Kontakte an die Taskleiste angeheftet werden. Mit dieser Klasse können Sie Kontakte anheften und lösen, bestimmen, ob der Kontakt hinzugefügt wird und ob das Anheften auf eine bestimmte Fläche vom System unterstützt wird, auf dem die Anwendung zurzeit ausgeführt wird.

Sie können das Objekt mit PinnedContactManager mithilfe der **GetDefault**- Methode abrufen:

```Csharp
PinnedContactManager pinnedContactManager = PinnedContactManager.GetDefault();
```

## <a name="pinning-and-unpinning-contacts"></a>Anheften und Loslösen von Kontakten
Sie können jetzt Kontakte, die Sie gerade erstellt haben, über die PinnedContactManager anheften und lösen. Die **RequestPinContactAsync** und **RequestUnpinContactAsync**-Methoden bieten dem Benutzer Bestätigungsdialogfelder, damit sie vom Anwendungsthread für Single-Threaded Apartment (ASTA, or UI) aufgerufen werden müssen.

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

Sie können ebenfalls mehrere Kontakte gleichzeitig anheften:

```Csharp
async Task PinMultipleContacts(Contact[] contacts)
{
    await pinnedContactManager.RequestPinContactsAsync(
        contacts, PinnedContactSurface.Taskbar);
}
```

> [!Note]
> Zurzeit besteht keine Batchvorgang zum Lösen von Kontakten.

**Hinweis**: 

## <a name="see-also"></a>Siehe auch
+ [Freigeben von „Meine Kontakte”](my-people-sharing.md)
+ [Meine Personen benachrichtigt](my-people-notifications.md)
+ [Channel 9-Video zum Hinzufügen von Support für meine Personen zu einer Anwendung](https://channel9.msdn.com/Events/Build/2017/P4056)
+ [Beispiel für die Integration von meine Personen](https://aka.ms/mypeoplebuild2017)
+ [Beispiel für eine Kontaktkarte](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
+ [Dokumentation der pinnedcontactmanager-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.pinnedcontactmanager)
+ [Verbinden der App mit Aktionen auf einer Visitenkarte](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/integrating-with-contacts)
