---
description: Dieses Tutorial veranschaulicht das Hinzufügen von UWP XAML-Benutzeroberflächen, MSIX-Pakete erstellen und andere moderne Komponenten in Ihrer WPF-Anwendung integrieren.
title: Packen und Bereitstellen mit MSIX
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, Uwp, Windows Forms, Wpf, XAML-Inseln
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: d11ef296b690297d33ebd5d366c2594f70b6d10b
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420099"
---
# <a name="part-5-package-and-deploy-with-msix"></a>Teil 5: Packen und Bereitstellen mit MSIX

Dies ist der letzte Teil eines Tutorials, die zeigt, wie Sie eine Beispiel WPF-desktop-app mit dem Namen Contoso-Ausgaben zu modernisieren. Eine Übersicht über das Tutorial, Voraussetzungen und Anweisungen zum Herunterladen der Beispiel-app, finden Sie unter [Lernprogramm: Modernisieren von WPF-app](modernize-wpf-tutorial.md). In diesem Artikel wird vorausgesetzt, Sie haben bereits abgeschlossen [Teil 4](modernize-wpf-tutorial-4.md).

In [Teil 4](modernize-wpf-tutorial-4.md) haben Sie gelernt, dass einige WinRT-APIs, die Benachrichtigungen-API, einschließlich Paketidentität erfordern, bevor sie in einer app verwendet werden können. Sie erhalten Paketidentität Packen mit Contoso-Ausgaben [MSIX](https://docs.microsoft.com/windows/msix), das verpackungsformat, eingeführt in Windows 10 zum Verpacken und Bereitstellen von Windows-Anwendungen. MSIX bietet Vorteile gegenüber der in der Tabelle sowohl für Entwickler und IT-Spezialisten, einschließlich:

- Optimiert die Netzwerk-Nutzung und Speicherplatz.
- Bereinigen abgeschlossen zu deinstallieren, Dank ein einfacher Container, in dem die app ausgeführt wird. Keine Registrierungsschlüssel und die temporären Dateien verbleiben auf dem System.
- Entkoppelt Betriebssystemupdates von Anwendungsupdates und Anpassungen.
- Vereinfacht die Installation, aktualisieren und deinstallieren Sie den Prozessserver. 

In diesem Teil des Tutorials erfahren Sie, wie die Contoso-Ausgaben-app in einem MSIX-Paket verpackt werden.

## <a name="package-the-application"></a>Packen der Anwendung

2019 für Visual Studio bietet eine einfache Möglichkeit zum Verpacken von einer Desktopanwendung mit dem Paketerstellungsprojekt für Windows-Anwendung. 

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die **ContosoExpenses** Lösung, und wählen Sie **hinzufügen -> Neues Projekt**.

    ![Neues Projekt hinzufügen](images/wpf-modernize-tutorial/AddNewProject.png)

3. In der **Hinzufügen eines neuen Projekts** (Dialogfeld), und suchen Sie `packaging`, wählen Sie die **Paketerstellungsprojekt für Windows-Anwendungen** -Projektvorlage in das C# Ereigniskategorie, und klicken Sie auf **weiter** .

    ![Paketerstellungsprojekt für Windows-Anwendungen](images/wpf-modernize-tutorial/WAP.png)

4. Nennen Sie das neue Projekt `ContosoExpenses.Package` , und klicken Sie auf **erstellen**.

5. Wählen Sie **Windows 10, Version 1903 (10.0; Erstellen Sie 18362)** für beide die **Zielversion** und **Mindestversion** , und klicken Sie auf **OK**.

    Die **ContosoExpenses.Package** Projekt wird hinzugefügt, um die **ContosoExpenses** Lösung. Dieses Projekt enthält eine [Paketmanifest](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root), das beschreibt, die Anwendung, und einige Standard-Objekte, die für Elemente wie das Symbol in das Menü "Programme" und die Kachel auf dem Startbildschirm verwendet werden. Im Gegensatz zu einer UWP-Projekt enthalten nicht das Packaging-Projekt jedoch Code. Dient zum Packen einer vorhandenen desktop-app.

6. In der **ContosoExpenses.Package** Projekt der rechten Maustaste auf die **Anwendungen** Knoten, und wählen Sie **Verweis hinzufügen**. Dieser Knoten gibt an, welche Anwendungen in der Projektmappe im Paket enthalten sein werden.

7. Wählen Sie in der Liste der Projekte, **ContosoExpenses.Core** , und klicken Sie auf **OK**.

8. Erweitern Sie die **Anwendungen** Knoten und überprüfen Sie, die die **ContosoExpense.Core** Projekt verwiesen wird und in Fettschrift hervorgehoben. Dies bedeutet, dass es als Ausgangspunkt für das Paket verwendet wird.

9. Mit der rechten Maustaste die **ContosoExpenses.Package** Projekt, und wählen **als Startprojekt festlegen**.

10. Drücken Sie **F5** die Pakete-app im Debugger zu starten.

An diesem Punkt können Sie einige Änderungen bemerken, die angeben, dass die app ist nun ebenfalls enthalten:

- Das Symbol auf der Taskleiste oder im Startmenü ist jetzt das Standard-Objekt, der Bestandteil jedes **Paketerstellungsprojekt für Windows-Anwendungen**.
- Wenn Sie mit der rechten Maustaste die **ContosoExpense.Package** Anwendung im Startmenü, sehen Sie Optionen, die in der Regel für die apps aus dem Microsoft Store, z. B. reserviert sind **Anwendungseinstellungen**, **Bewerten und überprüfen Sie** und **Freigabe**.

    ![ContosoExpenses im Menü "Start"](images/wpf-modernize-tutorial/StartMenu.png)

- Wenn Sie die app deinstallieren möchten, Sie können mit der rechten Maustaste **ContosoExpense.Package** im Startmenü, und wählen Sie **Deinstallieren**. Die app wird sofort entfernt werden ohne jede Überbleibsel auf dem System verlassen zu müssen.

## <a name="test-the-notification"></a>Zum Testen der Benachrichtigung

Nun, da Sie die Contoso-Ausgaben-app mit MSIX verpackt haben, können Sie das Notification-Szenario, das am Ende der Arbeit war nicht testen [Teil 4](modernize-wpf-tutorial-4.md).

1. In der Contoso-Ausgaben-app, wählen Sie einen Mitarbeiter aus der Liste aus, und klicken Sie dann auf die **Hinzufügen von neuen Ausgaben** Schaltfläche. 
2. Füllen Sie alle Felder in der Form, und drücken Sie **speichern**.
3. Vergewissern Sie sich, dass Sie eine Betriebssystem-Benachrichtigung angezeigt.

![Toast-Benachrichtigung](images/wpf-modernize-tutorial/ToastNotification.png)
