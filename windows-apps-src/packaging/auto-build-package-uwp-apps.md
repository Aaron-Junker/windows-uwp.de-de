---
title: Einrichten automatisierter Builds für UWP-Apps
description: Erfahren Sie, wie Sie automatisierte Builds konfigurieren, um Pakete zum Querladen oder zum Übermitteln an den Store zu erzeugen.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, UWP
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: cb21573dac0c4cc4fc2d6aa2e2345c56631fde87
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372761"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>Einrichten automatisierter Builds für UWP-Apps

Sie können Pipelines mit Azure verwenden, um automatisierte Builds für UWP-Projekte zu erstellen. In diesem Artikel betrachten wir verschiedene Möglichkeiten zur Verfügung. Wir außerdem zeigen Ihnen wie Sie diese Aufgaben ausführen, indem Sie über die Befehlszeile aus, sodass Sie in jedem anderen Buildsystem integrieren können.

## <a name="create-a-new-azure-pipeline"></a>Erstellen einer neuen Azure-Pipeline

Zunächst [Registrierung für Azure-Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up) , wenn Sie dies noch nicht geschehen.

Als Nächstes erstellen Sie eine Pipeline, die Sie verwenden können, um den Quellcode zu erstellen. Ein Lernprogramm zum Erstellen einer Pipeline zum Erstellen eines GitHub-Repositorys, finden Sie unter [erstellen Ihre erste Pipeline](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml). Azure-Pipelines unterstützt die aufgeführten repositorytypen [in diesem Artikel](https://docs.microsoft.com/azure/devops/pipelines/repos).

## <a name="set-up-an-automated-build"></a>Einrichten eines automatisierten Builds

Wir beginnen mit dem Standard, UWP, Builddefinition, die in Azure DevOps verfügbar ist, und klicken Sie dann zeigen, wie zum Konfigurieren der Pipeline.

Wählen Sie in der Liste der Builddefinitionsvorlagen die Vorlage **Universelle Windows-Plattform** aus.

![Wählen Sie die UWP-Vorlage](images/select-yaml-template.png)

Diese Vorlage umfasst die grundlegende Konfiguration, um das UWP-Projekt erstellen:

```yml
trigger:
- master

pool:
  vmImage: 'VS2017-Win2016'

variables:
  solution: '**/*.sln'
  buildPlatform: 'x86|x64|ARM'
  buildConfiguration: 'Release'
  appxPackageDir: '$(build.artifactStagingDirectory)\AppxPackages\\'

steps:
- task: NuGetToolInstaller@0

- task: NuGetCommand@2
  inputs:
    restoreSolution: '$(solution)'

- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" /p:AppxPackageDir="$(appxPackageDir)" /p:AppxBundle=Always /p:UapAppxPackageBuildMode=StoreUpload'

```

Die Standardvorlage versucht zum Signieren des Pakets mit dem Zertifikat, das in der CSPROJ-Datei angegeben. Wenn Sie Ihr Paket während des Builds signieren möchten benötigen Sie Zugriff auf den privaten Schlüssel. Andernfalls können Sie deaktivieren, Signierung durch Hinzufügen des Parameters `/p:AppxPackageSigningEnabled=false` auf die `msbuildArgs` Abschnitt in der YAML-Datei.

## <a name="add-your-project-certificate-to-a-repository"></a>Fügen Sie Ihr Projekt Zertifikat in einem repository

Pipelines funktioniert mit Azure-Repositorys Git- und TFVC-Repositorys. Bei Verwendung eines Git-Repositorys fügen Sie die Zertifikatdatei Ihres Projekts dem Repository hinzu, damit der Build-Agent das App-Paket signieren kann. Andernfalls wird die Zertifikatdatei vom Git-Repository ignoriert. Um die Zertifikatdatei in Ihrem Repository hinzuzufügen, Maustaste die Zertifikatdatei in **Projektmappen-Explorer**, und wählen Sie dann im Kontextmenü die Option, die **ignoriert Datei zur Quellcodeverwaltung hinzufügen** Befehl.

![Einschließen eines Zertifikats](images/building-screen1.png)

## <a name="configure-the-build-solution-build-task"></a>Konfigurieren der Buildaufgabe „Projektmappe erstellen“

Diese Aufgabe wird jede Lösung, die im Arbeitsverzeichnis zu den Binärdateien und erzeugt die Ausgabe-app-Paket-Datei kompiliert.
Dieser Task verwendet die MSBuild-Argumente. Sie müssen den Wert dieser Argumente angeben. Orientieren Sie sich an der folgenden Tabelle.

|**MSBuild-argument**|**Wert**|**Beschreibung**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Definiert den Ordner, in dem die generierten Artefakte gespeichert werden. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Können Sie definieren die Plattformen für die im Paket enthalten. |
| AppxBundle | Immer | Erstellt eine.msixbundle/.appxbundle mit den.msix/.appx-Dateien für die Plattform angegeben. |
| UapAppxPackageBuildMode | StoreUpload | Generiert die.msixupload/.appxupload-Datei und die **_Test** Ordner für das querladen. |
| UapAppxPackageBuildMode | CI | Wird nur die.msixupload/.appxupload-Datei generiert. |
| UapAppxPackageBuildMode | SideloadOnly | Generiert die **_Test** Ordner für das querladen nur |

Wenn Sie Ihre Lösung über die Befehlszeile oder mithilfe von jedem anderen Buildsystem erstellen möchten, führen Sie MSBuild mit diesen Argumenten.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

Die Parameter definiert die `$()` Syntax sind Variablen, die in der Builddefinition definiert und Änderungen in anderen Systemen erstellt.

![Standardvariablen](images/building-screen5.png)

Alle vordefinierten Variablen finden Sie unter [vordefinierte Buildvariablen](https://docs.microsoft.com/azure/devops/pipelines/build/variables).

## <a name="configure-the-publish-build-artifacts-task"></a>Konfigurieren Sie den Task Veröffentlichen von Buildartefakten

Die UWP-standardpipeline werden die generierten Elemente nicht gespeichert werden. Um die YAML-Definition die Publish-Funktionen hinzuzufügen, fügen Sie die folgenden Aufgaben hinzu.

```yml
- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**\bin\$(BuildConfiguration)\**'
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
```

Sehen Sie die generierten Elemente in der **Artefakte** Option des Builds Ergebnisse (Seite).

![Artefakte](images/building-screen6.png)

Da wir eingerichtet haben die `UapAppxPackageBuildMode` Argument `StoreUpload`, der Ordner Elemente enthält, das Paket für die Übermittlung an den Store (.msixupload/.appxupload). Beachten Sie, dass Sie auch einen regulären app-Paket (.msix/.appx) oder ein app-Bündel (.msixbundle/.appxbundle/) an den Store übermitteln können. Für die Zwecke dieses Artikels verwenden wir die appxupload-Datei.

## <a name="address-bundle-errors"></a>Bundle-Adressfehler

Wenn Sie mehr als eine UWP-Projekt der Projektmappe hinzufügen, und klicken Sie dann versuchen, ein Paket zu erstellen, erhalten Sie möglicherweise eine Fehlermeldung wie die folgende.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

Dieser Fehler tritt auf, da auf Projektmappenebene nicht eindeutig ist, welche App im Bündel enthalten sein soll. Um dieses Problem zu beheben, öffnen Sie jede Projektdatei aus, und fügen Sie die folgenden Eigenschaften am Ende des ersten `<PropertyGroup>` Element.

|**Projekt**|**Eigenschaften**|
|-------|----------|
|App|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

Entfernen Sie dann die `AppxBundle` MSBuild-Argument aus den Buildschritt.

## <a name="related-topics"></a>Verwandte Themen

- [Erstellen Sie Ihre app von .NET für Windows](https://www.visualstudio.com/docs/build/get-started/dot-net)
- [Verpacken von UWP-apps](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
- [Querladen von BRANCHENSPEZIFISCHEN apps unter Windows 10](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10)
- [Erstellen Sie ein Zertifikat zum Signieren des Pakets](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
