---
ms.assetid: ''
description: Enthält Informationen zum Aufzeichnen von Audio-, Video- und Metadaten aus Spielen in einer UWP-App.
title: Aufzeichnen von Screenshots, Audio-, Video- und Metadaten eines Spiels
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, spiel, aufzeichnen, audio, video, metadaten
ms.localizationpriority: medium
ms.openlocfilehash: c4d4d764395d7f383e9cefcb9d8b1121db098780
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601935"
---
# <a name="capture-game-audio-video-screenshots-and-metadata"></a>Aufzeichnen von Screenshots, Audio-, Video- und Metadaten eines Spiels
In diesem Artikel wird beschrieben, wie Audio- und Videosequenzen sowie Screenshots aufgezeichnet und Metadaten von Spielen übermittelt werden, die das System in aufgezeichnete und übertragene Videostreams einbettet. So kann Ihre App dynamische Erfahrungen erstellen, die mit Spielereignissen synchronisiert sind. 

Es gibt zwei Möglichkeiten, den Spielverlauf in einer UWP-App zu erfassen. Der Benutzer kann die Aufzeichnung mithilfe der integrierten Systembenutzeroberfläche initiieren. Medien, die auf diese Weise erfasst werden, werden in das Microsoft-Ökosystem für Spiele aufgenommen, können über Erstanbietererfahrungen wie die XBox App angezeigt und weitergegeben werden und stehen Ihrer App oder Nutzern nicht direkt zur Verfügung. In den ersten Abschnitten dieses Artikels erfahren Sie, wie Sie die vom System implementierte Erfassung mit der App aktivieren und deaktivieren, und wie Sie Benachrichtigungen erhalten, wenn die App eine Aufzeichnung startet oder beendet.

Die andere Möglichkeit zum Aufzeichnen von Medien ist die Verwendung der APIs aus dem **[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)** Namespace. Wenn das Aufzeichnen auf dem Gerät aktiviert ist, kann die App mit der Erfassung des Spielverlaufs beginnen. Sobald Sie die Aufzeichnung nach einiger Zeit stoppen, wird das Medium in eine Datei geschrieben. Wenn der Benutzer die rückwirkende Aufzeichnung aktiviert hat, können Sie auch bereits stattgefundene Spielszenen aufzeichnen, indem Sie eine Startzeit in der Vergangenheit und eine Aufzeichnungsdauer angeben. Beide Techniken generieren eine Videodatei, auf die Ihre App zugreifen kann, und je nachdem, wo Sie die Dateien speichern, auch der Benutzer. In den mittleren Abschnitten dieses Artikels wird die Implementierung dieser Szenarien erläutert.

