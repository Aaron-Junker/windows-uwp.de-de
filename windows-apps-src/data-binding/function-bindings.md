---
description: Die xBind-Markuperweiterung ermöglicht die Verwendung von Funktionen im Markup.
title: Funktionen in x:Bind
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, UWP, xBind
ms.localizationpriority: medium
ms.openlocfilehash: 879be9591bae36a1dbcd485387fbb4ac7f502fea
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "66360074"
---
# <a name="functions-in-xbind"></a>Funktionen in x:Bind

> [!NOTE]
> Allgemeine Informationen zur Verwendung der Datenbindung in Ihrer App mit **{x:Bind}** (sowie einen Gesamtvergleich von **{x:Bind}** und **{Binding}** ) finden Sie unter [Datenbindung im Detail](data-binding-in-depth.md).

Ab Windows 10, Version 1607, unterstützt **{x: Bind}** die Verwendung einer Funktion als blattbildenden Schritt des Bindungspfades. Dadurch wird Folgendes ermöglicht:

- Eine einfachere Möglichkeit der Konvertierung von Werten
- Eine Möglichkeit, Bindungen von mehr als einem Parameter abhängig zu machen

> [!NOTE]
> Wenn Sie Funktionen für **{x: Bind}** verwenden möchten, muss die Ziel-SDK-Version 14393 oder höher sein. Sie können keine Funktionen verwenden, wenn Ihre App für frühere Versionen von Windows 10 bestimmt ist. Weitere Informationen zu Zielversionen finden Sie unter [Versionsadaptiver Code](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

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

Statische Funktionen können mithilfe der XMLNamespace:ClassName.MethodName-Syntax angegeben werden. Verwende z. B. die folgende Syntax zum Binden an statische Funktionen in CodeBehind.

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

Du kannst Systemfunktionen auch direkt im Markup verwenden, um einfache Szenarien wie Datumsformatierung, Textformatierung, Textverkettungen usw. zu erreichen. Beispiel:

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

Die Bindungs-Engine reagiert auf ausgelöste Benachrichtigungen über Eigenschaftsänderungen mit dem Funktionsnamen und wertet Bindungen bei Bedarf neu aus. Zum Beispiel:

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
> Du kannst Funktionen in x:Bind verwenden, um dieselben Szenarien zu verwirklichen wie die, die mittels Konvertern und MultiBinding in WPF unterstützt wurden.

## <a name="function-arguments"></a>Funktionsargumente

Mehrere Argumente können durch Komma (,) voneinander getrennt angegeben werden

- Bindungspfad – dieselbe Syntax wie bei einer direkten Bindung an das Objekt.
  - Ist der Modus OneWay/TwoWay, wird eine Änderungserkennung angewendet, und die Bindung wird neu ausgewertet, wenn diese Objekte geändert wurden.
- Konstante Zeichenfolge in Anführungszeichen – Anführungszeichen müssen gesetzt werden, um Zeichenfolgen kenntlich zu machen Das Caret-Symbol (^) kann als Escapezeichen für Anführungszeichen innerhalb von Zeichenfolgen verwendet werden.
- Konstante Zahl – z. B. -123.456
- Boolean – als "x: True" oder "x: False" angeben

### <a name="two-way-function-bindings"></a>Bidirektionale Funktionsbindung

In einem Szenario mit bidirektionaler Bindung muss eine zweite Funktion für die umgekehrte Bindungsrichtung angegeben werden. Dies erfolgt mithilfe der Bindungseigenschaft **BindBack**. Im folgenden Beispiel muss die Funktion ein Argument übernehmen: den Wert, der vom Modell übernommen werden muss.

```xaml
<TextBlock Text="{x:Bind a.MyFunc(b), BindBack=a.MyFunc2, Mode=TwoWay}" />
```
