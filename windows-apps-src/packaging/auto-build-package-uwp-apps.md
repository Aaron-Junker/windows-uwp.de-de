---
title: Einrichten automatisierter Builds für UWP-Apps
description: Erfahren Sie, wie Sie automatisierte Builds konfigurieren, um Pakete zum Querladen oder zum Übermitteln an den Store zu erzeugen.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, UWP
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 5837674f2cb20710a59eeac0af59498bf28b197e
ms.sourcegitcommit: a86d0bd1c2f67e5986cac88a98ad4f9e667cfec5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68229377"
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

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>Fügen Sie Ihr Projekt-Zertifikat hinzu, um die Bibliothek für sichere Dateien

Übermitteln von Zertifikaten zu Ihrem Repository nach Möglichkeit vermeiden, und Git standardmäßig ignoriert. Um die sichere Handhabung von vertraulichen Dateien wie Zertifikate verwalten zu können, unterstützt Azure DevOps [sichere Dateien](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops).

So laden Sie ein Zertifikat für den automatisierten Build hoch:

1. Erweitern Sie im Azure-Pipelines, **Pipelines** im Navigationsbereich und klicken Sie auf **Bibliothek**.
2. Klicken Sie auf die **sichere Dateien** Registerkarte, und klicken Sie dann auf **+ sichere Datei**.

    ![Gewusst wie: Hochladen einer sicheren Datei](images/secure-file1.png)

3. Navigieren Sie zu der Zertifikatdatei, und klicken Sie auf **OK**.
4. Nachdem Sie das Zertifikat hochgeladen haben, wählen Sie ihn, um seine Eigenschaften anzuzeigen. Unter **Pipeline Berechtigungen**, aktivieren Sie die **autorisieren, für die Verwendung in allen Pipelines** umschalten.

    ![Gewusst wie: Hochladen einer sicheren Datei](images/secure-file2.png)

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
| UapAppxPackageBuildMode | SideloadOnly | Generiert die **_Test** Ordner für das querladen nur. |
| AppxPackageSigningEnabled | true | Ermöglicht das Paket zu signieren. |
| "Packagecertificatethumbprint" | Zertifikatfingerabdruck | Dieser Wert **müssen** entsprechen den Fingerabdruck im Signaturzertifikat oder eine leere Zeichenfolge sein. |
| PackageCertificateKeyFile | Pfad | Der Pfad zu das zu verwendende Zertifikat. Dies wird aus den Metadaten für die sichere Datei abgerufen. |

### <a name="configure-the-build"></a>Konfigurieren Sie den build

Wenn Sie Ihre Lösung über die Befehlszeile oder mithilfe von jedem anderen Buildsystem erstellen möchten, führen Sie MSBuild mit diesen Argumenten.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>Konfigurieren Sie die paketsignierung

Zum Signieren des Pakets MSIX (oder APPX) muss die Pipeline das Signaturzertifikat abrufen. Zu diesem Zweck fügen Sie eine Aufgabe DownloadSecureFile vor die Aufgabe VSBuild hinzu.
Dadurch erhalten Sie Zugriff auf das Signaturzertifikat über ```signingCert```.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

Als Nächstes aktualisieren Sie die VSBuild-Aufgabe, um das Signaturzertifikat zu verweisen:

```yml
- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" 
                  /p:AppxPackageDir="$(appxPackageDir)" 
                  /p:AppxBundle=Always 
                  /p:UapAppxPackageBuildMode=StoreUpload 
                  /p:AppxPackageSigningEnabled=true
                  /p:PackageCertificateThumbprint="" 
                  /p:PackageCertificateKeyFile="$(signingCert.secureFilePath)"'
```

> [!NOTE]
> Das Argument "packagecertificatethumbprint" wird als Vorsichtsmaßnahme absichtlich auf eine leere Zeichenfolge festgelegt. Wenn der Fingerabdruck im Projekt festgelegt ist, aber das signierende Zertifikat stimmt nicht überein, der Build mit dem Fehler fehl: `Certificate does not match supplied signing thumbprint`.

### <a name="review-parameters"></a>Überprüfen Sie die Parameter

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

- [Erstellen Sie Ihre app von .NET für Windows](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [Verpacken von UWP-apps](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
- [Querladen von BRANCHENSPEZIFISCHEN apps unter Windows 10](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [Erstellen Sie ein Zertifikat zum Signieren des Pakets](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
