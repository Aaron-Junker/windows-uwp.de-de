---
ms.assetid: c5246681-82c7-44df-87e1-a84a926e6496
description: Verwenden Sie diese Methode in der Microsoft Store Promotions-API, um die-Aktionen für Werbekampagnen zu verwalten.
title: Verwalten von Werbemitteln
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store promotionapi, Werbekampagnen
ms.localizationpriority: medium
ms.openlocfilehash: 90a186a63104c0393cb871ceffee023a05a7d514
ms.sourcegitcommit: 80ea62d6c0ee25d73750437fe1e37df5224d5797
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2021
ms.locfileid: "105619636"
---
# <a name="manage-creatives"></a>Verwalten von Werbemitteln

Verwenden Sie diese Methoden in der Microsoft Store promotionapi, um Ihre eigenen benutzerdefinierten erbungen zur Verwendung in Werbekampagnen für Werbeaktionen hochzuladen oder eine vorhandene Creative zu erhalten. Ein Creative-Vorgang kann mit einer oder mehreren Übermittlungs Zeilen verknüpft werden, auch bei Ad-Kampagnen, sofern er immer dieselbe APP darstellt.

Weitere Informationen zur Beziehung zwischen den-und-Werbekampagnen, Übermittlungs Zeilen und Ziel Profilen finden Sie unter [Ausführen von Ad-Kampagnen mit Microsoft Store-Diensten](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

> [!NOTE]
> Wenn Sie diese API zum Hochladen Ihrer eigenen Creative-API verwenden, ist die maximal zulässige Größe für Ihr Creative-Wert 40 KB. Wenn Sie eine Creative-Datei übermitteln, gibt diese API keinen Fehler zurück, aber die Kampagne wird nicht erfolgreich erstellt.

## <a name="prerequisites"></a>Voraussetzungen

Um diese Methoden zu verwenden, müssen Sie zunächst die folgenden Schritte ausführen:

* Wenn Sie dies noch nicht getan haben, müssen Sie alle [Voraussetzungen](run-ad-campaigns-using-windows-store-services.md#prerequisites) für die Microsoft Store promotionapi erfüllen.
* Rufen Sie [ein Azure AD Zugriffs Token](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) ab, das im-Anforderungs Header für diese Methoden verwendet werden soll. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.


## <a name="request"></a>Anforderung

Diese Methoden verfügen über die folgenden URIs.

| Methodentyp | Anforderungs-URI     |  BESCHREIBUNG  |
|--------|-----------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative``` |  Erstellt eine neue Creative-.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/{creativeId}``` |  Ruft das von " *kreativeid*" angegebene Creative-Element ab.  |

> [!NOTE]
> Diese API unterstützt derzeit keine Put-Methode.


### <a name="header"></a>Header

| Header        | type   | BESCHREIBUNG         |
|---------------|--------|---------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |
| Nachverfolgungs-ID   | GUID   | Optional. Eine ID, die den aufrufflow nachverfolgt.                                  |


### <a name="request-body"></a>Anforderungstext

Die Post-Methode erfordert einen JSON-Anforderungs Text mit den erforderlichen Feldern eines [kreativen](#creative) Objekts.


### <a name="request-examples"></a>Beispiele für Anforderungen

Im folgenden Beispiel wird veranschaulicht, wie die Post-Methode aufgerufen wird, um eine Creative-Methode zu erstellen. In diesem Beispiel wurde der *Inhalts* Wert aus Gründen der Kürze verkürzt.

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

Im folgenden Beispiel wird veranschaulicht, wie Sie die Get-Methode aufrufen, um einen kreativen abzurufen.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/106851  HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Antwort

Diese Methoden geben einen JSON-Antworttext mit einem [Creative](#creative) -Objekt zurück, das Informationen über das erstellte oder abgerufene Creative-Objekt enthält. Das folgende Beispiel zeigt einen Antworttext für diese Methoden. In diesem Beispiel wurde der *Inhalts* Wert aus Gründen der Kürze verkürzt.

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

## <a name="creative-object"></a>Creative-Objekt

Die Anforderungs-und Antwort Texte für diese Methoden enthalten die folgenden Felder. Diese Tabelle zeigt, welche Felder schreibgeschützt sind (was bedeutet, dass Sie in der Put-Methode nicht geändert werden können) und welche Felder im Anforderungs Text für die Post-Methode erforderlich sind.

| Feld        | type   |  BESCHREIBUNG      |  Nur Lesezugriff  | Standard  |  Für Post erforderlich |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  integer   |  Die ID der Creative.     |   Ja    |      |    Nein   |       
|  name   |  Zeichenfolge   |   Der Name der Creative.    |    Nein   |      |  Ja     |       
|  Inhalt   |  Zeichenfolge   |  Der Inhalt des kreativen Bilds im Base64-codierten Format.<br/><br/> &nbsp; Hinweis &nbsp; Die maximal zulässige Größe für Ihre Creative beträgt 40 KB. Wenn Sie eine Creative-Datei übermitteln, gibt diese API keinen Fehler zurück, aber die Kampagne wird nicht erfolgreich erstellt.     |  Nein     |      |   Ja    |       
|  height   |  integer   |   Die Höhe der Creative.    |    Nein    |      |   Ja    |       
|  width   |  integer   |  Die Breite des Creative-.     |  Nein    |     |    Ja   |       
|  landingurl   |  Zeichenfolge   |  Wenn Sie einen Kampagnen nach Verfolgungs Dienst wie appsflyer, Kochava, Tune oder vungle verwenden, um die Installations Analyse für Ihre APP zu messen, weisen Sie die nach Verfolgungs-URL in diesem Feld zu, wenn Sie die Post-Methode aufzurufen (wenn angegeben, muss dieser Wert ein gültiger URI sein). Wenn Sie keinen Kampagnen Überwachungsdienst verwenden, sollten Sie diesen Wert weglassen, wenn Sie die Post-Methode (in diesem Fall wird diese URL automatisch erstellt).   |  Nein    |     |   Ja    |
|  format   |  Zeichenfolge   |   Das Ad-Format. Derzeit ist der einzige unterstützte Wert **Banner**.    |   Nein    |  Banner   |  Nein     |       
|  Image Attributes   | [Image Attributes](#image-attributes)    |   Stellt Attribute für den kreativen bereit.     |   Nein    |      |   Ja    |       
|  storeproductid   |  Zeichenfolge   |   Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) für die APP, mit der diese AD-Kampagne verknüpft ist. Eine Beispiel-Speicher-ID für ein Produkt ist 9nblggh42cfd.    |   Nein    |    |  Nein     |


<span id="image-attributes"/>

## <a name="imageattributes-object"></a>Imageattributobjekt

| Feld        | type   |  BESCHREIBUNG      |  Schreibgeschützt  | Standardwert  | Für Post erforderlich |  
|--------------|--------|---------------|------|-------------|------------|
|  Image Erweiterung   |   Zeichenfolge  |   Einer der folgenden Werte: **PNG** oder **JPG**.    |    Nein   |      |   Ja    |


## <a name="related-topics"></a>Zugehörige Themen

* [Ausführen von Ad-Kampagnen mithilfe von Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md)
* [Verwalten von Anzeigenkampagnen](manage-ad-campaigns.md)
* [Verwalten von Übermittlungs Zeilen für Werbekampagnen](manage-delivery-lines-for-ad-campaigns.md)
* [Verwalten von Ziel Profilen für Werbekampagnen](manage-targeting-profiles-for-ad-campaigns.md)
* [Abrufen der Leistungsdaten einer Anzeigenkampagne](get-ad-campaign-performance-data.md)
