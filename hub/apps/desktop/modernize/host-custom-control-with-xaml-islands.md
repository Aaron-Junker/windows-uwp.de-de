---
description: Dieser Artikel veranschaulicht, wie Sie ein benutzerdefiniertes UWP-Steuerelement mithilfe von XAML Islands in einer WPF-App hosten.
title: Hosten eines benutzerdefinierten UWP-Steuerelements in einer WPF-App unter Verwendung von XAML Islands
ms.date: 01/24/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands, benutzerdefinierte Steuerelemente, Benutzersteuerelemente, Hosten von Steuerelementen
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 5f3e4eee486edd47901fc2b97a6e10c880cb04b1
ms.sourcegitcommit: 2571af6bf781a464a4beb5f1aca84ae7c850f8f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82606299"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Hosten eines benutzerdefinierten UWP-Steuerelements in einer WPF-App unter Verwendung von XAML Islands

Dieser Artikel veranschaulicht die Verwendung des [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)-Steuerelements im Windows Community Toolkit zum Hosten eines benutzerdefinierten UWP-Steuerelements in einer WPF-App, die auf .NET Core 3 ausgerichtet ist. Das benutzerdefinierte Steuerelement enthält verschiedene UWP-Originalsteuerelemente aus dem Windows SDK und bindet eine Eigenschaft eines der UWP-Steuerelemente an eine Zeichenfolge in der WPF-App. In diesem Artikel wird außerdem gezeigt, wie ein UWP-Steuerelement aus der [WinUI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/) gehostet wird.

