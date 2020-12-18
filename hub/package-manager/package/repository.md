---
title: Übermitteln des Manifests an das Repository
description: Nachdem Sie ein Paketmanifest erstellt, das Ihre Anwendung beschreibt, sind Sie bereit, Ihr Manifest an das Repository von Windows-Paket-Manager zu übermitteln.
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fef6cc96604639f84ee2c81e2de4fb28442e3f8d
ms.sourcegitcommit: 40b890c7b862f333879887cc22faff560c49eae6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97598801"
---
# <a name="submit-your-manifest-to-the-repository"></a>Übermitteln des Manifests an das Repository

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Nachdem Sie ein [Paketmanifest](manifest.md) erstellt, das Ihre Anwendung beschreibt, sind Sie bereit, Ihr Manifest an das Repository von Windows-Paket-Manager zu übermitteln. Dies ist ein öffentliches Repository, das eine Sammlung von Manifesten enthält, auf die das Tool **winget** zugreifen kann. Um Ihr Manifest zu übermitteln, laden Sie es in das Open Source-Repository [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) auf GitHub hoch.

Nachdem Sie eine Pull Request zum Hinzufügen eines neuen Manifests zum GitHub-Repository übermittelt haben, wird die Manifestdatei durch einen automatisierten Prozess validiert, um sicherzustellen, dass das Paket nicht als bösartig bekannt ist. Bei erfolgreicher Validierung wird das Paket dem öffentlichen Repository von Windows-Paket-Manager hinzugefügt, sodass es vom Clienttool **winget** erkannt werden kann. Beachten Sie die Unterscheidung zwischen den Manifesten im Open Source-GitHub-Repository und im öffentlichen Repository von Windows-Paket-Manager.

> [!IMPORTANT]
> Microsoft behält sich das Recht vor, eine Übermittlung gleich aus welchem Grund abzulehnen.

## <a name="third-party-repositories"></a>Repositorys von Drittanbietern

Derzeit gibt es keine bekannten Drittanbieter-Repositorys. Microsoft arbeitet mit mehreren Partnern an der Entwicklung von Protokollen oder einer API, um Drittanbieter-Repositorys zu aktivieren.

## <a name="manifest-validation"></a>Manifestvalidierung

Wenn Sie ein Manifest an das Repository [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) auf GitHub senden, wird das Manifest automatisch validiert und auf Sicherheit im Windows-Ökosystem geprüft. Manifeste können auch manuell überprüft werden.

## <a name="how-to-submit-your-manifest"></a>Übermitteln ihres Manifests

Führen Sie die folgenden Schritte aus, um ein Manifest an das Repository zu übermitteln.

### <a name="step-1-validate-your-manifest"></a>Schritt 1: Validieren des Manifests

Das Tool **winget-** stellt den Befehl [validate](..\winget\validate.md) bereit, um zu bestätigen, dass Sie das Manifest ordnungsgemäß erstellt haben. Verwenden Sie diesen Befehl, um das Manifest zu validieren.

```CMD
winget validate \<manifest-file>
```

Tritt bei der Überprüfung ein Fehler auf, suchen Sie anhand der Fehler die Zeilennummer, und nehmen Sie eine Korrektur vor. Nach dem Validieren des Manifests können Sie es an das Repository übermitteln.

### <a name="step-2-clone-the-repository"></a>Schritt 2: Klonen Sie das Repository

Erstellen Sie als nächstes eine Verzweigung des Repositorys, und klonen Sie diese.

1. Wechseln Sie im Browser zu [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs), und klicken Sie auf **Verzweigen**.
    ![Abbildung einer Verzweigung](images\fork.png)

2. Geben Sie in einer Befehlszeilenumgebung wie der Windows-Eingabeaufforderung oder PowerShell den folgenden Befehl an, um Ihre Verzweigung zu klonen.
    ```CMD
    git clone \<your-fork-name>
    ```

 3. Wenn Sie mehrere Übermittlungen vornehmen, erstellen Sie einen Branch anstelle einer Verzweigung. Derzeit ist nur eine Manifestdatei pro Übermittlung zulässig.
    ```CMD
    git checkout -b \<branch-name>
    ```

### <a name="step-3-add-your-manifest-to-the-local-repository"></a>Schritt 3: Hinzufügen des Manifests zum lokalen Repository

Sie müssen die Manifestdatei dem Repository in der folgenden Ordnerstruktur hinzufügen:

**manifests** / **publisher** / **application** / **version.yaml**

* Der Ordner **manifests** ist der Stammordner für alle Manifeste im Repository.
* Der Ordner **publisher** gibt den Namen des Unternehmens an, das die Software veröffentlicht. Dieser kann z. B. **Microsoft** lauten.
* Der Ordner **application** gibt den Namen der Anwendung oder des Tools an. Beispiel: **VSCode**.
* **version.yaml** ist der Dateiname des Manifests. Der Dateiname muss auf die aktuelle Version der Anwendung festgelegt werden. Beispiel: **1.0.0.yaml**.

