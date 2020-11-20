---
description: Diese Anleitung zeigt dir, wie du mit der Erstellung von .NET- und C++/Win32-Desktop Apps mit einer WinUI 3-Benutzeroberfläche beginnen kannst.
title: Erste Schritte mit WinUI 3 für Desktop-Apps
ms.date: 11/17/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 067e6a6798fbfc2633c3e356be64ecae0403cc6d
ms.sourcegitcommit: b69edc6d73370923f31df61c7e42b53de6c928ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870920"
---
# <a name="get-started-with-winui-3-for-desktop-apps"></a>Erste Schritte mit WinUI 3 für Desktop-Apps

WinUI 3 Vorschau 3 enthält neue Projektvorlagen, die es dir ermöglicht, verwaltete C#-/.NET Core-Desktop-Apps und native C++/Win32-Desktop-Apps mit einer vollständig WinUI-basierten Benutzeroberfläche zu erstellen. Wenn Sie Apps mit diesen Projektvorlagen erstellen, wird die gesamte Benutzeroberfläche Ihrer Anwendung mithilfe von Fenstern, Steuerelementen und anderen Benutzeroberflächentypen implementiert, die von WinUI 3 zur Verfügung gestellt werden. Eine vollständige Liste der Projektvorlagen finden Sie in [diesem Abschnitt](index.md#project-templates-for-winui-3).

## <a name="prerequisites"></a>Voraussetzungen

