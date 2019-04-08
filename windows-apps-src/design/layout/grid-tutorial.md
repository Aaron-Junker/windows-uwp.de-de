---
Description: In diesem Tutorial erläutert, wie eine einfache Anwendung-Benutzeroberfläche zu erstellen. Es erläutert und veranschaulicht die Verwendung von Grid und StackPanel, zwei der häufigsten XAML-Elemente.
title: Verwenden Sie Grid und StackPanel, um eine einfache Wetter-App zu erstellen.
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 9794a04d-e67f-472c-8ba8-8ebe442f6ef2
ms.localizationpriority: medium
ms.openlocfilehash: 5b221220d417df5b70927984ac65eff93fae54a4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646535"
---
# <a name="tutorial-use-grid-and-stackpanel-to-create-a-simple-weather-app"></a>Tutorial: Raster verwenden "und" StackPanel ", um eine einfache Wetter-app zu erstellen.

Verwenden Sie zum Erstellen des Layouts für eine einfache Wetter-App mit XAML die Elemente **Grid** und **StackPanel**. Mit diesen Tools können Sie hervorragend aussehende Apps erstellen, die auf jedem Gerät mit Windows 10 funktionieren. Dieses Lernprogramm dauert 10 bis 20 Minuten.

> **Wichtige APIs:** [Grid-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.grid), [StackPanel-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.stackpanel)

## <a name="prerequisites"></a>Voraussetzungen
- Windows 10 und Microsoft Visual Studio 2015 oder höher. (Neueste Visual Studio, die für die aktuelle Entwicklung "und" Security Updates empfohlen werden) [Hier klicken, um zu erfahren, wie Sie mit Visual Studio Einrichtung](../../get-started/get-set-up.md).
- Kenntnisse im Erstellen einer einfachen „Hello, World“-App mit XAML und C#. Wenn Sie diese noch nicht besitzen [klicken Sie hier, um zu erfahren, wie Sie eine „Hello World“-App erstellen](https://msdn.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="step-1-create-a-blank-app"></a>Schritt 1: Erstellen einer leeren app
1. Wählen Sie in Visual Studio **Datei** > **Neues Projekt** aus.
2. Wählen Sie im linken Bereich des Dialogfelds **Neues Projekt****Visual C#** > **Windows** > **Universell** oder **Visual C++** > **Windows** > **Universell** aus.
3. Wählen Sie im mittleren Bereich **Blank App** aus.
4. Geben Sie im Feld **Name****WeatherPanel** ein, und wählen Sie **OK** aus.
5. Wählen Sie zum Ausführen des Programms im Menü **Debugging** > **Debugging starten** aus, oder drücken Sie F5.

## <a name="step-2-define-a-grid"></a>Schritt 2: Definieren Sie ein Raster
In XAML besteht ein **Raster** aus einer Reihe von Zeilen und Spalten. Durch Angabe der Zeile und Spalte eines Elements innerhalb eines **Rasters** können Sie andere Elemente in einer Benutzeroberfläche platzieren und anordnen. Zeilen und Spalten werden mit den Elementen **RowDefinition** und **ColumnDefinition** definiert.

Um mit dem Erstellen eines Layouts zu beginnen, öffnen Sie **MainPage.xaml** mithilfe des **Projektmappen-Explorers** und ersetzen das automatisch generierte **Grid**-Element durch diesen Code.

```xml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="3*"/>
        <ColumnDefinition Width="5*"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="2*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
</Grid>
```

Das neue **Raster** erstellt eine Reihe von zwei Zeilen und Spalten, die das Layout der App-Oberfläche definieren. Die erste Spalte enthält eine **Breite** der "3\*", während die zweite "5\*", den horizontalen Abstand zwischen den beiden Spalten bei einem Verhältnis von 3:5 geteilt. Auf die gleiche Weise, die zwei Zeilen verfügen eine **Höhe** von "2\*"und"\*", also die **Raster** reserviert zwei Mal so viel Speicherplatz für die erste Zeile wie bei der zweiten ("\*"ist identisch mit" 1\*"). Diese Seitenverhältnisse werden bewahrt, auch wenn die Fenstergröße oder das Gerät geändert werden.

