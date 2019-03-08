---
title: Neuerungen für die Xbox Live SDK – August 2015
description: Neuerungen für die Xbox Live SDK – August 2015
ms.assetid: a034867b-7cc0-4b97-89d5-3486e95a80b4
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: a454b7339035475416934c2f921dae65283c340c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639815"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2015"></a>Neuerungen für die Xbox Live SDK – August 2015

Finden Sie unter den [– Juni 2015 Neuigkeiten](1506-whats-new.md) Artikel Neuheiten in der Version Juni 2015.

Die August-Version des Xbox Live SDK umfasst die folgenden Updates:

## <a name="os-and-tool-support"></a>Unterstützung für Betriebssystem und Tools
Das Xbox Live-SDK unterstützt jetzt Windows 10 RTM [Version 10.0.10240] und Visual Studio 2015 RTM [Version 14.0.23107.0].

## <a name="multiplayer-manager-winrt-apis"></a>Multiplayer-Manager-WinRT-APIs
Multiplayer-Manager (in der experimentellen Namespace) unterstützt jetzt auch die WinRT-APIs (zusätzlich zu den C++-APIs)

## <a name="submit-batch-feedback-from-a-title"></a>Übermitteln von Batch-Feedback über einen Titel
Sendet eine Anzahl von von feedbackelementen auf einmal von einem Titel.

## <a name="newupdated-documentation"></a>Neue/aktualisierte Dokumentation
Das Xbox Live SDK-Paket jetzt einen Ordner "Docs" enthält, enthält aktualisierte API-Referenzen und die neue "Xbox Live programming Guide".

Fehlerkorrekturen:

* Beim Entfernen des Abonnements im Real-Time-Aktivität-Dienst stürzt ab
* Stürzt ab, wenn Sie mit einem Gastkonto anmelden
* Zugriffsverletzung beim Ziehen, um das Netzwerkkabel
* Tunnel Fehler geben jetzt einen Fehlercode in der C++-APIs
* ETag-Problem mit TitleStorageService::DownloadBlobAsync
* Verschiedene Fehlerbehebungen für Beispiel-apps.
