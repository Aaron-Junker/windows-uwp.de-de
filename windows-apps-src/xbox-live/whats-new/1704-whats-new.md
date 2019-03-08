---
title: Neuerungen für die Xbox Live-APIs – April 2017
description: Neuerungen für die Xbox Live-APIs – April 2017
ms.assetid: ''
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Bereich, Turniere
ms.localizationpriority: medium
ms.openlocfilehash: a9256bfce05c14a0bfa63c7ec656dd899648d844
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651305"
---
# <a name="whats-new-for-the-xbox-live-apis---april-2017"></a>Neuerungen für die Xbox Live-APIs – April 2017

Informieren Sie sich die [– März 2017 Neuigkeiten](1703-whats-new.md) Artikel was hinzugefügt wurde im März 2017-Release.

## <a name="xbox-services-apis"></a>Xbox-Services-APIs

### <a name="visual-studio-2017"></a>Visual Studio 2017

Die Xbox Live-APIs wurden zur Unterstützung von Visual Studio 2017, für die universelle Windows-Plattform (UWP) und Xbox One Titel aktualisiert.

### <a name="tournaments"></a>Turniere

Neue APIs wurden hinzugefügt, um Turniere zu unterstützen. Sie können nun die `xbox::services::tournaments::tournament_service` Klasse, um den Titel den Turniere-Dienst zugreifen.

Diese neue Turnier APIs ermöglichen die folgenden Szenarien:

* Fragen Sie den Dienst, um alle vorhandenen Turniere für den aktuellen Titel finden.
* Abrufen von Details zu ein Turnier aus dem Dienst.
* Fragen Sie den Dienst, um eine Liste der Teams für ein Turnier abzurufen.
* Abrufen von Details zu den Teams für ein Turnier aus dem Dienst.
* Nachverfolgen von Änderungen mithilfe von Echtzeit-Aktivität (RTA) Abonnements Turniere und Teams.
