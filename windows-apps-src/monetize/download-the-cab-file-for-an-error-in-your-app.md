---
author: Xansky
ms.assetid: ''
description: Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um die CAB-Datei für einen Fehler in der App herunterzuladen.
title: Herunterladen der CAB-Datei bei einem Fehler in Ihrer App
ms.author: mhopkins
ms.date: 06/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP, Microsoft Store-Analyse-API, CAB herunterladen
ms.localizationpriority: medium
ms.openlocfilehash: 671c5c1b187ac48c12988a00d66acb366cae72f1
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "5470345"
---
# <a name="download-the-cab-file-for-an-error-in-your-app"></a>Herunterladen der CAB-Datei bei einem Fehler in Ihrer App

Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um die CAB-Datei herunterzuladen, die mit einem bestimmten im Dev Center gemeldeten Fehler in der App verknüpft ist. Diese Methode kann nur die CAB-Datei für einen App-Fehler herunterladen, die in den letzten 30Tagen aufgetreten ist. CAB-Datei-Downloads sind auch im Abschnitt **Fehler** des [Integritätsberichts](../publish/health-report.md) im Windows Dev Center-Dashboard verfügbar.

Bevor Sie diese Methode verwenden können, müssen Sie zuerst anhand der Methode zum [Abrufen von Informationen zu einem Fehler in der App](get-details-for-an-error-in-your-app.md) die ID der CAB-Datei abrufen, die Sie herunterladen möchten.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Rufen Sie die ID der CAB-Datei ab, die Sie herunterladen möchten. Verwenden Sie zum Abrufen dieser ID die Methode zum [Abrufen von Details zu einem Fehler in Ihrer App](get-details-for-an-error-in-your-app.md), um Details zu einem bestimmten Fehler in Ihrer App abzurufen, und verwenden Sie den **cabId**-Wert im Antworttext dieser Methode.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  |
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die Store-ID der App, für die Sie die CAB-Kaufdaten herunterladen möchten. Die Store-ID ist auf der [Seite mit der App-Identität](../publish/view-app-identity-details.md) des DevCenter-Dashboards verfügbar. Beispiel für eine Store-ID: 9WZDNCRFJ3Q8. |  Ja  |
| cabId | Zeichenfolge | Die eindeutige ID der CAB-Datei ab, die Sie herunterladen möchten. Verwenden Sie zum Abrufen dieser ID die Methode zum [Abrufen von Details zu einem Fehler in Ihrer App](get-details-for-an-error-in-your-app.md), um Details zu einem bestimmten Fehler in Ihrer App abzurufen, und verwenden Sie den **cabId**-Wert im Antworttext dieser Methode. |  Ja  |

 
### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird gezeigt, wie Sie mit dieser Methode eine CAB-Datei herunterladen. Ersetzen Sie die Parameter *applicationId* und *cabId* durch die entsprechende Werte für Ihre App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Bei dieser Methode wird ein 302-Antwortcode (Umleitung) zurückgegeben, und der **Speicherort**-Header in der Antwort wird der Shared Access Signature (SAS)-URI der CAB-Datei zugewiesen. Der Aufrufer wird zu dieser URI für den automatischen Download der CAB-Datei weitergeleitet.

## <a name="related-topics"></a>Verwandte Themen

* [Integritätsbericht](../publish/health-report.md)
* [Zugreifen auf Analysedaten mit MicrosoftStore-Diensten](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Fehlerberichtsdaten](get-error-reporting-data.md)
* [Abrufen von Details zu einem Fehler in der App](get-details-for-an-error-in-your-app.md)
* [Abrufen der Stapelüberwachung für einen Fehler in Ihrer App](get-the-stack-trace-for-an-error-in-your-app.md)
