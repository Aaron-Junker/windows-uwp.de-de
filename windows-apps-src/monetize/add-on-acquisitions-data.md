---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um aggregierte Add-on-Erfassungsdaten im JSON-Format für UWP-apps und Xbox One-Spiele zu erhalten, die über das Xbox Developer Portal (XDP) erfasst wurden und auf dem XDP Analytics Partner Center-Dashboard verfügbar sind.
title: Abrufen von Add-On-Kaufdaten für Ihre Spiele und Apps
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, Ankündigungs Netzwerk, App-Metadaten
ms.localizationpriority: medium
ms.openlocfilehash: 3e1349582515ce66232ea8266efc588610faae98
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493565"
---
# <a name="get-add-on-acquisitions-data-for-your-games-and-apps"></a>Abrufen von Add-On-Kaufdaten für Ihre Spiele und Apps 
Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um aggregierte Add-on-Erfassungsdaten im JSON-Format für UWP-apps und Xbox One-Spiele zu erhalten, die über das Xbox Developer Portal (XDP) erfasst wurden und auf dem XDP Analytics Partner Center-Dashboard verfügbar sind. 

## <a name="prerequisites"></a>Voraussetzungen
Zur Verwendung dieser Methode sind folgende Schritte erforderlich: 

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md) für die Microsoft Store Analytics-API erfüllen. 
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen. 

> [!NOTE]
> Diese API stellt vor dem 1. Oktober 2016 keine täglichen aggregierten Daten bereit. 

## <a name="request"></a>Anforderung

