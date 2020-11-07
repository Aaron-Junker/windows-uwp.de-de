---
title: Freigeben von „Meine Kontakte”
description: Verwenden Sie meine Personen Freigaben, damit Benutzer Kontakte an Ihre Taskleiste anheften und problemlos von jedem beliebigen Ort in Windows aus in Kontakt treten können.
ms.date: 06/28/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8cbf82592e91b82e2d9d34d116d00aecf2ddd021
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339408"
---
# <a name="my-people-sharing"></a>Freigeben von „Meine Kontakte”

Mit der Funktion "meine Personen" können Benutzer Kontakte an Ihre Taskleiste anheften, sodass Sie von überall in Windows problemlos kontaktiert werden können, unabhängig von der Anwendung, mit der Sie verbunden sind. Nun können Benutzerinhalte mit ihren angehefteten Kontakten freigeben, indem Sie Dateien aus dem Datei-Explorer auf die PIN "meine Personen" ziehen. Sie können auch über den Charm "Standardteilen" für alle Kontakte im Windows-Kontakt Speicher freigeben. Lesen Sie weiter, um zu erfahren, wie Sie Ihre Anwendung als Freigabe Ziel für meine Personen aktivieren.

![Freigabe Bereich "meine Personen"](images/my-people-sharing.png)

## <a name="requirements"></a>Anforderungen

+ Windows 10 und Microsoft Visual Studio 2019. Weitere Informationen zur Installation finden Sie unter [Einrichten von Visual Studio](/windows/apps/get-started/get-set-up).
+ Grundkenntnisse in C# oder einer ähnlichen objektorientierten Programmiersprache. Informationen zu den ersten Schritten mit c# finden Sie unter [Erstellen einer "Hello, World"-App](../get-started/create-a-hello-world-app-xaml-universal.md).

## <a name="overview"></a>Übersicht

Es gibt drei Schritte, die Sie durchführen müssen, um Ihre Anwendung als Freigabe Ziel für meine Personen zu aktivieren:

