---
title: Bedingtes XAML
description: Verwenden neuer APIs in XAML-Markup bei gleichzeitiger Gewährleistung der Kompatibilität mit früheren Versionen
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5d02c75775dfd63281dbf46c7f9fc58f48ac1e20
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359968"
---
# <a name="conditional-xaml"></a>Bedingtes XAML

*Bedingtes XAML* bietet eine Möglichkeit, die Methode [ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent) in XAML-Markup zu verwenden. Sie sind damit in der Lage, im Markup nur dann Eigenschaften festzulegen und Objekte zu initialisieren, wenn die entsprechende API vorhanden ist, ohne CodeBehind zu verwenden. Elemente oder Attribute werden selektiv analysiert, um zu bestimmen, ob sie zur Laufzeit zur Verfügung stehen. Bedingte Anweisungen werden zur Laufzeit ausgewertet, und Elemente, die mit einem bedingten XAML-Tag gekennzeichnet sind, werden analysiert, wenn ihre Auswertung **true** ergibt. Andernfalls werden sie ignoriert.

Bedingtes XAML ist ab dem Creators Update (Version 1703, Build 15063) verfügbar. Die Verwendung von bedingtem XAML setzt voraus, dass Sie in Ihrem Visual Studio-Projekt Build 15063 (Creators Update) oder höher als Mindestversion und eine höhere Version als Zielversion festlegen. Weitere Informationen zum Konfigurieren Ihres Visual Studio-Projekts finden Sie unter [Versionsadaptive Apps](version-adaptive-apps.md).

> [!NOTE]
> Sie müssen zum Erstellen einer versionsadaptiven App mit einer Mindestversion kleiner als Build 15063 [versionsadaptiven Code](version-adaptive-code.md) verwenden, nicht XAML.

Wichtige Hintergrundinformationen über ApiInformation und API-Verträge finden Sie unter [Versionsadaptive Apps](version-adaptive-apps.md).

## <a name="conditional-namespaces"></a>Bedingte Namespaces

Um eine bedingte Methode in XAML zu verwenden, müssen Sie zuerst einen bedingten [XAML-Namespace](../xaml-platform/xaml-namespaces-and-namespace-mapping.md) am Beginn der Seite deklarieren. Hier ein Pseudocodebeispiel für einen bedingten Namespace:

```xaml
xmlns:myNamespace="schema?conditionalMethod(parameter)"
```

Ein bedingter Namespace besteht aus zwei Teilen, die durch das Zeichen „?“ getrennt sind. 

- Der Inhalt vor dem Trennzeichen gibt den Namespace oder das Schema an, in dem die referenzierte API enthalten ist. 
- Der Inhalt nach dem Trennzeichen „?“' stellt die bedingte Methode dar, die bestimmt, ob die Auswertung für den bedingten Namespace **true** oder **false** ergibt.

In den meisten Fällen wird das Schema der standardmäßige XAML-Namespace sein:

```xaml
xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
```

Bedingtes XAML unterstützt die folgenden bedingten Methoden:

Methode | Umgekehrt
------ | -------
IsApiContractPresent(ContractName, VersionNumber) | IsApiContractNotPresent(ContractName, VersionNumber)
IsTypePresent(ControlType) | IsTypeNotPresent(ControlType)
IsPropertyPresent(ControlType, PropertyName) | IsPropertyNotPresent(ControlType, PropertyName)

Wir besprechen diese Methoden weiter unten in diesem Artikel.

> [!NOTE]
> Es wird empfohlen, dass Sie „IsApiContractPresent“ und „IsApiContractNotPresent“ verwenden. Andere Bedingungen werden in der Visual Studio-Entwurfserfahrung nicht vollständig unterstützt.

## <a name="create-a-namespace-and-set-a-property"></a>Erstellen eines Namespace und Festlegen einer Eigenschaft

In diesem Beispiel zeigen Sie „Hello, Conditional XAML“ als Inhalt eines Textblocks an, wenn die App mit dem Fall Creators Update oder später läuft, und standardmäßig keinen Inhalt, wenn sie unter einer früheren Version läuft.

