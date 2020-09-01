---
ms.assetid: 4311D293-94F0-4BBD-A22D-F007382B4DB8
title: Auflisten von Geräten
description: Der Enumeration-Namespace ermöglicht die Suche nach Geräten, die intern mit dem System verbunden, extern verbunden oder über Drahtlos- oder Netzwerkprotokolle erkannt werden können.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 499af7929c67064623e15ee1d475e0f643fafd2b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165464"
---
# <a name="enumerate-devices"></a>Auflisten von Geräten


## <a name="samples"></a>Beispiele

Die einfachste Möglichkeit zum Auflisten aller verfügbaren Geräte ist, mithilfe des [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)-Befehls eine Momentaufnahme zu erstellen (hierauf wird in einem späteren Abschnitt eingegangen).

```CSharp
async void enumerateSnapshot(){
  DeviceInformationCollection collection = await DeviceInformation.FindAllAsync();
}
```

Wenn Sie ein Beispiel zur fortgeschrittenen Verwendung der [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)-APIs herunterladen möchten, klicken Sie [hier](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing).

## <a name="enumeration-apis"></a>Enumerations-APIs

Der Enumeration-Namespace ermöglicht die Suche nach Geräten, die intern mit dem System verbunden, extern verbunden oder über Drahtlos- oder Netzwerkprotokolle erkannt werden können. Die APIs, die Sie für die Enumeration möglicher Geräte verwenden, gehören zum [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)-Namespace. Im Folgenden finden Sie einige Gründe für die Verwendung dieser APIs:

-   Die Suche nach einem Gerät, das mit Ihrer Anwendung verbunden werden kann.
-   Abrufen von Informationen zu Geräten, die mit dem System verbunden sind oder von diesem erkannt werden können.
-   Verfügbarkeit einer App, die Benachrichtigungen empfängt, wenn Geräte hinzugefügt, verbunden oder getrennt werden bzw. ihren Onlinestatus oder andere Eigenschaften ändern.
-   Verfügbarkeit einer App, die Hintergrundtrigger empfängt, wenn Geräte verbunden oder getrennt werden bzw. ihren Onlinestatus oder andere Eigenschaften ändern.

Diese APIs können Geräte über jedes der folgenden Protokolle und jeden der folgenden Busse auflisten, sofern das jeweilige Gerät und das System, unter dem die App ausgeführt wird, diese Technologie unterstützen. Dies ist keine vollständige Liste, und von bestimmten Geräten können auch andere Protokolle unterstützt werden.

-   Physisch verbundene Busse. Dies umfasst PCI und USB. Beispielsweise alle Elemente, die im **Geräte-Manager** angezeigt werden.
-   [UPnP](/windows/desktop/UPnP/universal-plug-and-play-start-page)
-   Digital Living Network Alliance (DLNA)
-   [**Discovery and Launch (DIAL)**](/uwp/api/Windows.Media.DialProtocol)
-   [**DNS Service Discovery (DNS-SD)**](/uwp/api/Windows.Networking.ServiceDiscovery.Dnssd)
-   [Web Services on Devices (WSD)](/windows/desktop/WsdApi/wsd-portal)
-   [Bluetooth](/windows/desktop/Bluetooth/bluetooth-start-page)
-   [**Wi-Fi Direct**](/uwp/api/Windows.Devices.WiFiDirect)
-   WiGig
-   [**Point of Service**](/uwp/api/Windows.Devices.PointOfService)

In vielen Fällen müssen Sie sich über die Verwendung der Enumerations-APIs keine Gedanken machen. Der Grund hierfür ist, dass viele APIs, die Geräte verwenden, automatisch das entsprechende Standardgerät auswählen oder eine optimierte Enumerations-API bereitstellen. Beispielsweise verwendet [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) automatisch das standardmäßige Audiowiedergabegerät. Solange die App das Standardgerät verwenden kann, ist es nicht erforderlich, die Enumerations-APIs in Ihrer Anwendung zu verwenden. Die Enumerations-APIs bieten eine allgemeine und flexible Möglichkeit zum Ermitteln verfügbarer Geräte und zur Verbindung mit den Geräten. Dieses Thema enthält Informationen zur Enumeration von Geräten und beschreibt die vier gängigen Methoden hierfür.

