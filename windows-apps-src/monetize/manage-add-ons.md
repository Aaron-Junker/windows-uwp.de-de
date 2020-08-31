---
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: Verwenden Sie diese Methoden in der Microsoft Store Übermittlungs-API, um Add-ons für apps zu verwalten, die bei Ihrem Partner Center-Konto registriert sind.
title: Verwalten von Add-Ons
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Add-ons, in-App-Produkt, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 7f02e222cf495f56352a645ac3a366da39dc5e3a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158414"
---
# <a name="manage-add-ons"></a>Verwalten von Add-Ons

Verwenden Sie die folgenden Methoden in der Microsoft Store Übermittlungs-API, um Add-ons für Ihre apps zu verwalten. Eine Einführung in die Microsoft Store Übermittlungs-API, einschließlich der Voraussetzungen für die Verwendung der API, finden [Sie unter Erstellen und Verwalten von Übermittlungen mithilfe Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md).

Diese Methoden können nur verwendet werden, um Add-Ons abzurufen, zu erstellen oder zu löschen. Um Übermittlungen für Add-Ons zu erstellen, verwenden Sie die Methoden in [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md).

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Methode</th>
<th align="left">URI</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts</td>
<td align="left"><a href="get-all-add-ons.md">Abrufen aller Add-Ons für Ihre Apps</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}</td>
<td align="left"><a href="get-an-add-on.md">Abrufen eines bestimmten Add-Ons</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts</td>
<td align="left"><a href="create-an-add-on.md">Erstellen eines Add-Ons</a></td>
</tr>
<tr>
<td align="left">Delete</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}</td>
<td align="left"><a href="delete-an-add-on.md">Löschen eines Add-Ons</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie dies noch nicht getan haben, müssen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store Übermittlungs-API erfüllen, bevor Sie versuchen, diese Methoden zu verwenden.

## <a name="data-resources"></a>Datenressourcen

Die Methoden zur Übermittlung von Microsoft Store Übermittlungs-API zum Verwalten von Add-ons verwenden die folgenden JSON-Datenressourcen.

<span id="add-on-object" />

### <a name="add-on-resource"></a>Add-On-Ressource

Diese Ressource beschreibt ein Add-On.

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
  "id": "9NBLGGH4TNMP",
  "productId": "TestAddOn",
  "productType": "Durable",
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
  "lastPublishedInAppProductSubmission": {
    "id": "1152921504621243705",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
  }
}
```

Die Ressource hat die folgenden Werte.

| Wert      | Typ   | BESCHREIBUNG        |
|------------|--------|--------------|
| applications      | array  | Ein Array mit genau einer [Anwendungsressource](#application-object), die die App darstellt, der dieses Add-On zugeordnet ist. In diesem Array wird nur ein Element unterstützt.  |
| id | Zeichenfolge  | Die Store-ID des Add-Ons. Dieser Wert wird vom Store bereitgestellt. Beispiel für eine Store-ID: 9NBLGGH4TNMP.  |
| productId | Zeichenfolge  | Die Produkt-ID des Add-Ons. Dies ist die ID, die vom Entwickler während der Erstellung des Add-Ons angegeben wurde. Weitere Informationen finden Sie unter [Festlegen von Produkttyp und Produkt-ID für das IAP](../publish/set-your-add-on-product-id.md). |
| productType | Zeichenfolge  | Der Produkttyp des Add-Ons. Die folgenden Werte werden unterstützt: **Durable** und **Consumable**.  |
| lastPublishedInAppProductSubmission       | Objekt (object) | Eine [Übermittlungsressource](#submission-object) mit Informationen über die letzte veröffentlichte Übermittlung für das Add-On.         |
| pendingInAppProductSubmission        | Objekt (object)  |  Eine [Übermittlungsressource](#submission-object) mit Informationen über die aktuelle ausstehende Übermittlung für das Add-On.  |   |

<span id="application-object" />

### <a name="application-resource"></a>Anwendungsressource

Diese Ressource beschreibt die App, der ein Add-On zugeordnet ist. Das folgende Beispiel veranschaulicht das Format der Ressource.

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
}
```

Die Ressource hat die folgenden Werte.

| Wert           | Typ    | BESCHREIBUNG        |
|-----------------|---------|-----------|
| value            | Objekt (object)  |  Ein Objekt, das die folgenden Werte enthält: <br/><br/> <ul><li>*ID*. die Speicher-ID der app. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](../publish/view-app-identity-details.md).</li><li>*resourcelokation*. Ein relativer Pfad, den Sie an den Basisanforderungs-URI ```https://manage.devcenter.microsoft.com/v1.0/my/``` anfügen können, um die vollständigen Daten für die App abzurufen.</li></ul>   |
| totalCount   | INT  | Die Anzahl der App-Objekte im *applications*-Array des Antworttexts.                                                                                                                                                 |

<span id="submission-object" />

### <a name="submission-resource"></a>Übermittlungsressource

Diese Ressource enthält Informationen über eine Übermittlung für ein Add-On. Das folgende Beispiel veranschaulicht das Format der Ressource.

```json
{
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
}
```

Die Ressource hat die folgenden Werte.

| Wert           | Typ    | BESCHREIBUNG     |
|-----------------|---------|------------------|
| id            | Zeichenfolge  | Die ID der Übermittlung.    |
| resourceLocation   | Zeichenfolge  | Ein relativer Pfad, den Sie an den Basisanforderungs-URI ```https://manage.devcenter.microsoft.com/v1.0/my/``` anfügen können, um die vollständigen Daten für die Übermittlung abzurufen.     |
 
<span/>

## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Add-on-Übermittlungen mithilfe der Microsoft Store Übermittlungs-API](manage-add-on-submissions.md)
* [Abrufen aller Add-Ons](get-all-add-ons.md)
* [Abrufen eines Add-Ons](get-an-add-on.md)
* [Erstellen eines Add-Ons](create-an-add-on.md)
* [Löschen eines Add-Ons](delete-an-add-on.md)