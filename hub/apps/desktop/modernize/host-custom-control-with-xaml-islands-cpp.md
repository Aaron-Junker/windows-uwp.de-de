---
description: Dieser Artikel veranschaulicht, wie ein benutzerdefiniertes UWP-Steuerelement mithilfe der XAML-Hosting-API in einer C++-Win32-App gehostet wird.
title: Hosten eines benutzerdefinierten UWP-Steuerelements in einer C++-Win32-App unter Verwendung der XAML-Hosting-API
ms.date: 04/07/2020
ms.topic: article
keywords: Windows 10, UWP, C++, Win32, XAML Islands, benutzerdefinierte Steuerelemente, Benutzersteuerelemente, Hosten von Steuerelementen
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: d61abe8b59f916ed56c1fefe0bda4b9f25b673a4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173724"
---
# <a name="host-a-custom-uwp-control-in-a-c-win32-app"></a>Hosten eines benutzerdefinierten UWP-Steuerelements in einer C++-Win32-App

Dieser Artikel veranschaulicht die Verwendung der [XAML-Hosting-API](using-the-xaml-hosting-api.md) zum Hosten eines benutzerdefinierten UWP-XAML-Steuerelements in einer neuen C++-Win32-App. Wenn du über ein vorhandenes C++-Win32-Projekt verfügst, kannst du diese Schritte und Codebeispiele für dein Projekt anpassen.

Um ein benutzerdefiniertes UWP-XAML-Steuerelement zu hosten, erstellst du im Rahmen dieser exemplarischen Vorgehensweise die folgenden Projekte und Komponenten:

* **Projekt für eine Windows-Desktopanwendung**. Dieses Projekt implementiert eine native C++-Win32-Desktop-App. Du fügst diesem Projekt Code hinzu, der die XAML-Hosting-API zum Hosten eines benutzerdefinierten UWP-XAML-Steuerelements verwendet.

* **UWP-App-Projekt (C++/WinRT)** . Dieses Projekt implementiert ein benutzerdefiniertes UWP-XAML-Steuerelement. Darüber hinaus implementiert es einen Stamm-Metadatenanbieter zum Laden von Metadaten für benutzerdefinierte UWP-XAML-Typen in das Projekt.

## <a name="requirements"></a>Anforderungen

