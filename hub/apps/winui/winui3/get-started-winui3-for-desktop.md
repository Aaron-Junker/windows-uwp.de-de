---
description: Diese Anleitung zeigt dir, wie du mit der Erstellung von .NET- und C++/Win32-Desktop Apps mit einer WinUI 3-Benutzeroberfläche beginnen kannst.
title: Erste Schritte mit WinUI 3 für Desktop-Apps
ms.date: 05/19/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 2b946047602013704b27fb5c5565155d38dbb7f8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154824"
---
# <a name="get-started-with-winui-3-for-desktop-apps"></a>Erste Schritte mit WinUI 3 für Desktop-Apps

WinUI 3 Vorschau 2 führt neue Projektvorlagen ein, die es dir ermöglicht, verwaltete C#-/.NET Core-Desktop-Apps und native C++/Win32-Desktop-Apps mit einer vollständig WinUI-basierten Benutzeroberfläche zu erstellen. Wenn Sie Apps mit diesen Projektvorlagen erstellen, wird die gesamte Benutzeroberfläche Ihrer Anwendung mithilfe von Fenstern, Steuerelementen und anderen Benutzeroberflächentypen implementiert, die von WinUI 3 zur Verfügung gestellt werden. Eine vollständige Liste der Projektvorlagen finden Sie in [diesem Abschnitt](index.md#project-templates-for-winui-3).

## <a name="prerequisites"></a>Voraussetzungen

