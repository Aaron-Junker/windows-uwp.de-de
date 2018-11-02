---
author: normesta
Description: Test your app for Windows 10 in S mode.
Search.Product: eADQiWindows 10XVcnh
title: Testen Ihrer Windows-App für Windows 10 S
ms.author: normesta
ms.date: 05/11/2017
ms.topic: article
keywords: Windows10 S, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3307506c5cf62d04cd19fbc302ad14bfcedd0045
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "5933198"
---
# <a name="test-your-windows-app-for-windows-10-in-s-mode"></a>Testen Ihrer Windows-App für Windows 10 im S Modus

Sie können Ihre Windows-App testen, um sicherzustellen, dass sie auf Geräten, auf denen Windows 10 im S Modus ausgeführt wird, ordnungsgemäß funktioniert. Sie müssen diesen Test sogar vornehmen, wenn Sie Ihre App im Microsoft Store veröffentlichen möchten, da dies eine Store-Anforderung ist. Um Ihre App zu testen, können Sie eine Richtlinie für Device Guard-Codeintegrität auf einem Gerät anwenden, auf dem Windows10 Pro ausgeführt wird.

> [!NOTE]
> Auf dem Gerät, auf das Sie die Device Guard-Codeintegritätsrichtlinie anwenden, muss Windows10 Creators Edition (10.0; Build 15063) oder höher ausgeführt werden.

Die Device Guard-Codeintegritätsrichtlinie erzwingt die Regeln, denen Apps entsprechen müssen, damit sie auf Windows10 S ausgeführt werden können.

> [!IMPORTANT]
>Es wird empfohlen, diese Richtlinien auf einem virtuellen Computer anzuwenden. Wenn Sie sie jedoch auf Ihrem lokalen Computer anwenden möchten, dann lesen Sie unsere Informationen zu bewährten Methoden im Abschnitt „Installieren der Richtlinie und Neustart des Systems” in diesem Thema, bevor Sie eine Richtlinie anwenden.

<a id="choose-policy" />

## <a name="first-download-the-policies-and-then-choose-one"></a>Herunterladen der Richtlinien und anschließende Auswahl einer Richtlinie

