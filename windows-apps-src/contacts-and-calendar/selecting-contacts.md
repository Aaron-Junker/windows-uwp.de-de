---
description: Mit dem Windows.ApplicationModel.Contacts-Namespace verfügen Sie über mehrere Optionen zum Auswählen von Kontakten.
title: Auswählen von Kontakten
ms.assetid: 35FEDEE6-2B0E-4391-84BA-5E9191D4E442
keywords: Kontakte, auswählen, einzelnen Kontakt auswählen, mehrere Kontakte auswählen, Kontakte, mehrere auswählen; bestimmten Kontakt auswählen, Kontaktdaten, bestimmte auswählen, Kontaktdaten, bestimmte Felder auswählen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dcf8fefb82de3cccf3d914ae6003c991107888e1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154684"
---
# <a name="select-contacts"></a>Auswählen von Kontakten



Mit dem [**Windows.ApplicationModel.Contacts**](/uwp/api/Windows.ApplicationModel.Contacts)-Namespace verfügen Sie über mehrere Optionen zum Auswählen von Kontakten. Wir zeigen Ihnen hier, wie Sie einen einzelnen Kontakt oder mehrere Kontakte auswählen und wie Sie die Kontaktauswahl so konfigurieren, dass nur die von der App benötigten Kontaktinformationen abgerufen werden.

## <a name="set-up-the-contact-picker"></a>Einrichten der Kontaktauswahl

Erstellen Sie eine Instanz von [**Windows.ApplicationModel.Contacts.ContactPicker**](/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker), und weisen Sie sie einer Variablen zu.

```cs
var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
```

## <a name="set-the-selection-mode-optional"></a>Festlegen des Auswahlmodus (optional)

Standardmäßig ruft die Kontaktauswahl alle verfügbaren Daten für die von Benutzern ausgewählten Kontakte ab. Mithilfe der [**SelectionMode**](/uwp/api/windows.applicationmodel.contacts.contactpicker.selectionmode)-Eigenschaft können Sie die Kontaktauswahl so konfigurieren, dass nur die von der App benötigten Datenfelder abgerufen werden. Diese effiziente Verwendungsweise der Kontaktauswahl eignet sich besonders dann, wenn Sie nur einen Teil der verfügbaren Kontaktdaten benötigen.

Legen Sie zunächst die [**SelectionMode**](/uwp/api/windows.applicationmodel.contacts.contactpicker.selectionmode)-Eigenschaft auf **Fields** fest.

```cs
contactPicker.SelectionMode = Windows.ApplicationModel.Contacts.ContactSelectionMode.Fields;
```

Geben Sie dann mithilfe der [**DesiredFieldsWithContactFieldType**](/uwp/api/windows.applicationmodel.contacts.contactpicker.desiredfieldswithcontactfieldtype)-Eigenschaft die Felder an, die von der Kontaktauswahl abgerufen werden sollen. In diesem Beispiel wird die Kontaktauswahl so konfiguriert, dass E-Mail-Adressen abgerufen werden:

``` cs
contactPicker.DesiredFieldsWithContactFieldType.Add(Windows.ApplicationModel.Contacts.ContactFieldType.Email);
```

## <a name="launch-the-picker"></a>Starten der Auswahl

```cs
Contact contact = await contactPicker.PickContactAsync();
```

Verwenden Sie [**pickcontactionasync**](/uwp/api/windows.applicationmodel.contacts.contactpicker.pickcontactsasync) , wenn der Benutzer einen oder mehrere Kontakte auswählen soll.

```cs
public IList<Contact> contacts;
contacts = await contactPicker.PickContactsAsync();
```

## <a name="process-the-contacts"></a>Verarbeiten der Kontakte

Überprüfen Sie in der Auswahl, ob der Benutzer Kontakte ausgewählt hat. Wenn ja, verarbeiten Sie die Kontaktinformationen.

