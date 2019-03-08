---
title: Problembehandlung bei Xbox Live-Anmeldung
description: Informationen Sie zum Behandeln von Problemen mit Xbox Live-Anmeldung.
ms.assetid: 87b70b4c-c9c1-48ba-bdea-b922b0236da4
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, eine Anmeldung, Problembehandlung
ms.localizationpriority: medium
ms.openlocfilehash: ff8d66105d8a1a44708bf23a681767a3044cb654
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630325"
---
# <a name="troubleshooting-xbox-live-sign-in"></a>Problembehandlung bei Xbox Live-Anmeldung

Es gibt mehrere Probleme, die Anmeldung für Probleme verursachen können.  Wenn Sie die Schritte im [erste Schritte mit Visual Studio für UWP-Spiele](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) können Sie die Möglichkeit, alle unerwarteten Fehler verringern.

## <a name="common-issues"></a>Allgemeine Probleme

### <a name="sandbox-problems"></a>Sandbox-Probleme

Im Allgemeinen sollten Sie sich vertraut machen mit dem Konzept von Sandkästen und wie sie Xbox Live betreffen.  Sie können weitere Informationen finden Sie unter der [Xbox Live-Sandboxes](../../xbox-live-sandboxes.md) Guide.

Kurz gesagt, erzwingen Sandboxes Inhaltssteuerelement an Isolation und den Zugriff vor Verkaufsversion an.  Benutzer ohne Zugriff auf Ihre Entwicklung Sandbox können keine Lese- oder Schreibvorgänge, die den Titel des betreffen.  Sie können auch Variationen der Dienstkonfiguration auf unterschiedliche Sandboxes zu Testzwecken veröffentlichen.

Einige Punkte aufgeführt, die im Hinblick auf die Sandboxen in Betracht ziehen, werden im folgenden erörtert.

#### <a name="developer-account-doesnt-have-access-to-the-right-sandbox-for-run-time-access"></a>Developer-Konto keinen Zugriff auf die richtige Sandbox für die Run-Time-Zugriff

* Ein Testkonto (auch bekannt als Development-Konto) oder einem autorisierten-Entwicklerkonto muss für die Anmeldung in einem Titel, die in der Entwicklung verwendet werden.  Stellen Sie sicher, dass Sie versuchen, eine Anmeldung oder das andere Test-Konten werden erstellt, auf XDP am [ https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts ](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts). Sie können ein Xbox live zugeordneten Entwicklerkonto im Partner Center auf Autorisieren. [https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator](https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator)
* Stellen Sie sicher, dass das Konto Zugriff auf die Sandbox, die der Titel veröffentlicht wird.  Die Testkonten in XDP erstellten erben die Berechtigungen des Kontos XDP, das sie erstellt wurden

#### <a name="your-device-is-not-on-the-correct-sandbox"></a>Ihr Gerät ist nicht auf die richtige sandbox

Das Gerät, das auf dem Sie entwickeln, muss einer Entwicklung Sandbox festgelegt werden.  Auf der Xbox One können Sie festlegen, die mit der Sandbox *Xbox One Manager*.  Für Windows 10-Desktop können Sie das Skript SwitchSandbox.cmd, das in das Verzeichnis "Tools" der Xbox Live SDK-Installation befindet.

#### <a name="your-titles-service-configuration-is-not-published-to-the-correct-development-sandbox"></a>Konfiguration des Titels ist nicht in der korrekten Entwicklung Sandbox veröffentlicht.

