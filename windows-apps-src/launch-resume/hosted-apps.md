---
Description: Erfahren Sie, wie Sie eine gehostete app erstellen, die die ausführbaren Attribute, den Einstiegspunkt und die Runtime-Attribute einer Host-App erbt.
title: Erstellen gehosteter Apps
ms.date: 04/23/2020
ms.topic: article
keywords: Windows 10, Desktop, Paket, Identität, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3424b7a0ead3d4a3abeebc6286aebe935d09eab2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172994"
---
# <a name="create-hosted-apps"></a>Erstellen gehosteter Apps

Ab Windows 10, Version 2004, können Sie *gehostete Apps*erstellen. Eine gehostete App nutzt dieselbe ausführbare Datei und Definition wie eine übergeordnete *Host* -APP, sieht aber wie eine separate App auf dem System aus.

Gehostete Apps sind nützlich für Szenarios, in denen sich eine Komponente (z. B. eine ausführbare Datei oder eine Skriptdatei) wie eine eigenständige Windows 10-App verhalten soll. Die Komponente erfordert jedoch einen Hostprozess, um ausgeführt werden zu können. Beispielsweise könnte ein PowerShell-oder Python-Skript als gehostete App übermittelt werden, für die ein Host installiert werden muss, um ausgeführt werden zu können. Eine gehostete App kann über eine eigene Startkachel, eine eigene Identität und eine umfassende Integration in Windows 10-Features verfügen, z. B. Hintergrundaufgaben, Benachrichtigungen, Kacheln und Freigabeziele.

Die Funktion "gehostete Apps" wird von mehreren Elementen und Attributen im Paket Manifest unterstützt, die einer gehosteten App ermöglichen, eine ausführbare Datei und eine Definition in einem Host-App-Paket zu verwenden. Wenn ein Benutzer die gehostete App ausführt, wird die ausführbare Datei des Hosts automatisch von dem Betriebssystem unter der Identität der gehosteten App gestartet. Der Host kann dann visuelle Assets, Inhalte oder Aufrufe von APIs als gehostete App laden. Die gehostete APP erhält die Schnittmenge der Funktionen, die zwischen dem Host und der gehosteten App deklariert werden. Dies bedeutet, dass eine gehostete APP nicht mehr Funktionen anfordern kann, als Sie vom Host bereitstellt wird.

## <a name="define-a-host"></a>Definieren eines Hosts

Der *Host* ist die hauptausführ Bare Datei oder der Lauf Zeit Prozess für die gehostete app. Zurzeit sind die einzigen unterstützten Hosts Desktop-Apps (.net oder C++/Win32), die über die *Paket Identität*verfügen. UWP-apps werden zurzeit nicht als Hosts unterstützt. Für eine Desktop-App gibt es mehrere Möglichkeiten, die Paket Identität zu haben:

* Die gängigste Methode zum gewähren einer Paket Identität an eine Desktop-App besteht darin, [Sie in einem msix-Paket zu verpacken](/windows/msix).
* In einigen Fällen können Sie alternativ die Paket Identität durch Erstellen eines [sparsepakets](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)erteilen. Diese Option ist nützlich, wenn Sie keine msix-Paket Erstellung für die Bereitstellung Ihrer Desktop-App durchführen können.

Der Host wird von der [**uap10: hostrauuntime**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntime) -Erweiterung im Paket Manifest deklariert. Diese Erweiterung verfügt über ein **ID** -Attribut, dem ein Wert zugewiesen werden muss, auf den auch vom Paket Manifest für die gehostete App verwiesen wird. Wenn die gehostete App aktiviert ist, wird der Host unter der Identität der gehosteten App gestartet und kann Inhalte oder Binärdateien aus dem gehosteten App-Paket laden.

Im folgenden Beispiel wird veranschaulicht, wie Sie einen Host in einem Paket Manifest definieren. Die " **uap10:** "-Erweiterung ist Paket weit und wird daher als untergeordnetes Element des [**Package**](/uwp/schemas/appxpackage/uapmanifestschema/element-package) -Elements deklariert.

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Extensions>
    <uap10:Extension Category="windows.hostRuntime"  
        Executable="PyScriptEngine\PyScriptEngine.exe"  
        uap10:RuntimeBehavior="packagedClassicApp"  
        uap10:TrustLevel="mediumIL">
      <uap10:HostRuntime Id="PythonHost" />
    </uap10:Extension>
  </Extensions>

