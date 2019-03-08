---
title: Multiplayer-Sitzungsstatuscodes
description: Beschreibt die Statuscodes, die aus dem Xbox Live-Dienst zurückgegeben werden, wenn eine Multiplayer-Sitzung anfordert.
ms.assetid: 4ab320d6-8050-41a9-9f00-faaad3b128fd
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine Multiplayer-2015-Statuscodes "," Sitzung
ms.localizationpriority: medium
ms.openlocfilehash: 8fbddd0070eb24d6fc050c59fa2a0197f98ee08c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646305"
---
# <a name="multiplayer-session-status-codes"></a>Multiplayer-Sitzungsstatuscodes

Dieses Thema enthält Multiplayer-Statuscodes, die Sitzungen zu.

| Hinweis                                                                                                         |
|---------------------------------------------------------------------------------------------------------------------------|
| Die 4xx-Statuscodes, die die Sitzung immer zurückgeben zurückgeben die gesamte Sitzung aus, selbst wenn der URI auf ein Session-Element zeigt. |


| Statuscode | Zeichenfolge              | Content-Type     | Textkörper    | Beschreibung |
|----|
| 200         | OK                  | Anwendung/JSON | Sitzung | Erfolgreich lesen (GET) oder (PUT) aktualisiert.                                                                                                                                                                                                                                                                                                             |
| 201         | Erstellt             | Anwendung/JSON | Sitzung | Wurde erfolgreich erstellt.                                                                                                                                                                                                                                                                                                                                 |
| 202         | Akzeptiert            | Text/plain       | keine    | Die Anforderung wurde akzeptiert, jedoch wurde noch nicht abgeschlossen.                                                                                                                                                                                                                                                                                             |
| 204         | Kein Inhalt          |                  |         | Für GET für eine Sitzung ist die Sitzung nicht vorhanden. Auf Abruf der Session-Element die Sitzung ist vorhanden, aber das Element nicht. Für PUT-Vorgang für eine Sitzung wurde die Sitzung aufgrund der PUT-Vorgang gelöscht. Von PUT oder DELETE für Session-Element war die Sitzung bei Beginn des Vorgangs, jedoch entweder die Sitzung oder das Element nicht mehr vorhanden ist. |
| 304         | nicht geändert.        |                  |         | Für GET mit If-None-Match-Header wurde die Sitzung nicht geändert werden.                                                                                                                                                                                                                                                                                        |
| 400         | Ungültige Anforderung         | Text/plain       | Nachricht | Die Anforderung wird angenommen, bei der ersten Prüfung ungültig ist. Es fehlt ein erforderliches Feld, oder die JSON-Datei ist falsch formatiert. Der Text enthält weitere Details.                                                                                                                                                                                        |
| 403         | Verboten           | Text/plain       | Nachricht | Die Anforderung kann in bestimmten Kontexten gültig sein, jedoch ist für seinen Kontext ungültig. Fehler bei der Autorisierung.                                                                                                                                                                                                                                                |
|             |                     | Anwendung/JSON | Sitzung | Die Sitzung kann nicht vom Benutzer aktualisiert werden, aber Sie können gelesen werden.                                                                                                                                                                                                                                                                                           |
| 404         | Wurde nicht gefunden.           | Text/plain       | Nachricht | Die Sitzung kann nicht zugegriffen werden, da der URI ungültig ist; Das Handle, SCID oder sitzungsvorlage wurde nicht gefunden; eine Hopper kann nicht gefunden werden; Session-Element kann nicht zugegriffen werden, da die Sitzung nicht beendet wird; oder die Element-Suche ist für die Sitzung ungültig.                                                                                 |
| 405         | Methode, die nicht zulässig  | Text/plain       | Nachricht | Der Anforderungs-URI ist plausibel, aber das Verb ist falsch. Beispielsweise ist die Anforderung für eine POST-Vorgang, wenn ein PUT-Vorgang benötigt wird.                                                                                                                                                                                                                 |
| 407         | Konflikt            | Text/plain       | Nachricht | Die Sitzung konnte nicht aktualisiert werden, da die Anforderung mit der Sitzung nicht kompatibel ist. Z. B. Konstanten, die in der Anforderung verursachen einen Konflikt mit Konstanten in der Sitzung oder die Vorlage "Sitzung", oder der Aufrufer über die Member hinzugefügt oder aus einer großen Sitzung entfernt wurden.                                                                         |
| 412         | Vorbedingung nicht erfüllt |                  |         | Der If-Match-Header, oder der If-None-Match-Header (für einen Vorgang als GET), konnte nicht erfüllt werden.                                                                                                                                                                                                                                           |
|             |                     | Anwendung/JSON | Sitzung | Der If-Match-Header konnte nicht auf einen PUT- oder DELETE-Vorgang für eine vorhandene Sitzung erfüllt werden. Der aktuelle Status der Sitzung wird zusammen mit den aktuellen ETag-Wert zurückgegeben.                                                                                                                                                                      |
| 429 | Zu viele Anforderungen | Anwendung/JSON | Nachricht | Der Dienstaufruf wurde aufgrund der Überschreitung der differenzierte ratenbegrenzung Einschränkungen gedrosselt. Weitere Informationen finden Sie unter [fein abgestimmte Begrenzung der Übertragungsrate](../../using-xbox-live/best-practices/fine-grained-rate-limiting.md). |
| 503         | Dienst nicht verfügbar. | Text/plain       | keine    | Der Dienst ist überlastet, und die Anforderung später noch Mal wiederholt werden soll. Dieser Code enthält einen Retry-After-Header, den der Client berücksichtigt werden sollten.                                                                                                                                                                                                              |
