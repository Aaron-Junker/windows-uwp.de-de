---
description: Erstellen von .NET- und C++/Win32-Desktop-Apps mit einer WinUI 3-Benutzeroberfläche.
title: Erstellen einer einfachen WinUI 3-App für den Desktop
ms.date: 03/19/2021
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: e7453f620b7c82c46a44e293a36a9eb51bfb36ae
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730745"
---
# <a name="build-a-basic-winui-3---project-reunion-05-desktop-app"></a>Erstellen einer grundlegenden WinUI 3 – Project Reunion 0.5-Desktop-App

In diesem Artikel wird Schritt für Schritt gezeigt, wie Sie grundlegende verwaltete C#/.NET 5- und native C++/Win32 WinUI 3 – Project Reunion 0.5-Desktopanwendungen mit Benutzeroberflächen erstellen, die vollständig aus WinUI 3-Steuerelementen und -Features bestehen.

## <a name="prerequisites"></a>Voraussetzungen

1. Einrichten Ihrer Entwicklungsumgebung. Weitere Informationen finden Sie unter [Erste Schritte mit Project Reunion](../../project-reunion/get-started-with-project-reunion.md).
1. Testen Sie Ihre Konfiguration, indem Sie Ihre erste WinUI 3-Desktop-App für C#- und .>NET 5-Projekte erstellen. Weitere Informationen finden Sie unter [Erste Schritte mit WinUI 3 für Desktop-Apps](get-started-winui3-for-desktop.md).

:::image type="content" source="images/build-basic/template-app.png" alt-text="Die anfängliche Vorlagen-APP, die ausgeführt wird.":::<br/>
*Die anfängliche Vorlagen-APP, die ausgeführt wird.*

## <a name="initial-template-app"></a>Anfängliche Vorlagen-App

Wir beginnen die Erstellung aus der anfänglichen Vorlagenanwendung, aber vorher sehen wir uns die Struktur und den Inhalt der Vorlagen-App an.

### <a name="the-application-solution"></a>Die Anwendungsprojektmappe

Standardmäßig enthält die Projektmappe zwei Projekte: die Anwendung selbst und eine weitere zum Erstellen eines [MSIX](/windows/msix)-App-Pakets.

> [!NOTE]
> Das MSIX-App-Paket ist zurzeit erforderlich, um Apps bereitzustellen, die Project Reunion 0.5 auf anderen Computern verwenden. Zukünftige Releases von Project Reunion unterstützen jedoch die Bereitstellung von nicht gepackten Apps.

:::image type="content" source="images/build-basic/template-app-solution-explorer.png" alt-text="Projektmappen-Explorer, der die Dateistruktur der anfänglichen Vorlagen-App zeigt.":::<br/>
*Projektmappen-Explorer, der die Dateistruktur der anfänglichen Vorlagen-App zeigt.*

### <a name="the-application-project"></a>Das Anwendungsprojekt

Doppelklicken Sie nun auf die Anwendungsprojektdatei (oder drücken Sie rechte Maustaste, und wählen Sie „Projektdatei bearbeiten“ aus), wodurch die Datei in einem XML-Text-Editor geöffnet wird.

Im Folgenden sehen Sie die Projektdatei der ursprünglichen Vorlagen-App, die zwei erwähnenswerte Elemente enthält:

- Das `TargetFramework` ist [.NET 5](/dotnet/core/dotnet-five), von nun an die primäre Implementierung von .NET.
- Die NuGet-`PackageReference`-Elemente für die verschiedenen Project Reunion-Features, einschließlich WinUI.

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
    <TargetPlatformMinVersion>10.0.17763.0</TargetPlatformMinVersion>
    <RootNamespace>WinUIApp2</RootNamespace>
    <Platforms>x86;x64;arm64</Platforms>
    <RuntimeIdentifiers>win10-x86;win10-x64;win10-arm64</RuntimeIdentifiers>
    <NoWin32Manifest>true</NoWin32Manifest>
    <ApplicationIcon />
    <StartupObject />
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.ProjectReunion" Version="0.5.0-prerelease" />
    <PackageReference Include="Microsoft.ProjectReunion.Foundation" Version="0.5.0-prerelease" />
    <PackageReference Include="Microsoft.ProjectReunion.WinUI" Version="0.5.0-prerelease" />
    <Manifest Include="$(ApplicationManifest)" />
  </ItemGroup>
