---
description: Die Markuperweiterung xBind ermöglicht Funktionen, die im Markup verwendet werden.
title: Funktionen in x:Bind
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, Uwp, xBind
ms.localizationpriority: medium
ms.openlocfilehash: b85777c254c36cc7bf5b156569c7cef267a6c567
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626215"
---
# <a name="functions-in-xbind"></a>Funktionen in x:Bind

> [!NOTE]
> Allgemeine Informationen zum Verwenden der Datenbindung in Ihrer app mit **{X: Bind}** (und eine allumfassende Vergleich zwischen **{X: Bind}** und **{Binding}**), finden Sie unter [Daten Binden ausführlich](data-binding-in-depth.md).

Ab Windows 10, Version 1607, unterstützt **{x: Bind}** die Verwendung einer Funktion als blattbildenden Schritt des Bindungspfades. Dadurch können:

- Eine einfachere Möglichkeit der Konvertierung von Werten
- Eine Möglichkeit, Bindungen von mehr als einem Parameter abhängig zu machen

> [!NOTE]
> Wenn Sie Funktionen für **{x: Bind}** verwenden möchten, muss die Ziel-SDK-Version 14393 oder höher sein. Sie können keine Funktionen verwenden, wenn Ihre App für frühere Versionen von Windows 10 bestimmt ist. Weitere Informationen zu Zielversionen finden Sie unter [Versionsadaptiver Code](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

Im folgenden Beispiel werden Hintergrund und Vordergrund des Elements an Funktionen gebunden, um eine Konvertierung basierend auf dem Farbparameter durchzuführen

```xaml
<DataTemplate x:DataType="local:ColorEntry">
    <Grid Background="{x:Bind local:ColorEntry.Brushify(Color), Mode=OneWay}" Width="240">
        <TextBlock Text="{x:Bind ColorName}" Foreground="{x:Bind TextColor(Color)}" Margin="10,5" />
    </Grid>
</DataTemplate>
```

```csharp
class ColorEntry
{
    public string ColorName { get; set; }
    public Color Color { get; set; }

    public static SolidColorBrush Brushify(Color c)
    {
        return new SolidColorBrush(c);
    }

    public SolidColorBrush TextColor(Color c)
    {
        return new SolidColorBrush(((c.R * 0.299 + c.G * 0.587 + c.B * 0.114) > 150) ? Colors.Black : Colors.White);
    }
}
```

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

```xaml
<object property="{x:Bind pathToFunction.FunctionName(functionParameter1, functionParameter2, ...), bindingProperties}" ... />
```

## <a name="path-to-the-function"></a>Pfad der Funktion

Der Pfad der Funktion wird wie jeder andere Eigenschaftspfad angegeben. Er kann Punkte (.), Indexer oder Umwandlungen für die Suche nach der Funktion enthalten.

Statische Funktionen können mithilfe der XMLNamespace:ClassName.MethodName-Syntax angegeben werden. Verwenden Sie z. B. der folgenden Syntax für die Bindung an statische Funktionen im Code-Behind.

```xaml
<Page 
     xmlns:local="using:MyNamespace">
     ...
    <StackPanel>
        <TextBlock x:Name="BigTextBlock" FontSize="20" Text="Big text" />
        <TextBlock FontSize="{x:Bind local:MyHelpers.Half(BigTextBlock.FontSize)}" 
                   Text="Small text" />
    </StackPanel>
</Page>
```

```csharp
namespace MyNamespace
{
    static public class MyHelpers
    {
        public static double Half(double value) => value / 2.0;
    }
}
```

Sie können auch direkt im Markup Systemfunktionen verwenden, um einfache Szenarien wie die Formatierung des Datums, Text zu formatieren, Text Verkettungen, usw., z. B. zu erreichen:

```xaml
<Page 
     xmlns:sys="using:System"
     xmlns:local="using:MyNamespace">
     ...
     <CalendarDatePicker Date="{x:Bind sys:DateTime.Parse(TextBlock1.Text)}" />
     <TextBlock Text="{x:Bind sys:String.Format('{0} is now available in {1}', local:MyPage.personName, local:MyPage.location)}" />
</Page>
```

Ist der Modus OneWay/TwoWay, wird auf den Pfad der Funktion eine Änderungserkennung angewendet, und die Bindung wird neu ausgewertet, wenn diese Objekte geändert wurden.

Für die zu bindende Funktion müssen folgende Voraussetzungen gelten:

- Code und die Metadaten müssen auf sie zugreifen können, d. h. interne/private Aktionen in C#, aber für C++/CX werden öffentliche WinRT-Methoden benötigt
- Überladung basiert auf der Anzahl der Argumente, nicht auf ihrem Typ, und es wird die erste Übereinstimmung mit dieser Anzahl von Argumenten gesucht
- Die Argumenttypen müssen den übergebenen Daten entsprechen. Es werden keine einschränkenden Konvertierungen durchgeführt
- Der Rückgabetyp der Funktion muss mit dem Typ der Eigenschaft übereinstimmen, für die die Bindung verwendet wird

Die Bindungs-Engine reagiert auf die Eigenschaftenänderung Benachrichtigungen ausgelöst werden, mit dem Funktionsnamen und Bindungen nach Bedarf neu zu bewerten. Zum Beispiel:

```xaml
<DataTemplate x:DataType="local:Person">
   <StackPanel>
      <TextBlock Text="{x:Bind FullName}" />
      <Image Source="{x:Bind IconToBitmap(Icon, CancellationToken), Mode=OneWay}" />
   </StackPanel>
</DataTemplate>
```

```csharp
public class Person:INotifyPropertyChanged
{
    //Implementation for an Icon property and a CancellationToken property with PropertyChanged notifications
    ...

    //IconToBitmap function is essentially a multi binding converter between several options.
    public Uri IconToBitmap (Uri icon, Uri cancellationToken)
    {
        Uri foo = new Uri(...);        
        if (isCancelled)
        {
            foo = cancellationToken;
        }
        else 
        {
            if (this.fullName.Contains("Sr"))
            {
               //pass a different Uri back
               foo = new Uri(...);
            }
            else
            {
                foo = icon;
            }
        }
        return foo;
    }

    //Ensure FullName property handles change notification on itself as well as IconToBitmap since the function uses it
    public string FullName
    {
        get { return this.fullName; }
        set
        {
            this.fullName = value;
            this.OnPropertyChanged ();
            this.OnPropertyChanged ("IconToBitmap"); 
            //this ensures Image.Source binding re-evaluates when FullName changes in addition to Icon and CancellationToken
        }
    }
}
```

> [!TIP]
> Sie können Funktionen in X: Bind als was über Konverter und MultiBinding-Instanz in WPF unterstützt dieselben Szenarien erreichen.

## <a name="function-arguments"></a>Funktionsargumente

Mehrere Argumente können durch Komma (,) voneinander getrennt angegeben werden

- Bindungspfad – dieselbe Syntax wie bei einer direkten Bindung an das Objekt.
  - Ist der Modus OneWay/TwoWay, wird eine Änderungserkennung angewendet, und die Bindung wird neu ausgewertet, wenn diese Objekte geändert wurden.
- Konstante Zeichenfolge in Anführungszeichen – Anführungszeichen müssen gesetzt werden, um Zeichenfolgen kenntlich zu machen Das Caret-Symbol (^) kann als Escapezeichen für Anführungszeichen innerhalb von Zeichenfolgen verwendet werden.
- Konstante Zahl – z. B. -123.456
- Boolean – als "x: True" oder "x: False" angeben

### <a name="two-way-function-bindings"></a>Bidirektionale Funktionsbindung

In einem Szenario mit bidirektionaler Bindung muss eine zweite Funktion für die umgekehrte Bindungsrichtung angegeben werden. Dies erfolgt mithilfe der **BindBack** Eigenschaft binden. In der folgenden Beispiel gibt es dauert für die Funktion ein Argument, das den Wert, der wieder in das Modell mithilfe von Push übertragen werden muss.

```xaml
<TextBlock Text="{x:Bind a.MyFunc(b), BindBack=a.MyFunc2, Mode=TwoWay}" />
```
