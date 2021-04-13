---
description: Diese Anleitung zeigt Ihnen, wie Sie mit der Erstellung von UWP-Apps mit einer WinUI 3-Benutzeroberfläche beginnen können.
title: Erste Schritte mit WinUI 3 für UWP-Apps (Vorschau)
ms.date: 03/19/2021
ms.topic: article
keywords: Windows 10, UWP, WinUI
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 0f6f8a17a4f3d6ca684854a64de05da87b27c44c
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730534"
---
# <a name="get-started-with-winui-3-for-uwp-apps-preview"></a>Erste Schritte mit WinUI 3 für UWP-Apps (Vorschau)

Sie können eine UWP-App (Universelle Windows-Plattform) mit einer Benutzeroberfläche erstellen können, die vollständig auf WinUI 3 basiert, indem Sie Projektvorlagen verwenden, die in [WinUI 3 – Project Reunion 0.5 Preview](release-notes/winui3-project-reunion-0.5-preview.md) enthalten sind. Wenn Sie Apps mit diesen Projektvorlagen erstellen, wird die gesamte Benutzeroberfläche Ihrer Anwendung mithilfe von Fenstern, Steuerelementen und Stilen implementiert, die von WinUI 3 zur Verfügung gestellt werden. Eine vollständige Liste der unterstützten WinUI 3-Projektvorlagen finden Sie unter [Projektvorlagen für WinUI 3](winui-project-templates-in-visual-studio.md#project-templates-for-winui-3).

WinUI 3 wird als Teil des Project Reunion-Pakets ausgeliefert. Weitere Informationen zu Project Reunion finden Sie unter [Erstellen von Windows-Desktop-Apps mit Project Reunion 0.5](../../project-reunion/index.md).

> [!NOTE] 
> Die WinUI 3-Unterstützung zum Entwickeln von UWP-Apps befindet sich derzeit in der Vorschau und ist nicht für die Produktion bereit. Sie können WinUI 3-UWP-Apps nicht an den Microsoft Store senden.

## <a name="prerequisites"></a>Voraussetzungen

Um die WinUI 3 für UWP Preview-Projektvorlagen zu verwenden, konfigurieren Sie Ihren Entwicklungscomputer, indem Sie den Anweisungen in der Anleitung [Einrichten der Entwicklungsumgebung für Project Reunion](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment) folgen. 

> [!NOTE]
> Sie können keine WinUI 3-UWP-Apps mit dem VSIX-Paket von Project Reunion 0.5 erstellen. Sie müssen das [VSIX-Paket von Project Reunion 0.5 **Preview**](https://aka.ms/projectreunion/previewdownload) herunterladen, um die UWP Preview-Projektvorlagen zu erhalten und UWP-Apps mit WinUI 3 zu erstellen. 

## <a name="create-a-winui-3-app-in-uwp-for-c"></a>Erstellen einer „WinUI 3-App in UWP“ für C#

1. Erstellen Sie unter Verwendung von Visual Studio 2019 ein neues Projekt.
   - Wenn Visual Studio bereits ausgeführt wird, wählen Sie **Datei** -> **Neu** -> **Projekt** aus.

       :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-file-new-project.png" alt-text="Visual Studio 2019 – Menü „Datei -> Neu -> Projekt“":::

   - Starten Sie andernfalls Visual Studio, und wählen Sie **Neues Projekt erstellen** aus.

       :::image type="content" source="images/WinUI-and-UWP/vs2019-splash-new-project.png" alt-text="Visual Studio 2019 – Neues Projekt erstellen":::

2. Wählen Sie im Dialogfeld **Neues Projekt erstellen** **C#** , **Windows** und **WinUI** in den Dropdownfiltern des Projekts aus.

3. Wählen Sie den Projekttyp **Leere App (WinUI in UWP) – Vorschau** aus, und klicken Sie auf **Weiter**.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-create-new-project-dialog.png" alt-text="Visual Studio 2019 – Dialogfeld „Neues Projekt erstellen“":::

4. Gib einen Projektnamen ein, wähle alle anderen Optionen wie gewünscht aus, und klicke auf **Erstellen**.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-configure-new-project-dialog.png" alt-text="Screenshot des Dialogfelds zum Konfigurieren Ihres neuen Projekts mit hervorgehobenem Textfeld „Speicherort“ und der Option „Erstellen“.":::

5. Legen Sie im folgenden Dialogfeld die **Zielversion** auf „Windows 10, Version 1903 (Build 18362)“ und die **Mindestversion** auf „Windows 10, Version 1809 (Build 17763)“ fest, und klicken Sie anschließend auf **OK**.

    :::image type="content" source="images/WinUI-min-target-version.png" alt-text="Dialogfeld „Ziel- und Mindestversion“":::

6. Visual Studio generiert das Projekt **WinUI in UWP** mit den folgenden Objekten:

    - ***Projektname* (Universelles Windows)** : Enthält Ihren Anwendungscode. Dies ist das Standardstartprojekt für Ihre Projektmappe.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-project.png" alt-text="Screenshot des Projektmappen-Explorer-Bereichs mit hervorgehobener Projektmappe „Universelles Windows“.":::

    - **Package.appxmanifest**: Enthält Informationen, die das System benötigt, um Ihre App bereitzustellen, anzuzeigen oder zu aktualisieren. Weitere Informationen finden Sie unter [App-Paketmanifest](/uwp/schemas/appxpackage/appx-package-manifest).

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-package-manifest.png" alt-text="Visual Studio 2019 – App-Paketmanifest":::

    - **App.xaml/App.xaml.cs**: Codedateien, die die `Application`-Klasse definieren, die ihre App-Instanz darstellt.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml.png" alt-text="Visual Studio 2019 – Datei „App.xaml“":::

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml-cs.png" alt-text="Visual Studio 2019 – Datei „App.xaml.cs“":::

    - **MainPage.xaml/MainPage.xaml.cs**: Codedateien, die die von Ihrer App angezeigten Hauptfenster darstellen. Diese Klassen leiten sich von Typen im Namespace **Microsoft.UI.Xaml** ab, der von WinUI bereitgestellt wird.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml.png" alt-text="Visual Studio 2019 – Datei „MainPage.xaml“":::

        :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml-cs.png" alt-text="Visual Studio 2019 – Datei „MainPage.xaml.cs“":::

7. Um Ihrem App-Projekt ein neues Element hinzuzufügen, klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten ***Projektname* (Universelles Windows)** , und wählen Sie **Hinzufügen** -> **Neues Element** aus. Wähle im Dialogfeld **Neues Element hinzufügen** die Registerkarte **WinUI** aus. Wähle das Element, das du hinzufügen möchtest, und klicke dann auf **Hinzufügen**. Weitere Informationen zu den verfügbaren Elementen finden Sie unter [Elementvorlagen für WinUI 3](winui-project-templates-in-visual-studio.md#item-templates-for-winui-3).

    :::image type="content" source="images/WinUI-and-UWP/vs2019-add-new-item-dialog.png" alt-text="Visual Studio 2019 – Dialogfeld „Neues Element hinzufügen“":::

8. Um festzustellen, wie Ihre App aussieht, erstellen Sie sie, stellen Sie sie bereit, und starten Sie sie.

    1. Sie können Ihre App auf dem lokalen Computer, in einem Simulator oder Emulator oder auf einem Remotegerät debuggen. Wählen Sie Ihr Zielgerät aus der Dropdownliste aus.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-target-device.png" alt-text="Screenshot der Dropdownliste „Lokaler Computer“.":::

    1. Drücken Sie F5, klicken Sie auf die Schaltfläche **Erstellen**, oder wählen Sie **Debuggen -> Debuggen starten** aus, um Ihre Projektmappe zu erstellen und auszuführen und zu bestätigen, dass die App fehlerfrei funktioniert.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-project-running.png" alt-text="Screenshot der ausgeführten App mit angezeigter Schaltfläche „Hier klicken“.":::

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

Weitere Informationen finden Sie unter [Windows-UI-Bibliothek 3 – Project Reunion 0.5 Preview](release-notes/winui3-project-reunion-0.5-preview.md) im Abschnitt [Einschränkungen und bekannte Probleme](index.md#limitations-and-known-issues).

## <a name="related-topics"></a>Zugehörige Themen

- [Windows-UI-Bibliothek 3 – Project Reunion 0.5 Preview](release-notes/winui3-project-reunion-0.5-preview.md)
- [Erste App erstellen](/windows/uwp/get-started/your-first-app)