-   Verwenden der [**DevicePicker**](/uwp/api/Windows.Devices.Enumeration.DevicePicker)-Benutzeroberfläche
-   Enumerieren von Momentaufnahmen der zurzeit vom System entdeckbaren Geräte
-   Auflisten der zurzeit auffindbaren Geräte und Überwachung von Änderungen
-   Auflisten der zurzeit auffindbaren Geräte und Überwachen von Änderungen in einer Hintergrundaufgabe

## <a name="deviceinformation-objects"></a>DeviceInformation-Objekte


Beim Arbeiten mit Enumerations-APIs werden Sie häufig [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekte verwenden müssen. Diese Objekte enthalten einen Großteil der verfügbaren Informationen zum Gerät. Die folgende Tabelle beschreibt einige der **DeviceInformation**-Eigenschaften, die für Sie von Interesse sein werden. Die vollständige Liste finden Sie auf der Referenzseite für **DeviceInformation**.

| Eigenschaft                         | Kommentare                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **DeviceInformation.Id**         | Dies ist der eindeutige Bezeichner des Geräts, der als Zeichenfolgenvariable bereitgestellt wird. In den meisten Fällen ist dies ein verdeckter Wert, den Sie einfach von einer Methode an eine andere übergeben, um das für Sie relevante Gerät anzugeben. Sie können diese Eigenschaft und die **DeviceInformation.Kind**-Eigenschaft auch nach dem Schließen und erneuten Öffnen Ihrer App verwenden. Hierdurch wird sichergestellt, dass Sie dasselbe [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekt wiederherstellen und wiederverwenden können. |
| **DeviceInformation.Kind**       | Diese gibt die Art des Geräteobjekts an, das durch das [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekt dargestellt wird. Dies entspricht nicht der Gerätekategorie oder dem Gerätetyp. Ein einzelnes Gerät kann durch mehrere verschiedene **DeviceInformation**-Objekte unterschiedlicher Art dargestellt werden. Die möglichen Werte dieser Eigenschaft sowie ihre Beziehung zueinander werden unter [**DeviceInformationKind**](/uwp/api/windows.devices.enumeration.deviceinformationkind) aufgeführt.                           |
| **DeviceInformation.Properties** | Die Eigenschaftensammlung enthält Informationen, die für das [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekt angefordert werden. Auf die gebräuchlichsten Eigenschaften wird einfach als Eigenschaften des **DeviceInformation**-Objekts verwiesen, z. B. mit [**DeviceInformation.Name**](/uwp/api/windows.devices.enumeration.deviceinformation.name). Weitere Informationen finden Sie unter [Geräteinformationseigenschaften](device-information-properties.md).                                                                |

 

## <a name="devicepicker-ui"></a>DevicePicker-UI


Der [**DevicePicker**](/uwp/api/Windows.Devices.Enumeration.DevicePicker) ist ein von Windows bereitgestelltes Steuerelement, das eine kleine Benutzeroberfläche erstellt, über die der Benutzer ein Gerät aus einer Liste auswählen kann. Sie können das **DevicePicker**-Fenster auf verschiedene Weisen anpassen.

-   Sie können die auf der Benutzeroberfläche angezeigten Geräte steuern, indem Sie [**SupportedDeviceSelectors**](/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors)oder [**SupportedDeviceClasses**](/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) oder beide zum [**DevicePicker.Filter**](/uwp/api/windows.devices.enumeration.devicepicker.filter)-Objekt hinzufügen. In den meisten Fällen müssen Sie nur einen Selektor oder eine Klasse hinzufügen. Sie können bei Bedarf jedoch auch mehrere Selektoren oder Klassen hinzufügen. Wenn Sie mehrere Selektoren oder Klassen hinzufügen, werden diese über eine OR-Logikfunktion verbunden.
-   Sie können die für das Gerät abzurufenden Eigenschaften angeben. Fügen Sie hierzu Eigenschaften zu [**DevicePicker.RequestedProperties**](/uwp/api/windows.devices.enumeration.devicepicker.requestedproperties) hinzu.
-   Sie können die Darstellung der [**devicepicker**](/uwp/api/Windows.Devices.Enumeration.DevicePicker) [**mithilfe von**](/uwp/api/windows.devices.enumeration.devicepicker.appearance)Darstellung ändern.
-   Sie können die Größe und Position von [**DevicePicker**](/uwp/api/Windows.Devices.Enumeration.DevicePicker) angeben, wenn dieses angezeigt wird.

Während [**DevicePicker**](/uwp/api/Windows.Devices.Enumeration.DevicePicker) angezeigt wird, werden die Inhalte der Benutzeroberfläche automatisch aktualisiert, wenn Geräte hinzugefügt, entfernt oder aktualisiert werden.

**Hinweis**    Sie können " [**toviceingeformationkind**](/uwp/api/windows.devices.enumeration.deviceinformationkind) " nicht mithilfe von " [**devicepicker**](/uwp/api/Windows.Devices.Enumeration.DevicePicker)" angeben. Wenn Sie Geräte mit einem bestimmten **DeviceInformationKind** ermitteln möchten, müssen Sie einen [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) erstellen und eine eigene Benutzeroberfläche bereitstellen.

 

Für die Übertragung von Medieninhalten und DIAL werden jeweils eigene Auswahlelemente bereitgestellt, wenn Sie diese verwenden möchten. Diese sind jeweils [**CastingDevicePicker**](/uwp/api/Windows.Media.Casting.CastingDevicePicker) und [**DialDevicePicker**](/uwp/api/Windows.Media.DialProtocol.DialDevicePicker).

## <a name="enumerate-a-snapshot-of-devices"></a>Enumerieren einer Momentaufnahme von Geräten


In einigen Szenarien ist [**DevicePicker**](/uwp/api/Windows.Devices.Enumeration.DevicePicker) wahrscheinlich nicht für Ihre Zwecke geeignet, und Sie benötigen eine flexiblere Alternative. Vielleicht möchten Sie eine eigene Benutzeroberfläche erstellen oder müssen Geräte auflisten, ohne dem Benutzer eine Benutzeroberfläche anzuzeigen. In diesen Fällen können Sie eine Momentaufnahme der Geräte auflisten. Dies umfasst das Durchsuchen der Geräte, die derzeit mit dem System verbunden oder gekoppelt sind. Beachten Sie, dass diese Methode nur Momentaufnahmen der verfügbaren Geräte erfasst. Es werden also keine Geräte gefunden, die verbunden werden, nachdem die Liste durchlaufen wurde. Außerdem werden Sie nicht benachrichtigt, wenn ein Gerät aktualisiert oder entfernt wird. Ein weiterer möglicher Nachteil ist, dass diese Methode sämtliche Ergebnisse zurückhält, bis die gesamte Enumeration abgeschlossen ist. Aus diesem Grund sollten Sie diese Methode nicht verwenden, wenn Sie an den Objekten **AssociationEndpoint**, **AssociationEndpointContainer** oder **AssociationEndpointService** interessiert sind, da diese über ein Netzwerk- oder Drahtlosprotokoll gefunden werden. Dieser Vorgang kann bis zu 30 Sekunden dauern. In diesem Szenario sollten Sie ein [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)-Objekt verwenden, um die möglichen Geräte zu enumerieren.

Um Momentaufnahmen von Geräten zu enumerieren, verwenden Sie die [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)-Methode. Diese Methode wartet, bis der gesamte Enumerationsvorgang abgeschlossen ist, und gibt alle Ergebnisse als ein [**DeviceInformationCollection**](/uwp/api/windows.devices.enumeration.deviceinformationcollection)-Objekt zurück. Zudem handelt es sich um eine überladene Methode, die Ihnen verschiedene Optionen zum Filtern und Einschränken der Ergebnisse auf die für Sie relevanten Geräte bietet. Zu diesem Zweck können Sie [**DeviceClass**](/uwp/api/Windows.Devices.Enumeration.DeviceClass) bereitstellen oder eine Geräteauswahl übergeben. Die Geräteauswahl ist eine AQS-Zeichenfolge, die die zu enumerierenden Geräte angibt. Weitere Informationen finden Sie unter [Erstellen einer Geräteauswahl](build-a-device-selector.md).

Nachfolgend finden Sie ein Beispiel für die Momentaufnahme einer Geräteaufzählung:



Zusätzlich zur Einschränkung der Ergebnisse können Sie auch die Eigenschaften angeben, die Sie für die Geräte abrufen möchten. In diesem Fall sind die angegebenen Eigenschaften in der Eigenschaftensammlung jedes [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekts verfügbar, das in der Sammlung zurückgegeben wird. Es muss beachtet werden, dass nicht alle Eigenschaften für sämtliche Gerätearten verfügbar sind. Informationen zu den Eigenschaften, die für welche Geräte Arten verfügbar sind, finden Sie unter [Eigenschaften von Geräteinformationen](device-information-properties.md).



## <a name="enumerate-and-watch-devices"></a>Auflisten und Anzeigen von Geräten


Eine leistungsfähigere und flexiblere Methode für die Enumeration von Geräten ist die Erstellung von [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher). Diese Option bietet die größte Flexibilität bei der Enumeration von Geräten. Sie ermöglicht Ihnen die Enumeration der aktuell verfügbaren Geräte sowie den Empfang von Benachrichtigungen, wenn mit der Geräteauswahl übereinstimmende Geräte hinzugefügt oder entfernt bzw. deren Eigenschaften geändert werden. Beim Erstellen von **DeviceWatcher** stellen Sie eine Geräteauswahl bereit. Weitere Informationen zur Geräteauswahl finden Sie unter [Erstellen einer Geräteauswahl](build-a-device-selector.md). Nachdem Sie die Überwachung erstellt haben, erhalten Sie für jedes Gerät, das die angegebenen Kriterien erfüllt, die folgenden Benachrichtigungen.

-   Benachrichtigung über das Hinzufügen eines neuen Geräts
-   Benachrichtigung über die Änderung einer für Sie relevanten Eigenschaft
-   Benachrichtigung über das Entfernen eines Geräts, das entweder nicht mehr verfügbar ist oder die Filterkriterien nicht mehr erfüllt

In den meisten Fällen, in denen Sie [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) verwenden, verwalten Sie eine Liste der Geräte, fügen Elemente hinzu und entfernen oder aktualisieren Elemente, während das Überwachungselement Updates von den überwachten Geräten empfängt. Wenn Sie eine Aktualisierungsbenachrichtigung empfangen, sind die aktualisierten Informationen als [**DeviceInformationUpdate**](/uwp/api/windows.devices.enumeration.deviceinformationupdate)-Objekt verfügbar. Um die Geräteliste zu aktualisieren, suchen Sie zunächst die entsprechenden geänderten [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation). Anschließend rufen Sie die [**Update**](/uwp/api/windows.devices.enumeration.deviceinformation.update)-Methode für dieses Objekt auf, indem Sie das **DeviceInformationUpdate**-Objekt angeben. Dabei handelt es sich um eine benutzerfreundliche Funktion, die Ihr **DeviceInformation**-Objekt automatisch aktualisiert.

Da ein [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) Benachrichtigungen sendet, wenn Geräte hinzugefügt und geändert werden, sollten Sie diese Methode für die Enumeration von Geräten verwenden, wenn Sie an **AssociationEndpoint**-, **AssociationEndpointContainer**- oder **AssociationEndpointService**-Objekten interessiert sind, da diese über Netzwerk- oder Drahtlosprotokolle enumeriert werden.

Zum Erstellen von [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) verwenden Sie eine der [**CreateWatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher)-Methoden. Es handelt sich um überladene Methoden, die die Angabe der für Sie relevanten Geräte ermöglichen. Zu diesem Zweck können Sie [**DeviceClass**](/uwp/api/Windows.Devices.Enumeration.DeviceClass) bereitstellen oder eine Geräteauswahl übergeben. Die Geräteauswahl ist eine AQS-Zeichenfolge, die die zu enumerierenden Geräte angibt. Weitere Informationen finden Sie unter [Erstellen einer Geräteauswahl](build-a-device-selector.md). Sie können auch die Eigenschaften angeben, die Sie für die Geräte abrufen möchten, die für Sie interessant sind. In diesem Fall sind die angegebenen Eigenschaften in der Eigenschaftensammlung jedes [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekts verfügbar, das in der Sammlung zurückgegeben wird. Es muss beachtet werden, dass nicht alle Eigenschaften für sämtliche Gerätearten verfügbar sind. Welche Eigenschaften für welche Gerätearten verfügbar sind, erfahren Sie unter [Geräteinformationseigenschaften](device-information-properties.md).

## <a name="watch-devices-as-a-background-task"></a>Überwachen von Geräten als Hintergrundaufgabe


Die Geräteüberwachung als Hintergrundaufgabe ist nahezu identisch mit der oben beschriebenen Erstellung von [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher). Tatsächlich müssen Sie zunächst ein normales **DeviceWatcher**-Objekt erstellen wie im vorangehenden Abschnitt beschrieben. Nach der Erstellung rufen Sie [**GetBackgroundTrigger**](/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) anstelle von [**DeviceWatcher.Start**](/uwp/api/windows.devices.enumeration.devicewatcher.start) auf. Beim Aufruf von **GetBackgroundTrigger** müssen Sie angeben, welche der Benachrichtigungen Ihnen wichtig sind: Hinzufügen, Entfernen oder Aktualisieren. Benachrichtigungen über das Aktualisieren oder Entfernen können nur zusammen mit einer Benachrichtigung über das Hinzufügen angefordert werden. Nachdem Sie den Trigger registriert haben, wird die Ausführung von **DeviceWatcher** sofort im Hintergrund gestartet. Ab dann wird die Hintergrundaufgabe jedes Mal ausgelöst, wenn sie eine neue mit den Kriterien übereinstimmende Benachrichtigung für Ihre Anwendung empfängt. Sie teilt Ihnen die seit der letzten Auslösung der Anwendung vorgenommenen Änderungen mit.

**Wichtig**    Wenn ein [**devicewatchertrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceWatcherTrigger) zum ersten Mal die Anwendung auslöst, wird der Status " **enumerationabgeschlossene** " erreicht. Dies bedeutet, dass alle anfänglich verfügbaren Ergebnisse enthalten sind. Wenn die Anwendung in Zukunft ausgelöst wird, enthält die Hintergrundaufgabe nur die Benachrichtigungen über Hinzufügungs-, Aktualisierungs- und Entfernungsvorgänge, die seit dem letzten Trigger aufgetreten sind. Dies unterscheidet sich geringfügig von dem im Vordergrund ausgeführten [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)-Objekt, da die anfänglichen Ergebnisse nicht nacheinander eingehen, sondern erst nach Erreichen des Zustands **EnumerationCompleted** gebündelt übermittelt werden.

 

Abhängig davon, ob der Scanvorgang im Vorder- oder Hintergrund ausgeführt wird, unterscheidet sich das Verhalten einiger Drahtlosprotokolle. Einige Protokolle unterstützen möglicherweise keine Scans im Hintergrund. Für Scans im Hintergrund gibt es drei Möglichkeiten. Die folgende Tabelle enthält die Möglichkeiten und Auswirkungen, die sich ggf. für die Anwendung ergeben. Beispielsweise unterstützen Bluetooth und Wi-Fi Direct keine Hintergrundscans, sodass Sie von der Erweiterung keine [**devicewatcher-**](/uwp/api/Windows.ApplicationModel.Background.DeviceWatcherTrigger)Funktion unterstützen.

| Verhalten                                  | Auswirkung                                                                                                                                  |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| Identisches Verhalten im Hintergrund               | Keine                                                                                                                                    |
| Nur passive Hintergrundscans | Beim Warten auf einen passiven Scan dauert die Geräteermittlung möglicherweise länger.                                                           |
| Keine Unterstützung von Hintergrundscans            | Von [**DeviceWatcherTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceWatcherTrigger) werden keine Geräte erkannt, und es werden keine Updates gemeldet. |

 

Wenn [**DeviceWatcherTrigger**](/uwp/api/Windows.ApplicationModel.Background.DeviceWatcherTrigger) ein Protokoll enthält, von dem das Scannen als Hintergrundaufgabe nicht unterstützt wird, ist der Trigger trotzdem funktionsfähig. Sie sind jedoch nicht in der Lage, Updates oder Ergebnisse über dieses Protokoll abzurufen. Die Updates für andere Protokolle oder Geräte werden weiterhin auf normale Weise erkannt.

## <a name="using-deviceinformationkind"></a>Verwenden von DeviceInformationKind


In den meisten Szenarien müssen Sie sich nicht mit [**DeviceInformationKind**](/uwp/api/windows.devices.enumeration.deviceinformationkind) eines [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekts befassen. Die von der verwendeten Geräte-API zurückgegebene Geräteauswahl sorgt in vielen Fällen dafür, dass Sie die richtigen Geräteobjektarten für die betreffende API erhalten. In einigen Szenarien möchten Sie jedoch **DeviceInformation** für Geräte abrufen, haben jedoch keine entsprechende Geräte-API zur Bereitstellung einer Geräteauswahl zur Verfügung. In diesen Fällen müssen Sie eine eigene Auswahl erstellen. Webdienste für Geräte (Web Services on Devices) verfügt beispielsweise über keine dedizierte API. Sie können jedoch die [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)-APIs verwenden, um diese Geräte zu ermitteln und Informationen über sie zu erhalten, und sie anschließend mit Sockets-APIs verwenden.

Wenn Sie eine eigene Geräteauswahl zum Enumerieren von Geräteobjekten erstellen, kommt es auf das genaue Verständnis von [**DeviceInformationKind**](/uwp/api/windows.devices.enumeration.deviceinformationkind) an. Alle möglichen Arten sowie ihre Beziehungen zueinander werden auf der Referenzseite für **DeviceInformationKind** beschrieben. Eine der häufigsten Arten der Verwendung von **DeviceInformationKind** besteht darin, beim Übermitteln einer Abfrage zusammen mit einer Geräteauswahl die Art der gesuchten Geräte anzugeben. Auf diese Weise wird sichergestellt, dass nur Geräte aufgelistet werden, die mit der bereitgestellten **DeviceInformationKind** übereinstimmen. Beispielsweise könnten Sie ein **DeviceInterface**-Objekt finden und anschließend eine Abfrage ausführen, um die Informationen für das übergeordnete **Device**-Objekt abzurufen. Das übergeordnete Objekt enthält möglicherweise zusätzliche Informationen.

Es ist wichtig, zu beachten, dass die in der Eigenschaftensammlung für ein [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekt verfügbaren Eigenschaften je nach [**DeviceInformationKind**](/uwp/api/windows.devices.enumeration.deviceinformationkind) des Geräts variieren. Manche Eigenschaften sind nur für bestimmte Arten verfügbar. Weitere Informationen dazu, welche Eigenschaften für welche Arten verfügbar sind, finden Sie unter [Geräteinformationseigenschaften](device-information-properties.md). Daher erhalten Sie im vorangehenden Beispiel bei der Suche nach dem übergeordneten **Device** Zugriff auf weitere Informationen, die über das **DeviceInterface**-Geräteobjekt nicht zur Verfügung gestellt wurden. Beim Erstellen von AQS-Filterzeichenfolgen muss deshalb sichergestellt werden, dass die angeforderten Eigenschaften für die aufzulistenden **DeviceInformationKind**-Objekte verfügbar sind. Weitere Informationen zum Erstellen eines Filters finden Sie unter [Erstellen einer Geräteauswahl](build-a-device-selector.md).

Beim Enumerieren von **AssociationEndpoint**-, **AssociationEndpointContainer**- oder **AssociationEndpointService**-Objekten führen Sie eine Enumeration über ein Drahtlos- oder Netzwerkprotokoll durch. In diesen Situationen empfiehlt es sich, dass Sie nicht [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync), sondern [**CreateWatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) verwenden. Das liegt daran, dass die Suche über ein Netzwerk häufig zu Suchvorgängen führt, die erst nach mindestens 10 Sekunden ablaufen, bevor [**EnumerationCompleted**](/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) generiert wird. **FindAllAsync** schließt den Vorgang erst ab, wenn **EnumerationCompleted** ausgelöst wird. Bei Verwendung von [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) liegen die Ergebnisse näher an der Echtzeit, unabhängig davon, wann **EnumerationCompleted** aufgerufen wird.

## <a name="save-a-device-for-later-use"></a>Speichern eines Geräts zur späteren Verwendung


Jedes [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekt wird durch eine Kombination aus zwei Informationselementen eindeutig identifiziert: [**DeviceInformation.Id**](/uwp/api/windows.devices.enumeration.deviceinformation.id) und [**DeviceInformation.Kind**](/uwp/api/windows.devices.enumeration.deviceinformation.kind). Wenn Sie diese beiden Informationen beibehalten, können Sie ein **DeviceInformation** -Objekt neu erstellen, nachdem es verloren gegangen ist, indem Sie diese Informationen für " [**kreatefroferasync**](/uwp/api/windows.devices.enumeration.deviceinformation.createfromidasync)" bereitstellen. Auf diese Weise können Sie Benutzereinstellungen für ein Gerät speichern, mit dem die App verwendet wird.


 

 