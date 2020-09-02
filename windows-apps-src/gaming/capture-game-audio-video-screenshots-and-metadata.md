---
ms.assetid: ''
description: Erfahren Sie, wie Sie Spiele Videos, Audiodateien und Screenshots erfassen und Metadaten übermitteln, die das System in erfasste und Broadcast Medien einbettet.
title: Aufzeichnen von Screenshots sowie Audio-, Video- und Metadaten eines Spiels
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, Game, Capture, Audiodaten, Videos, Metadaten
ms.localizationpriority: medium
ms.openlocfilehash: ea01139d4945d1e1b7e9a49078cd93725388e946
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364143"
---
# <a name="capture-game-audio-video-screenshots-and-metadata"></a>Aufzeichnen von Screenshots sowie Audio-, Video- und Metadaten eines Spiels
In diesem Artikel wird beschrieben, wie Sie Spiele Videos, Audiodateien und Screenshots erfassen und Metadaten übermitteln, die das System in erfasste und Broadcast Medien einbettet, sodass Ihre APP und andere dynamische Umgebungen erstellen können, die mit den Spielereignissen synchronisiert werden. 

Es gibt zwei verschiedene Möglichkeiten, wie das-Spiel in einer UWP-App aufgezeichnet werden kann. Der Benutzer kann die Erfassung mithilfe der integrierten Benutzeroberfläche des Systems initiieren. Medien, die mit dieser Technik aufgezeichnet werden, werden in das Microsoft Gaming-Ökosystem aufgenommen und können über die erstmaligen Benutzererfahrung, wie z. b. die Xbox-APP, angezeigt und freigegeben werden und sind nicht direkt für Ihre APP oder Benutzer verfügbar. In den ersten Abschnitten dieses Artikels erfahren Sie, wie Sie die vom System implementierte App-Erfassung aktivieren und deaktivieren und wie Sie Benachrichtigungen empfangen, wenn die APP-Erfassung startet oder beendet wird.

Die andere Möglichkeit zum Erfassen von Medien besteht darin, die APIs des **[Windows. Media. apprecording](/uwp/api/windows.media.apprecording)** -Namespace zu verwenden. Wenn die Erfassung auf dem Gerät aktiviert ist, kann Ihre APP mit dem Erfassen des Spiels beginnen, und nach einiger Zeit können Sie die Erfassung abbrechen. an diesem Punkt wird das Medium in eine Datei geschrieben. Wenn der Benutzer die Verlaufs Erfassung aktiviert hat, können Sie auch das bereits aufgetretene Spiel aufzeichnen, indem Sie eine Startzeit in der Vergangenheit und eine zu erfassende Dauer angeben. Beide Verfahren führen zu einer Videodatei, auf die von ihrer App zugegriffen werden kann, und abhängig davon, wo die Dateien vom Benutzer gespeichert werden. In den mittleren Abschnitten dieses Artikels werden Sie durch die Implementierung dieser Szenarien geführt.

Der **[Windows. Media. Capture](/uwp/api/windows.media.capture)** -Namespace stellt APIs zum Erstellen von Metadaten bereit, die das erfasste oder übertragene Spielelement beschreiben. Dies kann Text-oder numerische Werte mit einer Text Bezeichnung enthalten, die die einzelnen Datenelemente identifiziert. Metadaten können ein Ereignis darstellen, das zu einem bestimmten Zeitpunkt auftritt, z. b. wenn der Benutzer eine Runde in einem Renn Spiel beendet oder einen "Zustand" darstellen kann, der über einen bestimmten Zeitraum beibehalten wird, z. b. die aktuelle Spiel Zuordnung, in der der Benutzer abgespielt wird. Die Metadaten werden in einen Cache geschrieben, der vom System für Ihre APP zugewiesen und verwaltet wird. Die Metadaten werden in Broadcast Datenströme und erfasste Videodateien eingebettet, einschließlich integrierter System Erfassungs Verfahren oder benutzerdefinierter App-Erfassungs Verfahren. In den letzten Abschnitten dieses Artikels wird erläutert, wie Sie die Metadaten des-Spiels schreiben.

> [!NOTE] 
> Da die Metadaten des Spiels in Mediendateien eingebettet werden können, die potenziell über das Netzwerk freigegeben werden können, sollten Sie keine personenbezogenen Informationen oder andere potenziell sensible Daten in die Metadaten einschließen.


