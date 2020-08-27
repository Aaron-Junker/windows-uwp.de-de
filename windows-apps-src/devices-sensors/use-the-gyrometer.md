---
ms.assetid: 454953E1-DD8F-44B7-A614-7BAD8C683536
title: Verwenden des Gyrometers
description: Erfahren Sie, wie Sie die Gyrometer-API verwenden, um die Gyrometer-Eingabe in Ihre APP zu integrieren, die Änderungen an der Benutzer Bewegung erkennt, z. b. Winkelgeschwindigkeit und Bewegung
ms.date: 06/06/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 10d65a6b752c904d7145c3c5e09194dab7c8e0a5
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943090"
---
# <a name="use-the-gyrometer"></a>Verwenden des Gyrometers


**Wichtige APIs**

-   [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)
-   [**Gyrometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer)

**Beispiel**

-   Eine umfassendere Implementierung finden Sie im Beispiel für das [Gyrometer-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/gyrometer).

Hier erfahren Sie, wie Sie mithilfe des Gyrometers Bewegungsänderungen des Benutzers erkennen.

Gyrometer und Beschleunigungssensoren ergänzen sich gegenseitig als Spielecontroller. Mit dem Beschleunigungsmesser können Sie die Bewegung in eine Richtung messen, während das Gyrometer die Winkelgeschwindigkeit (oder Drehbewegungen) misst.

## <a name="prerequisites"></a>Voraussetzungen

Sie sollten mit XAML (Extensible Application Markup Language), Microsoft Visual C# und Ereignissen vertraut sein.

Das verwendete Gerät oder der Emulator muss ein Gyrometer unterstützen.

## <a name="create-a-simple-gyrometer-app"></a>Erstellen einer einfachen Gyrometer-App

Dieser Abschnitt ist in zwei Unterabschnitte unterteilt: Der erste Unterabschnitt enthält die Schritte zum Erstellen einer einfachen Gyrometeranwendung. Im zweiten Unterabschnitt wird die erstellte App dann näher erläutert.

###  <a name="instructions"></a>Instructions

-   Erstellen Sie ein neues Projekt. Wählen Sie dabei unter den Projektvorlagen für **Visual C#** die Option **Leere App (Universelle Windows-App)** aus.

-   Öffnen Sie die Projektdatei „MainPage.xaml.cs“, und ersetzen Sie den vorhandenen Code durch den folgenden Code.

```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Windows.Foundation;
    using Windows.Foundation.Collections;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Controls.Primitives;
    using Windows.UI.Xaml.Data;
    using Windows.UI.Xaml.Input;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;

    using Windows.UI.Core; // Required to access the core dispatcher object
    using Windows.Devices.Sensors; // Required to access the sensor platform and the gyrometer


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Gyrometer _gyrometer; // Our app' s gyrometer object

            // This event handler writes the current gyrometer reading to
            // the three textblocks on the app' s main page.

            private async void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    GyrometerReading reading = e.Reading;
                    txtXAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityX);
                    txtYAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityY);
                    txtZAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityZ);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _gyrometer = Gyrometer.GetDefault(); // Get the default gyrometer sensor object

                if (_gyrometer != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _gyrometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _gyrometer.ReportInterval = reportInterval;

                    // Assign an event handler for the gyrometer reading-changed event
                    _gyrometer.ReadingChanged += new TypedEventHandler<Gyrometer, GyrometerReadingChangedEventArgs>(ReadingChanged);
                }

            }
        }
    }
```

Sie müssen den Namespace im vorhergehenden Codeausschnitt durch den Namen ersetzen, den Sie für Ihr Projekt angegeben haben. Wenn Sie etwa ein Projekt mit dem Namen **GyrometerCS** erstellt haben, ersetzen Sie `namespace App1` durch `namespace GyrometerCS`.

-   Öffnen Sie die Datei „MainPage.xaml“, und ersetzen Sie den ursprünglichen Inhalt durch den folgenden XML-Code.

