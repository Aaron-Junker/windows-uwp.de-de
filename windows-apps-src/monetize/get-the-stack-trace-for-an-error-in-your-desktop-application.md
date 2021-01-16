---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um die Stapel Überwachung für einen Fehler in der Desktop Anwendung zu erhalten.
title: Abrufen der Stapelüberwachung für einen Fehler in Ihrer Desktopanwendung
ms.date: 06/05/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Stapel Überwachung, Fehler, Desktop Anwendung
ms.localizationpriority: medium
ms.openlocfilehash: 319323ffbd8ed9aecda29cb012a47476f74c7811
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254175"
---
# <a name="get-the-stack-trace-for-an-error-in-your-desktop-application"></a>Abrufen der Stapelüberwachung für einen Fehler in Ihrer Desktopanwendung

Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um die Stapel Überwachung für einen Fehler in einer Desktop Anwendung zu erhalten, die Sie dem [Windows-Desktop Anwendungsprogramm](/windows/desktop/appxpkg/windows-desktop-application-program)hinzugefügt haben. Diese Methode kann nur die Stapel Überwachung für einen Fehler, der in den letzten 30 Tagen aufgetreten ist, herunterladen. Stapel Überwachungen sind auch im Integritäts [Bericht](/windows/desktop/appxpkg/windows-desktop-application-program) für Desktop Anwendungen im Partner Center verfügbar.

Bevor Sie diese Methode verwenden können, müssen Sie zunächst die [Get Details für einen Fehler in der Desktop Anwendungs](get-details-for-an-error-in-your-desktop-application.md) Methode verwenden, um den ID-Hash der CAB-Datei abzurufen, die dem Fehler zugeordnet ist, für den Sie die Stapel Überwachung abrufen möchten.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Rufen Sie den ID-Hash der CAB-Datei ab, die dem Fehler zugeordnet ist, für den Sie die Stapel Überwachung abrufen möchten. Um diesen Wert abzurufen, verwenden Sie die [Get Details für einen Fehler in der Desktop Anwendungs](get-details-for-an-error-in-your-desktop-application.md) Methode, um Details für einen bestimmten Fehler in Ihrer APP abzurufen, und verwenden Sie den **cabidhash** -Wert im Antworttext dieser Methode.

## <a name="request"></a>Anforderung

### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                                   |
|--------|-------------------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace` |

### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |

### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | type   |  BESCHREIBUNG      |  Erforderlich  |
|---------------|--------|---------------|------|
| applicationId | string | Die Produkt-ID der Desktop Anwendung, für die Sie eine Stapel Überwachung erhalten möchten. Wenn Sie die Produkt-ID einer Desktop Anwendung abrufen möchten, öffnen Sie einen beliebigen [Analysebericht für Ihre Desktop Anwendung im Partner Center](/windows/desktop/appxpkg/windows-desktop-application-program) (z. b. den Integritäts **Bericht**), und rufen Sie die Produkt-ID aus der URL ab. |  Ja  |
| cabidhash | string | Der eindeutige ID-Hash der CAB-Datei, die dem Fehler zugeordnet ist, für den Sie die Stapel Überwachung abrufen möchten. Um diesen Wert abzurufen, verwenden Sie die [Get Details für einen Fehler in der Desktop Anwendungs](get-details-for-an-error-in-your-desktop-application.md) Methode, um Details für einen bestimmten Fehler in Ihrer Anwendung abzurufen, und verwenden Sie den **cabidhash** -Wert im Antworttext dieser Methode. |  Ja  |

### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird gezeigt, wie Sie mit dieser Methode eine Stapelüberwachung abrufen. Ersetzen Sie die Parameter *ApplicationId* und *cabidhash* durch die entsprechenden Werte für Ihre Desktop Anwendung.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

### <a name="response-body"></a>Antworttext

| Wert      | Type    | BESCHREIBUNG                  |
|------------|---------|--------------------------------|
| Wert      | array   | Ein Array von Objekten, die jeweils einen einzelnen Frame der Stapelüberwachungsdaten enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie unten im Abschnitt [Stapelüberwachungswerte](#stack-trace-values). |
| @nextLink  | string  | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 10 festgelegt ist, es jedoch mehr als 10 Zeilen mit Fehlern für die Abfrage gibt. |
| TotalCount | integer | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.          |

### <a name="stack-trace-values"></a>Stapelüberwachungswerte

Elemente im Array *Value* enthalten die folgenden Werte.

| Wert           | Type    | BESCHREIBUNG      |
|-----------------|---------|----------------|
| Level            | string  |  Die Framenummer, die dieses Element im Aufrufstapel darstellt.  |
| image   | Zeichenfolge  |   Den Namen der ausführbaren Datei oder des Bibliothekbilds, die/das die Funktion enthält, die in diesem Stapelframe aufgerufen wird.           |
| Funktion | string  |  Der Name der Funktion, die in diesem Stapelframe aufgerufen wird. Dies ist nur verfügbar, wenn Ihre App Symbole für die ausführbare Datei oder die Bibliothek enthält.              |
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

## <a name="related-topics"></a>Zugehörige Themen

* [Integritätsbericht](../publish/health-report.md)
* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Fehlerberichtsdaten für Ihre Desktopanwendung](get-desktop-application-error-reporting-data.md)
* [Abrufen von Details zu einem Fehler in Ihrer Desktopanwendung](get-details-for-an-error-in-your-desktop-application.md)
* [Herunterladen der CAB-Datei bei einem Fehler in der Desktopanwendung](download-the-cab-file-for-an-error-in-your-desktop-application.md)