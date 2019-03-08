---
description: Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API zum Herunterladen der CAB-Datei für einen Fehler in Ihrem Spiel Xbox One.
title: Herunterladen der CAB-Datei zu einem Fehler in einem Xbox One-Spiel
ms.date: 11/06/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Analyse-API, CAB herunterladen
ms.localizationpriority: medium
ms.openlocfilehash: 736219533a254e6380c10600e97f707f15e37de6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604325"
---
# <a name="download-the-cab-file-for-an-error-in-your-xbox-one-game"></a>Herunterladen der CAB-Datei zu einem Fehler in einem Xbox One-Spiel

Verwenden Sie diese Methode in der Microsoft Store-Textanalyse-API, die CAB-Datei herunterladen, die einem bestimmten Fehler in einem Xbox One-Spiel, das über die Xbox-Entwickler-Portal (XDP) erfasst wurde zugeordnet und im XDP Analytics Partner Center-Dashboard verfügbar. Diese Methode kann nur für einen Fehler in die CAB-Datei herunterladen, die in den letzten 30 Tagen aufgetreten sind.

Bevor Sie diese Methode verwenden können, müssen Sie zunächst mithilfe der [Abrufen von Details für einen Fehler in Ihrer Xbox One-Spiel](get-details-for-an-error-in-your-xbox-one-game.md) Methode zum Abrufen der ID der CAB-Datei, Sie herunterladen möchten.

## <a name="prerequisites"></a>Voraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Rufen Sie die ID der CAB-Datei ab, die Sie herunterladen möchten. Rufen Sie diese ID mit der [Abrufen von Details für einen Fehler in Ihrer Xbox One-Spiel](get-details-for-an-error-in-your-xbox-one-game.md) Methode zum Abrufen von Details zu einem bestimmten Fehler in Ihrer app, und Verwenden der **CabId** Wert im Antworttext zurückgeben dieser Methode.

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  |
|---------------|--------|---------------|------|
| applicationId | string | Die Produkt-ID des Xbox One-Spiels für die Sie die CAB-Datei heruntergeladen haben. Um die Produkt-ID Ihres Spiels zu erhalten, wechseln Sie zu Ihrem Spiel in der Xbox-Entwickler-Portal (XDP) und rufen Sie die Produkt-ID von der URL ab. Wenn Sie Ihre Gesundheitsdaten aus den Windows Partner Center-Analysebericht herunterladen, ist die Produkt-ID auch in das TSV-Datei enthalten. |  Ja  |
| cabId | string | Die eindeutige ID der CAB-Datei ab, die Sie herunterladen möchten. Rufen Sie diese ID mit der [Abrufen von Details für einen Fehler in Ihrer Xbox One-Spiel](get-details-for-an-error-in-your-xbox-one-game.md) Methode zum Abrufen von Details zu einem bestimmten Fehler in Ihrer app, und Verwenden der **CabId** Wert im Antworttext zurückgeben dieser Methode. |  Ja  |

 
### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird gezeigt, wie Sie mit dieser Methode eine CAB-Datei herunterladen. Ersetzen Sie die Parameter *applicationId* und *cabId* durch die entsprechende Werte für Ihre App.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Bei dieser Methode wird ein 302-Antwortcode (Umleitung) zurückgegeben, und der **Speicherort**-Header in der Antwort wird der Shared Access Signature (SAS)-URI der CAB-Datei zugewiesen. Der Aufrufer wird zu dieser URI für den automatischen Download der CAB-Datei weitergeleitet.

## <a name="related-topics"></a>Verwandte Themen

* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
* [Erhalten Sie die Daten für Ihre Xbox One Fehlerberichterstattung Spiele](get-error-reporting-data-for-your-xbox-one-game.md)
* [Abrufen von Details für einen Fehler in Ihrer Xbox One-Spiel](get-details-for-an-error-in-your-xbox-one-game.md)
* [Die stapelüberwachung für einen Fehler in Ihrer Xbox One Spiel abrufen](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
