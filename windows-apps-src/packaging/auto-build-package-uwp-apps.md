---
title: Einrichten automatisierter Builds für UWP-Apps
description: Erfahre, wie du automatisierte Builds konfigurierst, um Pakete zum Querladen und/oder Übermitteln an den Store zu erzeugen.
ms.date: 07/17/2019
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 70415c9f3d58625cfdc651ec67c8a9f37c23cffa
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "77089496"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>Einrichten automatisierter Builds für UWP-Apps

Du kannst Azure-Pipelines verwenden, um automatisierte Builds für UWP-Projekte zu erstellen. In diesem Artikel werden verschiedene Möglichkeiten erläutert, dies zu realisieren. Außerdem wird erläutert, wie diese Aufgaben über die Befehlszeile ausgeführt werden, um die Integration in beliebige andere Buildsysteme zu ermöglichen.

## <a name="create-a-new-azure-pipeline"></a>Erstellen einer neuen Azure-Pipeline

Beginne damit, [dich für Azure-Pipelines anzumelden](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up), falls dies noch nicht geschehen ist.

Als nächstes erstellst du eine Pipeline, die du zum Erstellen deines Quellcodes verwenden kannst. Ein Tutorial über das Erstellen einer Pipeline zur Erstellung eines GitHub-Repositorys findest du unter [Erstellen einer ersten Pipeline](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml). Azure Pipelines unterstützt die [in diesem Artikel](https://docs.microsoft.com/azure/devops/pipelines/repos) aufgeführten Repositorytypen.

## <a name="set-up-an-automated-build"></a>Einrichten eines automatisierten Builds

Wir beginnen mit der standardmäßigen UWP-Builddefinition, die in Azure Dev Ops verfügbar ist, und zeigen dir, wie du die Pipeline konfigurierst.

Wählen Sie in der Liste der Builddefinitionsvorlagen die Vorlage **Universelle Windows-Plattform** aus.

![Auswählen der UWP-Vorlage](images/select-yaml-template.png)

Diese Vorlage enthält die grundlegende Konfiguration zum Erstellen des UWP-Projekts:

```yml
trigger:
- master

pool:
  vmImage: 'windows-latest'

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

Die Standardvorlage versucht, das Paket mit dem Zertifikat zu signieren, das in der CSPROJ-Datei angegeben ist. Wenn du das Paket während des Builds signieren möchtest, musst du Zugriff auf den privaten Schlüssel haben. Andernfalls kannst du die Signierung deaktivieren, indem du den Parameter `/p:AppxPackageSigningEnabled=false` dem Abschnitt `msbuildArgs` in der YAML-Datei hinzufügst.

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>Hinzufügen eines Projektzertifikats zur Bibliothek für sichere Dateien

Du solltest die Übermittlung von Zertifikaten an dein Repository möglichst vermeiden und von Git werden sie standardmäßig ignoriert. Um die sichere Handhabung vertraulicher Dateien wie Zertifikate zu verwalten, unterstützt Azure DevOps das Feature [Sichere Dateien](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops).

So lädst du ein Zertifikat für deinen automatisierten Build hoch

1. Erweitere in Azure Pipelines die Option **Pipelines** im Navigationsbereich und klicke auf **Bibliothek**.
2. Klicke auf die Registerkarte **Sichere Dateien** und dann auf **+ Sichere Datei**.

    ![Hochladen einer sicheren Datei](images/secure-file1.png)

3. Navigiere zu der Zertifikatsdatei und klicke auf **OK**.
4. Nachdem du das Zertifikat hochgeladen hast, wähle es aus, um seine Eigenschaften anzuzeigen. Aktiviere unter **Pipelineberechtigungen** den Schalter **Zur Verwendung in allen Pipelines autorisieren**.

    ![Hochladen einer sicheren Datei](images/secure-file2.png)

5. Wenn der private Schlüssel im Zertifikat ein Kennwort aufweist, wird empfohlen, das Kennwort in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) zu speichern und das Kennwort dann mit einer [Variablengruppe](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups) zu verknüpfen. Mithilfe der Variablen kannst du auf das Kennwort von der Pipeline zugreifen. Hierbei ist zu beachten, dass ein Kennwort nur für den privaten Schlüssel unterstützt wird. Die Verwendung einer Zertifikatsdatei, die selbst kennwortgeschützt ist, wird derzeit nicht unterstützt.

> [!NOTE]
> Ab Visual Studio 2019 wird in UWP-Projekten kein temporäres Zertifikat mehr generiert. Verwende zum Erstellen oder Exportieren von Zertifikaten die in [diesem Artikel](/windows/msix/package/create-certificate-package-signing) beschriebenen PowerShell-Cmdlets.

## <a name="configure-the-build-solution-build-task"></a>Konfigurieren der Buildaufgabe „Projektmappe erstellen“

Mit dieser Aufgabe wird eine im Arbeitsordner enthaltene Projektmappe in Binärdateien kompiliert, und die App-Paketdatei wird erstellt. In dieser Aufgabe werden MSBuild-Argumente verwendet. Sie müssen den Wert dieser Argumente angeben. Orientieren Sie sich an der folgenden Tabelle.

|**MSBuild-Argument**|**Wert**|**Beschreibung**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Definiert den Ordner, in dem die generierten Artefakte gespeichert werden. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Ermöglicht es dir, die Plattformen zu definieren, die in das Bundle aufgenommen werden sollen. |
| AppxBundle | Immer | Erstellt ein .msixbundle/.appxbundle mit den MSIX-/APPX-Dateien für die angegebene Plattform. |
| UapAppxPackageBuildMode | StoreUpload | Generiert die .msixupload-/.appxupload-Datei und den Ordner **_Test** für das Querladen. |
| UapAppxPackageBuildMode | CI | Generiert nur die .msixupload-/.appxupload-Datei. |
| UapAppxPackageBuildMode | SideloadOnly | Generiert den Ordner **_Test** nur für das Querladen. |
| AppxPackageSigningEnabled | wahr | Aktiviert das Signieren von Paketen. |
| PackageCertificateThumbprint | Zertifikatfingerabdruck | Dieser Wert **muss** mit dem Fingerabdruck im Signaturzertifikat übereinstimmen oder eine leere Zeichenfolge sein. |
| PackageCertificateKeyFile | Pfad | Der Pfad zu dem zu verwendenden Zertifikat. Dieser wird aus den sicheren Dateimetadaten abgerufen. |
| PackageCertificatePassword | Kennwort | Das Kennwort für den privaten Schlüssel im Zertifikat. Es wird empfohlen, das Kennwort in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) zu speichern und es mit der [Variablengruppe](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups) zu verknüpfen. Du kannst die Variable an dieses Argument übergeben. |

### <a name="configure-the-build"></a>Konfigurieren des Builds

Wenn du deine Projektmappe über die Befehlszeile oder mit einem anderen Buildsystem erstellen möchtest, führe MSBuild mit diesen Argumenten aus.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>Konfigurieren der Paketsignierung

Die Pipeline muss zum Signieren des MSIX-Pakets (oder APPX-Pakets) das Signaturzertifikat abrufen. Füge dazu vor dem VSBuild-Task einen DownloadSecureFile-Task hinzu.
Damit erhältst du über ```signingCert``` Zugriff auf das Signaturzertifikat.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

Aktualisiere als nächstes den VSBuild-Task, um auf das Signaturzertifikat zu verweisen:

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
> Das PackageCertificateThumbprint-Argument wird vorsichtshalber absichtlich auf eine leere Zeichenfolge festgelegt. Wenn der Fingerabdruck im Projekt festgelegt ist, aber nicht mit dem Signaturzertifikat übereinstimmt, tritt der folgende Fehler beim Build auf: `Certificate does not match supplied signing thumbprint`.

### <a name="review-parameters"></a>Überprüfen der Parameter

Die mit der `$()`-Syntax definierten Parameter sind Variablen, die in der Builddefinition definiert sind und sich in anderen Buildsystemen unterscheiden.

![Standardvariablen](images/building-screen5.png)

Unter [Vordefinierte Buildvariablen](https://docs.microsoft.com/azure/devops/pipelines/build/variables) sind alle vordefinierten Variablen aufgeführt.

## <a name="configure-the-publish-build-artifacts-task"></a>Konfigurieren des Tasks „Buildartefakte veröffentlichen“

Die UWP-Standardpipeline speichert die generierten Artefakte nicht. Um deiner YAML-Definition die Veröffentlichungsfunktionen hinzuzufügen, füge die folgenden Tasks hinzu.

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

Du kannst die generierten Artefakte in der Option **Artefakte** der Seite der Buildergebnisse sehen.

![Artefakte](images/building-screen6.png)

Da wir das `UapAppxPackageBuildMode`-Argument auf `StoreUpload` festlegen, enthält der Artefaktordner das Paket, das für die Übermittlung an den Store empfohlen wird (.msixupload/.appxupload). Beachte, dass du auch ein normales App-Paket (.msix/.appx) oder ein App-Bündel (.msixbundle/.appxbundle) an den Store übermitteln kannst. Für die Zwecke dieses Artikels verwenden wir die APPXUPLOAD-Datei.

## <a name="address-bundle-errors"></a>Behandeln von Bündelfehlern

Wenn du deiner Projektmappe mehrere UWP-Projekte hinzufügst und dann versuchst, ein Bündel zu erstellen, erhältst du möglicherweise eine mit der folgenden Fehlermeldung vergleichbare Meldung.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

Dieser Fehler tritt auf, da auf Projektmappenebene nicht eindeutig ist, welche App im Bündel enthalten sein soll. Um dieses Problem zu beheben, öffne jede Projektdatei und füge am Ende des ersten `<PropertyGroup>`-Elements die folgenden Eigenschaften hinzu.

|**Projekt**|**Eigenschaften**|
|-------|----------|
|App|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

Entferne dann das MSBuild-Argument `AppxBundle` aus dem Buildschritt.

## <a name="related-topics"></a>Verwandte Themen

- [Erstellen einer .NET-App für Windows](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [Verpacken von UWP-Apps](/windows/msix/package/packaging-uwp-apps)
- [Querladen von branchenspezifischen Apps in Windows 10](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [Erstellen eines Paketsignaturzertifikats](/windows/msix/package/create-certificate-package-signing)
