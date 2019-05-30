---
description: Dient zum eindeutigen Identifizieren von Elementen, die als Ressourcen erstellt und referenziert werden und innerhalb eines ResourceDictionary-Elements vorhanden sind.
title: xKey-Attribut
ms.assetid: 141FC5AF-80EE-4401-8A1B-17CB22C2277A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: cb42fcb17cfcad76989732b1a1482d9fbc85be5e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372322"
---
# <a name="xkey-attribute"></a>x:Key-Attribut


Dient zum eindeutigen Identifizieren von Elementen, die als Ressourcen erstellt und referenziert werden und innerhalb eines [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary)-Elements vorhanden sind.

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

``` syntax
<ResourceDictionary>
  <object x:Key="stringKeyValue".../>
</ResourceDictionary>
```

## <a name="xaml-attribute-usage-implicit-resourcedictionary"></a>XAML-Attributverwendung (implizites **ResourceDictionary**)

``` syntax
<object.Resources>
  <object x:Key="stringKeyValue".../>
</object.Resources>
```

## <a name="xaml-values"></a>XAML-Werte

| Begriff | Beschreibung |
|------|-------------|
| object | Beliebige Objekte, die freigegeben werden können. Weitere Informationen finden Sie unter [ResourceDictionary- und XAML-Ressourcenverweise](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references). |
| stringKeyValue | Eine als Schlüssel verwendete echte Zeichenfolge, die der _XamlName_-Grammatik entsprechen muss. Weitere Informationen erhalten Sie in "XamlName-Grammatik" unten. | 

##  <a name="xamlname-grammar"></a>XamlName-Grammatik

Im Anschluss finden Sie die maßgebende Grammatik für eine Zeichenfolge, die in der UWP-XAML-Implementierung (Universelle Windows-Plattform) als Schlüssel verwendet wird:

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   Zeichen sind eingeschränkt, auf den niedrigeren ASCII-Bereich und genauer gesagt mit lateinischen Alphabet in Großbuchstaben und Kleinbuchstaben, Ziffern und Unterstriche (\_) Zeichen.
-   Der Unicode-Zeichenbereich wird nicht unterstützt.
-   Ein Name darf nicht mit einer Ziffer beginnen.

## <a name="remarks"></a>Hinweise

Zu den untergeordneten Elementen eines [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary)-Elements gehört im Allgemeinen ein **x:Key**-Attribut zum Angeben eines eindeutigen Schlüsselwerts innerhalb des Wörterbuchs. Die Eindeutigkeit von Schlüsseln wird beim Laden durch den XAML-Prozessor erzwungen. Nicht eindeutige **x:Key**-Werte haben XAML-Analyseausnahmen zur Folge. Auf Anforderung der [{StaticResource}-Markuperweiterung](staticresource-markup-extension.md) haben auch nicht aufgelöste Schlüssel XAML-Analyseausnahmen zur Folge.

**x:Key** und [x:Name](x-name-attribute.md) sind nicht identisch. **x:Key** wird ausschließlich in Ressourcenwörterbüchern verwendet. x:Name wird in allen XAML-Bereichen verwendet. Durch einen [**FindName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.findname)-Aufruf mit einem Schlüsselwert wird keine Ressource mit Schlüssel abgerufen. Objekte, die in einem Ressourcenwörterbuch definiert sind, verfügen über **x: Key** oder **x: Name** oder über beide. „Key“ und „Name“ müssen nicht übereinstimmen.

Beachten Sie, dass das [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary)-Objekt in der gezeigten impliziten Syntax dahingehend implizit ist, wie der XAML-Prozessor ein neues Objekt erzeugt, um eine [**Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources)-Sammlung aufzufüllen.

Der Code zum Angeben von **x:Key** entspricht einem beliebigen Vorgang, in dem ein Schlüssel mit dem zugrunde liegenden [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) verwendet wird. So entspricht beispielsweise ein im Markup für eine Ressource angewendeter **x:Key** dem Wert des *key*-Parameters **Insert**, wenn Sie die Ressource einem **ResourceDictionary** hinzufügen.

Ein Element in einem Ressourcenwörterbuch kann einen Wert für **x:Key** auslassen, wenn es sich um ein zielgerichtetes [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style)- oder [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)-Element handelt. In all diesen Fällen ist der implizite Schlüssel des Ressourcenelements der als Zeichenfolge interpretierte **TargetType**-Wert. Weitere Informationen finden Sie unter [Schnellstart: Formatieren von Steuerelementen](https://docs.microsoft.com/previous-versions/windows/apps/hh465498(v=win.10)) und [ResourceDictionary- und XAML-Ressourcenverweise](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