>[!IMPORTANT]
> Der `Id`-Wert im Manifest muss mit dem Namen für Herausgeber und Anwendung im Ordnerpfad des Manifests identisch sein, und der `version`-Wert im Manifest muss der Version im Dateinamen entsprechen. Weitere Informationen finden Sie unter [Erstellen des Paketmanifests](manifest.md#tips-and-best-practices).

### <a name="step-4-submit-your-manifest-to-the-remote-repository"></a>Schritt 4: Übermitteln des Manifests an das Remoterepository

Nun können Sie das neue Manifest per Push an das Remoterepository übermitteln.

1. Bereiten Sie mit dem `add`-Befehl die Übermittlung vor.
    ```CMD
    git add manifests\Contoso\ContosoApp\1.0.0.yaml
    ```

2. Verwenden Sie den `commit`-Befehl, um einen Commit der Änderung auszuführen und Informationen zur Übermittlung anzugeben.
    ```CMD
    git commit -m "Submitting  ContosoApp version 1.0.0.yaml"
    ```

3. Übermitteln Sie mit dem `push`-Befehl die Änderungen per Push an das Remoterepository.
    ```CMD
    git push
    ```

### <a name="step-5-create-a-pull-request"></a>Schritt 5: Erstellen einer Pull-Anforderung

Nachdem Sie Ihre Änderungen per Push übertragen haben, kehren Sie zu [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) zurück, und erstellen Sie eine Pull Request, um die Verzweigung oder den Branch mit dem Hauptbranch zusammenzuführen.

![Abbildung der Registerkarte „Pull Request“](images\pull-request.png)

## <a name="validation-process"></a>Überprüfungsprozess

Beim Erstellen einer Pull Request wird ein Automatisierungsprozess gestartet, der das Manifest validiert und ihre Pull Request verarbeitet. Wir fügen der Pull-Anforderung Beschriftungen hinzu, damit Sie den Fortschritt nachverfolgen können.

### <a name="submission-expectations"></a>Erwartungen hinsichtlich der Übermittlung

Alle Anwendungsübermittlungen in das Repository von Windows-Paket-Manager müssen gut konzipiert sein. Im Folgenden sind einige Ansprüche erläutert, welche von Übermittlungen erfüllt werden müssen:

* Das Manifest entspricht den [Schemaanforderungen](manifest.md#manifest-contents).
* Alle URLs im Manifest führen zu sicheren Websites.
* Installer und Anwendung sind virenfrei. Das Paket kann versehentlich als Schadsoftware identifiziert werden. Wenn Sie der Meinung sind, dass es sich um ein falsch positives Ergebnis handelt, können Sie das Installationsprogramm [hierüber](https://www.microsoft.com/wdsi/filesubmission) an das Defender-Team zur Analyse senden.
* Die Anwendung kann sowohl von Administratoren als auch von Nicht-Administratoren ordnungsgemäß installiert und deinstalliert werden.
* Der Installer unterstützt nicht interaktive Modi.
* Alle Manifesteinträge sind genau und nicht irreführend.
* Der Installer stammt direkt von der Website des Herausgebers.

### <a name="pull-request-labels"></a>Pull Request-Beschriftungen

Während der Validierung wenden wir eine Reihe von Bezeichnungen auf die Pull Request an, um den Fortschritt zu kommunizieren.

* **Needs: author feedback** (Erforderlich: Autor-Feedback): Bei der Übermittlung ist ein Fehler aufgetreten. Der Pull Request wird Ihnen erneut zugewiesen. Wenn Sie das Problem nicht innerhalb von 10 Tagen beheben, schließen wir den Pull Request.
* **Manifest-Validation-Error** (Manifestvalidierungfehler): Das übermittelte Manifest enthält einen Syntaxfehler.
* **URL-Validation-Error** (URL-Validierungsfehler): Mindestens eine URLs in der Übermittlung hat die [SmartScreen](/windows/security/threat-protection/microsoft-defender-smartscreen/microsoft-defender-smartscreen-overview)-Validierung nicht bestanden.
* **Binary-Validation-Error** (Binärdateien-Validierungsfehler): Der übermittelte Anwendungsinstaller hat die Virenscantests nicht bestanden, oder ein Hashkonflikt liegt vor.
* **Pull-Request-Error** (Pull-Request-Fehler): Es liegt ein Problem mit dem Pull-Request vor. Die Ordnerstruktur weist beispielsweise nicht das [erforderliche Format](#step-3-add-your-manifest-to-the-local-repository) auf.
* **Validation-Error** (Validierungsfehler): Die übermittelte Anwendung hat einen allgemeinen Validierungstest nicht bestanden.
* **Validation-Installation-Error** (Validierungsfehler bei Installation): Die übermittelte Anwendung hat die Installationstests nicht bestanden.
* **Validation-Uninstall-Error** (Validierungsfehler bei Deinstallation): Die übermittelte Anwendung hat die Deinstallationstests nicht bestanden.
* **Validation-Virus-Scan-Error** (Validierungsfehler bei Virenscan): Die übermittelte Anwendung hat die Virenscantests nicht bestanden.
* **Azure-Pipeline-Passed** (Azure-Pipeline passiert): Der erste Teil der Validierung des Manifests wurde abgeschlossen. Nach diesem Schritt wird Ihr Pull Request unserem Testteam zur abschließenden Validierung zugewiesen.
* **Validation-Completed** (Validierung abgeschlossen): Die Validierung wurde abgeschlossen, und Ihr Pull Request wird zusammengeführt.