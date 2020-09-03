---
description: In diesem Tutorial wird veranschaulicht, wie Sie UWP-XAML-Benutzeroberflächen hinzufügen, MSIX-Pakete erstellen und weitere moderne Komponenten in Ihre WPF-App integrieren.
title: Migrieren der Contoso-Spesen-App zu .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, XAML Islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: c11f1cab37e79fc320f1fb38f5b909d2cecd1ad4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161584"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>Teil 1: Migrieren der Contoso-Spesen-App zu .NET Core 3

Dies ist der erste Teil eines Tutorials, in dem das Modernisieren der WPF-Beispiel-Desktop-App Contoso Expenses veranschaulicht wird. Eine Übersicht über das Tutorial, die Voraussetzungen und Anweisungen zum Herunterladen der Beispiel-App finden Sie unter [Tutorial: Modernisieren einer WPF-App](modernize-wpf-tutorial.md).
  
In diesem Teil des Tutorials migrieren Sie die gesamte Contoso-Spesen-App von .NET Framework 4.7.2 zu [.NET Core 3](modernize-wpf-tutorial.md#net-core-3). Bevor Sie mit diesem Teil des Tutorials beginnen, stellen Sie sicher, dass Sie das [ContosoExpenses-Beispiel in Visual Studio 2019 öffnen und erstellen](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app).

> [!NOTE]
> Weitere Informationen zum Migrieren einer WPF-Anwendung von .NET Framework zu .NET Core 3 finden Sie in [dieser Reihe mit Blogbeiträgen](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>Migrieren des ContosoExpenses-Projekts zu .NET Core 3

In diesem Abschnitt migrieren Sie das ContosoExpenses-Projekt in der Contoso-Spesen-App zu .NET Core 3. Hierzu erstellen Sie eine neue Projektdatei, die die gleichen Dateien wie das vorhandene ContosoExpenses-Projekt enthält, die aber für die Zielplattform .NET Core 3 anstelle von .NET Framework 4.7.2 erstellt wird. Auf diese Weise können Sie eine einzelne Projektmappe mit beiden Versionen der App unterhalten, sowohl für .NET Framework als auch für .NET Core.

1. Vergewissern Sie sich, dass das ContosoExpenses-Projekt aktuell auf .NET Framework 4.7.2 ausgerichtet ist. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das **ContosoExpenses**-Projekt, wählen Sie **Eigenschaften** aus, und vergewissern Sie sich, dass die Eigenschaft **Zielframework** auf der Registerkarte **Anwendung** auf .NET Framework 4.7.2 festgelegt ist.

    ![.NET Framework-Version 4.7.2 für das Projekt](images/wpf-modernize-tutorial/NETFramework472.png)

3. Navigieren Sie im Windows-Explorer zum Ordner **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses**, und erstellen Sie eine neue Textdatei mit dem Namen **ContosoExpenses.Core.csproj**.

4. Klicken Sie mit der rechten Maustaste auf die Datei, wählen Sie **Öffnen mit** aus, und öffnen Sie sie dann in einem Text-Editor Ihrer Wahl, z. B. Notepad, Visual Studio Code oder Visual Studio.

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

6. Schließen Sie die Datei, und kehren Sie zur Projektmappe **ContosoExpenses** in Visual Studio zurück.

7. Klicken Sie mit der rechten Maustaste auf die Projektmappe **ContosoExpenses**, und wählen Sie **Hinzufügen > Vorhandenes Projekt** aus. Wählen Sie die **ContosoExpenses.Core.csproj**-Datei aus, die Sie soeben im Ordner `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` erstellt haben, um sie der Projektmappe hinzuzufügen.

**ContosoExpenses.Core.csproj** beinhaltet die folgenden Elemente:

* Das **Project**-Element gibt eine SDK-Version von **Microsoft.NET.Sdk.WindowsDesktop** an. Dies bezieht sich auf .NET-Anwendungen für Windows Desktop und beinhaltet Komponenten für WPF- und Windows Forms-Apps.
* Das **PropertyGroup**-Element enthält untergeordnete Elemente, die angeben, dass die Projektausgabe eine ausführbare Datei ist (keine DLL), auf .NET Core 3 abzielt und WPF verwendet. Für eine Windows Forms-App würden Sie ein **UseWinForms**-Element anstelle des **UseWPF**-Elements verwenden.

> [!NOTE]
> Beim Arbeiten mit dem CSPROJ-Format, das in .NET Core 3.0 eingeführt wurde, werden alle Dateien im gleichen Ordner wie die CSPROJ-Datei als Teil des Projekts angesehen. Daher müssen Sie nicht jede im Projekt enthaltene Datei angeben. Sie müssen lediglich die Dateien angeben, für die Sie eine benutzerdefinierte Buildaktion definieren oder die Sie ausschließen möchten.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>Migrieren des ContosoExpenses.Data-Projekts zu .NET Standard

Die **ContosoExpenses**-Projektmappe umfasst eine **ContosoExpenses.Data**-Klassenbibliothek, die Modelle und Schnittstellen für Dienste enthält und auf .NET 4.7.2 abzielt. .NET Core 3.0-Apps können .NET Framework-Bibliotheken nutzen, sofern sie keine APIs verwenden, die in .NET Core nicht verfügbar sind. Der beste Modernisierungspfad besteht jedoch darin, Ihre Bibliotheken auf .NET Standard umzustellen. Dadurch wird sichergestellt, dass Ihre Bibliothek von Ihrer .NET Core 3.0-App vollständig unterstützt wird. Außerdem können Sie die Bibliothek auch mit anderen Plattformen verwenden, wie etwa Web (mithilfe von ASP.NET Core) und mobil (mithilfe von Xamarin).

So migrieren Sie das **ContosoExpenses.Data**-Projekt zu .NET Standard:

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das **ContosoExpenses.Data**-Projekt, und wählen Sie **Projekt entladen** aus. Klicken Sie erneut mit der rechten Maustaste auf das Projekt, und wählen Sie dann **ContosoExpenses.Data.csproj bearbeiten** aus.

2. Löschen Sie den gesamten Inhalt der Projektdatei.

3. Kopieren Sie den folgenden XML-Code, fügen Sie ihn in die Datei ein, und speichern Sie:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. Klicken Sie mit der rechten Maustaste auf das **ContosoExpenses.Data**-Projekt, und wählen Sie **Projekt erneut laden** aus.

## <a name="configure-nuget-packages-and-dependencies"></a>Konfigurieren von NuGet-Paketen und Abhängigkeiten

Beim Migrieren der **ContosoExpenses.Core**- und **ContosoExpenses.Data**-Projekte in den vorherigen Abschnitten haben Sie die Verweise auf das NuGet-Paket aus den Projekten entfernt. In diesem Abschnitt fügen Sie diese Verweise wieder hinzu.

So konfigurieren Sie NuGet-Pakete für das **ContosoExpenses.Data**-Projekt:

1. Klappen Sie im **ContosoExpenses.Data**-Projekt den Knoten **Abhängigkeiten** auf. Beachten Sie, dass der **NuGet**-Abschnitt fehlt.

    ![NuGet-Pakete](images/wpf-modernize-tutorial/NuGetPackages.png)

    Wenn Sie die Datei **Packages.config** im **Projektmappen-Explorer** öffnen, finden Sie dort die ‚alten‘ Verweise der NuGet-Pakete, die im Projekt verwendet wurden, als das gesamte .NET Framework zum Einsatz kam.

    ![Abhängigkeiten und Pakete](images/wpf-modernize-tutorial/Packages.png)

    Hier sehen Sie den Inhalt der **Packages.config**-Datei. Sie werden feststellen, dass alle NuGet-Pakete auf das vollständige .NET Framework 4.7.2 abzielen:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. Löschen Sie im **ContosoExpenses.Data**-Projekt die Datei **Packages.config**.

4. Klicken Sie im **ContosoExpenses.Data**-Projekt mit der rechten Maustaste auf den Knoten **Abhängigkeiten**, und wählen Sie **NuGet-Pakete verwalten** aus.

  ![NuGet-Pakete verwalten...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. Klicken Sie im Fenster **NuGet-Paket-Manager** auf **Durchsuchen**. Suchen Sie nach dem Paket `Bogus`, und installieren Sie die letzte stabile Version.

    ![Bogus-NuGet-Paket](images/wpf-modernize-tutorial/Bogus.png)

6. Suchen Sie nach dem Paket `LiteDB`, und installieren Sie die letzte stabile Version.

    ![LiteDB-NuGet-Paket](images/wpf-modernize-tutorial/LiteDB.png)

    Sie fragen sich vielleicht, wo diese Liste von NuGet-Paketen gespeichert ist, da das Projekt nicht mehr über eine packages.config-Datei verfügt. Die NuGet-Pakete, auf die verwiesen wird, sind direkt in der CSPROJ-Datei gespeichert. Sie können dies überprüfen, indem Sie den Inhalt der **ContosoExpenses.Data.csproj**-Projektdatei in einem Text-Editor anzeigen. Sie werden feststellen, dass am Ende der Datei die folgenden Zeilen hinzugefügt wurden:

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > Möglicherweise fällt Ihnen auch auf, dass Sie für dieses .NET Core 3-Projekt dieselben Pakete installieren, wie sie für .NET Framework 4.7.2-Projekte verwendet werden. NuGet-Pakete unterstützen Multi-Targeting. Bibliotheksautoren können verschiedene Versionen einer Bibliothek in das gleiche Paket aufnehmen, die für verschiedene Architekturen und Plattformen kompiliert sind. Diese Pakete unterstützen das gesamte .NET Framework sowie .NET Standard 2.0, das mit .NET Core 3-Projekten kompatibel ist. Weitere Informationen zu den Unterschieden zwischen .NET Framework, .NET Core und .NET Standard finden Sie unter [.NET Standard](/dotnet/standard/net-standard).

So konfigurieren Sie NuGet-Pakete für das **ContosoExpenses.Core**-Projekt:

1. Öffnen Sie im **ContosoExpenses.Core**-Projekt die **packages.config**-Datei. Beachten Sie, dass sie aktuell die folgenden Verweise enthält, die auf .NET Framework 4.7.2 abzielen.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    In den folgenden Schritten legen Sie .NET Standard-Versionen der Pakete `MvvmLightLibs` und `Unity` fest. Die anderen beiden sind Abhängigkeiten, die automatisch von NuGet heruntergeladen werden, wenn Sie diese zwei Bibliotheken installieren.

2. Löschen Sie im **ContosoExpenses.Core**-Projekt die Datei **Packages.config**.

3. Klicken Sie mit der rechten Maustaste auf das **ContosoExpenses.Core**-Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.

4. Klicken Sie im Fenster **NuGet-Paket-Manager** auf **Durchsuchen**. Suchen Sie nach dem Paket `Unity`, und installieren Sie die letzte stabile Version.

    ![Unity-Paket](images/wpf-modernize-tutorial/UnityPackage.png)

5. Suchen Sie nach dem Paket `MvvmLightLibsStd10`, und installieren Sie die letzte stabile Version. Dies ist die .NET Standard-Version des `MvvmLightLibs`-Pakets. Für dieses Paket hat der Autor die .NET Standard-Version der Bibliothek in einem von der .NET Framework Version getrennten Paket verpackt.

    ![MvvmLightsLibs-Paket](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. Klicken Sie im Projekt **ContosoExpenses.Core** mit der rechten Maustaste auf den Knoten **Abhängigkeiten**, und wählen Sie **Verweis hinzufügen** aus.

7. Wählen Sie in der Kategorie **Projekte > Projektmappe** **ContosoExpenses.Data** aus, und klicken Sie auf **OK**.

    ![Verweis hinzufügen](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>Deaktivieren automatisch generierter Assemblyattribute

Wenn Sie an diesem Punkt im Migrationsprozess versuchen, das **ContosoExpenses.Core**-Projekt zu erstellen, werden einige Fehler angezeigt.

![Neue .NET Core 3-Buildfehler](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

Dieses Problem tritt auf, weil das neue CSPROJ-Format, das in .NET Core 3.0 eingeführt wurde, die Assembly in der Projektdatei statt in der Datei **AssemblyInfo.cs** speichert. Um diese Fehler zu beheben, deaktivieren Sie dieses Verhalten, und lassen Sie das Projekt weiterhin die Datei **AssemblyInfo.cs** verwenden.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das **ContosoExpenses.Core**-Projekt, und wählen Sie **Projekt entladen** aus. Klicken Sie erneut mit der rechten Maustaste auf das Projekt, und wählen Sie dann **ContosoExpenses.Core.csproj bearbeiten** aus.

1. Fügen Sie im Abschnitt **PropertyGroup** das folgende Element hinzu, und speichern Sie die Datei.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Nach dem Hinzufügen dieses Elements sollte der Abschnitt **PropertyGroup** jetzt so aussehen:

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. Klicken Sie mit der rechten Maustaste auf das **ContosoExpenses.Core**-Projekt, und wählen Sie **Projekt erneut laden** aus.

4. Klicken Sie mit der rechten Maustaste auf das **ContosoExpenses.Data**-Projekt, und wählen Sie **Projekt entladen** aus. Klicken Sie erneut mit der rechten Maustaste auf das Projekt, und wählen Sie dann **ContosoExpenses.Data.csproj bearbeiten** aus.

5. Fügen Sie im Abschnitt **PropertyGroup** denselben Eintrag hinzu, und speichern Sie die Datei.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Nach dem Hinzufügen dieses Elements sollte der Abschnitt **PropertyGroup** jetzt so aussehen:

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. Klicken Sie mit der rechten Maustaste auf das **ContosoExpenses.Data**-Projekt, und wählen Sie **Projekt erneut laden** aus.

## <a name="add-the-windows-compatibility-pack"></a>Hinzufügen des Windows-Kompatibilitätspakets

Wenn Sie nun versuchen, die Projekte **ContosoExpenses.Core** und **ContosoExpenses.Data** zu kompilieren, werden Sie feststellen, dass die vorherigen Fehler jetzt korrigiert sind, aber immer noch einige Fehler in der **ContosoExpenses.Data**-Bibliothek vorhanden sind, die diesen ähnlich sind.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

Diese Fehler sind das Ergebnis der Konvertierung des **ContosoExpenses.Data**-Projekts aus einer .NET Framework-Bibliothek (die Windows-spezifisch ist) in eine .NET Standard-Bibliothek, die auf mehreren Plattformen ausgeführt werden kann, einschließlich Linux, Android, iOS und mehr. Das **ContosoExpenses.Data**-Projekt enthält eine Klasse mit dem Namen **RegistryService**, die mit der Registrierung interagiert, einem Windows-exklusiven Konzept.

Um diese Fehler zu beheben, installieren Sie das NuGet-Paket [Windows-Kompatibilität](https://www.nuget.org/packages/Microsoft.Windows.Compatibility). Dieses Paket bietet Unterstützung für die Verwendung vieler Windows-spezifischer APIs in einer .NET Standard-Bibliothek. Die Bibliothek ist nach der Anwendung dieses Pakets nicht mehr plattformübergreifend, sie zielt aber weiterhin auf .NET Standard ab. 

1. Klicken Sie mit der rechten Maustaste auf das **ContosoExpenses.Data**-Projekt.
2. Wählen Sie **NuGet-Pakete verwalten** aus.
3. Klicken Sie im Fenster **NuGet-Paket-Manager** auf **Durchsuchen**. Suchen Sie nach dem Paket `Microsoft.Windows.Compatibility`, und installieren Sie die letzte stabile Version.

    ![NuGet-Paket installieren](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. Versuchen Sie dann erneut, das Projekt zu kompilieren, indem Sie mit der rechten Maustaste auf das Projekt **ContosoExpenses.Data** klicken und **Build**auswählen.

Dieses Mal wird der Buildprozess ohne Fehler beendet.

## <a name="test-and-debug-the-migration"></a>Testen und Debuggen der Migration

Jetzt, da sich die Pakete erfolgreich erstellen lassen, sind Sie bereit, die App auszuführen und zu testen, um festzustellen, ob Laufzeitfehler auftreten.

1. Klicken Sie mit der rechten Maustaste auf das Projekt **ContosoExpenses.Core**, und wählen Sie **Als Startprojekt festlegen** aus.

2. Drücken Sie F5, um das **ContosoExpenses.Core**-Projekt im Debugger zu starten. Eine Ausnahme ähnlich der folgenden wird angezeigt.

    ![In Visual Studio angezeigte Ausnahme](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    Diese Ausnahme wird ausgelöst, weil Sie beim Löschen des Inhalts aus der CSPROJ-Datei zu Beginn der Migration die Informationen über den **Buildvorgang** für die Bilddateien entfernt haben. Mit den folgenden Schritten wird dieses Problem behoben.

3. Beenden Sie den Debugger.

4. Klicken Sie mit der rechten Maustaste auf das **ContosoExpenses.Core**-Projekt, und wählen Sie **Projekt entladen** aus. Klicken Sie erneut mit der rechten Maustaste auf das Projekt, und wählen Sie dann **ContosoExpenses.Core.csproj bearbeiten** aus.

5. Fügen Sie vor dem schließenden **Project**-Element den folgenden Eintrag hinzu:

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. Klicken Sie mit der rechten Maustaste auf das **ContosoExpenses.Core**-Projekt, und wählen Sie **Projekt erneut laden** aus.

7. Um der App die Contoso.ico zuzuweisen, klicken Sie mit der rechten Maustaste auf das **ContosoExpenses.Core**-Projekt, und wählen Sie **Eigenschaften** aus. Klicken Sie auf der geöffneten Seite auf die Dropdownliste unter **Icon**, und wählen Sie `Images\contoso.ico` aus.

    ![Contoso-Symbol in den Eigenschaften des Projekts](images/wpf-modernize-tutorial/ContosoIco.png)

8. Klicken Sie auf **Speichern**.

9. Drücken Sie F5, um das **ContosoExpenses.Core**-Projekt im Debugger zu starten. Vergewissern Sie sich, dass die App nun ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

An diesem Punkt im Tutorial haben Sie die Contoso Expenses-App erfolgreich zu .NET Core 3 migriert. Sie sind nun bereit für [Teil 2: Hinzufügen eines UWP-InkCanvas-Steuerelements mithilfe von XAML Islands](modernize-wpf-tutorial-2.md).

> [!NOTE]
> Wenn Sie einen Bildschirm mit hoher Auflösung verwenden, stellen Sie möglicherweise fest, dass die App sehr klein aussieht. Dieses Problem wird im nächsten Schritt des Tutorials behandelt.