</Project>
```

### <a name="the-mainwindowxaml-file"></a>Die Datei „MainWindow.xaml“

Mit WinUI 3 können Sie jetzt Instanzen der [Window](/windows/winui/api/microsoft.ui.xaml.window)-Klasse in XAML-Markup erstellen.

Die XAML-Klasse „Window“ wurde um die Unterstützung von Desktopfenstern erweitert, wodurch sie zu einer Abstraktion der einzelnen von den UWP- und Desktop-App-Modellen verwendeten Fensterimplementierungen auf niedriger Ebene wird. Genauer gesagt verwendet UWP CoreWindow, während Win32 Fensterhandles (oder HWNDs) und entsprechende Win32-APIs verwendet.

Das folgende Codebeispiel zeigt die Datei „MainWindow.xaml“ aus der anfänglichen Vorlagen-App, die die Klasse „Window“ als Stammelement für die App verwendet.

```xaml
<Window
    x:Class="WinUIApp2.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:WinUIApp2"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Orientation="Horizontal" HorizontalAlignment="Center" VerticalAlignment="Center">
        <Button x:Name="myButton" Click="MyButton_Click">Click Me</Button>
    </StackPanel>
</Window>
```

## <a name="basic-managed-cnet-5-app"></a>Grundlegende verwaltete C#/.NET 5-App

Für dieses erste Beispiel verwenden wir die [ShowWindow](/windows/win32/api/winuser/nf-winuser-showwindow)-API aus „User32.dll“, um das Fenster programmatisch zu maximieren.

1. Um Win32-APIs aus User32.dll aufzurufen, fügen Sie zunächst das NuGet-Paket **PInvoke.User32** zu Ihrem Projekt hinzu (wählen Sie in den Visual Studio-Menüs **Extras -> NuGet-Paket-Manager -> NuGet-Pakete für Projektmappe verwalten...** aus). Weitere Details finden Sie unter [Aufrufen nativer Funktionen aus verwaltetem Code](/cpp/dotnet/calling-native-functions-from-managed-code).
1. Nachdem Sie das Paket hinzugefügt haben, öffnen Sie die Code Behind-Datei „MainWindow.xaml.cs“, und ersetzen Sie den `MyButton_Click`-Ereignishandler durch den folgenden Code:

    ```csharp
            private void MyButton_Click(object sender, RoutedEventArgs e)
            {
                myButton.Content = "Clicked";
    
                IntPtr hwnd = (App.Current as App).MainWindowWindowHandle;
                PInvoke.User32.ShowWindow(hwnd, PInvoke.User32.WindowShowStyle.SW_MAXIMIZE);
            }
    ```

    Die Methode [PInvoke](/dotnet/standard/native-interop/pinvoke) [ShowWindow](/windows/win32/api/winuser/nf-winuser-showwindow) verwendet ein Fensterhandle (`hwnd`) als ersten Parameter und gibt mit dem zweiten Parameter an, dass das Fenster maximiert werden soll. 

1. Um ein Fensterhandle abzurufen, verwenden Sie die [GetActiveWindow](/windows/win32/api/winuser/nf-winuser-getactivewindow)-Methode. Diese Methode gibt das Fensterhandle des aktuell aktiven Fensters zurück (rufen Sie diese Methode auf, nachdem Sie das Zielfenster aktiviert haben).

    Das `MainWindow`-Objekt (siehe „MainWindow.xaml“) wird im `OnLaunched`-Ereignishandler erstellt, instanziiert und aktiviert, der sich in der Code-Behind-Datei „App.xaml.cs“ befindet. Ersetzen Sie das Ereignis `OnLaunched` durch den folgenden Code:

    ```csharp
        protected override void OnLaunched(Microsoft.UI.Xaml.LaunchActivatedEventArgs args)
        {
            m_window = new MainWindow();
            m_window.Activate();
            m_windowhandle = PInvoke.User32.GetActiveWindow();
        }

        IntPtr m_windowhandle;
        public IntPtr MainWindowWindowHandle { get { return m_windowhandle; } }
    ```

1. Kompiliere die App, und führe sie aus.
1. Drücken Sie abschließend die Schaltfläche „Hier klicken“, und das App-Fenster sollte maximiert werden.

## <a name="full-trust-desktop-app"></a>„Voll vertrauenswürdige“ Desktop-App

WinUI 3 bietet die Möglichkeit, Anwendungen außerhalb der Sicherheitssandbox eines [AppContainer](/windows/win32/secauthz/appcontainer-for-legacy-applications-)-Objekts mit der Berechtigung „Voll vertrauenswürdig“ auszuführen.

Verwaltete C#/.NET 5- und native C++/Win32 WinUI 3 – Project Reunion 0.5-Desktopanwendungen können .NET 5-APIs ohne Einschränkungen aufrufen. Im folgenden Beispiel (abgeleitet von der anfänglichen Vorlagen-App aus dem vorherigen Beispiel) zeigen wir, wie Sie den aktuellen Prozess abfragen und eine Liste aller geladenen Module abrufen können (bei UWP-Apps nicht möglich).

1. Fügen Sie in der Datei „MainWindow.xaml“ ein [ContentDialog](/windows/winui/api/microsoft.ui.xaml.controls.contentdialog)-Element hinzu.

    ```xaml
    <StackPanel 
        Orientation="Horizontal" 
        HorizontalAlignment="Center" 
        VerticalAlignment="Center">
        <Button x:Name="myButton" Click="MyButton_Click">Click Me</Button>
        <ContentDialog x:Name="contentDialog" CloseButtonText="Close">
            <StackPanel>
                <TextBlock Name="cdTextBlock"/>
            </StackPanel>
        </ContentDialog>
    </StackPanel>
    ```

1. Rufen Sie in der Code-Behind-Datei „MainWindow.xaml.cs“ die .NET-APIs aus [System.Diagnostics](/dotnet/api/system.diagnostics) auf, um die im [aktuellen Prozess](/dotnet/api/system.diagnostics.process.getcurrentprocess) geladenen Module zu erhalten. In diesem Beispiel werden einfach alle [ProcessModule](/dotnet/api/system.diagnostics.processmodule)-Elemente im [Process.Modules](/dotnet/api/system.diagnostics.process.modules)-Objekt durchlaufen.

    ```csharp
    private async void MyButton_Click(object sender, RoutedEventArgs e)
    {
        myButton.Content = "Clicked";

        var description = new System.Text.StringBuilder();
        var process = System.Diagnostics.Process.GetCurrentProcess();
        foreach (System.Diagnostics.ProcessModule module in process.Modules)
        {
            description.AppendLine(module.FileName);
        }

        cdTextBlock.Text = description.ToString();
        await contentDialog.ShowAsync();
    }
    ```
1. Kompiliere die App, und führe sie aus.
1. Klicken Sie abschließend auf die Schaltfläche „Hier klicken“, und das Dialogfeld mit der Liste der Prozesse sollte angezeigt werden.

    :::image type="content" source="images/build-basic/template-app-full-trust.png" alt-text="Beispiel für eine voll vertrauenswürdige Anwendung.":::<br/>*Beispiel für eine voll vertrauenswürdige Anwendung.*

## <a name="summary"></a>Zusammenfassung

In diesem Thema wurde der Zugriff auf die zugrunde liegende Fensterimplementierung, in diesem Fall Win32 und HWNDs, sowie die Verwendung von Win32-APIs in Ihrer App zusammen mit den WinRT-APIs von Windows 10 behandelt. Dadurch können Sie beim Erstellen neuer WinUI 3-Desktop-Apps einen Großteil Ihres vorhandenen Desktop Anwendungscodes verwenden.

Außerdem wurde beschrieben, wie Sie die .NET 5-APIs mit WinUI 3 als Benutzeroberflächenframework verwenden können.

## <a name="see-also"></a>Weitere Informationen

- [Windows-UI-Bibliothek 3 – Project Reunion 0.5 (März 2021)](index.md)
- [Erste Schritte mit Project Reunion](../../project-reunion/get-started-with-project-reunion.md)
- [Erste Schritte mit WinUI 3 für Desktop-Apps](get-started-winui3-for-desktop.md)
