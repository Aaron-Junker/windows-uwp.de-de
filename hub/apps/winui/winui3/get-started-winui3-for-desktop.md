---
description: Diese Anleitung zeigt dir, wie du mit der Erstellung von .NET- und C++/Win32-Desktop Apps mit einer WinUI 3-Benutzeroberfläche beginnen kannst.
title: Erste Schritte mit WinUI 3 für Desktop-Apps
ms.date: 03/19/2021
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 8f8462a3645aff1113918f92bf53adb672ae3a61
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730564"
---
# <a name="get-started-with-winui-3-for-desktop-apps"></a>Erste Schritte mit WinUI 3 für Desktop-Apps

WinUI 3 – Project Reunion 0.5 enthält neue Projektvorlagen, die es Ihnen ermöglichen, verwaltete C#-/.NET 5-Apps und native C++/Win32-Desktop-Apps mit einer vollständig WinUI-basierten Benutzeroberfläche zu erstellen. Wenn Sie Apps mit diesen Projektvorlagen erstellen, wird die gesamte Benutzeroberfläche Ihrer Anwendung mithilfe von Fenstern, Steuerelementen und anderen Benutzeroberflächentypen implementiert, die von WinUI 3 zur Verfügung gestellt werden. Eine vollständige Liste der Projektvorlagen finden Sie unter [Projektvorlagen für WinUI 3](winui-project-templates-in-visual-studio.md#project-templates-for-winui-3).

WinUI 3 wird als Teil des Project Reunion-Pakets ausgeliefert. Weitere Informationen zu Project Reunion finden Sie unter [Erstellen von Windows-Desktop-Apps mit Project Reunion 0.5 (März 2021)](../../project-reunion/index.md).

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie die in diesem Artikel beschriebenen WinUI 3 für Desktop-Projektvorlagen verwenden möchten, müssen Sie Ihren Entwicklungscomputer entsprechend konfigurieren und die Project Reunion 0.5-Visual Studio-Erweiterung installieren. Weitere Informationen finden Sie unter [Einrichten der Entwicklungsumgebung](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment).

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>Erstellen einer WinUI 3-Desktop-App für C# und .NET 5

1. Wähle in Visual Studio 2019 **Datei** -> **Neu** -> **Projekt** aus.

2. Wähle in den Dropdownfiltern für das Projekt **C#** , **Windows** und **WinUI** aus.

3. Wählen Sie den Projekttyp **Leere App, Gepackt (WinUI 3 in Desktop)** aus, und klicken Sie auf **Weiter**.

    ![Screenshot des Assistenten zum Erstellen eines neuen Projekts mit der hervorgehobenen Option „Leere App, Gepackt (WinUI in Desktop)“.](images/WinUI3-csharp-newproject.png)

4. Gib einen Projektnamen ein, wähle alle anderen Optionen wie gewünscht aus, und klicke auf **Erstellen**.

5. Legen Sie im folgenden Dialogfeld die **Zielversion** auf „Windows 10, Version 2004 (Build 19041)“ und die **Mindestversion** auf „Windows 10, Version 1809 (Build 17763)“ fest, und klicken Sie anschließend auf **OK**.

    ![Ziel- und Mindestversion](images/WinUI3-minversion.png)

6. An diesem Punkt generiert Visual Studio zwei Projekte:

    * **_Projektname_ (Desktop)** : Dieses Projekt enthält den Code deiner App. Die Datei **App.xaml** und die Code-Behind-Datei **App.xaml.cs** definieren eine `Application`-Klasse, die Ihre App-Instanz darstellt. Die Datei **MainWindow.xaml** und die Code-Behind-Datei **MainWindow.xaml.cs** definieren eine `MainWindow`-Klasse, die das von Ihrer App angezeigte Hauptfenster darstellt. Diese Klassen leiten sich von Typen im Namespace **Microsoft.UI.Xaml** ab, der von WinUI bereitgestellt wird.

        ![Screenshot von Visual Studio mit dem Projektmappen-Explorer-Bereich und den Inhalten der Datei „Main Windows X A M L Dot C S“.](images/WinUI-csharp-appproject.png)

    * **_Projektname_ (Paket)** : Dies ist ein [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), das so konfiguriert ist, dass es die App in ein [MSIX-Paket](/windows/msix/overview) umwandelt. Dies bietet eine moderne Bereitstellungserfahrung, die Möglichkeit zur Integration in Windows 10-Features mittels Paketerweiterungen und vieles mehr. Dieses Projekt enthält das [Paketmanifest](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) für deine App und ist standardmäßig das Startprojekt in deiner Projektmappe.

        ![Screenshot von Visual Studio mit dem Projektmappen-Explorer-Bereich und den Inhalten der Datei des Package App x-Manifests.](images/WinUI-csharp-packageproject.png)