### <a name="request-syntax"></a>Anforderungssyntax
| Methode | Anforderungs-URI |
| --- | --- | 
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions` |

### <a name="request-header"></a>Anforderungsheader 
| Header | type | BESCHREIBUNG | 
| --- | --- | --- |
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD Zugriffs Token im Formular **Träger** `<token>` . |

### <a name="request-parameters"></a>Anforderungsparameter
Der *ApplicationId* -Parameter oder der *addonproductid-* Parameter ist erforderlich. Um Kaufdaten für alle für die App registrierten Add-Ons abzurufen, geben Sie den Parameter *applicationId* an. Um Erwerbs Daten für ein einzelnes Add-on abzurufen, geben Sie den *addonproductid-* Parameter an. Wenn Sie beide Parameter angeben, wird der Parameter *applicationId* ignoriert. 

| Parameter | type | BESCHREIBUNG | Erforderlich | 
| --- | --- | --- | --- |
| applicationId | Zeichenfolge | Die *ProductID* des Xbox One-Spiels, für das Sie die Erfassungsdaten abrufen. Um die *ProductID* Ihres Spiels zu erhalten, navigieren Sie zu Ihrem Spiel im XDP-Analyseprogramm, und rufen Sie die *ProductID* aus der URL ab. Wenn Sie Ihre Erstellungs Daten aus dem Partner Center Analytics-Bericht herunterladen, ist die *ProductID* alternativ in der TSV-Datei enthalten. | Ja |
| addonproductid | Zeichenfolge | Die *ProductID* des Add-on, für das Sie Erwerbs Daten abrufen möchten. | Ja |
| startDate | date | Das Startdatum im Datumsbereich der Add-On-Kaufdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. | Nein |
| endDate | date | Das Enddatum im Datumsbereich der Add-On-Kaufdaten, die abgerufen werden sollen. Als Standardeinstellung wird das aktuelle Datum festgelegt. | Nein |
| top | INT | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. | Nein |
| skip | INT | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. | Nein |
| filter | Zeichenfolge | Mindestens eine Anweisung, die die Zeilen in der Antwort filtert. Jede-Anweisung enthält einen Feldnamen aus dem Antworttext und den Wert, die den EQ-oder ne-Operatoren zugeordnet sind, und-Anweisungen können mithilfe von and oder or kombiniert werden. Zeichenfolgenwerte im Parameter filter müssen von einfachen Anführungszeichen eingeschlossen werden. Beispiel: Filter = Market EQ ' US ' und Geschlecht EQ 'm '. <br/> Sie können die folgenden Felder aus dem Antworttext angeben: <ul><li>**acquisitionType**</li><li>**Eder**</li><li>**storeClient**</li><li>**gender**</li><li>**Marktforschungs**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | Nein |
| aggregationLevel | Zeichenfolge | Gibt den Zeitraum an, für den aggregierte Daten abgerufen werden sollen. Dies kann eine der folgenden Zeichenfolgen sein: **day**, **week** oder **month**. Wenn keine Angabe erfolgt, lautet der Standardwert **day**. | Nein |
| orderby | Zeichenfolge | Eine Anweisung, die die Ergebnisdatenwerte für die einzelnen Add-On-Käufe anfordert. Syntax *: OrderBy = Field [Order], Field [Order],...* Der *Feld* Parameter kann eine der folgenden Zeichen folgen sein: <ul><li>**date**</li><li>**acquisitionType**</li><li>**Eder**</li><li>**storeClient**</li><li>**gender**</li><li>**Marktforschungs**</li><li>**osVersion**</li><li>**deviceType**</li><li>**orderName**</li></ul> Der Parameter order ist optional und kann **asc** oder **desc** sein, um die auf- oder absteigende Anordnung der einzelnen Felder anzugeben. Der Standardwert ist **ASC**. <br/> Hier ist ein Beispiel für eine *OrderBy* -Zeichenfolge: *OrderBy = Date, Market* | Nein |
| groupby | Zeichenfolge | Eine Anweisung, die nur auf die angegebenen Felder Datenaggregationen anwendet. Sie können die folgenden Felder angeben: <ul><li>**date**</li><li>**applicationName**</li><li>**addonproductname**</li> <li>**acquisitionType**</li><li>**Eder**</li> <li>**storeClient**</li><li>**gender**</li> <li>**Marktforschungs**</li> <li>**osVersion**</li><li>**deviceType**</li><li>**paymentinstrumenttype**</li><li>**sandboxId**</li><li>**xboxtitleidhex**</li></ul> Die zurückgegebenen Datenzeilen enthalten die Felder, die im Parameter *groupby* angegeben sind, sowie die folgenden: <ul><li>**date**</li><li>**applicationId**</li><li>**addonproductid**</li><li>**acquisitionQuantity**</li></ul> Der Parameter groupby kann mit dem Parameter *aggregationLevel* verwendet werden. Beispiel: *&GroupBy = Age, Market&aggregationlevel = Week* | Nein |

### <a name="request-example"></a>Anforderungsbeispiel
Die folgenden Beispiele zeigen verschiedene Anforderungen für das Abrufen von Add-On-Kaufdaten. Ersetzen Sie die Werte " *addonproductid* " und " *ApplicationId* " durch die entsprechende Speicher-ID für das Add-on oder die app. 

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0 HTTP/1.1 

Authorization: Bearer <your access token> 

 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0&filter=market eq 'GB' and gender eq 'm' HTTP/1.1 

Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

### <a name="response-body"></a>Antworttext

| Wert | type | BESCHREIBUNG |
| --- | --- | --- |
| Wert | array | Ein Array von Objekten, die aggregierte Add-On-Kaufdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Add-On-Kaufwerte](#add-on-acquisition-values). |
| @nextLink | Zeichenfolge | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10000 festgelegt ist, es jedoch mehr als 10.000 Zeilen mit Add-On-Kaufdaten für die Abfrage gibt. |
| TotalCount | INT | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage. |

### <a name="add-on-acquisition-values"></a>Add-On-Kaufwerte
Elemente im Array Value enthalten die folgenden Werte.

| Wert | type | BESCHREIBUNG | 
| --- | --- | --- |
| date | Zeichenfolge | Das erste Datum im Datumsbereich für die Kaufdaten. Wenn die Anforderung einen einzelnen Tag angibt, ist dieses Datum dieser Wert. Wenn die Anforderung eine Woche, einen Monat oder einen anderen Datumsbereich angibt, ist dieser Wert das erste Datum in diesem Datumsbereich. |
| addonproductid | Zeichenfolge | Die *ProductID* des Add-Ins, für das Sie Erfassungsdaten abrufen. |
| addonproductname | Zeichenfolge | Der Anzeigename des Add-Ons. Dieser Wert wird nur in den Antwortdaten angezeigt, wenn der *aggregationlevel* -Parameter auf **Day**festgelegt ist, es sei denn, Sie geben das **addonproductname** -Feld im *GroupBy* -Parameter an. |
| applicationId | Zeichenfolge | Die *ProductID* der APP, für die Sie Add-on-Erwerbs Daten abrufen möchten. |
| applicationName | Zeichenfolge | Der Anzeige Name des Spiels. |
| deviceType | Zeichenfolge | Eine der folgenden Zeichen folgen, die den Typ des Geräts angibt, das den Erwerb abgeschlossen hat: <ul><li>PCs</li><li>Smartphone</li><li>"Console-Xbox One"</li><li>"Console-Xbox Series X"</li><li>IOT</li><li>Servers</li><li>Tablet</li><li>Holographic</li><li>Unbekannter</li></ul> |
| storeClient | Zeichenfolge | Eine der folgenden Zeichen folgen, die die Version des Speicher Orts angibt, in dem der Erwerb erfolgt ist: <ul><li>"Windows Phone Store (Client)"</li><li>"Microsoft Store (Client)" (oder "Windows Store (Client)" bei der Abfrage von Daten vor dem 23. März 2018)</li><li>"Microsoft Store (Web)" (oder "Windows Store (Web)" beim Abfragen von Daten vor dem 23. März 2018)</li><li>"Volume Purchase by Unternehmen"</li><li>Außer</li></ul> |
| osVersion | Zeichenfolge | Die Version des Betriebssystems, auf dem der Kauf ausgeführt wurde. Bei dieser Methode ist dieser Wert immer "Windows 10". |
| market | Zeichenfolge | Die ISO 3166-Ländercode des Markts, in dem der Kauf erfolgte. |
| gender | Zeichenfolge | Eine der folgenden Zeichen folgen, die das Geschlecht des Benutzers angibt, der die Übernahme durchgeführt hat: <ul><li>"m"</li><li>"f"</li><li>Unbekannter</li></ul> |
| age | Zeichenfolge | Eine der folgenden Zeichen folgen, die die Altersgruppe des Benutzers angibt, der die Übernahme durchgeführt hat: <ul><li>"weniger als 13"</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>"größer als 55"</li><li>Unbekannter</li></ul> |
| acquisitionType | Zeichenfolge | Eine der folgenden Zeichen folgen, die den Typ des Erwerbs angibt: <ul><li>Kosten </li><li>Versuchten </li><li>Teten</li><li>"Werbe Code" </li><li>IAP</li><li>"Abonnement-IAP"</li><li>"Private Zielgruppe"</li><li>"Vorab anordnen"</li><li>"Xbox Game Pass" (oder "Game Pass" bei der Abfrage von Daten vor dem 23. März 2018)</li><li>Diskette</li><li>"Voraus bezahlter Code"</li><li>"Berechnete Vorabbestellung"</li><li>"Abbruch vor der Bestellung"</li><li>"Fehler bei Vorbestellung"</li></ul> |
| acquisitionQuantity | integer | Die Anzahl der Käufe, die ausgeführt wurden. |
| inAppProductId | Zeichenfolge | Produkt-ID des Produkts, in dem dieses Add-on verwendet wird.  |
| inAppProductName | Zeichenfolge | Produkt Name des Produkts, in dem dieses Add-on verwendet wird.  |
| paymentinstrumenttype | Zeichenfolge | Der für den Erwerb verwendete Zahlungs Instrumentationstyp.  |
| sandboxId | Zeichenfolge | Die Sandbox-ID, die für das Spiel erstellt wurde. Dabei kann es sich um den Wert **Retail** oder eine private Sandbox-ID handeln.  |
| xboxtitleid | Zeichenfolge | Die Xbox-Titel-ID des Produkts aus XDP, falls zutreffend.  |
| localcurrency-Code | Zeichenfolge | Lokaler Währungscode basierend auf dem Land des Partner Center-Kontos.  |
| xboxproductid | Zeichenfolge | Die Xbox-Produkt-ID des Produkts aus XDP, falls zutreffend.  |
| availabilityId | Zeichenfolge | Verfügbarkeits-ID des Produkts aus XDP, falls zutreffend.  |
| skuId | Zeichenfolge | SKU-ID des Produkts aus XDP, falls zutreffend.  |
| skudisplayname | Zeichenfolge | SKU-Anzeige Name des Produkts aus XDP, falls zutreffend.  |
| xboxparameentproductid | Zeichenfolge | Die übergeordnete Xbox-Produkt-ID des Produkts aus XDP, falls zutreffend.  |
| parentProductName | Zeichenfolge | Der übergeordnete Produkt Name des Produkts aus XDP, falls zutreffend.  |
| producttyname | Zeichenfolge | Der Produkttyp Name des Produkts aus XDP, falls zutreffend.  |
| purchasetaxtype | Zeichenfolge | Kauf Steuerungstyp des Produkts aus XDP, falls zutreffend.  |
| purchasepriceusdamount | number | Der Betrag, der vom Kunden für das Add-on gezahlt und in USD konvertiert wurde.  |
| purchasepricelocalamount | number | Der auf das Add-on angewendete Steuerbetrag.  |
| purchasetax"damount | number | Der auf das Add-on angewendete Steuerbetrag, der in USD konvertiert wurde.  |
| purchasetaxlocalamount | number | Kaufen Sie ggf. den lokalen Betrag des Produkts aus XDP.  |

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