## <a name="enable-and-disable-system-app-capture"></a>Aktivieren und Deaktivieren der System-App-Erfassung
Die System-App-Erfassung wird vom Benutzer mit der integrierten Benutzeroberfläche des Systems initiiert. Die Dateien werden vom Windows-Gaming-Ökosystem erfasst und sind für Ihre APP oder den Benutzer nicht verfügbar, außer für die erstmaligen Benutzererfahrung wie die Xbox-app. Ihre APP kann die vom System initiierte App-Erfassung deaktivieren und aktivieren, sodass Sie verhindern können, dass der Benutzer bestimmte Inhalte oder ein bestimmtes Spiel erfassen kann. 

Um die System-App-Erfassung zu aktivieren **oder zu deaktivieren** , rufen Sie einfach die statische Methode " **[appcapture. abtallowedasync](/uwp/api/windows.media.capture.appcapture.setallowedasync)** " auf, und übergeben Sie " **false** ", um die Erfassung zu deaktivieren

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetSetAppCaptureAllowed":::


## <a name="receive-notifications-when-system-app-capture-starts-and-stops"></a>Benachrichtigungen empfangen, wenn die System-App-Erfassung gestartet und beendet wird
Um eine Benachrichtigung zu erhalten, wenn die System-App-Erfassung beginnt oder endet, rufen Sie zuerst eine Instanz der **[appcapture](/uwp/api/windows.media.capture.appcapture)** -Klasse ab, indem Sie die Factorymethode **[getforcurrentview](/uwp/api/windows.media.capture.appcapture.GetForCurrentView)** aufrufen. Registrieren Sie als nächstes einen Handler für das **[capturingchanged](/uwp/api/windows.media.capture.appcapture.CapturingChanged)** -Ereignis.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRegisterCapturingChanged":::

Im-Handler für das **capturingchanged** -Ereignis können Sie die Eigenschaften **[iscapturingaudiound](/uwp/api/windows.media.capture.appcapture.IsCapturingAudio)** **[iscapturingvideo](/uwp/api/windows.media.capture.appcapture.IsCapturingVideo)** überprüfen, um festzustellen, ob Audiodaten bzw. Video aufgezeichnet werden. Möglicherweise möchten Sie die Benutzeroberfläche Ihrer APP aktualisieren, um den aktuellen Erfassungs Status anzugeben.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnCapturingChanged":::

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>Hinzufügen der Windows-Desktop Erweiterungen für die UWP zu Ihrer APP
Die APIs zum Aufzeichnen von Audiodaten und Videos sowie zum Erfassen von Screenshots direkt aus der APP, die im **[Windows. Media. apprecording](/uwp/api/windows.media.apprecording)** -Namespace enthalten sind, sind nicht im universellen API-Vertrag enthalten. Um auf die APIs zuzugreifen, müssen Sie mit den folgenden Schritten einen Verweis auf die Windows-Desktop Erweiterungen für die UWP zu Ihrer APP hinzufügen.

1. Erweitern Sie in Visual Studio in **Projektmappen-Explorer**das UWP-Projekt, klicken Sie mit der rechten Maustaste auf **Verweise** , und wählen Sie dann **Verweis hinzufügen...** aus. 
2. Erweitern Sie den Knoten **Universal Windows** , und wählen Sie **Erweiterungen**aus.
3. Aktivieren Sie in der Liste der Erweiterungen das Kontrollkästchen neben den **Windows-Desktop Erweiterungen für den UWP-** Eintrag, der mit dem Zielbuild für Ihr Projekt übereinstimmt. Für die APP-Broadcast Features muss die Version 1709 oder höher sein.
4. Klicken Sie auf **OK**.

