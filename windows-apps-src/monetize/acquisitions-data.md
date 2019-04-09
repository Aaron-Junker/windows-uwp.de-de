---
description: Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API zum Abrufen von aggregierten Kaufdaten werden im JSON-Format für UWP-apps und Xbox One Spiele, die über die Xbox-Entwickler-Portal (XDP) erfasste und im XDP Analytics-Dashboard verfügbar waren.
title: Abrufen von Übernahmen Daten für Ihre Spiele und apps
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, Anzeigennetzwerke, App-Metadaten
ms.localizationpriority: medium
ms.openlocfilehash: beca5620f25713e8a07e5dbaf64e955d920702a7
ms.sourcegitcommit: df8e4143e81a1c5fe1aa5f14407b8dd5f155a12e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829864"
---
# <a name="get-acquisitions-data-for-your-games-and-apps"></a>Abrufen von Übernahmen Daten für Ihre Spiele und apps 
Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API zum Abrufen von aggregierten Kaufdaten werden im JSON-Format für UWP-apps und Xbox One Spiele, die über die Xbox-Entwickler-Portal (XDP) erfasste und im XDP Analytics-Dashboard verfügbar waren. 

> [!NOTE]
> Diese API bietet keine täglichen aggregierte Daten vor dem 1. Oktober 2016. 

## <a name="prerequisites"></a>Vorraussetzungen
Zur Verwendung dieser Methode sind folgende Schritte erforderlich: 

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md) für die Microsoft Store-Analyse-API. 
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen. 

## <a name="request"></a>Anforderung
### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI |
| --- | --- |
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions` |

### <a name="request-header"></a>Anforderungsheader

| Header | Typ | Beschreibung |
| --- | --- | --- |
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** `<token>`. |

### <a name="request-parameters"></a>Anforderungsparameter

| Parameter | Typ | Beschreibung | Erforderlich |
| --- | --- | --- | --- |
| applicationId | String | Die Produkt-ID des Xbox One Spiels, für das Sie Kaufdaten abrufen. Navigieren Sie zu Ihr Spiel in der XDP Analytics-Anwendung, und rufen Sie die Productid aus der URL, rufen Sie die Produkt-ID Ihres Spiels. Wenn Sie die Übernahmen Daten aus den Partner Center-Analysebericht herunterladen, ist die Produkt-ID auch in das TSV-Datei enthalten.  | Ja |
| startDate | date | Das Startdatum im Datumsbereich der Kaufdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum.  | Nein |
| endDate | date | Das Enddatum im Datumsbereich der Kaufdaten, die abgerufen werden sollen. Der Standardwert ist das aktuelle Datum.  | Nein |
| top | Ganzzahl | Die Anzahl der Datenzeilen, die zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können.  | Nein |
| skip | Ganzzahl | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Z. B. *oben = 10000 und überspringen = 0* Ruft ab, die zuerst 10000 Zeilen mit Daten, *oben = 10000 und überspringen = 10000* Ruft ab, die als Nächstes 10000 Zeilen von Daten und So weiter.  | Nein |
| filter | String | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede Anweisung enthält einen Feldnamen aus dem Antworttext und einen Wert, die mit den Operatoren **eq** oder **ne** verknüpft sind. Anweisungen können mit **and** oder **or** kombiniert werden. Zeichenfolgenwerte müssen in einfache Anführungszeichen im Parameter "Filter" gesetzt werden. Beispiel: *filter=market eq 'US' and gender eq 'm'*.  <br/> Sie können die folgenden Felder aus dem Antworttext angeben: <ul><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | Nein |
| aggregationLevel | String | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: **day**, **week** oder **month**. Wenn keine Angabe erfolgt, lautet der Standardwert **day**.  | Nein |
| orderby | String | Eine Anweisung, die die Ergebnisdatenwerte für die einzelnen Käufe anfordert. Die Syntax ist *Orderby = Feld [Reihenfolge], [Order]...* Der Parameter *field* kann eine der folgenden Zeichenfolgen sein: <ul><li>**date**</li><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> Der Parameter *order* ist optional und kann **asc** oder **desc** sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standard ist **asc**. Dies ist eine Beispielzeichenfolge für *orderby*: *orderby=date,market*  | Nein |
| groupby | String | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben: <ul><li>**date**</li><li>**applicationName**</li><li>**acquisitionType**</li><li>**ageGroup**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter *groupby* angegeben sind, sowie die folgenden: <ul><li>**date**</li><li>**applicationId**</li><li>**acquisitionQuantity**</li></ul> Die *Groupby* Parameter kann mit dem AggregationLevel-Parameter verwendet werden. Zum Beispiel: *& Groupby = Altersgruppe, Markt & AggregationLevel = Woche*  | Nein |

### <a name="request-example"></a>Anforderungsbeispiel
Das folgende Beispiel zeigt verschiedene Anforderungen für den Abruf von Spielekaufdaten für Xbox One. Ersetzen Sie die *ApplicationId* Wert mit dem Productid für Ihr Spiel.  

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&top=10&skip=0 HTTP/1.1 
Authorization: Bearer <your access token> 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1 
Authorization: Bearer <your access token> 
```