Der Namespace **[Windows.Media.Capture](https://docs.microsoft.com/uwp/api/windows.media.capture)** enthält APIs für das Erstellen von Metadaten, die das aufgezeichnete oder übertragene Spiel beschreiben. Dabei kann es sich um Text oder numerische Werte handeln, wobei ein Textlabel jedes Datenelement kennzeichnet. Metadaten können ein „Ereignis” beschreiben, das zu einem bestimmten Zeitpunkt auftritt, beispielsweise dann, wenn der Benutzer eine Runde in einem Rennspiel beendet, oder einen „Zustand”, der sich über einen bestimmten Zeitraum erstreckt, wie etwa die Landkarte, in der momentan gespielt wird. Die Metadaten werden in einen Zwischenspeicher geschrieben, der vom System für Ihre App zugewiesen und verwaltet wird. Die Metadaten werden in Übertragungsstreams und erfasste Videodateien eingebettet, einschließlich der systemeigenen oder benutzerdefinierten Erfassungstechniken für die App. In den letzten Abschnitten dieses Artikels wird das Schreiben von Spielmetadaten behandelt.

> [!NOTE] 
> Da die Mediendateien mit eingebetteten Metadaten möglicherweise ohne Kontrolle des Benutzers im Netzwerk freigegeben werden, sollten Sie keine personenbezogenen oder andere potenziell vertrauliche Daten in die Metadaten einschließen.


## <a name="enable-and-disable-system-app-capture"></a>Aktivieren und deaktivieren der systemeigenen App-Erfassung
Systemeigene App-Aufzeichnungen werden durch den Benutzer mithilfe der integrierten Systembenutzeroberfläche initiiert. Die Dateien werden in das Windows-Spieleökosystem aufgenommen und stehen Ihrer App oder dem Benutzer nicht zur Verfügung, wohl aber internen Erfahrungen wie der XBox App. Ihre App kann die systeminitiierte App-Erfassung deaktivieren und aktivieren, sodass Sie verhindern können, dass der Nutzer bestimmte Inhalte oder den Spielverlauf aufzeichnet. 

Zum Aktivieren oder Deaktivieren der systemeigenen App-Aufzeichnung rufen Sie einfach die statische Methode **[AppCapture.SetAllowedAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.setallowedasync)** auf und übergeben **false**, um die Aufnahme zu deaktivieren, oder **true**, um sie zu aktivieren.

[!code-cpp[SetAppCaptureAllowed](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetSetAppCaptureAllowed)]


## <a name="receive-notifications-when-system-app-capture-starts-and-stops"></a>Empfangen von Benachrichtigungen, wenn die systemeigene App-Aufzeichnung beginnt oder endet
Um eine Benachrichtigung erhalten, wenn die systemeigene App-Aufzeichnung beginnt oder endet, rufen Sie zunächst eine Instanz der Klasse **[AppCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture)** ab, indem Sie die Factorymethode **[GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.GetForCurrentView)** aufrufen. Dann registrieren Sie einen Handler für das Ereignis **[CapturingChanged](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.CapturingChanged)**.

[!code-cpp[RegisterCapturingChanged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRegisterCapturingChanged)]

Im Handler für das Ereignis **CapturingChanged** können Sie die Eigenschaften **[IsCapturingAudio](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.IsCapturingAudio)** und **[IsCapturingVideo](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapture.IsCapturingVideo)** überprüfen, um festzustellen, ob Audio- oder Videodaten erfasst werden. Sie können den aktuellen Status der Aufzeichnung in der Benutzeroberfläche Ihrer App anzeigen.

[!code-cpp[OnCapturingChanged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnCapturingChanged)]

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>Hinzufügen der „Windows Desktop Extensions for the UWP” zu Ihrer App
Die APIs im Namespace **[Windows.Media.AppRecording](https://docs.microsoft.com/uwp/api/windows.media.apprecording)** für das Aufzeichnen von Audio- und Videodaten sowie von Screenshots direkt aus Ihrer App sind nicht im Universal API-Vertrag enthalten. Für den Zugriff auf die APIs müssen Sie Ihrer App mit den folgenden Schritten einen Verweis auf die „Windows Desktop Extensions for the UWP” hinzufügen.

1. Erweitern Sie im **Projektmappen-Explorer** Ihr UWP-Projekt, klicken Sie mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen** aus. 
2. Erweitern Sie den Knoten **Universal Windows**, und wählen Sie **Erweiterungen** aus.
3. In der Liste der Erweiterungen aktivieren Sie das Kontrollkästchen neben dem Eintrag **Windows Desktop Extensions for the UWP**, der dem Zielbuild für Ihr Projekt entspricht. Die Übertragungsfeatures der App sind ab der Version 1709 verfügbar.
4. Klicken Sie auf **OK**.

## <a name="get-an-instance-of-apprecordingmanager"></a>Abrufen einer AppRecordingManager-Instanz
Die Klasse **[AppRecordingManager](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager)** ist die zentrale API zum Verwalten von App-Aufzeichnungen. Sie können eine Instanz dieser Klasse durch einen Aufruf der Factorymethode **[GetDefault](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetDefault)** abrufen. Vor der Verwendung von APIs aus dem Namespace **Windows.Media.AppRecording** sollten Sie deren Vorhandensein auf dem aktuellen Gerät überprüfen. Die APIs sind nur auf Geräten verfügbar, auf denen Version 1709 (oder höher) von Windows 10 ausgeführt wird. Statt die Version des Betriebssystems zu überprüfen, können Sie die Methode **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** verwenden, um festzustellen, ob Version 1.0 von *Windows.Media.AppBroadcasting.AppRecordingContract* vorhanden ist. Wenn dieser Vertrag vorhanden ist, sind die Aufnahme-APIs auf dem Gerät verfügbar. Der Beispielcode in diesem Artikel sucht zunächst nach den APIs und prüft dann, ob **AppRecordingManager** Null ist, bevor weitere Operationen ausgeführt werden.

[!code-cpp[GetAppRecordingManager](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetGetAppRecordingManager)]

## <a name="determine-if-your-app-can-currently-record"></a>Ermitteln, ob die App momentan aufzeichnen kann
Es gibt verschiedene Gründe dafür, dass Ihre App momentan möglicherweise keine Audio- oder Videoaufzeichnungen erstellen kann. Beispielsweise könnte das aktuelle Gerät nicht den Hardwareanforderungen für Aufnahmen entsprechen oder eine andere App gerade Daten übertragen. Vor dem Initiieren einer Aufzeichnung können Sie überprüfen, ob Ihre App momentan aufzeichnen kann. Rufen Sie die Methode **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** des Objekts **AppRecordingManager** auf, und überprüfen Sie dann die Eigenschaft **[CanRecord](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecord)** des zurückgegebenen Objekts **[AppRecordingStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus)**. Wenn **CanRecord** gibt **"false"**, was bedeutet, dass es sich bei Ihrer app derzeit nicht aufzeichnen kann, sehen Sie sich die **[Details](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.Details)** Eigenschaft, um zu bestimmen Der Grund. Je nach Ursache sollten Sie dem Benutzer den Status oder Anweisungen zum Aktivieren der App-Aufzeichnung anzeigen.



[!code-cpp[CanRecord](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCanRecord)]

## <a name="manually-start-and-stop-recording-your-app-to-a-file"></a>Manuelles Starten und Beenden einer App-Aufzeichnung, die in einer Datei gespeichert wird

Nachdem Sie sicher sind, dass Ihre App aufzeichnen kann, können Sie eine neue Aufzeichnung starten, indem Sie die Methode **[StartRecordingToFileAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.startrecordingtofileasync)** des Objekts **AppRecordingManager** aufrufen.

Im folgenden Beispiel wird der erste **then**-Block ausgeführt, wenn im asynchronen Task ein Fehler auftritt. Der zweite **then**-Block versucht, auf das Taskergebnis zuzugreifen und, wenn das Ergebnis Null ist, ist der Task beendet. In beiden Fällen wird die Hilfsmethode **OnRecordingComplete** (unten dargestellt) aufgerufen, um das Ergebnis zu behandeln. 

[!code-cpp[StartRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartRecordToFile)]

Überprüfen Sie nach Abschluss des Aufzeichnungsvorgangs die Eigenschaft **[Succeeded](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** des zurückgegebenen Objekts **[AppRecordingResult](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult)**, um zu ermitteln, ob der Aufzeichnungsvorgang erfolgreich verlaufen ist. Wenn das der Fall ist, können Sie anhand der Eigenschaft **[IsFileTruncated](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** überprüfen, ob das System aus Speicherplatzmangel gezwungen war, die erfasste Datei zu kürzen. Überprüfen Sie anhand der Eigenschaft **[Duration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** die tatsächliche Dauer der gespeicherten Datei. Dieser Wert wird kleiner sein als die Dauer der Aufzeichnung, wenn die Datei nicht vollständig gespeichert wurde.

[!code-cpp[OnRecordingComplete](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnRecordingComplete)]

Die folgenden Beispiele zeigen grundlegenden Code zum Starten und Stoppen der im vorherigen Beispiel gezeigten Aufzeichnungsoperation.

[!code-cpp[CallStartRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallStartRecordToFile)]

[!code-cpp[FinishRecordToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetFinishRecordToFile)]

## <a name="record-a-historical-time-span-to-a-file"></a>Speichern einer vergangenen Zeitspanne in einer Datei
Wenn der Benutzer die rückwirkende Aufzeichnung für Ihre App in den Systemeinstellungen aktiviert hat, können Sie eine Zeitspanne eines Spiels aufzeichnen, die zuvor abgespielt wurde. In einem früheren Beispiel dieses Artikels wurde gezeigt, wie Sie feststellen können, ob Ihre App momentan aufnehmen kann. Mit einer weiteren Überprüfung ermitteln Sie, ob die rückwirkende Aufzeichnung des Spielverlaufs aktiviert ist. Rufen Sie wieder **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.GetStatus)** auf, und überprüfen Sie die Eigenschaft **[CanRecordTimeSpan](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.CanRecordTimeSpan)** des zurückgegebenen Objekts **AppRecordingStatus**. In diesem Beispiel wird ebenfalls die Eigenschaft **[HistoricalBufferDuration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingstatus.HistoricalBufferDuration)** von **AppRecordingStatus** zurückgegeben und dazu verwendet, eine gültige Startzeit für die Aufzeichnung zu bestimmen.

[!code-cpp[CanRecordTimeSpan](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCanRecordTimeSpan)]

Um eine vergangene Zeitspanne aufzuzeichnen, müssen Sie eine Startzeit für die Aufzeichnung und eine Dauer angeben. Die Startzeit wird als **[DateTime](https://docs.microsoft.com/uwp/api/windows.foundation.datetime)**-Struktur bereitgestellt. Die Startzeit muss vor der aktuellen Zeit und innerhalb der Länge des Puffers für zurückliegende Aufzeichnungen liegen. In diesem Beispiel wird die Pufferlänge als Teil der Prüfung abgerufen, um festzustellen, ob die rückwirkende Aufzeichnung aktiviert ist, was im vorherigen Codebeispiel gezeigt wurde. Die Dauer der rückwirkenden Aufzeichnung wird als **[TimeSpan](https://docs.microsoft.com/uwp/api/windows.foundation.timespan)**-Struktur bereitgestellt und sollte gleich oder kleiner sein als die Dauer des Puffers für zurückliegende Aufzeichnungen. Nachdem Sie die gewünschte Startzeit und Dauer festgelegt haben, rufen Sie **[RecordTimeSpanToFileAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.recordtimespantofileasync)** auf, um die Aufzeichnung zu starten.

Wie beim Aufzeichnen mit manuellem Start und Stopp können Sie nach Abschluss der rückwirkenden Aufzeichnung die Eigenschaft **[Succeeded](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Succeeded)** des zurückgegebenen Objekts **[AppRecordingResult](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult)** überprüfen, um festzustellen, ob der Aufzeichnungsvorgang erfolgreich war, und anhand der Eigenschaften **[IsFileTruncated](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.IsFileTruncated)** und **[Duration](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingresult.Duration)** können Sie die tatsächliche Dauer der aufgezeichneten Datei ermitteln. Dieser Wert wird kleiner sein als das angeforderte Zeitfenster, wenn die Datei abgeschnitten wurde.

[!code-cpp[RecordTimeSpanToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRecordTimeSpanToFile)]

Das folgende Beispiel zeigt grundlegenden Code zum Initiieren der rückwirkenden Aufzeichnung aus dem vorherigen Beispiel.

[!code-cpp[CallRecordTimeSpanToFile](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallRecordTimeSpanToFile)]

## <a name="save-screenshot-images-to-files"></a>Speichern von Screenshots in Dateien
Ihre App kann eine Erfassung des Bildschirminhalts initiieren, die den aktuellen Inhalt des App-Fensters in einer Bilddatei oder in mehreren, unterschiedlich codierten Bilddateien speichert. Um die gewünschten Bildcodierungen anzugeben, erstellen Sie eine Liste von Zeichenfolgen, die jeweils einen Bildtyp darstellen. Die Eigenschaften von **[ImageEncodingSubtypes](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)** stellen die korrekte Zeichenfolge für jeden unterstützten Bildtyp bereit, beispielsweise **MediaEncodingSubtypes.Png** oder **MediaEncodingSubtypes.JpegXr**.

Initiieren Sie die Aufnahme eines Screenshots durch Aufrufen der Methode **[SaveScreenshotToFilesAsync](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingmanager.savescreenshottofilesasync)** des Objekts **AppRecordingManager**. Der erste Parameter für diese Methode ist ein **StorageFolder**, in dem die Bilddateien gespeichert werden. Der zweite Parameter ist ein Dateinamenpräfix, an den das System die Erweiterung für den jeweiligen Bildtyp anfügt, beispielsweise PNG.

Der dritte Parameter für **SaveScreenshotToFilesAsync** ist erforderlich, damit das System in der Lage ist, die richtige Farbraumkonvertierung vorzunehmen, wenn das aufzuzeichnende Fenster HDR-Inhalt anzeigt. Wenn HDR-Inhalt vorhanden ist, sollte dieser Parameter auf **AppRecordingSaveScreenshotOption.HdrContentVisible** festgelegt werden. Verwenden Sie andernfalls **AppRecordingSaveScreenshotOption.None**. Der letzte Parameter der Methode ist die Liste der Bildformate, für die Bildschirm erfasst werden soll.

Wenn der asynchrone Aufruf von **SaveScreenshotToFilesAsync** abgeschlossen ist, wird ein **[AppRecordingSavedScreenshotInfo](https://docs.microsoft.com/uwp/api/windows.media.apprecording.apprecordingsavedscreenshotinfo)**-Objekt zurückgegeben, dass neben dem Wert **StorageFile** auch den zugehörigen Wert **MediaEncodingSubtypes** bereitstellt, der den Bildtyp für jedes gespeicherte Bild angibt.

[!code-cpp[SaveScreenShotToFiles](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetSaveScreenShotToFiles)]

Das folgende Beispiel zeigt grundlegenden Code zum Initiieren der Screenshotoperation aus dem vorherigen Beispiel.

[!code-cpp[CallSaveScreenShotToFiles](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCallSaveScreenShotToFiles)]

## <a name="add-game-metadata-for-system-and-app-initiated-capture"></a>Hinzufügen von Metadaten für systemeigene und App-initiierte Aufzeichnungen
In den folgenden Abschnitten dieses Artikels wird beschrieben, wie Metadaten bereitgestellt werden, die das System in den MP4-Datenstrom des aufgenommenen oder zu übertragenen Spielverlaufs einbettet. Metadaten können in Medien eingebettet werden, die mithilfe der integrierten Systembenutzeroberfläche aufgezeichnet werden, und in Medien, die von der App mithilfe von **AppRecordingManager** aufgezeichnet werden. Diese Metadaten können von Ihrer App und anderen Apps während der Medienwiedergabe extrahiert werden, um kontextabhängige Erlebnisse bereitzustellen, die mit dem erfassten oder übertragenen Spielverlauf synchronisiert sind.

### <a name="get-an-instance-of-appcapturemetadatawriter"></a>Abrufen einer Instanz von AppCaptureMetadataWriter
Die primäre Klasse zur Verwaltung von Metadaten einer App-Aufnahme ist **[AppCaptureMetadataWriter](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter)**. Verwenden Sie vor dem Initialisieren einer Instanz dieser Klasse die Methode **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)**, um zu prüfen, ob Version 1.0 des Vertrags *Windows.Media.Capture.AppCaptureMetadataContract* vorhanden ist. Wenn das der Fall ist, steht die API auf dem aktuellen Gerät zur Verfügung.

[!code-cpp[GetMetadataWriter](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetGetMetadataWriter)]

### <a name="write-metadata-to-the-system-cache-for-your-app"></a>Schreiben von Metadaten in den Zwischenspeicher für Ihre App
Ein Metadatenelement enthält ein Bezeichnung in Form einer Zeichenfolge, die das Metadatenelement identifiziert, einen zugeordneten Wert, der eine Zeichenfolge, eine ganze Zahl oder ein Double-Wert sein kann, und einen Wert aus der Enumeration **[AppCaptureMetadataPriority](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)**, der die relative Priorität des Datenelements angibt Ein Metadatenelement stellt entweder ein „Ereignis” dar, das zu einem bestimmten Zeitpunkt eintritt, oder einen „Status”, der einen Wert während eines Zeitfensters beibehält. Metadaten werden in einen Zwischenspeicher geschrieben, der vom System für Ihre App zugewiesen und verwaltet wird. Das System erzwingt eine Größenbeschränkung für den Metadaten-Zwischenspeicher und löscht, sobald das Limit erreicht ist, die Daten basierend auf der Priorität, mit der jedes Metadatenelement geschrieben wurde. Im nächsten Abschnitt dieses Artikels wird die Verwaltung der Speicherzuordnung für die Metadaten einer App beschrieben.

Eine typische App könnte einige Metadaten zu Beginn der Erfassungssitzung schreiben, um einen Kontext für die nachfolgenden Daten bereitzustellen. Für dieses Szenario wird empfohlen, dass Sie unmittelbare „Ereignis”-Daten verwenden. Dieses Beispiel ruft **[AddStringEvent](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.addstringevent)**, **[AddDoubleEvent](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.adddoubleevent)** und **[AddInt32Event](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.addint32event)** auf, um unmittelbare Werte für jeden Datentyp festzulegen.

[!code-cpp[StartSession](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartSession)]

Ein häufiges Szenario für die Verwendung von „Status”-Daten, die über einen bestimmten Zeitraum erhalten bleiben, ist die Nachverfolgung einer Landkarte, auf der sich der Spieler gerade befindet. Dieses Beispiel ruft **[StartStringState](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.startstringstate)** auf, um den Statuswert festlegen. 

[!code-cpp[StartMap](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetStartMap)]

Rufen Sie **[StopState](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.stopstate)** auf, um aufzuzeichnen, dass ein bestimmter Status beendet wurde.

[!code-cpp[EndMap](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetEndMap)]

Sie können einen Zustand überschreiben, indem Sie einen neuen Wert für eine vorhandene Zustandsbezeichnung festlegen.

[!code-cpp[LevelUp](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetLevelUp)]

Durch Aufrufen von **[StopAllStates](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.StopAllStates)** beenden Sie alle Zustände, die momentan geöffnet sind.

[!code-cpp[RaceComplete](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRaceComplete)]

### <a name="manage-metadata-cache-storage-limit"></a>Verwalten des Limits für den Metadaten-Zwischenspeicher
Die Metadaten, die Sie mit **AppCaptureMetadataWriter** schreiben, werden vom System zwischengespeichert, bis sie in den zugehörigen Datenstrom geschrieben werden. Das System definiert ein Limit für die Größe des Metadaten-Zwischenspeichers jeder App. Sobald diese Limit erreicht ist, beginnt das System mit dem Löschen von zwischengespeicherten Metadaten. Das System löscht Metadaten, die mit geschrieben wurde **[AppCaptureMetadataPriority.Informational](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)** Priority-Wert vor dem Löschen von Metadaten mit den **[ AppCaptureMetadataPriority.Important](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatapriority)** Priorität.

Zu jedem Zeitpunkt können Sie durch Aufruf von **[RemainingStorageBytesAvailable](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.RemainingStorageBytesAvailable)** die Anzahl der verfügbaren Bytes Metadaten-Zwischenspeicher der App ermitteln. Sie können auch selbst einen Schwellenwert für die App festlegen, um nach dessen Erreichen die Menge der Metadaten zu reduzieren, die Sie in den Zwischenspeicher schreiben. Das folgende Beispiel zeigt eine einfache Implementierung dieses Musters.

[!code-cpp[CheckMetadataStorage](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetCheckMetadataStorage)]

[!code-cpp[ComboExecuted](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetComboExecuted)]

### <a name="receive-notifications-when-the-system-purges-metadata"></a>Empfangen von Benachrichtigungen, wenn das System Metadaten löscht
Sie können registrieren, um eine Benachrichtigung zu erhalten, wenn das System beginnt, Löschen von Metadaten für Ihre app durch Registrieren eines Handlers für die **[MetadataPurged](https://docs.microsoft.com/uwp/api/windows.media.capture.appcapturemetadatawriter.MetadataPurged)** Ereignis.

[!code-cpp[RegisterMetadataPurged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetRegisterMetadataPurged)]

Im Handler für das Ereignis **MetadataPurged** können Sie Platz im Metadaten-Zwischenspeicher schaffen, indem Sie Zustände mit niedriger Priorität beenden, Sie können eine App-bezogene Logik implementieren, um die Menge der Metadaten zu reduzieren, die Sie in den Zwischenspeicher schreiben, oder Sie können nichts tun und das System weiterhin den Zwischenspeicher leeren lassen, basierend auf der Priorität, mit der die Metadaten geschrieben wurden.

[!code-cpp[OnMetadataPurged](./code/AppRecordingExample/cpp/AppRecordingExample/App.cpp#SnippetOnMetadataPurged)]

## <a name="related-topics"></a>Verwandte Themen

* [Gaming](index.md)
 

 




