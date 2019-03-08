---
title: Auflisten von PointOfService-Geräten
description: Hier erfahren Sie, wie Sie PointOfService Geräte auflisten.
ms.date: 10/08/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 27d25864941b9d73c9b12e6329eab79fac1b15bf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620905"
---
# <a name="enumerating-point-of-service-devices"></a>Auflisten von Point of Service-Geräten
In diesem Abschnitt erfahren Sie, wie Sie [definieren eine Geräteauswahl](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) wird, Fragen Sie Geräte, die für das System verfügbar, und verwenden diese Auswahl zum Aufzählen von Point-of-Service-Geräte, die mit einer der folgenden Methoden:

**Methode 1:** [Verwenden Sie eine Geräte-Auswahl](#method-1-use-a-device-picker)
<br/>
Zeigen Sie eine Auswahl-UI für das Gerät und des Benutzers ein verbundenes Gerät wählen. Diese Methode behandelt, die Liste zu aktualisieren, wenn Geräte verbunden und entfernt werden, und es ist einfacher und sicherer als die anderen Methoden.

**Methode 2:** [Das erste verfügbare Gerät abrufen](#method-2-get-first-available-device)<br />Verwendung [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) auf das erste verfügbare Gerät in einer bestimmten Point-of-Service-Device-Klasse.

**Methode 3:** [Momentaufnahme von Geräten](#method-3-snapshot-of-devices)<br />Zählt eine Momentaufnahme der Point-of-Service-Geräte, die auf dem System zu einem bestimmten Zeitpunkt rechtzeitig vorhanden sind. Dies ist nützlich, wenn Sie eine eigene Benutzeroberfläche erstellen möchten oder Geräte auflisten müssen, ohne dem Benutzer eine Benutzeroberfläche anzuzeigen. [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) enthalten die wieder Ergebnisse bis zum Abschluss der gesamten Enumeration.

**Methode 4:** [Auflisten und überwachen](#method-4-enumerate-and-watch)<br />[DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) ist ein leistungsfähiger und flexibler Enumeration-Modell, mit dem Sie zum Auflisten von Geräten, bei denen derzeit vorhanden sind und auch Benachrichtigungen zu empfangen, wenn Geräte hinzugefügt oder aus dem System entfernt werden.  Dies ist hilfreich, wenn Sie eine aktuelle Liste von Geräten im Hintergrund zur Anzeige auf Ihrer Benutzeroberfläche verwalten möchten, statt darauf zu warten, bis eine Momentaufnahme erstellt wird.

## <a name="define-a-device-selector"></a>Definieren einer Geräteauswahl
Mit einer Geräteauswahl können Sie die Geräte begrenzen, die Sie beim Auflisten von Geräten durchsuchen.  Dadurch können Sie nur auf relevante Ergebnisse zu erhalten und reduzieren die Zeit, die benötigt wird, um die gewünschten Geräte aufgezählt wurden.

Sie können die **GetDeviceSelector** Methode für den Typ des Geräts, das Sie suchen, um der Geräteauswahl für diesen Typ zu erhalten. Verwenden Sie beispielsweise [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) erhalten Sie eine Auswahl zum Aufzählen aller [PosPrinters](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter) an das System, einschließlich USB-, Netzwerk- und Bluetooth-POS-Drucker angefügt.

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

Die **GetDeviceSelector** Methoden für die verschiedenen Gerätetypen sind:

* [BarcodeScanner.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

Mit einer **GetDeviceSelector** Methode, eine [PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) Wert als Parameter verwendet, Sie können einschränken, Ihre Auswahl zum Auflisten von lokalen Netzwerk oder Bluetooth-attached POS-Geräten, verringert die Zeitaufwand für die Abfrage ausführen.  Das folgende Beispiel zeigt, dass eine Verwendung dieser Methode, um eine Auswahl zu definieren, die nur lokal unterstützt POS-Drucker angefügt.

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> Finden Sie unter [erstellen Sie eine Geräteauswahl](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector) zum Erstellen von mehr erweiterte Auswahl Zeichenfolgen.

## <a name="method-1-use-a-device-picker"></a>Methode 1: Verwenden Sie eine Geräte-Auswahl

Die [DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker) Klasse können Sie ein Datumsauswahl-Flyout anzeigen, die eine Liste der Geräte für den Benutzer zur Auswahl enthält. Sie können die [Filter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter) Eigenschaft, auf welche Arten von Geräten in der Auswahl angezeigt. Diese Eigenschaft ist vom Typ [DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter). Sie können die Gerätetypen hinzufügen, mit dem Filter die [SupportedDeviceClasses](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) oder [SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) Eigenschaft.

Wenn Sie die Auswahl der Geräte anzeigen möchten, können Sie rufen die [PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) -Methode, die die Auswahl-UI angezeigt wird, und das ausgewählte Gerät zurück. Sie müssen an einer [Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect) , die bestimmen, wo das Flyout angezeigt wird. Diese Methode zurück eine [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) -Objekt um es mit den Punkt des Service-APIs verwenden, müssen Sie verwenden die **FromIdAsync** -Methode für die bestimmte Geräteklassen, die Sie möchten. Sie übergeben die [DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) -Eigenschaft, wie der Methode *"DeviceID"* Parameter auf, und rufen Sie eine Instanz der Geräteklasse als Rückgabewert.

Der folgende Codeausschnitt erstellt einen **DevicePicker**, einen Barcode-Scanner-Filter hinzugefügt, hat der Benutzer ein Gerät auszuwählen, und erstellt dann eine **BarcodeScanner** -Objekt auf Grundlage der Geräte-ID:

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

## <a name="method-2-get-first-available-device"></a>Methode 2: Das erste verfügbare Gerät abrufen

Die einfachste Möglichkeit, eine Point-of-Service-Gerät zu versetzen, ist die Verwendung **GetDefaultAsync** um das erste verfügbare Gerät in einer Point-of-Service-Device-Klasse zu erhalten. 

Das folgende Beispiel veranschaulicht die Verwendung der [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) für [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner). Das Schreiben von Code Muster ist ähnlich für alle Klassen von Point-of-Service-Gerät.

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync** muss mit Vorsicht verwendet werden, wie sie ein anderes Gerät aus einer Sitzung zur nächsten zurückgeben kann. Viele Ereignisse können diese Auflistung beeinflussen und zu einem anderen ersten verfügbaren Gerät führen, darunter: 
> - Änderung bei den an Ihren Computer angeschlossenen Kameras 
> - Point-Dienst Geräte an den Computer angeschlossenen ändern
> - Ändern Sie in den NAS-Geräten in Ihrem Netzwerk verfügbar
> - Ändern Sie im Point-of-Service für Bluetooth-Geräte in Reichweite Ihres Computers 
> - Änderungen an der Point-of-Service-Konfiguration 
> - Installation von Treibern oder OPOS-Serviceobjekten
> - Installation von Point-of-Service-Erweiterungen
> - Updates am Windows-Betriebssystem

## <a name="method-3-snapshot-of-devices"></a>Methode 3: Momentaufnahme von Geräten

In einigen Szenarien möchten Sie vielleicht eine eigene Benutzeroberfläche erstellen oder müssen Geräte auflisten, ohne dem Benutzer eine Benutzeroberfläche anzuzeigen.  In diesen Fällen können Sie eine Momentaufnahme der Geräte, die derzeit verbundene oder kombiniert mit dem System mit aufzählen [DeviceInformation.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync).  Diese Methode hält alle Ergebnisse zurück, bis die gesamte Auflistung abgeschlossen ist.

> [!TIP]
> Es wird empfohlen, verwenden Sie die **GetDeviceSelector** -Methode mit dem **PosConnectionTypes** Parameter bei Verwendung **FindAllAsync** beschränken Sie Ihre Abfrage in den Verbindungstyp die gewünschte.  Netzwerk- und Bluetooth-Verbindungen können die Ergebnisse verzögern, wie ihre Enumerationen abgeschlossen werden müssen, bevor Sie **FindAllAsync** Ergebnisse zurückgegeben werden.

> [!CAUTION] 
> **FindAllAsync** gibt ein Array von Geräten.  Da sich die Reihenfolge dieses Arrays von Sitzung zu Sitzung ändern kann, wird nicht empfohlen, sich durch Verwendung eines hartcodierten Index für das Array auf eine bestimmte Reihenfolge zu verlassen.  Verwendung [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) Eigenschaften, um die Ergebnisse zu filtern, oder stellen eine Benutzeroberfläche für den Benutzer zur Auswahl.

In diesem Beispiel wird die Auswahl einer Momentaufnahme von Geräten mit der oben definierten **FindAllAsync** anschließend durchläuft alle Elemente der Auflistung zurückgegeben und der Gerätename und die ID in die Debugausgabe geschrieben. 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> Bei der Arbeit mit der [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) -APIs, müssen Sie häufig verwenden [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) Objekte zum Abrufen von Informationen zu einem bestimmten Gerät. Z. B. die [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) Eigenschaft kann verwendet werden, wiederherstellen und das gleiche Gerät wiederverwenden, wenn sie in einer zukünftigen Sitzung verfügbar ist und die [DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) Eigenschaft kann verwendet werden, für die Anzeige in Ihrer app zu.  Finden Sie unter den [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) Referenzseite für das Informationen zu zusätzlichen Eigenschaften zur Verfügung.

## <a name="method-4-enumerate-and-watch"></a>Methode 4: Auflisten und überwachen

Eine Methode leistungsfähige und flexible aufzeählen von Geräten ist das Erstellen einer [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher).  Ein Geräteüberwachungselement listet Geräte dynamisch auf, sodass die Anwendung Benachrichtigungen erhält, wenn Geräte hinzugefügt, entfernt oder geändert werden, nachdem die ursprüngliche Aufzählung abgeschlossen ist.  Ein **DeviceWatcher** können Sie erkennen, wenn ein Netzwerk verbundenen Gerät online geschaltet wird, ein Bluetooth-Gerät befindet sich im Bereich auch so, als ob ein lokal angeschlossenes Gerät getrennt wird, sodass Sie in die entsprechenden Maßnahmen ergreifen können Ihre die Anwendung.

In diesem Beispiel wird die Auswahl, die oben definierte, zum Erstellen einer **DeviceWatcher** ebenfalls definiert als Ereignishandler für die [Added](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added), [entfernt](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed), und [aktualisiert ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) Benachrichtigungen. Sie müssen die Details zu den Aktionen ausfüllen, die bei jeder Benachrichtigung durchgeführt werden sollen.

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
> Finden Sie unter [Enumerate "und" Überwachen von Geräten]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices) für Weitere Informationen zur Verwendung von einem **DeviceWatcher**.

## <a name="see-also"></a>Siehe auch
* [Erste Schritte mit Point-of-Service](pos-basics.md)
* [DeviceInformation-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [PosPrinter-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [PosConnectionTypes-Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [BarcodeScanner-Klasse](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [DeviceWatcher-Klasse](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]