Zwar wird in diesem Artikel das Vorgehen in einer WPF-App gezeigt, das Vorgehen für eine Windows Forms-App ist aber ähnlich. Eine Übersicht über das Hosten von UWP-Steuerelementen in WPF- und Windows Forms-Apps finden Sie in [diesem Artikel](xaml-islands.md#wpf-and-windows-forms-applications).

## <a name="required-components"></a>Erforderliche Komponenten

Um ein benutzerdefiniertes UWP-Steuerelement in einer WPF-App (oder einer Windows Forms-App) zu hosten, benötigen Sie die folgenden Komponenten in Ihrer Projektmappe. Dieser Artikel stellt Anweisungen zum Erstellen jeder dieser Komponenten bereit.

* **Projekt- und Quellcode für Ihre App**. Die Verwendung des [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)-Steuerelements zum Hosten benutzerdefinierter UWP-Steuerelemente wird nur in Apps für die Zielplattform .NET Core 3 unterstützt. Dieses Szenario wird in Apps für die Zielplattform .NET Framework nicht unterstützt.

* **Das benutzerdefinierte UWP-Steuerelement**. Sie benötigen den Quellcode für das benutzerdefinierte UWP-Steuerelement, das Sie hosten möchten, damit Sie es mit Ihrer Anwendung kompilieren können. In der Regel ist das benutzerdefinierte Steuerelement in einem UWP-Klassenbibliotheks-Projekt definiert, auf das Sie in der Projektmappe Ihres WPF- oder Windows Forms-Projekts verweisen.

* **UWP-App-Projekt mit Definition einer Stammanwendungsklasse, die von XamlApplication abgeleitet ist**. Dein WPF- oder Windows Forms-Projekt muss Zugriff auf eine Instanz der [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)-Klasse haben, die durch das Windows-Community-Toolkit bereitgestellt wird, sodass benutzerdefinierte UWP XAML-Steuerelemente ermittelt und geladen werden können. Die empfohlene Vorgehensweise hierfür besteht darin, dieses Objekt in einem separaten UWP-App-Projekt zu definieren, das nicht Bestandteil der Projektmappe für Ihre WPF- oder Windows Forms-App ist. 

    > [!NOTE]
    > Deine Projektmappe kann nur ein Projekt enthalten, das ein `XamlApplication`-Objekt definiert. Alle benutzerdefinierten UWP-Steuerelemente in Ihrer App nutzen gemeinsam dasselbe `XamlApplication`-Objekt. Das Projekt, in dem das `XamlApplication`-Objekt definiert ist, muss Verweise auf alle weiteren UWP-Bibliotheken und -Projekte enthalten, die zum Hosten von UWP-Steuerelementen in XAML Islands verwendet werden.

## <a name="create-a-wpf-project"></a>Erstellen eines WPF-Projekts

Befolgen Sie vor dem Einstieg diese Anweisungen, um ein WPF-Projekt zu erstellen und es zum Hosting in XAML Islands zu konfigurieren. Wenn Sie über ein vorhandenes WPF-Projekt verfügen, können Sie diese Schritte und Codebeispiele für Ihr Projekt anpassen.

> [!NOTE]
> Wenn Sie über ein vorhandenes Projekt verfügen, das auf .NET Framework abzielt, müssen Sie Ihr Projekt zu .NET Core 3 migrieren. Weitere Informationen finden Sie in [dieser Reihe von Blogbeiträgen](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

1. Wenn dies noch nicht geschehen ist installieren Sie die neueste Version von [.NET Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. Erstellen Sie in Visual Studio 2019 ein neues **WPF-App (.NET Core)** -Projekt.

3. Vergewissern Sie sich, dass [Paketverweise](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) aktiviert sind:

    1. Klicken Sie in Visual Studio auf **Extras > NuGet-Paket-Manager > Paket-Manager-Einstellungen**.
    2. Stellen Sie sicher, dass die Einstellung **Standardformat für Paketverwaltung** auf **PackageReference** festgelegt ist.

4. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf Ihr WPF-Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.

5. Stellen Sie sicher, dass im Fenster **NuGet-Paket-Manager** die Einstellung **Vorabversion einbeziehen** ausgewählt ist.

6. Wählen die Registerkarte **Durchsuchen** aus, suchen Sie nach dem Paket [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (Version v6.0.0 oder höher), und installieren Sie das Paket. Dieses Paket enthält alles, was Sie benötigen, um das **WindowsXamlHost**-Steuerelement zum Hosten eines UWP-Steuerelements zu verwenden, einschließlich weiterer zugehöriger NuGet-Pakete.
    > [!NOTE]
    > Windows Forms-Apps müssen das Paket [Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) verwenden (Version 6.0.0 oder höher).

7. Konfigurieren Sie Ihre Projektmappe für eine Zielplattform, beispielsweise x86 oder x64. Benutzerdefinierte UWP-Steuerelemente werden nicht in Projekten unterstützt, für die als Ziel **Beliebige CPU** festgelegt ist.

    1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappenknoten, und wählen Sie **Eigenschaften** -> **Konfigurationseigenschaften** -> **Konfigurations-Manager** aus.
    2. Klicken Sie unter **Aktive Projektmappenplattform** auf **Neu**. 
    3. Wählen Sie im Dialogfeld **Neue Projektmappenplattform** die Option **x64** oder **x86** aus, und klicken Sie auf **OK**. 
    4. Schließen Sie die geöffneten Dialogfelder.

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>Definieren einer XamlApplication-Klasse in einem UWP-App-Projekt

Als Nächstes fügen Sie Ihrer Projektmappe ein UWP-App-Projekt hinzu und überarbeiten die `App`-Standardklasse in diesem Projekt, um sie von der [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)-Klasse abzuleiten, die im Windows-Community-Toolkit bereitgestellt wird. Diese Klasse unterstützt die [IXamlMetadaraProvider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider)-Schnittstelle, mit der deine App Metadaten für benutzerdefinierte UWP-XAML-Steuerelemente in Assemblys im aktuellen Verzeichnis der Anwendung zur Laufzeit ermitteln und laden kann. Diese Klasse initialisiert außerdem das UWP-XAML-Framework für den aktuellen Thread. 

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappenknoten, und wählen Sie **Hinzufügen** -> **Neues Projekt** aus.
2. Fügen Sie Ihrer Projektmappe ein Projekt vom Typ **Leere App (Universal Windows)** hinzu. Vergewissern Sie sich, dass sowohl die Zielversion als auch die mindestens erforderliche Version auf **Windows 10, Version 1903** oder höher festgelegt ist.
3. Installieren Sie im UWP-App-Projekt das NuGet-Paket [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (Version 6.0.0 oder höher).
4. Öffnen Sie die Datei **App.xaml**, und ersetzen Sie deren Inhalte durch den nachstehenden XAML-Ausschnitt. Ersetzen Sie `MyUWPApp` durch den Namespace Ihres UWP-App-Projekts.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. Öffnen Sie die Datei **App.xaml.cs**, und ersetzen Sie die Inhalte dieser Datei durch den nachstehenden Code. Ersetzen Sie `MyUWPApp` durch den Namespace Ihres UWP-App-Projekts.

    ```csharp
    namespace MyUWPApp
    {
        public sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. Löschen Sie die Datei **MainPage.xaml** aus dem UWP-App-Projekt.
7. Bereinigen Sie das UWP-App-Projekt, und erstellen Sie es.
8. Klicken Sie in Ihrem WPF-Projekt mit der rechten Maustaste auf den Knoten **Abhängigkeiten**, und fügen Sie einen Verweis auf Ihr UWP-App-Projekt hinzu.

## <a name="instantiate-the-xamlapplication-object-in-the-entry-point-of-your-wpf-app"></a>Instanziieren des XamlApplication-Objekts im Einstiegspunkt Ihrer WPF-App

Als Nächstes fügst du Code zum Einstiegspunkt für deine WPF-App hinzu, um eine Instanz der `App`-Klasse zu erstellen, die du soeben im UWP-Projekt definiert hast (dies ist die Klasse, die jetzt von `XamlApplication` abgeleitet wird).

1. Klicke in deinem WPF-Projekt mit der rechten Maustaste auf den Projektknoten, wähle **Hinzufügen** -> **Neues Element** und dann **Klasse** aus. Geben Sie der Klasse den Namen **Program**, und klicken Sie auf **Hinzufügen**.

2. Ersetzen Sie die generierte `Program`-Klasse durch den folgenden Code, und speichern Sie dann die Datei. Ersetzen Sie `MyUWPApp` durch den Namespace Ihres UWP-App-Projekts und `MyWPFApp` durch den Namespace Ihres WPF-App-Projekts.

    ```csharp
    public class Program
    {
        [System.STAThreadAttribute()]
        public static void Main()
        {
            using (new MyUWPApp.App())
            {
                MyWPFApp.App app = new MyWPFApp.App();
                app.InitializeComponent();
                app.Run();
            }
        }
    }
    ```

3. Klicken Sie mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Eigenschaften** aus.

4. Klicken Sie auf der Registerkarte **Anwendung** der Eigenschaften auf die Dropdownliste **Startobjekt**, und wählen Sie den vollqualifizierten Namen der `Program`-Klasse aus, die Sie im vorherigen Schritt hinzugefügt haben. 
    > [!NOTE]
    > Standardmäßig definieren WPF-Projekte eine `Main`-Einstiegspunktfunktion in einer generierten Codedatei, die nicht zur Bearbeitung vorgesehen ist. Durch diesen Schritt wird der Einstiegspunkt für Ihr Projekt in die `Main`-Methode der neuen `Program`-Klasse geändert, wodurch Sie Code hinzufügen können, der so früh wie möglich in der Startphase der App ausgeführt wird. 

5. Speichern Sie Ihre Änderungen an den Projekteigenschaften.

## <a name="create-a-custom-uwp-control"></a>Erstellen eines benutzerdefinierten UWP-Steuerelements

Zum Hosten eines benutzerdefinierten UWP-Steuerelements in Ihrer WPF-App müssen Sie über den Quellcode des Steuerelements verfügen, damit Sie ihn mit Ihrer App kompilieren können. Normalerweise sind benutzerdefinierte Steuerelemente zwecks leichter Portierbarkeit in einem UWP-Klassenbibliotheksprojekt definiert.

In diesem Abschnitt definieren Sie ein einfaches benutzerdefiniertes UWP-Steuerelement in einem neuen Klassenbibliotheksprojekt. Alternativ können Sie das benutzerdefinierte UWP-Steuerelement im UWP-App-Projekt definieren, das Sie im vorherigen Abschnitt erstellt haben. Diese Schritte führen dies jedoch der Anschaulichkeit halber in einem separaten Klassenbibliotheksprojekt aus, da dies aus Gründen der Portierbarkeit die typische Implementierung von benutzerdefinierten Steuerelementen darstellt.

Wenn Sie bereits über ein benutzerdefiniertes Steuerelement verfügen, können Sie es anstelle des hier gezeigten Steuerelements verwenden. Sie müssen jedoch trotzdem das Projekt konfigurieren, das das Steuerelement enthält, wie in den folgenden Schritten gezeigt.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektmappenknoten, und wählen Sie **Hinzufügen** -> **Neues Projekt** aus.
2. Fügen Sie Ihrer Projektmappe ein Projekt **Klassenbibliothek (Universal Windows)** hinzu. Vergewissern Sie sich, dass sowohl die Zielversion als auch die mindestens erforderliche Version auf **Windows 10, Version 1903** oder höher festgelegt ist.
3. Klicken Sie mit der rechten Maustaste auf die Projektdatei, und wählen Sie dann **Projekt entladen** aus. Klicken Sie mit der rechten Maustaste erneut auf die Projektdatei, und wählen Sie **Bearbeiten** aus.
4. Fügen Sie vor dem schließenden `</Project>`-Element den folgenden XML-Code hinzu, um mehrere Eigenschaften zu deaktivieren, und speichern Sie dann die Projektdatei. Diese Eigenschaften müssen aktiviert sein, um das benutzerdefinierte UWP-Steuerelement in einer WPF-App (oder Windows Forms-App) zu hosten.

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. Klicken Sie mit der rechten Maustaste auf die Projektdatei, und wählen Sie dann **Projekt erneut laden** aus.
6. Löschen Sie die Standarddatei **Class1.cs**, und fügen Sie dem Projekt ein neues Element **Benutzersteuerelement** hinzu.
7. Fügen Sie in der XAML-Datei für das Benutzersteuerelement das folgende `StackPanel` als untergeordnetes Element des standardmäßigen `Grid` hinzu. In diesem Beispiel wird ein ``TextBlock``-Steuerelement hinzugefügt und dann das ``Text``-Attribut dieses Steuerelements an das ``XamlIslandMessage``-Feld gebunden.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. Fügen Sie in der Code-Behind-Datei des Benutzersteuerelements der Benutzersteuerelement-Klasse das Feld `XamlIslandMessage` hinzu, wie unten dargestellt.

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

9. Erstellen Sie das UWP-Klassenbibliotheksprojekt.
10. Klicken Sie in Ihrem WPF-Projekt mit der rechten Maustaste auf den Knoten **Abhängigkeiten**, und fügen Sie einen Verweis auf das UWP-Klassenbibliotheksprojekt hinzu.
11. Klicken Sie im zuvor konfigurierten UWP-App-Projekt mit der rechten Maustaste auf den Knoten **Verweise**, und fügen Sie einen Verweis auf das UWP-Klassenbibliotheksprojekt hinzu.
12. Erstellen Sie die gesamte Projektmappe erneut, und vergewissern Sie sich, dass alle Projekte erfolgreich erstellt werden.

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>Hosten des benutzerdefinierten UWP-Steuerelements in Ihrer WPF-App

1. Erweitern Sie im **Projektmappen-Explorer** das WPF-Projekt, und öffnen Sie die Datei „MainWindow.xaml“ oder ein anderes Fenster, in dem Sie das benutzerdefinierte Steuerelement hosten möchten.
2. Fügen Sie in der XAML-Datei dem `<Window>`-Element die folgende Namespacedeklaration hinzu.

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. Fügen Sie in derselben Datei dem `<Grid>`-Element das folgende Steuerelement hinzu. Ändern Sie das `InitialTypeName`-Attribut in den vollqualifizierten Namen des Benutzersteuerelements in Ihrem UWP-Klassenbibliotheksprojekt.

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. Öffnen Sie die Code-Behind-Datei, und fügen Sie der Klasse `Window` folgenden Code hinzu. Dieser Code definiert einen `ChildChanged`-Ereignishandler, der den Wert des ``XamlIslandMessage``-Felds des benutzerdefinierten UWP-Steuerelements dem Wert des `WPFMessage`-Felds in der WPF-App zuweist. Ändern Sie `UWPClassLibrary.MyUserControl` in den vollqualifizierten Namen des Benutzersteuerelements in Ihrem UWP-Klassenbibliotheksprojekt.

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

6. Erstellen Sie Ihre App, führen Sie sie aus, und vergewissern Sie sich, dass das UWP-Benutzersteuerelement wie erwartet angezeigt wird.

## <a name="add-a-control-from-the-winui-library-to-the-custom-control"></a>Hinzufügen eines Steuerelements aus der WinUI-Bibliothek zum benutzerdefinierten Steuerelement

Traditionell wurden UWP-Steuerelemente als Teil des Windows 10-Betriebssystems veröffentlicht und Entwicklern über das Windows SDK zur Verfügung gestellt. Die [WinUI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/) ist ein alternativer Ansatz, bei dem aktualisierte Versionen von UWP-Steuerelementen aus dem Windows SDK in einem NuGet-Paket verteilt werden, das nicht an Windows SDK-Releases gebunden ist. Diese Bibliothek enthält darüber hinaus neue Steuerelemente, die nicht Teil des Windows SDK und der UWP-Standardplattform sind. Weitere Informationen finden Sie in der [Roadmap für die WinUI-Bibliothek](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md).

In diesem Abschnitt wird veranschaulicht, wie Sie dem Benutzersteuerelement ein UWP-Steuerelement aus der WinUI-Bibliothek hinzufügen, sodass Sie dieses Steuerelement in Ihrer WPF-App hosten können.

1. Installieren Sie im UWP-App-Projekt die neueste Releaseversion oder Vorabversion des [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)-NuGet-Pakets.

    > [!NOTE]
    > Wenn Ihre Desktop-App in einem [MSIX-Paket](https://docs.microsoft.com/windows/msix) gepackt ist, können Sie entweder eine Vorabversion oder eine Releaseversion des [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)-NugGet-Pakets verwenden. Wenn Ihre Desktop-App nicht unter Verwendung von MSIX gepackt ist, müssen Sie eine Vorabversion des [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)-NuGet-Pakets installieren.

2. Fügen Sie der App.xaml-Datei dieses Projekts dem `<xaml:XamlApplication>`-Element das folgende untergeordnete Element hinzu.

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    Nachdem dieses Element hinzugefügt wurde, sollte der Inhalt dieser Datei in etwa wie folgt aussehen.

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

3. Installieren Sie im UWP-Klassenbibliotheksprojekt die neueste Version des [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)-NuGet-Pakets (die gleiche Version, die Sie im UWP-App-Projekt installiert haben).

4. Öffnen Sie im gleichen Projekt die XAML-Datei für das Benutzersteuerelement, und fügen Sie dem `<UserControl>`-Element die folgende Namespacedeklaration hinzu.

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. Fügen Sie in derselben Datei ein `<winui:RatingControl />`-Element als untergeordnetes Element von `<StackPanel>` hinzu. Dieses Element fügt eine Instanz der [RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol)-Klasse aus der WinUI-Bibliothek hinzu. Nachdem dieses Element hinzugefügt wurde, sollte das `<StackPanel>` nun ungefähr wie hier aussehen:

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
        <winui:RatingControl />
    </StackPanel>
    ```

6. Erstellen Sie Ihre App, führen Sie sie aus, und vergewissern Sie sich, dass das neue Bewertungsssteuerelenent wie erwartet angezeigt wird.

## <a name="package-the-app"></a>Verpacken der App

Sie können die WPF-App optional in einem [MSIX-Paket](https://docs.microsoft.com/windows/msix) für die Bereitstellung verpacken. MSIX ist eine moderne App-Pakettechnologie für Windows, die auf einer Kombination aus MSI-, APPX-, App-V- und ClickOnce-Installationstechnologien basiert.

Die folgenden Anweisungen veranschaulichen, wie Sie alle Komponenten in der Projektmappe in ein MSIX-Paket einschließen können, indem Sie die Option [Paketerstellungsprojekt für Windows-Anwendungen](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) in Visual Studio 2019 verwenden. Diese Schritte sind nur erforderlich, wenn Sie die WPF-App in einem MSIX-Paket verpacken möchten. Beachten Sie, dass diese Schritte derzeit einige Problemumgehungen beinhalten, die für das Szenario des Hostens benutzerdefinierter UWP-Steuerelemente spezifisch sind.

> [!NOTE]
> Wenn Sie sich entscheiden, Ihre Anwendung nicht für die Bereitstellung in einem [MSIX-Paket](https://docs.microsoft.com/windows/msix) zu verpacken, muss auf Computern zur Ausführung Ihrer App die [Visual C++-Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) installiert sein.

1. Fügen Sie Ihrer Projektmappe ein [Paketerstellungsprojekt für Windows-Anwendungen](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) hinzu. Wählen Sie beim Erstellen des Projekts **Windows 10, Version 1903 (10.0; Build 18362)** sowohl für **Zielversion** als auch für **Mindestversion** aus.

2. Klicken Sie im Paketprojekt mit der rechten Maustaste auf den Knoten **Anwendungen**, und wählen Sie **Verweis hinzufügen** aus. Wählen Sie in der Liste der Projekte das WPF-Projekt in Ihrer Projektmappe aus, und klicken Sie auf **OK**.

3. Bearbeiten Sie die WPF-Projektdatei. Diese Änderungen sind aktuell zum Verpacken von WPF-Apps erforderlich, die benutzerdefinierte UWP-Steuerelemente hosten.

    1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den WPF-Projektknoten, und wählen Sie **Projekt entladen** aus.
    2. Klicken Sie mit der rechten Maustaste auf den WPF-Projektknoten, und wählen Sie **Bearbeiten** aus.
    3. Suchen Sie nach dem letzten `</PropertyGroup>`-Endtag in der Datei, und fügen Sie den folgenden XML-Code unmittelbar hinter diesem Tag ein.

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. Speichere die Projektdatei und schließe sie dann.
    5. Klicken Sie mit der rechten Maustaste auf den WPF-Projektknoten, und wählen Sie **Projekt erneut laden** aus.

4. Kompilieren Sie das Paketerstellungsprojekt, und führen Sie es aus. Vergewissern Sie sich, dass die WPF-App ausgeführt und das benutzerdefinierte UWP-Steuerelement wie erwartet angezeigt wird.

## <a name="related-topics"></a>Zugehörige Themen

* [Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML Islands)](xaml-islands.md)
* [XAML Islands-Codebeispiele](https://github.com/microsoft/Xaml-Islands-Samples)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
