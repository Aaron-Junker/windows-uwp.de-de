---
description: Identifizieren Sie die Eingabegeräte, die mit einem Windows App-Gerät verbunden sind, und identifizieren Sie Ihre Funktionen und Attribute.
title: Identifizieren von Eingabegeräten
ms.assetid: B2E93FBF-C508-44D9-BA46-ECFDAA8746F4
label: Identify input devices
template: detail.hbs
keywords: Gerät, Digitalisierer, Eingabe, Interaktion
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 51d5ea7bf6776c2c728fd000ffea63b79a4d7464
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030423"
---
# <a name="identify-input-devices"></a>Identifizieren von Eingabegeräten


Identifizieren Sie die Eingabegeräte, die mit einem Windows App-Gerät verbunden sind, und identifizieren Sie Ihre Funktionen und Attribute.

> **Wichtige APIs** : [**Windows. Devices. Input**](/uwp/api/Windows.Devices.Input), [**Windows. UI. Input**](/uwp/api/Windows.UI.Core), [**Windows. UI. XAML. Input**](/uwp/api/Windows.UI.Input)

## <a name="retrieve-mouse-properties"></a>Abrufen von Mauseigenschaften


Der [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input)-Namespace enthält die [**MouseCapabilities**](/uwp/api/Windows.Devices.Input.MouseCapabilities)-Klasse, mit der Sie die Eigenschaften abrufen können, die von einer oder mehreren angeschlossenen Mäusen bereitgestellt werden. Erstellen Sie einfach ein neues **MouseCapabilities** -Objekt, und rufen Sie die benötigten Eigenschaften ab.

**Hinweis** Die von den hier beschriebenen Eigenschaften zurückgegebenen Werte basieren auf allen ermittelten Mäusen: Boolesche Eigenschaften geben Werte ungleich 0 zurück, wenn mindestens eine Maus eine bestimmte Funktion unterstützt, während numerische Eigenschaften den größten Wert einer der Mäuse zurückgeben.

 

Der folgende Code verwendet eine Reihe von [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Elementen, um die einzelnen Mauseigenschaften und -werte anzuzeigen.

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


Der [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input)-Namespace enthält die [**KeyboardCapabilities**](/uwp/api/Windows.Devices.Input.KeyboardCapabilities)-Klasse, mit der Sie ermitteln können, ob eine Tastatur angeschlossen ist. Erstellen Sie einfach ein neues **KeyboardCapabilities** -Objekt, und rufen Sie die [**KeyboardPresent**](/uwp/api/windows.devices.input.keyboardcapabilities.keyboardpresent)-Eigenschaft ab.

Der folgende Code verwendet ein [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element, um die Tastatureigenschaft und ihren Wert anzuzeigen.

```CSharp
private void GetKeyboardProperties()
{
    KeyboardCapabilities keyboardCapabilities = new Windows.Devices.Input.KeyboardCapabilities();
    KeyboardPresent.Text = keyboardCapabilities.KeyboardPresent != 0 ? "Yes" : "No";
}
```

## <a name="retrieve-touch-properties"></a>Abrufen von Berührungseigenschaften


Der [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input)-Namespace enthält die [**TouchCapabilities**](/uwp/api/Windows.Devices.Input.TouchCapabilities)-Klasse, mit der Sie ermitteln können, ob Touchdigitalisierungsgeräte angeschlossen sind. Erstellen Sie einfach ein neues **TouchCapabilities** -Objekt, und rufen Sie die benötigten Eigenschaften ab.

**Hinweis** Die von den hier beschriebenen Eigenschaften zurückgegebenen Werte basieren auf allen ermittelten Touchdigitalisierungsgeräten: Boolesche Eigenschaften geben Werte ungleich 0 zurück, wenn mindestens eine Maus eine bestimmte Funktion unterstützt, während numerische Eigenschaften den größten Wert einer der Mäuse zurückgeben.

 

Der folgende Code verwendet eine Reihe von [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Elementen, um die Eigenschaften und Werte der einzelnen Touchdigitalisierer anzuzeigen.

```CSharp
private void GetTouchProperties()
{
    TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();
    TouchPresent.Text = touchCapabilities.TouchPresent != 0 ? "Yes" : "No";
    Contacts.Text = touchCapabilities.Contacts.ToString();
}
```

## <a name="retrieve-pointer-properties"></a>Abrufen von Zeigereigenschaften


Der [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input)-Namespace enthält die [**PointerDevice**](/uwp/api/Windows.Devices.Input.PointerDevice)-Klasse, mit der Sie abrufen können, ob eines der erkannten Geräte Zeigereingaben (Toucheingabe, Stift oder Maus) unterstützt. Erstellen Sie einfach ein neues **PointerDevice** -Objekt, und rufen Sie die benötigten Eigenschaften ab.

**Hinweis**  Die von den hier beschriebenen Eigenschaften zurückgegebenen Werte basieren auf allen ermittelten Zeigegeräten: Boolesche Eigenschaften geben Werte ungleich 0 zurück, wenn mindestens ein Zeigegerät eine bestimmte Funktion unterstützt, während numerische Eigenschaften den größten Wert eines der Zeigegeräte zurückgeben.

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

### <a name="samples"></a>Beispiele

- [Einfaches Eingabebeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Eingabebeispiel mit geringer Latenz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)

### <a name="archive-samples"></a>Archivbeispiele

- [Eingabe: Beispiel für Gerätefunktionen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
