---
title: Xbox Live-C-APIs
description: Erfahren Sie, bis das flache C-API-Modell, das Sie verwenden können, um die Interaktion mit dem Xbox Live-Dienst.
ms.date: 06/05/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, c, xsapi
ms.localizationpriority: medium
ms.openlocfilehash: a1c73661b561d586f9e28957c7caa6a1b1f9cb03
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597525"
---
# <a name="introduction-to-the-xbox-live-c-apis"></a>Einführung in die Xbox Live-C-APIs

Im Juni 2018 wurde eine neue Flatfile-C-API-Ebene XSAPI hinzugefügt. Diese neue API-Ebene löst einige Probleme, die mit C++ und WinRT-API-Schichten aufgetreten sind.

Die C-API wird nicht noch alle XSAPI Funktionen behandelt, aber zusätzliche Funktionen gearbeitet werden. Alle 3-API-Ebenen, C, C++ und WinRT weiterhin unterstützt werden, und haben zusätzliche Funktionen, die im Laufe der Zeit hinzugefügt.

> [!NOTE]
> Die C-APIs funktionieren derzeit nur mit den Titeln, die der xdk (Xbox Developer Kit-Version) zu verwenden. Sie unterstützen keine UWP-Spiele zu diesem Zeitpunkt.

## <a name="features-covered-by-the-c-apis"></a>Funktionen, die von den C#-APIs behandelt

Die C-API unterstützt derzeit die folgenden Features und Dienste:

- Erfolge
- Präsenz
- Profil
- Gemeinschaft
- Soziale Medien-Manager

## <a name="benefits-of-the-c-api-for-xsapi"></a>Vorteile von der C-API für XSAPI

- Ermöglicht die Titel für die speicherbelegung zu steuern, wann XSAPI aufrufen.
- Ermöglicht die Titel für die vollständige Kontrolle über die Behandlung beim Aufrufen von XSAPI Thread zu erhalten.
- Verwendet eine neue HTTP-Bibliothek, LibHttpClient, für Spieleentwickler konzipiert.

Können Sie die C-APIs zusammen mit C++ XSAPI, aber Sie werden nicht die zuvor aufgeführten Vorteile erhalten, mit der C++-APIs.

### <a name="managing-memory-allocations"></a>Verwaltung von speicherbelegungen

Mit der neuen C-API können Sie jetzt ein Funktionsrückruf angeben, die XSAPI aufgerufen wird, wenn er versucht, den Speicher zu belegen. Wenn Sie keine Funktionsrückrufe angeben, wird XSAPI standard-Speicher-speicherbelegungsroutinen verwenden.

Um Ihre Speicher-Routinen manuell angeben möchten, können Sie Folgendes tun:

- Beim Starten des Spiels:
  - Rufen Sie `XblMemSetFunctions(memAllocFunc, memFreeFunc)` an der Zuordnung von Rückrufen für zuweisen und Freigeben von Arbeitsspeicher.
  - Rufen Sie `XblInitialize()` zum Initialisieren der Bibliothek-Instanz.  
- Beim Ausführen des Spiels ist:
  - Aufrufen eine der neuen C-APIs in XSAPI, zuordnen oder freier Speicher bewirkt, dass XSAPI Behandlung von Rückrufen aufrufen, den angegebenen Speicher.  
- Wenn das Spiel beendet wird:
  - Rufen Sie `XblCleanup()` , alle mit der Bibliothek XSAPI zugeordnete Ressourcen freizugeben.
  - Bereinigen Sie Ihr Spiel für den benutzerdefinierten Speicher-Manager.

### <a name="managing-asynchronous-threads"></a>Verwalten von asynchronen threads

Die C-API führt einen neuen asynchrone Thread-Muster, das Entwickler vollständige Kontrolle über dem Threadingmodell kann aufrufen. Weitere Informationen finden Sie unter [aufrufen-Muster für XSAPI flache C-Ebene asynchrone Aufrufe](flatc-async-patterns.md).

## <a name="migrating-code-to-use-c-xsapi"></a>Migrieren von Code mit C XSAPI

Die XSAPI-C-APIs können zusammen mit den XSAPI C++-APIs in einem Projekt verwendet werden, daher wird empfohlen, ein Feature gleichzeitig zu migrieren.

Die APIs der C und C++-APIs sind eigentlich nur dünne Wrapper für ein gemeinsamer Kern, nur für verschiedene Einstiegspunkte, damit die Funktionalität nicht geändert werden soll. Allerdings können nur die C-APIs des benutzerdefinierten Speicher und Threads Verwaltungsfunktionen nutzen.

> [!IMPORTANT]
> Sie können keine XSAPI WinRT-APIs mit der C-APIs kombinieren.

## <a name="where-to-view-the-c-apis"></a>Zum Anzeigen von der C-APIs

- [Headerdateien der C-API](https://github.com/Microsoft/xbox-live-api/tree/master/Include/xsapi-c)
- [Beispielcode, die mit den neuen C#-APIs](https://github.com/Microsoft/xbox-live-api/tree/master/InProgressSamples/Social/Xbox/C)
- [libHttpClient](https://github.com/Microsoft/libHttpClient)
