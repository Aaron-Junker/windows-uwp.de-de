---
title: Abrufen und Verstehen von Magnetstreifendaten
description: Erfahren Sie, wie Sie die Daten von einem Magnetstreifenleser mithilfe von universelle Windows-Plattform-APIs (Point of Service, POS) abrufen und interpretieren.
ms.date: 10/04/2018
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS, Magnetstreifenleser
ms.localizationpriority: medium
ms.openlocfilehash: 2a405a66fbc243925c4c9bbd5b1ee499a9a7b9f8
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238275"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>Abrufen und Verstehen von Magnetstreifendaten

Nachdem Sie Ihren Magnetstreifenleser in der Anwendung mithilfe der Schritte in erste Schritte [mit Point of Service](pos-basics.md)eingerichtet haben, können Sie damit beginnen, Daten aus ihm zu erhalten.

## <a name="subscribe-to-datareceived-events"></a>* DataReceived-Ereignisse abonnieren

Jedes Mal, wenn der Reader eine über wandelte Karte erkennt, wird eines von drei Ereignissen angezeigt:

* [Aamvacarddatareceived-Ereignis](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived): tritt auf, wenn eine Fahrzeug Karte per Schwenk angezeigt wird.
* Bankkonto " [bankcarddatareceived](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived)": tritt auf, wenn eine Bank Karte per schwenken gerenppt wird.
* [Vendorspecificdatareceived-Ereignis](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived): tritt auf, wenn eine herstellerspezifische Karte übertragen wird.

Die Anwendung muss nur die Ereignisse abonnieren, die vom Magnet Stripe Reader unterstützt werden. Sie können sehen, welche Kartentypen mit " [magneticstripereader. supportedcardtypes](/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes)" unterstützt werden.

Der folgende Code veranschaulicht das Abonnieren der drei ***DataReceived** -Ereignisse:

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

Dem Ereignishandler werden das [claimedmagneticstripereader](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader) -Objekt und ein *args* -Objekt übergeben, dessen Typ je nach Ereignis variiert:

* **Aamvacarddatareceived** -Ereignis: [magneticstripereaderaamvacarddatareceivedeventargs-Klasse](/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* **Bankcarddatareceived** -Ereignis: [magneticstripereaderbankcarddatareceivedeventargs-Klasse](/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* **Vendorspecificdatareceived** -Ereignis: [magneticstripereadervendorspecificcarddatareceivedeventargs-Klasse](/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>Abrufen von Daten

Für das **aamvacarddatareceived** -und das **bankcarddatareceived** -Ereignis können Sie einige der Daten direkt aus dem *args* -Objekt erhalten. Das folgende Beispiel zeigt, wie Sie einige Eigenschaften erhalten und diese den Element Variablen zuweisen:

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

Allerdings müssen einige Daten, einschließlich aller Daten aus dem **vendorspecificdatareceived** -Ereignis, über das **Report** -Objekt abgerufen werden, das eine Eigenschaft des *args* -Parameters ist. Dies ist vom Typ " [magneticstripereaderreport](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)".

Sie können die [cardtype](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype) -Eigenschaft verwenden, um herauszufinden, welcher Kartentyp übergegangen ist, und dann verwenden, um zu informieren, wie Sie die Daten von [Track1](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1), [track2](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2), [Track3](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3)und [Track4](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4)interpretieren.

Die Daten der einzelnen Spuren werden als " [magneticstripereadertrackdata](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata) "-Objekte dargestellt. Aus dieser Klasse können Sie die folgenden Datentypen erhalten:

* [Data](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data): die Rohdaten oder decodierten Daten.
* [Diskretionarydata](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata): die freigegebenen Daten. 
* [Verschlüsselteddata](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata): die verschlüsselten Daten.

Der folgende Code Ausschnitt Ruft den Bericht und die Nachverfolgung von Daten ab und überprüft dann den Kartentyp:

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

## <a name="see-also"></a>Weitere Informationen

* [Magnetstreifenleser](pos-magnetic-stripe-reader.md)
* [Claimedmagneticstripereader-Klasse](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [Magneticstripereaderaamvacarddatareceivedeventargs-Klasse](/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [Magneticstripereaderbankcarddatareceivedeventargs-Klasse](/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [Magneticstripereadervendorspecificcarddatareceivedeventargs-Klasse](/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* ["Magneticstripereaderreport"](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* ["Magneticstripereadertrackdata"](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)