Dieses Beispiel zeigt, wie ein einzelner Kontakt verarbeitet wird. Hier wird der Name des Kontakts abgerufen und in ein [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Steuerelement namens *OutputName* kopiert.

```cs
if (contact != null)
{
    OutputName.Text = contact.DisplayName;
}
else
{
    rootPage.NotifyUser("No contact was selected.", NotifyType.ErrorMessage);
}
```

Dieses Beispiel zeigt die Verarbeitung mehrerer Kontakte.

```cs
if (contacts != null && contacts.Count > 0)
{
    foreach (Contact contact in contacts)
    {
        // Do something with the contact information.
    }
}
```

## <a name="complete-example-single-contact"></a>Vollständiges Beispiel (einzelner Kontakt)

In diesem Beispiel werden mithilfe der Kontaktauswahl der Name sowie E-Mail-Adresse, Standort oder Telefonnummer eines einzelnen Kontakts abgerufen.

```cs
// ...
using Windows.ApplicationModel.Contacts;
// ...

private async void PickAContactButton-Click(object sender, RoutedEventArgs e)
{
    ContactPicker contactPicker = new ContactPicker();

    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.Email);
    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.Address);
    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.PhoneNumber);

    Contact contact = await contactPicker.PickContactAsync();

    if (contact != null)
    {
        OutputFields.Visibility = Visibility.Visible;
        OutputEmpty.Visibility = Visibility.Collapsed;

        OutputName.Text = contact.DisplayName;

        AppendContactFieldValues(OutputEmails, contact.Emails);
        AppendContactFieldValues(OutputPhoneNumbers, contact.Phones);
        AppendContactFieldValues(OutputAddresses, contact.Addresses);
    }
    else
    {
        OutputEmpty.Visibility = Visibility.Visible;
        OutputFields.Visibility = Visibility.Collapsed;
    }
}

private void AppendContactFieldValues<T>(TextBlock content, IList<T> fields)
{
    if (fields.Count > 0)
    {
        StringBuilder output = new StringBuilder();

        if (fields[0].GetType() == typeof(ContactEmail))
        {
            foreach (ContactEmail email in fields as IList<ContactEmail>)
            {
                output.AppendFormat("Email: {0} ({1})\n", email.Address, email.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactPhone))
        {
            foreach (ContactPhone phone in fields as IList<ContactPhone>)
            {
                output.AppendFormat("Phone: {0} ({1})\n", phone.Number, phone.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactAddress))
        {
            List<String> addressParts = null;
            string unstructuredAddress = "";

            foreach (ContactAddress address in fields as IList<ContactAddress>)
            {
                addressParts = (new List<string> { address.StreetAddress, address.Locality, address.Region, address.PostalCode });
                unstructuredAddress = string.Join(", ", addressParts.FindAll(s => !string.IsNullOrEmpty(s)));
                output.AppendFormat("Address: {0} ({1})\n", unstructuredAddress, address.Kind);
            }
        }

        content.Visibility = Visibility.Visible;
        content.Text = output.ToString();
    }
    else
    {
        content.Visibility = Visibility.Collapsed;
    }
}
```

## <a name="complete-example-multiple-contacts"></a>Vollständiges Beispiel (mehrere Kontakte)

In diesem Beispiel werden mithilfe der Kontaktauswahl mehrere Kontakte abgerufen und einem [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView)-Steuerelement namens `OutputContacts` hinzugefügt.

```cs
MainPage rootPage = MainPage.Current;
public IList<Contact> contacts;

private async void PickContactsButton-Click(object sender, RoutedEventArgs e)
{
    var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
    contactPicker.CommitButtonText = "Select";
    contacts = await contactPicker.PickContactsAsync();

    // Clear the ListView.
    OutputContacts.Items.Clear();

    if (contacts != null && contacts.Count > 0)
    {
        OutputContacts.Visibility = Windows.UI.Xaml.Visibility.Visible;
        OutputEmpty.Visibility = Visibility.Collapsed;

        foreach (Contact contact in contacts)
        {
            // Add the contacts to the ListView.
            OutputContacts.Items.Add(new ContactItemAdapter(contact));
        }
    }
    else
    {
        OutputEmpty.Visibility = Visibility.Visible;
    }         
}
```

``` cs
public class ContactItemAdapter
{
    public string Name { get; private set; }
    public string SecondaryText { get; private set; }

    public ContactItemAdapter(Contact contact)
    {
        Name = contact.DisplayName;
        if (contact.Emails.Count > 0)
        {
            SecondaryText = contact.Emails[0].Address;
        }
        else if (contact.Phones.Count > 0)
        {
            SecondaryText = contact.Phones[0].Number;
        }
        else if (contact.Addresses.Count > 0)
        {
            List<string> addressParts = (new List<string> { contact.Addresses[0].StreetAddress,
              contact.Addresses[0].Locality, contact.Addresses[0].Region, contact.Addresses[0].PostalCode });
              string unstructuredAddress = string.Join(", ", addressParts.FindAll(s => !string.IsNullOrEmpty(s)));
            SecondaryText = unstructuredAddress;
        }
    }
}
```

## <a name="summary-and-next-steps"></a>Zusammenfassung und nächste Schritte

Sie verfügen nun über grundlegende Kenntnisse zum Abrufen von Kontaktinformationen mithilfe der Kontaktauswahl. Laden Sie die [Beispiele für universelle Windows-Apps](https://github.com/Microsoft/Windows-universal-samples) von GitHub herunter, um sich weitere Beispiele für die Verwendung von Kontakten und Kontaktauswahl anzusehen.