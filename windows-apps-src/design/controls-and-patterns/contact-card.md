---
description: Erfahren Sie, wie Sie Visitenkarten verwenden, damit Benutzer Kontaktinformationen wie Namen, Telefonnummern und Adressen anzeigen und bearbeiten können.
title: Visitenkarte
ms.date: 03/07/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kele
design-contact: tbd
dev-contact: tbd
doc-status: not-published
ms.localizationpriority: medium
ms.openlocfilehash: 2817977533b63df8498faa1ecbc5cc57a4987c30
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160594"
---
# <a name="contact-card"></a>Visitenkarte

Die Visitenkarte umfasst Kontaktinformationen wie Name, Telefonnummer und Adresse für einen [Kontakt](/uwp/api/Windows.ApplicationModel.Contacts.Contact) (Mechanismus, der in Windows zum Darstellen von Personen und Unternehmen verwendet wird).  Benutzer können Kontaktinformationen in der Visitenkarte auch ändern. Sie können eine kompakte Visitenkarte oder eine vollständige Visitenkarte anzeigen, die zusätzliche Informationen enthält.

> **Wichtige APIs:** [ShowContactCard-Methode](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard), [ShowFullContactCard-Methode](/uwp/api/windows.applicationmodel.contacts.contactmanager.showfullcontactcard), [IsShowContactCardSupported-Methode](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported), [Contact-Klasse](/uwp/api/Windows.ApplicationModel.Contacts.Contact)  

Es gibt zwei Möglichkeiten, die Visitenkarte anzuzeigen:  
* Als Standardvisitenkarte, die in einem ausblendbaren Flyout angezeigt wird (die Visitenkarte wird ausgeblendet, wenn der Benutzer auf eine Stelle außerhalb der Visitenkarte klickt). 
* Als vollständige Visitenkarte, die mehr Platz in Anspruch nimmt und nicht ausgeblendet werden kann (der Benutzer muss auf **Schließen** klicken, um sie zu schließen). 


<figure>
    <img src="images/contact-card/contact-card-standard.png" alt="The full contact card">
    <figcaption>Standardvisitenkarte</figcaption>
</figure>

<figure>
    <img src="images/contact-card/contact-card-full.png" alt="The full contact card">
    <figcaption>Vollständige Visitenkarte</figcaption>
</figure>


## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie die Visitenkarte, wenn Kontaktinformationen zu einem Kontakt angezeigt werden sollen. Wenn Sie nur den Namen und das Bild des Kontakts anzeigen möchten, verwenden Sie das [Steuerelement für Bilder von Personen](person-picture.md). 


<!-- TODO: Add examples back when the contact card has been added. -->

<!-- ## Examples

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>If you have the <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> app installed, click here to <a href="xamlcontrolsgallery:/item/Button">open the app and see the Button in action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Get the source code (GitHub)</a></li>
    </ul>
</td>
</tr>
</table> -->

## <a name="show-a-standard-contact-card"></a>Anzeigen einer Standardvisitenkarte

1. Eine Visitenkarte wird in der Regel angezeigt, weil der Benutzer auf ein Element geklickt hat: auf eine Schaltfläche oder möglicherweise auf das [Steuerelement für Bilder von Personen](person-picture.md). Das Element soll nicht ausgeblendet werden. Um zu vermeiden, dass es ausgeblendet wird, müssen wir ein [Rect](/uwp/api/windows.foundation.rect) erstellen, das die Position und Größe des Elements beschreibt. 

    Wir erstellen eine Hilfsfunktion, die dies automatisch ausführt (diese wird später verwendet).
    ```csharp
    // Gets the rectangle of the element 
    public static Rect GetElementRectHelper(FrameworkElement element) 
    { 
        // Passing "null" means set to root element. 
        GeneralTransform elementTransform = element.TransformToVisual(null); 
        Rect rect = elementTransform.TransformBounds(new Rect(0, 0, element.ActualWidth, element.ActualHeight)); 
        return rect; 
    } 

    ```

