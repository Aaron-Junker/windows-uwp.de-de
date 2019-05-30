---
title: Smartcards
description: In diesem Thema wird erläutert, wie UWP-Apps Smartcards verwenden können, um Benutzer mit sicheren Netzwerkdiensten zu verbinden, einschließlich Informationen für den Zugriff auf physische Smartcardleser, zum Erstellen virtueller Smartcards, zum Kommunizieren mit Smartcards, zum Authentifizieren von Benutzern, zum Zurücksetzen von Benutzer-PINs und zum Entfernen oder Trennen von Smartcards.
ms.assetid: 86524267-50A0-4567-AE17-35C4B6D24745
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Sicherheit
ms.localizationpriority: medium
ms.openlocfilehash: 5498480e0dbe2c8be96d92df766b15676a3e6b7b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371935"
---
# <a name="smart-cards"></a>Smartcards




In diesem Thema wird erläutert, wie UWP-Apps Smartcards verwenden können, um Benutzer mit sicheren Netzwerkdiensten zu verbinden, einschließlich Informationen für den Zugriff auf physische Smartcardleser, zum Erstellen virtueller Smartcards, zum Kommunizieren mit Smartcards, zum Authentifizieren von Benutzern, zum Zurücksetzen von Benutzer-PINs und zum Entfernen oder Trennen von Smartcards. 

## <a name="configure-the-app-manifest"></a>Konfigurieren des App-Manifests


Bevor Ihre App Benutzer mithilfe von Smartcards oder virtuellen Smartcards authentifizieren kann, müssen Sie die Funktion **Freigegebene Benutzerzertifikate** in der Datei „Package.appxmanifest“ festlegen.

## <a name="access-connected-card-readers-and-smart-cards"></a>Zugriff auf verbundene Kartenleser und Smartcards


Sie können Lesegeräte und verbundene Smartcards abrufen, indem Sie die Geräte-ID (in [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) angegeben) an die [**SmartCardReader.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.fromidasync)-Methode übergeben. Zum Zugriff auf die derzeit mit dem zurückgegebenen Lesegerät verbundenen Smartcards rufen Sie [**SmartCardReader.FindAllCardsAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.findallcardsasync) auf.

```cs
string selector = SmartCardReader.GetDeviceSelector();
DeviceInformationCollection devices =
    await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation device in devices)
{
    SmartCardReader reader =
        await SmartCardReader.FromIdAsync(device.Id);

    // For each reader, we want to find all the cards associated
    // with it.  Then we will create a SmartCardListItem for
    // each (reader, card) pair.
    IReadOnlyList<SmartCard> cards =
        await reader.FindAllCardsAsync();
}
```

Zudem sollten Sie der App ermöglichen, [**CardAdded**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.cardadded)-Ereignisse zu beobachten, indem eine Methode zum Behandeln des App-Verhaltens beim Einsetzen einer Karte implementiert wird.

```cs
private void reader_CardAdded(SmartCardReader sender, CardAddedEventArgs args)
{
  // A card has been inserted into the sender SmartCardReader.
}
```

Anschließend können Sie jedes zurückgegebene [**SmartCard**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCard)-Objekt an [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) übergeben, um auf die Methoden zuzugreifen, die Ihrer App den Zugriff auf und die Anpassung ihrer Konfiguration ermöglichen.

## <a name="create-a-virtual-smart-card"></a>Erstellen einer virtuellen Smartcard


Zum Erstellen einer virtuellen Smartcard mithilfe von [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning) muss Ihre App zunächst einen Anzeigenamen, einen Administratorschlüssel und eine [**SmartCardPinPolicy**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardPinPolicy) bereitstellen. Der Anzeigename wird der App in der Regel bereitgestellt, die App muss jedoch trotzdem einen Administratorschlüssel bereitstellen und eine Instanz der aktuellen **SmartCardPinPolicy** generieren, bevor alle drei Werte an [**RequestVirtualSmartCardCreationAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync) übergeben werden.