Zuerst definieren Sie einen benutzerdefinierten Namespace mit dem Präfix „contract5Present“ und verwenden den standardmäßigen XAML-Namespace (https://schemas.microsoft.com/winfx/2006/xaml/presentation) als das Schema, das die Eigenschaft [TextBlock.Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.Text) enthält). Um daraus einen bedingten Namespace zu machen, fügen Sie das Trennzeichen „?“ hinter dem Schema hinzu.

Dann definieren Sie eine Bedingung, die **true** auf Geräten zurückgibt, die das Fall Creators Update ausführen. Sie können die ApiInformation-Methode **IsApiContractPresent** verwenden, um zu prüfen, ob die 5. Version von „UniversalApiContract“ vorliegt. Version 5 von „UniversalApiContract“ wurde mit dem Fall Creators Update (SDK 16299) veröffentlicht.

```xaml
xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

Nachdem der Namespace definiert ist, stellen Sie das Namespacepräfix der Text-Eigenschaft Ihrer TextBox voran, um sie als Eigenschaft auszuzeichnen, die bedingt zur Laufzeit festgelegt werden soll.

```xaml
<TextBlock contract5Present:Text="Hello, Conditional XAML"/>
```

Hier finden Sie den gesamten XAML.

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock contract5Present:Text="Hello, Conditional XAML"/>
    </Grid>
</Page>
```

Wenn Sie dieses Beispiel mit Fall Creators Update ausführen, wird der Text „Hello, Conditional XAML“ angezeigt; wenn Sie es mit Creators Update ausführen, wird kein Text angezeigt.

Mit bedingtem XAML können Sie die API-Prüfungen statt im Code in Ihrem Markup ausführen. Hier finden Sie den entsprechenden Code für diese Überprüfung.

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    textBlock.Text = "Hello, Conditional XAML";
}
```

Beachten Sie: Obwohl die Methode „IsApiContractPresent“ eine Zeichenfolge für den Parameter *contractName* erwartet, wird diese in der XAML-Namespace-Deklaration nicht in Anführungszeichen (" ") gesetzt.

## <a name="use-ifelse-conditions"></a>Verwenden von if/else-Bedingungen

Im vorherigen Beispiel wird die Eigenschaft „Text“ nur festgelegt, wenn die App unter Fall Creators Update ausgeführt wird. Wie ist aber vorzugehen, um einen alternativen Text anzuzeigen, wenn das Creators Update ausgeführt wird? Sie können versuchen, die „Text“-Eigenschaft wie folgt ohne einen bedingten Qualifizierer festzulegen.

```xaml
<!-- DO NOT USE -->
<TextBlock Text="Hello, World" contract5Present:Text="Hello, Conditional XAML"/>
```

Dies funktioniert, wenn sie mit Creators Update ausgeführt wird, aber wenn sie mit Fall Creators Update ausgeführt wird, erhalten Sie eine Fehlermeldung, dass die „Text“-Eigenschaft mehr als einmal festgelegt ist.

Um unterschiedliche Texte festzulegen für den Fall, dass die App unter verschiedenen Versionen von Windows 10 ausgeführt wird, benötigen Sie eine andere Bedingung. Bedingtes XAML bietet eine Umkehrung jeder unterstützten „ApiInformation“-Methode, damit Sie wie folgt if/else-Szenarien erstellen können.

Die Methode „IsApiContractPresent“ liefert **true**, wenn das aktuelle Gerät die angegebene Vertrags- und Versionsnummer enthält. Wenn Ihre App beispielsweise unter dem Creators-Update ausgeführt wird, hat der universelle API-Vertrag die Versionsnummer 4.

Verschiedene Aufrufe von „IsApiContractPresent“ würden diese Ergebnisse liefern:

- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 5) = **false**
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 4) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 3) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 2) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 1) = true.

„IsApiContractNotPresent“ liefert die Umkehrung von „IsApiContractPresent“. Aufrufe von „IsApiContractNotPresent“ würden folgende Ergebnisse liefern:

- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 5) = **true**
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 4) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 3) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 2) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 1) = false

Um die umgekehrte Bedingung zu verwenden, erstellen Sie einen zweiten bedingten XAML-Namespace, der die Bedingung **IsApiContractNotPresent** verwendet. Hier hat sie das Präfix „contract5NotPresent“.

```xaml
xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)"

xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

Sind beide Namespaces definiert, können Sie die Text-Eigenschaft doppelt festlegen, solange Sie Qualifizierer voranstellen, die gewährleisten, dass zur Laufzeit nur eine Einstellung der Eigenschaft verwendet wird. Beispiel:

```xaml
<TextBlock contract5NotPresent:Text="Hello, World"
           contract5Present:Text="Hello, Fall Creators Update"/>
```

