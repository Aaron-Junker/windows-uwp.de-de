---
description: Hier erfahren Sie, wie Sie das Dialogfeld zum Verfassen einer E-Mail starten, damit Benutzer eine E-Mail senden können. Sie können die Felder der E-Mail vor dem Anzeigen des Dialogfelds mit Daten füllen. Die Nachricht wird erst gesendet, wenn Benutzer auf die Schaltfläche „Senden“ tippen.
title: Senden von E-Mails
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: Kontakte, E-Mail, Senden
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 524e1f12c3da0d9d06e73d84e08e2d54efde9a7e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361218"
---
# <a name="send-email"></a>Senden von E-Mails

Hier erfahren Sie, wie Sie das Dialogfeld zum Verfassen einer E-Mail starten, damit Benutzer eine E-Mail senden können. Sie können die Felder der E-Mail vor dem Anzeigen des Dialogfelds mit Daten füllen. Die Nachricht wird erst gesendet, wenn Benutzer auf die Schaltfläche „Senden“ tippen.

**In diesem Artikel**

-   [Starten Sie das Dialogfeld der Compose-e-Mail](#launch-the-compose-email-dialog)
-   [Zusammenfassung und nächste Schritte](#summary-and-next-steps)
-   [Verwandte Themen](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>Starten des Dialogfelds zum Verfassen einer E-Mail

Erstellen Sie ein neues [**EmailMessage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email.EmailMessage)-Objekt, und legen Sie die Daten fest, die im Dialogfeld zum Verfassen einer E-Mail bereits vorhanden sein sollen. Rufen Sie [**ShowComposeNewEmailAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailmanager.showcomposenewemailasync) auf, um das Dialogfeld anzuzeigen.

``` cs
private async Task ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient,
    string subject, string messageBody)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
        emailMessage.Subject = subject;
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
}
```

>[!NOTE]
> Anlagen, die Sie an eine e-Mail-Adresse hinzufügen, mit der [EmailAttachment](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailattachment) Klasse wird nur in der Mail-app angezeigt. Wenn Benutzer andere e-Mail-Programm als ihre e-Mail-Standardprogramm konfiguriert haben, wird das Fenster "Compose" ohne die Anlage angezeigt. Dies ist ein bekanntes Problem.

## <a name="summary-and-next-steps"></a>Zusammenfassung und nächste Schritte

In diesem Thema haben Sie erfahren, wie Sie das Dialogfeld zum Verfassen einer E-Mail starten. Informationen zum Auswählen von Kontakten als E-Mail-Empfänger finden Sie unter [Auswählen von Kontakten](selecting-contacts.md). Informationen zum Auswählen einer Datei als E-Mail-Anlage finden Sie unter [**PickSingleFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync).

## <a name="related-topics"></a>Verwandte Themen

* [Kontakte auswählen](selecting-contacts.md)
* [Wie Sie Ihre Windows Phone-app weiterhin nach dem Aufruf von eine Dateiauswahl](https://docs.microsoft.com/previous-versions/windows/apps/dn614994(v=win.10))
 

 
