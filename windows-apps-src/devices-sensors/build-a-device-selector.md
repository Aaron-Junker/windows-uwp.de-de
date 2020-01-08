---
ms.assetid: D06AA3F5-CED6-446E-94E8-713D98B13CAA
title: Erstellen einer Geräteauswahl
description: Durch das Erstellen einer Geräteauswahl können Sie die Geräte begrenzen, die Sie beim Auflisten von Geräten durchsuchen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 67d83a66687bb8719dc374a2a8a3e30eaac82c71
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684833"
---
# <a name="build-a-device-selector"></a>Erstellen einer Geräteauswahl



**Wichtige APIs**

- [**Windows. Devices. Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)

Durch das Erstellen einer Geräteauswahl können Sie die Geräte begrenzen, die Sie beim Auflisten von Geräten durchsuchen. Auf diese Weise erhalten Sie lediglich die relevanten Ergebnisse, und die Leistung des Systems wird optimiert. In den meisten Szenarien erhalten Sie eine Geräteauswahl von einem Gerätestapel. Beispielsweise können Sie [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.usb.usbdevice.getdeviceselector) für Geräte verwenden, die über USB erkannt werden. Diese Geräteauswahl gibt eine AQS-Zeichenfolge (Advanced Query Syntax) zurück. Wenn Sie mit dem AQS-Format nicht vertraut sind, erhalten Sie weitere Informationen unter [Programmgesteuerte Verwendung der erweiterten Abfragesyntax](https://docs.microsoft.com/windows/desktop/search/-search-3x-advancedquerysyntax).

## <a name="building-the-filter-string"></a>Erstellen der Filterzeichenfolge

Es gibt Situationen, in denen Sie Geräte auflisten müssen, eine angegebene Geräteauswahl für Ihr Szenario aber nicht verfügbar ist. Eine Geräteauswahl ist eine AQS-Filterzeichenfolge, die die folgenden Informationen enthält. Vor dem Erstellen einer Filterzeichenfolge müssen Sie einige wichtige Informationen zu den Geräten kennen, die Sie aufzählen möchten.

-   Die [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung der betreffenden Geräte. Weitere Informationen dazu, wie sich **DeviceInformationKind** auf die Aufzählung von Geräten auswirkt, finden Sie unter [Auflisten von Geräten](enumerate-devices.md).
-   Vorgehensweise bei der Erstellung einer AQS-Filterzeichenfolge (in diesem Thema beschrieben)
-   Für Sie interessante Eigenschaften Die verfügbaren Eigenschaften richten sich nach der [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung. Weitere Informationen finden Sie unter [Geräteinformationseigenschaften](device-information-properties.md).
-   Von Ihnen für die Abfrage verwendete Protokolle Dies ist nur erforderlich, wenn Sie über ein verkabeltes oder Drahtlosnetzwerk nach Geräten suchen. Weitere Informationen hierzu finden Sie unter [Auflisten von Geräten über ein Netzwerk](enumerate-devices-over-a-network.md).

Bei Verwendung der [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)-APIs kombinieren Sie die Geräteauswahl häufig mit der für Sie relevanten Geräteart. Die Liste der verfügbaren Gerätearten wird durch die [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung definiert. Diese Kombination von Faktoren hilft Ihnen, die verfügbaren Geräte auf diejenigen Geräte zu beschränken, die für Sie relevant sind. Wenn Sie die **DeviceInformationKind**-Aufzählung nicht angeben oder die verwendete Methode keinen **DeviceInformationKind**-Parameter bereitstellt, wird als Standard **DeviceInterface** verwendet.

Die [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)-APIs enthalten eine Canonical AQS-Syntax, die jedoch nicht alle Operatoren unterstützt. Eine Liste der beim Erstellen der Filterzeichenfolge verfügbaren Eigenschaften finden Sie unter [Geräteinformationseigenschaften](device-information-properties.md).

**Vorsicht**  benutzerdefinierte Eigenschaften, die mit dem `{GUID} PID` Format definiert werden, können beim Erstellen der AQS-Filter Zeichenfolge nicht verwendet werden. Dies liegt daran, dass der Eigenschaftstyp vom bekannten Eigenschaftennamen abgeleitet wird.

 

Die folgende Tabelle enthält die AQS-Operatoren mit den von ihnen unterstützten Parametertypen.

| Operator                       | Unterstützte Typen                                                             |
|--------------------------------|-----------------------------------------------------------------------------|
| **COP-\_gleich**                 | String, Boolean, GUID, UInt16, UInt32                                       |
| **COP\_NotEqual**              | String, Boolean, GUID, UInt16, UInt32                                       |
| **COP\_LessThan**              | UInt16, UInt32                                                              |
| **COP\_GreaterThan**           | UInt16, UInt32                                                              |
| **COP\_LessThanOrEqual**       | UInt16, UInt32                                                              |
| **COP\_GreaterThanOrEqual**    | UInt16, UInt32                                                              |
| **\_Wert für Cop-\_enthält**       | String, String Array, Boolean Array, GUID Array, UInt16 Array, UInt32 Array |
| **COP-\_Wert\_Notare**    | String, String Array, Boolean Array, GUID Array, UInt16 Array, UInt32 Array |
| **COP\_Wert\_starttwith**     | Zeichenfolge                                                                      |
| **COP-\_Wert\_EndsWith**       | Zeichenfolge                                                                      |
| **COP-\_doswildcards**          | Nicht unterstützt.                                                               |
| **COP\_Wort\_gleich**           | Nicht unterstützt.                                                               |
| **COP\_Word\_startzwith**      | Nicht unterstützt.                                                               |
| **COP\_Anwendung\_spezifisch** | Nicht unterstützt.                                                               |


> **Tipp**  Sie können **null** für **Cop\_gleich** oder **Cop\_NotEqual**angeben. Dies führt zu einer Eigenschaft ohne Wert oder ohne vorhandenen Wert. In AQS geben Sie mithilfe leerer Klammern \[\]**null** an.

> **Wichtig**  bei der Verwendung des **Cop-\_Werts\_enthält und enthält** **\_Wert\_Notare** -Operatoren unterschiedlich verhalten sich mit Zeichen folgen und Zeichen folgen Arrays. Im Falle einer Zeichenfolge führt das System eine Suche ohne Berücksichtigung der Groß-/Kleinschreibung durch, um festzustellen, ob das Gerät die angegebene Zeichenfolge als Teilzeichenfolge enthält. Im Falle eines Zeichenfolgenarrays werden Teilzeichenfolgen nicht gesucht. Mit dem Zeichenfolgenarray wird das Array durchsucht, um festzustellen, ob es die gesamte angegebene Zeichenfolge enthält. Es ist nicht möglich, ein Zeichenfolgenarray zu durchsuchen, um festzustellen, ob die Elemente im Array eine Teilzeichenfolge enthalten.

Wenn keine einzelne AQS-Filterzeichenfolge erstellt werden kann, die den richtigen Ergebnisbereich herausfiltert, können Sie Ihre Ergebnisse nach Erhalt filtern. In diesem Fall wird jedoch empfohlen, die Ergebnisse der anfänglichen AQS-Filterzeichenfolge so weit wie möglich einzuschränken, wenn Sie sie für [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)-APIs bereitstellen. Dadurch wird die Leistung der Anwendung verbessert.

## <a name="aqs-string-examples"></a>Beispiele für AQS-Zeichenfolgen

Die folgenden Beispiele veranschaulichen, wie die AQS-Syntax verwendet werden kann, um die aufzulistenden Geräte einzuschränken. Alle diese Filterzeichenfolgen werden mit einer [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) kombiniert, um einen vollständigen Filter zu erstellen. Wenn keine Art angegeben ist, wird **DeviceInterface** als Standardart verwendet.

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung vom Typ **DeviceInterface** kombiniert wird, listet er alle Objekte auf, die die Schnittstellenklasse für die Audioaufzeichnung enthalten und derzeit aktiviert sind. **=** in **Cop\_entspricht**.

``` syntax
System.Devices.InterfaceClassGuid:="{2eef81be-33fa-4800-9670-1cd474972c3f}" AND
System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung vom Typ **Device** kombiniert wird, listet er alle Objekte auf, die mindestens eine Hardware-ID von „GenCdRom“ aufweisen. **~~** in den **Cop-\_Wert**übersetzt, der\_enthält.

``` syntax
System.Devices.HardwareIds:~~"GenCdRom"
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung vom Typ **DeviceContainer** kombiniert wird, listet er alle Objekte auf, die einen Modellnamen aufweisen, der die Teilzeichenfolge „Microsoft“ enthält. **~~** in den **Cop-\_Wert**übersetzt, der\_enthält.

``` syntax
System.Devices.ModelName:~~"Microsoft"
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung vom Typ **DeviceInterface** kombiniert wird, listet er alle Objekte auf, deren Name mit der Teilzeichenfolge „Microsoft“ beginnt. **~&lt;** in **Cop\_Start mit**übersetzt.

``` syntax
System.ItemNameDisplay:~<"Microsoft"
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung vom Typ **Device** kombiniert wird, listet er alle Objekte auf, für die eine **System.Devices.IpAddress**-Eigenschaft festgelegt wurde. **&lt;&gt;\[\]** mit einem **null** -Wert in **Cop\_NotEquals** übersetzt.

``` syntax
System.Devices.IpAddress:<>[]
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung vom Typ **Device** kombiniert wird, listet er alle Objekte auf, für die keine **System.Devices.IpAddress**-Eigenschaft festgelegt wurde. **=\[\]** in **Cop\_ist gleich** kombiniert mit einem **null** -Wert.

``` syntax
System.Devices.IpAddress:=[]
```

 

 