Hier ist ein weiteres Beispiel, das die Hintergrundfarbe einer Schaltfläche festlegt. Die Funktion [Acryl-Material](../design/style/acrylic.md) ist ab dem Fall Creators Update verfügbar, sodass Sie Acryl für den Hintergrund verwenden, wenn die App unter Fall Creators Update läuft. Unter früheren Versionen ist Acryl nicht verfügbar; in diesen Fällen legen Sie die Hintergrundfarbe auf Rot fest.

```xaml
<Button Content="Button"
        contract5NotPresent:Background="Red"
        contract5Present:Background="{ThemeResource SystemControlAcrylicElementBrush}"/>
```

## <a name="create-controls-and-bind-properties"></a>Erstellen von Steuerelementen und Binden von Eigenschaften

Bisher haben Sie erfahren, wie Eigenschaften mithilfe von bedingtem XAML festgelegt werden. Sie können aber auch Steuerelemente bedingt instanziieren, je nach dem API-Vertrag, der zur Laufzeit verfügbar ist

Hier wird ein [ColorPicker](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker) instanziiert, wenn die App unter Fall Creators Update ausgeführt wird, in der das Steuerelement verfügbar ist. Der ColorPicker ist erst ab Fall Creators Update verfügbar. Wenn die App unter früheren Versionen ausgeführt wird, verwenden Sie eine [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox), um eine vereinfachte Farbauswahl für den Benutzer bereitzustellen.

```xaml
<contract5Present:ColorPicker x:Name="colorPicker"
                              Grid.Column="1"
                              VerticalAlignment="Center"/>

<contract5NotPresent:ComboBox x:Name="colorComboBox"
                              PlaceholderText="Pick a color"
                              Grid.Column="1"
                              VerticalAlignment="Center">
```

Sie können bedingte Qualifizierer mit unterschiedlichen Arten von [XAML-Syntax für Eigenschaften](../xaml-platform/xaml-syntax-guide.md) verwenden. Hier wird für Fall Creators Update die „Fill“-Eigenschaft des Rechtecks mit Syntax für Eigenschaftselemente festgelegt und für frühere Versionen mithilfe der Attributsyntax.

```xaml
<Rectangle x:Name="colorRectangle" Width="200" Height="200"
           contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
    <contract5Present:Rectangle.Fill>
        <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
    </contract5Present:Rectangle.Fill>
</Rectangle>
```

Wenn Sie eine Eigenschaft an eine anderen Eigenschaft binden, die von einem bedingten Namespace abhängig ist, müssen Sie dieselbe Bedingung für beide Eigenschaften verwenden. Hier hängt `colorPicker.Color` vom bedingten Namespace „contract5Present“ ab. Daher müssen Sie das Präfix „contract5Present“ auf die Eigenschaft „SolidColorBrush.Color“ festlegen. (Oder verwenden Sie das Präfix „contract5Present“ mit der Eigenschaft „SolidColorBrush“ statt mit der Color-Eigenschaft.) Andernfalls tritt während der Kompilierung ein Fehler auf.

```xaml
<SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
```

Hier folgt der vollständige XAML-Code, der diese Szenarien veranschaulicht. Dieses Beispiel enthält ein Rechteck und eine Benutzeroberfläche, mit der Sie die Farbe des Rechtecks festlegen können.

Wenn die App unter Fall Creators Update ausgeführt wird, verwenden Sie einen ColorPicker, um dem Benutzer die Farbauswahl zu ermöglichen. Der ColorPicker ist erst ab Fall Creators Update verfügbar. Wenn die App unter früheren Versionen ausgeführt wird, verwenden Sie eine ComboBox, um eine vereinfachte Farbauswahl für den Benutzer bereitzustellen.

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
    xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <Rectangle x:Name="colorRectangle" Width="200" Height="200"
                   contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
            <contract5Present:Rectangle.Fill>
                <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
            </contract5Present:Rectangle.Fill>
        </Rectangle>

        <contract5Present:ColorPicker x:Name="colorPicker"
                                      Grid.Column="1"
                                      VerticalAlignment="Center"/>

        <contract5NotPresent:ComboBox x:Name="colorComboBox"
                                      PlaceholderText="Pick a color"
                                      Grid.Column="1"
                                      VerticalAlignment="Center">
            <ComboBoxItem>Red
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Red"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Blue
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Blue"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Green
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Green"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
        </contract5NotPresent:ComboBox>
    </Grid>
</Page>
```

## <a name="related-articles"></a>Verwandte Artikel

- [Anleitung für UWP-Apps](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [Dynamisches Erkennen von Features mithilfe von API-Verträgen](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [API-Verträge](https://channel9.msdn.com/Events/Build/2015/3-733) (Video für Build 2015)