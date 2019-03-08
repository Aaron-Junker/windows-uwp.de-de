---
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: Verwenden Sie diese Methoden in der Microsoft Store-Übermittlung API zum Verwalten von Add-ons für apps, die mit Ihrem Partner Center-Konto registriert sind.
title: Verwalten von Add-Ons
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Add-Ons, In-App-Produkt, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 51c940fffde3c770f397999e566570410528a1e8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617825"
---
# <a name="manage-add-ons"></a>Verwalten von Add-Ons

Mithilfe der folgenden Methoden in der Microsoft Store-Übermittlungs-API können Sie Add-Ons für Ihre Apps verwalten. Eine Einführung in die Microsoft Store-Übermittlungs-API einschließlich der Voraussetzungen für die Verwendung der API finden Sie unter [Erstellen und Verwalten von Übermittlungen mit Microsoft Store-Diensten](create-and-manage-submissions-using-windows-store-services.md).

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
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts</td>
<td align="left"><a href="get-all-add-ons.md">Alle Add-Ons für Ihre apps</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}</td>
<td align="left"><a href="get-an-add-on.md">Abrufen eines bestimmten Add-Ons</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts</td>
<td align="left"><a href="create-an-add-on.md">Ein Add-on erstellen</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}</td>
<td align="left"><a href="delete-an-add-on.md">Add-on löschen</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>Voraussetzungen

Falls noch nicht geschehen, sorgen Sie vor der Verwendung dieser Methoden dafür, dass alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store-Übermittlungs-API erfüllt sind.

## <a name="data-resources"></a>Datenressourcen

Die Methoden der Microsoft Store-Übermittlungs-API für das Verwalten von Add-Ons verwenden die folgenden JSON-Datenressourcen.

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

| Wert      | Typ   | Beschreibung        |
|------------|--------|--------------|
| applications      | array  | Ein Array mit genau einer [Anwendungsressource](#application-object), die die App darstellt, der dieses Add-On zugeordnet ist. In diesem Array wird nur ein Element unterstützt.  |
| id | string  | Die Store-ID des Add-Ons. Dieser Wert wird vom Store bereitgestellt. Beispiel für eine Store-ID: 9NBLGGH4TNMP.  |
| productId | string  | Die Produkt-ID des Add-Ons. Dies ist die ID, die vom Entwickler während der Erstellung des Add-Ons angegeben wurde. Weitere Informationen finden Sie unter [Festlegen von Produkttyp und Produkt-ID für das IAP](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id). |
| productType | string  | Der Produkttyp des Add-Ons. Die folgenden Werte werden unterstützt: **Dauerhafte** und **nutzbar**.  |
| lastPublishedInAppProductSubmission       | object | Eine [Übermittlungsressource](#submission-object) mit Informationen über die letzte veröffentlichte Übermittlung für das Add-On.         |
| pendingInAppProductSubmission        | object  |  Eine [Übermittlungsressource](#submission-object) mit Informationen über die aktuelle ausstehende Übermittlung für das Add-On.  |   |

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

| Wert           | Typ    | Beschreibung        |
|-----------------|---------|-----------|
| value            | object  |  Ein Objekt, das die folgenden Werte enthält: <br/><br/> <ul><li>*id*. Die Store-ID der App. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).</li><li>*resourceLocation*. Ein relativer Pfad, den Sie an den Basisanforderungs-URI ```https://manage.devcenter.microsoft.com/v1.0/my/``` anfügen können, um die vollständigen Daten für die App abzurufen.</li></ul>   |
| totalCount   | int  | Die Anzahl der App-Objekte im *applications*-Array des Antworttexts.                                                                                                                                                 |

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

| Wert           | Typ    | Beschreibung     |
|-----------------|---------|------------------|
| id            | string  | Die ID der Übermittlung.    |
| resourceLocation   | string  | Ein relativer Pfad, den Sie an den Basisanforderungs-URI ```https://manage.devcenter.microsoft.com/v1.0/my/``` anfügen können, um die vollständigen Daten für die Übermittlung abzurufen.     |
 
<span/>

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Verwenden die Übermittlung zum Microsoft Store-API-Add-On-Übermittlungen verwalten](manage-add-on-submissions.md)
* [Alle Add-ons zu erhalten](get-all-add-ons.md)
* [Ein Add-on abzurufen](get-an-add-on.md)
* [Ein Add-on erstellen](create-an-add-on.md)
* [Add-on löschen](delete-an-add-on.md)
