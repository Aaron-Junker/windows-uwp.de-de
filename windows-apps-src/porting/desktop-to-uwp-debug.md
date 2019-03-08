---
Description: Führen Sie Ihre verpackte App aus und prüfen Sie, wie sie aussieht, ohne sie zu signieren. Legen Sie anschließend Haltepunkte fest und lassen Sie den Code schrittweise durchlaufen. Wenn Sie bereit sind, Ihre App in einer Produktionsumgebung zu testen, signieren Sie Ihre App und installieren Sie sie.
Search.Product: eADQiWindows 10XVcnh
title: Ausführen, Debuggen und Testen einer verpackten Desktop-App (Desktop-Brücke)
ms.date: 08/31/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: 8b2350c8164548121baec231335e747166f1c082
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589755"
---
# <a name="run-debug-and-test-a-packaged-desktop-application"></a>Ausführen, Debuggen und Testen einer App-Pakete desktop-Anwendung

Führen Sie die gepackte Anwendung, und sehen Sie, wie es aussieht, ohne es anmelden zu müssen. Legen Sie anschließend Haltepunkte fest und lassen Sie den Code schrittweise durchlaufen. Wenn Sie zum Testen Ihrer Anwendung in einer produktionsumgebung bereit sind, Signieren Sie Ihre Anwendung, und installieren Sie es. In diesem Thema erfahren Sie, wie Sie diese einzelnen Schritte ausführen.

<a id="run-app" />

## <a name="run-your-application"></a>Führen Sie die Anwendung

Sie können Ihre Anwendung lokal, ohne dass ein Zertifikat zu erhalten, und signieren Sie es testen ausführen. Wie Sie die Anwendung ausführen, was Ihnen tool hängt verwendet, um das Paket zu erstellen.

### <a name="you-created-the-package-by-using-visual-studio"></a>Erstellen des Pakets mit Visual Studio

Legen Sie das Paketerstellungsprojekt als Startprojekt fest, und drücken Sie anschließend Strg+F5, um Ihre App zu starten.

### <a name="you-created-the-package-manually-or-by-using-the-desktop-app-converter"></a>Erstellen des Pakets manuell oder mithilfe des Desktop App Converter

Öffnen Sie eine Windows PowerShell-Eingabeaufforderung, und führen Sie dieses Cmdlet aus dem Unterordner **PackageFiles** Ihres Ausgabeordners aus:

```
Add-AppxPackage –Register AppxManifest.xml
```
Finden Sie Ihre App im Windows-Startmenü, um sie auszuführen.

