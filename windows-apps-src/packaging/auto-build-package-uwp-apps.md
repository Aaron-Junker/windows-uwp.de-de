---
title: Einrichten automatisierter Builds für UWP-Apps
description: Erfahren Sie, wie Sie automatisierte Builds konfigurieren, um Pakete zum Querladen oder zum Übermitteln an den Store zu erzeugen.
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, UWP
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 838bd9cb790893ea24b57bb2b0bad49aa262fdbc
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682529"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>Einrichten automatisierter Builds für UWP-Apps

Sie können Azure Pipelines verwenden, um automatisierte Builds für UWP-Projekte zu erstellen. In diesem Artikel werden verschiedene Möglichkeiten erläutert. Außerdem zeigen wir Ihnen, wie Sie diese Aufgaben mithilfe der Befehlszeile ausführen, damit Sie Sie in beliebige andere Buildsysteme integrieren können.

## <a name="create-a-new-azure-pipeline"></a>Erstellen einer neuen Azure-Pipeline

Melden Sie [sich zunächst für Azure Pipelines an,](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up) Wenn Sie dies noch nicht getan haben.

Erstellen Sie als nächstes eine Pipeline, die Sie zum Erstellen des Quellcodes verwenden können. Ein Tutorial zum Erstellen einer Pipeline zum Erstellen eines GitHub-Repository finden [Sie unter Erstellen Ihrer ersten Pipeline](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml). Azure Pipelines unterstützt die [in diesem Artikel](https://docs.microsoft.com/azure/devops/pipelines/repos)aufgeführten Repository-Typen.

## <a name="set-up-an-automated-build"></a>Einrichten eines automatisierten Builds

Wir beginnen mit der standardmäßigen UWP-Builddefinition, die in Azure dev OPS verfügbar ist, und zeigen Ihnen, wie Sie die Pipeline konfigurieren.

Wählen Sie in der Liste der Builddefinitionsvorlagen die Vorlage **Universelle Windows-Plattform** aus.

![UWP-Vorlage auswählen](images/select-yaml-template.png)

Diese Vorlage enthält die grundlegende Konfiguration zum Erstellen des UWP-Projekts:

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

Die Standardvorlage versucht, das Paket mit dem Zertifikat zu signieren, das in der CSPROJ-Datei angegeben ist. Wenn Sie das Paket während des Builds signieren möchten, müssen Sie Zugriff auf den privaten Schlüssel haben. Andernfalls können Sie die Signierung deaktivieren, indem Sie `/p:AppxPackageSigningEnabled=false` dem `msbuildArgs` Abschnitt in der YAML-Datei den-Parameter hinzufügen.

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>Fügen Sie das Projekt Zertifikat der Bibliothek für sichere Dateien hinzu.

Sie sollten das Übermitteln von Zertifikaten an Ihr Repository vermeiden, wenn dies möglich ist, und git diese standardmäßig ignoriert. Zum Verwalten der sicheren Behandlung vertraulicher Dateien, wie z. b. Zertifikate, unterstützt Azure devops das Feature " [sichere Dateien](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops) ".

So laden Sie ein Zertifikat für den automatisierten Build hoch:

1. Erweitern Sie in Azure Pipelines im Navigationsbereich **Pipelines** , und klicken Sie auf **Bibliothek**.
2. Klicken Sie auf die Registerkarte **sichere Dateien** , und klicken Sie dann auf **+ sichere Datei**.

    ![Hochladen einer sicheren Datei](images/secure-file1.png)

3. Navigieren Sie zur Zertifikat Datei, und klicken Sie auf **OK**.
4. Nachdem Sie das Zertifikat hochgeladen haben, wählen Sie es aus, um seine Eigenschaften anzuzeigen. Aktivieren Sie unter **Pipeline Berechtigungen**die UMSCHALT Fläche **für die Verwendung in allen Pipelines autorisieren** .

    ![Hochladen einer sicheren Datei](images/secure-file2.png)

5. Wenn das Zertifikat über ein Kennwort verfügt, empfiehlt es sich, das Kennwort in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) zu speichern und das Kennwort dann mit einer [Variablen Gruppe](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups)zu verknüpfen. Sie können die Variable verwenden, um auf das Kennwort aus der Pipeline zuzugreifen.

> [!NOTE]
> Ab Visual Studio 2019 wird ein temporäres Zertifikat nicht mehr in UWP-Projekten generiert. Um Zertifikate zu erstellen oder zu exportieren, verwenden Sie die in [diesem Artikel](/windows/msix/package/create-certificate-package-signing)beschriebenen PowerShell-Cmdlets.

## <a name="configure-the-build-solution-build-task"></a>Konfigurieren der Buildaufgabe „Projektmappe erstellen“

Mit diesem Task werden alle Lösungen, die sich im Arbeitsordner befinden, in Binärdateien kompiliert und die Ausgabe-App-Paketdatei erstellt. Diese Aufgabe verwendet MSBuild-Argumente. Sie müssen den Wert dieser Argumente angeben. Orientieren Sie sich an der folgenden Tabelle.

