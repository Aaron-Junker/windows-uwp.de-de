---
ms.assetid: b556a245-6359-4ddc-a4bd-76f9873ab694
description: Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um die Stapelüberwachung für einen Fehler in Ihrer App abzurufen.
title: Abrufen der Stapelüberwachung für einen Fehler in Ihrer App
ms.date: 06/05/2018
ms.topic: article
keywords: Windows10, UWP, Store-Dienste, Microsoft Store-Analyse-API, Stapelüberwachung, Fehler
ms.localizationpriority: medium
ms.openlocfilehash: ceffc7622f756eb17c8475208852e013df814554
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "8213325"
---
# <a name="get-the-stack-trace-for-an-error-in-your-app"></a>Abrufen der Stapelüberwachung für einen Fehler in Ihrer App

Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um die Stapelüberwachung für einen Fehler in Ihrer App abzurufen. Diese Methode kann nur die Stapelüberwachung für einen App-Fehler herunterladen, die in den letzten 30Tagen aufgetreten ist. Stapelüberwachungen sind auch im Abschnitt **Fehler** im [Bericht "Integrität"](../publish/health-report.md) im Partner Center verfügbar.

Bevor Sie diese Methode verwenden können, müssen Sie zuerst die Methode für das [Abrufen von Details zu einem Fehler in Ihrer App](get-details-for-an-error-in-your-app.md) aufrufen, um die ID der CAB-Datei abzurufen, die mit dem Fehler verknüpft ist, für den Sie die Stapelüberwachung abrufen möchten.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Rufen Sie die ID der CAB-Datei an, die mit dem Fehler verknüpft ist, für den Sie die Stapelüberwachung abrufen möchten. Verwenden Sie zum Abrufen dieser ID die Methode zum [Abrufen von Details zu einem Fehler in Ihrer App](get-details-for-an-error-in-your-app.md), um Details zu einem bestimmten Fehler in Ihrer App abzurufen, und verwenden Sie den **cabId**-Wert im Antworttext dieser Methode.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  |
|---------------|--------|---------------|------|
| applicationId | string | Die Store-ID der App, für die Sie die Stapelüberwachung abrufen möchten. Die Store-ID ist auf der [Seite App-Identität](../publish/view-app-identity-details.md) im Partner Center verfügbar. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8. |  Ja  |
| cabId | string | Die eindeutige ID der CAB-Datei, die mit dem Fehler verknüpft ist, für den Sie die Stapelüberwachung abrufen möchten. Verwenden Sie zum Abrufen dieser ID die Methode zum [Abrufen von Details zu einem Fehler in Ihrer App](get-details-for-an-error-in-your-app.md), um Details zu einem bestimmten Fehler in Ihrer App abzurufen, und verwenden Sie den **cabId**-Wert im Antworttext dieser Methode. |  Ja  |

 
### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird gezeigt, wie Sie mit dieser Methode eine Stapelüberwachung abrufen. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihrer App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ    | Beschreibung                  |
|------------|---------|--------------------------------|
| Wert      | array   | Ein Array von Objekten, die jeweils einen einzelnen Frame der Stapelüberwachungsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Stapelüberwachungswerte](#stack-trace-values). |
| @nextLink  | String  | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10 festgelegt ist, es jedoch mehr als 10 Zeilen mit Fehlern für die Abfrage gibt. |
| TotalCount | Ganzzahl | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.          |


### <a name="stack-trace-values"></a>Stapelüberwachungswerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert           | Typ    | Beschreibung      |
|-----------------|---------|----------------|
| level            | string  |  Die Framenummer, die dieses Element im Aufrufstapel darstellt.  |
| image   | string  |   Den Namen der ausführbaren Datei oder des Bibliothekbilds, die/das die Funktion enthält, die in diesem Stapelframe aufgerufen wird.           |
| function | string  |  Der Name der Funktion, die in diesem Stapelframe aufgerufen wird. Dies ist nur verfügbar, wenn Ihre App Symbole für die ausführbare Datei oder die Bibliothek enthält.              |
| offset     | string  |  Der Byte-Offset der aktuellen Anweisung relativ zum Start der Funktion.      |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "level": "0",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.DoWork",
      "offset": "0x25C"
    }
    {
      "level": "1",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.Initialize",
      "offset": "0x26"
    }
    {
      "level": "2",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.Start",
      "offset": "0x66"
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}

```

## <a name="related-topics"></a>Verwandte Themen

* [Integritätsbericht](../publish/health-report.md)
* [Zugreifen auf Analysedaten mit MicrosoftStore-Diensten](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md)
* [Abrufen von Details zu einem Fehler in Ihrer App](get-details-for-an-error-in-your-app.md)
* [Herunterladen der CAB-Datei bei einem Fehler in Ihrer App](download-the-cab-file-for-an-error-in-your-app.md)