## <a name="get-an-instance-of-apprecordingmanager"></a>Rufen Sie eine Instanz von apprecordingmanager ab.
Die **[apprecordingmanager](/uwp/api/windows.media.apprecording.apprecordingmanager)** -Klasse ist die zentrale API, die Sie zum Verwalten der APP-Aufzeichnung verwenden werden. Rufen Sie eine Instanz dieser Klasse ab, indem Sie die Factorymethode **[GetDefault](/uwp/api/windows.media.apprecording.apprecordingmanager.GetDefault)** aufrufen. Bevor Sie eine der APIs im **Windows. Media. apprecording** -Namespace verwenden, sollten Sie überprüfen, ob Sie auf dem aktuellen Gerät vorhanden sind. Die APIs sind nicht auf Geräten verfügbar, auf denen eine ältere Betriebssystemversion als Windows 10, Version 1709, ausgeführt wird. Anstatt eine bestimmte Betriebssystemversion zu überprüfen, verwenden Sie die Methode **[apiinformation. isapikontratpresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** , um die *Windows. Media. appbroadcasting. apprecordingcontract* -Version 1,0 abzufragen. Wenn dieser Vertrag vorhanden ist, sind die Aufzeichnungs-APIs auf dem Gerät verfügbar. Der Beispielcode in diesem Artikel prüft einmal die APIs und überprüft dann, ob **apprecordingmanager** vor nachfolgenden Vorgängen NULL ist.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetGetAppRecordingManager":::

## <a name="determine-if-your-app-can-currently-record"></a>Ermitteln, ob Ihre APP zurzeit Daten aufzeichnen kann
Es gibt mehrere Gründe, warum Ihre APP derzeit nicht in der Lage ist, Audiodaten oder Videos zu erfassen. Dies umfasst u. a., ob das aktuelle Gerät die Hardwareanforderungen für die Aufzeichnung nicht erfüllt oder ob derzeit eine andere APP sendet. Vor dem Initiieren einer Aufzeichnung können Sie überprüfen, ob Ihre APP aktuell aufgezeichnet werden kann. Rufen Sie die **[GetStatus](/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** -Methode des **apprecordingmanager** -Objekts auf, und überprüfen Sie dann die **[canrecord](/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecord)** -Eigenschaft des zurückgegebenen **[apprecordingstatus](/uwp/api/windows.media.apprecording.apprecordingstatus)** -Objekts. Wenn **canrecord** **false**zurückgibt, was bedeutet, dass Ihre APP derzeit nicht aufzeichnen kann, können Sie die **[Details](/uwp/api/windows.media.apprecording.apprecordingstatus.Details)** -Eigenschaft überprüfen, um die Ursache zu ermitteln. Abhängig von der Ursache können Sie den Status des Benutzers anzeigen oder Anweisungen zum Aktivieren der APP-Aufzeichnung anzeigen.



:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCanRecord":::

## <a name="manually-start-and-stop-recording-your-app-to-a-file"></a>Manuelles Starten und Anhalten der Aufzeichnung ihrer app in einer Datei

Nachdem Sie überprüft haben, dass Ihre APP aufzeichnen kann, können Sie eine neue Aufzeichnung starten, indem Sie die **[startrecordingtofileasync](/uwp/api/windows.media.apprecording.apprecordingmanager.startrecordingtofileasync)** -Methode des **apprecordingmanager** -Objekts aufrufen.

Im folgenden Beispiel wird der erste **Then** -Block ausgeführt, wenn die asynchrone Aufgabe fehlschlägt. Der zweite **dann** -Block versucht, auf das Ergebnis der Aufgabe zuzugreifen, und wenn das Ergebnis NULL ist, wurde die Aufgabe abgeschlossen. In beiden Fällen wird die Hilfsmethode **onrecordingcomplete** , wie unten gezeigt, aufgerufen, um das Ergebnis zu verarbeiten. 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartRecordToFile":::

Wenn der Aufzeichnungs Vorgang abgeschlossen ist, überprüfen Sie die Eigenschaft **[erfolgreich](/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** des zurückgegebenen **[apprecordingresult](/uwp/api/windows.media.apprecording.apprecordingresult)** -Objekts, um zu bestimmen, ob der Daten Satz Vorgang erfolgreich war. Wenn dies der Fall ist, können Sie die Eigenschaft " **[isfiletruncated](/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** " überprüfen, um zu ermitteln, ob das System aus Speicher Gründen gezwungen wurde, die erfasste Datei abzuschneiden. Sie können die **[Duration](/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** -Eigenschaft überprüfen, um die tatsächliche Dauer der aufgezeichneten Datei zu ermitteln, die, wenn die Datei abgeschnitten ist, möglicherweise kürzer als die Dauer des Aufzeichnungs Vorgangs ist.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnRecordingComplete":::

Die folgenden Beispiele zeigen einen grundlegenden Code zum Starten und Beenden des Aufzeichnungs Vorgangs, der im vorherigen Beispiel gezeigt wurde.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallStartRecordToFile":::

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetFinishRecordToFile":::

## <a name="record-a-historical-time-span-to-a-file"></a>Aufzeichnen einer historischen Zeitspanne in einer Datei
Wenn der Benutzer die Verlaufs Aufzeichnung für Ihre APP in den Systemeinstellungen aktiviert hat, können Sie eine Zeitspanne des zuvor erstellenden Spiels aufzeichnen. In einem vorherigen Beispiel in diesem Artikel wurde gezeigt, wie Sie sicherstellen können, dass Ihre APP aktuell ein Spiel aufzeichnen kann. Es gibt eine zusätzliche Überprüfung, um zu bestimmen, ob die Verlaufs Erfassung aktiviert ist. Rufen Sie erneut **[GetStatus](/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** auf, und überprüfen Sie die **[canrecordtimespan](/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecordTimeSpan)** -Eigenschaft des zurückgegebenen **apprecordingstatus** -Objekts. In diesem Beispiel wird auch die **[historicalbufferduration](/uwp/api/windows.media.apprecording.apprecordingstatus.HistoricalBufferDuration)** -Eigenschaft von **apprecordingstatus** zurückgegeben, die verwendet wird, um eine gültige Startzeit für den Aufzeichnungs Vorgang zu bestimmen.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCanRecordTimeSpan":::

Zum Erfassen eines historischen Zeitraums müssen Sie eine Startzeit für die Aufzeichnung und eine Dauer angeben. Die Startzeit wird als **[DateTime](/uwp/api/windows.foundation.datetime)** -Struktur bereitgestellt. Die Startzeit muss eine Uhrzeit vor der aktuellen Uhrzeit innerhalb der Länge des Verlaufs Aufzeichnungs Puffers sein. In diesem Beispiel wird die Pufferlänge als Teil der Überprüfung abgerufen, um festzustellen, ob die historische Aufzeichnung aktiviert ist. Dies wird im obigen Codebeispiel veranschaulicht. Die Dauer der Verlaufs Aufzeichnung wird als  **[TimeSpan](/uwp/api/windows.foundation.timespan)** -Struktur bereitgestellt, die auch gleich oder kleiner als die Dauer des Verlaufs Puffers sein sollte. Nachdem Sie die gewünschte Startzeit und die gewünschte Dauer festgelegt haben, können Sie **[recordtimespandefileasync](/uwp/api/windows.media.apprecording.apprecordingmanager.recordtimespantofileasync)** aufrufen, um den Aufzeichnungs Vorgang zu starten.

Wie bei der Aufzeichnung mit manuellem starten und anhalten, wenn eine Verlaufs Aufzeichnung abgeschlossen ist, können Sie die Eigenschaft **[erfolgreich](/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** des zurückgegebenen **[apprecordingresult](/uwp/api/windows.media.apprecording.apprecordingresult)** -Objekts überprüfen, um zu ermitteln, ob der Daten Satz Vorgang erfolgreich war, und Sie können die Eigenschaft **[isfiletruncated](/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** und **[Duration](/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** überprüfen, um die tatsächliche Dauer der aufgezeichneten Datei zu ermitteln. wenn die Datei abgeschnitten ist, kann die Dauer des angeforderten Zeitfensters kürzer sein

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRecordTimeSpanToFile":::

Das folgende Beispiel zeigt einen grundlegenden Code zum Initiieren des Verlaufs Daten Satz Vorgangs, der im vorherigen Beispiel gezeigt wurde.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallRecordTimeSpanToFile":::

## <a name="save-screenshot-images-to-files"></a>Screenshot-Bilder in Dateien speichern
Ihre APP kann eine Screenshot-Erfassung initiieren, bei der der aktuelle Inhalt des App-Fensters in einer Bilddatei oder in mehreren Bilddateien mit unterschiedlichen Bild Codierungen gespeichert wird. Um die Bild Codierungen anzugeben, die Sie verwenden möchten, erstellen Sie eine Liste von Zeichen folgen, in denen jedes einen Bildtyp darstellt. Die Eigenschaften von **[imageencodingsubtypes](/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)** geben die richtige Zeichenfolge für jeden unterstützten Bildtyp an, z. b. **MediaEncodingSubtypes.Png** oder **mediaencodingsubtypes. jpgxr**.

Initiieren Sie die Bildschirmaufnahme, indem Sie die **[savescreenshottofilesasync](/uwp/api/windows.media.apprecording.apprecordingmanager.savescreenshottofilesasync)** -Methode des **apprecordingmanager** -Objekts aufrufen. Der erste Parameter für diese Methode ist ein **storagefolder-Ordner** , in dem die Bilddateien gespeichert werden. Der zweite Parameter ist ein Dateinamen Präfix, an das das System die Erweiterung für jeden gespeicherten Bildtyp anfügt (z. b. ". png").

Der dritte Parameter für **saveskreenshotdefilesasync** ist erforderlich, damit das System in der Lage ist, die ordnungsgemäße colorspace-Konvertierung durchzuführen, wenn das aktuelle Fenster, das aufgezeichnet werden soll, HDR-Inhalt anzeigt. Wenn HDR-Inhalt vorhanden ist, sollte dieser Parameter auf **apprecordingsaveskreenshotoption. hdrcontentvisible**festgelegt werden. Verwenden Sie andernfalls **apprecordingsaveskreenshotoption. None**. Der letzte Parameter für die-Methode ist die Liste der Bildformate, in denen der Bildschirm aufgezeichnet werden soll.

Wenn der asynchrone Aufrufen von **saveskreenshottofilesasync** abgeschlossen ist, gibt er ein **[apprecordingsavedscreenshotinfo](/uwp/api/windows.media.apprecording.apprecordingsavedscreenshotinfo)** -Objekt zurück, das den **storagefile** -und den zugehörigen **mediaencodingsubtypes** -Wert bereitstellt, der den Bildtyp für jedes gespeicherte Bild angibt.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetSaveScreenShotToFiles":::

Das folgende Beispiel zeigt einen grundlegenden Code zum Initiieren des Screenshot-Vorgangs, der im vorherigen Beispiel gezeigt wurde.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCallSaveScreenShotToFiles":::

## <a name="add-game-metadata-for-system-and-app-initiated-capture"></a>Hinzufügen von Spiel Metadaten für die von System und App initiierte Erfassung
In den folgenden Abschnitten dieses Artikels wird beschrieben, wie Metadaten bereitgestellt werden, die das System in den MP4-Datenstrom des aufgezeichneten oder Broadcast-Spiels einbettet. Metadaten können in Medien eingebettet werden, die mit der integrierten Systembenutzer Oberfläche und den von der APP mit **apprecordingmanager**aufgezeichneten Medien aufgezeichnet werden. Diese Metadaten können von Ihrer APP und anderen apps während der Medienwiedergabe extrahiert werden, um kontextabhängige Funktionen bereitzustellen, die mit dem erfassten oder Broadcast-Spiel synchronisiert werden.

### <a name="get-an-instance-of-appcapturemetadatawriter"></a>Eine Instanz von AppCaptureMetadataWriter erhalten
Die primäre Klasse für die Verwaltung von App Capture-Metadaten ist **[AppCaptureMetadataWriter](/uwp/api/windows.media.capture.appcapturemetadatawriter)**. Verwenden Sie vor dem Initialisieren einer Instanz dieser Klasse die **[apiinformation. isapikonpresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** -Methode, um die *Windows. Media. Capture. AppCaptureMetadataContract* -Version 1,0 abzufragen und zu überprüfen, ob die API auf dem aktuellen Gerät verfügbar ist.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetGetMetadataWriter":::

### <a name="write-metadata-to-the-system-cache-for-your-app"></a>Schreiben von Metadaten in den System Cache für Ihre APP
Jedes Metadatenelement verfügt über eine Zeichen folgen Bezeichnung, die das Metadatenelement identifiziert, einen zugeordneten Datenwert, der eine Zeichenfolge, eine ganze Zahl oder einen Double-Wert sein kann, und einen Wert aus der **[AppCaptureMetadataPriority](/uwp/api/windows.media.capture.appcapturemetadatapriority)** -Enumeration, der die relative Priorität des Datenelements angibt. Ein Metadatenelement kann entweder als "Ereignis" angesehen werden, das zu einem bestimmten Zeitpunkt auftritt, oder ein "State", das einen Wert über ein Zeitfenster verwaltet. Metadaten werden in einen Speicher Cache geschrieben, der vom System für Ihre APP zugewiesen und verwaltet wird. Das System erzwingt eine Größenbeschränkung für den metadatenarbeitsspeicher-Cache und löscht, wenn der Grenzwert erreicht wird, Daten basierend auf der Priorität, mit der die einzelnen Metadatenelemente geschrieben wurden. Im nächsten Abschnitt dieses Artikels wird erläutert, wie Sie die Metadaten-Speicher Belegung Ihrer APP verwalten.

Eine typische App kann am Anfang der Erfassungs Sitzung einige Metadaten schreiben, um einen Kontext für die nachfolgenden Daten bereitzustellen. Für dieses Szenario wird empfohlen, dass Sie sofortige Ereignisdaten verwenden. In diesem Beispiel werden **[addstringevent](/uwp/api/windows.media.capture.appcapturemetadatawriter.addstringevent)**, **[adddoubleevent](/uwp/api/windows.media.capture.appcapturemetadatawriter.adddoubleevent)** und **[AddInt32Event](/uwp/api/windows.media.capture.appcapturemetadatawriter.addint32event)** aufgerufen, um sofortige Werte für jeden Datentyp festzulegen.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartSession":::

Ein häufiges Szenario für die Verwendung von "State"-Daten, die im Laufe der Zeit beibehalten werden, besteht darin, die Spiel Zuordnung zu verfolgen, in der sich der Spieler In diesem Beispiel wird **[startstringstate](/uwp/api/windows.media.capture.appcapturemetadatawriter.startstringstate)** aufgerufen, um den Statuswert festzulegen. 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetStartMap":::

Ruft den **[stopstate](/uwp/api/windows.media.capture.appcapturemetadatawriter.stopstate)** auf, um aufzuzeichnen, dass ein bestimmter Zustand beendet wurde.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetEndMap":::

Sie können einen Zustand überschreiben, indem Sie einen neuen Wert mit einer vorhandenen Zustands Bezeichnung festlegen.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetLevelUp":::

Sie können alle gegenwärtig geöffneten Zustände beenden, indem Sie **[stopallstates](/uwp/api/windows.media.capture.appcapturemetadatawriter.StopAllStates)** aufrufen.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRaceComplete":::

### <a name="manage-metadata-cache-storage-limit"></a>Speicherlimit für Metadatencache verwalten
Die Metadaten, die Sie mit **AppCaptureMetadataWriter** schreiben, werden vom System zwischengespeichert, bis Sie in den zugehörigen Mediendaten Strom geschrieben werden. Das System definiert eine Größenbeschränkung für den Metadatencache jeder app. Nachdem die Cache Größenbeschränkung erreicht wurde, beginnt das System mit der Löschung von zwischengespeicherten Metadaten. Das System löscht Metadaten, die mit dem Prioritätswert **[AppCaptureMetadataPriority. Information](/uwp/api/windows.media.capture.appcapturemetadatapriority)** geschrieben wurden, bevor Metadaten mit dem AppCaptureMetadataPriority-Wert gelöscht werden **[. wichtige](/uwp/api/windows.media.capture.appcapturemetadatapriority)** Priorität.

Sie können jederzeit überprüfen, ob die Anzahl der im Metadatencache ihrer App verfügbaren Bytes durch Aufrufen von " **[restingstoragebytesavailable](/uwp/api/windows.media.capture.appcapturemetadatawriter.RemainingStorageBytesAvailable)**" angezeigt wird. Sie können auch einen eigenen, von der APP definierten Schwellenwert festlegen, nach dem Sie die Menge der Metadaten, die Sie in den Cache schreiben, reduzieren können. Das folgende Beispiel zeigt eine einfache Implementierung dieses Musters.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetCheckMetadataStorage":::

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetComboExecuted":::

### <a name="receive-notifications-when-the-system-purges-metadata"></a>Benachrichtigungen empfangen, wenn das System Metadaten löscht
Sie können sich registrieren, um eine Benachrichtigung zu erhalten, wenn das System mit dem Löschen von Metadaten für Ihre APP beginnt, indem Sie einen Handler für das **[MetadataPurged](/uwp/api/windows.media.capture.appcapturemetadatawriter.MetadataPurged)** -Ereignis registrieren.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetRegisterMetadataPurged":::

Im-Handler für das **MetadataPurged** -Ereignis können Sie einen gewissen Raum im Metadatencache löschen, indem Sie den Status der niedrigeren Priorität beenden. Sie können APP-definierte Logik implementieren, um die Menge der Metadaten zu verringern, die Sie in den Cache schreiben, oder Sie können nichts tun, und das System kann den Cache weiterhin basierend auf der Priorität bereinigen, mit der er geschrieben wurde.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppRecordingExample/cpp/AppRecordingExample/App.cpp" id="SnippetOnMetadataPurged":::

## <a name="related-topics"></a>Verwandte Themen

* [Spiele](index.md)
 

 
