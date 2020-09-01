---
ms.assetid: D06AA3F5-CED6-446E-94E8-713D98B13CAA
title: Erstellen einer Geräteauswahl
description: Durch das Erstellen einer Geräteauswahl können Sie die Geräte begrenzen, die Sie beim Auflisten von Geräten durchsuchen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 057b6d3087ae08cab6704dd83ebf1bf93b933a21
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159694"
---
# <a name="build-a-device-selector"></a>Erstellen einer Geräteauswahl



**Wichtige APIs**

- [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)

Durch das Erstellen einer Geräteauswahl können Sie die Geräte begrenzen, die Sie beim Auflisten von Geräten durchsuchen. Auf diese Weise erhalten Sie lediglich die relevanten Ergebnisse, und die Leistung des Systems wird optimiert. In den meisten Szenarien erhalten Sie eine Geräteauswahl von einem Gerätestapel. Beispielsweise können Sie [**GetDeviceSelector**](/uwp/api/windows.devices.usb.usbdevice.getdeviceselector) für Geräte verwenden, die über USB erkannt werden. Diese Geräteauswahl gibt eine AQS-Zeichenfolge (Advanced Query Syntax) zurück. Wenn Sie mit dem AQS-Format nicht vertraut sind, erhalten Sie weitere Informationen unter [Programmgesteuerte Verwendung der erweiterten Abfragesyntax](/windows/desktop/search/-search-3x-advancedquerysyntax).

## <a name="building-the-filter-string"></a>Erstellen der Filterzeichenfolge

Es gibt Situationen, in denen Sie Geräte auflisten müssen, eine angegebene Geräteauswahl für Ihr Szenario aber nicht verfügbar ist. Eine Geräteauswahl ist eine AQS-Filterzeichenfolge, die die folgenden Informationen enthält. Vor dem Erstellen einer Filterzeichenfolge müssen Sie einige wichtige Informationen zu den Geräten kennen, die Sie aufzählen möchten.

