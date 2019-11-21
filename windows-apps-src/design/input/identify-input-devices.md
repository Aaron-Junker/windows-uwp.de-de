---
Description: Identifizieren Sie die Eingabegeräte, die mit einem Gerät für die universelle Windows-Plattform (UWP) verbunden sind, sowie deren Funktionen und Attribute.
title: Identifizieren von Eingabegeräten
ms.assetid: B2E93FBF-C508-44D9-BA46-ECFDAA8746F4
label: Identify input devices
template: detail.hbs
keywords: Gerät, Digitalisierer, Eingabe, Interaktion
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b2a17d1f4664326cb54d9c53d828eb372ef93fe4
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257885"
---
# <a name="identify-input-devices"></a>Identifizieren von Eingabegeräten


Identifizieren Sie die Eingabegeräte, die mit einem Gerät für die universelle Windows-Plattform (UWP) verbunden sind, sowie deren Funktionen und Attribute.

> **Wichtige APIs**: [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)

## <a name="retrieve-mouse-properties"></a>Abrufen von Mauseigenschaften


Der [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input)-Namespace enthält die [**MouseCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.MouseCapabilities)-Klasse, mit der Sie die Eigenschaften abrufen können, die von einer oder mehreren angeschlossenen Mäusen bereitgestellt werden. Erstellen Sie einfach ein neues **MouseCapabilities**-Objekt, und rufen Sie die benötigten Eigenschaften ab.

**Beachten Sie**  die Werte, die von den hier behandelten Eigenschaften zurückgegeben werden, auf allen erkannten Mäusen basieren: boolesche Eigenschaften geben einen Wert ungleich 0 (null) zurück, wenn mindestens eine Maus eine bestimmte Funktion unterstützt und numerische Eigenschaften den von einer beliebigen Maus verfügbar gemachten maximalen Wert zurückgeben.

 

