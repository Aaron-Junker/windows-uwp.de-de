---
Description: Eine mit Desktop Bridge Paketierte App verteilen
title: Veröffentlichen Sie die gepackte Desktop Anwendung im Microsoft Store, oder querladen Sie Sie auf einem oder mehreren Geräten.
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 15970afbeb5d9dee1c2079cd5933b1250ecb2f09
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74734776"
---
# <a name="distribute-your-packaged-desktop-app"></a>Verteilen der APP für gepackte Desktops

Wenn Sie [Ihre Desktop-app in einem msix-Paket Verpacken](/windows/msix/desktop/desktop-to-uwp-root)möchten, können Sie die gepackte Anwendung im Microsoft Store veröffentlichen oder auf einem oder mehreren Geräten querladen.

> [!NOTE]
> Haben Sie einen Plan für die Umstellung von Benutzern auf Ihre gepackte Anwendung? Schauen Sie sich den Abschnitt [Umstellung von Benutzern auf Ihre verpackte App](#transition-users) dieses Handbuchs an, um eine Vorstellung davon zu bekommen, bevor Sie Ihre App verteilen.

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>Verteilen Sie Ihre Anwendung, indem Sie Sie in der Microsoft Store veröffentlichen.

Der [Microsoft Store](https://www.microsoft.com/store/apps) ist eine bequeme Möglichkeit für Kunden, Ihre App zu beziehen.

Veröffentlichen Sie Ihre Anwendung auf dem Microsoft Store, um die breiteste Zielgruppe zu erreichen. Außerdem können Organisations Kunden Ihre Anwendung zur internen Verteilung an ihre Organisationen über die [Microsoft Store für Unternehmen](https://businessstore.microsoft.com/store)erwerben.

Wenn Sie eine Veröffentlichung im Microsoft Store planen, werden Ihnen als Teil des Übermittlungsprozesses einige zusätzliche Fragen gestellt. Der Grund dafür ist, dass Ihr Paketmanifest eine eingeschränkte Funktion mit dem Namen **runFullTrust** deklariert und wir die Verwendung dieser Funktion durch Ihre Anwendung genehmigen müssen. Weitere Informationen zu dieser Anforderung finden Sie hier: [Eingeschränkte Funktionen](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities).

Sie müssen Ihre Anwendung nicht signieren, bevor Sie Sie an den Store übermitteln.

>[!IMPORTANT]
> Wenn Sie beabsichtigen, die Anwendung auf dem Microsoft Store zu veröffentlichen, stellen Sie sicher, dass Ihre Anwendung auf Geräten mit Windows 10 S ordnungsgemäß ausgeführt wird. Dies ist eine Speicher Anforderung. Weitere Informationen finden Sie unter [Testen Ihrer Windows-App für Windows 10 S](/windows/msix/desktop/desktop-to-uwp-test-windows-s).

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>Verteilen Sie Ihre Anwendung, ohne Sie auf die Microsoft Store

Wenn Sie Ihre Anwendung lieber ohne den Store verteilen möchten, können Sie apps manuell auf ein oder mehrere Geräte verteilen.

Dies eignet sich ggf., wenn Sie eine bessere Kontrolle über die Verteilung haben oder sich nicht mit dem Microsoft Store-Zertifizierungsprozess auseinandersetzen möchten.

Wenn Sie Ihre Anwendung an andere Geräte verteilen möchten, ohne Sie in den Speicher zu versetzen, müssen Sie ein Zertifikat abrufen, die Anwendung mit diesem Zertifikat signieren und die Anwendung dann auf diese Geräte querladen.

Sie können [ein Zertifikat erstellen](/windows/msix/package/create-certificate-package-signing) oder eines von einem beliebten Anbieter wie z. B. [Verisign](https://www.verisign.com/) erhalten.

Wenn Sie Ihre Anwendung auf Geräte verteilen möchten, auf denen Windows 10 S ausgeführt wird, muss die Anwendung von der Microsoft Store signiert werden, damit Sie den Übermittlungs Prozess des Stores durchlaufen müssen, bevor Sie Ihre Anwendung auf diese Geräte verteilen können.

Wenn Sie ein Zertifikat zu erstellen, müssen Sie es im Zertifikatspeicher **Vertrauenswürdiger Stamm** oder **Vertrauenswürdige Personen** von jedem Gerät installieren, auf dem Ihre App ausgeführt wird. Wenn Sie ein Zertifikat von einem beliebten Anbieter erhalten, müssen Sie auf anderen Systemen neben Ihrer App nichts weiteres installieren.  

> [!IMPORTANT]
> Stellen Sie sicher, dass der Name des Herausgebers auf dem Zertifikat dem Namen des Herausgebers Ihrer App entspricht.

Informationen zum Signieren der Anwendung mit einem Zertifikat finden Sie unter [Signieren eines Anwendungspakets mit SignTool](/windows/msix/package/sign-app-package-using-signtool).

Informationen zum querladen Ihrer Anwendung auf andere Geräte finden Sie unter querladen von branchenspezifischen [apps in Windows 10](/windows/application-management/sideload-apps-in-windows-10).

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>Umstellung von Benutzern auf Ihre verpackte App

Bevor Sie Ihre App verteilen, sollten Sie das Hinzufügen einiger Erweiterungen zu Ihrem Paketmanifest in Betracht ziehen, damit sich Benutzer daran gewöhnen, Ihre verpackte App zu verwenden. Hier sind einige Dinge, die Sie tun können.

* Verweisen Sie mit vorhandenen Startkacheln und Taskleistenschaltflächen auf Ihre verpackte App.
* Ordnen Sie die gepackte Anwendung einem Satz von Dateitypen zu.
* Öffnen Sie die gepackte Anwendung standardmäßig für bestimmte Dateitypen.

Eine vollständige Liste der Erweiterungen und die Richtlinien für deren Verwendung finden Sie unter [Umstellung von Benutzern auf Ihre App](desktop-to-uwp-extensions.md#transition-users-to-your-app).

Fügen Sie außerdem Code zu Ihrer APP-Paket Anwendung hinzu, die diese Aufgaben erledigt:

* Migriert die der Desktop Anwendung zugeordneten Benutzerdaten zu den entsprechenden Ordner Speicherorten Ihrer APP mit App-Paketen.
* Bereitstellung der Option für Benutzer, die Desktopversion Ihrer App zu deinstallieren

Erfahren wir mehr über diese einzelnen Aufgaben. Wir beginnen mit der Migration von Benutzerdaten.

### <a name="migrate-user-data"></a>Migrieren von Benutzerdaten

Wenn Sie Code hinzufügen, mit dem Benutzerdaten migriert werden, ist es am besten, den Code nur beim ersten Start der Anwendung auszuführen. Bevor Sie die Benutzerdaten migrieren, zeigen Sie dem Benutzer ein Dialogfeld an, das erläutert, was passiert, warum es empfohlen wird und was mit den vorhandenen Daten geschehen wird.

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

Es ist besser, die Desktop Anwendung "Users" nicht zu deinstallieren, ohne Sie zuvor zur Berechtigung aufzufordern. Zeigen Sie ein Dialogfeld an, das den Benutzer nach dieser Berechtigung fragt. Benutzer möchten gegebenenfalls die Desktop-Version Ihrer App nicht deinstallieren. Wenn dies der Fall ist, müssen Sie entscheiden, ob Sie die Verwendung der Desktop Anwendung blockieren oder die parallele Verwendung beider apps unterstützen möchten.

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

**Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

Wenn beim Veröffentlichen Ihrer Anwendung an den Store Probleme auftreten, enthält dieser [Blogbeitrag](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/) einige hilfreiche Tipps.

**Feedback geben oder Funktions Vorschläge machen**

Weitere Informationen finden Sie unter [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
