---
author: jwmsft
description: Dient zum eindeutigen Identifizieren von Elementen, die als Ressourcen erstellt und referenziert werden und innerhalb eines ResourceDictionary-Elements vorhanden sind.
title: xKey-Attribut
ms.assetid: 141FC5AF-80EE-4401-8A1B-17CB22C2277A
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8d48ccb93a411e92b57059192de38366f27353a3
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "7557124"
---
# <a name="xkey-attribute"></a>x:Key-Attribut


Dient zum eindeutigen Identifizieren von Elementen, die als Ressourcen erstellt und referenziert werden und innerhalb eines [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-Elements vorhanden sind.

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

| Benennung | Beschreibung |
|------|-------------|
| object | Beliebige Objekte, die freigegeben werden können. Weitere Informationen finden Sie unter [ResourceDictionary- und XAML-Ressourcenverweise](https://msdn.microsoft.com/library/windows/apps/mt187273). |
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

-   Die Zeichen sind auf den unteren ASCII-Bereich beschränkt – genauer: auf Groß- und Kleinbuchstaben des lateinischen Alphabets sowie auf Ziffern und den Unterstrich (\_).
-   Der Unicode-Zeichenbereich wird nicht unterstützt.
-   Ein Name darf nicht mit einer Ziffer beginnen.

## <a name="remarks"></a>Hinweise

Zu den untergeordneten Elementen eines [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-Elements gehört im Allgemeinen ein **x:Key**-Attribut zum Angeben eines eindeutigen Schlüsselwerts innerhalb des Wörterbuchs. Die Eindeutigkeit von Schlüsseln wird beim Laden durch den XAML-Prozessor erzwungen. Nicht eindeutige **x:Key**-Werte haben XAML-Analyseausnahmen zur Folge. Auf Anforderung der [{StaticResource}-Markuperweiterung](staticresource-markup-extension.md) haben auch nicht aufgelöste Schlüssel XAML-Analyseausnahmen zur Folge.

**x:Key** und [x:Name](x-name-attribute.md) sind nicht identisch. **x:Key** wird ausschließlich in Ressourcenwörterbüchern verwendet. x:Name wird in allen XAML-Bereichen verwendet. Durch einen [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)-Aufruf mit einem Schlüsselwert wird keine Ressource mit Schlüssel abgerufen. Objekte, die in einem Ressourcenwörterbuch definiert sind, verfügen über **x: Key** oder **x: Name** oder über beide. „Key“ und „Name“ müssen nicht übereinstimmen.

Beachten Sie, dass das [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)-Objekt in der gezeigten impliziten Syntax dahingehend implizit ist, wie der XAML-Prozessor ein neues Objekt erzeugt, um eine [**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740)-Sammlung aufzufüllen.

Der Code zum Angeben von **x:Key** entspricht einem beliebigen Vorgang, in dem ein Schlüssel mit dem zugrunde liegenden [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) verwendet wird. So entspricht beispielsweise ein im Markup für eine Ressource angewendeter **x:Key** dem Wert des *key*-Parameters **Insert**, wenn Sie die Ressource einem **ResourceDictionary** hinzufügen.

Ein Element in einem Ressourcenwörterbuch kann einen Wert für **x:Key** auslassen, wenn es sich um ein zielgerichtetes [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849)- oder [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)-Element handelt. In all diesen Fällen ist der implizite Schlüssel des Ressourcenelements der als Zeichenfolge interpretierte **TargetType**-Wert. Weitere Informationen finden Sie unter [Schnellstart: Formatieren von Steuerelementen](https://msdn.microsoft.com/library/windows/apps/hh465498) und [ResourceDictionary- und XAML-Ressourcenverweise](https://msdn.microsoft.com/library/windows/apps/mt187273).

