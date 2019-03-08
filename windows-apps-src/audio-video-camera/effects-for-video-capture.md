---
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: In diesem Thema erfahren Sie, wie Sie Effekte auf die Kameravorschau und auf Videoaufnahmedatenströme anwenden und wie Sie den Videostabilisierungseffekt verwenden.
title: Effekte für die Videoaufnahme
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e9960e66c6bcdd7105e201d48e2317de4a39a19a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636625"
---
# <a name="effects-for-video-capture"></a>Effekte für die Videoaufnahme


In diesem Thema erfahren Sie, wie Sie Effekte auf die Kameravorschau und auf Videoaufnahmedatenströme anwenden und wie Sie den Videostabilisierungseffekt verwenden.

> [!NOTE] 
> Dieser Artikel baut auf Konzepten und Code auf, die unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Wir empfehlen Ihnen, sich mit dem grundlegenden Medienaufnahmemuster in diesem Artikel vertraut zu machen, bevor Sie sich komplexeren Aufnahmeszenarien zuwenden. Der Code in diesem Artikel setzt voraus, dass Ihre App bereits über eine korrekt initialisierte MediaCapture-Instanz verfügt.

## <a name="adding-and-removing-effects-from-the-camera-video-stream"></a>Hinzufügen und Entfernen von Effekten im Kameravideodatenstrom
Um Videos mithilfe der Gerätekamera aufzunehmen oder in der Vorschau anzuzeigen, verwenden Sie das [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture)-Objekt, wie unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) beschrieben. Nachdem Sie das **MediaCapture**-Objekt initialisiert haben, können Sie dem Vorschau- oder Aufnahmedatenstrom einen oder mehrere Videoeffekte hinzufügen, indem Sie [**AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035) aufrufen, ein [**IVideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IVideoEffectDefinition)-Objekt übergeben, das den hinzuzufügenden Effekt darstellt, und einen Member der [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaStreamType)-Enumeration übergeben, der angibt, ob der Effekt dem Vorschaudatenstrom oder Aufnahmedatenstrom der Kamera hinzugefügt werden soll.

> [!NOTE]
> Auf einigen Geräten sind der Vorschaudatenstrom und Aufnahmedatenstrom identisch. Wenn Sie **MediaStreamType.VideoPreview** oder **MediaStreamType.VideoRecord** angeben und **AddVideoEffectAsync** aufrufen, wird der Effekt folglich sowohl auf den Vorschaudatenstrom als auch auf den Aufnahmedatenstrom angewendet. Sie können bestimmen, ob der Vorschaudatenstrom und Aufnahmedatenstrom auf dem aktuellen Gerät identisch sind, indem Sie die [**VideoDeviceCharacteristic**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSettings.VideoDeviceCharacteristic)-Eigenschaft von [**MediaCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.MediaCaptureSettings) auf das **MediaCapture**-Objekt überprüfen. Wenn der Wert dieser Eigenschaft **VideoDeviceCharacteristic.AllStreamsIdentical** oder **VideoDeviceCharacteristic.PreviewRecordStreamsIdentical** lautet, sind die Datenströme identisch, und alle auf einen Datenstrom angewendeten Effekte wirken sich auch auf den anderen aus.

Im folgenden Beispiel wird sowohl dem Vorschaudatenstrom als auch dem Aufnahmedatenstrom der Kamera ein Effekt hinzugefügt. Dieses Beispiel veranschaulicht, wie Sie überprüfen, ob der Aufnahme- und Vorschaudatenstrom identisch sind.

[!code-cs[BasicAddEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetBasicAddEffect)]

Beachten Sie, dass **AddVideoEffectAsync** ein Objekt zurückgibt, das die [**IMediaExtension**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.IMediaExtension)-Schnittstelle implementiert, die den hinzugefügten Videoeffekt darstellt. Bei einigen Effekten können die Effekteinstellungen geändert werden, indem Sie [**PropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Collections.PropertySet) an die [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986)-Methode übergeben.