1. [Deklarieren Sie die Unterstützung für den sharetarget-Aktivierungs Vertrag in Ihrem Anwendungs Manifest.](#declaring-support-for-the-share-contract)
2. [Kommentieren Sie die Kontakte, die die Benutzer für die Verwendung Ihrer APP freigeben können.](#annotating-contacts)
3. Unterstützung mehrerer Instanzen der Anwendung, die gleichzeitig ausgeführt werden.  Benutzer müssen in der Lage sein, mit einer vollständigen Version Ihrer Anwendung zu interagieren, während Sie Sie auch für andere Benutzer freigeben können. Sie können Sie in mehreren Freigabe Fenstern gleichzeitig verwenden. Um dies zu unterstützen, muss Ihre Anwendung in der Lage sein, mehrere Sichten gleichzeitig auszuführen. Weitere Informationen hierzu finden Sie im Artikel ["Anzeigen mehrerer Ansichten für eine App"](../design/layout/show-multiple-views.md).

Wenn Sie dies abgeschlossen haben, wird die Anwendung als Freigabe Ziel im Freigabe Fenster "meine Personen" angezeigt, das auf zwei Arten gestartet werden kann:
1. Ein Kontakt wird über den Charm "teilen" ausgewählt.
2. Dateien werden per Drag und Drop an einem Kontakt abgelegt, der an die Taskleiste angeheftet wurde.

## <a name="declaring-support-for-the-share-contract"></a>Deklarieren der Unterstützung für den Freigabe Vertrag

Um die Unterstützung für Ihre Anwendung als Freigabe Ziel zu deklarieren, öffnen Sie zuerst die Anwendung in Visual Studio. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Package. appxmanifest** , und wählen Sie **Öffnen mit** aus. Wählen Sie im Menü **XML (Text)-Editor** aus, und klicken Sie auf **OK**. Nehmen Sie dann die folgenden Änderungen am Manifest vor:


**Vorher**
```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
    </Application>
</Applications>
```

**Nachher**

```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
        <Extensions>
            <uap:Extension Category="windows.shareTarget">
                <uap:ShareTarget Description="Share with MyApp">
                    <uap:SupportedFileTypes>
                        <uap:SupportsAnyFileType/>
                    </uap:SupportedFileTypes>
                    <uap:DataFormat>Text</uap:DataFormat>
                    <uap:DataFormat>Bitmap</uap:DataFormat>
                    <uap:DataFormat>Html</uap:DataFormat>
                    <uap:DataFormat>StorageItems</uap:DataFormat>
                    <uap:DataFormat>URI</uap:DataFormat>
                </uap:ShareTarget>
            </uap:Extension>
         </Extensions>
    </Application>
</Applications>
```

Dieser Code bietet Unterstützung für alle Dateien und Datenformate, Sie können jedoch angeben, welche Dateitypen und Datenformate unterstützt werden (Weitere Informationen finden Sie in der [Dokumentation zur sharetarget-Klasse](/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget) ).

## <a name="annotating-contacts"></a>Kommentieren von Kontakten

Damit das Fenster "meine Personen freigeben" Ihre Anwendung als Freigabe Ziel für Ihre Kontakte anzeigen kann, müssen Sie Sie in den Windows-Kontakt Speicher schreiben. Informationen zum Schreiben von Kontakten finden Sie unter Beispiel für die [Integration der Kontaktkarte](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration). 

Damit Ihre Anwendung bei der Freigabe an einen Kontakt als "meine Personen"-Freigabe Ziel angezeigt wird, muss Sie eine Anmerkung in diesen Kontakt schreiben. Anmerkungen sind Daten aus Ihrer Anwendung, die mit einem Kontakt verknüpft sind. Die-Anmerkung muss die aktivierbare Klasse enthalten, die der gewünschten Ansicht in Ihrem **providerproperties** -Member entspricht, und die Unterstützung für den **Freigabe** Vorgang deklarieren.

Sie können Kontakte jederzeit mit Anmerkungen versehen, während Ihre APP ausgeführt wird. in der Regel sollten Sie Kontakte kommentieren, sobald Sie dem Windows-Kontakt Speicher hinzugefügt werden.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and Share support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactShareAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations::Share;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation);
}
```

Die "AppID" ist der Paket Familienname, gefolgt von "!". und die aktivierbare Klassen-ID. Um den Paket Familiennamen zu ermitteln, öffnen Sie **Package. appxmanifest** mit dem Standard-Editor, und suchen Sie auf der Registerkarte "Packaging" (Verpacken). Hier ist "App" die aktivierbare Klasse, die der Freigabe Ziel Ansicht entspricht.

## <a name="running-as-a-my-people-share-target"></a>Ausführen als Freigabe Ziel "meine Personen"

Zum Schluss müssen Sie zum Ausführen der APP die [onsharetargetaktivierte](/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_) -Methode in der Hauptklasse Ihrer APP überschreiben, um die Freigabe Ziel Aktivierung zu verarbeiten. Die Eigenschaft " [sharetargetactivatedeventargs. shareoperation. Contacts](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation#Properties) " enthält die Kontakte, die für freigegeben werden, oder ist leer, wenn es sich um einen Standard Freigabe Vorgang (nicht um eine eigene Freigabe) handelt.

```Csharp
protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isPeopleShare = false;
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
    {
        // Make sure the current OS version includes the My People feature before
        // accessing the ShareOperation.Contacts property
        isPeopleShare = (args.ShareOperation.Contacts.Count > 0);
    }

    if (isPeopleShare)
    {
        // Show share UI for MyPeople contact(s)
    }
    else
    {
        // Show standard share UI for unpinned contacts
    }
}
```

## <a name="see-also"></a>Weitere Informationen
+ [Unterstützung für meine Personen hinzufügen](my-people-support.md)
+ [Sharetarget-Klasse](/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)
+ [Beispiel für eine Kontaktkarten Integration](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)