Informationen zu weiteren Methoden für die Größeneinstellung von Zeilen und Spalten finden Sie unter [Definieren von Layouts mit XAML](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml#layout-properties).

Wenn Sie die Anwendung jetzt ausführen, wird Ihnen lediglich eine leere Seite angezeigt, da keiner der **Raster**-Bereiche Inhalte enthält. Um das **Raster** anzuzeigen, fügen wir Farbe hinzu.

## <a name="step-3-color-the-grid"></a>Schritt 3: Farbe des Rasters
Um dem **Raster** Farbe hinzuzufügen, fügen wir drei **Border**-Elemente mit jeweils einer anderen Hintergrundfarbe hinzu. Jedes Element wird darüber hinaus einer Zeile und Spalte im übergeordneten **Grid** zugewiesen. Dies erfolgt mithilfe der Attribute **Grid.Row** und **Grid.Column**. Die Werte dieser Attribute sind standardmäßig 0. Daher müssen Sie diese dem ersten **Border** nicht zuweisen. Fügen Sie dem **Grid**-Element nach den Zeilen- und Spaltendefinitionen den folgenden Code hinzu.

```xml
<Border Background="#2f5cb6"/>
<Border Grid.Column ="1" Background="#1f3d7a"/>
<Border Grid.Row="1" Grid.ColumnSpan="2" Background="#152951"/>
```

Beachten Sie, dass für das dritte **Border**-Element ein zusätzliches Attribut verwendet wird, **Grid.ColumnSpan**. Dieses bewirkt, dass dieses **Border**-Element beide Spalten in der unteren Zeile umfasst. Sie können **Grid.RowSpan** auf die gleiche Weise verwenden. Mit diesen Attributen können Sie ein Element über eine beliebige Anzahl von Zeilen und Spalten ausdehnen. Die obere linke Ecke einer solchen Ausdehnung entspricht stets der **Grid.Column** und **Grid.Row**, die in den Elementattributen angegeben werden.

Wenn Sie die App ausführen, sieht das Ergebnis wie folgt aus.

![Färben des Rasters](images/grid-weather-1.png)

## <a name="step-4-organize-content-by-using-stackpanel-elements"></a>Schritt 4: Organisieren Sie Inhalte mithilfe von StackPanel-Elementen
**StackPanel** ist das zweite UI-Element, das wir verwenden, um die Wetter-App zu erstellen. **StackPanel** ist ein grundlegender Bestandteil zahlreicher Basislayouts von Apps, mit dem Sie Elemente vertikal oder horizontal stapeln können.

Im folgenden Code erstellen wir zwei **StackPanel**-Elemente und füllen jedes mit drei **TextBlocks**. Fügen Sie diese **StackPanel**-Elemente dem **Grid** unterhalb der **Border**-Elemente aus Schritt 3 hinzu. Dies bewirkt, dass die **TextBlock**-Elemente auf dem zuvor erstellten farbigen **Grid** gerendert werden.

```xml
<StackPanel Grid.Column="1" Margin="40,0,0,0" VerticalAlignment="Center">
    <TextBlock Foreground="White" FontSize="25" Text="Today - 64° F"/>
    <TextBlock Foreground="White" FontSize="25" Text="Partially Cloudy"/>
    <TextBlock Foreground="White" FontSize="25" Text="Precipitation: 25%"/>
</StackPanel>
<StackPanel Grid.Row="1" Grid.ColumnSpan="2" Orientation="Horizontal"
            HorizontalAlignment="Center" VerticalAlignment="Center">
    <TextBlock Foreground="White" FontSize="25" Text="High: 66°" Margin="0,0,20,0"/>
    <TextBlock Foreground="White" FontSize="25" Text="Low: 43°" Margin="0,0,20,0"/>
    <TextBlock Foreground="White" FontSize="25" Text="Feels like: 63°"/>
</StackPanel>
```

Im ersten **Stackpanel** wird jeder **TextBlock** vertikal unterhalb des nächsten gestapelt. Dies ist das Standardverhalten eines StackPanel. Daher müssen wir das **Orientation**-Attribut nicht festlegen. Im zweiten StackPanel-Element möchten wir die untergeordneten Elemente horizontal von links nach rechts stapeln. Daher legen wir das **Orientation**-Attribut auf „Horizontal“ fest. Außerdem müssen wir das **Grid.ColumnSpan**-Attribut auf „2“ festlegen, damit der Text über dem unteren **Border**-Element zentriert wird.

Wenn Sie die App jetzt ausführen, wird sie Ihnen ungefähr wie folgt angezeigt.

![Hinzufügen von StackPanels](images/grid-weather-2.png)

## <a name="step-5-add-an-image-icon"></a>Schritt 5: Fügen Sie ein Image-Symbol hinzu.

Zum Schluss füllen Sie den leeren Abschnitt im **Grid** mit einem Bild, das das Wetter von heute zeigt – ein Bild, das einen teilweise bewölkten Himmel anzeigt.

Laden Sie das Bild unten herunter, und speichern Sie es als PNG-Datei mit dem Namen „Teilweise-bewölkt“.

![Teilweise bewölkt](images/partially-cloudy.PNG)

In der **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die **Assets** Ordner, und wählen **hinzufügen** -> **vorhandenes Element...** Suchen von teilweise cloudy.png im Browser, die eingeblendet, wählen Sie sie und klicken Sie auf **hinzufügen**.

Fügen Sie als Nächstes in **"MainPage.xaml"** das folgende **Image**-Element unterhalb der StackPanels aus Schritt 4 hinzu.

```xml
<Image Margin="20" Source="Assets/partially-cloudy.png"/>
```

Da das Bild in der ersten Zeile und Spalte angezeigt werden soll, müssen wir dessen **Grid.Row**- oder **Grid.Column**-Attribute nicht festlegen. Diese sind standardmäßig auf „0“ festgelegt.

Das war’s! Sie haben das Layout für eine einfache Wetter-App erstellt. Wenn Sie die Anwendung durch Drücken von **F5** ausführen, sollte Ihnen ungefähr Folgendes angezeigt werden:

![Beispiel für Wetterbereich](images/grid-weather-3.PNG)

Wenn Sie möchten, können Sie mit dem Layout oben experimentieren und verschiedene Möglichkeiten erkunden, wie Sie Wetterdaten darstellen können.

## <a name="related-articles"></a>Verwandte Artikel
Eine Einführung in das Entwerfen von UWP-App-Layouts finden Sie unter [Einführung in das UWP-App-Design](https://msdn.microsoft.com/windows/uwp/layout/design-and-ui-intro).

Informationen zum Erstellen von dynamischen Layouts, die an verschiedene Bildschirmgrößen angepasst werden können, finden Sie unter [Definieren von Seitenlayouts mit XAML](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml).
