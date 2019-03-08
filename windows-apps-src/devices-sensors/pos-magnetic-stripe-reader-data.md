---
title: Abrufen und Verstehen von Magnetstreifendaten
description: Informationen Sie zum Abrufen und Interpretieren der Daten aus einer magnetstreifens.
ms.date: 10/04/2018
ms.topic: article
keywords: Zeigen Sie die Windows 10, Uwp, Service, pos, Magnetstreifenleser
ms.localizationpriority: medium
ms.openlocfilehash: 1805213c7c30ccbc67fb96098f11480703589bb4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651605"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>Abrufen und Verstehen von Magnetstreifendaten

Nachdem Sie Ihre Magnetstreifenleser in Ihrer Anwendung mithilfe der Schritte in eingerichtet haben [erste Schritte mit Point-of-Service-](pos-basics.md), Sie sind bereit, um Daten aus.

## <a name="subscribe-to-datareceived-events"></a>Abonnieren Sie * "DataReceived"-Ereignisse

Wenn der Reader eine gestreiften Karte erkennt, wird eines der drei Ereignisse ausgelöst:

* [AamvaCardDataReceived Ereignis](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): Tritt auf, wenn eine Karte kraftfahrzeugbehörde Transaktionsdatenbank ist.
* [BankCardDataReceived Ereignis](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): Tritt auf, wenn es sich bei eine Bankkarte Transaktionsdatenbank ist.
* [VendorSpecificDataReceived Ereignis](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived): Tritt auf, wenn eine Karte anbieterspezifische Transaktionsdatenbank ist.

Ihre Anwendung muss nur die Ereignisse abonnieren, die vom Magnetstreifen Reader unterstützt werden. Sie können sehen, welche Typen von Karten mit unterstützt werden [MagneticStripeReader.SupportedCardTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes
).

Der folgende Code zeigt die drei abonnieren ***"DataReceived"** Ereignisse:

```cs
private void SubscribeToEvents(ClaimedMagneticStripeReader claimedReader, MagneticStripeReader reader)
{
    foreach (var type in reader.SupportedCardTypes)
    {
        if (item == MagneticStripeReaderCardTypes.Aamva)
        {
            claimedReader.AamvaCardDataReceived += Reader_AamvaCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.Bank)
        {
            claimedReader.BankCardDataReceived += Reader_BankCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.ExtendedBase)
        {
            claimedReader.VendorSpecificDataReceived += Reader_VendorSpecificDataReceived;
        }
    }
}
```

Der Ereignishandler übergeben die [ClaimedMagneticStripeReader](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader) und ein *Args* Objekt, dessen Typ abhängig vom Ereignis variieren:

* **AamvaCardDataReceived** Ereignis: [MagneticStripeReaderAamvaCardDataReceivedEventArgs-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* **BankCardDataReceived** Ereignis: [MagneticStripeReaderBankCardDataReceivedEventArgs-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* **VendorSpecificDataReceived** Ereignis: [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs Class](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>Abrufen der Daten

Für die **AamvaCardDataReceived** und **BankCardDataReceived** Ereignisse, erhalten Sie einige der Daten direkt aus der *Args* Objekt. Das folgende Beispiel zeigt einige Eigenschaften abrufen und sie Membervariablen zugewiesen werden:

```cs
private string _accountNumber;
private string _expirationDate;
private string _firstName;

private void Reader_BankCardDataReceived(
    ClaimedMagneticStripeReader sender, 
    MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    _accountNumber = args.AccountNumber;
    _expirationDate = args.ExpirationDate;
    _firstName = args.FirstName;
    // etc...
}
```

Allerdings einige Daten, einschließlich aller Daten aus der **VendorSpecificDataReceived** Ereignis abgerufen werden muss, bis die **Bericht** -Objekt, das eine Eigenschaft ist von der *Args* der Parameter. Dies ist vom Typ [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport).

Sie können die [CardType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype) Eigenschaft, um herauszufinden, welche Art von Karte Transaktionsdatenbank wurde hat dann verwenden, um darüber zu informieren, wie die Daten aus interpretiert [Track1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1), [Track2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2), [ Track3](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3), und [Track4](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4).

Die Daten aus einzelnen Tracks des werden als dargestellt [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata) Objekte. Von dieser Klasse können Sie die folgenden Arten von Daten abrufen:

* [Daten](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data): Die unformatierten bzw. entschlüsselte Daten.
* [DiscretionaryData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata): Die freigegebenen Daten. 
* [EncryptedData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata): Die verschlüsselten Daten.

Der folgende Codeausschnitt Ruft den Bericht und die Titeldaten ab und überprüft dann die Art der Netzwerkschnittstellenkarte:

```cs
private void GetTrackData(MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    MagneticStripeReaderReport report = args.Report;
    IBuffer track1 = report.Track1.Data;
    IBuffer track2 = report.Track2.Data;
    IBuffer track3 = report.Track3.Data;
    IBuffer track4 = report.Track4.Data;

    if (report.CardType == MagneticStripeReaderCardTypes.Aamva)
    {
        // Card type is AAMVA
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Bank)
    {
        // Card type is bank card
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.ExtendedBase)
    {
        // Card type is vendor-specific
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Unknown)
    {
        // Card type is unknown
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>Siehe auch

* [Magnetstreifenleser](pos-magnetic-stripe-reader.md)
* [ClaimedMagneticStripeReader-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [MagneticStripeReaderAamvaCardDataReceivedEventArgs-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [MagneticStripeReaderBankCardDataReceivedEventArgs-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)