Der folgende Code verwendet eine Reihe von [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Elementen, um die einzelnen Mauseigenschaften und -werte anzuzeigen.

```CSharp
private void GetMouseProperties()
{
    MouseCapabilities mouseCapabilities = new Windows.Devices.Input.MouseCapabilities();
    MousePresent.Text = mouseCapabilities.MousePresent != 0 ? "Yes" : "No";
    VertWheel.Text = mouseCapabilities.VerticalWheelPresent != 0 ? "Yes" : "No";
    HorzWheel.Text = mouseCapabilities.HorizontalWheelPresent != 0 ? "Yes" : "No";
    SwappedButtons.Text = mouseCapabilities.SwapButtons != 0 ? "Yes" : "No";
    NumButtons.Text = mouseCapabilities.NumberOfButtons.ToString();
}
```

## <a name="retrieve-keyboard-properties"></a>Abrufen von Tastatureigenschaften


Der [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input)-Namespace enthält die [**KeyboardCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.KeyboardCapabilities)-Klasse, mit der Sie ermitteln können, ob eine Tastatur angeschlossen ist. Erstellen Sie einfach ein neues **KeyboardCapabilities**-Objekt, und rufen Sie die [**KeyboardPresent**](https://docs.microsoft.com/uwp/api/windows.devices.input.keyboardcapabilities.keyboardpresent)-Eigenschaft ab.

Der folgende Code verwendet ein [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element, um die Tastatureigenschaft und ihren Wert anzuzeigen.

```CSharp
private void GetKeyboardProperties()
{
    KeyboardCapabilities keyboardCapabilities = new Windows.Devices.Input.KeyboardCapabilities();
    KeyboardPresent.Text = keyboardCapabilities.KeyboardPresent != 0 ? "Yes" : "No";
}
```

## <a name="retrieve-touch-properties"></a>Abrufen von Berührungseigenschaften


Der [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input)-Namespace enthält die [**TouchCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.TouchCapabilities)-Klasse, mit der Sie ermitteln können, ob Touchdigitalisierungsgeräte angeschlossen sind. Erstellen Sie einfach ein neues **TouchCapabilities**-Objekt, und rufen Sie die benötigten Eigenschaften ab.

**Beachten Sie**  die Werte, die von den hier behandelten Eigenschaften zurückgegeben werden, auf allen erkannten Fingerabdruck-Digitalisierern basieren: boolesche Eigenschaften geben einen Wert ungleich 0 (null) zurück, wenn mindestens ein Digitalisierer eine bestimmte Funktion unterstützt und numerische Eigenschaften den von einem einzelnen Digitalisierer verfügbar gemachten maximalen Wert

 

Der folgende Code verwendet eine Reihe von [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Elementen, um die Eigenschaften und Werte der einzelnen Touchdigitalisierer anzuzeigen.

```CSharp
private void GetTouchProperties()
{
    TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();
    TouchPresent.Text = touchCapabilities.TouchPresent != 0 ? "Yes" : "No";
    Contacts.Text = touchCapabilities.Contacts.ToString();
}
```

## <a name="retrieve-pointer-properties"></a>Abrufen von Zeigereigenschaften


Der [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input)-Namespace enthält die [**PointerDevice**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.PointerDevice)-Klasse, mit der Sie abrufen können, ob eines der erkannten Geräte Zeigereingaben (Toucheingabe, Stift oder Maus) unterstützt. Erstellen Sie einfach ein neues **PointerDevice**-Objekt, und rufen Sie die benötigten Eigenschaften ab.

**Beachten Sie**  die Werte, die von den hier behandelten Eigenschaften zurückgegeben werden, auf allen erkannten Zeiger Geräten basieren: boolesche Eigenschaften geben einen Wert ungleich 0 (null) zurück, wenn mindestens ein Gerät eine bestimmte Funktion unterstützt und numerische Eigenschaften den Maximalwert zurückgeben, der von einem Zeiger Gerät verfügbar gemacht wird.

Der folgende Code zeigt in einer Tabelle die Eigenschaften und Werte der einzelnen Zeigergeräte an.

```CSharp
private void GetPointerDevices()
{
    IReadOnlyList<PointerDevice> pointerDevices = Windows.Devices.Input.PointerDevice.GetPointerDevices();
    int gridRow = 0;
    int gridColumn = 0;

    for (int i = 0; i < pointerDevices.Count; i++)
    {
        // Pointer device type.
        TextBlock textBlock1 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock1);
        textBlock1.Text = (i + 1).ToString() + " Pointer Device Type:";
        Grid.SetRow(textBlock1, gridRow);
        Grid.SetColumn(textBlock1, gridColumn);

        TextBlock textBlock2 = new TextBlock();
        textBlock2.Text = pointerDevices[i].PointerDeviceType.ToString();
        Grid_PointerProps.Children.Add(textBlock2);
        Grid.SetRow(textBlock2, gridRow++);
        Grid.SetColumn(textBlock2, gridColumn + 1);

        // Is external?
        TextBlock textBlock3 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock3);
        textBlock3.Text = (i + 1).ToString() + " Is External?";
        Grid.SetRow(textBlock3, gridRow);
        Grid.SetColumn(textBlock3, gridColumn);

        TextBlock textBlock4 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock4);
        textBlock4.Text = pointerDevices[i].IsIntegrated.ToString();
        Grid.SetRow(textBlock4, gridRow++);
        Grid.SetColumn(textBlock4, gridColumn + 1);

        // Maximum contacts.
        TextBlock textBlock5 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock5);
        textBlock5.Text = (i + 1).ToString() + " Max Contacts:";
        Grid.SetRow(textBlock5, gridRow);
        Grid.SetColumn(textBlock5, gridColumn);

        TextBlock textBlock6 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock6);
        textBlock6.Text = pointerDevices[i].MaxContacts.ToString();
        Grid.SetRow(textBlock6, gridRow++);
        Grid.SetColumn(textBlock6, gridColumn + 1);

        // Physical device rectangle.
        TextBlock textBlock7 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock7);
        textBlock7.Text = (i + 1).ToString() + " Physical Device Rect:";
        Grid.SetRow(textBlock7, gridRow);
        Grid.SetColumn(textBlock7, gridColumn);

        TextBlock textBlock8 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock8);
        textBlock8.Text = pointerDevices[i].PhysicalDeviceRect.X.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Y.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Width.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Height.ToString();
        Grid.SetRow(textBlock8, gridRow++);
        Grid.SetColumn(textBlock8, gridColumn + 1);

        // Screen rectangle.
        TextBlock textBlock9 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock9);
        textBlock9.Text = (i + 1).ToString() + " Screen Rect:";
        Grid.SetRow(textBlock9, gridRow);
        Grid.SetColumn(textBlock9, gridColumn);

        TextBlock textBlock10 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock10);
        textBlock10.Text = pointerDevices[i].ScreenRect.X.ToString() + "," +
            pointerDevices[i].ScreenRect.Y.ToString() + "," +
            pointerDevices[i].ScreenRect.Width.ToString() + "," +
            pointerDevices[i].ScreenRect.Height.ToString();
        Grid.SetRow(textBlock10, gridRow++);
        Grid.SetColumn(textBlock10, gridColumn + 1);

        gridColumn += 2;
        gridRow = 0;
    }
```

## <a name="related-articles"></a>Verwandte Artikel


**Beispiele**
* [Beispiel für eine einfache Eingabe](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Eingabe Beispiel mit niedriger Latenz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)

**Archivbeispiele**
* [Eingabe: Beispiel für Gerätefunktionen](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
 

 