</Package>
```

Notieren Sie sich diese wichtigen Details zu den folgenden Elementen.

| Element              | Details |
|----------------------|-------|
| [**uap10:Extension**](/wp/schemas/appxpackage/uapmanifestschema/element-uap10-extension) | Die `windows.hostRuntime` Kategorie deklariert eine Paket weite Erweiterung zum Definieren der Laufzeitinformationen, die beim Aktivieren einer gehosteten App verwendet werden sollen. Eine gehostete APP wird mit den in der Erweiterung deklarierten Definitionen ausgeführt. Wenn Sie die im vorherigen Beispiel deklarierte Host-App verwenden, wird eine gehostete App als ausführbare Datei **PyScriptEngine.exe** auf der **mediumil** -Vertrauens Ebene ausgeführt.<br/><br/>Die Attribute " **ausführbare**", " **uap10: runtimebehavior**" und " **uap10: TrustLevel** " geben den Namen der Binärdatei des Host Prozesses im Paket an und wie die gehosteten apps ausgeführt werden. Beispielsweise wird eine gehostete APP, die die Attribute im vorherigen Beispiel verwendet, als ausführbare Datei PyScriptEngine.exe auf der mediumil-Vertrauens Ebene ausgeführt. |
| [**uap10:HostRuntime**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntime) | Das **ID** -Attribut deklariert den eindeutigen Bezeichner dieser bestimmten Host-App im Paket. Ein Paket kann über mehrere Host-apps verfügen, und jedes muss über ein **uap10: hostrauuntime** -Element mit einer eindeutigen **ID**verfügen.

## <a name="declare-a-hosted-app"></a>Deklarieren einer gehosteten App

Eine *gehostete App* deklariert eine Paketabhängigkeit auf einem *Host*. Die gehostete App nutzt die Host-ID (d. h. das **ID** -Attribut der **uap10: hostrauuntime** -Erweiterung im Host Paket) zur Aktivierung, anstatt eine ausführbare Einstiegspunkt Datei im eigenen Paket anzugeben. Die gehostete app enthält in der Regel Inhalte, visuelle Objekte, Skripts oder Binärdateien, auf die der Host zugreifen kann. Der [**targetdevicefamily**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) -Wert im gehosteten App-Paket sollte auf denselben Wert wie der Host abzielen.

Gehostete App-Pakete können signiert oder nicht signiert werden:

* Signierte Pakete können ausführbare Dateien enthalten. Dies ist nützlich in Szenarien mit einem binären Erweiterungsmechanismus, der es dem Host ermöglicht, eine DLL oder eine registrierte Komponente in das gehostete App-Paket zu laden.
* Nicht signierte Pakete können nur nicht ausführbare Dateien enthalten. Dies ist in Szenarien nützlich, in denen der Host nur Bilder, Ressourcen und Inhalte oder Skriptdateien laden muss. Nicht signierte Pakete müssen einen speziellen `OID` Wert in Ihrem [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) -Element enthalten, oder Sie dürfen nicht registriert werden. Dadurch wird verhindert, dass nicht signierte Pakete mit der Identität eines signierten Pakets in Konflikt stehen oder Sie Spoofing.

Um eine gehostete APP zu definieren, deklarieren Sie die folgenden Elemente im Paket Manifest:

* Das [**uap10: ":**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntimedependency) "-Element. Dies ist ein untergeordnetes Element des Elements [Abhängigkeiten](/uwp/schemas/appxpackage/uapmanifestschema/element-dependencies) .
* Das **uap10: hostid-** Attribut des [**Application**](/uwp/schemas/appxpackage/uapmanifestschema/element-application) -Elements (für eine APP) oder das [**Erweiterungs**](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) Element (für eine aktivierbare Erweiterung).

Im folgenden Beispiel werden die relevanten Abschnitte eines Paket Manifests für eine nicht signierte gehostete App veranschaulicht.

``` xml
<Package xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10">

  <Identity Name="NumberGuesserManifest"
    Publisher="CN=AppModelSamples, OID.2.25.311729368913984317654407730594956997722=1"
    Version="1.0.0.0" />

  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19041.0" MaxVersionTested="10.0.19041.0" />
    <uap10:HostRuntimeDependency Name="PyScriptEnginePackage" Publisher="CN=AppModelSamples" MinVersion="1.0.0.0"/>
  </Dependencies>

  <Applications>
    <Application Id="NumberGuesserApp"  
      uap10:HostId="PythonHost"  
      uap10:Parameters="-Script &quot;NumberGuesser.py&quot;">
    </Application>
  </Applications>