2. Ermitteln Sie, ob Sie die Visitenkarte anzeigen können, indem Sie die [ContactManager.IsShowContactCardSupported](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported)-Methode aufrufen. Wenn sie nicht unterstützt wird, wird eine Fehlermeldung angezeigt. (In diesem Beispiel wird davon ausgegangen, dass Sie die Visitenkarte als Reaktion auf ein Click-Ereignis anzeigen.)
    ```csharp
    // Contact and Contact Managers are existing classes 
    private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
    { 
        if (ContactManager.IsShowContactCardSupported()) 
        { 

    ```

3. Verwenden Sie die in Schritt 1 erstellte Hilfsfunktion, um die Grenzen des Steuerelements abzurufen, von dem das Ereignis ausgelöst wurde (damit es nicht von der Visitenkarte verdeckt wird).

    ```csharp
            Rect selectionRect = GetElementRect((FrameworkElement)sender); 
    ```

4. Rufen Sie das [Contact](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact)-Objekt auf, das angezeigt werden soll. In diesem Beispiel wird lediglich ein einfacher Kontakt erstellt, der Code sollte jedoch ein tatsächlicher Kontakt abrufen. 

    ```csharp
                // Retrieve the contact to display
                var contact = new Contact(); 
                var email = new ContactEmail(); 
                email.Address = "jsmith@contoso.com"; 
                contact.Emails.Add(email); 
    ```
5. Zeigen Sie die Visitenkarte durch Aufrufen der [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard)-Methode an. 

    ```csharp
            ContactManager.ShowFullContactCard(
                contact, selectionRect, Placement.Default); 
        } 
    } 
    ```

Hier sehen Sie den vollständigen Beispielcode:

```csharp
// Gets the rectangle of the element 
public static Rect GetElementRect(FrameworkElement element) 
{ 
    // Passing "null" means set to root element. 
    GeneralTransform elementTransform = element.TransformToVisual(null); 
    Rect rect = elementTransform.TransformBounds(new Rect(0, 0, element.ActualWidth, element.ActualHeight)); 
    return rect; 
} 
 
// Display a contact in response to an event
private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
{ 
    if (ContactManager.IsShowContactCardSupported()) 
    { 
        Rect selectionRect = GetElementRect((FrameworkElement)sender);

        // Retrieve the contact to display
        var contact = new Contact(); 
        var email = new ContactEmail(); 
        email.Address = "jsmith@contoso.com"; 
        contact.Emails.Add(email); 
    
        ContactManager.ShowContactCard(
            contact, selectionRect, Placement.Default); 
    } 
} 

```

## <a name="show-a-full-contact-card"></a>Anzeigen einer vollständigen Visitenkarte

Rufen Sie zum Anzeigen der vollständigen Visitenkarte die [ShowFullContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showfullcontactcard)-Methode anstelle der [ShowContactCard ](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard)-Methode auf.

```csharp
private void onUserClickShowContactCard() 
{ 
   
    Contact contact = new Contact(); 
    ContactEmail email = new ContactEmail(); 
    email.Address = "jsmith@hotmail.com"; 
    contact.Emails.Add(email); 
 
 
    // Setting up contact options.     
    FullContactCardOptions fullContactCardOptions = new FullContactCardOptions(); 
 
    // Display full contact card on mouse click.   
    // Launch the People’s App with full contact card  
    fullContactCardOptions.DesiredRemainingView = ViewSizePreference.UseLess; 
     
 
    // Shows the full contact card by launching the People App. 
    ContactManager.ShowFullContactCard(contact, fullContactCardOptions); 
} 

```

## <a name="retrieving-real-contacts"></a>Abrufen von „echten“ Kontakten

In den Beispielen in diesem Artikel wird ein einfacher Kontakt erstellt. In einer echten App würden Sie wahrscheinlich einen vorhandenen Kontakt abrufen. Anweisungen finden Sie im Artikel [Kontakte und Kalender](../../contacts-and-calendar/index.md).




## <a name="related-articles"></a>Verwandte Artikel
- [Kontakte und Kalender](../../contacts-and-calendar/index.md)
- [Beispiel für Visitenkarten](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCards)
- [Steuerelement für Bilder von Personen](/windows/uwp/controls-and-patterns/person-picture/)