Laden Sie die Richtlinien für die Device Guard-Codeintegrität [hier](https://go.microsoft.com/fwlink/?linkid=849018) herunter.

Wählen Sie anschließend die am besten geeignete aus. Hier finden Sie eine Zusammenfassung der einzelnen Richtlinien.

|Richtlinie |Erzwingung |Signaturzertifikat |Dateiname |
|--|--|--|--|
|Richtlinie für den Überwachungsmodus |Protokolliert Probleme/keine Blockierung |Store |SiPolicy_Audit.p7b |
|Richtlinie für den Produktionsmodus |Ja |Store |SiPolicy_Enforced.p7b |
|Richtlinie für den Produktionsmodus mit selbstsignierten Apps |Ja |AppX-Testzertifikat  |SiPolicy_DevModeEx_Enforced.p7b |

Es wird empfohlen, mit der Richtlinie für den Überwachungsmodus zu beginnen. Sie können die Ereignisprotokolle zur Codeintegrität überprüfen und diese Informationen verwenden, um Anpassungen an Ihrer App vorzunehmen. Wenden Sie dann die Richtlinie für den Produktionsmodus an, wenn Sie bereit sind, den endgültigen Test durchzuführen.

Im Folgenden erhalten Sie weitere Informationen zu den einzelnen Richtlinien.

### <a name="audit-mode-policy"></a>Richtlinie für den Überwachungsmodus
In diesem Modus wird Ihre App auch dann ausgeführt, wenn sie Aufgaben durchführt, die unter Windows10 S nicht unterstützt werden. Windows protokolliert alle ausführbaren Dateien, die blockiert worden wären, in den Ereignisprotokollen zur Codeintegrität.

Sie finden diese Protokolle durch Öffnen der **Ereignisanzeige**, und dann an diesem Speicherort: Anwendungs- und Dienstprotokolle > Microsoft > Windows > CodeIntegrity > Operational.

![code-integrity-event-logs](images/desktop-to-uwp/code-integrity-logs.png)

Dieser Modus ist sicher und verhindert nicht, dass Ihr System starten.

#### <a name="optional-find-specific-failure-points-in-the-call-stack"></a>(Optional) Suchen Sie bestimmte Fehlerquellen in der Aufrufliste
Um bestimmte Punkte in der Aufrufliste zu suchen, in denen Probleme auftreten, fügen Sie diesen Registrierungsschlüssel hinzu, und richten Sie dann [eine Kernelmodus-Debugging-Umgebung ein](https://docs.microsoft.com/windows-hardware/drivers/debugger/getting-started-with-windbg--kernel-mode-#span-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanset-up-a-kernel-mode-debugging).

|Schlüssel|Name|Typ|Wert|
|--|---|--|--|
|HKEY_LOCAL_MACHINE\SYSTEM\CurentControlSet\Control\CI| DebugFlags |REG_DWORD | 1 |


![reg-setting](images/desktop-to-uwp/ci-debug-setting.png)

### <a name="production-mode-policy"></a>Richtlinie für den Produktionsmodus
Diese Richtlinie erzwingt Codeintegritätsregeln, die mit Windows10 S übereinstimmen, sodass Sie eine Ausführung unter Windows10 S simulieren können. Hierbei handelt es sich um die strengste Richtlinie. Sie ist für den endgültigen Produkttest hervorragend geeignet. In diesem Modus gelten für Ihre App dieselben Einschränkungen, die auch auf dem Gerät eines Benutzers gelten würden. Um diesen Modus zu verwenden, muss Ihre App vom Microsoft Store signiert werden.

### <a name="production-mode-policy-with-self-signed-apps"></a>Richtlinie für den Produktionsmodus mit selbstsignierten Apps
Dieser Modus ist vergleichbar mit der Richtlinie für den Produktionsmodus, er erlaubt jedoch außerdem das Ausführen von Elementen, die mit dem in der ZIP-Datei enthaltenen Testzertifikat signiert sind. Installieren der PFX-Datei, die im Ordner **AppxTestRootAgency** dieser Zip-Datei enthalten ist. Signieren Sie dann Ihre App damit. Auf diese Weise können Sie schnell iterieren, ohne dass dafür eine Store-Signatur erforderlich ist.

Da der Herausgebername Ihres Zertifikats dem Herausgebernamen Ihrer App entsprechen muss, müssen Sie den Wert des **Identität**-Elements des **Publisher**-Attributs vorübergehend auf „CN=Appx Test Root Agency Ex“ ändern. Sie können dieses Attribut auf den ursprünglichen Wert ändern, nachdem Sie die Tests abgeschlossen haben.

## <a name="next-install-the-policy-and-restart-your-system"></a>Installieren der Richtlinie und Neustart des Systems

Es wird empfohlen, diese Richtlinien auf einem virtuellen Computer anzuwenden, da diese Richtlinien zu Startfehlern führen können. Dies liegt daran, dass diese Richtlinien die Ausführung von Code blockieren, der nicht vom Microsoft Store signiert ist. Dazu zählen auch Treiber.

Wenn Sie diese Richtlinien auf Ihrem lokalen Computer anwenden möchten, empfiehlt es sich, mit der Richtlinie für den Überwachungsmodus zu beginnen. Mit dieser Richtlinie können Sie die Ereignisprotokolle zur Codeintegrität überprüfen, um sicherzustellen, dass keine wichtigen Elemente in einer erzwungenen Richtlinie blockiert werden würden.

Wenn Sie eine Richtlinie anwenden möchten, suchen Sie die Datei „.P7B” für die gewählte Richtlinie, benennen Sie die Datei in **SIPolicy.P7B** um, und speichern Sie diese Datei an folgendem Speicherort auf Ihrem System: **C:\Windows\System32\CodeIntegrity\\**.

Starten Sie das System anschließend neu.

>[!NOTE]
>Um eine Richtlinie vom System zu entfernen, löschen Sie die .P7B-Datei und starten das System neu.

## <a name="next-steps"></a>Nächste Schritte

**Finden Sie Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Geben Sie Feedback oder Verbesserungsvorschläge**

Weitere Informationen finden Sie unter [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Lesen Sie einen detaillierten Blogartikel, der von unserer App-Team bereitgestellt wurde**

Weitere Informationen finden Sie unter [Portieren und Testen Ihrer klassischen Desktopanwendungen auf Windows10 S mit der Desktop-Brücke](https://blogs.msdn.microsoft.com/appconsult/2017/06/15/porting-and-testing-your-classic-desktop-applications-on-windows-10-s-with-the-desktop-bridge/).

**Erfahren Sie mehr über Tools, die den Test für den S Modus von Windows erleichtern**

Weitere Informationen finden Sie unter [Entpacken, Ändern, neu Packen und Signieren eines APPX](https://blogs.msdn.microsoft.com/appconsult/2017/08/07/unpack-modify-repack-sign-appx/).
