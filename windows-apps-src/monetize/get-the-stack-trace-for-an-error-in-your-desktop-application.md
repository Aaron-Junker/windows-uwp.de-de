---
description: Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um die Stapelüberwachung für einen Fehler in Ihrer Desktopanwendung abzurufen.
title: Abrufen der Stapelüberwachung für einen Fehler in Ihrer Desktopanwendung
ms.date: 06/05/2018
ms.topic: article
keywords: Windows10, UWP, Store-Dienste, Microsoft Store-Analyse-API, Stapelüberwachung, Fehler, Desktopanwendung
ms.localizationpriority: medium
ms.openlocfilehash: 8cc8aaef2b26af88234efe62bf7cf1cb998e19bc
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "8331845"
---
# <a name="get-the-stack-trace-for-an-error-in-your-desktop-application"></a>Abrufen der Stapelüberwachung für einen Fehler in Ihrer Desktopanwendung

Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um die Stapelüberwachung für einen Fehler in einer Desktopanwendung abzurufen, die Sie zum [Windows-Desktopanwendungsprogramm](https://msdn.microsoft.com/library/windows/desktop/mt826504) hinzugefügt haben. Diese Methode kann nur die Stapelüberwachung für einen Fehler herunterladen, die in den letzten 30Tagen aufgetreten ist. Stapelüberwachungen sind auch im [Bericht "Integrität"](https://msdn.microsoft.com/library/windows/desktop/mt826504) für desktopanwendungen im Partner Center verfügbar.

Bevor Sie diese Methode verwenden können, müssen Sie zuerst die Methode für das [Abrufen von Details zu einem Fehler in Ihrer Desktopanwendung](get-details-for-an-error-in-your-desktop-application.md) aufrufen, um die ID-Hash der CAB-Datei abzurufen, die mit dem Fehler verknüpft ist, für den Sie die Stapelüberwachung abrufen möchten.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Rufen Sie die ID-Hash der CAB-Datei ab, die mit dem Fehler verknüpft ist, für den Sie die Stapelüberwachung abrufen möchten. Verwenden Sie zum Abrufen dieses Wertes die Methode zum [Abrufen von Details zu einem Fehler in Ihrer Desktopanwendung](get-details-for-an-error-in-your-desktop-application.md), um Details zu einem bestimmten Fehler in Ihrer App abzurufen, und verwenden Sie den **cabIdHash**-Wert im Antworttext dieser Methode.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |
 

### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  |
|---------------|--------|---------------|------|
| applicationId | string | Die Produkt-ID der Desktopanwendung, für die Sie eine Stapelüberwachung abrufen möchten. Um die Produkt-ID einer desktop-Anwendung zu erhalten, öffnen Sie alle [-Analysebericht für Ihre desktop-Anwendung im Partner Center](https://msdn.microsoft.com/library/windows/desktop/mt826504) (z. B. den **Bericht "Integrität"**), und rufen Sie die Produkt-ID aus der URL. |  Ja  |
| cabIdHash | String | Die eindeutige ID-Hash der CAB-Datei, die mit dem Fehler verknüpft ist, für den Sie die Stapelüberwachung abrufen möchten. Verwenden Sie zum Abrufen dieses Wertes die Methode zum [Abrufen von Details zu einem Fehler in Ihrer Desktopanwendung](get-details-for-an-error-in-your-desktop-application.md), um Details zu einem bestimmten Fehler in Ihrer Anwendung abzurufen, und verwenden Sie den **cabIdHash**-Wert im Antworttext dieser Methode. |  Ja  |

 
### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird gezeigt, wie Sie mit dieser Methode eine Stapelüberwachung abrufen. Ersetzen Sie die Parameter *applicationId* und *cabIdHash* durch die entsprechende Werte für Ihre Desktopanwendung.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
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
* [Abrufen von Fehlerberichtsdaten für Ihre Desktopanwendung](get-desktop-application-error-reporting-data.md)
* [Abrufen von Details zu einem Fehler in Ihrer Desktopanwendung](get-details-for-an-error-in-your-desktop-application.md)
* [Herunterladen der CAB-Datei bei einem Fehler in Ihrer Desktopanwendung](download-the-cab-file-for-an-error-in-your-desktop-application.md)
