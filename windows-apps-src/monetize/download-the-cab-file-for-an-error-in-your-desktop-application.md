---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um die CAB-Datei für einen Fehler in der Desktop Anwendung herunterzuladen.
title: Herunterladen der CAB-Datei bei einem Fehler in der Desktopanwendung
ms.date: 03/06/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Analytics-API, CAB herunterladen, Desktop Anwendung
ms.localizationpriority: medium
ms.openlocfilehash: 98a0d6931a59e037bb15679a20fae11eb82dae32
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162494"
---
# <a name="download-the-cab-file-for-an-error-in-your-desktop-application"></a>Herunterladen der CAB-Datei bei einem Fehler in der Desktopanwendung

Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um die CAB-Datei herunterzuladen, die einem bestimmten Fehler für eine Desktop Anwendung zugeordnet ist, die Sie dem [Windows-Desktop Anwendungsprogramm](/windows/desktop/appxpkg/windows-desktop-application-program)hinzugefügt haben. Diese Methode kann nur die CAB-Datei für einen app-Fehler, der in den letzten 30 Tagen aufgetreten ist, herunterladen. Downloads von CAB-Dateien sind auch im Integritäts [Bericht](/windows/desktop/appxpkg/windows-desktop-application-program) für Desktop Anwendungen in Partner Center verfügbar.

Bevor Sie diese Methode verwenden können, müssen Sie zunächst die [Get Details für einen Fehler in der Desktop Anwendungs](get-details-for-an-error-in-your-desktop-application.md) Methode verwenden, um den ID-Hash der CAB-Datei abzurufen, die Sie herunterladen möchten.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Sie erhalten den ID-Hash der CAB-Datei, die Sie herunterladen möchten. Um diesen Wert abzurufen, verwenden Sie die [Get Details für einen Fehler in der Desktop Anwendungs](get-details-for-an-error-in-your-desktop-application.md) Methode, um Details für einen bestimmten Fehler in Ihrer APP abzurufen, und verwenden Sie den **cabidhash** -Wert im Antworttext dieser Methode.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  BESCHREIBUNG      |  Erforderlich  |
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die Produkt-ID der Desktop Anwendung, für die Sie eine CAB-Datei herunterladen möchten. Um die Produkt-ID einer Desktop Anwendung abzurufen, öffnen Sie einen beliebigen [Partner Center Analytics-Bericht für Ihre Desktop Anwendung](/windows/desktop/appxpkg/windows-desktop-application-program) (z. b. den Integritäts **Bericht**), und rufen Sie die Produkt-ID aus der URL ab. |  Ja  |
| cabidhash | Zeichenfolge | Der eindeutige ID-Hash der CAB-Datei, die Sie herunterladen möchten. Um diesen Wert abzurufen, verwenden Sie die [Get Details für einen Fehler in der Desktop Anwendungs](get-details-for-an-error-in-your-desktop-application.md) Methode, um Details für einen bestimmten Fehler in Ihrer Anwendung abzurufen, und verwenden Sie den **cabidhash** -Wert im Antworttext dieser Methode. |  Ja  |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird veranschaulicht, wie Sie mit dieser Methode eine CAB-Datei herunterladen. Ersetzen Sie die Parameter *ApplicationId* und *cabidhash* durch die entsprechenden Werte für Ihre Desktop Anwendung.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Diese Methode gibt einen 302 (Redirect)-Antwort Code zurück, und der **Location** -Header in der Antwort wird dem SAS-URI (Shared Access Signature) der CAB-Datei zugewiesen. Der Aufrufer wird zu diesem URI umgeleitet, um die CAB-Datei automatisch herunterzuladen.

## <a name="related-topics"></a>Zugehörige Themen

* [Integritätsbericht](../publish/health-report.md)
* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Fehlerberichtsdaten für Ihre Desktopanwendung](get-desktop-application-error-reporting-data.md)
* [Abrufen von Details zu einem Fehler in Ihrer Desktopanwendung](get-details-for-an-error-in-your-desktop-application.md)
* [Abrufen der Stapelüberwachung für einen Fehler in Ihrer Desktopanwendung](get-the-stack-trace-for-an-error-in-your-desktop-application.md)