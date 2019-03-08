---
title: Lokalisierte Zeichenfolgen
description: Erfahren Sie, wie zum Lokalisieren von Zeichenfolgen in Partner Center
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 11/17/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, Spiele, Uwp, Windows 10, Xbox, lokalisierte Zeichenfolgen, die Partner Center
ms.openlocfilehash: 127f566dc5ae57b920d396623b6a84ff5d5eed96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656595"
---
# <a name="configuring-localized-strings-in-partner-center"></a>Konfigurieren lokalisierte Zeichenfolgen im Partner Center

Sie können diese Seite verwenden, um alle Ihre Xbox Live Konfigurationen in allen Sprachen zu lokalisieren, die Ihr Spiel unterstützt. Alle die Dienstkonfigurationen, die Sie in den nachfolgenden Xbox Live-Seiten erstellt haben wird die Datei hinzugefügt werden, die Sie herunterladen sollen.

Sie können [Partner Center](https://partner.microsoft.com/dashboard) so konfigurieren Sie die lokalisierten Zeichenfolgen in allen Sprachen, die Ihr Spiel zugeordnet. Fügen Sie Konfiguration wie folgt hinzu:

1. Navigieren Sie zu der **lokalisierte Zeichenfolgen** Titel, befindet sich im Abschnitt **Services** > **Xbox Live** > **lokalisierte Zeichenfolgen**.
2. Klicken Sie auf die **herunterladen** Schaltfläche, die eine localization.xml-Datei auf Ihrem lokalen Computer heruntergeladen.

![Screenshot der Konfigurationsseite lokalisierte Zeichenfolgen im Partner Center](../../images/dev-center/localized-strings/localized-strings-1.png)

3. Sie können die lokalisierten Zeichenfolgen hinzufügen, indem das Duplizieren der <Value locale="en-US">Labyrinthe wiedergegeben</Value> Tag, und ändern den Wert des Gebietsschemas in der Sprache Ihrer Wahl und der Wert, der die lokalisierte Zeichenfolge. Sie benötigen mindestens einen Tag innerhalb des Gebietsschemas Developer anzeigen, um Fehler zu vermeiden.

![Bearbeiten Sie die lokalisierte Zeichenfolgen](../../images/dev-center/localized-strings/localized-strings.gif)

4. Nachdem Sie alle lokalisierten Zeichenfolgen hinzugefügt haben, können Sie die Datei hochladen, durch Ziehen oder den lokalen Computer zu durchsuchen.

![Bild der Schaltfläche zum Hochladen der Datei localization.xml](../../images/dev-center/localized-strings/localized-strings-2.png)

Beachten Sie, dass die folgenden Fehler beim Hochladen der Datei localization.xml angezeigt werden können:

| Fehler | Grund |
|---------------------------|-------------|
| Fehlerhafte XSD-Überprüfung: Das Element 'LocalizedString' im Namespace 'http://config.mgt.xboxlive.com/schema/localization/1' darf keinen Text enthalten. Die Liste der erwarteten möglichen Elemente: "Value" im Namespace 'http://config.mgt.xboxlive.com/schema/localization/1" | Dieses Problem tritt bei der XML-Dokument falsch formatiert ist. |
| Lokalisierungszeichenfolgen ist kein Eintrag für das Gebietsschema für Entwickler die Anzeige vorhanden. | Dies geschieht, wenn eine lokalisierte Zeichenfolge kein Eintrag vorhanden ist, deren Gebietsschema nicht das Gebietsschema der Dev-Anzeige entspricht |
| Fehlerhafte XSD-Überprüfung: Das Attribut "Locale" ist ungültig: der Wert ' "ist gemäß seinem Datentyp ungültig"http://config.mgt.xboxlive.com/schema/localization/1:NonEmptyString'-die mustereinschränkung ist fehlgeschlagen. | Dies geschieht, wenn eine lokalisierte Zeichenfolge den Wert der Gebietsschema in fehlt die <Value> tag|