Stellen Sie sicher, dass die Dienstkonfiguration des Titels in einem Sandkasten Entwicklung veröffentlicht wird.  Sie können nicht für Xbox Live in einer bestimmten Entwicklung Sandbox für einen Titel, es sei denn, Anmeldung, die Titel in der gleichen Sandbox veröffentlicht wird.  Informieren Sie sich die [XDP Dokumentation](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig) Informationen zur Vorgehensweise. Sie erhalten die [Partner Center-Dokumentation](../../get-started-with-creators/xbox-live-service-configuration-creators.md#publish-your-xbox-live-service-configuration) erfahren, wie die Partner Center-Konfiguration veröffentlichen.

### <a name="ids-configured-incorrectly"></a>IDs, die falsch konfiguriert

Es gibt mehrere Teile ID erforderlich, um Ihr Spiel zu konfigurieren.  Sie können weitere Informationen finden Sie unter [erste Schritte mit Visual Studio für UWP-Spiele](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) und [erste Schritte mit Cross-Play Games](../../get-started-with-partner/get-started-with-cross-play-games.md) je nach Typ des Titels für Sie erstellen.

Einige Punkte aufgeführt, die in Betracht ziehen, sind:

* Stellen Sie sicher, dass Ihre App-ID in XDP oder Partner Center richtig eingegeben wurde
* Stellen Sie sicher, dass Ihre PFN XDP oder Partner Center ordnungsgemäß eingegeben wird
* Überprüfen Sie haben eine xboxservices.config im gleichen Verzeichnis wie Ihr Visual Studio-Projekt erstellt, wie oben in der [Hinzufügen von Xbox Live zu einem neuen oder vorhandenen UWP-Projekt](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) Guide.
* Stellen Sie sicher, dass die "Package Identity" in Ihrem Appxmanifest korrekt ist.  Dies wird als "Paket/Identity/Name" im Abschnitt App-Identität im Partner Center angezeigt.

### <a name="title-id-or-scid-not-configured-correctly"></a>Titel-ID oder SCID nicht ordnungsgemäß konfiguriert

* Für UWP-Titel muss Ihre Titel-ID und SCID auf den richtigen Wert in der Datei xboxservices.config festgelegt werden.  Stellen Sie außerdem sicher, dass diese Datei als UTF8 ordnungsgemäß formatiert ist.  Sie können weitere Informationen finden Sie unter [erste Schritte mit Visual Studio für UWP-Spiele](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md). Die xboxservices.config-Datei ist die Groß-/Kleinschreibung beachtet.
* Für den Titel der xdk-Version werden diese Werte in "Package.appxmanifest" festgelegt.
* Sehen Sie Beispiele für die UWP und xdk-Version Titel Konfiguration im Verzeichnis "Samples" des Xbox Live SDK ein.

## <a name="test-using-the-xbox-app"></a>Testen Sie mit der Xbox-App

Wenn Sie eine UWP-Anwendung entwickeln, können Sie einige Probleme mit der Xbox-App Debuggen:

1. Festlegen Sie Ihres Geräts Sandbox einer Entwicklung Sandbox, die mit dem Skript SwitchSandbox.cmd
2. Öffnen Sie die Xbox-App, und versuchen Sie, melden Sie sich mit einem Testkonto mit Zugriff auf der gleichen Sandbox.

Wenn Sie können erfolgreich-Anmeldung, klicken Sie dann diese überprüft, ob Ihre Entwicklung Sandbox ordnungsgemäß auf Ihrem Gerät festgelegt wurde, und Ihrem Testkonto an, kann darauf zugreifen.

Wenn Sie weiterhin Anmeldefehler erhalten, ist es wahrscheinlich, dass die Dienstkonfiguration nicht in Ihrer Sandbox veröffentlicht wird, oder Ihre xboxservices.config kein Setup ordnungsgemäß ist. Die xboxservices.config-Datei ist die Groß-/Kleinschreibung beachtet.

## <a name="debug-based-on-error-code"></a>Debuggen, die basierend auf den Fehlercode:

Es folgen einige der Fehlercodes, die angezeigt werden können, bei der Anmeldung und Schritte, die Sie ergreifen können, um diese zu debuggen.  Sehen Sie den Fehlercode wie gezeigt in den folgenden Screenshot.

![0x8015DC12 Anmeldefehler – Screenshot](../../images/troubleshooting/sign_in_error.png)

### <a name="0x80860003-the-application-is-either-disabled-or-incorrectly-configured"></a>0x80860003 die Anwendung ist deaktiviert oder nicht ordnungsgemäß konfiguriert

1. Versuchen Sie, Ihre PFX-Datei zu löschen.

![PFX-Datei im Projektmappen-explorer](../../images/troubleshooting/pfx_file.png)

Visual Studio erkennt automatisch, wenn Sie Visual Studio mit der für die Bereitstellung der app im Partner Center verwendet Microsoft Account anmelden nicht, generieren eine Signatur PFX-Datei, die basierend auf Ihrem Microsoft Account oder Ihrem Domänenkonto anzumelden. Wenn Sie die Appx-Paket zu erstellen, wird Visual Studio verwenden, die automatisch generierten PFX-Dateien zum Signieren des Pakets, und ändern den "Publisher" Teil der Paketidentität in der Datei "Package.appxmanifest". Daher müssen die erzeugten Bits (insbesondere der "appxmanifest.xml") eine anderes Paket-Identität als Sie verwenden möchten. 

2. Überprüfen Sie, dass Ihr "Package.appxmanifest" im Partner Center auf die gleiche Identität als Titel des festgelegt ist. Sie können klicken Sie mit der rechten Maustaste auf das Projekt und wählen Sie die Store -> App mit Store verknüpfen... Siehe den folgenden Screenshot. Oder Ihr "Package.appxmanifest" manuell bearbeiten. Finden Sie unter [erste Schritte mit Visual Studio für UWP-Spiele](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) für Weitere Informationen.

![Mit Store verknüpfen](../../images/troubleshooting/appxmanifest_binding.png)

### <a name="0x8015dc12-content-isolation-error"></a>Inhaltsisolation 0x8015DC12-Fehler

Zusammengefasst bedeutet dies, dass entweder das Gerät oder der Benutzer den Zugriff auf den angegebenen Titel nicht.

1. Dies könnte bedeuten, Sie nicht verwenden, ein Testkonto um zu Anmeldeversuch oder Ihrem Testkonto an, keinen Zugriff auf der gleichen Sandbox, die, der Sie als angemeldet sind. Bitte überprüfen Sie die Anweisungen zum Erstellen von Testkonten in der [XDP Dokumentation](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/creating_development_accounts_03_31_16.aspx) und [Partner Center-Dokumentation.](../../xbox-live-test-accounts.md) Bei Bedarf Erstellen eines neuen Testkontos mit Zugriff auf die Sandbox geeignet.

Möglicherweise müssen Sie Ihr alte Konto aus Windows 10 zu entfernen, Sie können dies durch die Einstellungen über das Startmenü, und klicken Sie dann auf Konten

2. Überprüfen Sie, dass Ihre Titel mit der Sandbox veröffentlicht wird, die Sie verwenden möchten. Informieren Sie sich die [XDP Dokumentation](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig) oder [Partner Center-Dokumentation](../../xbox-live-service-configuration.md#sandbox-ids) Informationen zur Vorgehensweise.

### <a name="0x87dd0005-unexpected-or-unknown-title"></a>0x87DD0005 Unerwarteter oder unbekannte Titel

Vergewissern Sie sich die ID Anwendungssetup und -Bindung in XDP-Entwicklercenter. Sehen Sie die Anweisungen im [Xbox Live-Unterstützung Hinzufügen einer neuen oder vorhandenen Visual Studio-UWP](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-1--create-a-uwp-device-app#span-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanassociate-your-app-with-the-microsoft-store)

### <a name="0x87dd000e-title-not-authorized"></a>0x87DD000E Titel durch nicht erfolgte Autorisierung

Vergewissern Sie sich, dass Ihr Gerät mit der richtigen Entwicklung Sandbox festgelegt ist und der Benutzer Zugriff auf die Sandbox hat. Finden Sie unter den [testen Sie die Xbox-App mit](#test-using-the-xbox-app) Abschnitt, um weitere Informationen an, damit Sie diese mit der Xbox-App überprüfen können.

Wenn, die das Problem nicht behoben wird, klicken Sie dann auch überprüfen Sie das Setup Dev Center binden und App-ID wie oben beschrieben.

Wenn Sie nicht den hier beschriebenen Fehler angezeigt werden, finden Sie die Fehlerliste in der Dokumentation Xbox::services::xbox_live_error_code, um weitere Informationen zu den Fehlercodes zu erhalten. Sie finden auch errors.h in die XSAPI enthält.

Nach diesen Schritten, wenn immer noch durch den Titel Anmeldung nicht möglich, veröffentlichen Sie einen Support-Thread auf die [Foren](https://forums.xboxlive.com) oder wenden Sie sich an Ihre DAM.