```xml
        <Page
        x:Class="App1.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="#FF0C0C0C">
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,8,0,0" TextWrapping="Wrap" Text="X-Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFDFDFD"/>
            <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="23" Margin="67,8,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="88" Foreground="#FFFDFAFA"/>
            <TextBlock HorizontalAlignment="Left" Height="20" Margin="8,52,0,0" TextWrapping="Wrap" Text="Y Axis:" VerticalAlignment="Top" Width="46" Foreground="White"/>
            <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="24" Margin="54,48,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="80" Foreground="#FFFBFBFB"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="8,93,0,0" TextWrapping="Wrap" Text="Z Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFEFBFB"/>
            <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="21" Margin="54,93,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="63" Foreground="#FFF8F3F3"/>

        </Grid>
    </Page>
```

Sie müssen den ersten Teil des Klassennamens im vorhergehenden Codeausschnitt durch den Namespace Ihrer App ersetzen. Wenn Sie etwa ein Projekt mit dem Namen **GyrometerCS** erstellt haben, ersetzen Sie `x:Class="App1.MainPage"` durch `x:Class="GyrometerCS.MainPage"`. Ersetzen Sie außerdem `xmlns:local="using:App1"` durch `xmlns:local="using:GyrometerCS"`.

-   Drücken Sie F5, **oder wählen Sie**  >  **Debuggen Debugging starten** aus, um die APP zu erstellen, bereitzustellen und auszuführen

Wenn die App ausgeführt wird, können Sie die Gyrometerwerte ändern, indem Sie das Gerät bewegen oder die Emulatortools verwenden.

-   Halten Sie die APP an, indem Sie zu Visual Studio zurückkehren und UMSCHALT + F5 drücken, **oder wählen Sie**  >  **Debuggen Debuggen** , um die APP

###  <a name="explanation"></a>Erklärung

Das vorherige Beispiel zeigt, wie wenig Code Sie schreiben müssen, um Gyrometerangaben in Ihre App zu integrieren.

Die App stellt eine Verbindung mit dem Standardgyrometer in der **MainPage**-Methode her.

```csharp
_gyrometer = Gyrometer.GetDefault(); // Get the default gyrometer sensor object
```

Die App legt das Berichtsintervall in der **MainPage**-Methode fest. Mit diesem Code wird das vom Gerät unterstützte Mindestintervall abgerufen und mit einem angeforderten Intervall von 16 Millisekunden verglichen (entspricht etwa einer Aktualisierungsrate von 60 Hz). Wenn das unterstützte Mindestintervall größer als das angeforderte Intervall ist, legt der Code den Wert auf das Minimum fest. Andernfalls wird der Wert auf das angeforderte Intervall festgelegt.

```csharp
uint minReportInterval = _gyrometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_gyrometer.ReportInterval = reportInterval;
```

Die neuen Gyrometerdaten werden in der **ReadingChanged**-Methode erfasst. Wenn der Sensortreiber neue Daten vom Sensor empfängt, übergibt er die Werte mithilfe dieses Ereignishandlers an Ihre App. Die App registriert diesen Ereignishandler in der folgenden Zeile.

```csharp
_gyrometer.ReadingChanged += new TypedEventHandler<Gyrometer,
GyrometerReadingChangedEventArgs>(ReadingChanged);
```

Die neuen Werte werden in die TextBlock-Elemente des XAML-Projektcodes geschrieben.

```xml
        <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,8,0,0" TextWrapping="Wrap" Text="X-Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFDFDFD"/>
        <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="23" Margin="67,8,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="88" Foreground="#FFFDFAFA"/>
        <TextBlock HorizontalAlignment="Left" Height="20" Margin="8,52,0,0" TextWrapping="Wrap" Text="Y Axis:" VerticalAlignment="Top" Width="46" Foreground="White"/>
        <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="24" Margin="54,48,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="80" Foreground="#FFFBFBFB"/>
        <TextBlock HorizontalAlignment="Left" Height="21" Margin="8,93,0,0" TextWrapping="Wrap" Text="Z Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFEFBFB"/>
        <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="21" Margin="54,93,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="63" Foreground="#FFF8F3F3"/>
```

 ## <a name="related-topics"></a>Zugehörige Themen

* [Gyrometerbeispiel](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Gyrometer%20sensor%20sample%20(Windows%208))
