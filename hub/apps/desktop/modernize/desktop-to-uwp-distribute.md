---
Description: Verteilen einer mit der Desktop-Brücke gepackten App
title: Veröffentliche deine gepackte Desktopanwendung im Microsoft Store, oder lade sie auf einem oder mehreren Geräten quer.
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 7eb57e8cea83a4d45087be4c4685ada8d108fa7a
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334497"
---
# <a name="distribute-your-packaged-desktop-app"></a>Verteilen einer gepackten Desktop-App

Wenn du deine [Desktop-App in einem MSIX-Paket packst](/windows/msix/desktop/desktop-to-uwp-root), kannst du die gepackte Anwendung anschließend im Microsoft Store veröffentlichen oder auf einem oder mehreren Geräten querladen.

> [!NOTE]
> Hast du einen Plan, wie du für Benutzer den Übergang auf deine gepackte Anwendung ermöglichen möchtest? Sieht dir den Abschnitt [Umstellung von Benutzern auf deine gepackte App](#transition-users) dieser Anleitung an, um einen Eindruck davon zu erhalten, bevor du deine App verteilst.

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>Verteilen der App durch Veröffentlichen im Microsoft Store

Der [Microsoft Store](https://www.microsoft.com/store/apps) ist eine bequeme Möglichkeit für Kunden, deine App zu beziehen.

Veröffentliche deine Anwendung im Microsoft Store, um eine möglichst große Zielgruppe zu erreichen. Darüber hinaus können Unternehmenskunden deine Anwendung erwerben und sie intern in ihren Organisationen über den [Microsoft Store für Unternehmen](https://businessstore.microsoft.com/store) verteilen.

Wenn du eine Veröffentlichung im Microsoft Store planst, werden dir im Rahmen des Übermittlungsprozesses einige zusätzliche Fragen gestellt. Der Grund dafür ist, dass dein Paketmanifest eine eingeschränkte Funktion mit dem Namen **runFullTrust** deklariert und wir die Verwendung dieser Funktion durch deine Anwendung genehmigen müssen. Weitere Informationen zu dieser Anforderung findest du hier: [Eingeschränkte Funktionen](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities).

Du musst deine Anwendung nicht signieren, bevor du sie an den Store übermittelst.

>[!IMPORTANT]
> Wenn du deine Anwendung im Microsoft Store veröffentlichen möchtest, stelle sicher, dass sie korrekt auf Geräten unter Windows 10 S ausgeführt wird. Dies ist eine Store-Anforderung. Weitere Informationen findest du unter [Testen deiner Windows-App für Windows 10 S](/windows/msix/desktop/desktop-to-uwp-test-windows-s).

<a id="side-load"></a>

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>Verteilen der App ohne Veröffentlichung im Microsoft Store

Wenn du deine Anwendung lieber verteilen möchtest, ohne sie im Store zu veröffentlichen, kannst du Apps auf einem oder mehreren Geräten manuell verteilen.

Dies eignet sich z. B., wenn du eine bessere Kontrolle über die Verteilung haben oder dich nicht mit dem Microsoft Store-Zertifizierungsprozess auseinandersetzen möchtest.

Um deine Anwendung auf anderen Geräten zu verteilen, ohne sie im Store zu veröffentlichen, musst du ein Zertifikat einholen, deine Anwendung mit diesem Zertifikat signieren und sie anschließend auf diese Geräte querladen.

Du kannst [ein Zertifikat erstellen](/windows/msix/package/create-certificate-package-signing) oder eines von einem gängigen Anbieter wie z. B. [Verisign](https://www.verisign.com/) erhalten.

Wenn du deine Anwendung auf Geräten mit Windows 10 S verteilen möchtest, muss deine Anwendung vom Microsoft Store signiert werden. Daher musst du den Prozess für die Store-Übermittlung durchlaufen, bevor du deine Anwendung auf diesen Geräten verteilen kannst.

Wenn du ein Zertifikat erstellst, musst du es im Zertifikatspeicher **Vertrauenswürdiger Stamm** oder **Vertrauenswürdige Personen** von jedem Gerät installieren, auf dem deine App ausgeführt wird. Wenn du ein Zertifikat von einem Drittanbieter erwirbst, musst du auf anderen Systemen neben deiner App nichts weiter installieren.  

> [!IMPORTANT]
> Stelle sicher, dass der Name des Herausgebers auf dem Zertifikat dem Namen des Herausgebers deiner App entspricht.

Informationen zum Signieren deiner Anwendung mithilfe eines Zertifikats findest du unter [Signieren eines Anwendungspakets mit SignTool](/windows/msix/package/sign-app-package-using-signtool).

Weitere Informationen zum Querladen deiner Anwendung auf anderen Geräten findest du unter [Querladen von branchenspezifischen Apps in Windows 10](/windows/application-management/sideload-apps-in-windows-10).

<a id="transition-users"></a>

## <a name="transition-users-to-your-packaged-app"></a>Umstellen von Benutzern auf eine gepackte App

Bevor du deine App verteilst, solltest du das Hinzufügen einiger Erweiterungen zu deinem Paketmanifest in Betracht ziehen, damit sich Benutzer daran gewöhnen, deine gepackte App zu verwenden. Hier sind einige Dinge, die du tun kannst.

* Verweise mit vorhandenen Startkacheln und Taskleistenschaltflächen auf deine gepackte App.
* Ordne deine gepackte Anwendung einer Gruppe von Dateitypen zu.
* Sorge dafür, dass deine gepackte Anwendung bestimmte Dateitypen standardmäßig öffnet.

Eine vollständige Liste der Erweiterungen und Richtlinien für deren Verwendung findest du unter [Umstellen von Benutzern auf deine App](desktop-to-uwp-extensions.md#transition-users-to-your-app).

Erwäge außerdem, deiner gepackten Anwendung Code hinzuzufügen, der die folgenden Aufgaben erledigt:

* Migrieren von Benutzerdaten, die deiner Desktopanwendung zugeordnet sind, zu den entsprechenden Ordnerspeicherorten deiner gepackten App
* Bereitstellen einer Option für Benutzer, die Desktopversion deiner App zu deinstallieren

Im Folgenden werden diese Aufgaben genauer beschrieben. Wir beginnen mit der Migration von Benutzerdaten.

### <a name="migrate-user-data"></a>Migrieren von Benutzerdaten

Wenn du Code hinzufügst, durch den Benutzerdaten migriert werden, empfiehlt es sich, diesen Code nur dann auszuführen, wenn die Anwendung das erste Mal gestartet wird. Bevor du die Benutzerdaten migrierst, solltest du den Benutzern ein Dialogfeld anzeigen, das erläutert, was passiert, warum dies empfohlen wird und was mit den vorhandenen Daten geschieht.

Hier ist ein Beispiel, wie du dies in einer .NET-basierten gepackten App erreichen kannst.

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

### <a name="uninstall-the-desktop-version-of-your-app"></a>Deinstallieren der Desktopversion deiner App

Die Desktopanwendung von Benutzern sollte nicht deinstalliert werden, ohne diese nach der Berechtigung zu fragen. Zeige ein Dialogfeld an, das den Benutzer um diese Berechtigung bittet. Benutzer möchten eventuell die Desktopversion deiner App nicht deinstallieren. In diesem Fall musst du entscheiden, ob du die Nutzung der Desktopanwendung blockieren oder die parallele Nutzung beider Apps unterstützen möchtest.

Hier ist ein Beispiel, wie du dies in einer .NET-basierten gepackten App erreichen kannst.

Den vollständigen Kontext dieses Codeausschnitts findest du in der Datei **MainWindow.cs** des Beispiels [WPF-Bildanzeige mit Übergang/Migration/Deinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition).

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

Haben Sie Fragen? Frage uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Du kannst uns auch [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D) fragen.

Wenn beim Veröffentlichen deiner Anwendung im Store Probleme auftreten, enthält dieser [Blogbeitrag](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/) einige hilfreiche Tipps.
