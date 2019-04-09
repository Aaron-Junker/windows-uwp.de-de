---
description: Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API zum Abrufen von aggregate-Add-On Kaufdaten werden im JSON-Format für UWP-apps und Xbox One Spiele, die über die Xbox-Entwickler-Portal (XDP) erfasste und im XDP Analytics Partner Center-Dashboard verfügbar waren.
title: Abrufen von Add-On-Akquisitionen Daten für Ihre Spiele und apps
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, Anzeigennetzwerke, App-Metadaten
ms.localizationpriority: medium
ms.openlocfilehash: 518648d52c613a3dd5f1bca0d34a7f533b59733f
ms.sourcegitcommit: df8e4143e81a1c5fe1aa5f14407b8dd5f155a12e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829859"
---
# <a name="get-add-on-acquisitions-data-for-your-games-and-apps"></a>Abrufen von Add-On-Akquisitionen Daten für Ihre Spiele und apps 
Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API zum Abrufen von aggregate-Add-On Kaufdaten werden im JSON-Format für UWP-apps und Xbox One Spiele, die über die Xbox-Entwickler-Portal (XDP) erfasste und im XDP Analytics Partner Center-Dashboard verfügbar waren. 

## <a name="prerequisites"></a>Vorraussetzungen
Zur Verwendung dieser Methode sind folgende Schritte erforderlich: 

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md) für die Microsoft Store-Analyse-API. 
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen. 

> [!NOTE]
> Diese API bietet keine täglichen aggregierte Daten vor dem 1. Oktober 2016. 

## <a name="request"></a>Anforderung

