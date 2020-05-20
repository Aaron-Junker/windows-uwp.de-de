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
ms.openlocfilehash: ab67507153e0ff7065baffa92ea6ec35aee5b132
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580767"
---
# <a name="get-started-with-winui-30-for-desktop-apps"></a>Erste Schritte mit WinUI 3.0 für Desktop-Apps

WinUI 3.0 Vorschau 1 führt neue Projektvorlagen ein, die es dir ermöglicht, verwaltete C#-/.NET-Desktop-Apps und native C++/Win32-Desktop-Apps mit einer vollständig WinUI-basierten Benutzeroberfläche zu erstellen. Wenn du Apps mit diesen Projektvorlagen erstellst, wird die gesamte Benutzeroberfläche deiner Anwendung mit Hilfe von Fenstern, Steuerelementen und anderen Benutzeroberflächentypen implementiert, die von WinUI 3.0 zur Verfügung gestellt werden. 

WinUI 3.0 Vorschau 1 fügt die folgenden **WinUI in Desktop**-Projektvorlagen in Visual Studio 2019 hinzu:

* C#-Apps und -Bibliotheken für .NET 5:
  * Leere App, Gepackt (WinUI in Desktop)
  * Klassenbibliothek (WinUI in Desktop)

* C++/Win32-Apps:
  * Leere App, Gepackt (WinUI in Desktop)

Die App-Projektvorlagen generieren ein WinUI-App-Projekt und ein [Paketerstellungsprojekt für Windows-Anwendungen](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), das so konfiguriert ist, dass es die App für die Bereitstellung in ein [MSIX-Paket](https://docs.microsoft.com/windows/msix/overview) umwandelt.

## <a name="prerequisites"></a>Voraussetzungen

Um die in diesem Artikel beschriebene WinUI 3 für Desktop-Projektvorlagen zu verwenden, konfiguriere deinen Entwicklungscomputer entsprechend den folgenden Anweisungen:

1. Stelle sicher, dass auf deinem Entwicklungscomputer mindestens Windows 10 (Version 1803, Build 17134) installiert ist. WinUI 3 für Desktop-Apps erfordert mindestens die Betriebssystemversion 1803.

2. Installiere Visual Studio 2019, Version 16.7 Vorschau 1. Ausführliche Informationen findest du unter [diesen Anweisungen](index.md#configure-your-dev-environment).

3. Installiere sowohl die x64- als auch die x86-Version von .NET 5 Vorschau 4:
    * x64: [https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x64.exe)
    * x86: [https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe](https://aka.ms/dotnet/net5/preview4/Sdk/dotnet-sdk-win-x86.exe)

4. Installiere die VSIX-Erweiterung, die die WinUI 3.0 Vorschau 1-Projektvorlagen für Visual Studio 2019 enthält. Ausführliche Informationen findest du unter [diesen Anweisungen](index.md#visual-studio-project-templates).

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

    * ***Projektname* (Paket)** : Dies ist ein [Paketerstellungsprojekt für Windows-Anwendungen](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), das so konfiguriert ist, dass es die App für die Bereitstellung in ein MSIX-Paket umwandelt. Dieses Projekt enthält das [Paketmanifest](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) für deine App und ist standardmäßig das Startprojekt in deiner Projektmappe.

        ![App-Projekt](images/WinUI-csharp-packageproject.png)

7. Um deinem App-Projekt ein neues Element hinzuzufügen, klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten ***Projektname* (Desktop)** und wähle **Hinzufügen** -> **Neues Element** aus. Wähle im Dialogfeld **Neues Element hinzufügen** die Registerkarte **WinUI** aus. Wähle das Element, das du hinzufügen möchtest, und klicke dann auf **Hinzufügen**. Du kannst aus folgenden Elementtypen auswählen:

    * **Leere Seite**
    * **Leeres Fenster**
    * **Benutzerdefiniertes Steuerelement**
    * **Ressourcenverzeichnis**
    * **Ressourcendatei**
    * **Benutzersteuerelement**

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

    * ***Projektname* (Paket)** : Dies ist ein [Paketerstellungsprojekt für Windows-Anwendungen](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), das so konfiguriert ist, dass es die App für die Bereitstellung in ein MSIX-Paket umwandelt. Dieses Projekt enthält das [Paketmanifest](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) für deine App und ist standardmäßig das Startprojekt in deiner Projektmappe.

        ![Packen des Projekts](images/WinUI-cpp-packageproject.png)

7. Um deinem App-Projekt ein neues Element hinzuzufügen, klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten ***Projektname* (Desktop)** und wähle **Hinzufügen** -> **Neues Element** aus. Wähle im Dialogfeld **Neues Element hinzufügen** die Registerkarte **WinUI** aus. Wähle das Element, das du hinzufügen möchtest, und klicke dann auf **Hinzufügen**. Du kannst aus folgenden Elementtypen auswählen:

    * **Leere Seite**
    * **Leeres Fenster**
    * **Benutzerdefiniertes Steuerelement**
    * **Ressourcenverzeichnis**
    * **Ressourcendatei**
    * **Benutzersteuerelement**

    ![Neues Element](images/WinUI-cpp-newitem.png)

8. Du musst nun einen Build für deine Projektmappe erstellen und sie ausführen, um zu bestätigen, dass die App einwandfrei läuft.

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

Eine Liste bekannter Probleme und Einschränkungen in Vorschau 1 findest du unter [diesem Abschnitt](index.md#preview-1-limitations-and-known-issues).

## <a name="related-topics"></a>Zugehörige Themen

* [WinUI 3.0](index.md)