7. Um deinem App-Projekt ein neues Element hinzuzufügen, klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten **_Projektname_ (Desktop)** und wähle **Hinzufügen** -> **Neues Element** aus. Wähle im Dialogfeld **Neues Element hinzufügen** die Registerkarte **WinUI** aus. Wähle das Element, das du hinzufügen möchtest, und klicke dann auf **Hinzufügen**. Weitere Informationen zu den verfügbaren Elementen finden Sie unter [Elementvorlagen für WinUI 3](winui-project-templates-in-visual-studio.md#item-templates-for-winui-3).

    ![Screenshot des Dialogfelds „Neues Element hinzufügen“, in dem „Installiert“ > „Visuelle C-Sharp-Elemente“ > „Win UI“ ausgewählt und die Option „Leere Seite“ hervorgehoben ist.](images/winui3-addnewitem.png)

8. Du musst nun einen Build für deine Projektmappe erstellen und sie ausführen, um zu bestätigen, dass die App einwandfrei läuft.

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>Erstellen einer WinUI 3-Desktop-App für C++/Win32

1. Wähle in Visual Studio 2019 **Datei** -> **Neu** -> **Projekt** aus.

2. Wähle in den Dropdownfiltern für das Projekt **C#** , **Windows** und **WinUI** aus.

3. Wählen Sie den Projekttyp **Leere App, Gepackt (WinUI 3 in Desktop)** aus, und klicken Sie auf **Weiter**.

    ![Ein weiterer Screenshot des Assistenten zum Erstellen eines neuen Projekts mit der hervorgehobenen Option „Leere App, Gepackt (WinUI in Desktop)“.](images/WinUI3-newproject-cpp.png)

4. Gib einen Projektnamen ein, wähle alle anderen Optionen wie gewünscht aus, und klicke auf **Erstellen**.

5. Legen Sie im folgenden Dialogfeld die **Zielversion** auf „Windows 10, Version 20004 (Build 19041)“ und die **Mindestversion** auf „Windows 10, Version 1809 (Build 17763)“ fest, und klicken Sie anschließend auf **OK**.

    ![Ziel- und Mindestversion](images/WinUI3-minversion.png)

6. An diesem Punkt generiert Visual Studio zwei Projekte:

    * **_Projektname_ (Desktop)** : Dieses Projekt enthält den Code deiner App. Die Codedatei **App.xaml** und verschiedene **App**-Codedateien definieren eine `Application`-Klasse, die deine App-Instanz darstellt. Die Codedatei **MainWindow.xaml** und verschiedene **MainWindow**-Codedateien definieren eine `MainWindow`-Klasse, die das von deiner App angezeigte Hauptfenster darstellt. Diese Klassen leiten sich von Typen im Namespace **Microsoft.UI.Xaml** ab, der von WinUI bereitgestellt wird.

        ![Screenshot von Visual Studio mit dem Projektmappen-Explorer-Bereich und den Inhalten der Datei „Main Windows X A M L“.](images/WinUI-csharp-appproject.png)

    * **_Projektname_ (Paket)** : Dies ist ein [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net), das so konfiguriert ist, dass es die App in ein [MSIX-Paket](/windows/msix/overview) umwandelt. Dies bietet eine moderne Bereitstellungserfahrung, die Möglichkeit zur Integration in Windows 10-Features mittels Paketerweiterungen und vieles mehr. Dieses Projekt enthält das [Paketmanifest](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) für deine App und ist standardmäßig das Startprojekt in deiner Projektmappe.

        ![Ein weiterer Screenshot von Visual Studio mit dem Projektmappen-Explorer-Bereich und den Inhalten der Datei des Package App x-Manifests.](images/WinUI-cpp-packageproject.png)

7. Um deinem App-Projekt ein neues Element hinzuzufügen, klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten **_Projektname_ (Desktop)** und wähle **Hinzufügen** -> **Neues Element** aus. Wähle im Dialogfeld **Neues Element hinzufügen** die Registerkarte **WinUI** aus. Wähle das Element, das du hinzufügen möchtest, und klicke dann auf **Hinzufügen**. Weitere Informationen zu den verfügbaren Elementen finden Sie unter [Elementvorlagen für WinUI 3](winui-project-templates-in-visual-studio.md#item-templates-for-winui-3).

    ![Neues Element](images/winui3-addnewitem-cpp.png)

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

Weitere Informationen finden Sie unter [Windows-UI-Bibliothek 3 – Project Reunion 0.5](index.md) im Abschnitt [Einschränkungen und bekannte Probleme](index.md#limitations-and-known-issues).

## <a name="related-topics"></a>Zugehörige Themen

[Windows-UI-Bibliothek 3 – Project Reunion 0.5](index.md)