### <a name="request-syntax"></a>Anforderungssyntax
| Methode | Anforderungs-URI |
| --- | --- | 
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions` |

### <a name="request-header"></a>Anforderungsheader 
| Header | Typ | Beschreibung | 
| --- | --- | --- |
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** `<token>`. |

### <a name="request-parameters"></a>Anforderungsparameter
Die *ApplicationId* oder *AddonProductId* -Parameter ist erforderlich. Um Kaufdaten für alle für die App registrierten Add-Ons abzurufen, geben Sie den Parameter *applicationId* an. Um die Kaufdaten für eine einzelne Add-on abzurufen, geben die *AddonProductId* Parameter. Wenn Sie beide Parameter angeben, wird der Parameter *applicationId* ignoriert. 

| Parameter | Typ | Beschreibung | Erforderlich | 
| --- | --- | --- | --- |
| applicationId | String | Die *"ProductID"* des Xbox One-Spiels für die Sie Kaufdaten abrufen. Zum Abrufen der *"ProductID"* Ihres Spiels, navigieren Sie zu Ihr Spiel im Programm Analytics XDP und Abrufen der *"ProductID"* aus der URL. Sie können auch, wenn Sie die Übernahmen Daten aus den Partner Center-Analysebericht Herunterladen der *"ProductID"* befindet sich im TSV-Datei. | Ja |
| addonProductId | String | Die *"ProductID"* das Add-On für die Kaufdaten werden abgerufen werden sollen. | Ja |
| startDate | date | Das Startdatum im Datumsbereich der Add-On-Kaufdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. | Nein |
| endDate | date | Das Enddatum im Datumsbereich der Add-On-Kaufdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum. | Nein |
| top | int | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. | Nein |
| skip | int | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. | Nein |
| filter | String | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, der die Operatoren Eq "oder" Ne zugeordnet sind, und Anweisungen mit kombiniert werden können und bzw. oder. Zeichenfolgenwerte müssen in einfache Anführungszeichen im Parameter "Filter" gesetzt werden. Z. B. Filtern = Markt Eq 'US' und Geschlecht Eq bin ". <br/> Sie können die folgenden Felder aus dem Antworttext angeben: <ul><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | Nein |
| aggregationLevel | String | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: **day**, **week** oder **month**. Wenn keine Angabe erfolgt, lautet der Standardwert **day**. | Nein |
| orderby | String | Eine Anweisung, die die Ergebnisdatenwerte für die einzelnen Add-On-Käufe anfordert. Die Syntax ist *Orderby = Feld [Reihenfolge], [Order]...* Der Parameter *field* kann eine der folgenden Zeichenfolgen sein: <ul><li>**date**</li><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**orderName**</li></ul> Die Order-Parameter ist optional und kann **Asc** oder **Desc** angeben von auf- oder absteigender Reihenfolge für jedes Feld. Der Standard ist **asc**. <br/> Dies ist eine Beispielzeichenfolge für *orderby*: *orderby=date,market* | Nein |
| groupby | String | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben: <ul><li>**date**</li><li>**applicationName**</li><li>**addonProductName**</li> <li>**acquisitionType**</li><li>**age**</li> <li>**storeClient**</li><li>**gender**</li> <li>**market**</li> <li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter *groupby* angegeben sind, sowie die folgenden: <ul><li>**date**</li><li>**applicationId**</li><li>**addonProductId**</li><li>**acquisitionQuantity**</li></ul> Der Groupby-Parameter kann verwendet werden, mit der *AggregationLevel* Parameter. Zum Beispiel: *& Groupby = Age, Markt & AggregationLevel = Woche* | Nein |

### <a name="request-example"></a>Anforderungsbeispiel
Die folgenden Beispiele zeigen verschiedene Anforderungen für das Abrufen von Add-On-Kaufdaten. Ersetzen Sie die *AddonProductId* und *ApplicationId* Werte mit die entsprechende Store-ID für Ihre-Add-On oder die app. 

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0 HTTP/1.1 

Authorization: Bearer <your access token> 

 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0&filter=market eq 'GB' and gender eq 'm' HTTP/1.1 

Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

### <a name="response-body"></a>Antworttext

| Wert | Typ | Beschreibung |
| --- | --- | --- |
| Wert | array | Ein Array von Objekten, die aggregierte Add-On-Kaufdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Add-On-Kaufwerte](#add-on-acquisition-values). |
| @nextLink | String | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Add-On-Kaufdaten für die Abfrage gibt. |
| TotalCount | int | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage. |

### <a name="add-on-acquisition-values"></a>Add-On-Kaufwerte
Elemente im Array Wert werden die folgenden Werte enthalten.

| Wert | Typ | Beschreibung | 
| --- | --- | --- |
| date | String | Das erste Datum im Datumsbereich für die Kaufdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| addonProductId | String | Die *"ProductID"* das Add-On für den Sie Kaufdaten abrufen. |
| addonProductName | String | Der Anzeigename des Add-Ons. Dieser Wert wird nur in der Antwort angezeigt, wenn die *AggregationLevel* Parametersatz zu **Tag**, es sei denn, Sie geben die **AddonProductName** -Feld in der *Groupby* Parameter. |
| applicationId | String | Die *"ProductID"* der app für das Add-on Kaufdaten werden abgerufen werden sollen. |
| applicationName | String | Der Anzeigename des Spiels. |
| deviceType | String | Eine der folgenden Zeichenfolgen, die den Typ des Geräts angibt, mit dem der Kauf getätigt wurde: <ul><li>"PC"</li><li>"Phone"</li><li>"Console"</li><li>"IoT"</li><li>"Server"</li><li>"Tablet"</li><li>"Holographic"</li><li>"Unknown"</li></ul> |
| storeClient | String | Eine der folgenden Zeichenfolgen, die die Version des Store anzeigt, wo der Kauf getätigt wurde: <ul><li>"Windows Phone Store (Client)"</li><li>"Microsoft Store (Client)" (oder "Windows Store (Client)" beim Abfragen von Daten vor dem 23. März 2018)</li><li>"Microsoft Store (Web)" (oder "Windows Store (Web)" beim Abfragen von Daten vor dem 23. März 2018)</li><li>"Volume-Purchase von Unternehmen"</li><li>"Other"</li></ul> |
| osVersion | String | Die Version des Betriebssystems, auf dem der Kauf ausgeführt wurde. Für diese Methode ist dieser Wert immer "Windows 10". |
| market | String | Die ISO 3166-Ländercode des Markts, in dem der Kauf erfolgte. |
| gender | String | Eine der folgenden Zeichenfolgen, die das Geschlecht des Benutzer angibt, der den Kauf getätigt hat: <ul><li>"m"</li><li>"f"</li><li>"Unknown"</li></ul> |
| Alter | String | Eine der folgenden Zeichenfolgen, die die Altersgruppe des Benutzers anzeigt, der den Kauf getätigt hat: <ul><li>"kleiner als"13 "</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>"größer als 55"</li><li>"Unknown"</li></ul> |
| acquisitionType | String | Eine der folgenden Zeichenfolgen, die den Typ des Kaufes angibt: <ul><li>"Free" </li><li>"Trial" </li><li>"Kostenpflichtiges"</li><li>"Angebotscode" </li><li>"Iap"</li><li>"Abonnement Iap"</li><li>"Audience" Private "</li><li>"Order" vor "</li><li>"Xbox Game Pass" (oder "Spiel übergeben", wenn Abfragen für Daten vor dem 23. März 2018)</li><li>"Disk"</li><li>"Im Voraus bezahlten Code"</li><li>"Vor Reihenfolge berechnet"</li><li>"Vor Auftrag abgebrochen"</li><li>"Fehler bei der Pre-Auftrag"</li></ul> |
| acquisitionQuantity | Ganzzahl | Die Anzahl der Käufe, die ausgeführt wurden. |
| inAppProductId | String | Produkt-ID des Produkts, in denen dieses Add-on verwendet wird.  |
| inAppProductName | String | Der Produktname des Produkts, in denen dieses Add-on verwendet wird.  |
| paymentInstrumentType | String | Zahlungsmitteltyp für den tokenabruf verwendet.  |
| sandboxId | String | Die Sandkasten-ID für das Spiel erstellt. Dies kann der Wert sein **RETAIL** oder eine private Sandkasten-ID  |
| xboxTitleId | String | Xbox-Titel-ID des Produkts aus XDP, falls zutreffend.  |
| localCurrencyCode | String | Lokale Währungscode auf Basis des Ursprungslands des Partner Center-Kontos.  |
| xboxProductId | String | Xbox-Produkt-ID des Produkts aus XDP, falls zutreffend.  |
| availabilityId | String | Verfügbarkeitsgruppen-ID des Produkts aus XDP, falls zutreffend.  |
| skuId | String | SKU-ID des Produkts aus XDP, falls zutreffend.  |
| skuDisplayName | String | SKU-Anzeigename des Produkts aus XDP, falls zutreffend.  |
| xboxParentProductId | String | Xbox übergeordneten Produkt-ID des Produkts aus XDP, falls zutreffend.  |
| parentProductName | String | Übergeordnete Product Name des Produkts aus XDP, falls zutreffend.  |
| productTypeName | String | Produkt-Typname des Produkts aus XDP, falls zutreffend.  |
| purchaseTaxType | String | Erwerben Sie Steuer-Typ des Produkts aus XDP, falls zutreffend.  |
| purchasePriceUSDAmount | number | Der Betrag bezahlt der Kunde für das Add-on, konvertiert in US-Dollar.  |
| purchasePriceLocalAmount | number | Der Steuerbetrag, der auf das Add-on angewendet wird.  |
| purchaseTaxUSDAmount | number | Steuerbetrag angewendet, für das Add-on, konvertiert in US-Dollar.  |
| purchaseTaxLocalAmount | number | Erwerben Sie lokale Steuerbetrag des Produkts aus XDP, falls zutreffend.  |

### <a name="response-example"></a>Antwortbeispiel
Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung. 

```JSON
{ 
  "Value": [ 
    { 
            "inAppProductId": "9NBLGGH1864K", 
            "inAppProductName": "866879", 
            "addonProductId": "9NBLGGH1864K", 
            "addonProductName": "866879", 
            "date": "2017-11-05", 
            "applicationId": "9WZDNCRFJ314", 
            "applicationName": "Tetris Blitz", 
            "acquisitionType": "Iap", 
            "age": "35-49", 
            "deviceType": "Phone", 
            "gender": "m", 
            "market": "US", 
            "osVersion": "Windows Phone 8.1", 
            "paymentInstrumentType": "Credit Card", 
            "sandboxId": "RETAIL", 
            "storeClient": "Windows Phone Store (client)", 
            "xboxTitleId": "", 
            "localCurrencyCode": "USD", 
            "xboxProductId": "00000000-0000-0000-0000-000000000000", 
            "availabilityId": "", 
            "skuId": "", 
            "skuDisplayName": "Full", 
            "xboxParentProductId": "", 
            "parentProductName": "Tetris Blitz", 
            "productTypeName": "Add-On", 
            "purchaseTaxType": "", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 1.08, 
            "purchasePriceLocalAmount": 0.09, 
            "purchaseTaxUSDAmount": 1.08, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": null, 
    
    "TotalCount": 7601 
} 
```