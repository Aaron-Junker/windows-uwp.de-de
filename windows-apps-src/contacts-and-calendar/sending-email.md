---
description: Hier erfahren Sie, wie Sie das Dialogfeld zum Verfassen einer E-Mail starten, damit Benutzer eine E-Mail senden können. Sie können die Felder der E-Mail vor dem Anzeigen des Dialogfelds mit Daten füllen. Die Nachricht wird erst gesendet, wenn Benutzer auf die Schaltfläche „Senden“ tippen.
title: E-Mail senden
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: Kontakte, E-Mail, Senden
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 47d07fa1932aae87704f7922762a8f0b7e430444
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154664"
---
# <a name="send-email"></a>E-Mail senden

Hier erfahren Sie, wie Sie das Dialogfeld zum Verfassen einer E-Mail starten, damit Benutzer eine E-Mail senden können. Sie können die Felder der E-Mail vor dem Anzeigen des Dialogfelds mit Daten füllen. Die Nachricht wird erst gesendet, wenn Benutzer auf die Schaltfläche „Senden“ tippen.

**In diesem Artikel**

-   [Starten des Dialogfelds zum Verfassen einer E-Mail](#launch-the-compose-email-dialog)
-   [Zusammenfassung und nächste Schritte](#summary-and-next-steps)
-   [Zugehörige Themen](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>Starten des Dialogfelds zum Verfassen einer E-Mail

Erstellen Sie ein neues [**EmailMessage**](/uwp/api/Windows.ApplicationModel.Email.EmailMessage)-Objekt, und legen Sie die Daten fest, die im Dialogfeld zum Verfassen einer E-Mail bereits vorhanden sein sollen. Rufen Sie [**ShowComposeNewEmailAsync**](/uwp/api/windows.applicationmodel.email.emailmanager.showcomposenewemailasync) auf, um das Dialogfeld anzuzeigen.

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
> Anlagen, die Sie einer e-Mail mithilfe der [emailattachment](/uwp/api/windows.applicationmodel.email.emailattachment) -Klasse hinzufügen, werden nur in der Mail-App angezeigt. Wenn Benutzer ein anderes e-Mail-Programm als Standard-e-Mail-Programm konfiguriert haben, wird das Fenster verfassen ohne Anhang angezeigt. Dies ist ein bekanntes Problem.

## <a name="summary-and-next-steps"></a>Zusammenfassung und nächste Schritte

In diesem Thema haben Sie erfahren, wie Sie das Dialogfeld zum Verfassen einer E-Mail starten. Informationen zum Auswählen von Kontakten als E-Mail-Empfänger finden Sie unter [Auswählen von Kontakten](selecting-contacts.md). Informationen zum Auswählen einer Datei als E-Mail-Anlage finden Sie unter [**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync).

## <a name="related-topics"></a>Zugehörige Themen

* [Auswählen von Kontakten](selecting-contacts.md)
 