---
description: Die Binding-Markuperweiterung wird beim Laden von XAML in eine Instanz der Binding-Klasse konvertiert.
title: Binding-Markuperweiterung
ms.assetid: 3BAFE7B5-AF33-487F-9AD5-BEAFD65D04C3
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b197ea668ec73711b7a9c63e516b4ec9a5f54d62
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618505"
---
# <a name="binding-markup-extension"></a>{Binding}-Markuperweiterung


**Beachten Sie**  ein neuer Bindungsmechanismus ist für Windows 10, die für die Leistung und Produktivität optimiert ist. Weitere Informationen finden Sie unter [{x:Bind}-Markuperweiterung](x-bind-markup-extension.md)

**Beachten Sie**  allgemeine Informationen zum Verwenden der Datenbindung in Ihrer app mit **{Binding}** (und eine allumfassende Vergleich zwischen **{X: Bind}** und **{Binding}**), finden Sie unter [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946).

Die **{Binding}** Markuperweiterung wird verwendet, um Bind Dateneigenschaften auf die Steuerelemente zum Werte stammen aus einer Datenquelle, z. B. Code. description: Die **{Binding}**-Markuperweiterung wird beim Laden von XAML in eine Instanz der [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)-Klasse konvertiert. Dieses Bindungsobjekt erhält einen Wert von der Eigenschaft einer Datenquelle und leitet ihn an die Eigenschaft des Steuerelements weiter. Das Bindungsobjekt kann optional konfiguriert werden, um Änderungen am Wert der Datenquelleneigenschaft zu beobachten und sich basierend auf diesen Änderungen zu aktualisieren. Es kann optional auch so konfiguriert werden, dass Änderungen am Wert des Steuerelements per Push zurück an die Quelleigenschaft gesendet werden. Die als Ziel einer Datenbindung verwendete Eigenschaft muss eine Abhängigkeitseigenschaft sein. Weitere Informationen finden Sie unter [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md).

**{Binding}** weist die gleiche Rangfolge für Abhängigkeitseigenschaften auf wie ein lokaler Wert. So wird beim Festlegen eines lokalen Werts im imperativen Code der Effekt aller im Markup festgelegten **{Binding}**-Objekte entfernt.

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax


``` syntax
<object property="{Binding}" .../>
-or-
<object property="{Binding propertyPath}" .../>
-or-
<object property="{Binding bindingProperties}" .../>
-or-
<object property="{Binding propertyPath, bindingProperties}" .../>
```

