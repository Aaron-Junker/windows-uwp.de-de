---
title: Testen von Unity-Spiel in Visual Studio
description: Checkliste für die zum erfolgreichen Testen von Unity-builds in Visual Studio.
ms.date: 03/19/2018
ms.topic: get-started-article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, unity
ms.openlocfilehash: 4d5a1a5596beba2839e01ca5be6e6d2dbff0c148
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589875"
---
# <a name="test-your-unity-game-build-in-visual-studio"></a>Testen Sie Ihr Unity-spielen-Build in Visual Studio

Um die Xbox Live-Funktionalität von Ihr Unity-Spiel mit echten Daten zu testen, müssen Sie das Spiel erstellen, wie in beschrieben die **erstellen und Testen des Projekts** Teil der [konfigurieren Xbox Live in Unity Artikel](configure-xbox-live-in-unity.md). Im folgende Artikel erhalten Sie eine Prüfliste der Elemente, um sicherzustellen, erfolgreiche Tests, der Ihr Unity-Spiel in Visual Studio.

1. **Überprüfen Sie, dass Sie einen ordnungsgemäß konfigurierten Titel, die Ihr Unity-Spiel zugeordnet haben.**
    Wenn Sie Xbox Live, über den Xbox Live-Zuordnung-Assistenten aktiviert haben, sollten Sie sich mit den Grundlagen [Partner Center](https://partner.microsoft.com/dashboard). Partner Center können Sie konfigurieren die Xbox Live-Einstellungen für den Titel und muss Setup ordnungsgemäß in der Reihenfolge für Ihre Titel für die Kommunikation mit Xbox Live. Der Artikel [einen neuen Titel für die Xbox Live Creators-Programm zu erstellen und veröffentlichen Sie in der testumgebung](create-and-test-a-new-creators-title.md) führt Sie durch den Partner Center-Setupvorgang. Wenn Setup Ihres Spiels über noch die **Xbox-Konfigurations-Assistenten** in Unity können Sie mit dem Abschnitt fortfahren [Test Xbox Live-Dienstkonfiguration in Ihrem Spiel](create-and-test-a-new-creators-title.md#test-xbox-live-service-configuration-in-your-game). Klicken Sie im Partner Center unbedingt überprüfen, ob die Informationen in Ihrer Xbox Live-Konfiguration für Ihr Unity-Spiel mit der Partner Center-Konfiguration für Ihr Spiel übereinstimmt.

2. **Sicherstellen Sie, dass der Titel einer autorisierten Microsoft Account(with gamertag), die den Titel anmelden können.**
    Ohne ein autorisiertes Konto nicht werden können vollständige Anmeldung in während des Tests des Titels, noch ist es möglich, andere Xbox Live-Funktionen zu verwenden. Stellen Sie sicher, dass ein autorisierter Microsoft Account und das Gamertag lesen [autorisieren Xbox Live-Konto für Tests in Ihrer Umgebung](authorize-xbox-live-accounts.md).

3. **Stellen Sie sicher, dass Ihre Titel für das Testen veröffentlicht wurde.**
    Wenn Sie Änderungen an Ihrem Titel diese Änderungen vornehmen müssen zum Sand-Feld veröffentlicht werden, bevor sie verwendet werden können. Stellen Sie sicher, dass Sie per Push übertragen die **Test** Schaltfläche, um Ihre Änderungen an der Konfiguration zu veröffentlichen.

    ![Für testimage veröffentlichen](../images/creators_udc/creators_udc_xboxlive_config_test.png)

    Die **Test** Schaltfläche befindet sich im [Partner Center](https://partner.microsoft.com/dashboard) unter der Titelleiste Ihres Xbox Live-diensteinstellungen. Wenn Sie alle Änderungen, um Ihre Konfiguration vorgenommen, z. B. das Hinzufügen eines neuen autorisierte Testkontos mithilfe von Push übertragen werden sollen die **testen** Schaltfläche, um die neuen Konfigurationseinstellungen im Xbox Live-Dienst veröffentlichen.

4. **Stellen Sie sicher, dass Ihr PC im Entwicklermodus, und Sie ist, werden die entsprechenden Xbox Live-Sandbox verwendet.**
    Wenn für der Titel veröffentlicht wird wird Testen in einer bestimmten Umgebung, die Namen einer Sandbox veröffentlicht. Wenn es sich bei Ihrem Entwicklungs-PC verwenden diese Sandbox, die Sie nicht zum Testen von Xbox Live-Funktionen können nicht festgelegt ist. Erfahren Sie, um zu überprüfen und Ändern von PC Sandbox mit der [Xbox Live-Sandboxes Einführung](xbox-live-sandboxes-creators.md).

5. **Stellen Sie sicher, dass es sich bei der projekterstellung als x X64 erstellen, auf dem lokalen Computer, auf dem Computer zu erstellen.**

    ![Erstellen von Einstellungen](../images/unity/get-started-with-creators/vsBuildSettings.JPG)