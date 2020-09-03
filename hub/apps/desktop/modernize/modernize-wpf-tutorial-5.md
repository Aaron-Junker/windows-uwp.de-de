---
description: In diesem Tutorial wird veranschaulicht, wie du UWP-XAML-Benutzeroberflächen hinzufügst, MSIX-Pakete erstellst und andere moderne Komponenten in deine WPF-App integrierst.
title: Verpacken und Bereitstellen mit MSIX
ms.topic: article
ms.date: 01/23/2020
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 18b89caa0de947d2b95b46c3deb11378912b6012
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161424"
---
# <a name="part-5-package-and-deploy-with-msix"></a>Teil 5: Verpacken und Bereitstellen mit MSIX

Dies ist der letzte Teil eines Tutorials, in dem das Modernisieren der WPF-Beispiel-Desktop-App Contoso Expenses veranschaulicht wird. Eine Übersicht über das Tutorial, die Voraussetzungen und Anweisungen zum Herunterladen der Beispiel-App findest du unter [Tutorial: Modernisieren einer WPF-App](modernize-wpf-tutorial.md). In diesem Artikel wird davon ausgegangen, dass du [Teil 4](modernize-wpf-tutorial-4.md) bereits abgeschlossen hast.

In [Teil 4](modernize-wpf-tutorial-4.md) hast du erfahren, dass einige WinRT-APIs (einschließlich der Benachrichtigungs-API) eine Paketidentität erfordern, bevor sie in einer App verwendet werden können. Du kannst die Paketidentität abrufen, indem du die App Contoso Expenses mit [MSIX](/windows/msix) packst. Dies ist das in Windows 10 eingeführte Paketformat zum Packen und Bereitstellen von Windows-Anwendungen. MSIX bietet Entwicklern und IT-Experten Vorteile wie die folgenden:

- Optimierte Nutzung von Netzwerk und Speicherplatz
- Vollständige Deinstallation dank eines einfachen Containers, in dem die App ausgeführt wird. Auf dem System bleiben keine Registrierungsschlüssel und temporären Dateien zurück.
- Entkoppelung der Betriebssystemupdates von den Anwendungsupdates und -anpassungen
- Vereinfachte Installation, Aktualisierung und Deinstallation

In diesem Teil des Tutorials erfährst du, wie du die App Contoso Expenses in einem MSIX-Paket packst.

## <a name="package-the-application"></a>Packen der Anwendung

Visual Studio 2019 bietet mit dem Paketerstellungsprojekt für Windows-Anwendungen eine einfache Möglichkeit zum Packen einer Desktopanwendung. 

1. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe **ContosoExpenses**, und wähle **Hinzufügen > Neues Projekt** aus.

    ![Hinzufügen eines neuen Projekts](images/wpf-modernize-tutorial/AddNewProject.png)

3. Suche im Dialogfeld **Hinzufügen eines neuen Projekts** nach `packaging`, wähle unter der Kategorie „C#“ die Projektvorlage **Paketerstellungsprojekt für Windows-Anwendungen** aus, und klicke auf **Weiter**.

    ![Paketerstellungsprojekt für Windows-Anwendungen](images/wpf-modernize-tutorial/WAP.png)

4. Benenne das neue Projekt mit `ContosoExpenses.Package`, und klicke auf **Erstellen**.

5. Wähle **Windows 10, Version 1903 (10.0; Build 18362)** für **Zielversion** und **Mindestversion** aus, und klicke auf **OK**.

    Das Projekt **ContosoExpenses.Package** wird der Projektmappe **ContosoExpenses** hinzugefügt. Dieses Projekt enthält ein [Paketmanifest](/uwp/schemas/appxpackage/uapmanifestschema/schema-root), das die Anwendung beschreibt, sowie einige Standardobjekte, die für Elemente wie das Symbol im Menü „Programme“ und die Kachel auf dem Startbildschirm verwendet werden. Anders als bei einem UWP-Projekt enthält das Paketerstellungsprojekt jedoch keinen Code. Der Zweck besteht ausschließlich darin, eine vorhandene Desktop-App zu packen.

6. Klicke im Projekt **ContosoExpenses.Package** mit der rechten Maustaste auf den Knoten **Anwendungen**, und wähle **Verweis hinzufügen** aus. Dieser Knoten gibt an, welche Anwendungen aus der Projektmappe in das Paket aufgenommen werden.

6. Wähle in der Liste der Projekte **ContosoExpenses.Core** aus, und klicke auf **OK**.

7. Erweitere den Knoten **Anwendungen**, und vergewissere dich, dass das Projekt **ContosoExpense.Core** referenziert und fett hervorgehoben wird. Dies bedeutet, dass es als Ausgangspunkt für das Paket verwendet wird.

8. Klicke mit der rechten Maustaste auf das Projekt **ContosoExpenses.Package**, und wähle **Als Startprojekt festlegen** aus.

9. Drücke **F5**, um die gepackte App im Debugger zu starten.

An diesem Punkt fallen einige Änderungen auf, die darauf hinweisen, dass die App nun in gepackter Form ausgeführt wird:

- Das Symbol auf der Taskleiste oder im Startmenü ist nun das Standardobjekt, das in jedem **Paketerstellungsprojekt für Windows-Anwendungen** enthalten ist.
- Wenn du mit der rechten Maustaste auf die im Startmenü aufgelistete Anwendung **ContosoExpense.Package** klickst, werden Optionen angezeigt, die in der Regel für Apps aus dem Microsoft Store heruntergeladene reserviert sind, z. B. **App-Einstellungen**, **Bewerten und Kritik schreiben** und **Freigeben**.

    ![ContosoExpenses im Startmenü](images/wpf-modernize-tutorial/StartMenu.png)

- Wenn du die App deinstallieren möchtest, kannst du im Startmenü mit der rechten Maustaste auf **ContosoExpense.Package** klicken und **Deinstallieren** auswählen. Die App wird sofort entfernt, ohne dass etwas im System zurückbleibt.

## <a name="test-the-notification"></a>Testen der Benachrichtigung

Nachdem du die App Contoso Expenses nun mit MSIX gepackt hast, kannst du das Benachrichtigungsszenario testen, das am Ende von [Teil 4](modernize-wpf-tutorial-4.md) noch nicht funktioniert hat.

1. Wähle in der App Contoso Expenses einen Mitarbeiter in der Liste aus, und klicke dann auf die Schaltfläche **Add new expense** (Neue Ausgaben hinzufügen).
2. Fülle alle Felder im Formular aus, und klicke auf **Speichern**.
3. Vergewissere dich, dass eine Betriebssystembenachrichtigung angezeigt wird.

![Popupbenachrichtigung](images/wpf-modernize-tutorial/ToastNotification.png)