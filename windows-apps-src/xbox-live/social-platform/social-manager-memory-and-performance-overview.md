---
title: Social Manager-Speicher und Leistung
description: Beschreibt Überlegungen zum Arbeitsspeicher und Leistung, Xbox Live, social-Manager-API-Manager verwenden.
ms.assetid: 2540145e-b8e2-4ab5-9390-65e2c9b19792
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, social-Manager und Personen
ms.localizationpriority: medium
ms.openlocfilehash: 7c6a0a31c8fe82faa644a59147060f9323d42eb0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618455"
---
# <a name="social-manager-memory-and-performance-overview"></a>Soziale Medien-Manager-Speicher und Leistung (Übersicht)

## <a name="memory"></a>Arbeitsspeicher
Soziale Medien-Manager können sie jetzt persistenten Speicher, die im Titel Raum gespeichert werden zugewiesen hat. Eine benutzerdefinierten Speicherbelegungsfunktion für die Verwendung von social-Manager kann angegeben werden, durch den Aufruf `xbox_live_services_settings::set_memory_allocation_hooks()`. Dieser Speicher Allocator-Hook wird auch für zukünftige großen speicherzuweisungen verwendet werden kann, die die Xbox Live-API (XSAPI) verwendet.

Der schlimmste Fall für die speicherbelegung durch soziale Manager standardmäßig sollte ungefähr 4,75 mb (1). Es gibt einige zusätzliche Mehraufwand, soziale Netzwerke Manager zuordnen kann basierend auf der Echtzeitaggregation und einem HTTP-Updates. Beim Erstellen einer `xbox_social_user_group` aus der Liste, jeden hinzugefügten Member wird eine zusätzliche 2,43 kb dauern. Wenn ein Benutzer, entweder über hinzugefügt wird die `create_social_social_user_group_from_list`, `update_social_user_group`, oder der Benutzer, Hinzufügen eines Freundes außerhalb der Titel, dadurch möglicherweise eine Realloc zusammenhängenden Speicherbereich zu finden.

(1) die 4,75 mb stammt = 1000 `xbox_social_user` 2,43 KB * 2. Die 2 ist, dass, soziale Netzwerke Manager einen doppelten Arbeitsspeicherpuffer verfolgt.

## <a name="additional-user-limits"></a>Für zusätzliche Benutzer
Soziale Medien-Manager weist derzeit eine Beschränkung von 100 hinzugefügte Benutzer in das Diagramm. Dies ist aufgrund von zwei Problemen:

1. Die maximale Anzahl von RTA-Abonnements, die ein Benutzer verwenden kann, ist 1100. Verfügt ein lokaler Benutzer auf 1000 Freunde, verbleibt, die nur 100 für weitere Abonnements.
2. Der maximale Umfang der Batchgröße von Benutzern, die an PeopleHub gesendet werden kann, ist derzeit bei 100.

Soziale Medien-Manager kommuniziert diese lässt nicht zu einer social Benutzergruppe aus mehr als 100 Benutzer enthalten. Es ist eine globale Beschränkung von 100 zusätzlichen Benutzern insgesamt, die im System, jederzeit über werden können `create_social_user_group_from_list`.

## <a name="processing-time"></a>Verarbeitungszeit zusammenstellen
Soziale Medien-Manager wird im schlimmsten Fall 1100-Benutzer haben. Die Leistungsmerkmale des social-Managers auf eine Xbox One hat einen schlimmsten Fall 0.3 MS auf 0,5 ms für `do_work` je nach Anzahl der soziogrammen erstellt. Der Normalfall ist 0,01 ms für die keine Gruppen erstellt und bis zu 0,2 ms für eine Gruppe mit 1.000 Benutzern darin.
