---
Description: Verteilen einer App-Pakete desktop-Anwendung (Desktop-Brücke)
title: Veröffentlichen Sie Ihre App-Pakete Desktopanwendung an den Microsoft Store oder per sideload übertragen sie auf einem oder mehreren Geräten.
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, UWP
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 45d298aca60155915900f494654dce8e89fb1ee0
ms.sourcegitcommit: b9e2cd5232ad98f4ef367881b92000a3ae610844
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67131907"
---
# <a name="distribute-your-packaged-desktop-app"></a>Verteilen Sie Ihre desktop-app-Paket

Wenn Sie entscheiden, [Packen Ihrer desktop-app in einem Paket MSIX](/windows/msix/desktop/desktop-to-uwp-root), Sie können die gepackte Anwendung in den Microsoft Store oder per sideload übertragen veröffentlichen auf einem oder mehreren Geräten.

> [!NOTE]
> Haben Sie einen Plan für die wie Benutzer der gepackten Anwendung Umstellung kann? Schauen Sie sich den Abschnitt [Umstellung von Benutzern auf Ihre verpackte App](#transition-users) dieses Handbuchs an, um eine Vorstellung davon zu bekommen, bevor Sie Ihre App verteilen.

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>Verteilen Sie Ihre Anwendung durch das Veröffentlichen auf dem Microsoft Store

Der [Microsoft Store](https://www.microsoft.com/store/apps) ist eine bequeme Möglichkeit für Kunden, Ihre App zu beziehen.

Veröffentlichen Sie Ihre Anwendung auf dem Microsoft Store das breiteste Publikum zu erreichen. Darüber hinaus erhalten Organisation Kunden Ihrer Anwendung intern in der eigenen Organisation durch Verteilen der [Microsoft Store für Unternehmen](https://www.microsoft.com/business-store).

Wenn Sie eine Veröffentlichung im Microsoft Store planen, werden Ihnen als Teil des Übermittlungsprozesses einige zusätzliche Fragen gestellt. Der Grund dafür ist, dass Ihr Paketmanifest eine eingeschränkte Funktion mit dem Namen **runFullTrust** deklariert und wir die Verwendung dieser Funktion durch Ihre Anwendung genehmigen müssen. Erfahren Sie mehr zu dieser Anforderung hier: [Eingeschränkte Funktionen](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities).

Sie müssen die Anwendung zu signieren, bevor Sie sie an den Store übermitteln.

>[!IMPORTANT]
> Wenn Sie Ihre Anwendung in der Microsoft Store veröffentlichen möchten, stellen Sie sicher, dass Ihre Anwendung auf Geräten, auf denen Windows 10 s ausgeführt, ordnungsgemäß ausgeführt wird Dies ist eine Store-Anforderung. Weitere Informationen finden Sie unter [Testen Ihrer Windows-App für Windows 10 S](/windows/msix/desktop/desktop-to-uwp-test-windows-s).

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>Vertrieb Ihrer Anwendung, ohne dass es auf dem Microsoft Store

Wenn Sie Ihre Anwendung stattdessen ohne Verwendung der Store verteilen würde, können Sie manuell ein oder mehrere Geräte-apps verteilen.

Dies eignet sich ggf., wenn Sie eine bessere Kontrolle über die Verteilung haben oder sich nicht mit dem Microsoft Store-Zertifizierungsprozess auseinandersetzen möchten.

Um Ihre Anwendung mit anderen Geräten ohne ablegen, damit er in den Store zu verteilen, müssen Sie ein Zertifikat zu erhalten, melden Sie sich Ihre Anwendung mit diesem Zertifikat und dann per sideload übertragen Ihrer Anwendung auf diesen Geräten.

Sie können [ein Zertifikat erstellen](/windows/uwp/packaging/create-certificate-package-signing) oder eines von einem beliebten Anbieter wie z. B. [Verisign](https://www.verisign.com/) erhalten.

Wenn die Anwendung auf Geräte zu verteilen, auf denen Windows 10 S ausgeführt werden sollen, muss Ihre Anwendung von der Microsoft Store signiert werden, daher müssen Sie den Store Übermittlung-Prozess durchlaufen zu lassen, bevor Sie Ihre Anwendung auf diesen Geräten verteilen können.

Wenn Sie ein Zertifikat zu erstellen, müssen Sie es im Zertifikatspeicher **Vertrauenswürdiger Stamm** oder **Vertrauenswürdige Personen** von jedem Gerät installieren, auf dem Ihre App ausgeführt wird. Wenn Sie ein Zertifikat von einem beliebten Anbieter erhalten, müssen Sie auf anderen Systemen neben Ihrer App nichts weiteres installieren.  

> [!IMPORTANT]
> Stellen Sie sicher, dass der Name des Herausgebers auf dem Zertifikat dem Namen des Herausgebers Ihrer App entspricht.

Um Ihre Anwendung mit einem Zertifikat zu signieren, finden Sie unter [melden Sie ein Anwendungspaket mithilfe von SignTool](/windows/uwp/packaging/sign-app-package-using-signtool).

Zum querladen Ihrer Anwendung auf anderen Geräten finden Sie unter [Querladen von branchenanwendungen in Windows 10-](/windows/application-management/sideload-apps-in-windows-10).

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>Umstellung von Benutzern auf Ihre verpackte App

Bevor Sie Ihre App verteilen, sollten Sie das Hinzufügen einiger Erweiterungen zu Ihrem Paketmanifest in Betracht ziehen, damit sich Benutzer daran gewöhnen, Ihre verpackte App zu verwenden. Hier sind einige Dinge, die Sie tun können.

* Verweisen Sie mit vorhandenen Startkacheln und Taskleistenschaltflächen auf Ihre verpackte App.
* Ordnen Sie einen Satz von Dateitypen mit der gepackten Anwendung.
* Stellen Sie der gepackten Anwendung bestimmte Arten von Dateien, die standardmäßig geöffnet.

Eine vollständige Liste der Erweiterungen und die Richtlinien für deren Verwendung finden Sie unter [Umstellung von Benutzern auf Ihre App](desktop-to-uwp-extensions.md#transition-users-to-your-app).

Berücksichtigen Sie auch das Hinzufügen von Code zu der gepackten Anwendung, die diese Aufgaben erledigt:

* Migrieren von Benutzerdaten, die die desktop-Anwendung zu den entsprechenden Ordnern von Ihrem app-Paket zugeordnet.
* Bereitstellung der Option für Benutzer, die Desktopversion Ihrer App zu deinstallieren

Erfahren wir mehr über diese einzelnen Aufgaben. Wir beginnen mit der Migration von Benutzerdaten.

### <a name="migrate-user-data"></a>Migrieren von Benutzerdaten

Wenn Sie vorhaben, Code hinzufügen, die Daten des Benutzers migriert werden, empfiehlt es sich um diesen Code nur ausgeführt, wenn die Anwendung zuerst gestartet wird. Bevor Sie die Benutzerdaten migrieren, zeigen Sie dem Benutzer ein Dialogfeld an, das erläutert, was passiert, warum es empfohlen wird und was mit den vorhandenen Daten geschehen wird.

Hier ist ein Beispiel, wie Sie dies in einer NET-basierten verpackten App erreichen können.

```csharp
private void MigrateUserData()
{
    String sourceDir = Environment.GetFolderPath
        (Environment.SpecialFolder.ApplicationData) + "\\AppName";

    if (sourceDir != null)
    {
        DialogResult migrateResult = MessageBox.Show
            ("Would you like to migrate your data from the previous version of this app?",
             "Data Migration", MessageBoxButtons.YesNo);

        if (migrateResult.Equals(DialogResult.Yes))
        {
            String destinationDir =
                Windows.Storage.ApplicationData.Current.LocalFolder.Path + "\\AppName";

            Process process = new Process();
            process.StartInfo.FileName = "robocopy.exe";
            process.StartInfo.Arguments = "%LOCALAPPDATA%\\AppName " + destinationDir + " /move";
            process.StartInfo.CreateNoWindow = true;
            process.Start();
            process.WaitForExit();

            if (process.ExitCode > 1)
            {
                //Migration was unsuccessful -- you can choose to block/retry/other action
            }
        }
    }
}
```

### <a name="uninstall-the-desktop-version-of-your-app"></a>Deinstallieren Sie die Desktopversion Ihrer App.

Es empfiehlt sich nicht um die Benutzer-desktop-Anwendung zu deinstallieren, ohne diese Berechtigung aufzufordern. Zeigen Sie ein Dialogfeld an, das den Benutzer nach dieser Berechtigung fragt. Benutzer möchten gegebenenfalls die Desktop-Version Ihrer App nicht deinstallieren. In diesem Fall müssen Sie entscheiden, ob blockiert die Verwendung von desktop-Anwendung oder die Seite-an-Seite-Verwendung von sowohl apps unterstützt werden sollen.

Hier ist ein Beispiel, wie Sie dies in einer NET-basierten verpackten App erreichen können.

Den vollständigen Kontext dieses Codeausschnitts finden Sie in der **MainWindow.cs**-Datei des Beispiels [WPF-Bildanzeige mit Übergang/Migration/Deinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition).

```csharp
private void RemoveDesktopApp()
{              
    //Typically, you can find your uninstall string at this location.
    String uninstallString = (String)Microsoft.Win32.Registry.GetValue
        (@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion" +
         @"\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

    //Detect if the previous version of the Desktop application is installed.
    if (uninstallString != null)
    {
        DialogResult uninstallResult = MessageBox.Show
            ("To have the best experience, consider uninstalling the "
              + " previous version of this app. Would you like to do that now?",
              "Uninstall the previous version", MessageBoxButtons.YesNo);

        if (uninstallResult.Equals(DialogResult.Yes))
        {
                    string[] uninstallArgs = uninstallString.Split(' ');

            Process process = new Process();
            process.StartInfo.FileName = uninstallArgs[0];
            process.StartInfo.Arguments = uninstallArgs[1];
            process.StartInfo.CreateNoWindow = true;

            process.Start();
            process.WaitForExit();

            if (process.ExitCode != 0)
            {
                //Uninstallation was unsuccessful - You can choose to block the application here.
            }
        }
    }

}
```

## <a name="next-steps"></a>Nächste Schritte

**Hier finden Sie Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

Wenn beim Veröffentlichen Ihrer Anwendung an den Store Probleme auftreten, enthält dieser [Blogbeitrag](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/) einige hilfreiche Tipps.

**Geben Sie Feedback oder Vorschläge für Features**

Weitere Informationen finden Sie unter [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
