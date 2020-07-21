---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um aggregierte Erfassungsdaten im JSON-Format für UWP-apps und Xbox One-Spiele zu erhalten, die über das Xbox Developer Portal (XDP) erfasst und im XDP-Analyse Dashboard zur Verfügung stehen.
title: Abrufen von Kaufdaten für Ihre Spiele und Apps
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, Ankündigungs Netzwerk, App-Metadaten
ms.localizationpriority: medium
ms.openlocfilehash: f7db2659bc08ab6da426de94c7dcaf1fb8a7d802
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493225"
---
# <a name="get-acquisitions-data-for-your-games-and-apps"></a>Abrufen von Kaufdaten für Ihre Spiele und Apps 
Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um aggregierte Erfassungsdaten im JSON-Format für UWP-apps und Xbox One-Spiele zu erhalten, die über das Xbox Developer Portal (XDP) erfasst und im XDP-Analyse Dashboard zur Verfügung stehen. 

> [!NOTE]
> Diese API stellt vor dem 1. Oktober 2016 keine täglichen aggregierten Daten bereit. 

## <a name="prerequisites"></a>Voraussetzungen
Zur Verwendung dieser Methode sind folgende Schritte erforderlich: 

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md) für die Microsoft Store Analytics-API erfüllen. 
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen. 

## <a name="request"></a>Anforderung
### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI |
| --- | --- |
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions` |

### <a name="request-header"></a>Anforderungsheader

| Header | type | BESCHREIBUNG |
| --- | --- | --- |
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD Zugriffs Token im Formular **Träger** `<token>` . |

### <a name="request-parameters"></a>Anforderungsparameter

| Parameter | type | BESCHREIBUNG | Erforderlich |
| --- | --- | --- | --- |
| applicationId | Zeichenfolge | Die Produkt-ID des Xbox One-Spiels, für das Sie die Erwerbs Daten abrufen. Um die Produkt-ID Ihres Spiels zu erhalten, navigieren Sie zu Ihrem Spiel im XDP-Analyseprogramm, und rufen Sie die Produkt-ID aus der URL ab. Wenn Sie Ihre Erstellungs Daten aus dem Partner Center Analytics-Bericht herunterladen, ist die Produkt-ID in der TSV-Datei enthalten.  | Ja |
| startDate | date | Das Startdatum im Datumsbereich der Kaufdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt.  | Nein |
| endDate | date | Das Enddatum im Datumsbereich der Kaufdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt.  | Nein |
| top | integer | Die Anzahl der Daten Zeilen, die zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können.  | Nein |
| skip | integer | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise ruft *Top = 10000 und Skip = 0* die ersten 10000 Zeilen der Daten ab, *Top = 10000 und Skip = 10000* Ruft die nächsten 10000 Daten Zeilen ab usw.  | Nein |
| filter | Zeichenfolge | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede-Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, die den **EQ** -oder **ne** -Operatoren zugeordnet sind, und-Anweisungen können mithilfe von **and** oder **or**kombiniert werden. Zeichenfolgenwerte im Parameter filter müssen von einfachen Anführungszeichen eingeschlossen werden. Beispiel: *Filter = Market EQ ' US ' und Geschlecht EQ 'm '*.  <br/> Sie können die folgenden Felder aus dem Antworttext angeben: <ul><li>**acquisitionType**</li><li>**Eder**</li><li>**storeClient**</li><li>**gender**</li><li>**Marktforschungs**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | Nein |
| aggregationLevel | Zeichenfolge | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: **day**, **week** oder **month**. Wenn keine Angabe erfolgt, lautet der Standardwert **day**.  | Nein |
| orderby | Zeichenfolge | Eine Anweisung, die die Ergebnisdatenwerte für die einzelnen Käufe anfordert. Syntax *: OrderBy = Field [Order], Field [Order],...* Der *Feld* Parameter kann eine der folgenden Zeichen folgen sein: <ul><li>**date**</li><li>**acquisitionType**</li><li>**Eder**</li><li>**storeClient**</li><li>**gender**</li><li>**Marktforschungs**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentinstrumenttype**</li><li>**sandboxId**</li><li>**xboxtitleidhex**</li></ul> Der Parameter *order* ist optional und kann **asc** oder **desc** sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standardwert ist **ASC**. Hier ist ein Beispiel für eine *OrderBy* -Zeichenfolge: *OrderBy = Date, Market*  | Nein |
| groupby | Zeichenfolge | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben: <ul><li>**date**</li><li>**applicationName**</li><li>**acquisitionType**</li><li>**ageGroup**</li><li>**storeClient**</li><li>**gender**</li><li>**Marktforschungs**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentinstrumenttype**</li><li>**sandboxId**</li><li>**xboxtitleidhex**</li></ul> Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter *groupby* angegeben sind, sowie die folgenden: <ul><li>**date**</li><li>**applicationId**</li><li>**acquisitionQuantity**</li></ul> Der Parameter *groupby* kann mit dem Parameter aggregationLevel verwendet werden. Beispiel: *&GroupBy = AgeGroup, Market&aggregationlevel = Week*  | Nein |

### <a name="request-example"></a>Anforderungsbeispiel
Im folgenden Beispiel werden mehrere Anforderungen zum Abrufen von Xbox One-Spiel Erfassungsdaten veranschaulicht. Ersetzen Sie den Wert *ApplicationId* durch die Produkt-ID für das Spiel.  

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&top=10&skip=0 HTTP/1.1 
Authorization: Bearer <your access token> 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1 
Authorization: Bearer <your access token> 
```

