---
description: In diesem Tutorial wird veranschaulicht, wie Sie UWP-XAML-Benutzeroberflächen hinzufügen, msix-Pakete erstellen und andere moderne Komponenten in Ihre WPF-App integrieren.
title: Verpacken und Bereitstellen mit MSIX
ms.topic: article
ms.date: 01/23/2020
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 27906d9d389c065ab1fdf7124151cd1915f850eb
ms.sourcegitcommit: 8a88a05ad89aa180d41a93152632413694f14ef8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726013"
---
# <a name="part-5-package-and-deploy-with-msix"></a>Teil 5: Verpacken und Bereitstellen mit msix

Dies ist der letzte Teil eines Tutorials, in dem veranschaulicht wird, wie eine WPF-Beispiel-Desktop-App mit dem Namen "ca Eine Übersicht über das Tutorial, Voraussetzungen und Anweisungen zum Herunterladen der Beispiel-App finden Sie unter [Tutorial: modernisieren einer WPF-App](modernize-wpf-tutorial.md). In diesem Artikel wird davon ausgegangen, dass Sie [Teil 4](modernize-wpf-tutorial-4.md)bereits abgeschlossen haben.

In [Teil 4](modernize-wpf-tutorial-4.md) haben Sie gelernt, dass einige WinRT-APIs, einschließlich der Benachrichtigungs-API, eine Paket Identität erfordern, bevor Sie in einer App verwendet werden können. Sie können die Paket Identität abrufen, indem Sie die Kosten für " [msix](https://docs.microsoft.com/windows/msix)", das in Windows 10 eingeführte Verpackungs Format zum Packen und Bereitstellen von Windows-Anwendungen, verpacken. Msix bietet Entwicklern und IT-Experten Vorteile, einschließlich der folgenden:

- Optimierte Netzwerk Auslastung und Speicherplatz.
- Vollständige Deinstallation dank eines Lightweight-Containers, in dem die app ausgeführt wird. Auf dem System sind keine Registrierungsschlüssel und temporären Dateien vorhanden.
- Entkoppelt Betriebssystemupdates von Anwendungs Updates und-Anpassungen.
- Vereinfacht die Installation, Aktualisierung und Deinstallation.

In diesem Teil des Tutorials erfahren Sie, wie Sie die app "appto-Ausgaben" in einem msix-Paket verpacken.

## <a name="package-the-application"></a>Packen der Anwendung

Visual Studio 2019 bietet eine einfache Möglichkeit zum Packen einer Desktop Anwendung mithilfe des Windows-Anwendungspaket Erstellungs Projekts. 

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf die Projekt Mappe **conabsoaufwendungen** , und wählen Sie **Hinzufügen-> Neues Projekt**aus.

    ![Neues Projekt hinzufügen](images/wpf-modernize-tutorial/AddNewProject.png)

3. Suchen Sie im Dialogfeld **Neues Projekt hinzufügen** nach `packaging`, wählen Sie die Projektvorlage Projektvorlage für **Windows-Anwendungspakete** in der C# Kategorie aus, und klicken Sie auf **weiter**.

    ![Windows-anwendungspaketierungs Projekt](images/wpf-modernize-tutorial/WAP.png)

4. Benennen Sie das neue Projekt `ContosoExpenses.Package`, und klicken Sie auf **Erstellen**.

5. Wählen Sie **Windows 10, Version 1903 (10,0; Build 18362)** für die **Zielversion** und die **Mindestversion** , und klicken Sie auf **OK**.

    Das Projekt **condesoaufwands. Package** wird der Projekt Mappe **condesoaufwands** hinzugefügt. Dieses Projekt enthält ein [Paket Manifest](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root), in dem die Anwendung beschrieben wird, sowie einige Standardobjekte, die für Elemente verwendet werden, z. b. das Symbol im Menü Programme und die Kachel auf dem Start Bildschirm. Anders als bei einem UWP-Projekt enthält das Paket Erstellungs Projekt jedoch keinen Code. Der Zweck besteht darin, eine vorhandene Desktop-App zu verpacken.

6. Klicken Sie im Projekt **contosoaufwendungen. Package** mit der rechten Maustaste auf den Knoten **Anwendungen** , und wählen Sie **Verweis hinzufügen**aus. Dieser Knoten gibt an, welche Anwendungen in der Lösung in das Paket aufgenommen werden.

6. Wählen Sie in der Liste der Projekte die Option **contosoaufwendungen. Core** aus, und klicken Sie auf **OK**.

7. Erweitern Sie den Knoten **Anwendungen** , und vergewissern Sie sich, dass auf das Projekt **contosospesen. Core** verwiesen wird und fett hervorgehoben ist. Dies bedeutet, dass Sie als Ausgangspunkt für das Paket verwendet wird.

8. Klicken Sie mit der rechten Maustaste auf das Projekt **condesoaufwands. Package** , und wählen Sie **als Startprojekt festlegen**aus.

9. Drücken Sie **F5** , um die APP im Debugger zu starten.

An diesem Punkt können Sie einige Änderungen bemerken, die darauf hinweisen, dass die App nun als verpackt ausgeführt wird:

- Das Symbol in der Taskleiste oder im Startmenü ist nun das Standard Objekt, das in jedem Windows- **Anwendungspaket**enthalten ist.
- Wenn Sie mit der rechten Maustaste auf die im Startmenü aufgelistete Anwendung **condesospw. Package** klicken, werden Ihnen Optionen angezeigt, die in der Regel für apps reserviert sind, die aus dem Microsoft Store heruntergeladen wurden, z.b. **App-Einstellungen**, **Rate und Überprüfung** und **Freigabe**.

    ![Conum-Ausgaben im Startmenü](images/wpf-modernize-tutorial/StartMenu.png)

- Wenn Sie die APP deinstallieren möchten, klicken Sie im Startmenü mit der rechten Maustaste auf **condesospesen. Package** , und wählen Sie **deinstallieren**aus. Die APP wird sofort entfernt, ohne dass Sie auf dem System übrig bleibt.

## <a name="test-the-notification"></a>Testen der Benachrichtigung

Nachdem Sie die APP für die kostenpflichtige Geräteverwaltung mit msix verpackt haben, können Sie das Benachrichtigungs Szenario testen, das am Ende von [Teil 4](modernize-wpf-tutorial-4.md)nicht funktionierte.

1. Wählen Sie in der APP für die kostenpflichtige app einen Mitarbeiter aus der Liste aus, und klicken Sie dann auf die Schaltfläche **neue Ausgaben hinzufügen** .
2. Vervollständigen Sie alle Felder im Formular, und klicken Sie auf **Speichern**.
3. Vergewissern Sie sich, dass eine Betriebssystem Benachrichtigung angezeigt wird.

![Popup Benachrichtigung](images/wpf-modernize-tutorial/ToastNotification.png)