| Begriff | Beschreibung |
|------|-------------|
| *"PropertyPath"* | Eine Zeichenfolge, die den Eigenschaftspfad für die Bindung angibt. Weitere Informationen finden Sie unten im Abschnitt [Eigenschaftspfad](#property-path). |
| *bindingProperties* | *PropName*=*Wert*\[, *PropName*=*Wert*\]*<br/>Mindestens eine Bindungseigenschaft, die mithilfe einer Name-Wert-Paarsyntax angegeben wird. |
| *propName* | Der Zeichenfolgenname der für das [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)-Objekt festzulegenden Eigenschaft. Beispiel: „Konverter“. |
| *Wert* | Der für die Eigenschaft festzulegende Wert. Die Syntax des Arguments hängt von der Eigenschaft der festgelegten Binding-Klasse ab. Weitere Informationen finden Sie unten im Abschnitt [Mit {Binding} festlegbare Eigenschaften der Bindungsklasse](#properties-of-the-binding-class-that-can-be-set-with-binding). |

## <a name="property-path"></a>Eigenschaftspfad

[**Pfad** ](https://msdn.microsoft.com/library/windows/apps/br209830) wird beschrieben, die Eigenschaft, die Datenbindung (die Source-Eigenschaft). „Path“ ist ein Positionsparameter. Das bedeutet, dass Sie den Parameternamen explizit verwenden können (`{Binding Path=EmployeeID}`) oder dass Sie ihn als ersten nicht benannten Parameter angeben können (`{Binding EmployeeID}`).

Der [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830)-Typ ist ein Eigenschaftspfad. Es handelt sich dabei um eine Zeichenfolge, die als Eigenschaft oder Untereigenschaft Ihres benutzerdefinierten Typs oder eines Frameworktyps ausgewertet wird. Der Typ kann auch eine [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)-Klasse sein. Die Schritte in einem Eigenschaftspfad werden durch Punkte getrennt (.), und Sie können mehrere Trennzeichen angeben, um aufeinander folgende untergeordnete Eigenschaften zu durchlaufen. Verwenden Sie unabhängig von der verwendeten Programmiersprache einen Punkt als Trennzeichen, um das Objekt zu implementieren, an das die Bindung erfolgt.

Zum Binden der Benutzeroberfläche an die Vornameneigenschaft eines Mitarbeiterobjekts können Sie z. B. „Employee.FirstName“ als Eigenschaftspfad verwenden. Wenn Sie ein ItemsControl-Element an eine Eigenschaft binden, die die abhängigen Elemente des Mitarbeiters enthält, kann der Eigenschaftspfad „Employee.Dependents“ lauten, und die Elementvorlage des ItemsControl-Elements wäre für die Anzeige der Elemente „Dependents“ verantwortlich.

Wenn die Datenquelle eine Auflistung ist, kann der Eigenschaftspfad Elemente in der Auflistung anhand ihrer Position oder ihres Indexes angeben. Z. B. "Teams\[0\]. Dessen Mitglieder", in dem das Literal"\[\]"schließt"0", der das erste Element in einer Auflistung angibt.

Wenn Sie eine [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828)-Eigenschaft an eine vorhandene [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)Klasse binden, können Sie angefügte Eigenschaften als Teil des Eigenschaftspfads verwenden. Wenn Sie eine angefügte Eigenschaft eindeutig machen möchten, damit der Zwischenpunkt im Namen der angefügten Eigenschaften nicht als Schritt in einen Eigenschaftspfad angesehen wird, müssen Sie Klammern um den besitzerqualifizierten Namen der angefügten Eigenschaft setzen – z. B. `(AutomationProperties.Name)`.

Es wird ein Zwischenobjekt des Eigenschaftspfads als [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)-Objekt in einer Laufzeitdarstellung gespeichert, in den meisten Fällen ist jedoch keine Interaktion mit einem **PropertyPath**-Objekt im Code erforderlich. Sie können i. d. R. die Bindungsinformationen angeben, wenn Sie XAML verwenden möchten.

Weitere Informationen zur Zeichenfolgensyntax für einen Eigenschaftspfad, zu Eigenschaftspfaden in Animationsfeaturebereichen und zum Erstellen eines [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)-Objekts finden Sie unter [Property-path syntax](property-path-syntax.md).

## <a name="properties-of-the-binding-class-that-can-be-set-with-binding"></a>Mit {Binding} festlegbare Eigenschaften der Bindungsklasse


**{Binding}** wird mit der *bindingProperties*-Platzhaltersyntax angegeben, da in der Markuperweiterung mehrere Lese-/Schreibeigenschaften einer [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)-Klasse festgelegt werden können. Die Eigenschaften können in beliebiger Reihenfolge mithilfe von durch Kommas getrennten Paaren *propName*=*value* angegeben werden. Für einige Eigenschaften sind Typen erforderlich, die keine Typkonvertierung enthalten, sodass für diese eigene innerhalb von **{Binding}** geschachtelte Markuperweiterungen erforderlich sind.

| Eigenschaft | Beschreibung |
|----------|-------------|
| [**Pfad**](https://msdn.microsoft.com/library/windows/apps/br209830) | Weitere Informationen finden Sie weiter oben im Abschnitt [Eigenschaftspfad](#property-path). |
| [**Konverter**](https://msdn.microsoft.com/library/windows/apps/br209826) | Gibt ein Konverterobjekt an, das vom Bindungsmodul aufgerufen wird. Der Konverter kann im Markupcode mit der [{StaticResource}-Markuperweiterung](staticresource-markup-extension.md) festgelegt werden, um aus einem Ressourcenwörterbuch auf dieses Objekt zu verweisen. |
| [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) | Gibt die Kultur an, die vom Konverter verwendet werden soll. (Wenn Sie festlegen, sind [ **Konverter**](https://msdn.microsoft.com/library/windows/apps/br209826).) Die Kultur wird als eine standardbasierte-ID festgelegt. Weitere Informationen finden Sie unter [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880). |
| [**converterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827) | Gibt einen Konverterparameter an, der in der Konverterlogik verwendet werden kann. (Wenn Sie festlegen, sind [ **Konverter**](https://msdn.microsoft.com/library/windows/apps/br209826).) Die meisten Konverter verwenden Sie einfachen Logik, die alle benötigten Informationen aus dem übergebenen Wert zu konvertierenden abrufen und müssen keine **ConverterParameter** Wert. Der **ConverterParameter**-Parameter ist für komplexere Konverterimplementierungen mit bedingter Logik gedacht, bei der überprüft wird, was in **ConverterParameter** übergeben wird. Sie können auch einen Konverter schreiben, der keine Zeichenfolgenwerte verwendet. Dies ist jedoch sehr ungewöhnlich. Weitere Informationen finden Sie in den „Anmerkungen“ zu **ConverterParameter**. |
| [**elementName**](https://msdn.microsoft.com/library/windows/apps/br209828) | Gibt eine Datenquelle durch Verweis auf ein anderes Element in demselben XAML-Konstrukt an, das über eine **Name**-Eigenschaft oder ein [x:Name-Attribut](x-name-attribute.md) verfügt. Damit werden häufig zusammengehörige Werte oder Untereigenschaften eines Benutzeroberflächenelements gemeinsam verwendet, um einen bestimmten Wert für ein anderes Element bereitzustellen, z. B. in einer XAML-Steuerelementvorlage. |
| [**fallbackValue**](https://msdn.microsoft.com/library/windows/apps/dn279345) | Gibt einen Wert an, der angezeigt wird, wenn die Quelle oder der Pfad nicht aufgelöst werden können. |
| [**Modus**](https://msdn.microsoft.com/library/windows/apps/br209829) | Gibt den Bindungsmodus, als einen der folgenden Werte an: "OneTime", "OneWay" oder "TwoWay". Diese Zeichenfolgen entsprechen den Konstantennamen der [**BindingMode**](https://msdn.microsoft.com/library/windows/apps/br209822)-Enumeration. Der Standard lautet „OneWay“. Beachten Sie, dass dieser vom Standard für **{x:Bind}** abweicht, der „OneTime“ lautet. | 
| [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831) | Gibt durch das Beschreiben der Position der Bindungsquelle relativ zur Position des Bindungsziels eine Datenquelle an. Dies wird am häufigsten in Bindungen innerhalb von XAML-Steuervorlagen verwendet. Legen Sie die [{RelativeSource}-Markuperweiterung](relativesource-markup-extension.md) fest. |
| [**Quelle**](https://msdn.microsoft.com/library/windows/apps/br209832) | Gibt die Objektdatenquelle an. In der **Binding**-Markuperweiterung erfordert die [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832)-Eigenschaft einen Objektverweis, beispielsweise einen Verweis in der [{StaticResource}-Markuperweiterung](staticresource-markup-extension.md) Ist die Eigenschaft nicht angegeben, wird die Quelle vom aktiven Datenkontext angegeben. Häufig wird ein Source-Wert nicht in einzelnen Bindungen angegeben, sondern stattdessen der gemeinsame **DataContext** für mehrere Bindungen verwendet. Weitere Informationen finden Sie unter [**DataContext**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.datacontext.aspx) oder [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946). |
| [**TargetNullValue**](https://msdn.microsoft.com/library/windows/apps/dn279347) | Gibt einen Wert an, der angezeigt wird, wenn der Quellwert aufgelöst werden kann, aber explizit **null** ist. |
| [**UpdateSourceTrigger**](https://msdn.microsoft.com/library/windows/apps/dn279350) | Gibt den Zeitpunkt für Aktualisierungen von Bindungsquellen an. Wenn keine Angabe erfolgt, lautet der Standardwert **Default**. |

**Beachten Sie**  , wenn Sie Markup konvertieren **{X: Bind}** zu **{Binding}**, dann werden die Unterschiede in der Standardwerte für die **Modus** Diese Eigenschaft.

[**Konverter**](https://msdn.microsoft.com/library/windows/apps/br209826), [ **ConverterLanguage** ](https://msdn.microsoft.com/library/windows/apps/hh701880) und **ConverterLanguage** beziehen sich auf das Szenario der ein Wert konvertiert wird, oder geben Sie aus der die Bindungsquelle in einen Typ oder Wert, der mit die Bindungsziel-Eigenschaft kompatibel ist. Weitere Informationen und Beispiele finden Sie im Abschnitt „Datenkonvertierungen“ unter [Datenbindung im Detail](https://msdn.microsoft.com/library/windows/apps/mt210946).

> [!NOTE]
> Ab Windows 10, Version 1607, wird über das XAML-Framework ein integrierter Konverter für die Konvertierung eines booleschen Operanden in einen Sichtbarkeitszustand bereitgestellt. Der Konverter verknüpft **true** mit dem Enumerationswert **Visible** und **false** mit dem Wert **Collapsed**, sodass Sie eine Visibility-Eigenschaft an einen booleschen Wert binden können, ohne einen Konverter zu erstellen. Für die Verwendung des integrierten Konverters muss die SDK-Zielversion der App mindestens 14393 lauten. Die Verwendung ist nicht möglich, wenn Ihre App für frühere Versionen von Windows 10 bestimmt ist. Weitere Informationen zu Zielversionen finden Sie unter [Versionsadaptiver Code](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

[**Quelle**](https://msdn.microsoft.com/library/windows/apps/br209832), [ **RelativeSource**](https://msdn.microsoft.com/library/windows/apps/br209831), und [ **ElementName** ](https://msdn.microsoft.com/library/windows/apps/br209828) eine Bindungsquelle angeben, damit sie sind gegenseitig.

**Tipp**  bei Bedarf an einzelnen geschweiften Klammern nach einem Wert wie z. B. in [ **Pfad** ](https://msdn.microsoft.com/library/windows/apps/br209830) oder [ **ConverterParameter** ](https://msdn.microsoft.com/library/windows/apps/br209827), klicken Sie dann einen umgekehrten Schrägstrich voran: `\{`. Setzen Sie alternativ die gesamte Zeichenfolge mit den geschweiften Klammern, für die Escapezeichen verwendet werden müssen, in weitere Anführungszeichen, z. B. `ConverterParameter='{Mix}'`.

## <a name="examples"></a>Beispiele

```XML
<!-- binding a UI element to a view model -->    
<Page ... >
    <Page.DataContext>
        <local:BookstoreViewModel/>
    </Page.DataContext>

    <GridView ItemsSource="{Binding BookSkus}" SelectedItem="{Binding SelectedBookSku, Mode=TwoWay}" ... />
</Page>
```

```XML
<!-- binding a UI element to another UI element -->
<Page ... >
    <Page.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </Page.Resources>

    <Slider x:Name="sliderValueConverter" ... />
    <TextBox Text="{Binding Path=Value, ElementName=sliderValueConverter,
        Mode=OneWay,
        Converter={StaticResource GradeConverter}}"/>
</Page>
```

Im zweite Beispiel wird vier verschiedene [ **Bindung** ](https://msdn.microsoft.com/library/windows/apps/br209820) Eigenschaften: [**ElementName**](https://msdn.microsoft.com/library/windows/apps/br209828), [ **Pfad**](https://msdn.microsoft.com/library/windows/apps/br209830), [ **Modus** ](https://msdn.microsoft.com/library/windows/apps/br209829) und [ **Konverter** ](https://msdn.microsoft.com/library/windows/apps/br209826). **Path** wird in diesem Fall explizit als **Binding**-Eigenschaft benannt gezeigt. Die **Path**-Eigenschaft wird zu einer Datenbindungsquelle ausgewertet, bei der es sich um ein anderes Objekt in derselben Laufzeit-Objektstruktur handelt – ein [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) mit dem Namen `sliderValueConverter`.

Beachten Sie, dass der [**Converter**](https://msdn.microsoft.com/library/windows/apps/br209826)-Eigenschaftswert eine weitere Markuperweiterung ([{StaticResource} markup extension](staticresource-markup-extension.md)) verwendet, sodass hier zwei geschachtelte Markuperweiterungssyntaxen zu sehen sind. Die innere Syntax wird zuerst ausgewertet. Nach dem Abrufen der Ressource ist daher ein [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903) verfügbar (eine benutzerdefinierte Klasse, die vom `local:S2Formatter`-Element in Ressourcen instanziiert wird), den die Bindung verwenden kann.

## <a name="tools-support"></a>Unterstützung von Tools

Mit Microsoft IntelliSense in Microsoft Visual Studio werden die Eigenschaften des Datenkontexts beim Erstellen von **{Binding}** im XAML-Markup-Editor angezeigt. Sobald Sie „{Binding“ eingeben, werden für die [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830)-Eigenschaft geeignete Datenkontexteigenschaften in der Dropdownliste angezeigt. IntelliSense bietet auch Unterstützung für andere Eigenschaften von [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820). Damit dies funktioniert muss der Datenkontext oder der Entwurfszeit-Datenkontext auf der Markupseite festgelegt werden. **Gehe zu Definition** (F12) funktioniert auch bei **{Binding}**. Alternativ können Sie das Dialogfeld „Datenbindung“ verwenden.

 