* Visual Studio 2019, Version 16.4.3 oder höher.
* SDK-Version 10.0.18362 für Windows 10, Version 1903 oder höher.
* [C++/WinRT Visual Studio Extension (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264), installiert mit Visual Studio. C++/WinRT ist eine vollständig standardisierte, moderne C++17-Programmiersprache für Windows-Runtime-APIs (WinRT), die als headerdateibasierte Bibliothek implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet. Weitere Informationen findest du unter [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/).

## <a name="create-a-desktop-application-project"></a>Erstellen eines Desktopanwendungsprojekts

1. Erstelle in Visual Studio ein neues Projekt **Windows-Desktopanwendung** mit dem Namen **MyDesktopWin32App**. Diese Projektvorlage steht unter den Projektfiltern **C++** , **Windows** und **Desktop** zur Verfügung.

2. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappenknoten, klicke auf **Projektmappe neu zuweisen**, wähle **10.0.18362.0** oder eine höhere SDK-Version aus, und klicke dann auf **OK**.

3. Installiere das NuGet-Paket [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/), um die Unterstützung für [C++/WinRT](/windows/uwp/cpp-and-winrt-apis) in deinem Projekt zu aktivieren:

    1. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **MyDesktopWin32App**, und wähle **NuGet-Pakete verwalten** aus.
    2. Wähle die Registerkarte **Durchsuchen** aus, suche nach dem Paket [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/), und installiere die neueste Version dieses Pakets.

4. Installiere im Fenster **NuGet-Pakete verwalten** die folgenden zusätzlichen NuGet-Pakete:

    * [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) (Version v6.0.0 oder höher). Dieses Paket stellt verschiedene Ressourcen für Kompilier- und Laufzeit bereit, um XAML Islands in deiner App zu aktivieren.
    * [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (Version v6.0.0 oder höher). Mit diesem Paket wird die [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)-Klasse definiert, die du später in dieser exemplarischen Vorgehensweise verwendest.
    * [Microsoft.VCRTForwarders.140](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140).

5. Kompiliere die Projektmappe und stelle sicher, dass der Vorgang erfolgreich war.

## <a name="create-a-uwp-app-project"></a>Erstellen eines UWP-App-Projekts

Als Nächstes fügst du deiner Projektmappe ein **UWP (C++/WinRT)** -App-Projekt hinzu und nimmst einige Konfigurationsänderungen an diesem Projekt vor. Später in dieser exemplarischen Vorgehensweise fügst du diesem Projekt Code hinzu, um ein benutzerdefiniertes UWP-XAML-Steuerelement zu implementieren und eine Instanz der [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)-Klasse zu definieren. 

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappenknoten, und wählen Sie **Hinzufügen** -> **Neues Projekt** aus.

2. Füge deiner Projektmappe ein Projekt vom Typ **Leere App (C++/WinRT)** hinzu. Gibt dem Projekt den Namen **MyUWPApp**, und stelle sicher, dass sowohl die Zielversion als auch die mindestens erforderliche Version auf **Windows 10, Version 1903** oder höher festgelegt ist.

3. Installiere im **MyUWPApp**-Projekt das NuGet-Paket [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication). Mit diesem Paket wird die [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)-Klasse definiert, die du später in dieser exemplarischen Vorgehensweise verwendest.

    1. Klicke mit der rechten Maustaste auf das **MyUWPApp**-Projekt, und wähle **NuGet-Pakete verwalten** aus.
    2. Wähle die Registerkarte **Durchsuchen** aus, suche nach dem Paket [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication), und installiere Version v6.0.0 dieses Pakets oder eine höhere Version.

4. Klicke mit der rechten Maustaste auf den **MyUWPApp**-Knoten, und wähle **Eigenschaften** aus. Legen Sie auf der Seite **Allgemeine Eigenschaften** -> **C++/WinRT** die Eigenschaft **Verbosity** auf **normal** fest, und klicken Sie dann auf **Anwenden**. Anschließend sollte die Eigenschaftenseite wie folgt aussehen.

    ![Eigenschaften des C++/WinRT-Projekts](images/xaml-islands/xaml-island-cpp-1.png)

5. Lege auf der Seite **Konfigurationseigenschaften** -> **Allgemein** des Eigenschaftenfensters die Einstellung **Konfigurationstyp** auf **Dynamische Bibliothek (.dll)** fest, und klicke dann auf **OK**, um das Eigenschaftenfenster zu schließen.

    ![Allgemeine Projekteigenschaften](images/xaml-islands/xaml-island-cpp-2.png)

6. Füge dem Projekt **MyUWPApp** eine ausführbare Platzhalterdatei hinzu. Diese ausführbare Platzhalterdatei wird für Visual Studio benötigt, um die erforderlichen Projektdateien zu generieren und das Projekt ordnungsgemäß zu kompilieren.

    1. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **MyUWPApp**, und wähle **Hinzufügen** -> **Neues Element** aus.
    2. Klicke im Dialogfeld **Neues Element hinzufügen** auf der linken Seite auf **Hilfsprogramm**, und wähle **Textdatei (.txt)** aus. Gib den Namen **placeholder.exe** ein, und klicke auf **Hinzufügen**.
      ![Hinzufügen einer Textdatei](images/xaml-islands/xaml-island-cpp-3.png)
    3. Wähle im **Projektmappen-Explorer** die Datei **placeholder.exe** aus. Vergewissere dich im Fenster **Eigenschaften**, dass die Eigenschaft **Content** auf **True** festgelegt ist.
    4. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf die Datei **Package.appxmanifest** im Projekt **MyUWPApp**, wähle **Öffnen mit** und **XML (Text)-Editor** aus, und klicke auf **OK**.
    5. Suche nach dem Element **&lt;Application&gt;** , und ändere das Attribut **Executable** in den Wert `placeholder.exe`. Anschließend sollte das Element **&lt;Application&gt;** in etwa wie folgt aussehen.

        ```xml
        <Application Id="App" Executable="placeholder.exe" EntryPoint="MyUWPApp.App">
          <uap:VisualElements DisplayName="MyUWPApp" Description="Project for a single page C++/WinRT Universal Windows Platform (UWP) app with no predefined layout"
            Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png" BackgroundColor="transparent">
            <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
        ```

    6. Speichere und schließe die Datei **Package.appxmanifest**.

7. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Knoten **MyUWPApp**, und wähle **Projekt entladen** aus.
8. Klicke mit der rechten Maustaste auf den Knoten **MyUWPApp**, und wähle **MyUWPApp.vcxproj bearbeiten** aus.
9. Suche nach dem Element `<Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />`, und ersetze es durch den folgenden XML-Ausschnitt. Hierdurch werden direkt vor dem Element verschiedene neue Eigenschaften hinzugefügt.

    ```xml
    <PropertyGroup Label="Globals">
        <WindowsAppContainer>true</WindowsAppContainer>
        <AppxGeneratePriEnabled>true</AppxGeneratePriEnabled>
        <ProjectPriIndexName>App</ProjectPriIndexName>
        <AppxPackage>true</AppxPackage>
    </PropertyGroup>
    <Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />
    ```

10. Speichere und schließe die Projektdatei.
11. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Knoten **MyUWPApp**, und wähle **Projekt erneut laden** aus.

## <a name="configure-the-solution"></a>Konfigurieren der Projektmappe

In diesem Abschnitt aktualisierst du die Projektmappe, die beide Projekte enthält, um Projektabhängigkeiten zu konfigurieren und Eigenschaften zu erstellen, die zum ordnungsgemäßen Kompilieren der Projekte erforderlich sind.

1. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappenknoten, und füge eine neue XML-Datei namens **Solution.props** hinzu.
2. Füge der Datei **Solution.props** den folgenden XML-Ausschnitt hinzu.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <PropertyGroup>
        <IntDir>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</IntDir>
        <OutDir>$(SolutionDir)\bin\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</OutDir>
        <GeneratedFilesDir>$(IntDir)Generated Files\</GeneratedFilesDir>
      </PropertyGroup>
    </Project>
    ```

3. Klicke im Menü **Ansicht** auf **Eigenschaften-Manager** (je nach Konfiguration kann dieser Punkt sich auch unter **Ansicht** -> **Weitere Fenster** befinden).
4. Klicke im Fenster **Eigenschaften-Manager** mit der rechten Maustaste auf **MyDesktopWin32App**, und wähle **Vorhandenes Eigenschaftenblatt hinzufügen** aus. Navigiere zur gerade hinzugefügten Datei **Solution.props**, und klicke auf **Öffnen**.
5. Wiederhole die vorherigen Schritte, um die Datei **Solution.props** dem Projekt **MyUWPApp** im Fenster **Eigenschaften-Manager** hinzuzufügen.
6. Schließe das Fenster **Eigenschaften-Manager**.
7. Vergewissere dich, dass die Änderungen am Eigenschaftenblatt erfolgreich gespeichert wurden. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **MyDesktopWin32App** und dann auf **Eigenschaften**. Klicke auf **Konfigurationseigenschaften** -> **Allgemein**, und vergewissere dich, dass die Eigenschaften **Ausgabeverzeichnis** und **Zwischenverzeichnis** die Werte enthalten, die du der Datei **Solution.props** hinzugefügt hast. Führe die gleiche Überprüfung für das Projekt **MyUWPApp** durch.
    ![Projekteigenschaften](images/xaml-islands/xaml-island-cpp-4.png)

8. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappenknoten, und wähle **Projektabhängigkeiten** aus. Stelle sicher, dass in der Dropdownliste **Projekte** der Eintrag **MyDesktopWin32App** ausgewählt ist, und wähle in der Liste **Abhängigkeiten** den Eintrag **MyUWPApp** aus.
    ![Projektabhängigkeiten](images/xaml-islands/xaml-island-cpp-5.png)

9. Klicken Sie auf **OK**.

## <a name="add-code-to-the-uwp-app-project"></a>Hinzufügen von Code zum UWP-App-Projekt

Jetzt kannst du Code zum Projekt **MyUWPApp** hinzufügen, um diese Aufgaben auszuführen:

* Implementieren eines benutzerdefinierten UWP-XAML-Steuerelements. Später in dieser exemplarischen Vorgehensweise fügst du Code hinzu, mit dem dieses Steuerelement im Projekt **MyDesktopWin32App** gehostet wird.
* Definieren eines Typs, der von der Klasse [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) aus dem Windows-Community-Toolkit abgeleitet ist.

### <a name="define-a-custom-uwp-xaml-control"></a>Definieren eines benutzerdefinierten UWP-XAML-Steuerelements

1. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf **MyUWPApp**, und wähle **Hinzufügen** -> **Neues Element** aus. Wähle im linken Bereich **Visual C++** aus, klicke auf **Leeres Benutzersteuerelement (C++/WinRT)** , gib ihm den Namen **MyUserControl**, und klicke auf **Hinzufügen**.
2. Ersetze im XAML-Editor die Inhalte der Datei **MyUserControl.xaml** durch den folgenden XAML-Ausschnitt, und speichere die Datei.

    ```xml
    <UserControl
        x:Class="MyUWPApp.MyUserControl"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyUWPApp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <StackPanel HorizontalAlignment="Center" Spacing="10" 
                    Padding="20" VerticalAlignment="Center">
            <TextBlock HorizontalAlignment="Center" TextWrapping="Wrap" 
                           Text="Hello from XAML Islands" FontSize="30" />
            <TextBlock HorizontalAlignment="Center" Margin="15" TextWrapping="Wrap"
                           Text="😍❤💋🌹🎉😎�🐱‍👤" FontSize="16" />
            <Button HorizontalAlignment="Center" 
                    x:Name="Button" Click="ClickHandler">Click Me</Button>
        </StackPanel>
    </UserControl>
    ```

### <a name="define-a-xamlapplication-class"></a>Definieren einer XamlApplication-Klasse

Als Nächstes überarbeitest du die **App**-Standardklasse im Projekt **MyUWPApp**, um sie von der [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)-Klasse abzuleiten, die im Windows-Community-Toolkit bereitgestellt wird. Diese Klasse unterstützt die [IXamlMetadaraProvider](/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider)-Schnittstelle, mit der deine App Metadaten für benutzerdefinierte UWP-XAML-Steuerelemente in Assemblys im aktuellen Verzeichnis der Anwendung zur Laufzeit ermitteln und laden kann. Diese Klasse initialisiert außerdem das UWP-XAML-Framework für den aktuellen Thread. Später in dieser exemplarischen Vorgehensweise aktualisierst du das Desktopprojekt, um eine Instanz dieser Klasse zu erstellen.

  > [!NOTE]
  > Jede Projektmappe mit Verwendung von XAML Islands kann nur ein Projekt enthalten, das ein `XamlApplication`-Objekt definiert. Alle benutzerdefinierten UWP-XAML-Steuerelemente in deiner App nutzen gemeinsam dasselbe `XamlApplication`-Objekt. 

1. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf die Datei **MainPage.xaml** im Projekt **MyUWPApp**. Klicke auf **Entfernen** und dann auf **Löschen**, um diese Datei endgültig aus dem Projekt zu löschen.
2. Erweitere im Projekt **MyUWPApp** die Datei **App.xaml**.
3. Ersetze die Inhalte der Dateien **App.xaml**, **App.cpp**, **App.h** und **App.idl** durch den folgenden Code.

    * **App.xaml**:

        ```xml
        <Toolkit:XamlApplication
            x:Class="MyUWPApp.App"
            xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="using:MyUWPApp">
        </Toolkit:XamlApplication>
        ```

    * **App.idl**:

        ```IDL
        namespace MyUWPApp
        {
             [default_interface]
             runtimeclass App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
             {
                App();
             }
        }
        ```

    * **App.h**:

        ```cpp
        #pragma once
        #include "App.g.h"
        #include "App.base.h"
        namespace winrt::MyUWPApp::implementation
        {
            class App : public AppT2<App>
            {
            public:
                App();
                ~App();
            };
        }
        namespace winrt::MyUWPApp::factory_implementation
        {
            class App : public AppT<App, implementation::App>
            {
            };
        }
        ```

    * **App.cpp**:

        ```cpp
        #include "pch.h"
        #include "App.h"
        #include "App.g.cpp"
        using namespace winrt;
        using namespace Windows::UI::Xaml;
        namespace winrt::MyUWPApp::implementation
        {
            App::App()
            {
                Initialize();
                AddRef();
                m_inner.as<::IUnknown>()->Release();
            }
            App::~App()
            {
                Close();
            }
        }
        ```

        > [!NOTE]
        > Die `#include "App.g.cpp"`-Anweisung ist erforderlich, wenn die Eigenschaft **Optimized** auf der Seite **Common Properties** -> **C++/WinRT** der Projekteigenschaften auf **Yes** festgelegt ist. Dies ist die Standardeinstellung für neue C++/WinRT-Projekte. Weitere Informationen zu den Auswirkungen der Eigenschaft **Optimized** finden Sie in [diesem Abschnitt](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access).

4. Füge dem Projekt **MyUWPApp** eine neue Headerdatei namens **app.base.h** hinzu.
5. Füge der Datei **app.base.h** den folgenden Code hinzu, speichere und schließe die Datei.

    ```cpp
    #pragma once
    namespace winrt::MyUWPApp::implementation
    {
        template <typename D, typename... I>
        struct App_baseWithProvider : public App_base<D, ::winrt::Windows::UI::Xaml::Markup::IXamlMetadataProvider>
        {
            using IXamlType = ::winrt::Windows::UI::Xaml::Markup::IXamlType;
            IXamlType GetXamlType(::winrt::Windows::UI::Xaml::Interop::TypeName const& type)
            {
                return AppProvider()->GetXamlType(type);
            }
            IXamlType GetXamlType(::winrt::hstring const& fullName)
            {
                return AppProvider()->GetXamlType(fullName);
            }
            ::winrt::com_array<::winrt::Windows::UI::Xaml::Markup::XmlnsDefinition> GetXmlnsDefinitions()
            {
                return AppProvider()->GetXmlnsDefinitions();
            }
        private:
            bool _contentLoaded{ false };
            std::shared_ptr<XamlMetaDataProvider> _appProvider;
            std::shared_ptr<XamlMetaDataProvider> AppProvider()
            {
                if (!_appProvider)
                {
                    _appProvider = std::make_shared<XamlMetaDataProvider>();
                }
                return _appProvider;
            }
        };
        template <typename D, typename... I>
        using AppT2 = App_baseWithProvider<D, I...>;
    }
    ```

6. Kompiliere die Projektmappe und stelle sicher, dass der Vorgang erfolgreich war.

## <a name="configure-the-desktop-project-to-consume-custom-control-types"></a>Konfigurieren des Desktopprojekts zur Verwendung benutzerdefinierter Steuerelementtypen

Bevor die App **MyDesktopWin32App** ein benutzerdefiniertes UWP-XAML-Steuerelement in XAML Islands hosten kann, muss sie zur Nutzung benutzerdefinierter Steuerelementtypen aus dem Projekt **MyUWPApp** konfiguriert werden. Es gibt hierfür zwei Möglichkeiten, und du kannst dich in dieser exemplarischen Vorgehensweise für eine dieser Optionen entscheiden.

### <a name="option-1-package-the-app-using-msix"></a>Option 1: Packen der App mit MSIX

Du kannst die App für in einem [MSIX-Paket](/windows/msix) die Bereitstellung packen. MSIX ist eine moderne App-Pakettechnologie für Windows, die auf einer Kombination aus MSI-, APPX-, App-V- und ClickOnce-Installationstechnologien basiert.

1. Füge deiner Projektmappe ein [Paketerstellungsprojekt für Windows-Anwendungen](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) hinzu. Benenne das Projekt beim Erstellen als **MyDesktopWin32Project**, und wähle **Windows 10, Version 1903 (10.0; Build 18362)** sowohl für **Zielversion** als auch für **Mindestversion** aus.

2. Klicke im Paketprojekt mit der rechten Maustaste auf den Knoten **Anwendungen**, und wähle **Verweis hinzufügen** aus. Aktiviere in der Liste der Projekte das Kontrollkästchen neben dem Projekt **MyDesktopWin32App**, und klicke auf **OK**.
    ![Verweis auf Projekt](images/xaml-islands/xaml-island-cpp-6.png)

> [!NOTE]
> Wenn du deine Anwendung nicht in einem [MSIX-Paket](/windows/msix) für die Bereitstellung packst, muss auf Computern zur Ausführung deiner App die [Visual C++-Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) installiert sein.

### <a name="option-2-create-an-application-manifest"></a>Option 2: Erstellen eines Anwendungsmanifest

Du kannst deiner App ein [Anwendungsmanifest](/windows/desktop/SbsCs/application-manifests) hinzufügen.

1. Klicke mit der rechten Maustaste auf das Projekt **MyDesktopWin32App**, und wähle **Hinzufügen** -> **Neues Element** aus. 
2. Klicke im Dialogfeld **Neues Element hinzufügen** im linken Bereich auf **Web**, und wähle **XML-Datei (.xml)** aus. 
3. Gib der neuen Datei den Namen **app.manifest**, und klicke auf **Hinzufügen**.
4. Ersetze den Inhalt der Datei durch den folgenden XML-Ausschnitt. Hierdurch werden benutzerdefinierte Steuerelementtypen im Projekt **MyUWPApp** registriert.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly
     xmlns="urn:schemas-microsoft-com:asm.v1"
     xmlns:asmv3="urn:schemas-microsoft-com:asm.v3"
     manifestVersion="1.0">
      <asmv3:file name="MyUWPApp.dll">
        <activatableClass
            name="MyUWPApp.App"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.XamlMetadataProvider"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.MyUserControl"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
      </asmv3:file>
    </assembly>
    ```

## <a name="configure-additional-desktop-project-properties"></a>Konfigurieren zusätzlicher Eigenschaften für das Desktopprojekt

Als Nächstes aktualisierst du dein Projekt **MyDesktopWin32App**, um ein Makro für zusätzliche Includeverzeichnisse zu definieren und zusätzliche Eigenschaften zu konfigurieren.

1. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **MyDesktopWin32App** und dann auf **Projekt entladen**.

2. Klicke mit der rechten Maustaste auf **MyDesktopWin32App (Entladen)** , und wähle **MyDesktopWin32App.vcxproj bearbeiten** aus.

3. Füge den folgenden XML-Ausschnitt direkt vor dem schließenden `</Project>`-Tag am Ende der Datei ein. Speichere und schließe die Datei anschließend.

    ```xml
      <!-- Configure these for your UWP project -->
      <PropertyGroup>
        <AppProjectName>MyUWPApp</AppProjectName>
      </PropertyGroup>
      <PropertyGroup>
        <AppIncludeDirectories>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\;$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\Generated Files\;</AppIncludeDirectories>
      </PropertyGroup>
      <ItemGroup>
        <ProjectReference Include="..\$(AppProjectName)\$(AppProjectName).vcxproj" />
      </ItemGroup>
      <!-- End Section-->
    ```

4. Klicke im **Projektmappen-Explorer** mit der rechten Maustaste auf **MyDesktopWin32App (Entladen)** , und wähle **Projekt erneut laden** aus.

5. Klicke mit der rechten Maustaste auf **MyDesktopWin32App**, wähle **Eigenschaften** aus, und klicke im linken Bereich auf den Knoten **C/C++** . Vergewissere dich, dass das Makro **Additional Include Directories** über die im vorherigen Schritt durchgeführten Projektdateiänderungen definiert wurde.

    ![Einstellungen für C/C++-Projekt](images/xaml-islands/xaml-island-cpp-7.png)

6. Erweitere im Dialogfeld **Eigenschaftenseiten** die Einträge **Manifesttool** -> **Eingabe und Ausgabe**. Lege die Eigenschaft **DPI** auf **Hohe DPI-Werte pro Monitor** fest. Wenn du diese Eigenschaft nicht festlegst, kommt es in bestimmten Szenarien mit hohem DPI-Wert möglicherweise zu einem Manifestkonfigurationsfehler.

    ![Einstellungen für C/C++-Projekt](images/xaml-islands/xaml-island-cpp-8.png)

## <a name="host-the-custom-uwp-xaml-control-in-the-desktop-project"></a>Hosten des benutzerdefinierten UWP-XAML-Steuerelements im Desktopprojekt

Schließlich kannst du den Code zum Projekt **MyDesktopWin32App** hinzufügen, um das zuvor im Projekt **MyUWPApp** definierte benutzerdefinierte UWP-XAML-Steuerelement zu hosten.

1. Öffne im Projekt **MyDesktopWin32App** die Datei **framework.h**, und kommentiere die folgende Codezeile aus. Speichere die Datei anschließend.

    ```cpp
    #define WIN32_LEAN_AND_MEAN
    ```

2. Öffne die Datei **MyDesktopWin32App.h**, und ersetze deren Inhalte durch den folgenden Code, um auf erforderliche C++/WinRT-Headerdateien zu verweisen. Speichere die Datei anschließend.

    ```cpp
    #pragma once

    #include "resource.h"
    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>
    #include <winrt/Windows.UI.Core.h>
    #include <winrt/MyUWPApp.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI::Xaml::Controls;
    ```

3. Öffne die Datei **MyDesktopWin32App.cpp**, und füge den folgenden Code im Abschnitt `Global Variables:` ein.

    ```cpp
    winrt::MyUWPApp::App hostApp{ nullptr };
    winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource _desktopWindowXamlSource{ nullptr };
    winrt::MyUWPApp::MyUserControl _myUserControl{ nullptr };
    ```

4. Füge in derselben Datei den folgenden Code zum Abschnitt `Forward declarations of functions included in this code module:` hinzu.

    ```cpp
    void AdjustLayout(HWND);
    ```

5. Füge ebenfalls in derselben Datei den folgenden Code direkt nach dem `TODO: Place code here.`-Kommentar in der `wWinMain`-Funktion ein.

    ```cpp
    // TODO: Place code here.
    winrt::init_apartment(winrt::apartment_type::single_threaded);
    hostApp = winrt::MyUWPApp::App{};
    _desktopWindowXamlSource = winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource{};
    ```

6. Ersetze in derselben Datei die `InitInstance`-Standardfunktion durch den folgenden Code.

    ```cpp
    BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
    {
        hInst = hInstance; // Store instance handle in our global variable

        HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
            CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

        if (!hWnd)
        {
            return FALSE;
        }

        // Begin XAML Islands walkthrough code.
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            check_hresult(interop->AttachToWindow(hWnd));
            HWND hWndXamlIsland = nullptr;
            interop->get_WindowHandle(&hWndXamlIsland);
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(hWndXamlIsland, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
            _myUserControl = winrt::MyUWPApp::MyUserControl();
            _desktopWindowXamlSource.Content(_myUserControl);
        }
        // End XAML Islands walkthrough code.

        ShowWindow(hWnd, nCmdShow);
        UpdateWindow(hWnd);

        return TRUE;
    }
    ```

7. Füge in derselben Datei die folgende neue Funktion am Ende der Datei hinzu.

    ```cpp
    void AdjustLayout(HWND hWnd)
    {
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            HWND xamlHostHwnd = NULL;
            check_hresult(interop->get_WindowHandle(&xamlHostHwnd));
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(xamlHostHwnd, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
        }
    }
    ```

8. Suche in derselben Datei nach der `WndProc`-Funktion. Ersetze den `WM_DESTROY`-Standardhandler in der switch-Anweisung durch den folgenden Code.

    ```cpp
    case WM_DESTROY:
        PostQuitMessage(0);
        if (_desktopWindowXamlSource != nullptr)
        {
            _desktopWindowXamlSource.Close();
            _desktopWindowXamlSource = nullptr;
        }
        break;
    case WM_SIZE:
        AdjustLayout(hWnd);
        break;
    ```

9. Speichern Sie die Datei.
10. Kompiliere die Projektmappe und stelle sicher, dass der Vorgang erfolgreich war.

## <a name="add-a-control-from-the-winui-library-to-the-custom-control"></a>Hinzufügen eines Steuerelements aus der WinUI-Bibliothek zum benutzerdefinierten Steuerelement

Traditionell wurden UWP-Steuerelemente als Teil des Windows 10-Betriebssystems veröffentlicht und Entwicklern über das Windows SDK zur Verfügung gestellt. Die [WinUI-Bibliothek](/uwp/toolkits/winui/) ist ein alternativer Ansatz, bei dem aktualisierte Versionen von UWP-Steuerelementen aus dem Windows SDK in einem NuGet-Paket verteilt werden, das nicht an Windows SDK-Releases gebunden ist. Diese Bibliothek enthält darüber hinaus neue Steuerelemente, die nicht Teil des Windows SDK und der UWP-Standardplattform sind. Weitere Informationen finden Sie in der [Roadmap für die WinUI-Bibliothek](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md).

In diesem Abschnitt wird veranschaulicht, wie Sie dem Benutzersteuerelement ein UWP-Steuerelement aus der WinUI-Bibliothek hinzufügen, sodass Sie dieses Steuerelement in Ihrer WPF-App hosten können.

1. Installieren Sie im **MyUWPApp**-Projekt die neueste Vorabversion oder Releaseversion des [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)-NuGet-Pakets.

    > [!NOTE]
    > Wenn Ihre Desktop-App in einem [MSIX-Paket](/windows/msix) gepackt ist, können Sie entweder eine Vorabversion oder eine Releaseversion des [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)-NugGet-Pakets verwenden. Wenn Ihre Desktop-App nicht unter Verwendung von MSIX gepackt ist, müssen Sie eine Vorabversion des [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)-NuGet-Pakets installieren.

2. Fügen Sie der pch.h-Datei in diesem Projekt die folgenden `#include`-Anweisungen hinzu, und speichern Sie Ihre Änderungen. Diese Anweisungen holen einen erforderlichen Satz von Projektionsheadern aus der WinUI-Bibliothek in Ihr Projekt. Dieser Schritt ist für alle C++/WinRT-Projekte erforderlich, die die WinUI-Bibliothek verwenden. Weitere Informationen finden Sie in [diesem Artikel](/uwp/toolkits/winui/getting-started#additional-steps-for-a-cwinrt-project).

    ```cpp
    #include "winrt/Microsoft.UI.Xaml.Automation.Peers.h"
    #include "winrt/Microsoft.UI.Xaml.Controls.Primitives.h"
    #include "winrt/Microsoft.UI.Xaml.Media.h"
    #include "winrt/Microsoft.UI.Xaml.XamlTypeInfo.h"
    ```

3. Fügen Sie der App.xaml-Datei im selben Projekt dem `<xaml:XamlApplication>`-Element das folgende untergeordnete Element hinzu, und speichern Sie Ihre Änderungen.

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    Nachdem dieses Element hinzugefügt wurde, sollte der Inhalt dieser Datei in etwa wie folgt aussehen.

    ```xml
    <Toolkit:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyUWPApp">
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls"/>
        </Application.Resources>
    </Toolkit:XamlApplication>
    ```

4. Öffnen Sie im gleichen Projekt die MyUserControl.xaml-Datei, und fügen Sie dem `<UserControl>`-Element die folgende Namespacedeklaration hinzu.

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. Fügen Sie in derselben Datei ein `<winui:RatingControl />`-Element als untergeordnetes Element von `<StackPanel>` hinzu, und speichern Sie Ihre Änderungen. Dieses Element fügt eine Instanz der [RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol -Klasse aus der WinUI-Bibliothek hinzu. Nachdem dieses Element hinzugefügt wurde, sollte das `<StackPanel>` nun ungefähr wie hier aussehen:

    ```xml
    <StackPanel HorizontalAlignment="Center" Spacing="10" 
                Padding="20" VerticalAlignment="Center">
        <TextBlock HorizontalAlignment="Center" TextWrapping="Wrap" 
                       Text="Hello from XAML Islands" FontSize="30" />
        <TextBlock HorizontalAlignment="Center" Margin="15" TextWrapping="Wrap"
                       Text="😍❤💋🌹🎉😎�🐱‍👤" FontSize="16" />
        <Button HorizontalAlignment="Center" 
                x:Name="Button" Click="ClickHandler">Click Me</Button>
        <winui:RatingControl />
    </StackPanel>
    ```

6. Kompiliere die Projektmappe und stelle sicher, dass der Vorgang erfolgreich war.

## <a name="test-the-app"></a>Testen der App

Führe die Projektmappe aus, und vergewissere dich, dass **MyDesktopWin32App** mit dem folgenden Fenster geöffnet wird.

![MyDesktopWin32App-App](images/xaml-islands/xaml-island-cpp-9.png)

## <a name="next-steps"></a>Nächste Schritte

Viele Desktopanwendungen, die XAML Islands hosten, müssen zusätzliche Szenarien verarbeiten, um ein reibungsloses Benutzererlebnis zu gewährleisten. Beispielsweise müssen Desktopanwendungen möglicherweise Tastatureingaben in XAML Islands verarbeiten, den Navigationsfokus zwischen XAML Islands und anderen Benutzeroberflächenelementen verlagern und Layoutänderungen unterstützen.

Weitere Informationen zum Handhaben dieser Szenarien und Verweise auf entsprechende Codebeispiele findest du unter [Erweiterte Szenarien für XAML Islands in C++-Win32-Apps](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Zugehörige Themen

* [Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML Islands)](xaml-islands.md)
* [Verwenden der UWP-XAML-Hosting-API in einer C++-Win32-App](using-the-xaml-hosting-api.md)
* [Hosten eines UWP-Standardsteuerelements in einer C++-Win32-App](host-standard-control-with-xaml-islands-cpp.md)
* [Erweiterte Szenarien für XAML Islands in C++-Win32-Apps](advanced-scenarios-xaml-islands-cpp.md)
* [XAML Islands-Codebeispiele](https://github.com/microsoft/Xaml-Islands-Samples)