Um die in diesem Artikel beschriebenen WinUI 3 für Desktop-Projektvorlagen zu verwenden, konfigurieren Sie Ihren Entwicklungscomputer, und installieren Sie WinUI 3 Vorschau 3 entsprechend [diesen Anweisungen](index.md#install-winui-3-preview-3).

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>Erstellen einer WinUI 3-Desktop-App für C# und .NET 5

1. Wähle in Visual Studio 2019 **Datei** -> **Neu** -> **Projekt** aus.

2. Wähle in den Dropdownfiltern für das Projekt **C#** , **Windows** und **WinUI** aus.

3. Wähle den Projekttyp **Blank App, Packaged (WinUI in Desktop)** [Leere App, Gepackt (WinUI in Desktop)] aus, und klicke auf **Weiter**.

    ![Screenshot des Assistenten zum Erstellen eines neuen Projekts mit der hervorgehobenen Option „Leere App, Gepackt (WinUI in Desktop)“.](images/WinUI-csharp-newproject.png)

4. Gib einen Projektnamen ein, wähle alle anderen Optionen wie gewünscht aus, und klicke auf **Erstellen**.

5. Lege im folgenden Dialogfeld die **Zielversion** auf Windows 10, Version 1903 (Build 18362) und **Mindestversion** auf Windows 10, Version 1803 (Build 17134) fest, und klicke auf **OK**.

    ![Ziel- und Mindestversion](images/WinUI-min-target-version.png)

6. An diesem Punkt generiert Visual Studio zwei Projekte:

    * **_Projektname_ (Desktop)** : Dieses Projekt enthält den Code deiner App. Die Datei **App.xaml** und die Code-Behind-Datei **App.xaml.cs** definieren eine `Application`-Klasse, die Ihre App-Instanz darstellt. Die Datei **MainWindow.xaml** und die Code-Behind-Datei **MainWindow.xaml.cs** definieren eine `MainWindow`-Klasse, die das von Ihrer App angezeigte Hauptfenster darstellt. Diese Klassen leiten sich von Typen im Namespace **Microsoft.UI.Xaml** ab, der von WinUI bereitgestellt wird.

        ![Screenshot von Visual Studio mit dem Projektmappen-Explorer-Bereich und den Inhalten der Datei „Main Windows X A M L Dot C S“.](images/WinUI-csharp-appproject.png)

    * **_Projektname_ (Paket)** : Dies ist ein [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), das so konfiguriert ist, dass es die App in ein [MSIX-Paket](/windows/msix/overview) umwandelt. Dies bietet eine moderne Bereitstellungserfahrung, die Möglichkeit zur Integration in Windows 10-Features mittels Paketerweiterungen und vieles mehr. Dieses Projekt enthält das [Paketmanifest](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) für deine App und ist standardmäßig das Startprojekt in deiner Projektmappe.

        ![Screenshot von Visual Studio mit dem Projektmappen-Explorer-Bereich und den Inhalten der Datei des Package App x-Manifests.](images/WinUI-csharp-packageproject.png)

7. Um deinem App-Projekt ein neues Element hinzuzufügen, klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten **_Projektname_ (Desktop)** und wähle **Hinzufügen** -> **Neues Element** aus. Wähle im Dialogfeld **Neues Element hinzufügen** die Registerkarte **WinUI** aus. Wähle das Element, das du hinzufügen möchtest, und klicke dann auf **Hinzufügen**. Weitere Informationen zu den verfügbaren Elementen finden Sie in [diesem Abschnitt](index.md#item-templates-for-winui-3).

    ![Screenshot des Dialogfelds „Neues Element hinzufügen“, in dem „Installiert“ > „Visuelle C-Sharp-Elemente“ > „Win UI“ ausgewählt und die Option „Leere Seite“ hervorgehoben ist.](images/WinUI-csharp-newitem.png)

8. Du musst nun einen Build für deine Projektmappe erstellen und sie ausführen, um zu bestätigen, dass die App einwandfrei läuft.

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>Erstellen einer WinUI 3-Desktop-App für C++/Win32

1. Wähle in Visual Studio 2019 **Datei** -> **Neu** -> **Projekt** aus.

2. Wähle in den Dropdownfiltern für das Projekt **C#** , **Windows** und **WinUI** aus.

3. Wähle den Projekttyp **Blank App, Packaged (WinUI in Desktop)** [Leere App, Gepackt (WinUI in Desktop)] aus, und klicke auf **Weiter**.

    ![Ein weiterer Screenshot des Assistenten zum Erstellen eines neuen Projekts mit der hervorgehobenen Option „Leere App, Gepackt (WinUI in Desktop)“.](images/WinUI-cpp-newproject.png)

4. Gib einen Projektnamen ein, wähle alle anderen Optionen wie gewünscht aus, und klicke auf **Erstellen**.

5. Lege im folgenden Dialogfeld die **Zielversion** auf Windows 10, Version 1903 (Build 18362) und **Mindestversion** auf Windows 10, Version 1803 (Build 17134) fest, und klicke auf **OK**.

    ![Ziel- und Mindestversion](images/WinUI-min-target-version.png)

6. An diesem Punkt generiert Visual Studio zwei Projekte:

    * **_Projektname_ (Desktop)** : Dieses Projekt enthält den Code deiner App. Die Codedatei **App.xaml** und verschiedene **App**-Codedateien definieren eine `Application`-Klasse, die deine App-Instanz darstellt. Die Codedatei **MainWindow.xaml** und verschiedene **MainWindow**-Codedateien definieren eine `MainWindow`-Klasse, die das von deiner App angezeigte Hauptfenster darstellt. Diese Klassen leiten sich von Typen im Namespace **Microsoft.UI.Xaml** ab, der von WinUI bereitgestellt wird.

        ![Screenshot von Visual Studio mit dem Projektmappen-Explorer-Bereich und den Inhalten der Datei „Main Windows X A M L“.](images/WinUI-cpp-appproject.png)

    * **_Projektname_ (Paket)** : Dies ist ein [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), das so konfiguriert ist, dass es die App in ein [MSIX-Paket](/windows/msix/overview) umwandelt. Dies bietet eine moderne Bereitstellungserfahrung, die Möglichkeit zur Integration in Windows 10-Features mittels Paketerweiterungen und vieles mehr. Dieses Projekt enthält das [Paketmanifest](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) für deine App und ist standardmäßig das Startprojekt in deiner Projektmappe.

        ![Ein weiterer Screenshot von Visual Studio mit dem Projektmappen-Explorer-Bereich und den Inhalten der Datei des Package App x-Manifests.](images/WinUI-cpp-packageproject.png)

7. Um deinem App-Projekt ein neues Element hinzuzufügen, klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten **_Projektname_ (Desktop)** und wähle **Hinzufügen** -> **Neues Element** aus. Wähle im Dialogfeld **Neues Element hinzufügen** die Registerkarte **WinUI** aus. Wähle das Element, das du hinzufügen möchtest, und klicke dann auf **Hinzufügen**. Weitere Informationen zu den verfügbaren Elementen finden Sie in [diesem Abschnitt](index.md#item-templates-for-winui-3).

    ![Neues Element](images/WinUI-cpp-newitem.png)

8. Du musst nun einen Build für deine Projektmappe erstellen und sie ausführen, um zu bestätigen, dass die App einwandfrei läuft.

   > [!NOTE]
   > Nur das gepackte Projekt wird gestartet, stellen Sie also sicher, dass eines als Startprojekt festgelegt ist.

## <a name="localizing-your-winui-desktop-app"></a>Lokalisieren Ihrer WinUI-Desktop-App

Um mehrere Sprachen in einer WinUI-Desktop-App zu unterstützen und die ordnungsgemäße Lokalisierung Ihres gepackten Projekts sicherzustellen, fügen Sie dem Projekt die entsprechenden Ressourcen hinzu (weitere Informationen finden Sie unter [App-Ressourcen und das Ressourcenverwaltungssystem](/windows/uwp/app-resources/)), und deklarieren Sie jede unterstützte Sprache in der `package.appxmanifest`-Datei Ihres Projekts. Wenn Sie das Projekt erstellen, werden die angegebenen Sprachen dem generierten App-Manifest (`AppxManifest.xml`) hinzugefügt und die entsprechenden Ressourcen verwendet.

1. Öffnen Sie die `package.appxmanifest`-Datei des Wrapperprojekts (.wapproj) in einem Texteditor, und suchen Sie den folgenden Abschnitt:

    ```xml
    <Resources>
        <Resource Language="x-generate"/>
    </Resources>
    ```

2. Ersetzen Sie das `<Resource Language="x-generate">`-Element für jede Ihrer unterstützten Sprachen durch `<Resource />`-Elemente. Das folgende Markup gibt beispielsweise an, dass lokalisierte Ressourcen für „en-US“ und „es-ES“ verfügbar sind:

    ```xml
    <Resources>
        <Resource Language="en-US"/>
        <Resource Language="es-ES"/>
    </Resources>
    ```

## <a name="known-issues-and-limitations"></a>Bekannte Probleme und Einschränkungen

Eine Liste bekannter Probleme und Einschränkungen finden Sie in [diesem Abschnitt](index.md#preview-3-limitations-and-known-issues).

## <a name="related-topics"></a>Zugehörige Themen

* [Windows-UI-Bibliothek 3](index.md)