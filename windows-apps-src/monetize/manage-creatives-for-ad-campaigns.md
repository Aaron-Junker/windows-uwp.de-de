---
ms.assetid: c5246681-82c7-44df-87e1-a84a926e6496
description: Verwenden Sie diese Methode in der Microsoft Store-Werbungs-API, um Werbemittel für Werbeanzeigenkampagnen zu verwalten.
title: Verwalten von Werbemitteln
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Werbungs-API, Anzeigenkampagnen
ms.localizationpriority: medium
ms.openlocfilehash: 3411ee4c947d809009c2185389f5513a49afce98
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215033"
---
# <a name="manage-creatives"></a>Verwalten von Werbemitteln

Verwenden Sie diese Methoden in der Microsoft Store-Werbungs-API, um Ihre eigenen benutzerdefinierten Werbemittel zu Werbeanzeigenkampagnen hochzuladen oder ein vorhandenes Werbemittel abzurufen. Ein Werbemittel kann einer oder mehreren Lieferpositionen sogar in verschiedenen Anzeigenkampagnen, sofern es sich immer um dieselbe App handelt, zugeordnet werden.

Weitere Informationen zu der Beziehung zwischen Werbemitteln und Anzeigenkampagnen, Lieferpositionen und Zielgruppenprofilen finden Sie unter [Anzeigenkampagnen mit Microsoft Store-Diensten ausführen](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

> [!NOTE]
> Die maximal zulässige Größe für Ihre Werbemittel ist 40 KB, wenn Sie diese API für den Upload Ihrer eigenen Werbemittel verwenden. Wenn Sie eine größere Werbemitteldatei übermitteln, gibt diese API zwar keinen Fehler zurück, die Kampagne wird jedoch nicht erfolgreich erstellt.

## <a name="prerequisites"></a>Vorraussetzungen

Zur Verwendung dieser Methoden sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](run-ad-campaigns-using-windows-store-services.md#prerequisites) für die Microsoft Store-Werbungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token), das in der Anforderungskopfzeile für diese Methoden verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.


## <a name="request"></a>Anforderung

Diese Methoden haben die folgenden URIs.

| Methodentyp | Anforderungs-URI     |  Beschreibung  |
|--------|-----------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative``` |  Erstellt ein neues Werbemittel.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/{creativeId}``` |  Ruft das durch *CreativeId* angegebene Werbemittel ab.  |

> [!NOTE]
> Diese API unterstützt derzeit keine PUT-Methode.


### <a name="header"></a>Header

| Header        | Typ   | Beschreibung         |
|---------------|--------|---------------------|
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |
| Tracking-ID   | GUID   | Optional. Eine ID, die den Abfrageablauf verfolgt.                                  |


### <a name="request-body"></a>Anforderungstext

Die POST-Methode erfordert einen JSON-Anforderungstext mit den erforderlichen Feldern für ein [Werbemittel](#creative)-Objekt.


### <a name="request-examples"></a>Anforderungsbeispiele

Im folgenden Beispiel wird veranschaulicht, wie die POST-Methode zum Erstellen eines Werbemittels aufgerufen wird. In diesem Beispiel wurde der Wert *content* aus Platzgründen verkürzt.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative HTTP/1.1
Authorization: Bearer <your access token>

{
  "name": "Contoso App Campaign - Creative 1",
  "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
  "height": 80,
  "width": 480,
  "imageAttributes":
  {
    "imageExtension": "PNG"
  }
}
```

Im folgenden Beispiel wird veranschaulicht, wie die GET-Methode zum Abrufen eines Werbemittels aufgerufen wird.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/106851  HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Antwort

Diese Methoden geben ein JSON-Antworttext mit einem [Creative](#creative)-Objekt zurück, das Informationen über das Werbemittel enthält, die erstellt oder abgerufen wurden. Das folgende Beispiel zeigt einen Antworttext für diese Methoden. In diesem Beispiel wurde der Wert *content* aus Platzgründen verkürzt.

```json
{
    "Data": {
        "id": 106126,
        "name": "Contoso App Campaign - Creative 2",
        "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
        "height": 50,
        "width": 300,
        "format": "Banner",
        "imageAttributes":
        {
          "imageExtension": "PNG"
        },
        "storeProductId": "9nblggh42cfd"
    }
}
```


<span id="creative"/>

## <a name="creative-object"></a>Kreative-Objekt

Die Anforderungs- und Antworttexte für diese Methoden enthalten die folgenden Felder. Die folgende Tabelle zeigt, welche Felder schreibgeschützt sind (d. h. sie können in der PUT-Methode nicht geändert werden) und welche Felder in dem Anforderungstext für die POST-Methode erforderlich sind.

| Feld        | Typ   |  Beschreibung      |  Schreibgeschützt  | Default  |  Erforderlich für POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  Ganzzahl   |  Die ID des Werbemittels.     |   Ja    |      |    Nein   |       
|  NAME   |  String   |   Name des Werbemittels.    |    Nein   |      |  Ja     |       
|  content   |  String   |  Der Inhalt des Werbemittel-Image im Base64-codierten Format.<br/><br/>**Hinweis:** &nbsp;&nbsp;Die maximal zulässige Größe der Werbemitteldatei beträgt 40 KB. Wenn Sie eine größere Werbemitteldatei übermitteln, gibt diese API zwar keinen Fehler zurück, die Kampagne wird jedoch nicht erfolgreich erstellt.     |  Nein     |      |   Ja    |       
|  height   |  Ganzzahl   |   Die Höhe des Werbemittels.    |    Nein    |      |   Ja    |       
|  width   |  Ganzzahl   |  Die Breite des Werbemittels.     |  Nein    |     |    Ja   |       
|  landingUrl   |  String   |  Wenn Sie eine Kampagne Überwachungsdienst wie z. B. AppsFlyer Kochava, optimieren oder Vungle zum Messen von Install-Analytics für Ihre app verwenden, Ihre nachverfolgungs-URL in dieses Feld zuweisen, wenn Sie die POST-Methode aufrufen (Wenn angegeben, dieser Wert muss ein gültiger URI). Wenn Sie keinen Kampagnennachverfolgungsdienst verwenden, lassen Sie diesen Wert beim Aufruf der POST-Methode aus. (In diesem Fall wird diese URL automatisch erstellt.)   |  Nein    |     |   Ja    |
|  format   |  String   |   Das Anzeigenformat. Zurzeit ist **Banner** der einzige Wert, der unterstützt wird.    |   Nein    |  Banner   |  Nein     |       
|  imageAttributes   | [ImageAttributes](#image-attributes)    |   Stellt Attribute für das Werbemittel bereit.     |   Nein    |      |   Ja    |       
|  storeProductId   |  String   |   Die [Store-ID](in-app-purchases-and-trials.md#store-ids) der App, der diese Anzeigenkampagne zugeordnet ist. Ein Beispiel für eine Store-ID eines Produkts ist 9nblggh42cfd.    |   Nein    |    |  Nein     |   |  


<span id="image-attributes"/>

## <a name="imageattributes-object"></a>ImageAttributes-Objekt

| Feld        | Typ   |  Beschreibung      |  Schreibgeschützt  | Standardwert  | Erforderlich für POST |  
|--------------|--------|---------------|------|-------------|------------|
|  imageExtension   |   String  |   Einer der folgenden Werte: **PNG** oder **JPG**.    |    Nein   |      |   Ja    |       |


## <a name="related-topics"></a>Verwandte Themen

* [Ausführen von Ad-Kampagnen, die mithilfe von Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md)
* [Ad-Kampagnen verwalten](manage-ad-campaigns.md)
* [Übermittlung Zeilen für Ad-Kampagnen verwalten](manage-delivery-lines-for-ad-campaigns.md)
* [Für die Zielgruppenadressierung Profile für Ad-Kampagnen verwalten](manage-targeting-profiles-for-ad-campaigns.md)
* [Ad-Kampagne-Leistungsdaten abrufen](get-ad-campaign-performance-data.md)
