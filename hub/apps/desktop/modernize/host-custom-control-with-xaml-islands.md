---
description: In diesem Artikel wird veranschaulicht, wie Sie ein benutzerdefiniertes UWP-Steuerelement in einer WPF-App mit XAML-Inseln hosten.
title: Hosten eines benutzerdefinierten UWP-Steuer Elements in einer WPF-App mithilfe von XAML-Inseln
ms.date: 01/10/2010
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, XAML-Inseln, benutzerdefinierte Steuerelemente, Benutzer Steuerelemente, Host Steuerelemente
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 4fb6e2d4fc13d90ec69f962e69b1ee8cb5c1361c
ms.sourcegitcommit: 85fd390b1e602707bd9342cb4b84b97ae0d8b831
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76520395"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Hosten eines benutzerdefinierten UWP-Steuer Elements in einer WPF-App mithilfe von XAML-Inseln

In diesem Artikel wird veranschaulicht, wie das [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuerelement im Windows Community Toolkit zum Hosten eines benutzerdefinierten UWP-Steuer Elements in einer WPF-App verwendet wird, die auf .net Core 3 ausgerichtet ist. Das benutzerdefinierte Steuerelement enthält mehrere UWP-Steuerelemente von erst Anbietern aus dem Windows SDK und bindet eine Eigenschaft in einem der UWP-Steuerelemente an eine Zeichenfolge in der WPF-App. In diesem Artikel wird auch veranschaulicht, wie Sie ein UWP-Steuerelement einer ersten Partei aus der [WinUI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/)hosten.

Obwohl in diesem Artikel beschrieben wird, wie Sie dies in einer WPF-APP tun, ist der Prozess bei einer Windows Forms-App ähnlich. Eine Übersicht über das Hosting von UWP-Steuerelementen in WPF und Windows Forms-apps finden Sie in [diesem Artikel](xaml-islands.md#wpf-and-windows-forms-applications).

## <a name="overview"></a>Übersicht

Um ein benutzerdefiniertes UWP-Steuerelement in einer WPF-App zu hosten, benötigen Sie die folgenden Komponenten. Dieser Artikel enthält Anweisungen zum Erstellen dieser Komponenten.

* **Das Projekt und der Quellcode für Ihre WPF-App**. Das Verwenden des [windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) -Steuer Elements zum Hosten von benutzerdefinierten UWP-Steuerelementen wird nur in WPF-und Windows Forms-Apps unterstützt, die auf .net Core Dieses Szenario wird in apps, die auf die .NET Framework abzielen, nicht unterstützt.

* **Das benutzerdefinierte UWP-Steuer**Element. Sie benötigen den Quellcode für das benutzerdefinierte UWP-Steuerelement, das Sie hosten möchten, damit Sie es mit Ihrer APP kompilieren können. In der Regel wird das benutzerdefinierte Steuerelement in einem UWP-Klassen Bibliotheksprojekt definiert, auf das Sie in derselben Projekt Mappe verweisen wie das WPF-Projekt (oder Windows Forms).

* **Ein UWP-App-Projekt, das ein xamlapplication-Objekt definiert**. Ihr WPF-Projekt (oder Windows Forms) muss Zugriff auf eine Instanz der `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` Klasse haben, die vom Windows Community Toolkit bereitgestellt wird. Dieses Objekt fungiert als Stamm-Metadatenanbieter zum Laden von Metadaten für benutzerdefinierte UWP-XAML-Typen in Assemblys im aktuellen Verzeichnis der Anwendung. Die empfohlene Vorgehensweise besteht darin, ein **leeres App-Projekt (Universal Windows)** zur gleichen Projekt Mappe wie das WPF-Projekt (oder Windows Forms) hinzuzufügen und die Standard `App` Klasse in diesem Projekt zu überarbeiten.
  > [!NOTE]
  > Die Projekt Mappe kann nur ein Projekt enthalten, das ein `XamlApplication` Objekt definiert. Alle benutzerdefinierten UWP-Steuerelemente in Ihrer APP verwenden dasselbe `XamlApplication` Objekt. Das Projekt, das das `XamlApplication` Objekt definiert, muss Verweise auf alle anderen UWP-Bibliotheken und-Projekte enthalten, die in der XAML-Insel als Host-UWP-Steuerelemente verwendet werden.

## <a name="create-a-wpf-project"></a>Erstellen eines WPF-Projekts

Befolgen Sie vor dem Einstieg diese Anweisungen, um ein WPF-Projekt zu erstellen und zum Hosten von XAML-Inseln zu konfigurieren. Wenn Sie über ein vorhandenes WPF-Projekt verfügen, können Sie diese Schritte und Codebeispiele für Ihr Projekt anpassen.

> [!NOTE]
> Wenn Sie über ein vorhandenes Projekt verfügen, das auf den .NET Framework abzielt, müssen Sie Ihr Projekt zu .net Core 3 migrieren. Weitere Informationen finden Sie in [dieser Blog Reihe](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

1. Wenn Sie dies nicht bereits getan haben, installieren Sie die neueste Version des [.net Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. Erstellen Sie in Visual Studio 2019 ein neues Projekt **WPF-App (.net Core)** .

3. Stellen Sie sicher, dass die [Paket Verweise](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) aktiviert sind:

    1. Klicken Sie in Visual Studio auf Extras **-> nuget-Paket-Manager-> Paket-Manager-Einstellungen**.
    2. Stellen Sie sicher, dass **packagereferenzierung** für das **standardmäßige Paket Verwaltungs Format**ausgewählt ist.

4. Klicken Sie mit der rechten Maustaste auf das WPF-Projekt in **Projektmappen-Explorer** und wählen Sie **nuget-Pakete verwalten**.

5. Vergewissern Sie sich, dass im Fenster **nuget-Paket-Manager** die Option **Vorabversion einschließen** ausgewählt ist.

6. Wählen Sie die Registerkarte **Durchsuchen** aus, suchen Sie nach dem Paket [Microsoft. Toolkit. WPF. UI. xamlhost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (Version v 6.0.0 oder höher), und installieren Sie das Paket. Dieses Paket enthält alles, was Sie benötigen, um das **windowsxamlhost** -Steuerelement zum Hosten eines UWP-Steuer Elements zu verwenden, einschließlich anderer verwandter nuget-Pakete.
    > [!NOTE]
    > Windows Forms-apps müssen das [Microsoft. Toolkit. Forms. UI. xamlhost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) -Paket (Version v 6.0.0 oder höher) verwenden.

7. Konfigurieren Sie die Lösung so, dass Sie auf eine bestimmte Plattform wie x86 oder x64 ausgerichtet ist. Benutzerdefinierte UWP-Steuerelemente werden in Projekten, die auf **CPU**ausgerichtet sind, nicht unterstützt

    1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Knoten Projekt Mappe, und wählen Sie **Eigenschaften** -> **Konfigurations Eigenschaften** -> **Configuration Manager**aus. 
    2. Wählen Sie unter **Aktive Projektmappenplattform**die Option **neu**. 
    3. Wählen Sie im Dialogfeld **Neue Projektmappenplattform** **x64** oder **x86** aus, und klicken Sie auf **OK**. 
    4. Schließen Sie die Dialogfelder öffnen.

## <a name="create-a-xamlapplication-object-in-a-uwp-app-project"></a>Erstellen eines xamlapplication-Objekts in einem UWP-App-Projekt

Fügen Sie als nächstes der gleichen Projekt Mappe wie das WPF-Projekt ein UWP-App-Projekt hinzu. Die Standard `App` Klasse in diesem Projekt wird so überarbeitet, dass Sie von der `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` Klasse abgeleitet wird, die vom Windows Community Toolkit bereitgestellt wird. Das **windowsxamlhost** -Objekt in Ihrer WPF-App benötigt dieses `XamlApplication` Objekt, um benutzerdefinierte UWP-Steuerelemente zu hosten.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Knoten Projekt Mappe, und wählen Sie -> **Neues Projekt** **Hinzufügen** .
2. Fügen Sie der Projektmappe ein **Leere App (Universal Windows)** -Projekt hinzu. Stellen Sie sicher, dass die Zielversion und die Mindestversion auf **Windows 10, Version 1903** oder höher, festgelegt sind.
3. Installieren Sie im UWP-App-Projekt das nuget-Paket [Microsoft. Toolkit. Win32. UI. xamlapplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (Version v 6.0.0 oder höher).
4. Öffnen Sie die Datei " **app. XAML** ", und ersetzen Sie den Inhalt dieser Datei durch den folgenden XAML-Code. Ersetzen Sie `MyUWPApp` durch den Namespace des UWP-App-Projekts.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. Öffnen Sie die Datei **app.XAML.cs** , und ersetzen Sie den Inhalt dieser Datei durch den folgenden Code. Ersetzen Sie `MyUWPApp` durch den Namespace des UWP-App-Projekts.

    ```csharp
    namespace MyUWPApp
    {
        sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. Löschen Sie die Datei " **MainPage. XAML** " aus dem UWP-App-Projekt.
7. Bereinigen Sie das UWP-App-Projekt, und erstellen Sie es.
8. Klicken Sie im WPF-Projekt mit der rechten Maustaste auf den Knoten **Abhängigkeiten** , und fügen Sie einen Verweis auf das UWP-App-Projekt hinzu.

## <a name="create-a-custom-uwp-control"></a>Erstellen eines benutzerdefinierten UWP-Steuer Elements

Wenn Sie ein benutzerdefiniertes UWP-Steuerelement in Ihrer WPF-App hosten möchten, müssen Sie über den Quellcode für das Steuerelement verfügen, damit Sie ihn mit der APP kompilieren können. In der Regel werden benutzerdefinierte Steuerelemente in einem UWP-Klassen Bibliotheksprojekt für eine einfache Portabilität definiert.

In diesem Abschnitt definieren Sie ein einfaches benutzerdefiniertes UWP-Steuerelement in einem neuen Klassen Bibliotheksprojekt. Alternativ können Sie das benutzerdefinierte UWP-Steuerelement im UWP-App-Projekt definieren, das Sie im vorherigen Abschnitt erstellt haben. Diese Schritte führen dies jedoch in einem separaten Klassen Bibliotheksprojekt aus, um die Veranschaulichung zu veranschaulichen, da dies in der Regel die Implementierung benutzerdefinierter Steuerelemente für die Portabilität ist

Wenn Sie bereits über ein benutzerdefiniertes Steuerelement verfügen, können Sie es anstelle des hier gezeigten Steuer Elements verwenden. Sie müssen jedoch weiterhin das Projekt konfigurieren, das das Steuerelement enthält, wie in den folgenden Schritten gezeigt.

1. Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Knoten Projekt Mappe, und wählen Sie -> **Neues Projekt** **Hinzufügen** .
2. Fügen Sie der Projekt Mappe ein Projekt für eine **Klassenbibliothek (Universal Windows)** hinzu. Stellen Sie sicher, dass die Zielversion und die Mindestversion auf **Windows 10, Version 1903** oder höher, festgelegt sind.
3. Klicken Sie mit der rechten Maustaste auf die Projektdatei, und wählen Sie **Projekt entladen** Klicken Sie mit der rechten Maustaste erneut auf die Projektdatei, und wählen Sie **Bearbeiten**
4. Fügen Sie vor dem schließenden `</Project>`-Element den folgenden XML-Code hinzu, um mehrere Eigenschaften zu deaktivieren und die Projektdatei anschließend zu speichern. Diese Eigenschaften müssen aktiviert sein, um das benutzerdefinierte UWP-Steuerelement in einer WPF-App (oder Windows Forms) zu hosten.

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. Klicken Sie mit der rechten Maustaste auf die Projektdatei und wählen Sie **Projekt erneut laden**
6. Löschen Sie die Standarddatei **Class1.cs** , und fügen Sie dem Projekt ein neues **Benutzer Steuer** Element hinzu.
7. Fügen Sie in der XAML-Datei für das Benutzer Steuerelement den folgenden `StackPanel` als untergeordnetes Element des Standard `Grid`hinzu. In diesem Beispiel wird ein ``TextBlock``-Steuerelement hinzugefügt und dann das ``Text``-Attribut dieses Steuer Elements an das ``XamlIslandMessage`` Feld gebunden.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. Fügen Sie in der Code Behind-Datei des Benutzer Steuer Elements der Benutzer Steuerelement-Klasse das `XamlIslandMessage`-Feld hinzu, wie unten gezeigt.

    ```csharp
    public sealed partial class MyUserControl : UserControl
    {
        public string XamlIslandMessage { get; set; }

        public MyUserControl()
        {
            this.InitializeComponent();
        }
    }
    ```

9. Erstellen Sie das UWP-Klassen Bibliotheksprojekt.
10. Klicken Sie im WPF-Projekt mit der rechten Maustaste auf den Knoten **Abhängigkeiten** , und fügen Sie einen Verweis auf das UWP-Klassen Bibliotheksprojekt hinzu.
11. Klicken Sie im zuvor konfigurierten UWP-App-Projekt mit der rechten Maustaste auf den Knoten **Verweise** , und fügen Sie einen Verweis auf das UWP-Klassen Bibliotheksprojekt hinzu.
12. Erstellen Sie die gesamte Lösung neu, und stellen Sie sicher, dass alle Projekte erfolgreich erstellt werden.

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>Hosten des benutzerdefinierten UWP-Steuer Elements in Ihrer WPF-App

1. Erweitern Sie in **Projektmappen-Explorer**das WPF-Projekt, und öffnen Sie die Datei "MainWindow. XAML" oder ein anderes Fenster, in dem Sie das benutzerdefinierte Steuerelement hosten möchten.
2. Fügen Sie in der XAML-Datei die folgende Namespace Deklaration dem `<Window>`-Element hinzu.

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. Fügen Sie dem `<Grid>`-Element in derselben Datei das folgende-Steuerelement hinzu. Ändern Sie das `InitialTypeName`-Attribut in den voll qualifizierten Namen des Benutzer Steuer Elements im UWP-Klassen Bibliotheksprojekt.

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. Öffnen Sie die Code Behind-Datei, und fügen Sie der `Window`-Klasse den folgenden Code hinzu. Dieser Code definiert einen `ChildChanged` Ereignishandler, der den Wert des ``XamlIslandMessage`` Felds des benutzerdefinierten UWP-Steuer Elements dem Wert des Felds `WPFMessage` in der WPF-App zuweist. Ändern Sie `UWPClassLibrary.MyUserControl` in den voll qualifizierten Namen des Benutzer Steuer Elements im UWP-Klassen Bibliotheksprojekt.

    ```csharp
    private void WindowsXamlHost_ChildChanged(object sender, EventArgs e)
    {
        // Hook up x:Bind source.
        global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost windowsXamlHost =
            sender as global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost;
        global::UWPClassLibrary.MyUserControl userControl =
            windowsXamlHost.GetUwpInternalObject() as global::UWPClassLibrary.MyUserControl;

        if (userControl != null)
        {
            userControl.XamlIslandMessage = this.WPFMessage;
        }
    }

    public string WPFMessage
    {
        get
        {
            return "Binding from WPF to UWP XAML";
        }
    }
    ```

6. Erstellen und ausführen Sie Ihre APP, und vergewissern Sie sich, dass das UWP-Benutzer Steuerelement wie erwartet angezeigt wird

## <a name="add-a-control-from-the-winui-library-to-the-custom-control"></a>Hinzufügen eines Steuer Elements aus der WinUI-Bibliothek zum benutzerdefinierten Steuerelement

In der Vergangenheit wurden UWP-Steuerelemente als Teil des Windows 10-Betriebssystems veröffentlicht und Entwicklern über das Windows SDK zur Verfügung gestellt. Die [WinUI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/) ist ein alternativer Ansatz, bei dem aktualisierte Versionen der UWP-Steuerelemente der ersten Partei aus der Windows SDK in einem nuget-Paket verteilt werden, das nicht an Windows SDK Releases gebunden ist. Diese Bibliothek enthält auch neue Steuerelemente, die nicht Teil des Windows SDK und der UWP-Standardplattform sind. Weitere Informationen finden Sie in der Roadmap für die [WinUI-Bibliothek](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) .

In diesem Abschnitt wird veranschaulicht, wie Sie ein UWP-Steuerelement aus der WinUI-Bibliothek dem Benutzer Steuerelement hinzufügen, damit Sie dieses Steuerelement in Ihrer WPF-App hosten können

1. Installieren Sie im UWP-App-Projekt die neueste Version des nuget-Pakets " [Microsoft. UI. XAML](https://www.nuget.org/packages/Microsoft.UI.Xaml) ".

2. Fügen Sie in der Datei app. XAML in diesem Projekt dem `<xaml:XamlApplication>`-Element das folgende untergeordnete-Element hinzu.

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    Nachdem dieses Element hinzugefügt wurde, sollte der Inhalt dieser Datei nun in etwa wie folgt aussehen.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </xaml:XamlApplication>
    ```

3. Installieren Sie im UWP-Klassen Bibliotheksprojekt die neueste Version des nuget-Pakets " [Microsoft. UI. XAML](https://www.nuget.org/packages/Microsoft.UI.Xaml) " (die Version, die Sie im UWP-App-Projekt installiert haben).

4. Öffnen Sie im gleichen Projekt die XAML-Datei für das Benutzer Steuerelement, und fügen Sie die folgende Namespace Deklaration dem `<UserControl>`-Element hinzu.

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. Fügen Sie in derselben Datei ein `<winui:RatingControl />`-Element als untergeordnetes Element des-`<StackPanel>`hinzu. Dieses Element fügt eine Instanz der [ratingcontrol](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol?view=winui-2.2) -Klasse aus der WinUI-Bibliothek hinzu. Nachdem dieses Element hinzugefügt wurde, sollte der `<StackPanel>` jetzt in etwa wie folgt aussehen.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
        <winui:RatingControl />
    </StackPanel>
    ```

6. Erstellen und führen Sie Ihre APP aus, und vergewissern Sie sich, dass das neue Bewertungs Steuerelement wie erwartet angezeigt wird

## <a name="package-the-app"></a>Verpacken der APP

Sie können die WPF-App optional in einem [msix-Paket](https://docs.microsoft.com/windows/msix) für die Bereitstellung verpacken. Msix ist die moderne App-Verpackungstechnologie für Windows und basiert auf einer Kombination aus MSI-, AppX-, App-V-und ClickOnce-Installationstechnologien.

Die folgenden Anweisungen zeigen, wie Sie alle Komponenten in der Projekt Mappe in einem msix-Paket Verpacken, indem Sie das [Windows-Anwendungs](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) Paket in Visual Studio 2019 verwenden. Diese Schritte sind nur erforderlich, wenn Sie die WPF-App in einem msix-Paket verpacken möchten. Beachten Sie, dass diese Schritte derzeit einige Problem Umgehungen für das Szenario des Hostings benutzerdefinierter UWP-Steuerelemente beinhalten.

1. Fügen Sie der Projekt Mappe ein neues [Windows-Anwendungspaket](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) hinzu. Wählen Sie beim Erstellen des Projekts die Option **Windows 10, Version 1903 (10,0; Build 18362)** für die **Zielversion** und die **Mindestversion**.

2. Klicken Sie im Paket Projekt mit der rechten Maustaste auf den Knoten **Anwendungen** , und wählen Sie **Verweis hinzufügen**aus. Wählen Sie in der Liste der Projekte das WPF-Projekt in der Projekt Mappe aus, und klicken Sie auf **OK**.

3. Bearbeiten Sie die Projektdatei für die Paket Erstellung. Diese Änderungen sind zurzeit erforderlich, um WPF-apps zu verpacken, die auf .net Core 3 ausgerichtet sind und die XAML-Inseln hosten.

    1. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Knoten Verpackungsprojekt, und wählen Sie dann **Projektdatei bearbeiten**aus.
    2. Suchen Sie das `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />`-Element in der Datei. Ersetzen Sie dieses Element durch das folgende XML. Diese Änderungen sind derzeit erforderlich, um WPF-apps zu verpacken, die auf .net Core 3 ausgerichtet sind und UWP-Steuerelemente hosten.

        ``` xml
        <ItemGroup>
            <SDKReference Include="Microsoft.VCLibs,Version=14.0">
            <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
            <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
            <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
            <Implicit>true</Implicit>
            </SDKReference>
        </ItemGroup>
        <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
        <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
            <ItemGroup>
            <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
            <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
            <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
                <SourceProject></SourceProject>
                <TargetPath Condition="'%(FileName)%(Extension)'=='resources.pri'">app_resources.pri</TargetPath>
            </_FilteredNonWapProjProjectOutput>
            </ItemGroup>
        </Target>
        ```

    3. Speichern Sie die Projektdatei und schließen Sie sie.

4. Bearbeiten Sie die WPF-Projektdatei. Diese Änderungen sind zurzeit zum Verpacken von WPF-apps erforderlich, die benutzerdefinierte UWP-Steuerelemente hosten

    1. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den WPF-Projekt Knoten, und wählen Sie **Projekt entladen**aus.
    2. Klicken Sie mit der rechten Maustaste auf den Projekt Knoten WPF, und wählen Sie **Bearbeiten**.
    3. Suchen Sie nach dem letzten `</PropertyGroup>` schließenden Tag in der Datei, und fügen Sie den folgenden XML-Code direkt nach diesem Tag ein.

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. Speichern Sie die Projektdatei und schließen Sie sie.
    5. Klicken Sie mit der rechten Maustaste auf den Projekt Knoten WPF, und wählen Sie **Projekt erneut laden**

5. Erstellen Sie das Verpackungsprojekt, und führen Sie es aus. Vergewissern Sie sich, dass WPF ausgeführt wird und das benutzerdefinierte UWP-Steuerelement erwartungsgemäß angezeigt wird.

## <a name="related-topics"></a>Zugehörige Themen

* [UWP-Steuerelemente in Desktop Anwendungen](xaml-islands.md)
* [Windowsxamlhost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