Um die in diesem Artikel beschriebenen WinUI 3 für Desktop-Projektvorlagen zu verwenden, konfigurieren Sie Ihren Entwicklungscomputer, und installieren Sie WinUI 3 Vorschau 2 entsprechend [diesen Anweisungen](index.md#install-winui-3-preview-2).

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>Erstellen einer WinUI 3-Desktop-App für C# und .NET 5

1. Wähle in Visual Studio 2019 **Datei** -> **Neu** -> **Projekt** aus.

2. Wähle in den Dropdownfiltern für das Projekt **C#** , **Windows** und **WinUI** aus.

3. Wähle den Projekttyp **Blank App, Packaged (WinUI in Desktop)** [Leere App, Gepackt (WinUI in Desktop)] aus, und klicke auf **Weiter**.

    ![Projektvorlage „Leere App“](images/WinUI-csharp-newproject.png)

4. Gib einen Projektnamen ein, wähle alle anderen Optionen wie gewünscht aus, und klicke auf **Erstellen**.

5. Lege im folgenden Dialogfeld die **Zielversion** auf Windows 10, Version 1903 (Build 18362) und **Mindestversion** auf Windows 10, Version 1803 (Build 17134) fest, und klicke auf **OK**.

    ![Ziel- und Mindestversion](images/WinUI-min-target-version.png)

6. An diesem Punkt generiert Visual Studio zwei Projekte:

    * ***Projektname* (Desktop)** : Dieses Projekt enthält den Code deiner App. Die Codedatei **App.xaml.cs** definiert eine `Application`-Klasse, die deine App-Instanz darstellt. Die Codedatei **MainWindow.xaml.cs** definiert eine `MainWindow`-Klasse, die das von deiner App angezeigte Hauptfenster darstellt. Diese Klassen leiten sich von Typen im Namespace **Microsoft.UI.Xaml** ab, der von WinUI bereitgestellt wird.

        ![App-Projekt](images/WinUI-csharp-appproject.png)

    * ***Projektname* (Paket)** : Dies ist ein [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), das so konfiguriert ist, dass es die App in ein [MSIX-Paket](/windows/msix/overview) umwandelt. Dies bietet eine moderne Bereitstellungserfahrung, die Möglichkeit zur Integration in Windows 10-Features mittels Paketerweiterungen und vieles mehr. Dieses Projekt enthält das [Paketmanifest](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) für deine App und ist standardmäßig das Startprojekt in deiner Projektmappe.

        ![App-Projekt](images/WinUI-csharp-packageproject.png)

7. Um deinem App-Projekt ein neues Element hinzuzufügen, klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten ***Projektname* (Desktop)** und wähle **Hinzufügen** -> **Neues Element** aus. Wähle im Dialogfeld **Neues Element hinzufügen** die Registerkarte **WinUI** aus. Wähle das Element, das du hinzufügen möchtest, und klicke dann auf **Hinzufügen**. Weitere Informationen zu den verfügbaren Elementen finden Sie in [diesem Abschnitt](index.md#item-templates-for-winui-3).

    ![Neues Element](images/WinUI-csharp-newitem.png)

8. Du musst nun einen Build für deine Projektmappe erstellen und sie ausführen, um zu bestätigen, dass die App einwandfrei läuft.

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>Erstellen einer WinUI 3-Desktop-App für C++/Win32

1. Wähle in Visual Studio 2019 **Datei** -> **Neu** -> **Projekt** aus.

2. Wähle in den Dropdownfiltern für das Projekt **C#** , **Windows** und **WinUI** aus.

3. Wähle den Projekttyp **Blank App, Packaged (WinUI in Desktop)** [Leere App, Gepackt (WinUI in Desktop)] aus, und klicke auf **Weiter**.

    ![Projektvorlage „Leere App“](images/WinUI-cpp-newproject.png)

4. Gib einen Projektnamen ein, wähle alle anderen Optionen wie gewünscht aus, und klicke auf **Erstellen**.

5. Lege im folgenden Dialogfeld die **Zielversion** auf Windows 10, Version 1903 (Build 18362) und **Mindestversion** auf Windows 10, Version 1803 (Build 17134) fest, und klicke auf **OK**.

    ![Ziel- und Mindestversion](images/WinUI-min-target-version.png)

6. An diesem Punkt generiert Visual Studio zwei Projekte:

    * ***Projektname* (Desktop)** : Dieses Projekt enthält den Code deiner App. Die Codedatei **App.xaml** und verschiedene **App**-Codedateien definieren eine `Application`-Klasse, die deine App-Instanz darstellt. Die Codedatei **MainWindow.xaml** und verschiedene **MainWindow**-Codedateien definieren eine `MainWindow`-Klasse, die das von deiner App angezeigte Hauptfenster darstellt. Diese Klassen leiten sich von Typen im Namespace **Microsoft.UI.Xaml** ab, der von WinUI bereitgestellt wird.

        ![App-Projekt](images/WinUI-cpp-appproject.png)

    * ***Projektname* (Paket)** : Dies ist ein [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), das so konfiguriert ist, dass es die App in ein [MSIX-Paket](/windows/msix/overview) umwandelt. Dies bietet eine moderne Bereitstellungserfahrung, die Möglichkeit zur Integration in Windows 10-Features mittels Paketerweiterungen und vieles mehr. Dieses Projekt enthält das [Paketmanifest](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) für deine App und ist standardmäßig das Startprojekt in deiner Projektmappe.

        ![Packen des Projekts](images/WinUI-cpp-packageproject.png)

7. Um deinem App-Projekt ein neues Element hinzuzufügen, klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten ***Projektname* (Desktop)** und wähle **Hinzufügen** -> **Neues Element** aus. Wähle im Dialogfeld **Neues Element hinzufügen** die Registerkarte **WinUI** aus. Wähle das Element, das du hinzufügen möchtest, und klicke dann auf **Hinzufügen**. Weitere Informationen zu den verfügbaren Elementen finden Sie in [diesem Abschnitt](index.md#item-templates-for-winui-3).

    ![Neues Element](images/WinUI-cpp-newitem.png)

8. Du musst nun einen Build für deine Projektmappe erstellen und sie ausführen, um zu bestätigen, dass die App einwandfrei läuft.

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

Eine Liste bekannter Probleme und Einschränkungen finden Sie in [diesem Abschnitt](index.md#preview-2-limitations-and-known-issues).

## <a name="related-topics"></a>Zugehörige Themen

* [Windows-UI-Bibliothek 3](index.md)