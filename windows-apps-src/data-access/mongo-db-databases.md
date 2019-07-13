---
title: Verwenden einer MongoDB-Datenbank in einer UWP-App
description: Verwenden einer MongoDB-Datenbank in einer UWP-App
ms.date: 03/28/2019
ms.topic: article
keywords: Windows 10, UWP, MongoDB, Datenbank
ms.localizationpriority: medium
ms.openlocfilehash: ea3e630a3bb0b8fc5f7f4168b0b946f115b97446
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67713999"
---
# <a name="use-a-mongodb-database"></a>Verwenden einer MongoDB-Datenbank
Dieser Artikel enthält die erforderlichen Schritte zur Verwendung einer MongoDB-Datenbank in einer UWP-App. Er enthält auch einen kleinen Codeausschnitt zur Veranschaulichung, wie Sie im Code mit der Datenbank interagieren können.

## <a name="set-up-your-solution"></a>Einrichten der Lösung

Um Ihre App direkt mit einer MongoDB-Datenbank zu verbinden, stellen Sie sicher, dass für die Mindestversion Ihres Projekts das Fall Creators Update (Build 16299) festgelegt ist.  Sie finden diese Informationen auf der Eigenschaftenseite des UWP-Projekts.

![Abbildung des Eigenschaftenbereichs in Visual Studio mit dem Fall Creators Update als festgelegter Ziel- und Mindestversion](images/min-version-fall-creators.png)

Öffnen Sie die **Paket-Manager-Konsole** („Ansicht“ -> „Weitere Fenster“ -> „Paket-Manager-Konsole“). Verwenden Sie den Befehl **Install-Package MongoDB.Driver**, um den Treiber für MongoDB zu installieren. Dadurch können Sie programmgesteuert auf MongoDB-Datenbanken zugreifen.

## <a name="test-your-connection-using-sample-code"></a>Testen der Verbindung mit Beispielcode
Mit dem folgenden Beispielcode wird eine Sammlung von einem MongoDB-Remoteclient abgerufen und dann dieser Sammlung ein neues Dokument hinzugefügt. Anschließend werden über MongoDB-APIs die neue Größe der Sammlung sowie das eingefügte Dokument abgerufen und ausgegeben. Beachten Sie, dass die IP-Adresse und die Datenbanknamen angepasst werden müssen.

```csharp
var client = new MongoClient("mongodb://10.xxx.xx.xxx:xxx");
IMongoDatabase database = client.GetDatabase("foo");
IMongoCollection<BsonDocument> collection = database.GetCollection<BsonDocument>("bar");
BsonDocument document = new BsonDocument
{
     { "name","MongoDB"},
     { "type","Database"},
     { "count",1},
     { "info",new BsonDocument { { "x", 203 }, { "y", 102 } }}
};
collection.InsertOne(document);
long count = collection.CountDocuments(document);
Debug.WriteLine(count);
IFindFluent<BsonDocument, BsonDocument> document1 = collection.Find(document);
Debug.WriteLine(document1.ToString());
```