|**MSBuild-Argument**|**Wert**|**Beschreibung**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Definiert den Ordner, in dem die generierten Artefakte gespeichert werden. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Ermöglicht es Ihnen, die Plattformen zu definieren, die in das Paket eingeschlossen werden sollen. |
| AppxBundle | Immer | Erstellt ein. msixbundle/. appxbundle mit den msix-/AppX-Dateien für die angegebene Plattform. |
| UapAppxPackageBuildMode | StoreUpload | Generiert die. msixupload/. appxupload-Datei und den Ordner **_Test** für Sideloading. |
| UapAppxPackageBuildMode | CI | Generiert nur die. msixupload/. appxupload-Datei. |
| UapAppxPackageBuildMode | Sideloadonly | Generiert nur den Ordner **_Test** für Sideload. |
| Appxpackagesigningenabled | true | Aktiviert die Paket Signatur. |
| PackageCertificateThumbprint | Zertifikatfingerabdruck | Dieser Wert **muss** dem Fingerabdruck im Signaturzertifikat entsprechen oder eine leere Zeichenfolge sein. |
| PackageCertificateKeyFile | Pfad | Der Pfad zu dem Zertifikat, das verwendet werden soll. Dies wird aus den Metadaten der sicheren Datei abgerufen. |
| PackageCertificatePassword | Kennwort | Das Kennwort für das Zertifikat. Es wird empfohlen, dass Sie Ihr Kennwort in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) speichern und das Kennwort mit der [Variablen Gruppe](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups)verknüpfen. Sie können die Variable an dieses Argument übergeben. |

### <a name="configure-the-build"></a>Konfigurieren des Builds

Wenn Sie die Projekt Mappe mit der Befehlszeile oder mit einem anderen Buildsystem erstellen möchten, führen Sie MSBuild mit diesen Argumenten aus.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>Konfigurieren der Paket Signierung

Um das msix-Paket (oder das AppX-Paket) zu signieren, muss die Pipeline das Signaturzertifikat abrufen. Fügen Sie zu diesem Zweck vor der vsbuild-Aufgabe eine Aufgabe downloadsecurefile hinzu.
Dadurch erhalten Sie Zugriff auf das Signaturzertifikat über ```signingCert```.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

Aktualisieren Sie anschließend die vsbuild-Aufgabe, um auf das Signaturzertifikat zu verweisen:

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
> Das packagecertifierethumschlag-Print-Argument wird absichtlich als Vorsichtsmaßnahme auf eine leere Zeichenfolge festgelegt. Wenn der Fingerabdruck im Projekt festgelegt ist, aber nicht mit dem Signaturzertifikat identisch ist, schlägt der Build mit dem folgenden `Certificate does not match supplied signing thumbprint`Fehler fehl:.

### <a name="review-parameters"></a>Parameter überprüfen

Bei den mit der `$()` Syntax definierten Parametern handelt es sich um Variablen, die in der Builddefinition definiert sind, und in anderen Buildsystemen.

![Standardvariablen](images/building-screen5.png)

Informationen zum Anzeigen aller vordefinierten Variablen finden Sie unter [vordefinierte](https://docs.microsoft.com/azure/devops/pipelines/build/variables)Buildvariablen.

## <a name="configure-the-publish-build-artifacts-task"></a>Konfigurieren der Aufgabe "Build-Artefakte veröffentlichen"

Die UWP-Standard Pipeline speichert die generierten Artefakte nicht. Fügen Sie die folgenden Aufgaben hinzu, um die Veröffentlichungs Funktionen Ihrer YAML-Definition hinzuzufügen.

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

Die generierten Artefakte werden in der Option **Artefakte** der Seite Buildergebnisse angezeigt.

![Artefakte](images/building-screen6.png)

Da wir das `UapAppxPackageBuildMode` -Argument auf `StoreUpload`festgelegt haben, enthält der artefaktordner das Paket für die Übermittlung an den Speicher (. msixupload/. appxupload). Beachten Sie, dass Sie auch eine reguläre App-für (. msix/. AppX) oder eine APP Bundle (. msixbundle/. appxbundle/) an den Speicher übermitteln können. Für die Zwecke dieses Artikels verwenden wir die appxupload-Datei.

## <a name="address-bundle-errors"></a>Adress Bündel Fehler

Wenn Sie Ihrer Projekt Mappe mehrere UWP-Projekte hinzufügen und dann versuchen, ein Paket zu erstellen, erhalten Sie möglicherweise eine Fehlermeldung wie die folgende.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

Dieser Fehler tritt auf, da auf Projektmappenebene nicht eindeutig ist, welche App im Bündel enthalten sein soll. Um dieses Problem zu beheben, öffnen Sie jede Projektdatei, und fügen Sie am Ende des ersten `<PropertyGroup>` Elements die folgenden Eigenschaften hinzu.

|**Projekt**|**Eigenschaften**|
|-------|----------|
|App|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

Entfernen Sie dann das `AppxBundle` MSBuild-Argument aus dem Buildschritt.

## <a name="related-topics"></a>Verwandte Themen

- [Erstellen Ihrer .net-App für Windows](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [Packen von UWP-apps](/windows/msix/package/packaging-uwp-apps)
- [Querladen von Branchen-apps in Windows 10](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [Erstellen eines Zertifikats für die Paket Signierung](/windows/msix/package/create-certificate-package-signing)