## <a name="response"></a>Antwort

### <a name="response-body"></a>Antworttext
| Wert | type | BESCHREIBUNG |
| --- | --- | --- |
| Wert | array | Ein Array von-Objekten, die aggregierte Erfassungsdaten für das Spiel enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Kaufwerte](#acquisition-values). |
| @nextLink | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Kaufdaten für die Abfrage gibt. |
| TotalCount | integer | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage. |

### <a name="acquisition-values"></a>Kaufwerte 
Elemente im Array *Value* enthalten die folgenden Werte. 

| Wert | type | BESCHREIBUNG |
| --- | --- | --- |
| date | Zeichenfolge | Das erste Datum im Datumsbereich für die Kaufdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| applicationId | Zeichenfolge | Die Produkt-ID des Xbox One-Spiels, für das Sie die Erwerbs Daten abrufen. |
| applicationName | Zeichenfolge | Der Anzeige Name des Spiels. |
| acquisitionType | Zeichenfolge | Eine der folgenden Zeichen folgen, die den Typ des Erwerbs angibt:  <ul><li>**Free**</li><li>**Testversion**</li><li>**Bezahlt**</li><li>**Aktions Code**</li><li>**IAP**</li><li>**Abonnement-IAP**</li><li>**Private Zielgruppe**</li><li>**Vorab anordnen**</li><li>**Xbox Game Pass** (oder **Game Pass** bei der Abfrage von Daten vor dem 23. März 2018)</li><li>**Disk**</li><li>**Prepaid-Code**</li><li>**Vorabbestellung abgerechnet**</li><li>**Abbruch vor der Bestellung**</li><li>**Fehler bei der Vorabbestellung**</li></ul> |
| age | Zeichenfolge | Eine der folgenden Zeichen folgen, die die Altersgruppe des Benutzers angibt, der die Übernahme durchgeführt hat: <ul><li>**Weniger als 13**</li><li>**13-17**</li><li>**18-24**</li><li>**25-34**</li><li>**35-44**</li><li>**44-55**</li><li>**Größer als 55**</li><li>**Unbekannt**</li></ul> |
| deviceType | Zeichenfolge | Eine der folgenden Zeichen folgen, die den Typ des Geräts angibt, das den Erwerb abgeschlossen hat: <ul><li>**PC**</li><li>**Smartphone**</li><li>**Konsole-Xbox One**</li><li>**Konsole-Xbox Series X**</li><li>**IoT**</li><li>**Server**</li><li>**Tablet**</li><li>**Holographic**</li><li>**Unbekannt**</li></ul> |
| gender | Zeichenfolge | Eine der folgenden Zeichen folgen, die das Geschlecht des Benutzers angibt, der die Übernahme durchgeführt hat: <ul><li>**m**</li><li>**f**</li><li>**Unbekannt**</li></ul> |
| market | Zeichenfolge | Die ISO 3166-Ländercode des Markts, in dem der Kauf erfolgte. |
| osVersion | Zeichenfolge | Die Version des Betriebssystems, auf dem der Kauf ausgeführt wurde. Bei dieser Methode ist dieser Wert immer **Windows 10**. |
| paymentinstrumenttype | Zeichenfolge | Eine der folgenden Zeichen folgen, die die für den Erwerb verwendete Zahlungsanweisung angibt: <ul><li>**Kreditkarte**</li><li>**Direkt Debitkarte**</li><li>**Abherder Kauf**</li><li>**MS-Saldo**</li><li>**Mobilfunkanbieter**</li><li>**Online Bank Übertragung**</li><li>**PayPal**</li><li>**Transaktion aufteilen**</li><li>**Tokeneinlösung**</li><li>**Keine kostenpflichtige Menge**</li><li>**eWallet**</li><li>**Unbekannt**</li></ul> |
| sandboxId | Zeichenfolge | Die Sandbox-ID, die für das Spiel erstellt wurde. Dabei kann es sich um den Wert **Retail** oder eine private Sandbox-ID handeln. |
| storeClient | Zeichenfolge | Eine der folgenden Zeichen folgen, die die Version des Speicher Orts angibt, in dem der Erwerb erfolgt ist: <ul><li>**Windows Phone Store (client)**</li><li>**Microsoft Store (Client)** (oder **Windows Store (Client)** bei der Abfrage von Daten vor dem 23. März 2018) </li><li>**Microsoft Store (Web)** (oder **Windows Store (Web)** , wenn vor dem 23. März 2018 Daten abgefragt werden sollen) </li><li>**Volume purchase by organizations**</li><li>**Andere**</li></ul> |
| xboxtitleid | Zeichenfolge | Die Xbox Live-Titel-ID (dargestellt als Hexadezimalwert), die vom Xbox Developer Portal (XDP) für Xbox Live-aktivierte Spiele zugewiesen wird. |
| acquisitionQuantity | number | Die Anzahl der Käufe, die während der angegebenen Aggregationsebene erfolgten. |
| purchasepriceusdamount | number | Der Betrag, der vom Kunden für den Erwerb in USD mit dem monatlichen Wechselkurs gezahlt wird. |
| purchasetax"damount | number | Der auf den Erwerb angewendete Steuerbetrag, der in USD konvertiert wurde. |
| localcurrency-Code | Zeichenfolge | Lokaler Währungscode basierend auf dem Land des Partner Center-Kontos.  |
| xboxproductid | Zeichenfolge | Die Xbox-Produkt-ID des Produkts aus XDP, falls zutreffend.  |
| availabilityId | Zeichenfolge | Verfügbarkeits-ID des Produkts aus XDP, falls zutreffend.  |
| skuId | Zeichenfolge | SKU-ID des Produkts aus XDP, falls zutreffend.  |
| skudisplayname  | Zeichenfolge | SKU-Anzeige Name des Produkts aus XDP, falls zutreffend.  |
| xboxparameentproductid | Zeichenfolge | Die übergeordnete Xbox-Produkt-ID des Produkts aus XDP, falls zutreffend.  |
| parentProductName | Zeichenfolge | Der übergeordnete Produkt Name des Produkts aus XDP, falls zutreffend.  |
| producttyname | Zeichenfolge | Der Produkttyp Name des Produkts aus XDP, falls zutreffend.  |
| purchasetaxtype | Zeichenfolge | Kauf Steuerungstyp des Produkts aus XDP, falls zutreffend.  |
| purchasepricelocalamount | number | Der lokale Betrag des Produkts aus XDP, falls zutreffend.  |
| purchasetaxlocalamount | number | Kaufen Sie ggf. den lokalen Betrag des Produkts aus XDP.  |

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

 
