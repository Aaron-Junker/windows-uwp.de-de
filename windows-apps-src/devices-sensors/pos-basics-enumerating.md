---
title: Auflisten von pointo Service-Geräten
description: Informieren Sie sich über vier Methoden zur Verwendung einer Geräteauswahl zum Abfragen und Auflisten der für Ihr System verfügbaren pointfservice-Geräte.
ms.date: 10/08/2018
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 6d1767e49e14f424b7d21424fbfc02b76acd546a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159674"
---
# <a name="enumerating-point-of-service-devices"></a>Zählen von Point of Service-Geräten
In diesem Abschnitt erfahren Sie, wie Sie [eine Geräteauswahl definieren](./build-a-device-selector.md) , die zum Abfragen von Geräten verwendet wird, die für das System verfügbar sind, und diese Auswahl zum Auflisten von Point-of-Service-Geräten mithilfe einer der folgenden Methoden verwenden:

**Methode 1:** [Verwenden einer Geräte](#method-1-use-a-device-picker) Auswahl
<br/>
Anzeigen einer Benutzeroberfläche für die Geräteauswahl und Anzeigen eines verbundenen Geräts durch den Benutzer. Diese Methode verarbeitet das Aktualisieren der Liste, wenn Geräte angefügt und entfernt werden, und ist einfacher und sicherer als andere Methoden.

**Methode 2:** [erstes verfügbares Gerät erhalten](#method-2-get-first-available-device)<br />Verwenden Sie [getdefaultasync](/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) , um auf das erste verfügbare Gerät an einer bestimmten Stelle der Dienst Geräteklasse zuzugreifen.

**Methode 3:** [Momentaufnahme von Geräten](#method-3-snapshot-of-devices)<br />Listet eine Momentaufnahme von Point-of-Service-Geräten auf, die zu einem bestimmten Zeitpunkt auf dem System vorhanden sind. Dies ist hilfreich, wenn Sie eine eigene Benutzeroberfläche erstellen oder Geräte auflisten müssen, ohne dem Benutzer eine Benutzeroberfläche anzuzeigen. [Findallasync](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) hält Ergebnisse zurück, bis die gesamte Enumeration abgeschlossen ist.

**Methode 4:** [aufzählen und überwachen](#method-4-enumerate-and-watch)<br />[Devicewatcher](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) ist ein leistungsfähigeres und flexibleres enumerationsmodell, mit dem Sie derzeit vorhandene Geräte auflisten und außerdem Benachrichtigungen erhalten, wenn Geräte dem System hinzugefügt oder daraus entfernt werden.  Dies ist hilfreich, wenn Sie eine aktuelle Liste von Geräten im Hintergrund für die Anzeige auf der Benutzeroberfläche verwalten möchten, anstatt auf eine Momentaufnahme zu warten.

## <a name="define-a-device-selector"></a>Definieren einer Geräteauswahl
Mithilfe einer Geräteauswahl können Sie die gesuchten Geräte beim Auflisten von Geräten einschränken.  Dadurch erhalten Sie nur relevante Ergebnisse und verkürzen die Zeit, die zum Auflisten der gewünschten Geräte benötigt wird.

Sie können die **getdeviceselector** -Methode für den Typ des Geräts verwenden, das Sie suchen, um die Geräteauswahl für diesen Typ zu erhalten. Beispielsweise erhalten Sie mithilfe von " [posprinter. getdebug-elector](/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) " einen Selektor zum Auflisten aller an das System angeschlossenen [posprinters](/uwp/api/windows.devices.pointofservice.posprinter) , einschließlich USB-, Netzwerk-und Bluetooth-POS-Druckern.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

Die **getdeviceselector** -Methoden für die verschiedenen Gerätetypen lauten wie folgt:

* [Barcodescanner. getdeviceselector](/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [Cashqueue. getde viceselector](/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay. getde viceselector](/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* ["Magneticstripereader. getdeviceselector"](/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [Posprinter. getde viceselector](/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

Mithilfe einer **getdeviceselector** -Methode, die einen " [posconnectiontypes](/uwp/api/windows.devices.pointofservice.posconnectiontypes) "-Wert als Parameter annimmt, können Sie den Selektor so einschränken, dass er lokale, Netzwerk-oder Bluetooth-verbundene POS-Geräte aufzählt, um die Zeit zu verkürzen, die für den Abschluss der Abfrage benötigt wird.  Das folgende Beispiel zeigt die Verwendung dieser Methode, um eine Auswahl zu definieren, die nur lokal angeschlossene POS-Drucker unterstützt.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> Weitere Informationen finden Sie unter [Erstellen einer Geräteauswahl](./build-a-device-selector.md) zum Erstellen erweiterter Auswahl Zeichenfolgen.

## <a name="method-1-use-a-device-picker"></a>Methode 1: Verwenden einer Geräteauswahl

Mit der [devicepicker](/uwp/api/windows.devices.enumeration.devicepicker) -Klasse können Sie ein Auswahl-Flyout anzeigen, das eine Liste von Geräten enthält, aus denen der Benutzer auswählen kann. Mit der Eigenschaft [Filter](/uwp/api/windows.devices.enumeration.devicepicker.filter) können Sie auswählen, welche Gerätetypen in der Auswahl angezeigt werden sollen. Diese Eigenschaft hat den Typ [devicepickerfilter](/uwp/api/windows.devices.enumeration.devicepickerfilter). Sie können Gerätetypen dem Filter mithilfe der [supporteddebug](/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) -oder [supporteddebug](/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) -Eigenschaft hinzufügen.

Wenn Sie bereit sind, die Geräteauswahl anzuzeigen, können Sie die [picksingledeviceasync](/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) -Methode aufrufen, die die Auswahl Benutzeroberfläche anzeigt und das ausgewählte Gerät zurückgibt. Sie müssen ein [Rect](/uwp/api/windows.foundation.rect) angeben, das festlegt, wo das Flyout angezeigt wird. Diese Methode gibt ein " [DeviceInformation](/uwp/api/windows.devices.enumeration.deviceinformation) "-Objekt zurück. damit Sie es mit den Point of Service-APIs verwenden können, müssen Sie die **fromittel Async** -Methode für die jeweilige gewünschte Geräteklasse verwenden. Übergeben Sie die [DeviceInformation.ID](/uwp/api/windows.devices.enumeration.deviceinformation.id) -Eigenschaft als *DeviceID* -Parameter der Methode, und erhalten Sie eine Instanz der Geräteklasse als Rückgabewert.

Der folgende Code Ausschnitt erstellt eine **devicepicker**, fügt ihr einen Barcode Scanner-Filter hinzu, hat den Benutzer ein Gerät ausgewählt und erstellt dann ein **Barcodescanner** -Objekt auf der Grundlage der Geräte-ID:

```cs
private async Task<BarcodeScanner> GetBarcodeScanner()
{
    DevicePicker devicePicker = new DevicePicker();
    devicePicker.Filter.SupportedDeviceSelectors.Add(BarcodeScanner.GetDeviceSelector());
    Rect rect = new Rect();
    DeviceInformation deviceInformation = await devicePicker.PickSingleDeviceAsync(rect);
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceInformation.Id);
    return barcodeScanner;
}
```

## <a name="method-2-get-first-available-device"></a>Methode 2: erstes verfügbares Gerät erhalten

Die einfachste Möglichkeit, ein Point-of-Service-Gerät abzurufen, ist die Verwendung von **getdefaultasync** , um das erste verfügbare Gerät innerhalb einer Point of Service-Geräteklasse zu erhalten. 

Das folgende Beispiel veranschaulicht die Verwendung von [getdefaultasync](/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) für [Barcodescanner](/uwp/api/windows.devices.pointofservice.barcodescanner). Das Codierungs Muster ist für alle Point of Service-Geräteklassen ähnlich.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> " **Getdefaultasync** " muss mit Bedacht verwendet werden, da es möglicherweise ein anderes Gerät von einer Sitzung zur nächsten zurückgibt. Viele Ereignisse können diese Enumeration beeinflussen, was zu einem anderen ersten verfügbaren Gerät führt, einschließlich: 
> - Änderung an Kameras, die mit Ihrem Computer verbunden sind 
> - Änderung an Punkt der Dienst Geräte, die mit Ihrem Computer verbunden sind
> - Änderung des mit dem Netzwerk verbundenen Punkts der Dienst Geräte, die in Ihrem Netzwerk verfügbar sind
> - Ändern der Dienst Geräte für Bluetooth-Punkte innerhalb des Bereichs Ihres Computers 
> - Änderungen an der Point-of-Service-Konfiguration 
> - Installation von Treibern oder OPOS-Dienst Objekten
> - Installation von Point-of-Service-Erweiterungen
> - Aktualisieren des Windows-Betriebssystems

## <a name="method-3-snapshot-of-devices"></a>Methode 3: Momentaufnahme von Geräten

In einigen Szenarien möchten Sie möglicherweise eine eigene Benutzeroberfläche erstellen oder Geräte auflisten, ohne dem Benutzer eine Benutzeroberfläche anzuzeigen.  In diesen Fällen können Sie eine Momentaufnahme von Geräten auflisten, die zurzeit mit dem System verbunden sind oder mit dem System "Geräte" mit dem Typ "" [deaktiviert](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)sind.  Diese Methode hält alle Ergebnisse zurück, bis die gesamte Enumeration abgeschlossen ist.

> [!TIP]
> Es wird empfohlen, die **getdeviceselector** -Methode mit dem **posconnectiontypes** -Parameter zu verwenden, wenn Sie **findallasync** verwenden, um die Abfrage auf den gewünschten Verbindungstyp zu beschränken.  Netzwerk-und Bluetooth-Verbindungen können die Ergebnisse verzögern, wenn Ihre Enumerationen abgeschlossen sein müssen, bevor die Ergebnisse von **findallasync** zurückgegeben werden.

> [!CAUTION] 
> **Findallasync** gibt ein Array von Geräten zurück.  Die Reihenfolge dieses Arrays kann von Sitzung zu Sitzung geändert werden. Daher wird nicht empfohlen, sich auf eine bestimmte Reihenfolge zu verlassen, indem Sie einen hart codierten Index in das Array verwenden.  Verwenden Sie die Eigenschaften von " [endviceinformation](/uwp/api/windows.devices.enumeration.deviceinformation) ", um die Ergebnisse zu filtern, oder stellen Sie eine Benutzeroberfläche für den Benutzer bereit.

In diesem Beispiel wird der oben definierte Selektor verwendet, um mithilfe von **findallasync** eine Momentaufnahme von Geräten zu erstellen und dann die einzelnen von der Auflistung zurückgegebenen Elemente aufzulisten und den Gerätenamen und die ID in die Debugausgabe zu schreiben. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> Beim Arbeiten mit den [Windows. Devices. Enumeration](/uwp/api/Windows.Devices.Enumeration) -APIs müssen Sie häufig [DeviceInformation](/uwp/api/windows.devices.enumeration.deviceinformation) -Objekte verwenden, um Informationen zu einem bestimmten Gerät zu erhalten. Beispielsweise kann die [DeviceInformation.ID](/uwp/api/windows.devices.enumeration.deviceinformation.id) -Eigenschaft verwendet werden, um das gleiche Gerät wiederherzustellen und wiederzuverwenden, wenn es in einer zukünftigen Sitzung verfügbar ist und die [DeviceInformation.Name](/uwp/api/windows.devices.enumeration.deviceinformation.name) -Eigenschaft zu Anzeige Zwecken in der APP verwendet werden kann.  Weitere Informationen zu den verfügbaren zusätzlichen Eigenschaften finden Sie auf der Referenzseite zu " [DeviceInformation](/uwp/api/windows.devices.enumeration.deviceinformation) ".

## <a name="method-4-enumerate-and-watch"></a>Methode 4: aufzählen und überwachen

Eine leistungsfähigere und flexiblere Methode für die Enumeration von Geräten ist die Erstellung von [DeviceWatcher](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).  Ein Geräte-Watcher listet Geräte dynamisch auf, sodass die Anwendung Benachrichtigungen empfängt, wenn Geräte hinzugefügt, entfernt oder geändert werden, nachdem die anfängliche Enumeration fertiggestellt wurde.  Mit einem **devicewatcher** können Sie erkennen, ob ein mit dem Netzwerk verbundenes Gerät online geschaltet wird, ob ein Bluetooth-Gerät in Reichweite ist, und ob ein lokal verbundenes Gerät getrennt ist, sodass Sie die entsprechenden Maßnahmen in Ihrer Anwendung ausführen können.

In diesem Beispiel wird der oben definierte Selektor verwendet, um einen **devicewatcher** zu erstellen und Ereignishandler für die [hinzugefügten](/uwp/api/windows.devices.enumeration.devicewatcher.added), [entfernten](/uwp/api/windows.devices.enumeration.devicewatcher.removed)und [aktualisierten](/uwp/api/windows.devices.enumeration.devicewatcher.updated) Benachrichtigungen zu definieren. Sie müssen die Details der Aktionen ausfüllen, die Sie bei jeder Benachrichtigung durchführen möchten.

```Csharp
using Windows.Devices.Enumeration;

DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Removed += DeviceWatcher_Removed;
deviceWatcher.Updated += DeviceWatcher_Updated;

void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    // TODO: Add the DeviceInformation object to your collection
}

void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Remove the item in your collection associated with DeviceInformationUpdate
}

void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Update your collection with information from DeviceInformationUpdate
}
```

> [!TIP]
> Weitere Informationen zur Verwendung von **devicewatcher**finden Sie unter Auflisten [und Überwachen von Geräten]( ./enumerate-devices.md#enumerate-and-watch-devices) .

## <a name="see-also"></a>Weitere Informationen
* [Einführung in den Point of Service](pos-basics.md)
* [DeviceInformation-Klasse](/uwp/api/windows.devices.enumeration.deviceinformation)
* [Posprinter-Klasse](/uwp/api/windows.devices.pointofservice.posprinter)
* [Posconnectiontypes-Enumeration](/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [Barcodescanner-Klasse](/uwp/api/windows.devices.pointofservice.barcodescanner)
* [Devicewatcher-Klasse](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]