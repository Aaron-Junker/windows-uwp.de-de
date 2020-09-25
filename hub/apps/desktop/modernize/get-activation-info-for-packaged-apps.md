---
description: Erfahren Sie, wie Sie bestimmte Arten von Aktivierungsinformationen für gepackte .NET- und native C++/Win32-Apps abrufen
title: Erhalten von Aktivierungsinformationen für App-Pakete
ms.date: 09/17/2020
ms.topic: article
keywords: Windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 748646ce335e9e68ee7a22131ebd305db94e9801
ms.sourcegitcommit: 5d7168ebc9f43aa13051446aff45a46600e6aafe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2020
ms.locfileid: "90799849"
---
# <a name="get-activation-info-for-packaged-apps"></a>Erhalten von Aktivierungsinformationen für App-Pakete

Ab Windows 10, Version 1809, können gepackte Desktop-Apps die Methode [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) aufrufen, um bestimmte Arten von App-Aktivierungsinformationen während des Starts abzurufen. Sie können diese Methode beispielsweise aufrufen, um Informationen zur App-Aktivierung beim Öffnen einer Datei, beim Klicken auf ein interaktives Popup oder beim Verwenden eines Protokolls abzurufen. Ab Windows 10, Version 2004, wird dieses Feature auch in Apps unterstützt, die [Sparse Packages](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) verwenden.

> [!NOTE]
> Zusätzlich zum Abrufen bestimmter Arten von Aktivierungsinformationen mit der Methode [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs), wie in diesem Artikel beschrieben, können Sie auch Aktivierungsinformationen für Hintergrundaufgaben abrufen, indem Sie eine COM-Klasse definieren. Weitere Informationen finden Sie unter [Erstellen und Registrieren einer WinMain-COM-Hintergrundaufgabe](/windows/uwp/launch-resume/create-and-register-a-winmain-background-task).

## <a name="code-example"></a>Codebeispiel

Das folgende Codebeispiel veranschaulicht, wie die Methode [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) von der Funktion **Main**-Funktion in einer Windows Forms-App aufgerufen wird. Wandeln Sie für jeden Aktivierungstyp, den Ihre App unterstützt, den Rückgabewert `args` in den entsprechenden Ereignisargumenttyp um. In diesem Codebeispiel wird davor ausgegangen, dass es sich bei den `Handlexxx`-Methoden um dedizierten Aktivierungshandlercode handelt, den Sie an anderer Stelle definiert haben.

```csharp
static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    var args = AppInstance.GetActivatedEventArgs();
    switch (args.Kind)
    {
        case ActivationKind.Launch:
            HandleLaunch(args as LaunchActivatedEventArgs);
            break;
        case ActivationKind.ToastNotification:
            HandleToastNotification(args as ToastNotificationActivatedEventArgs);
            break;
        case ActivationKind.VoiceCommand:
            HandleVoiceCommand(args as VoiceCommandActivatedEventArgs);
            break;
        case ActivationKind.File:
            HandleFile(args as FileActivatedEventArgs);
            break;
        case ActivationKind.Protocol:
            HandleProtocol(args as ProtocolActivatedEventArgs);
            break;
        case ActivationKind.StartupTask:
            HandleStartupTask(args as StartupTaskActivatedEventArgs);
            break;
        default:
            HandleLaunch(null);
            break;
    }
```

## <a name="supported-activation-types"></a>Unterstützte Aktivierungstypen

Sie können die Methode [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) verwenden, um Aktivierungsinformationen aus dem unterstützten Satz von Ereignisargumantobjekten abzurufen, die in der folgenden Tabelle aufgeführt sind. Einige dieser Aktivierungstypen erfordern die Verwendung einer Paketerweiterung im Paketmanifest.

[ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.sharetargetactivatedeventargs)-Aktivierungsinformationen werden erst ab Windows 10, Version 2004, unterstützt. Alle anderen Aktivierungsinformationstypen werden ab Windows 10, Version 1809, unterstützt.

| Ereignisargumanttyp | Paketerweiterung | Verwandte Dokumentation | 
|-------------------|-----------------|-----------------------|
| [ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.sharetargetactivatedeventargs) | [uap:ShareTarget](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-sharetarget) | [Festlegen deiner Desktopanwendung als Freigabeziel](/windows/apps/desktop/modernize/desktop-to-uwp-extend#making-your-desktop-application-a-share-target) |
| [ProtocolActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.protocolactivatedeventargs) | [uap:Protocol](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol) | [Starten deiner Anwendung über ein Protokoll](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-your-application-by-using-a-protocol) |
| [ToastNotificationActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.toastnotificationactivatedeventarg) | desktop:ToastNotificationActivation | [Popupbenachrichtigungen über Desktop-Apps](/windows/uwp/design/shell/tiles-and-notifications/toast-desktop-apps). |
| [StartupTaskActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.startuptaskactivatedeventargs)  | desktop:StartupTask | [Starten einer ausführbaren Datei, wenn sich Benutzer bei Windows anmelden](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-an-executable-file-when-users-log-into-windows) |
| [FileActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.fileactivatedeventargs) | [uap:FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) | [Zuordnen einer gepackten Anwendung zu einer Gruppe von Dateitypen](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#associate-your-packaged-application-with-a-set-of-file-types) |
| [VoiceCommandActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.voicecommandactivatedeventargs) | Keine | [Behandeln der Aktivierung und Ausführen von Sprachbefehlen](/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana) |
| [LaunchActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) | Keine |  |
