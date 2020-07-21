---
description: Diese Anleitung zeigt Ihnen, wie Sie mit der Erstellung von UWP-Apps mit einer WinUI 3-Benutzeroberfläche beginnen können.
title: Erste Schritte mit WinUI 3 für UWP-Apps
ms.date: 07/13/2020
ms.topic: article
keywords: Windows 10, UWP, WinUI
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 0b62595850c2b3354c59d199f879d7e3b55cd3a2
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493953"
---
# <a name="get-started-with-winui-3-for-uwp-apps"></a>Erste Schritte mit WinUI 3 für UWP-Apps

WinUI 3 Vorschau 2 führt neue Projektvorlagen ein, mit denen Sie eine universelle Windows-Plattform-App (UWP) mit einer Benutzeroberfläche erstellen können, die vollständig mit WinUI erstellt wurde. Wenn Sie Apps mit diesen Projektvorlagen erstellen, wird die gesamte Benutzeroberfläche Ihrer Anwendung mithilfe von Fenstern, Steuerelementen und Stilen implementiert, die von WinUI 3 zur Verfügung gestellt werden. Eine vollständige Liste der unterstützten WinUI 3-Projektvorlagen finden Sie unter [Projektvorlagen für WinUI 3](index.md#project-templates-for-winui-3).

## <a name="prerequisites"></a>Voraussetzungen

Um die in diesem Artikel beschriebene WinUI 3 für Desktop-Projektvorlagen zu verwenden, konfigurieren Sie Ihren Entwicklungscomputer, und [installieren Sie WinUI 3 Vorschau 2](index.md#install-winui-3-preview-2).

## <a name="create-a-winui-3-app-in-uwp-for-c"></a>Erstellen einer „WinUI 3-App in UWP“ für C#

1. Erstellen Sie unter Verwendung von Visual Studio 2019 ein neues Projekt.
   - Wenn Visual Studio bereits ausgeführt wird, wählen Sie **Datei** -> **Neu** -> **Projekt** aus.

   :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-file-new-project.png" alt-text="Visual Studio 2019 – Menü „Datei -> Neu -> Projekt“":::

   - Starten Sie andernfalls Visual Studio, und wählen Sie **Neues Projekt erstellen** aus.

   :::image type="content" source="images/WinUI-and-UWP/vs2019-splash-new-project.png" alt-text="Visual Studio 2019 – Neues Projekt erstellen":::

2. Wählen Sie im Dialogfeld **Neues Projekt erstellen** **C#** , **Windows** und **WinUI** in den Dropdownfiltern des Projekts aus.

3. Wählen Sie den Projekttyp **Leere App (WinUI in UWP)** aus, und klicken Sie auf **Weiter**.

:::image type="content" source="images/WinUI-and-UWP/vs2019-create-new-project-dialog.png" alt-text="Visual Studio 2019 – Dialogfeld „Neues Projekt erstellen“":::

4. Gib einen Projektnamen ein, wähle alle anderen Optionen wie gewünscht aus, und klicke auf **Erstellen**.

:::image type="content" source="images/WinUI-and-UWP/vs2019-configure-new-project-dialog.png" alt-text="Visual Studio 2019 – Dialogfeld „Neues Projekt erstellen“":::

5. Lege im folgenden Dialogfeld die **Zielversion** auf Windows 10, Version 1903 (Build 18362) und **Mindestversion** auf Windows 10, Version 1803 (Build 17134) fest, und klicke auf **OK**.

:::image type="content" source="images/WinUI-min-target-version.png" alt-text="Dialogfeld „Ziel- und Mindestversion“":::

6. Visual Studio generiert das Projekt **WinUI in UWP** mit den folgenden Objekten:

    - ***Projektname* (Universelles Windows)** : Enthält Ihren Anwendungscode. Dies ist das Standardstartprojekt für Ihre Projektmappe.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-project.png" alt-text="Visual Studio 2019 – Dialogfeld „Neues Projekt erstellen“":::

    - **Package.appxmanifest**: Enthält Informationen, die das System benötigt, um Ihre App bereitzustellen, anzuzeigen oder zu aktualisieren. Weitere Informationen finden Sie unter [App-Paketmanifest](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest).

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-package-manifest.png" alt-text="Visual Studio 2019 – App-Paketmanifest":::

    - **App.xaml/App.xaml.cs**: Codedateien, die die `Application`-Klasse definieren, die ihre App-Instanz darstellt.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml.png" alt-text="Visual Studio 2019 – Datei „App.xaml“":::

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-app-xaml-cs.png" alt-text="Visual Studio 2019 – Datei „App.xaml.cs“":::

    - **MainPage.xaml/MainPage.xaml.cs**: Codedateien, die die von Ihrer App angezeigten Hauptfenster darstellen. Diese Klassen leiten sich von Typen im Namespace **Microsoft.UI.Xaml** ab, der von WinUI bereitgestellt wird.

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml.png" alt-text="Visual Studio 2019 – Datei „MainPage.xaml“":::

    :::image type="content" source="images/WinUI-and-UWP/vs2019-file-mainpage-xaml-cs.png" alt-text="Visual Studio 2019 – Datei „MainPage.xaml.cs“":::

7. Um Ihrem App-Projekt ein neues Element hinzuzufügen, klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten ***Projektname* (Universelles Windows)** , und wählen Sie **Hinzufügen** -> **Neues Element** aus. Wähle im Dialogfeld **Neues Element hinzufügen** die Registerkarte **WinUI** aus. Wähle das Element, das du hinzufügen möchtest, und klicke dann auf **Hinzufügen**. Weitere Informationen zu den verfügbaren Elementen finden Sie unter [Elementvorlagen für WinUI 3](index.md#item-templates-for-winui-3).

    :::image type="content" source="images/WinUI-and-UWP/vs2019-add-new-item-dialog.png" alt-text="Visual Studio 2019 – Dialogfeld „Neues Element hinzufügen“":::

8. Um festzustellen, wie Ihre App aussieht, erstellen Sie sie, stellen Sie sie bereit, und starten Sie sie.

    1. Sie können Ihre App auf dem lokalen Computer, in einem Simulator oder Emulator oder auf einem Remotegerät debuggen. Wählen Sie Ihr Zielgerät aus der Dropdownliste aus.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-menu-target-device.png" alt-text="Visual Studio 2019 – Menü mit Zielgeräten":::

    1. Drücken Sie F5, klicken Sie auf die Schaltfläche **Erstellen**, oder wählen Sie **Debuggen -> Debuggen starten** aus, um Ihre Projektmappe zu erstellen und auszuführen und zu bestätigen, dass die App fehlerfrei funktioniert.

        :::image type="content" source="images/WinUI-and-UWP/vs2019-project-running.png" alt-text="Visual Studio 2019 – Menü mit Zielgeräten":::

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

Eine Liste bekannter Probleme und Einschränkungen finden Sie in [diesem Abschnitt](index.md#preview-2-limitations-and-known-issues).

## <a name="related-topics"></a>Zugehörige Themen

- [WinUI 3](index.md)
- [Erste App erstellen](/windows/uwp/get-started/your-first-app)
