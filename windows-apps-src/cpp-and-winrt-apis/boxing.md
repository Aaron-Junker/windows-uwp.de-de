---
description: Ein Einzelwert muss in ein Referenzklassenobjekt gepackt werden, bevor er an eine Funktion übergeben wird, die **IInspectable** erwartet. Dieser Wrapping-Prozess wird als *Boxing* des Wertes bezeichnet.
title: Boxing und Unboxing von Einzelwerten für IInspectable mit C++/WinRT
ms.date: 04/10/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung, XAML, steuerelement, boxing, einzelwert
ms.localizationpriority: medium
ms.openlocfilehash: 5c86d1ac8ce83ea092ce0e2730ea0e9d4a201b94
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639675"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrt"></a>Boxing und Unboxing von Einzelwerten für IInspectable mit C++/WinRT
 
Die [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)-Schnittstellen ist die Root-Schnittstelle jeder Laufzeitklasse in Windows-Runtime (WinRT). Dies ist eine Idee, die analoge zu [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) am Stammknoten jeder COM-Schnittstelle und Klasse ist. **System.Object** ist der Stammknoten jeder [Common Type System](https://docs.microsoft.com/dotnet/standard/base-types/common-type-system)-Klasse.

Mit anderen Worten, eine Funktion, die **IInspectable** erwartet, kann eine Instanz einer beliebigen Laufzeitklasse übergeben werden. Sie können aber nicht direkt einen Einzelwert, wie z. B. einen Zahlen- oder Textwert, an eine solche Funktion übergeben. Stattdessen muss ein Einzelwert in ein Objekt der Referenzklasse gepackt werden. Dieser Wrapping-Prozess wird als *Boxing* des Wertes bezeichnet.

[C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) bietet die [ **winrt::box_value** ](/uwp/cpp-ref-for-winrt/box-value) -Funktion, die einen skalaren Wert akzeptiert, und gibt den Wert, der mittels Boxing konvertiert in eine **"iinspectable"**. Um ein **IInspectable** wieder in einen Einzelwert zu entpacken, gibt es die Funktionen [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) und [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or).

## <a name="examples-of-boxing-a-value"></a>Beispiele für das Boxen eines Wertes
Die Zugriffsfunktion [**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) gibt einen [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) zurück, der ein Einzelwert ist. Wir können diesen **hstring**-Wert einpacken und an eine Funktion übergeben, die so ein **IInspectable** erwartet.

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(winrt::xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

Um die Content-Eigenschaft eines XAML-[**Button**](/uwp/api/windows.ui.xaml.controls.button) festzulegen, rufen Sie die Zugriffsfunktion [**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?) auf. Um die Content-Eigenschaft auf einen String-Wert zu setzen, können Sie diesen Code verwenden.

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

Zuerst wandelt der [**hstring**](/uwp/cpp-ref-for-winrt/hstring)-Konvertierungskonstruktor das String-Literal in einen **hstring** um. Dann wird die Überladung von **winrt::box_value**, die einen **hstring** benötigt, aufgerufen.

## <a name="examples-of-unboxing-an-iinspectable"></a>Beispiele für das Unboxing eines IInspectable
In Ihren eigenen Funktionen (die ein **IInspectable** erwarten) können Sie [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) zum Unboxing verwenden. Sie können [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) zum Unboxing mit einem Standardwert verwenden.

```cppwinrt
void Unbox(winrt::Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="determine-the-type-of-a-boxed-value"></a>Ermitteln des Typs eines eingepackten Werts
Wenn Sie einen eingepackten Wert erhalten und nicht sicher sind, welchen Typ er enthält (Sie müssen den Typ wissen, um ihn zu entpacken), können Sie den eingepackten Wert nach seiner [**IPropertyValue**](/uwp/api/windows.foundation.ipropertyvalue)-Schnittstelle abfragen und dann **Type** dafür aufrufen. Dies ist ein Codebeispiel:

```cppwinrt
float pi = 3.14f;
auto piInspectable = winrt::box_value(pi);
auto piPropertyValue = piInspectable.as<winrt::Windows::Foundation::IPropertyValue>();
WINRT_ASSERT(piPropertyValue.Type() == winrt::Windows::Foundation::PropertyType::Single);
```

## <a name="important-apis"></a>Wichtige APIs
* [IInspectable-Schnittstelle](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Vorlage für WinRT::box_value-Funktion](/uwp/cpp-ref-for-winrt/box-value)
* [WinRT::hstring-Struktur](/uwp/cpp-ref-for-winrt/hstring)
* [Vorlage für WinRT::unbox_value-Funktion](/uwp/cpp-ref-for-winrt/unbox-value)
* [Vorlage für WinRT::unbox_value_or-Funktion](/uwp/cpp-ref-for-winrt/unbox-value-or)
