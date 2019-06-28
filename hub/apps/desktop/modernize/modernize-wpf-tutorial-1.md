---
description: Dieses Tutorial veranschaulicht das Hinzufügen von UWP XAML-Benutzeroberflächen, MSIX-Pakete erstellen und andere moderne Komponenten in Ihrer WPF-Anwendung integrieren.
title: Migrieren von Contoso Ausgaben-app auf .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, Uwp, Windows Forms, Wpf, XAML-Inseln
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: e718de7a22873ccf347e60c661f724ce3abdd2cf
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420131"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>Teil 1: Migrieren von Contoso Ausgaben-app auf .NET Core 3

Dies ist der erste Teil eines Tutorials, die zeigt, wie Sie eine Beispiel WPF-desktop-app mit dem Namen Contoso-Ausgaben zu modernisieren. Eine Übersicht über das Tutorial, Voraussetzungen und Anweisungen zum Herunterladen der Beispiel-app, finden Sie unter [Lernprogramm: Modernisieren von WPF-app](modernize-wpf-tutorial.md).
  
In diesem Teil des Tutorials, migrieren Sie die gesamte Contoso-Ausgaben-app von .NET Framework 4.7.2 [.NET Core 3](modernize-wpf-tutorial.md#net-core-3). Bevor Sie diesen Teil des Tutorials beginnen, stellen Sie sicher, dass Sie führen Sie Folgendes:

* [Öffnen Sie ein, und erstellen Sie das Beispiel ContosoExpenses](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app) in Visual Studio-2019.
* Wenn Sie eine Version von Visual Studio-2019 verwenden, aktivieren Sie die Preview-Versionen von .NET Core SDK. Wechseln Sie in Visual Studio zu **Tools > Optionen**"Preview" in das Suchfeld, und wählen Sie **verwenden Sie die Preview-Versionen von .NET Core SDK**. Bei Verwendung einer [Preview-Version von Visual Studio-2019](https://visualstudio.microsoft.com/vs/preview/), müssen Sie nicht diese Option auswählen, da .NET Core Preview-Versionen sind standardmäßig aktiviert.

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>Migrieren Sie das Projekt ContosoExpenses zu .NET Core-3

In diesem Abschnitt müssen Sie die ContosoExpenses-Projekts in der Contoso-Ausgaben-app auf .NET Core-3 migrieren. Dazu müssen Sie erstellen eine neue Projektdatei, die die gleichen Dateien wie dem vorhandenen ContosoExpenses-Projekt, aber .NET Core-3-Ziele anstelle von .NET Framework 4.7.2 enthält. Dadurch können Sie zu einem einzigen Projekt mit .NET Framework und .NET Core-Versionen der app zu verwalten.

1. Stellen Sie sicher, dass die ContosoExpenses derzeit Ziel .NET Framework 4.7.2 Projekt. Im Projektmappen-Explorer mit der Maustaste der **ContosoExpenses** Projekts **Eigenschaften**, und überprüfen Sie, ob die **Zielframework** Eigenschaft der  **Anwendung** Registerkarte auf .NET Framework 4.7.2 festgelegt ist.

    ![.NET Framework Version 4.7.2 für das Projekt](images/wpf-modernize-tutorial/NETFramework472.png)

3. Navigieren Sie im Windows-Explorer zu dem **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** Ordner, und erstellen Sie eine neue Textdatei mit dem Namen **ContosoExpenses.Core.csproj**.

4. Klicken Sie mit der rechten Maustaste auf die Datei, wählen Sie **Öffnen mit**, und klicken Sie dann in einem Text-Editor Ihrer Wahl, z. B. Editor, Visual Studio Code oder Visual Studio öffnen.

5. Kopieren Sie den folgenden Text in die Datei, und speichern Sie sie.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. Die Datei schließen und zurück zu den **ContosoExpenses** Projektmappe in Visual Studio.

7. Mit der rechten Maustaste die **ContosoExpenses** Lösung, und wählen Sie **hinzufügen -> vorhandenes Projekt**. Wählen Sie die **ContosoExpenses.Core.csproj** Datei, die Sie gerade erstellt haben, der `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` Ordner, um es zur Projektmappe hinzuzufügen.

Die **ContosoExpenses.Core.csproj** enthält die folgenden Elemente:

* Die **Projekt** Element gibt an, eine SDK-Version von **Microsoft.NET.Sdk.WindowsDesktop**. Dies bezieht sich auf Anwendungen von .NET für Windows Desktop, und sie enthält Komponenten für WPF und Windows Forms-apps.
* Die **PropertyGroup** Element enthält untergeordnete Elemente, die angeben, die Projektausgabe ist eine ausführbare Datei (nicht in eine DLL), .NET Core 3 abzielt und WPF verwendet. Verwenden Sie für eine Windows Forms-app eine **UseWinForms** -Element anstelle des dem **UseWPF** Element.

> [!NOTE]
> Bei der Arbeit mit csproj-Format mit .NET Core 3.0 eingeführt wurde, gelten alle Dateien im selben Ordner wie der CSPROJ-Datei Teil des Projekts. Aus diesem Grund müssen Sie jede Datei im Projekt enthaltenen angeben. Sie müssen angeben, dass nur die Dateien, die für die Sie eine benutzerdefinierte Aktion oder die Sie definieren möchten ausschließen möchten.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>Migrieren Sie das ContosoExpenses.Data-Projekt auf .NET Standard

Die **ContosoExpenses** Lösung umfasst eine **ContosoExpenses.Data** Klassenbibliothek, die Modelle und -Schnittstellen für Dienste enthält, und ist auf .NET 4.7.2. .NET Core-3.0-apps können .NET Framework-Bibliotheken verwenden, solange Sie nicht diese APIs verwenden, die in .NET Core nicht verfügbar sind. Ist jedoch der beste Migrationspfad für die Modernisierung Ihrer Bibliotheken in .NET Standard zu verschieben. Dadurch wird sichergestellt, dass Ihre Bibliothek vollständig von Ihrer app von .NET Core 3.0 unterstützt wird. Darüber hinaus können Sie die Bibliothek auch mit anderen Plattformen wie Web (über ASP.NET Core) und mobile Geräte (über Xamarin) wiederverwenden.

Migrieren der **ContosoExpenses.Data** Projekt auf .NET Standard:

1. In Visual Studio mit der Maustaste der **ContosoExpenses.Data** Projekt, und wählen **Projekt entladen**. Mit der rechten Maustaste erneut auf des Projekts, und wählen Sie dann **bearbeiten ContosoExpenses.Data.csproj**.

2. Löschen Sie den gesamten Inhalt der Projektdatei an.

3. Kopieren Sie und fügen Sie den folgenden XML-Code, und speichern Sie die Datei.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. Mit der rechten Maustaste die **ContosoExpenses.Data** Projekt, und wählen **Projekt erneut laden**.

## <a name="configure-nuget-packages-and-dependencies"></a>Konfigurieren von NuGet-Pakete und Abhängigkeiten

Wenn Sie migriert die **ContosoExpenses.Core** und **ContosoExpenses.Data** Projekte in den vorherigen Abschnitten haben Sie die NuGet-Paketverweise aus den Projekten entfernt. In diesem Abschnitt fügen Sie diese Verweise wieder.

So konfigurieren Sie die NuGet-Pakete für die **ContosoExpenses.Data** Projekt:

1.  In der **ContosoExpenses.Data** projizieren, erweitern Sie die **Abhängigkeiten** Knoten. Beachten Sie, dass die **NuGet** Abschnitt nicht vorhanden ist.

    ![NuGet-Pakete](images/wpf-modernize-tutorial/NuGetPackages.png)

    Wenn Sie öffnen die **"Packages.config"** in die **Projektmappen-Explorer** finden Sie 'ALTER' Verweise auf die NuGet-Pakete verwendet das Projekt aus, wenn sie das vollständige .NET Framework verwendet wurde.

    ![Abhängigkeiten und Pakete](images/wpf-modernize-tutorial/Packages.png)

    Hier ist der Inhalt der **"Packages.config"** Datei. Sie werden feststellen, dass alle NuGet-Pakete im vollständigen .NET Framework 4.7.2 als Ziel:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. In der **ContosoExpenses.Data** Projekt, löschen Sie die **"Packages.config"** Datei.

4. In der **ContosoExpenses.Data** Projekt der rechten Maustaste auf die **Abhängigkeiten** Knoten, und wählen Sie **NuGet-Pakete verwalten**.

  ![Verwalten Sie NuGet-Pakete...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. In der **NuGet Package Manager** Fenster, klicken Sie auf **Durchsuchen**. Suchen Sie nach der `Bogus` Packen und installieren Sie es.

    ![Gefälschte NuGet-Paket](images/wpf-modernize-tutorial/Bogus.png)

6. Suchen Sie nach der `LiteDB` Packen und installieren Sie es.

    ![LiteDB NuGet-Paket](images/wpf-modernize-tutorial/LiteDB.png)

    Sie Fragen sich vielleicht, wo dieser Liste von NuGet-Pakete gespeichert ist, da das Projekt nicht mehr als eine Datei "Packages.config" enthält. Die referenzierten NuGet-Pakete werden direkt in die CSPROJ-Datei gespeichert. Sie können dies überprüfen, indem der Inhalt von der **ContosoExpenses.Data.csproj** Projektdatei in einem Text-Editor. Sie finden die folgenden Zeilen am Ende der Datei hinzugefügt:

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > Sie können auch bemerken, dass Sie die gleichen Pakete für das .NET Core-3-Projekt wie die von .NET Framework 4.7.2-Projekten verwendet wird, installieren. NuGet-Pakete unterstützt die Festlegung von Zielversionen. Autoren von Klassenbibliotheken zählen verschiedene Versionen einer Bibliothek im selben Paket, für verschiedene Architekturen und -Plattformen kompiliert. Diese Pakete unterstützen die Vollversion von .NET Framework sowie .NET Standard 2.0, die mit .NET Core-3-Projekte kompatibel ist. Weitere Informationen zu den unterschieden, die .NET Framework, .NET Core und .NET Standard finden Sie unter [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

So konfigurieren Sie die NuGet-Pakete für die **ContosoExpenses.Core** Projekt:

1. In der **ContosoExpenses.Core** -Projekt, öffnen die **"Packages.config"** Datei. Beachten Sie, dass sie derzeit die folgenden Verweise enthält, die .NET Framework 4.7.2 als Ziel.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    In den folgenden Schritten müssen Sie .NET Standard-Versionen, der die `MvvmLightLibs` und `Unity` Pakete. Die anderen zwei sind Abhängigkeiten von NuGet automatisch heruntergeladen, wenn Sie diese beiden Bibliotheken installieren.

2. In der **ContosoExpenses.Core** Projekt, löschen Sie die **"Packages.config"** Datei.

3. Mit der rechten Maustaste die **ContosoExpenses.Core** Projekt, und wählen **NuGet-Pakete verwalten**.

4. In der **NuGet Package Manager** Fenster, klicken Sie auf **Durchsuchen**. Suchen Sie nach der `Unity` Packen und installieren Sie es.

    ![Unity-Pakets](images/wpf-modernize-tutorial/UnityPackage.png)

5. Suchen Sie nach der `MvvmLightLibsStd10` Packen und installieren Sie es. Dies ist die .NET Standard-Version von der `MvvmLightLibs` Paket. Für dieses Paket wurde der Autor die Version der Bibliothek in ein separates Paket als .NET Framework-Version von .NET Standard-Paket.

    ! MvvmLightsLibs-Paket[](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. In der **ContosoExpenses.Core** Projekt der rechten Maustaste auf die **Abhängigkeiten** Knoten, und wählen Sie **Verweis hinzufügen**.

7. In der **Projekte > Projektmappe** Kategorie **ContosoExpenses.Data** , und klicken Sie auf **OK**.

    ![Verweis hinzufügen](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>Deaktivieren von automatisch generierten Assemblynamen-Attribute

An diesem Punkt im Migrationsvorgang, wenn Sie versuchen, Sie erstellen die **ContosoExpenses.Core** Projekt werden einige Fehler angezeigt.

![Neue .NET Core-3-Buildfehler](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

Dieses Problem wird durchgeführt, da das neue csproj-Format mit .NET Core 3.0 speichert die Assemblyinformationen in der Projektdatei statt der **"AssemblyInfo.cs"** Datei. Um diese Fehler zu beheben, dieses Verhalten deaktivieren, und lassen Sie das Projekt weiterhin verwenden, die **"AssemblyInfo.cs"** Datei.

1. In Visual Studio mit der Maustaste der **ContosoExpenses.Core** Projekt, und wählen **Projekt entladen**. Mit der rechten Maustaste erneut auf des Projekts, und wählen Sie dann **bearbeiten ContosoExpenses.Core.csproj**.

1. Fügen Sie das folgende Element in der **PropertyGroup** aus, und speichern Sie die Datei.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Nach dem Hinzufügen dieses Elements, das **PropertyGroup** Abschnitt sollte jetzt wie folgt aussehen:

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. Mit der rechten Maustaste die **ContosoExpenses.Core** Projekt, und wählen **Projekt erneut laden**.

4. Mit der rechten Maustaste die **ContosoExpenses.Data** Projekt, und wählen **Projekt entladen**. Mit der rechten Maustaste erneut auf des Projekts, und wählen Sie dann **bearbeiten ContosoExpenses.Data.csproj**.

5. Fügen Sie den gleichen Eintrag in der **PropertyGroup** aus, und speichern Sie die Datei.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Nach dem Hinzufügen dieses Elements, das **PropertyGroup** Abschnitt sollte jetzt wie folgt aussehen:

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. Mit der rechten Maustaste die **ContosoExpenses.Data** Projekt, und wählen **Projekt erneut laden**.

## <a name="add-the-windows-compatibility-pack"></a>Fügen Sie das Windows Compatibility Pack

Wenn Sie nun versuchen, Sie kompilieren die **ContosoExpenses.Core** und **ContosoExpenses.Data** -Projekten werden Sie feststellen, dass die vorherigen Fehler sind jetzt behoben, aber es noch einige Fehler in gibt der  **ContosoExpenses.Data** Bibliothek, die ungefähr wie folgt aussehen.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

Diese Fehler sind das Ergebnis der Konvertierung der **ContosoExpenses.Data** Projekt aus einer .NET Framework-Bibliothek (die speziell für Windows), .NET Standard-Bibliothek, die auf mehreren Plattformen, einschließlich Linux, Android, iOS ausgeführt werden können, und vieles mehr. Die **ContosoExpenses.Data** -Projekt enthält eine Klasse namens **RegistryService**, die Interaktion mit der Registrierung ein nur-Windows-Konzept.

Um diese Fehler zu beheben, installieren die [Windows Compatibility](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) NuGet-Paket. Dieses Paket bietet Unterstützung für viele Windows-spezifische APIs in .NET Standard-Bibliothek verwendet werden. Die Bibliothek werden nicht mehr plattformübergreifende, nach dem verwenden dieses Paket, sondern immer noch .NET Standard als Ziel wird. 

1. Mit der rechten Maustaste auf die **ContosoExpenses.Data** Projekt.
2. Wählen Sie **NuGet-Pakete verwalten**.
3. In der **NuGet Package Manager** Fenster, klicken Sie auf **Durchsuchen**. Suchen Sie nach der `Microsoft.Windows.Compatibility` Packen und installieren Sie es. 

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. Jetzt wiederholen, um das Projekt zu kompilieren, indem Sie mit der rechten Maustaste auf die **ContosoExpenses.Data** Projekt- und Auswahl **erstellen**.

Dieses Mal wird der Buildprozess ohne Fehler abgeschlossen.

## <a name="test-and-debug-the-migration"></a>Testen Sie und Debuggen Sie die migration

Nun, dass die Projekte erfolgreich erstellen, können Sie zum Ausführen und testen die app, um festzustellen, ob alle Laufzeitfehler.

1. Mit der rechten Maustaste die **ContosoExpenses.Core** Projekt, und wählen **als Startprojekt festlegen**.

2. Drücken Sie F5, um die **ContosoExpenses.Core** Projekt im Debugger. Es wird eine Ausnahme, die etwa wie folgt angezeigt.

    ![Ausnahme in Visual Studio angezeigt](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    Diese Ausnahme ausgelöst wird, da Wenn Sie den Inhalt in die CSPROJ-Datei, die zu Beginn der Migration gelöscht, die Informationen über entfernt die **Buildvorgang** für die Bilddateien. Dieses Problem beheben, die folgenden Schritte aus.

3. Beenden Sie den Debugger an.

4. Mit der rechten Maustaste die **ContosoExpenses.Core** Projekt, und wählen **Projekt entladen**. Mit der rechten Maustaste erneut auf des Projekts, und wählen Sie dann **bearbeiten ContosoExpenses.Core.csproj**.

5. Vor dem schließenden **Projekt** -Element, fügen Sie den folgenden Eintrag hinzu:

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. Mit der rechten Maustaste die **ContosoExpenses.Core** Projekt, und wählen **Projekt erneut laden**.

7. Um die app die Contoso.ico zuzuweisen, Maustaste den **ContosoExpenses.Core** Projekt, und wählen **Eigenschaften**. Klicken Sie in der geöffneten Seite auf Dropdownliste **Symbol** , und wählen Sie `Images\contoso.ico`.

    ![Symbol "Contoso" in den Eigenschaften des Projekts](images/wpf-modernize-tutorial/ContosoIco.png)

8. Klicken Sie auf **Speichern**.

9. Drücken Sie F5, um die **ContosoExpenses.Core** Projekt im Debugger. Vergewissern Sie sich, dass die app nun ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

An diesem Punkt sind in diesem Tutorial Sie erfolgreich die Contoso-Ausgaben-app auf .NET Core 3 migriert. Sie sind nun bereit zur [Teil 2: Fügen Sie ein UWP-InkCanvas-Steuerelement, das mithilfe von XAML-Inseln](modernize-wpf-tutorial-2.md).

> [!NOTE]
> Wenn Sie einen Bildschirm hochauflösende haben, werden Sie feststellen, dass die app sehr kleinen Bildern. Sie müssen dieses Problem im nächsten Schritt des Tutorials zu beheben.
