---
description: Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API, um die stapelüberwachung für einen Fehler in Ihrer Xbox One Spiel zu erhalten.
title: Abrufen der Stapelüberwachung für einen Fehler in einem Xbox One-Spiel
ms.date: 11/06/2018
ms.topic: article
keywords: Windows 10, UWP, Store-Dienste, Microsoft Store-Analyse-API, Stapelüberwachung, Fehler
ms.localizationpriority: medium
ms.openlocfilehash: fd43305c54245c3281a0e840d3df4c5c87ff7ad8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658685"
---
# <a name="get-the-stack-trace-for-an-error-in-your-xbox-one-game"></a>Abrufen der Stapelüberwachung für einen Fehler in einem Xbox One-Spiel

Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API um die stapelüberwachung für einen Fehler in Ihrer Xbox One Spiel zu erhalten, die über die Xbox-Entwickler-Portal (XDP) erfasste und im XDP Analytics Partner Center-Dashboard verfügbar war. Diese Methode kann nur die Stapelüberwachung für einen Fehler herunterladen, die in den letzten 30 Tagen aufgetreten ist.

Bevor Sie diese Methode verwenden können, müssen Sie zunächst mithilfe der [Abrufen von Details für einen Fehler in Ihrer Xbox One-Spiel](get-details-for-an-error-in-your-xbox-one-game.md) Methode, um die ID der CAB-Datei abzurufen, die mit dem Fehler verknüpft ist, für die Sie die stapelüberwachung abrufen möchten.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Rufen Sie die ID der CAB-Datei an, die mit dem Fehler verknüpft ist, für den Sie die Stapelüberwachung abrufen möchten. Rufen Sie diese ID mit der [Abrufen von Details für einen Fehler in Ihrer Xbox One-Spiel](get-details-for-an-error-in-your-xbox-one-game.md) Methode zum Abrufen von Details zu einem bestimmten Fehler in Ihrer app, und Verwenden der **CabId** Wert im Antworttext zurückgeben dieser Methode.

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  |
|---------------|--------|---------------|------|
| applicationId | string | Die Produkt-ID des Xbox One-Spiels für die Sie eine stapelüberwachung abrufen. Um die Produkt-ID Ihres Spiels zu erhalten, wechseln Sie zu Ihrem Spiel in der Xbox-Entwickler-Portal (XDP) und rufen Sie die Produkt-ID von der URL ab. Wenn Sie Ihre Gesundheitsdaten aus den Windows Partner Center-Analysebericht herunterladen, ist die Produkt-ID auch in das TSV-Datei enthalten. |  Ja  |
| cabId | string | Die eindeutige ID der CAB-Datei, die mit dem Fehler verknüpft ist, für den Sie die Stapelüberwachung abrufen möchten. Rufen Sie diese ID mit der [Abrufen von Details für einen Fehler in Ihrer Xbox One-Spiel](get-details-for-an-error-in-your-xbox-one-game.md) Methode zum Abrufen von Details zu einem bestimmten Fehler in Ihrer app, und Verwenden der **CabId** Wert im Antworttext zurückgeben dieser Methode. |  Ja  |

 
### <a name="request-example"></a>Anforderungsbeispiel

Im folgende Beispiel wird veranschaulicht, wie Sie eine stapelüberwachung für eine Xbox One Spiele mit dieser Methode abrufen. Ersetzen Sie die *ApplicationId* Wert mit dem Productid für Ihr Spiel.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort


### <a name="response-body"></a>Antworttext

| Wert      | Typ    | Beschreibung                  |
|------------|---------|--------------------------------|
| Wert      | array   | Ein Array von Objekten, die jeweils einen einzelnen Frame der Stapelüberwachungsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Stapelüberwachungswerte](#stack-trace-values). |
| @nextLink  | string  | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10 festgelegt ist, es jedoch mehr als 10 Zeilen mit Fehlern für die Abfrage gibt. |
| TotalCount | Ganzzahl | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.          |


### <a name="stack-trace-values"></a>Stapelüberwachungswerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert           | Typ    | Beschreibung      |
|-----------------|---------|----------------|
| level            | string  |  Die Framenummer, die dieses Element im Aufrufstapel darstellt.  |
| image   | string  |   Den Namen der ausführbaren Datei oder des Bibliothekbilds, die/das die Funktion enthält, die in diesem Stapelframe aufgerufen wird.           |
| function | string  |  Der Name der Funktion, die in diesem Stapelframe aufgerufen wird. Dies ist nur verfügbar, wenn Ihr Spiel Symbole für die ausführbare Datei oder Bibliothek enthält.              |
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

* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
* [Erhalten Sie die Daten für Ihre Xbox One Fehlerberichterstattung Spiele](get-error-reporting-data-for-your-xbox-one-game.md)
* [Abrufen von Details für einen Fehler in Ihrer Xbox One-Spiel](get-details-for-an-error-in-your-xbox-one-game.md)
* [Laden Sie die CAB-Datei für einen Fehler in Ihrem Spiel Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
