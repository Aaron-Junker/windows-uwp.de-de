---
description: In diesem Thema erfahren Sie, wie Sie das Dialogfeld zum Verfassen einer SMS starten, damit Benutzer eine SMS senden können. Sie können die Felder der SMS vor dem Anzeigen des Dialogfelds mit Daten füllen. Die Nachricht wird erst gesendet, wenn Benutzer auf die Schaltfläche „Senden“ tippen.
title: Senden einer SMS
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: Kontakte, SMS, Senden
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ceaeffdb0d8b3207a95e981f16590fe8e9e12e46
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154654"
---
# <a name="send-an-sms-message"></a>Senden einer SMS

In diesem Thema erfahren Sie, wie Sie das Dialogfeld zum Verfassen einer SMS starten, damit Benutzer eine SMS senden können. Sie können die Felder der SMS vor dem Anzeigen des Dialogfelds mit Daten füllen. Die Nachricht wird erst gesendet, wenn Benutzer auf die Schaltfläche „Senden“ tippen.

Um diesen Code aufzurufen, deklarieren Sie die Funktionen **Chat**, **smssend**und **Chatsystem** in Ihrem Paket Manifest. Dies sind [eingeschränkte Funktionen](../packaging/app-capability-declarations.md#special-and-restricted-capabilities) , aber Sie können Sie in Ihrer APP verwenden. Sie benötigen nur dann eine Genehmigung, wenn Sie beabsichtigen, Ihre APP im Store zu veröffentlichen. Weitere Informationen finden Sie unter [Konto Typen, Standorte und Gebühren](../publish/account-types-locations-and-fees.md).

## <a name="launch-the-compose-sms-dialog"></a>Starten des Dialogfelds zum Verfassen einer SMS

Erstellen Sie ein neues [**ChatMessage**](/uwp/api/windows.applicationmodel.chat.chatmessage)-Objekt, und legen Sie die Daten fest, die im Dialogfeld zum Verfassen einer E-Mail bereits vorhanden sein sollen. Rufen Sie [**ShowComposeSmsMessageAsync**](/uwp/api/windows.applicationmodel.chat.chatmessagemanager.showcomposesmsmessageasync) auf, um das Dialogfeld anzuzeigen.

```cs
private async void ComposeSms(Windows.ApplicationModel.Contacts.Contact recipient,
    string messageBody,
    StorageFile attachmentFile,
    string mimeType)
{
    var chatMessage = new Windows.ApplicationModel.Chat.ChatMessage();
    chatMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Chat.ChatMessageAttachment(
            mimeType,
            stream);

        chatMessage.Attachments.Add(attachment);
    }

    var phone = recipient.Phones.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactPhone>();
    if (phone != null)
    {
        chatMessage.Recipients.Add(phone.Number);
    }
    await Windows.ApplicationModel.Chat.ChatMessageManager.ShowComposeSmsMessageAsync(chatMessage);
}
```

Mithilfe des folgenden Codes können Sie feststellen, ob das Gerät, auf dem Ihre APP ausgeführt wird, SMS-Nachrichten senden kann.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.ApplicationModel.Chat"))
{
   // Call code here.
}
```

## <a name="summary-and-next-steps"></a>Zusammenfassung und nächste Schritte

In diesem Thema haben Sie erfahren, wie Sie das Dialogfeld zum Verfassen einer SMS starten. Informationen zum Auswählen von Kontakten als SMS-Empfänger finden Sie unter [Auswählen von Kontakten](selecting-contacts.md). Laden Sie die [Beispiele für universelle Windows-Apps](https://github.com/Microsoft/Windows-universal-samples) von GitHub herunter, um sich weitere Beispiele zum Senden und Empfangen von SMS-Nachrichten unter Verwendung einer Hintergrundaufgabe anzusehen.

## <a name="related-topics"></a>Zugehörige Themen

* [Auswählen von Kontakten](selecting-contacts.md)