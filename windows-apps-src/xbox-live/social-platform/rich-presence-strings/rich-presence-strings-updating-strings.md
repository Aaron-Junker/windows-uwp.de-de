---
title: Umfassender vorhanden, die Zeichenfolgen aktualisieren
description: Erfahren Sie, wie Sie eine umfassende Vorhandensein von Xbox Live-Zeichenfolge zu aktualisieren.
ms.assetid: eb2bb82e-8730-4d74-9b33-95d133360e44
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, umfassender vorhanden
ms.localizationpriority: medium
ms.openlocfilehash: ac4549301c60eafb935dab0ac9c5b5028452edfb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610395"
---
# <a name="rich-presence-updating-strings"></a>Umfassender vorhanden, die Zeichenfolgen aktualisieren

Um die Rich-Präsenz-Zeichenfolge in den Titel zu aktualisieren, können Sie den schreiben Title-URI mit den entsprechenden Parametern in einem jsonobjekt aufrufen. Dieser Aufruf der restful wird auch von Xbox-Dienst-APIs umschlossen. Finden Sie unter **Microsoft.Xbox.Services.Presence Namespace** Informationen auf der entsprechenden-API.

Der URI sieht folgendermaßen aus:

          POST /users/xuid({xuid})/devices/current/titles/current

Im folgenden sind nur die Felder zum Festlegen der Anwesenheit von Rich-Zeichenfolgen. Es gibt andere optionale Felder, die im Zusammenhang mit der das Schreiben von Vorhandensein eines Titels hier nicht aufgeführt.

## <a name="titlerequest-object"></a>TitleRequest-Objekt

Eigenschaft | Typ | Req'd | Beschreibung
---|---|---|---
Aktivität|ActivityRequest|N|Datensatz, der beschreibt, in-softwaretitelinformationen (Rich-Anwesenheit und Medien-Info, falls verfügbar)

## <a name="activityrequest-object"></a>ActivityRequest-Objekt

Eigenschaft | Typ | Req'd | Beschreibung
---|---|---|---
richPresence|RichPresenceRequest|N|Die "FriendlyName" von der Anwesenheit von Rich-Zeichenfolge, die verwendet werden soll.

## <a name="richpresencerequest-object"></a>RichPresenceRequest-Objekt

Eigenschaft | Typ | Req'd | Beschreibung
---|---|---|---
Id|Zeichenfolge|„Y“ zugeordnet ist|Die "FriendlyName", der die Präsenz von Rich-Zeichenfolge, die verwendet werden soll
scid|Zeichenfolge|„Y“ zugeordnet ist|Scid, die mitteilt, wo die Anwesenheit von Rich-Zeichenfolgen definiert sind.

Wenn ich die Rich-Anwesenheit für Benutzer zu aktualisieren, deren Xuid 12345, würde z. B. Aufruf wie folgt aussehen:

          POST /users/xuid(12345)/devices/current/titles/current


Mit dem folgenden JSON-Text:

```json
          {
            activity:
            {
              richPresence:
              {
                id:"playingMap",
                scid:"0000-0000-0000-0000-01010101"
              }
            }
          }
```

Verwenden den API-Wrapper, wäre dies ein Aufruf von **PresenceService.SetPresenceAsync-Methode**

Wenn die beibehaltenen Header der Datenplattform auf dem neuesten Stand, verfügen Sie über die umfassende Anwesenheit Zeichenfolge jedes Mal zurückgesetzt. die Daten so füllen Sie die leere Änderungen. Im obigen Beispiel wissen wir, dass Sie die aktuelle Zuordnung verwenden möchten. Vorhandensein sieht Sie die Daten in die Data-Plattform, wenn ein Benutzer versucht, lesen Sie die Zeichenfolge, die den aktuellen Wert anzugeben. Selbst wenn der Spieler von der Karte, um wechseln ist Zuordnung zum zuordnen, müssen Sie die umfassender vorhanden-Zeichenfolge in Ihrem Spiel zurückgesetzt werden soll, solange Sie die entsprechenden Ereignisse für die Data-Plattform senden. Bedenken Sie, dass einige Sekunden, bis die Daten dauert auf ihrem Weg durch die Datenplattform zu suchen.

Wenn jemand versucht, Benutzeranwesenheit des 12345 Rich zu lesen, wird der Dienst dann sehen Sie sich, welche Gebietsschema angefordert wird und formatieren Sie die Zeichenfolge entsprechend vor der Rückgabe.

In diesem Fall nehmen wir an, dass ein Benutzer möchte, die En-US-Zeichenfolge zu lesen. Lesen von umfassender vorhanden funktioniert wie folgt (Weitere Informationen zu diesen Aufruf, finden Sie unter **GET (/users/xuid({xuid}))**

          GET /users/xuid(12345)?level=all

Wird von der API-Wrapper für diese **PresenceService.GetPresenceAsync-Methode**

Woran liegt das hier ist, dass Sie für die PresenceRecord des Benutzers, Fragen, deren Xuid 12345 ist. Und Sie können anfordern, dass die Detailebene "alle" sein. Wenn "all" wurde nicht angegeben, würden Rich Vorhandensein nicht zurückgegeben werden.

Und sie würde Folgendes in die JSON-Antwort zurück:

```json
          {
            xuid:"12345",
            state:"online",
            devices:
            [
              {
                type:"D",
                titles:
                [
                  {
                    id:"12345",
                    name:"Buckets are Awesome",
                    lastModified:"2012-09-17T07:15:23.4930000",
                    placement: "full",
                    state:"active",
                    activity:
                    {
                      richPresence:"Playing on map:Mountains"
                    }
                  }
                ]
              }
            ]
          }
```
