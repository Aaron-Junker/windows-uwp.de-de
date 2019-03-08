---
ms.assetid: A4C6098B-6CB9-4FAF-B2EA-50B03D027FF1
description: Verwenden Sie diese Methode in der Microsoft Store-API für gezielte Angebote, um die gezielten Angebote abzurufen, die für den aktuellen Benutzer im Zusammenhang mit der aktuellen App verfügbar sind.
title: Abrufen von gezielten Angeboten
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, Store-Dienste, Microsoft Store-API für gezielte Angebote, gezielte Angebote abrufen
ms.localizationpriority: medium
ms.openlocfilehash: 71cd6ce3b9736b812f8ccdf4d21d35357928c63c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622765"
---
# <a name="get-targeted-offers"></a>Abrufen von gezielten Angeboten

Verwenden Sie diese Methode, um die gezielten Angebote abzurufen, die für den aktuellen Benutzer verfügbar sind – basierend darauf, ob der Benutzer zum Kundensegment für das gezielte Angebot gehört. Weitere Informationen finden Sie unter [Verwalten gezielter Angebote mithilfe von Store-Diensten](manage-targeted-offers-using-windows-store-services.md).

## <a name="prerequisites"></a>Voraussetzungen

Um diese Methode zu verwenden, müssen Sie zunächst [ein Microsoft-Kontotoken abrufen](manage-targeted-offers-using-windows-store-services.md#obtain-a-microsoft-account-token), und zwar für den aktuell angemeldeten Benutzer Ihrer App. Sie müssen bei dieser Methode das Token an den ```Authorization```-Anforderungsheader übergeben. Dieses Token wird vom Store verwendet, um gezielte Angebote für den aktuellen Benutzer abzurufen.

## <a name="request"></a>Anfordern


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung  |
|---------------|--------|--------------|
| Autorisierung | string | Erforderlich. Das Microsoft-Account-Token für den aktuellen angemeldeten Benutzer Ihrer App in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

Diese Methode hat keinen URI oder Anforderungsparameter.

### <a name="request-example"></a>Anforderungsbeispiel

```syntax
GET https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user HTTP/1.1
Authorization: Bearer <Microsoft Account token>
```

## <a name="response"></a>Antwort

Diese Methode gibt einen Antworttext im JSON-Format zurück, der ein Array mit Objekten und den folgenden Feldern enthält. Jedes Objekt im Array stellt gezielte Angebote dar, die für den angegebenen Kunden im Rahmen eines bestimmten Kundensegments verfügbar sind.

| Feld      | Typ   | Beschreibung         |
|------------|--------|------------------|
| Angebote      | array  | Ein Array mit Produkt-IDs für die Add-Ons, die den gezielten Angeboten zugeordnet sind, die für den aktuellen Benutzer verfügbar sind. Diese Produkt-IDs werden angegeben, der **Ziel Angebote** für Ihre app im Partner Center.            |
| trackingId  | string | Eine GUID, mit der Sie optional das gezielte Angebot in Ihrem eigenen Code oder Diensten nachverfolgen können. |


### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
[
  {
    "offers": [
      "10x gold coins",
      "100x gold coins"
    ],
    "trackingId": "5de5dd29-6dce-4e68-b45e-d8ee6c2cd203"
  }
]
```

## <a name="related-topics"></a>Verwandte Themen

* [Verwalten von zielgerichteten angeboten, die mithilfe von Store-services](manage-targeted-offers-using-windows-store-services.md)

 

 