-   Die [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung der betreffenden Geräte. Weitere Informationen dazu, wie sich **DeviceInformationKind** auf die Aufzählung von Geräten auswirkt, finden Sie unter [Auflisten von Geräten](enumerate-devices.md).
-   Vorgehensweise bei der Erstellung einer AQS-Filterzeichenfolge (in diesem Thema beschrieben)
-   Für Sie interessante Eigenschaften Die verfügbaren Eigenschaften richten sich nach der [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung. Weitere Informationen finden Sie unter [Geräteinformationseigenschaften](device-information-properties.md).
-   Von Ihnen für die Abfrage verwendete Protokolle Dies ist nur erforderlich, wenn Sie über ein verkabeltes oder Drahtlosnetzwerk nach Geräten suchen. Weitere Informationen hierzu finden Sie unter [Auflisten von Geräten über ein Netzwerk](enumerate-devices-over-a-network.md).

Bei Verwendung der [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)-APIs kombinieren Sie die Geräteauswahl häufig mit der für Sie relevanten Geräteart. Die Liste der verfügbaren Gerätearten wird durch die [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung definiert. Diese Kombination von Faktoren hilft Ihnen, die verfügbaren Geräte auf diejenigen Geräte zu beschränken, die für Sie relevant sind. Wenn Sie die **DeviceInformationKind**-Aufzählung nicht angeben oder die verwendete Methode keinen **DeviceInformationKind**-Parameter bereitstellt, wird als Standard **DeviceInterface** verwendet.

Die [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)-APIs enthalten eine Canonical AQS-Syntax, die jedoch nicht alle Operatoren unterstützt. Eine Liste der beim Erstellen der Filterzeichenfolge verfügbaren Eigenschaften finden Sie unter [Geräteinformationseigenschaften](device-information-properties.md).

**Vorsicht**    Benutzerdefinierte Eigenschaften, die mithilfe des-Formats definiert werden, `{GUID} PID` können beim Erstellen der AQS-Filter Zeichenfolge nicht verwendet werden. Dies liegt daran, dass der Eigenschaftstyp vom bekannten Eigenschaftennamen abgeleitet wird.

 

Die folgende Tabelle enthält die AQS-Operatoren mit den von ihnen unterstützten Parametertypen.

| Operator                       | Unterstützte Typen                                                             |
|--------------------------------|-----------------------------------------------------------------------------|
| **COP \_ gleich**                 | String, Boolean, GUID, UInt16, UInt32                                       |
| **COP- \_ NotEqual**              | String, Boolean, GUID, UInt16, UInt32                                       |
| **COP \_ LessThan**              | UInt16, UInt32                                                              |
| **COP \_ GreaterThan**           | UInt16, UInt32                                                              |
| **COP \_ LessThanOrEqual**       | UInt16, UInt32                                                              |
| **COP \_ GreaterThanOrEqual**    | UInt16, UInt32                                                              |
| **COP- \_ Wert \_ enthält**       | String, String Array, Boolean Array, GUID Array, UInt16 Array, UInt32 Array |
| **COP- \_ Wert \_ Notare**    | String, String Array, Boolean Array, GUID Array, UInt16 Array, UInt32 Array |
| **COP- \_ Wert \_ Startwert**     | String                                                                      |
| **COP- \_ Wert \_ EndsWith**       | String                                                                      |
| **COP \_ doswildcards**          | Nicht unterstützt                                                               |
| **COP- \_ Wort \_ gleich**           | Nicht unterstützt                                                               |
| **COP \_ Word \_ Start with**      | Nicht unterstützt                                                               |
| **\_Anwendungs \_ spezifisch für Cop** | Nicht unterstützt                                                               |


> **Tipp**    Sie können **null** für **Cop \_ ** -oder **Cop- \_ NotEqual**angeben. Dies führt zu einer Eigenschaft ohne Wert oder ohne vorhandenen Wert. In AQS geben Sie mit leeren Klammern **null** an \[ \] .

> **Wichtig**    Wenn der **Cop- \_ Wert \_ enthält** und der **Cop- \_ Wert \_ Notare** -Operatoren verwendet, Verhalten Sie sich mit Zeichen folgen und Zeichen folgen Arrays anders. Im Falle einer Zeichenfolge führt das System eine Suche ohne Berücksichtigung der Groß-/Kleinschreibung durch, um festzustellen, ob das Gerät die angegebene Zeichenfolge als Teilzeichenfolge enthält. Im Falle eines Zeichenfolgenarrays werden Teilzeichenfolgen nicht gesucht. Mit dem Zeichenfolgenarray wird das Array durchsucht, um festzustellen, ob es die gesamte angegebene Zeichenfolge enthält. Es ist nicht möglich, ein Zeichenfolgenarray zu durchsuchen, um festzustellen, ob die Elemente im Array eine Teilzeichenfolge enthalten.

Wenn keine einzelne AQS-Filterzeichenfolge erstellt werden kann, die den richtigen Ergebnisbereich herausfiltert, können Sie Ihre Ergebnisse nach Erhalt filtern. In diesem Fall wird jedoch empfohlen, die Ergebnisse der anfänglichen AQS-Filterzeichenfolge so weit wie möglich einzuschränken, wenn Sie sie für [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)-APIs bereitstellen. Dadurch wird die Leistung der Anwendung verbessert.

## <a name="aqs-string-examples"></a>Beispiele für AQS-Zeichenfolgen

Die folgenden Beispiele veranschaulichen, wie die AQS-Syntax verwendet werden kann, um die aufzulistenden Geräte einzuschränken. Alle diese Filterzeichenfolgen werden mit einer [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) kombiniert, um einen vollständigen Filter zu erstellen. Wenn keine Art angegeben ist, wird **DeviceInterface** als Standardart verwendet.

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung vom Typ **DeviceInterface** kombiniert wird, listet er alle Objekte auf, die die Schnittstellenklasse für die Audioaufzeichnung enthalten und derzeit aktiviert sind. **=** übersetzt in **Cop ist \_ Gleichheits**.

``` syntax
System.Devices.InterfaceClassGuid:="{2eef81be-33fa-4800-9670-1cd474972c3f}" AND
System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung vom Typ **Device** kombiniert wird, listet er alle Objekte auf, die mindestens eine Hardware-ID von „GenCdRom“ aufweisen. **~~** übersetzt in den **Cop- \_ Wert \_ enthält**.

``` syntax
System.Devices.HardwareIds:~~"GenCdRom"
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung vom Typ **DeviceContainer** kombiniert wird, listet er alle Objekte auf, die einen Modellnamen aufweisen, der die Teilzeichenfolge „Microsoft“ enthält. **~~** übersetzt in den **Cop- \_ Wert \_ enthält**.

``` syntax
System.Devices.ModelName:~~"Microsoft"
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung vom Typ **DeviceInterface** kombiniert wird, listet er alle Objekte auf, deren Name mit der Teilzeichenfolge „Microsoft“ beginnt. **~&lt;** wird in **Cop \_ Start**übersetzt.

``` syntax
System.ItemNameDisplay:~<"Microsoft"
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung vom Typ **Device** kombiniert wird, listet er alle Objekte auf, für die eine **System.Devices.IpAddress**-Eigenschaft festgelegt wurde. **&lt;&gt;\[\]** übersetzt in **Cop- \_ NotEquals** in Kombination mit einem **null** -Wert.

``` syntax
System.Devices.IpAddress:<>[]
```

Wenn dieser Filter zusammen mit einer [**DeviceInformationKind**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind)-Aufzählung vom Typ **Device** kombiniert wird, listet er alle Objekte auf, für die keine **System.Devices.IpAddress**-Eigenschaft festgelegt wurde. **=\[\]** übersetzt in **Cop \_ gleich** , kombiniert mit einem **null** -Wert.

``` syntax
System.Devices.IpAddress:=[]
```

 

 