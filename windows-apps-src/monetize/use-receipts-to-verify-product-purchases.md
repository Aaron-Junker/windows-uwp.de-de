---
ms.assetid: E322DFFE-8EEC-499D-87BC-EDA5CFC27551
description: Jede Microsoft Store-Transaktion, die zu einem erfolgreichen Produktkauf führt, kann optional einen Transaktionsbeleg zurückgeben.
title: Überprüfen von Produktkäufen anhand von Belegen
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, In-App-Einkäufe, IAPs, Belege, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: a26d98de58c954f1bec588b335483de08404862b
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259224"
---
# <a name="use-receipts-to-verify-product-purchases"></a>Überprüfen von Produktkäufen anhand von Belegen

Jede Microsoft Store-Transaktion, die zu einem erfolgreichen Produktkauf führt, kann optional einen Transaktionsbeleg zurückgeben. Dieser Beleg enthalten Informationen zum gelisteten Produkt und zu den Kosten für den Kunden.

Der Zugriff auf diese Informationen unterstützt Szenarien, in denen Ihre App überprüfen muss, ob ein Benutzer Ihre App oder ein Add-On (auch als In-App-Produkt oder IAP bezeichnet) im Microsoft Store gekauft hat. Das kann zum Beispiel bei einem Spiel der Fall sein, für das Inhalte heruntergeladen werden können. Wenn der Benutzer, der die Spielinhalte gekauft hat, das Spiel auf einem anderen Gerät spielen möchte, müssen Sie überprüfen, ob der Benutzer die Inhalte bereits besitzt. Dies funktioniert so.