1.  Erstellen einer neuen Instanz einer [**SmartCardPinPolicy**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardPinPolicy)
2.  Generieren Sie den Administratorschlüsselwert durch Aufrufen von [**CryptographicBuffer.GenerateRandom**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.cryptographicbuffer.generaterandom) für den vom Dienst oder Verwaltungstool bereitgestellten Administratorschlüsselwert.
3.  Übergeben Sie diese Werte zusammen mit der *FriendlyNameText*-Zeichenfolge an [**RequestVirtualSmartCardCreationAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync).

```cs
SmartCardPinPolicy pinPolicy = new SmartCardPinPolicy();
pinPolicy.MinLength = 6;

IBuffer adminkey = CryptographicBuffer.GenerateRandom(24);

SmartCardProvisioning provisioning = await
     SmartCardProvisioning.RequestVirtualSmartCardCreationAsync(
          "Card friendly name",
          adminkey,
          pinPolicy);
```

Sobald [**RequestVirtualSmartCardCreationAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcardcreationasync) das zugehörige [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning)-Objekt zurückgegeben hat, wird die virtuelle Smartcard bereitgestellt und kann verwendet werden.

## <a name="handle-authentication-challenges"></a>Behandeln von Authentifizierungsaufforderungen


Für die Authentifizierung mit Smartcards oder virtuellen Smartcards muss Ihre App das Verhalten bereitstellen, um Aufforderungen zwischen den auf der Karte gespeicherten Administratorschlüsseldaten und den vom Authentifizierungsserver oder Verwaltungstool verwalteten Administratorschlüsseldaten abzuschließen.

Der folgende Code ist ein Beispiel für die Unterstützung der Smartcard-Authentifizierung für Dienste oder die Änderung von physischen oder virtuellen Kartendetails. Wenn die mithilfe des Administratorschlüssels auf der Karte generierten Daten ("challenge") mit den vom Server oder Verwaltungstool bereitgestellten Daten ("adminkey") übereinstimmen, ist die Authentifizierung erfolgreich.

```cs
static class ChallengeResponseAlgorithm
{
    public static IBuffer CalculateResponse(IBuffer challenge, IBuffer adminkey)
    {
        if (challenge == null)
            throw new ArgumentNullException("challenge");
        if (adminkey == null)
            throw new ArgumentNullException("adminkey");

        SymmetricKeyAlgorithmProvider objAlg = SymmetricKeyAlgorithmProvider.OpenAlgorithm(SymmetricAlgorithmNames.TripleDesCbc);
        var symmetricKey = objAlg.CreateSymmetricKey(adminkey);
        var buffEncrypted = CryptographicEngine.Encrypt(symmetricKey, challenge, null);
        return buffEncrypted;
    }
}
```

Auf diesen Code wird im gesamten verbleibenden Teil dieses Themas verwiesen, wenn erläutert wird, wie eine Authentifizierungsaktion abgeschlossen und Änderungen an Informationen zu Smartcards und virtuellen Smartcards übernommen werden.

## <a name="verify-smart-card-or-virtual-smart-card-authentication-response"></a>Überprüfen der Authentifizierungsantwort von Smartcards oder virtuellen Smartcards


Nachdem wir die Logik für Authentifizierungsaufforderungen definiert haben, können wir jetzt mit dem Leser kommunizieren, um auf die Smartcard oder alternativ auf eine virtuelle Smartcard zuzugreifen, um die Authentifizierung durchzuführen.

1.  Rufen Sie zum Starten der Aufforderung [**GetChallengeContextAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.getchallengecontextasync) vom [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning)-Objekt auf, das der Smartcard zugeordnet ist. Dadurch wird eine Instanz von [**SmartCardChallengeContext**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardChallengeContext) erstellt, die den [**Challenge**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardchallengecontext.challenge)-Wert der Karte enthält.

2.  Übergeben Sie als Nächstes den Aufforderungswert der Karte und den vom Dienst oder Verwaltungstool bereitgestellten Administratorschlüssel an das **ChallengeResponseAlgorithm**-Element, das wir im vorhergehenden Beispiel definiert haben.

