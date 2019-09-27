---
description: In diesem Tutorial wird veranschaulicht, wie Sie UWP-XAML-Benutzeroberflächen hinzufügen, msix-Pakete erstellen und andere moderne Komponenten in Ihre WPF-App integrieren.
title: Migrieren der Contoso-Spesen-App zu .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, XAML-Inseln
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6a52e12f9d60ee4abb4b1aed3043a69c25845267
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317104"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>Teil 1: Migrieren der Contoso-Spesen-App zu .NET Core 3

Dies ist der erste Teil eines Tutorials, in dem veranschaulicht wird, wie eine WPF-Beispiel-Desktop-App mit dem Namen "ca Eine Übersicht über das Tutorial, Voraussetzungen und Anweisungen zum Herunterladen der Beispiel-App finden [Sie unter Tutorial: Modernisieren einer WPF-](modernize-wpf-tutorial.md)app.
  
In diesem Teil des Tutorials migrieren Sie die gesamte app "apptoso-Ausgaben" von der .NET Framework 4.7.2 zu [.net Core 3](modernize-wpf-tutorial.md#net-core-3). Bevor Sie mit diesem Teil des Tutorials beginnen, stellen Sie sicher, dass Sie [das contosoausgaben-Beispiel](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app) in Visual Studio 2019 öffnen und erstellen.

> [!NOTE]
> Weitere Informationen zum Migrieren einer WPF-Anwendung vom .NET Framework zu .net Core 3 finden Sie in [dieser Blog Reihe](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>Migrieren des contosoaufwendungen-Projekts zu .net Core 3

In diesem Abschnitt Migrieren Sie das Projekt contosoaufwendungen in der Contoso-Ausgaben-APP zu .net Core 3. Hierzu erstellen Sie eine neue Projektdatei, die die gleichen Dateien wie das vorhandene contosoaufwendungen-Projekt enthält, aber .net Core 3 anstelle der .NET Framework 4.7.2. Auf diese Weise können Sie eine einzelne Lösung mit .NET Framework und .net Core-Versionen der App verwalten.

1. Vergewissern Sie sich, dass das Projekt condesoaufwands derzeit auf die .NET Framework 4.7.2 abzielt. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **condesoaufwands** , wählen Sie **Eigenschaften**aus, und vergewissern Sie sich, dass die Eigenschaft **Ziel Framework** auf der Registerkarte **Anwendung** auf .NET Framework 4.7.2 festgelegt ist.

    ![.NET Framework Version 4.7.2 für das Projekt](images/wpf-modernize-tutorial/NETFramework472.png)

3. Navigieren Sie in Windows-Explorer zum Ordner " **c:\winappsmodernizationworkshop\lab\exercise1\01-start\contosoaufwendungen** ", und erstellen Sie eine neue Textdatei mit dem Namen " **contosoaufwands. Core. csproj**".

4. Klicken Sie mit der rechten Maustaste auf die Datei, wählen Sie **Öffnen mit**aus, und öffnen Sie Sie dann in einem Text-Editor Ihrer Wahl, z. b. Notepad, Visual Studio Code oder Visual Studio.

5. Kopieren Sie den folgenden Text in die Datei, und speichern Sie ihn.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. Schließen Sie die Datei, und kehren Sie zur Projekt Mappe **condesoaufwands** in Visual Studio zurück.

7. Klicken Sie mit der rechten Maustaste auf die Lösung **conabsoaufwands** , und wählen Sie **Add-> vorhandenes Projekt** Wählen Sie die Datei **contosoaufwendungen. Core. csproj** aus, die Sie `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` soeben im Ordner erstellt haben, um Sie der Projekt Mappe hinzuzufügen.

**Contosoaufwands. Core. csproj** umfasst die folgenden Elemente:

* Das **Project** -Element gibt eine SDK-Version von **Microsoft. net. SDK. windowsdesktop**an. Dies bezieht sich auf .NET-Anwendungen für Windows Desktop und umfasst Komponenten für WPF-und Windows Forms-apps.
* Das **PropertyGroup** -Element enthält untergeordnete Elemente, die angeben, dass die Projekt Ausgabe eine ausführbare Datei (keine dll) ist, auf .net Core 3 ausgerichtet ist und WPF verwendet. Für eine Windows Forms-APP würden Sie ein **usewinforms** -Element anstelle des **usewpf** -Elements verwenden.

> [!NOTE]
> Wenn Sie mit dem csproj-Format arbeiten, das mit .net Core 3,0 eingeführt wurde, werden alle Dateien im selben Ordner wie die CSPROJ-Datei als Teil des Projekts betrachtet. Daher müssen Sie nicht jede Datei angeben, die im Projekt enthalten ist. Sie müssen nur die Dateien angeben, für die Sie eine benutzerdefinierte Buildaktion definieren möchten oder die Sie ausschließen möchten.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>Migrieren Sie das Projekt Conto soaufwands. Data zu .NET Standard

Die Lösung **condesoaufwands** umfasst eine **condesoaufwands. Data** -Klassenbibliothek, die Modelle und Schnittstellen für Dienste und .NET 4.7.2-Ziele enthält. .Net Core 3,0-Apps können .NET Framework Bibliotheken verwenden, sofern Sie keine APIs verwenden, die in .net Core nicht verfügbar sind. Der beste Modernisierungspfad besteht jedoch darin, Ihre Bibliotheken in .NET Standard zu verschieben. Dadurch wird sichergestellt, dass Ihre Bibliothek von Ihrer .net Core 3,0-App vollständig unterstützt wird. Außerdem können Sie die Bibliothek auch mit anderen Plattformen wieder verwenden, wie z. b. Web (über ASP.net Core) und Mobile (über xamarin).

So migrieren Sie das Projekt **Conto soaufwands. Data** zu .NET Standard:

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt **Conto soaufwands. Data** , und wählen Sie **Projekt entladen**aus. Klicken Sie mit der rechten Maustaste erneut auf das Projekt, und wählen Sie dann **condesoaufwands. Data. csproj bearbeiten**aus.

2. Löschen Sie den gesamten Inhalt der Projektdatei.

3. Kopieren Sie den folgenden XML-Code, und speichern Sie die Datei.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. Klicken Sie mit der rechten Maustaste auf das Projekt **Conto soaufwands. Data** , und wählen Sie **Projekt erneut laden**

## <a name="configure-nuget-packages-and-dependencies"></a>Konfigurieren von nuget-Paketen und-Abhängigkeiten

Beim Migrieren der Projekte " **contosoaufwendungen. Core** " und " **contosoaufwendungen. Data** " in den vorherigen Abschnitten haben Sie die nuget-Paket Verweise aus den Projekten entfernt. In diesem Abschnitt fügen Sie diese Verweise wieder hinzu.

So konfigurieren Sie nuget-Pakete für das Projekt **condesoaufwands. Data** :

1. Erweitern Sie im Projekt **condesoaufwands. Data** den Knoten **Abhängigkeiten** . Beachten Sie, dass der **nuget** -Abschnitt fehlt.

    ![Nuget-Pakete](images/wpf-modernize-tutorial/NuGetPackages.png)

    Wenn Sie die Datei " **Packages. config** " in der **Projektmappen-Explorer** öffnen, finden Sie die alten Verweise der nuget-Pakete, die das Projekt verwendet haben, als die vollständige .NET Framework verwendet wurde.

    ![Abhängigkeiten und Pakete](images/wpf-modernize-tutorial/Packages.png)

    Im folgenden finden Sie den Inhalt der Datei " **Packages. config** ". Sie werden feststellen, dass alle nuget-Pakete auf den vollständigen .NET Framework 4.7.2 abzielen:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. Löschen Sie die Datei " **Packages. config** " im Projekt " **condesoaufwands. Data** ".

4. Klicken Sie im Projekt **condesoaufwands. Data** mit der rechten Maustaste auf den Knoten **Abhängigkeiten** , und wählen Sie **nuget-Pakete verwalten**aus.

  ![Nuget-Pakete verwalten...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. Klicken Sie im Fenster **nuget-Paket-Manager** auf **Durchsuchen**. Suchen Sie nach `Bogus` dem Paket, und installieren Sie die neueste stabile Version.

    ![Bogus-nuget-Paket](images/wpf-modernize-tutorial/Bogus.png)

6. Suchen Sie nach `LiteDB` dem Paket, und installieren Sie die neueste stabile Version.

    ![Litedb-nuget-Paket](images/wpf-modernize-tutorial/LiteDB.png)

    Sie Fragen sich vielleicht, wo diese Liste von nuget-Paketen gespeichert ist, da das Projekt nicht mehr über die Datei "Packages. config" verfügt. Die nuget-Pakete, auf die verwiesen wird, werden direkt in der CSPROJ-Datei gespeichert. Sie können dies überprüfen, indem Sie den Inhalt der Projektdatei **condesoaufwands. Data. csproj** in einem Text-Editor anzeigen. Sie werden feststellen, dass am Ende der Datei die folgenden Zeilen hinzugefügt wurden:

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > Sie werden möglicherweise auch bemerken, dass Sie dieselben Pakete für dieses .net Core 3-Projekt installieren wie die von .NET Framework 4.7.2-Projekten verwendeten. Nuget-Pakete unterstützen die Zielvorgabe. Bibliotheks Autoren können unterschiedliche Versionen einer Bibliothek im selben Paket enthalten, die für verschiedene Architekturen und Plattformen kompiliert werden. Diese Pakete unterstützen das vollständige .NET Framework und .NET Standard 2,0, das mit .net Core 3-Projekten kompatibel ist. Weitere Informationen zu den unterschieden .NET Framework, .net Core und .NET Standard finden Sie unter [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

So konfigurieren Sie nuget-Pakete für das Projekt **contosoaufwendungen. Core** :

1. Öffnen Sie im Projekt **contosoaufwendungen. Core** die Datei **Packages. config** . Beachten Sie, dass Sie derzeit die folgenden Verweise enthält, die auf die .NET Framework 4.7.2 abzielen.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    In den folgenden Schritten .NET Standard Sie Versionen der `MvvmLightLibs` -und- `Unity` Pakete. Die anderen beiden sind Abhängigkeiten, die von nuget automatisch heruntergeladen werden, wenn Sie diese beiden Bibliotheken installieren.

2. Löschen Sie die Datei " **Packages. config** " im Projekt " **contosoaufwendungen. Core** ".

3. Klicken Sie mit der rechten Maustaste auf das Projekt **contosoaufwendungen. Core** , und wählen Sie **nuget-Pakete verwalten**.

4. Klicken Sie im Fenster **nuget-Paket-Manager** auf **Durchsuchen**. Suchen Sie nach `Unity` dem Paket, und installieren Sie die neueste stabile Version.

    ![Unity-Paket](images/wpf-modernize-tutorial/UnityPackage.png)

5. Suchen Sie nach `MvvmLightLibsStd10` dem Paket, und installieren Sie die neueste stabile Version. Dies ist die .NET Standard Version des `MvvmLightLibs` Pakets. Für dieses Paket hat der Autor die .NET Standard Version der Bibliothek in einem separaten Paket als die .NET Framework Version verpacken.

    ![Mvvmlightlisb-Paket](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. Klicken Sie im Projekt **contosoaufwendungen. Core** mit der rechten Maustaste auf den Knoten **Abhängigkeiten** , und wählen Sie **Verweis hinzufügen**aus.

7. Wählen Sie in der Kategorie **Projekte > Projekt** Mappe die Option **conessoaufwendungen. Data** aus, und klicken Sie auf **OK**.

    ![Verweis hinzufügen](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>Automatisch generierte Assemblyattribute deaktivieren

Wenn Sie an dieser Stelle im Migrations Vorgang versuchen, das Projekt **contosoaufwendungen. Core** zu erstellen, werden einige Fehler angezeigt.

![.Net Core 3 Erstellen neuer Fehler](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

Dieses Problem ist aufgetreten, weil das neue csproj-Format, das mit .net Core 3,0 eingeführt wurde, die Assemblyinformationen in der Projektdatei und nicht in der **AssemblyInfo.cs** -Datei speichert. Um diese Fehler zu beheben, deaktivieren Sie dieses Verhalten, und lassen Sie das Projekt die **AssemblyInfo.cs** -Datei weiterhin verwenden.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt **contosoaufwendungen. Core** , und wählen Sie **Projekt entladen**aus. Klicken Sie mit der rechten Maustaste erneut auf das Projekt, und wählen Sie dann **contosoaufwendungen. Core. csproj bearbeiten**aus.

1. Fügen Sie im Abschnitt **PropertyGroup** das folgende-Element hinzu, und speichern Sie die Datei.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Nach dem Hinzufügen dieses Elements sollte der **PropertyGroup** -Abschnitt nun wie folgt aussehen:

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. Klicken Sie mit der rechten Maustaste auf das Projekt **contosoaufwendungen. Core** , und wählen Sie **Projekt erneut laden**.

4. Klicken Sie mit der rechten Maustaste auf das Projekt **Conto soaufwands. Data** , und wählen Sie **Projekt entladen**aus. Klicken Sie mit der rechten Maustaste erneut auf das Projekt, und wählen Sie dann **condesoaufwands. Data. csproj bearbeiten**aus.

5. Fügen Sie im Abschnitt **PropertyGroup** denselben Eintrag hinzu, und speichern Sie die Datei.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Nach dem Hinzufügen dieses Elements sollte der **PropertyGroup** -Abschnitt nun wie folgt aussehen:

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. Klicken Sie mit der rechten Maustaste auf das Projekt **Conto soaufwands. Data** , und wählen Sie **Projekt erneut laden**

## <a name="add-the-windows-compatibility-pack"></a>Hinzufügen des Windows-Kompatibilitätspakets

Wenn Sie nun versuchen, die Projekte " **contosoaufwands. Core** " und " **contosoausgaben. Data** " zu kompilieren, sehen Sie, dass die vorherigen Fehler jetzt korrigiert sind, aber in der **Datei "contosoaufwendungen. Data** " ähnlich wie diese angezeigt werden.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

Diese Fehler sind das Ergebnis der Typumwandlung des Projekts **condesoaufwands. Data** aus einer .NET Framework-Bibliothek (die für Windows spezifisch ist) in eine .NET Standard Bibliothek, die auf mehreren Plattformen ausgeführt werden kann, einschließlich Linux, Android, IOS und mehr. Das Projekt " **condesoaufwands. Data** " enthält eine Klasse mit dem Namen " **registryservice**", die mit der Registrierung interagiert, einem reinen Windows-Konzept.

Um diese Fehler zu beheben, installieren Sie das nuget-Paket für die [Windows-Kompatibilität](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) . Dieses Paket bietet Unterstützung für viele Windows-spezifische APIs, die in einer .NET Standard Bibliothek verwendet werden. Die Bibliothek ist nach der Verwendung dieses Pakets nicht mehr plattformübergreifend, sondern wird weiterhin auf .NET Standard ausgerichtet. 

1. Klicken Sie mit der rechten Maustaste auf das Projekt **Conto soaufwands. Data** .
2. Wählen Sie **nuget-Pakete verwalten**aus.
3. Klicken Sie im Fenster **nuget-Paket-Manager** auf **Durchsuchen**. Suchen Sie nach `Microsoft.Windows.Compatibility` dem Paket, und installieren Sie die neueste stabile Version.

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. Wiederholen Sie den Vorgang, um das Projekt zu kompilieren, indem Sie mit der rechten Maustaste auf das Projekt **Conto soaufwands. Data** klicken und **Erstellen**wählen.

Dieses Mal wird der Buildprozess ohne Fehler beendet.

## <a name="test-and-debug-the-migration"></a>Testen und Debuggen der Migration

Nachdem Sie die Projekte erfolgreich aufgebaut haben, können Sie die app ausführen und testen, um festzustellen, ob Laufzeitfehler vorliegen.

1. Klicken Sie mit der rechten Maustaste auf das Projekt **contosoaufwendungen. Core** , und wählen Sie **als Startprojekt festlegen**aus.

2. Drücken Sie F5, um das Projekt **contosoaufwands. Core** im Debugger zu starten. Eine Ausnahme wie die folgende wird angezeigt.

    ![In Visual Studio angezeigte Ausnahme](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    Diese Ausnahme wird ausgelöst, weil Sie beim Löschen des Inhalts aus der CSPROJ-Datei zu Beginn der Migration die Informationen über die **Buildaktion** für die Bilddateien entfernt haben. Mit den folgenden Schritten wird dieses Problem behoben.

3. Beendet den Debugger.

4. Klicken Sie mit der rechten Maustaste auf das Projekt **contosoaufwendungen. Core** , und wählen Sie **Projekt entladen**aus. Klicken Sie mit der rechten Maustaste erneut auf das Projekt, und wählen Sie dann **contosoaufwendungen. Core. csproj bearbeiten**aus.

5. Fügen Sie vor dem schließenden **Projekt** Element den folgenden Eintrag hinzu:

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. Klicken Sie mit der rechten Maustaste auf das Projekt **contosoaufwendungen. Core** , und wählen Sie **Projekt erneut laden**.

7. Wenn Sie die Datei contoso. ico der APP zuweisen möchten, klicken Sie mit der rechten Maustaste auf das Projekt **contosoaufwendungen. Core** , und wählen Sie **Eigenschaften**aus. Klicken Sie auf der geöffneten Seite unter **Symbol** auf Dropdown, und `Images\contoso.ico`wählen Sie aus.

    ![Symbol "Configuration Manager" in den Projekteigenschaften](images/wpf-modernize-tutorial/ContosoIco.png)

8. Klicken Sie auf **Speichern**.

9. Drücken Sie F5, um das Projekt **contosoaufwands. Core** im Debugger zu starten. Vergewissern Sie sich, dass die App nun ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

An dieser Stelle im Tutorial haben Sie die APP für die APP für die kostenpflichtige app erfolgreich zu .net Core 3 migriert. Sie sind nun für [Teil 2 bereit: Fügen Sie ein UWP InkCanvas-Steuerelement mithilfe von](modernize-wpf-tutorial-2.md)XAML-Inseln hinzu.

> [!NOTE]
> Wenn Sie über einen Bildschirm mit hoher Auflösung verfügen, werden Sie möglicherweise bemerken, dass die APP sehr klein aussieht. Dieses Problem wird im nächsten Schritt des Tutorials behandelt.