> [!IMPORTANT]
> Dieser Artikel beschreibt, wie Sie Mitglieder des [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store)-Namespace verwenden, um einen Beleg für einen In-App-Kauf abzurufen und zu überprüfen. Wenn Sie den [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.Services.Store)-Namespace -Namespace für In-App-Käufe verwenden (eingeführt in **Windows 10 Anniversary Edition (10.0; Build 14393)** oder einer höheren Version in Visual Studio), müssen Sie beachten, dass dieser Namespace keine API zum Abrufen von Kaufbelegen für In-App-Einkäufe enthält. Sie können jedoch eine REST-Methode in der Microsoft Store-Sammlungs-API verwenden, um Daten für eine Kauftransaktion abzurufen. Weitere Informationen finden Sie unter [Belege für In-App-Käufe](in-app-purchases-and-trials.md#receipts).

## <a name="requesting-a-receipt"></a>Anfordern eines Belegs


Der **Windows.ApplicationModel.Store**-Namespace unterstützt verschiedene Möglichkeiten für das Abrufen eines Belegs:

* Wenn Sie mithilfe von [CurrentApp.RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) oder [CurrentApp.RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) (oder eine andere Überladung dieser Methode) einen Kauf durchführen, enthält der Rückgabewert den Beleg.
* Sie können die Methode [CurrentApp.GetAppReceiptAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync) aufrufen, um die aktuellen Beleginformationen für Ihre App und alle Add-Ons in Ihrer App abzurufen.

Ein App-Beleg sieht ungefähr wie folgt aus.

> [!NOTE]
> Dieses Beispiel ist formatiert, damit das XML lesbar ist. Echte App-Belege enthalten keine Leerzeichen zwischen Elementen.

> [!div class="tabbedCodeSnippets"]
```xml
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:10:05Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <AppReceipt Id="8ffa256d-eca8-712a-7cf8-cbf5522df24b" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" PurchaseDate="2012-06-04T23:07:24Z" LicenseType="Full" />
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>cdiU06eD8X/w1aGCHeaGCG9w/kWZ8I099rw4mmPpvdU=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>SjRIxS/2r2P6ZdgaR9bwUSa6ZItYYFpKLJZrnAa3zkMylbiWjh9oZGGng2p6/gtBHC2dSTZlLbqnysJjl7mQp/A3wKaIkzjyRXv3kxoVaSV0pkqiPt04cIfFTP0JZkE5QD/vYxiWjeyGp1dThEM2RV811sRWvmEs/hHhVxb32e8xCLtpALYx3a9lW51zRJJN0eNdPAvNoiCJlnogAoTToUQLHs72I1dECnSbeNPXiG7klpy5boKKMCZfnVXXkneWvVFtAA1h2sB7ll40LEHO4oYN6VzD+uKd76QOgGmsu9iGVyRvvmMtahvtL1/pxoxsTRedhKq6zrzCfT8qfh3C1w==</SignatureValue>
    </Signature>
</Receipt>
```

Ein Produktbeleg sieht wie folgt aus.

> [!NOTE]
> Dieses Beispiel ist formatiert, damit das XML lesbar ist. Echte Produkt-Belege enthalten keine Leerzeichen zwischen Elementen.

> [!div class="tabbedCodeSnippets"]
```xml
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:08:52Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>Uvi8jkTYd3HtpMmAMpOm94fLeqmcQ2KCrV1XmSuY1xI=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>TT5fDET1X9nBk9/yKEJAjVASKjall3gw8u9N5Uizx4/Le9RtJtv+E9XSMjrOXK/TDicidIPLBjTbcZylYZdGPkMvAIc3/1mdLMZYJc+EXG9IsE9L74LmJ0OqGH5WjGK/UexAXxVBWDtBbDI2JLOaBevYsyy+4hLOcTXDSUA4tXwPa2Bi+BRoUTdYE2mFW7ytOJNEs3jTiHrCK6JRvTyU9lGkNDMNx9loIr+mRks+BSf70KxPtE9XCpCvXyWa/Q1JaIyZI7llCH45Dn4SKFn6L/JBw8G8xSTrZ3sBYBKOnUDbSCfc8ucQX97EyivSPURvTyImmjpsXDm2LBaEgAMADg==</SignatureValue>
    </Signature>
</Receipt>
```

Sie können beide Belegbeispiele verwenden, um den Überprüfungscode zu testen. Weitere Informationen zum Inhalt des Belegs finden Sie unter [Beschreibungen von Elementen und Attributen](#receipt-descriptions).

## <a name="validating-a-receipt"></a>Überprüfen eines Belegs

Um die Authentizität eines Belegs zu überprüfen, muss das Back-End-System (ein Webdienst oder ähnlich) die Belegsignatur mithilfe des öffentlichen Zertifikats überprüfen. Um dieses Zertifikat abzurufen, verwenden Sie die URL ```https://go.microsoft.com/fwlink/p/?linkid=246509&cid=CertificateId```, wobei ```CertificateId``` der **CertificateId**-Wert im Beleg ist.

Im Folgenden sehen Sie ein Beispiel für diesen Überprüfungsvorgang. Dieser Code wird in einer .NET Framework-Konsolenanwendung ausgeführt, die einen Verweis auf die **System.Security**-Assembly enthält.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ReceiptVerificationSample](./code/ReceiptVerificationSample/cs/Program.cs#ReceiptVerificationSample)]

<span id="receipt-descriptions" />

## <a name="element-and-attribute-descriptions-for-a-receipt"></a>Beschreibungen von Elementen und Attributen eines Belegs

In diesem Abschnitt werden die Elemente und Attribute in einen Beleg beschrieben.

### <a name="receipt-element"></a>Element „Receipt“

Das Stammelement dieser Datei ist das Element **Receipt**, das Informationen zu App- und In-App-Käufen enthält. Dieses Element enthält die folgenden untergeordneten Elemente.

|  Element  |  Erforderlich  |  Anzahl  |  Beschreibung   |
|-------------|------------|--------|--------|
|  [AppReceipt](#appreceipt)  |    Nein        |  0 oder 1  |  Enthält Kaufinformationen für die aktuelle App.            |
|  [ProductReceipt](#productreceipt)  |     Nein       |  0 oder mehr    |   Enthält Informationen zu einem In-App-Kauf für die aktuelle App.     |
|  Signatur  |      „Ja“      |  1   |   Dieses Element ist ein standardmäßiges [XML-DSIG-Konstrukt](https://www.w3.org/TR/xmldsig-core/). Es enthält ein **SignatureValue**-Element, das die Signatur enthält, die Sie für die Überprüfung des Belegs verwenden können, und ein **SignedInfo**-Element.      |

**Receipt** hat die folgenden Attribute.

|  Attribut  |  Beschreibung   |
|-------------|-------------------|
|  **Version**  |    Die Versionsnummer des Belegs.            |
|  **CertificateId**  |     Der Fingerabdruck des Zertifikats, der für die Signierung des Belegs verwendet wurde.          |
|  **ReceiptDate**  |    Das Datum, an dem der Beleg signiert und heruntergeladen wurde.           |  
|  **ReceiptDeviceId**  |   Identifiziert das Gerät, das für die Anforderung dieses Belegs verwendet wurde.         |  |

<span id="appreceipt" />

### <a name="appreceipt-element"></a>Element „AppReceipt“

Dieses Element enthält Kaufinformationen für die aktuelle App.

**AppReceipt** hat die folgenden Attribute.

|  Attribut  |  Beschreibung   |
|-------------|-------------------|
|  **Id**  |    Identifiziert den Kauf.           |
|  **AppId**  |     Der Paketfamilienname-Wert, den das Betriebssystem für die App verwendet.           |
|  **LicenseType**  |    **Full**, wenn der Benutzer die Vollversion der App gekauft hat. **Trial**, wenn der Benutzer eine Testversion der App heruntergeladen hat.           |  
|  **PurchaseDate**  |    Das Datum, an dem die App gekauft wurde.          |  |

<span id="productreceipt" />

### <a name="productreceipt-element"></a>Element „ProductReceipt“

Dieses Element enthält Informationen zu einem In-App-Kauf für die aktuelle App.

**ProductReceipt** hat die folgenden Attribute.

|  Attribut  |  Beschreibung   |
|-------------|-------------------|
|  **Id**  |    Identifiziert den Kauf.           |
|  **AppId**  |     Identifiziert die App, über die der Benutzer den Kauf durchgeführt hat.           |
|  **ProductId**  |     Identifiziert das gekaufte Produkt.           |
|  **ProductType**  |    Legt den Produkttyp fest. Zurzeit wird nur der Wert **Durable** unterstützt.          |  
|  **PurchaseDate**  |    Das Datum, an dem der Kauf erfolgte.          |  |

 

 