## <a name="response"></a>Antwort

### <a name="response-body"></a>Antworttext
| Wert | Typ | Beschreibung |
| --- | --- | --- |
| Wert | array | Ein Array von Objekten, die aggregierte Kaufdaten für das Spiel enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Kaufwerte](#acquisition-values). |
| @nextLink | String | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Kaufdaten für die Abfrage gibt. |
| TotalCount | Ganzzahl | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage. |

### <a name="acquisition-values"></a>Kaufwerte 
Elemente im Array *Value* enthalten die folgenden Werte. 

| Wert | Typ | Beschreibung |
| --- | --- | --- |
| date | String | Das erste Datum im Datumsbereich für die Kaufdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| applicationId | String | Die Produkt-ID des Xbox One Spiels, für das Sie Kaufdaten abrufen. |
| applicationName | String | Der Anzeigename des Spiels. |
| acquisitionType | String | Eine der folgenden Zeichenfolgen, die den Typ des Kaufes angibt:  <ul><li>**kostenlos**</li><li>**Trial**</li><li>**Paid**</li><li>**Angebotscode**</li><li>**IAP**</li><li>**Abonnement Iap**</li><li>**Private-Zielgruppe**</li><li>**Pre-Reihenfolge**</li><li>**Xbox Game Pass** (oder **Game Pass** beim Abfragen von Daten vor dem 23. März 2018)</li><li>**Datenträger**</li><li>**Im Voraus bezahlten Code**</li><li>**Erfolgt die Abrechnung Pre-Reihenfolge**</li><li>**Abgebrochene Pre-Reihenfolge**</li><li>**Fehlerhafte Pre-Reihenfolge**</li></ul> |
| Alter | String | Eine der folgenden Zeichenfolgen, die die Altersgruppe des Benutzers anzeigt, der den Kauf getätigt hat: <ul><li>**kleiner als die 13**</li><li>**13-17**</li><li>**18-24**</li><li>**25-34**</li><li>**35-44**</li><li>**44-55**</li><li>**mehr als 55**</li><li>**Unbekannt**</li></ul> |
| deviceType | String | Eine der folgenden Zeichenfolgen, die den Typ des Geräts angibt, mit dem der Kauf getätigt wurde: <ul><li>**PC**</li><li>**Phone**</li><li>**Verwaltungskonsole**</li><li>**IoT**</li><li>**Server**</li><li>**Tablet**</li><li>**Holographic**</li><li>**Unbekannt**</li></ul> |
| gender | String | Eine der folgenden Zeichenfolgen, die das Geschlecht des Benutzer angibt, der den Kauf getätigt hat: <ul><li>**m**</li><li>**f**</li><li>**Unbekannt**</li></ul> |
| market | String | Die ISO 3166-Ländercode des Markts, in dem der Kauf erfolgte. |
| osVersion | String | Die Version des Betriebssystems, auf dem der Kauf ausgeführt wurde. Bei dieser Methode ist dieser Wert immer **Windows 10**. |
| paymentInstrumentType | String | Eine der folgenden Zeichenfolgen, die die Kaufanweisungen anzeigt, die für den Erwerb verwendet wurde: <ul><li>**Kreditkarte**</li><li>**Direkte Debitkarte**</li><li>**Abgeleitete erwerben**</li><li>**MS-Lastenausgleich**</li><li>**Mobilfunkanbieter**</li><li>**Online-Überweisung**</li><li>**PayPal**</li><li>**Split-Transaktion**</li><li>**Token einlösen**</li><li>**0 (null) entrichteten Gesamtbetrag**</li><li>**eWallet**</li><li>**Unbekannt**</li></ul> |
| sandboxId | String | Das Sandbox-ID, die für das Spiel erstellt wurde. Dies kann der Wert sein **RETAIL** oder eine private Sandkasten-ID |
| storeClient | String | Eine der folgenden Zeichenfolgen, die die Version des Store anzeigt, wo der Kauf getätigt wurde: <ul><li>**Windows Phone Store (Client)**</li><li>**Microsoft Store (Client)** (oder **Windows Store (Client)** bei Abfragen von Daten vor dem 23. März 2018) </li><li>**Microsoft Store (Web)** (oder **Windows Store (Web)** bei Abfragen von Daten vor dem 23. März 2018) </li><li>**VPP von Organisationen**</li><li>**Andere**</li></ul> |
| xboxTitleId | String | Die Xbox Live Titel-ID (dargestellt in hexadezimalen Wert), die vom der Xbox Developer-Portal (XDP) für Xbox Live-fähige Spiele zugewiesen wurde. |
| acquisitionQuantity | number | Die Anzahl der Käufe, die während der angegebenen Aggregationsebene erfolgten. |
| purchasePriceUSDAmount | number | Der vom Kunden gezahlte Betrag für den Kauf, der mithilfe des monatlichen Wechselkurses in USD konvertiert wurde. |
| purchaseTaxUSDAmount | number | Der in USD konvertierte Steuerbetrag, der für den Kauf fällig ist. |
| localCurrencyCode | String | Lokale Währungscode auf Basis des Ursprungslands des Partner Center-Kontos.  |
| xboxProductId | String | Xbox-Produkt-ID des Produkts aus XDP, falls zutreffend.  |
| availabilityId | String | Verfügbarkeitsgruppen-ID des Produkts aus XDP, falls zutreffend.  |
| skuId | String | SKU-ID des Produkts aus XDP, falls zutreffend.  |
| skuDisplayName  | String | SKU Anzeigename des Produkts aus XDP, falls zutreffend.  |
| xboxParentProductId | String | Xbox übergeordneten Produkt-ID des Produkts aus XDP, falls zutreffend.  |
| parentProductName | String | Übergeordnete Product Name des Produkts aus XDP, falls zutreffend.  |
| productTypeName | String | Produkt-Typname des Produkts aus XDP, falls zutreffend.  |
| purchaseTaxType | String | Erwerben Sie Steuer-Typ des Produkts aus XDP, falls zutreffend.  |
| purchasePriceLocalAmount | number | Kaufsumme Preis lokalen des Produkts aus XDP, falls zutreffend.  |
| purchaseTaxLocalAmount | number | Erwerben Sie lokale Steuerbetrag des Produkts aus XDP, falls zutreffend.  |

### <a name="response-example"></a>Antwortbeispiel
Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung. 

```JSON
{ 
    "Value": [ 
        { 
            "date": "2019-01-15T01:00:00.0000000Z", 
            "applicationId": "9WZDNCRFHXHT", 
            "applicationName": null, 
            "acquisitionType": "Paid", 
            "age": null, 
            "deviceType": "Phone", 
            "gender": null, 
            "market": "US", 
            "osVersion": "Windows 10", 
            "paymentInstrumentType": null, 
            "sandboxId": "RETAIL", 
            "storeClient": "Microsoft Store (client)", 
            "xboxTitleId": null, 
            "localCurrencyCode": "USD", 
            "xboxProductId": null, 
            "availabilityId": "B42LRTSZ2MCJ", 
            "skuId": "0010", 
            "skuDisplayName": null, 
            "xboxParentProductId": null, 
            "parentProductName": null, 
            "productTypeName": "Game", 
            "purchaseTaxType": "TaxesNotIncluded", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 3.08, 
            "purchasePriceLocalAmount": 3.08, 
            "purchaseTaxUSDAmount": 0.09, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": "acquisitions?applicationId=9WZDNCRFHXHT&aggregationLevel=day&startDate=2017-01-01T08:00:00.0000000Z&endDate=2019-01-16T08:44:15.6045249Z&top=1&skip=1", 
    
    "TotalCount": 12221 
} 
```

 