Ab Windows 10, Version 1607, können Sie auch das von **AddVideoEffectAsync** zurückgegebene Objekt verwenden, um den Effekt aus der Videopipeline zu entfernen, indem Sie es an [**RemoveEffectAsync**](https://msdn.microsoft.com/library/windows/apps/mt667957) übergeben. **RemoveEffectAsync** ermittelt automatisch, ob der Effektobjektparameter dem Vorschau- oder Aufnahmedatenstrom hinzugefügt wurde. Auf diese Weise müssen Sie beim Aufrufen keinen Datenstromtyp angeben.

[!code-cs[RemoveOneEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetRemoveOneEffect)]

Sie können auch alle Effekte aus dem Vorschau- oder Aufnahmedatenstrom entfernen, indem Sie [**ClearEffectsAsync**](https://msdn.microsoft.com/library/windows/apps/br226592) aufrufen und den Datenstrom angeben, für den alle Effekte entfernt werden sollen.

[!code-cs[ClearAllEffects](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetClearAllEffects)]

## <a name="video-stabilization-effect"></a>Videostabilisierungseffekt

Der Videostabilisierungseffekt ändert die Frames eines Videodatenstroms, um das Wackeln durch das Halten des Aufnahmegeräts mit der Hand zu minimieren. Da durch diese Technik Pixel nach rechts, links, oben und unten verschoben werden und der Effekt den Inhalt außerhalb des Videoframes nicht kennen kann, wird das stabilisierte Video im Vergleich zum Originalvideo leicht zugeschnitten. Eine Hilfsfunktion wird bereitgestellt, damit Sie Ihre Videocodierungseinstellungen zum optimalen Verwalten der durch den Effekt vorgenommenen Zuschneidung anpassen können.

Auf Geräten, die dies unterstützen, stabilisiert OIS (Optical Image Stabilization) das Video durch mechanisches Manipulieren das Aufnahmegeräts, sodass die Kanten der Videoframes nicht zugeschnitten werden müssen. Weitere Informationen finden Sie unter [Steuerelemente des Aufnahmegeräts für Videoaufnahmen](capture-device-controls-for-video-capture.md).

### <a name="set-up-your-app-to-use-video-stabilization"></a>Einrichten der App für die Verwendung der Videostabilisierung

Neben den Namespaces, die für die grundlegende Medienaufnahme erforderlich sind, erfordert die Verwendung des Videostabilisierungseffekts den folgenden Namespace.

[!code-cs[VideoStabilizationEffectUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetVideoStabilizationEffectUsing)]

Deklarieren Sie eine Membervariable zum Speichern des [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760)-Objekts. Im Rahmen der Effektimplementierung ändern Sie die Codierungseigenschaften, die Sie für das Codieren des aufgezeichneten Videos verwenden. Deklarieren Sie zwei Variablen zum Speichern einer Sicherungskopie der ursprünglichen Eingabe- und Ausgabecodierungseigenschaften, sodass Sie diese später wiederherstellen können, wenn der Effekt deaktiviert ist. Deklarieren Sie abschließend eine Membervariable vom Typ [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026), da auf dieses Objekt von mehreren Positionen innerhalb des Codes aus zugegriffen wird.

[!code-cs[DeclareVideoStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetDeclareVideoStabilizationEffect)]

Für dieses Szenario sollten Sie einer Membervariablen das Profilobjekt für die Mediencodierung zuweisen, damit Sie später darauf zugreifen können.

[!code-cs[EncodingProfileMember](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetEncodingProfileMember)]

### <a name="initialize-the-video-stabilization-effect"></a>Initialisieren des Videostabilisierungseffekts

Erstellen Sie nach dem Initialisieren des **MediaCapture**-Objekts eine neue Instanz des [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762)-Objekts. Rufen Sie [**MediaCapture.AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035) auf, um den Effekt der Videopipeline hinzuzufügen und eine Instanz der [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760)-Klasse abzurufen. Geben Sie [**MediaStreamType.VideoRecord**](https://msdn.microsoft.com/library/windows/apps/br226640) an, um anzuzeigen, dass der Effekt auf den Videoaufnahmedatenstrom angewendet werden soll.

Registrieren Sie einen Ereignishandler für das Ereignis [**EnabledChanged**](https://msdn.microsoft.com/library/windows/apps/dn948982), und rufen Sie die Hilfsmethode **SetUpVideoStabilizationRecommendationAsync** auf. Beide werden später in diesem Artikel erläutert. Legen Sie abschließend die [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926775)-Eigenschaft des Effekts auf „true“ fest, um den Effekt zu aktivieren.

[!code-cs[CreateVideoStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetCreateVideoStabilizationEffect)]

### <a name="use-recommended-encoding-properties"></a>Verwenden empfohlener Codierungseigenschaften

Wie weiter oben in diesem Artikel beschrieben wurde, verursacht die Technik, die der Videostabilisierungseffekt verwendet, eine geringe Zuschneidung des stabilisierten Videos im Vergleich zum Quellvideo. Definieren Sie die folgende Hilfsfunktion in Ihrem Code, um die Videocodierungseigenschaften anzupassen und diese Beschränkung des Effekts optimal zu verarbeiten. Dieser Schritt ist nicht erforderlich, um den Videostabilisierungseffekt zu verwenden. Wenn Sie diesen Schritt jedoch nicht ausführen, wird das resultierende Video leicht hochskaliert und hat dadurch eine etwas niedrigere Bildqualität.

Rufen Sie [**GetRecommendedStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948983) für die Instanz des Videostabilisierungseffekts auf, übergeben Sie das [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825)-Objekt, das den Effekt über Ihre aktuellen Eingabedatenstrom-Codierungseigenschaften informiert, und Ihr **MediaEncodingProfile**, sodass der Effekt Ihre aktuellen Ausgabecodierungseigenschaften kennt. Diese Methode gibt ein [**VideoStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn926727)-Objekt zurück, das neue empfohlene Eingabe- und Ausgabedatenstrom-Codierungseigenschaften enthält.

Die empfohlenen Eingabecodierungseigenschaften bieten, sofern vom Gerät unterstützt, eine höhere Auflösung als die ursprünglichen Einstellungen, die Sie bereitgestellt haben, sodass es einen minimalen Datenverlust bei der Auflösung nach dem Anwenden der Zuschneidung des Effekts gibt.

Rufen Sie [**VideoDeviceController.SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895) auf, um die neuen Codierungseigenschaften festzulegen. Verwenden Sie vor dem Festlegen der neuen Eigenschaften die Membervariable, um die ursprünglichen Codierungseigenschaften zu speichern, damit Sie die Einstellungen wieder zurückändern können, wenn Sie den Effekt deaktivieren.

Wenn der Videostabilisierungseffekt das Ausgabevideo zuschneiden muss, entsprechen die empfohlenen Ausgabecodierungseigenschaften der Größe des zugeschnittenen Videos. Das bedeutet, dass die Ausgabeauflösung mit der Größe des zugeschnittenen Videos übereinstimmt. Wenn Sie die empfohlenen Ausgabeeigenschaften nicht verwenden, wird das Video hochskaliert, um der anfänglichen Ausgabegröße zu entsprechen, was zu einem Verlust an Bildqualität führt.

Legen Sie die [**Video**](https://msdn.microsoft.com/library/windows/apps/hh701124)-Eigenschaft des **MediaEncodingProfile**-Objekts fest. Verwenden Sie vor dem Festlegen der neuen Eigenschaften die Membervariable, um die ursprünglichen Codierungseigenschaften zu speichern, damit Sie die Einstellungen wieder zurückändern können, wenn Sie den Effekt deaktivieren.

[!code-cs[SetUpVideoStabilizationRecommendationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetSetUpVideoStabilizationRecommendationAsync)]

### <a name="handle-the-video-stabilization-effect-being-disabled"></a>Behandeln des Deaktivierens des Videostabilisierungseffekts

Das System kann den Videostabilisierungseffekt automatisch deaktivieren, wenn Sie der Pixeldurchsatz zu groß für die Verarbeitung durch den Effekt ist oder wenn erkannt wird, dass der Effekt langsam ausgeführt wird. In diesem Fall wird das „EnabledChanged“-Ereignis ausgelöst. Die **VideoStabilizationEffect**-Instanz im Parameter *sender* gibt den neuen Zustand des Effekts (aktiviert oder deaktiviert) an. [  **VideoStabilizationEffectEnabledChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn948979) hat einen [**VideoStabilizationEffectEnabledChangedReason**](https://msdn.microsoft.com/library/windows/apps/dn948981)-Wert, der angibt, warum der Effekt aktiviert oder deaktiviert wurde. Beachten Sie, dass dieses Ereignis auch ausgelöst wird, wenn Sie den Effekt programmgesteuert aktivieren oder deaktivieren. In diesem Fall ist der Grund **Programmatic**.

In der Regel verwenden Sie dieses Ereignis zum Anpassen der Benutzeroberfläche Ihrer App, um den aktuellen Status der Videostabilisierung anzugeben.

[!code-cs[VideoStabilizationEnabledChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetVideoStabilizationEnabledChanged)]

### <a name="clean-up-the-video-stabilization-effect"></a>Bereinigen des Videostabilisierungseffekts

Rufen Sie zum Bereinigen des Videostabilisierungseffekts [**RemoveEffectAsync**](https://msdn.microsoft.com/library/windows/apps/mt667957) auf, um den Effekt aus der Videopipeline zu entfernen. Wenn die Membervariablen mit den ursprünglichen Codierungseigenschaften nicht NULL sind, verwenden Sie diese zum Wiederherstellen der Codierungseigenschaften. Entfernen Sie schließlich den Ereignishandler **EnabledChanged**, und legen Sie den Effekt auf Null fest.

[!code-cs[CleanUpVisualStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetCleanUpVisualStabilizationEffect)]

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Erfassen Sie grundlegende Foto, Video- und Audiodateien mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