</Package>
```

Notieren Sie sich diese wichtigen Details zu den folgenden Elementen.

| Element              | Details |
|----------------------|-------|
| [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) | Da das in diesem Beispiel gehostete App-Paket nicht signiert ist, muss das **Publisher** -Attribut die `OID.2.25.311729368913984317654407730594956997722=1` Zeichenfolge enthalten. Dadurch wird sichergestellt, dass das nicht signierte Paket die Identität eines signierten Pakets nicht verspo, kann. |
| [**TargetDeviceFamily**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) | Das **MinVersion** -Attribut muss 10.0.19041.0 oder eine spätere Betriebssystemversion angeben. |
| [**uap10:HostRuntimeDependency**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-hostruntimedependency)  | Dieses Element deklariert eine Abhängigkeit vom Host-App-Paket. Dies besteht aus dem **Namen** und dem **Verleger** des Host Pakets und der **MinVersion** , von der es abhängt. Diese Werte finden Sie unter dem [Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) -Element im Host Paket. |
| [**Anwendung**](/uwp/schemas/appxpackage/uapmanifestschema/element-application) | Das **uap10: hostid-** Attribut drückt die Abhängigkeit auf dem Host aus. Das gehostete App-Paket muss dieses Attribut anstelle der üblichen **ausführbaren** Attribute und **entryPoint** -Attribute für ein [**Anwendungs**](/uwp/schemas/appxpackage/uapmanifestschema/element-application) -oder [**Erweiterungs**](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) Element deklarieren. Folglich erbt die gehostete APP die Attribute für **ausführbare**, **entryPoint** und Runtime vom Host mit dem entsprechenden **HostID-** Wert.<br/><br/>Das Attribut **uap10: Parameters** gibt Parameter an, die an die Einstiegspunkt Funktion der ausführbaren Datei des Hosts übermittelt werden. Da der Host wissen muss, was mit diesen Parametern zu tun ist, gibt es einen impliziten Vertrag zwischen dem Host und der gehosteten app. |

## <a name="register-an-unsigned-hosted-app-package-at-run-time"></a>Registrieren eines nicht signierten gehosteten App-Pakets zur Laufzeit

Ein Vorteil der **uap10: hostrauuntime** -Erweiterung besteht darin, dass ein Host ein gehostetes App-Paket zur Laufzeit dynamisch generieren und mithilfe der [**packagemanager**](/uwp/api/windows.management.deployment.packagemanager) -API registrieren kann, ohne es signieren zu müssen. Auf diese Weise kann ein Host den Inhalt und das Manifest für das gehostete App-Paket dynamisch generieren und dann registrieren.

Verwenden Sie die folgenden Methoden der [**packagemanager**](/uwp/api/windows.management.deployment.packagemanager) -Klasse, um ein nicht signiertes gehostete App-Paket zu registrieren. Diese Methoden sind ab Windows 10, Version 2004, verfügbar.

* [**Addpackagebyuriasync**](/uwp/api/windows.management.deployment.packagemanager.addpackagebyuriasync): registriert ein nicht signiertes msix-Paket, indem die Eigenschaft " **Zuteilungen** " des Parameters " *options* " verwendet wird.
* [**Registerpackagebyuriasync**](/uwp/api/windows.management.deployment.packagemanager.registerpackagebyuriasync): führt eine lose Paket Manifest-Datei Registrierung aus. Wenn das Paket signiert ist, muss der Ordner, der das Manifest enthält, eine [. P7X "-Datei](/windows/msix/overview#inside-an-msix-package) und einen-Katalog enthalten. Wenn Sie nicht signiert ist, muss die Eigenschaft " **zugwunsigned** " des *options* -Parameters festgelegt werden.

### <a name="requirements-for-unsigned-hosted-apps"></a>Anforderungen für nicht signierte gehostete Apps

* Die [**Anwendungs**](/uwp/schemas/appxpackage/uapmanifestschema/element-application) -oder [**Erweiterungs**](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) Elemente im Paket Manifest dürfen keine Aktivierungsdaten enthalten, wie z. b **. die Attribute**"exe", " **entryPoint**" oder " **TrustLevel** " Stattdessen können diese Elemente nur ein **uap10: hostid-** Attribut enthalten, das die Abhängigkeit des Hosts und eines **uap10: Parameters** -Attributs ausdrückt.
* Das Paket muss ein Hauptpaket sein. Es darf sich nicht um ein Bündel, ein frameworkpaket, eine Ressource oder ein optionales Paket handeln.

### <a name="requirements-for-a-host-that-installs-and-registers-an-unsigned-hosted-app-package"></a>Anforderungen für einen Host, von dem ein nicht signiertes gehostete App-Paket installiert und registriert wird

* Der Host muss über eine [Paket Identität](#define-a-host)verfügen.
* Der Host muss über die eingeschränkte **packagemanagement** - [Funktion](../packaging/app-capability-declarations.md#restricted-capabilities)verfügen.
    ```xml
    <rescap:Capability Name="packageManagement" />
    ```

<!--
* If the host runs in app container (for example, it is a UWP app), it must also have the unsigned package management [custom capability](../packaging/app-capability-declarations.md#custom-capabilities) and a [Signed Custom Capability Descriptor (SCCD) file](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file).
    ```xml
    <uap4:CustomCapability Name="Microsoft.unsignedPackageManagement_cw5n1h2txyewy" />
    ```
-->

## <a name="sample"></a>Beispiel

Eine voll funktionsfähige Beispiel-APP, die sich selbst als Host deklariert und dann ein gehostetes App-Paket zur Laufzeit dynamisch registriert, finden Sie unter [Beispiel für gehostete Apps](https://github.com/microsoft/AppModelSamples/tree/master/Samples/HostedApps).

### <a name="the-host"></a>Host

Der Host hat den Namen **pyscriptengine**. Dabei handelt es sich um einen in c# geschriebenen Wrapper, der python-Skripts ausführt. Wenn Sie mit dem- `-Register` Parameter ausführen, wird von der Skript-Engine eine gehostete App mit einem Python-Skript installiert. Wenn ein Benutzer versucht, die neu installierte gehostete APP zu starten, wird der Host gestartet, und das Python-Skript " **Zahlungszeit** " wird ausgeführt.

Das Paket Manifest für die Host-app (die Datei "Package. appxmanifest" im Ordner "pyscriptenginepackage") enthält eine **uap10: hostrauuntime** -Erweiterung, die die APP als Host mit der ID " **pythonhost** " und der ausführbaren **PyScriptEngine.exe**deklariert.  

> [!NOTE]
> In diesem Beispiel heißt das Paket Manifest "Package. appxmanifest" und ist Teil eines Windows- [anwendungspaketprojekts](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net). Wenn dieses Projekt erstellt wird, [generiert es ein Manifest mit dem Namen AppxManifest.xml](/uwp/schemas/appxpackage/uapmanifestschema/generate-package-manifest) und erstellt das msix-Paket für die Host-app.

### <a name="the-hosted-app"></a>Die gehostete App

Die gehostete App besteht aus einem Python-Skript und Paket Artefakten, wie z. b. dem Paket Manifest. Sie enthält keine PE-Dateien.

Das Paket Manifest für die gehostete app (die Datei "zahlername/AppxManifest.xml") enthält die folgenden Elemente:

* Das **Publisher** -Attribut des [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) -Elements enthält den `OID.2.25.311729368913984317654407730594956997722=1` identierer, der für ein nicht signiertes Paket erforderlich ist.
* Das **uap10: hostid-** Attribut des [**Application**](/uwp/schemas/appxpackage/uapmanifestschema/element-application) -Elements identifiziert **pythonhost** als Host.

### <a name="run-the-sample"></a>Ausführen des Beispiels

Das Beispiel erfordert die Version 10.0.19041.0 oder höher von Windows 10 und die Windows SDK.

1. Laden Sie das [Beispiel](https://aka.ms/hostedappsample) in einen Ordner auf Ihrem Entwicklungs Computer herunter.
2. Öffnen Sie die Projekt Mappe pyscriptengine. sln in Visual Studio, und legen Sie das **pyscriptenginepackage** -Projekt als Startprojekt fest.
3. Erstellen Sie das **pyscriptenginepackage** -Projekt.
4. Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **pyscriptenginepackage** , **und wählen Sie**bereitstellen aus.
5. Öffnen Sie ein Eingabe Aufforderungs Fenster für das Verzeichnis, in das Sie die Beispieldateien kopiert haben, und führen Sie den folgenden Befehl aus, um **die Beispiel-** APP zu registrieren (die gehostete APP). Wechseln `D:\repos\HostedApps` Sie zu dem Pfad, in den Sie die Beispieldateien kopiert haben.

    ```CMD
    D:\repos\HostedApps>pyscriptengine -Register D:\repos\HostedApps\NumberGuesser\AppxManifest.xml
    ```

    > [!NOTE]
    > Sie können `pyscriptengine` in der Befehlszeile ausführen, da der Host im Beispiel einen [**appexecutionalias**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias)deklariert.

6. Öffnen Sie das **Startmenü** , und klicken Sie auf " **zahlender** ", um die gehostete App auszuführen.