![Gepackte Anwendung im Startmenü](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> Eine gepackte Anwendung immer ausgeführt wird, als interaktiver Benutzer aus, und alle Festplatten, die Sie bei die gepackte Anwendung installieren muss NTFS-Format formatiert sein.

## <a name="debug-your-app"></a>Debuggen Sie Ihre App

Wie Sie die Anwendung debuggen, was Ihnen tool hängt verwendet, um das Paket zu erstellen.

Wenn Sie Ihr Paket mithilfe des in Version 15.4 von Visual Studio 2017 verfügbaren [neuen Paketerstellungsprojekts](desktop-to-uwp-packaging-dot-net.md#new-packaging-project) erstellt haben, legen Sie das Paketerstellungsprojekt als Startprojekt fest und drücken Sie F5, um Ihre App zu debuggen.

Wenn Sie Ihr Paket mit einem anderen Tool erstellt haben, gehen Sie folgendermaßen vor.

1. Stellen Sie sicher, dass Sie die gepackte Anwendung mindestens einmal beginnen, sodass sie auf dem lokalen Computer installiert ist.

   Informationen hierzu finden Sie im Abschnitt [Ausführen Ihrer App](#run-app) weiter oben.

2. Starten Sie Visual Studio.

   Wenn Sie Ihre Anwendung mit erweiterten Berechtigungen debuggen möchten, starten Sie Visual Studio mithilfe der **als Administrator ausführen** Option.

3. Wählen Sie in Visual Studio **Debuggen**->**Andere Debugziele**->**Installiertes App-Paket debuggen**.

4. In der **installierte App-Pakete** Liste, wählen Sie das App-Paket, und wählen Sie dann die **Anhängen** Schaltfläche.

#### <a name="modify-your-application-in-between-debug-sessions"></a>Ändern Sie die Anwendung zwischen einzelnen Debugsitzungen

Wenn Sie die Änderungen auf die Anwendung auf Fehler zu beheben vornehmen, verpackt es mit dem Tool MakeAppx neu. Informationen hierzu finden Sie unter [Ausführen des MakeAppx-Tools](desktop-to-uwp-manual-conversion.md#make-appx).

### <a name="debug-the-entire-application-lifecycle"></a>Debuggen Sie den gesamten Lebenszyklus der Anwendung

In einigen Fällen sollten Sie eine umfassendere Steuerung über den Debugprozess, einschließlich der Möglichkeit, Ihre Anwendung zu debuggen, bevor es gestartet wird.

Sie können [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) vollständige Kontrolle über Anwendungslebenszyklus, einschließlich anhalten, fortsetzen und beenden.

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) ist im Windows SDK enthalten.

## <a name="test-your-app"></a>Testen der App

Zum Testen der Anwendung in einer realistischen Umgebung, wie Sie für die Verteilung vorbereiten, empfiehlt es sich zum Signieren Ihrer Anwendung, und installieren Sie es.

### <a name="test-an-application-that-you-packaged-by-using-visual-studio"></a>Testen einer Anwendung, die Sie mithilfe von Visual Studio gepackt

Visual Studio signiert Ihre Anwendung mit einem Testzertifikat. Sie finden dieses Zertifikat im Ausgabeordner, den der Assistent **App-Pakete erstellen** generiert. Die Zertifikatdatei wurde die *CER* -Erweiterung, und Sie müssen zum Installieren dieses Zertifikats die **Trusted Root Certification Authorities** speichern, auf dem PC, die Sie für Ihre Anwendung testen möchten. Weitere Informationen finden Sie unter [Querladen des App-Pakets](../packaging/packaging-uwp-apps.md#sideload-your-app-package).

### <a name="test-an-application-that-you-packaged-by-using-the-desktop-app-converter-dac"></a>Testen einer Anwendung, die Sie mit der Desktop App Converter (DAC) gepackt

Wenn Sie Ihre Anwendung mithilfe der Desktop App Converter packen, können Sie mithilfe der ``sign`` Parameter, um die Anwendung automatisch mit einem generierten Zertifikat zu signieren. Sie müssen das Zertifikat und anschließend die App installieren. Weitere Informationen finden Sie unter [Ausführung der verpackten App](desktop-to-uwp-run-desktop-app-converter.md#run-app).   


### <a name="manually-sign-apps-optional"></a>Manuelles signieren von Apps (Optional)

Sie können Ihre Anwendung auch manuell signieren. So geht’s

1. Erstellen eines Zertifikats. Weitere Informationen finden Sie unter [Erstellen eines Zertifikats](../packaging/create-certificate-package-signing.md).

2. Installieren Sie das Zertifikat im Zertifikatsspeicher **Vertrauenswürdiger Stamm** oder **Vertrauenswürdige Personen** auf Ihrem System.

3. Mit diesem Zertifikat signieren Ihrer Anwendung, finden Sie unter [Signieren ein app-Pakets mithilfe von SignTool](../packaging/sign-app-package-using-signtool.md).

  > [!IMPORTANT]
  > Stellen Sie sicher, dass der Name des Herausgebers auf dem Zertifikat dem Namen des Herausgebers Ihrer App entspricht.

**Beispiel**

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-application-for-windows-10-s"></a>Testen Sie Ihre Anwendung für Windows 10 S

Bevor Sie Ihre app veröffentlichen, stellen Sie sicher, dass es korrekt funktioniert auf Geräten, auf denen Windows 10 s ausgeführt. In der Tat, wenn Sie Ihre Anwendung in der Microsoft Store veröffentlichen möchten, müssen Sie so vorgehen, da es sich um eine Store-Anforderung ist. Apps, die auf Geräten unter Windows 10 S nicht ordnungsgemäß funktionieren, werden nicht zertifiziert.

Finden Sie unter [testen Sie Ihre Windows-Anwendung für Windows 10 S](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s).

### <a name="run-another-process-inside-the-full-trust-container"></a>Ausführen eines anderen Prozesses im vollständig vertrauenswürdigen Container

Sie können benutzerdefinierte Prozesse im Container eines angegebenen App-Pakets aufrufen. Dies kann nützlich sein, um Szenarien zu testen (z. B. wenn Sie über eine benutzerdefinierte Testumgebung verfügen und die Ausgabe der App testen möchten). Verwenden Sie hierzu das PowerShell-Cmdlet ```Invoke-CommandInDesktopPackage```:

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>Nächste Schritte

**Hier finden Sie Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Geben Sie Feedback oder Vorschläge für Features**

Weitere Informationen finden Sie unter [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