3.  [**VerifyResponseAsync** ](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardchallengecontext.verifyresponseasync) zurück **"true"** wenn Authentifizierung erfolgreich ist.

```cs
bool verifyResult = false;
SmartCard card = await rootPage.GetSmartCard();
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

using (SmartCardChallengeContext context =
       await provisioning.GetChallengeContextAsync())
{
    IBuffer response = ChallengeResponseAlgorithm.CalculateResponse(
        context.Challenge,
        rootPage.AdminKey);

    verifyResult = await context.VerifyResponseAsync(response);
}
```

## <a name="change-or-reset-a-user-pin"></a>Ändern oder Zurücksetzen einer Benutzer-PIN


So ändern Sie die einer Smartcard zugeordnete PIN:

1.  Greifen Sie auf die Karte zu, und generieren Sie das zugehörige [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning)-Objekt.
2.  Rufen Sie [**RequestPinChangeAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinchangeasync) auf, um eine Benutzeroberfläche anzuzeigen, auf der der Benutzer diesen Vorgang abschließen kann.
3.  Wenn die PIN erfolgreich geändert wurde, gibt der Aufruf den Wert **true** zurück.

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinChangeAsync();
```

So fordern Sie das Zurücksetzen einer PIN an

1.  Rufen Sie [**RequestPinResetAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinresetasync) auf, um den Vorgang zu initiieren. Dieser Aufruf enthält eine [**SmartCardPinResetHandler**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardpinresethandler)-Methode, die die Smartcard und die Anforderung der PIN-Zurücksetzung darstellt.
2.  [**SmartCardPinResetHandler** ](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardpinresethandler) enthält Informationen, die unsere **ChallengeResponseAlgorithm**, eingeschlossen in einem [ **SmartCardPinResetDeferral** ](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardPinResetDeferral) aufrufen, zum Vergleichen der Karte Herausforderung Wert und der Admin-Schlüssel bereitgestellt werden, indem der Dienst oder ein externes Verwaltungstool zum Authentifizieren der Anforderung verwendet wird.

3.  Bei einer erfolgreichen Aufforderung ist der [**RequestPinResetAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestpinresetasync)-Aufruf abgeschlossen; bei einem erfolgreichen Zurücksetzen der PIN wird der Wert **true** zurückgegeben.

```cs
SmartCardProvisioning provisioning =
    await SmartCardProvisioning.FromSmartCardAsync(card);

bool result = await provisioning.RequestPinResetAsync(
    (pinResetSender, request) =>
    {
        SmartCardPinResetDeferral deferral =
            request.GetDeferral();

        try
        {
            IBuffer response =
                ChallengeResponseAlgorithm.CalculateResponse(
                    request.Challenge,
                    rootPage.AdminKey);
            request.SetResponse(response);
        }
        finally
        {
            deferral.Complete();
        }
    });
}
```

## <a name="remove-a-smart-card-or-virtual-smart-card"></a>Entfernen einer Smartcard oder virtuellen Smartcard


Wenn eine physische Smartcard entfernt wird, wird beim Löschen der Karte ein [**CardRemoved**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardreader.cardremoved)-Ereignis ausgelöst.

Ordnen Sie das Auslösen dieses Ereignisses mithilfe der Methode, die das Verhalten Ihrer App beim Entfernen einer Karte oder eines Lesers definiert, dem Kartenleser als Ereignishandler zu. Bei diesem Verhalten kann es sich z. B. einfach um eine Benachrichtigung des Benutzers handeln, dass die Karte entfernt wurde.

```cs
reader = card.Reader;
reader.CardRemoved += HandleCardRemoved;
```

Das Entfernen einer virtuellen Smartcard wird programmgesteuert behandelt, indem zuerst die Karte abgerufen und anschließend [**RequestVirtualSmartCardDeletionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.smartcards.smartcardprovisioning.requestvirtualsmartcarddeletionasync) aus dem zurückgegebenen [**SmartCardProvisioning**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardProvisioning)-Objekt aufgerufen wird.

```cs
bool result = await SmartCardProvisioning
    .RequestVirtualSmartCardDeletionAsync(card);
```