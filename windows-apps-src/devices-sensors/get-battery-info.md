---
ms.assetid: 90BB59FC-90FE-453E-A8DE-9315E29EB98C
title: Abrufen von Akkuinformationen
description: Erfahren Sie, wie Sie mithilfe von APIs im Windows.Devices.Power-Namespace ausführliche Akkuinformationen erhalten
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 383cb37e044e7e13080526d8e8270e7fc36560ff
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172234"
---
# <a name="get-battery-information"></a>Abrufen von Akkuinformationen


** Wichtige APIs **

-   [**Windows.Devices.Power**](/uwp/api/Windows.Devices.Power)
-   [**DeviceInformation.FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)

Erfahren Sie, wie Sie ausführliche Akku Informationen mithilfe von APIs im [**Windows. Devices. Power**](/uwp/api/Windows.Devices.Power) -Namespace erhalten. In einem *Akkubericht* ([**BatteryReport**](/uwp/api/Windows.Devices.Power.BatteryReport)) werden der Ladezustand, die Kapazität und der Status eines Akkus oder Akkuaggregats beschrieben. In diesem Thema wird veranschaulicht, wie Ihre App Akkuberichte abrufen und über Veränderungen informiert werden kann. Die Codebeispiele stammen aus der einfachen Akku-App, die am Ende dieses Themas angegeben wird.

## <a name="get-aggregate-battery-report"></a>Abrufen eines zusammengefassten Akkuberichts


Einige Geräte verfügen über mehr als einen Akku, und es ist nicht immer eindeutig, in welcher Form die einzelnen Akkus zur gesamten Energiekapazität des Geräts beitragen. An dieser Stelle wird die [**AggregateBattery**](/uwp/api/windows.devices.power.battery.aggregatebattery)-Klasse verwendet. *AggregateBattery* repräsentiert alle Akkucontroller, die mit dem Gerät verbunden sind. Damit kann ein zusammengefasstes [**BatteryReport**](/uwp/api/Windows.Devices.Power.BatteryReport)-Objekt bereitgestellt werden.

**Hinweis**    Eine [**Akku**](/uwp/api/Windows.Devices.Power.Battery) Klasse entspricht tatsächlich einem Akku Controller. Je nach Gerät kann der Controller auch am physischen Akku angeschlossen sein, und es kommt auch vor, dass er am Gehäuse des Geräts angeschlossen ist. So kann auch dann ein Akkuobjekt erstellt werden, wenn keine Akkus vorhanden sind. In anderen Fällen kann für das Akkuobjekt **null** gelten.

Wenn Sie über ein zusammengefasstes Akkuobjekt verfügen, rufen Sie [**GetReport**](/uwp/api/windows.devices.power.battery.getreport) auf, um den entsprechenden [**BatteryReport**](/uwp/api/Windows.Devices.Power.BatteryReport) abzurufen.

```csharp
private void RequestAggregateBatteryReport()
{
    // Create aggregate battery object
    var aggBattery = Battery.AggregateBattery;

    // Get report
    var report = aggBattery.GetReport();

    // Update UI
    AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
}
```

## <a name="get-individual-battery-reports"></a>Abrufen von individuellen Akkuberichten

Sie können auch ein [**BatteryReport**](/uwp/api/Windows.Devices.Power.BatteryReport)-Objekt für individuelle Akkus erstellen. Verwenden Sie [**GetDeviceSelector**](/uwp/api/windows.devices.power.battery.getdeviceselector) mit der [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)-Methode zum Abrufen einer Sammlung mit [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekten, die alle mit dem Gerät verbundenen Akkucontroller repräsentieren. Erstellen Sie anschließend mit der **Id**-Eigenschaft des gewünschten **DeviceInformation**-Objekts ein entsprechendes [**Battery**](/uwp/api/Windows.Devices.Power.Battery)-Element mit der Methode [**FromIdAsync**](/uwp/api/windows.devices.power.battery.fromidasync). Rufen Sie zuletzt [**GetReport**](/uwp/api/windows.devices.power.battery.getreport) auf, um den individuellen Akkubericht abzurufen.

Dieses Beispiel zeigt, wie Sie einen Akkubericht für alle Akkus erstellen, die mit dem Gerät verbunden sind.

```csharp
async private void RequestIndividualBatteryReports()
{
    // Find batteries 
    var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
    foreach(DeviceInformation device in deviceInfo)
    {
        try
        {
        // Create battery object
        var battery = await Battery.FromIdAsync(device.Id);

        // Get report
        var report = battery.GetReport();

        // Update UI
        AddReportUI(BatteryReportPanel, report, battery.DeviceId);
        }
        catch { /* Add error handling, as applicable */ }
    }
}
```

## <a name="access-report-details"></a>Zugreifen auf Berichtdetails

Das [**BatteryReport**](/uwp/api/Windows.Devices.Power.BatteryReport)-Objekt liefert zahlreiche Akkuinformationen. Weitere Informationen finden Sie in der API-Referenz für die Eigenschaften: **Status** (eine [**BatteryStatus**](/previous-versions/windows/dn818458(v=win.10))-Enumerierung), [**ChargeRateInMilliwatts**](/uwp/api/windows.devices.power.batteryreport.chargerateinmilliwatts), [**DesignCapacityInMilliwattHours**](/uwp/api/windows.devices.power.batteryreport.designcapacityinmilliwatthours), [**FullChargeCapacityInMilliwattHours**](/uwp/api/windows.devices.power.batteryreport.fullchargecapacityinmilliwatthours) und [**RemainingCapacityInMilliwattHours**](/uwp/api/windows.devices.power.batteryreport.remainingcapacityinmilliwatthours). Dieses Beispiel enthält einige Eigenschaften des Akkuberichts, die von der einfachen Akku-App verwendet werden. Die App wird weiter unten in diesem Thema bereitgestellt.

```csharp
...
TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };
...
...
```

## <a name="request-report-updates"></a>Anfordern von Berichtsaktualisierungen

Das [**Battery**](/uwp/api/Windows.Devices.Power.Battery)-Objekt löst das [**ReportUpdated**](/uwp/api/windows.devices.power.battery.reportupdated)-Ereignis aus, wenn sich Ladezustand, Kapazität oder Status des Akkus ändern. Dies geschieht für Statusänderungen in der Regel sofort, und für alle anderen Änderungen in regelmäßigen Abständen. Dieses Beispiel zeigt, wie Sie die Registrierung für Aktualisierungen des Akkuberichts durchführen.

```csharp
...
Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
...
```

## <a name="handle-report-updates"></a>Behandeln von Berichtsaktualisierungen

Bei einer Aktualisierung des Akkuberichts übergibt das [**ReportUpdated**](/uwp/api/windows.devices.power.battery.reportupdated)-Ereignis das entsprechende [**Battery**](/uwp/api/Windows.Devices.Power.Battery)-Objekt an die Ereignishandlermethode. Dieser Ereignishandler wird jedoch nicht über den UI-Thread aufgerufen. Sie müssen das [**Dispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)-Objekt verwenden, um UI-Änderungen aufzurufen. Dies wird im folgenden Beispiel gezeigt.

```csharp
async private void AggregateBattery_ReportUpdated(Battery sender, object args)
{
    if (reportRequested)
    {

        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }
        });
    }
}
```

## <a name="example-basic-battery-app"></a>Beispiel: einfache Akku-App

Testen Sie diese APIs, indem Sie die folgende einfache Akku-App in Microsoft Visual Studio erstellen. Klicken Sie auf der Startseite von Visual Studio auf **Neues Projekt**, und erstellen Sie dann im Bereich mit den Vorlagen unter **Visual C# &gt; Windows &gt; Universal** eine neue App, indem Sie die Vorlage **Leere App** verwenden.

Öffnen Sie als Nächstes die Datei **MainPage.xaml**, und kopieren Sie den folgenden XML-Code in diese Datei, sodass der Originalinhalt ersetzt wird.

```xml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" >
        <StackPanel VerticalAlignment="Center" Margin="15,30,0,0" >
            <RadioButton x:Name="AggregateButton" Content="Aggregate results" GroupName="Type" IsChecked="True" />
            <RadioButton x:Name="IndividualButton" Content="Individual results" GroupName="Type" IsChecked="False" />
        </StackPanel>
        <StackPanel Orientation="Horizontal">
        <Button x:Name="GetBatteryReportButton" 
                Content="Get battery report" 
                Margin="15,15,0,0" 
                Click="GetBatteryReport"/>
        </StackPanel>
        <StackPanel x:Name="BatteryReportPanel" Margin="15,15,0,0"/>
    </StackPanel>
</Page>
```

Wenn Ihre App nicht den Namen **App1** hat, müssen Sie den ersten Teil des Klassennamens im vorhergehenden Codeausschnitt durch den Namespace Ihrer App ersetzen. Wenn Sie z. B. ein Projekt mit dem Namen **BasicBatteryApp** erstellt haben, ersetzen Sie `x:Class="App1.MainPage"` durch `x:Class="BasicBatteryApp.MainPage"`. Ersetzen Sie außerdem `xmlns:local="using:App1"` durch `xmlns:local="using:BasicBatteryApp"`.

Öffnen Sie als Nächstes die Datei **MainPage.xaml.cs** des Projekts, und ersetzen Sie den vorhandenen Code durch Folgendes.

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Devices.Enumeration;
using Windows.Devices.Power;
using Windows.UI.Core;

namespace App1
{
    public sealed partial class MainPage : Page
    {
        bool reportRequested = false;
        public MainPage()
        {
            this.InitializeComponent();
            Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
        }


        private void GetBatteryReport(object sender, RoutedEventArgs e)
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }

            // Note request
            reportRequested = true;
        }

        private void RequestAggregateBatteryReport()
        {
            // Create aggregate battery object
            var aggBattery = Battery.AggregateBattery;

            // Get report
            var report = aggBattery.GetReport();

            // Update UI
            AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
        }

        async private void RequestIndividualBatteryReports()
        {
            // Find batteries 
            var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
            foreach(DeviceInformation device in deviceInfo)
            {
                try
                {
                // Create battery object
                var battery = await Battery.FromIdAsync(device.Id);

                // Get report
                var report = battery.GetReport();

                // Update UI
                AddReportUI(BatteryReportPanel, report, battery.DeviceId);
                }
                catch { /* Add error handling, as applicable */ }
            }
        }


        private void AddReportUI(StackPanel sp, BatteryReport report, string DeviceID)
        {
            // Create battery report UI
            TextBlock txt1 = new TextBlock { Text = "Device ID: " + DeviceID };
            txt1.FontSize = 15;
            txt1.Margin = new Thickness(0, 15, 0, 0);
            txt1.TextWrapping = TextWrapping.WrapWholeWords;

            TextBlock txt2 = new TextBlock { Text = "Battery status: " + report.Status.ToString() };
            txt2.FontStyle = Windows.UI.Text.FontStyle.Italic;
            txt2.Margin = new Thickness(0, 0, 0, 15);

            TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
            TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
            TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
            TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };

            // Create energy capacity progress bar & labels
            TextBlock pbLabel = new TextBlock { Text = "Percent remaining energy capacity" };
            pbLabel.Margin = new Thickness(0,10, 0, 5);
            pbLabel.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            ProgressBar pb = new ProgressBar();
            pb.Margin = new Thickness(0, 5, 0, 0);
            pb.Width = 200;
            pb.Height = 10;
            pb.IsIndeterminate = false;
            pb.HorizontalAlignment = HorizontalAlignment.Left;

            TextBlock pbPercent = new TextBlock();
            pbPercent.Margin = new Thickness(0, 5, 0, 10);
            pbPercent.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            // Disable progress bar if values are null
            if ((report.FullChargeCapacityInMilliwattHours == null)||
                (report.RemainingCapacityInMilliwattHours == null))
            {
                pb.IsEnabled = false;
                pbPercent.Text = "N/A";
            }
            else
            {
                pb.IsEnabled = true;
                pb.Maximum = Convert.ToDouble(report.FullChargeCapacityInMilliwattHours);
                pb.Value = Convert.ToDouble(report.RemainingCapacityInMilliwattHours);
                pbPercent.Text = ((pb.Value / pb.Maximum) * 100).ToString("F2") + "%";
            }

            // Add controls to stackpanel
            sp.Children.Add(txt1);
            sp.Children.Add(txt2);
            sp.Children.Add(txt3);
            sp.Children.Add(txt4);
            sp.Children.Add(txt5);
            sp.Children.Add(txt6);
            sp.Children.Add(pbLabel);
            sp.Children.Add(pb);
            sp.Children.Add(pbPercent);
        }

        async private void AggregateBattery_ReportUpdated(Battery sender, object args)
        {
            if (reportRequested)
            {

                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    // Clear UI
                    BatteryReportPanel.Children.Clear();


                    if (AggregateButton.IsChecked == true)
                    {
                        // Request aggregate battery report
                        RequestAggregateBatteryReport();
                    }
                    else
                    {
                        // Request individual battery report
                        RequestIndividualBatteryReports();
                    }
                });
            }
        }
    }
}
```

Wenn Ihre App nicht den Namen **App1** hat, müssen Sie den Namespace im vorherigen Beispiel durch den Namen ersetzen, den Sie für Ihr Projekt vergeben haben. Wenn Sie z. B. ein Projekt mit dem Namen **BasicBatteryApp** erstellt haben, ersetzen Sie den Namespace `App1` durch den Namespace `BasicBatteryApp`.

Führen Sie schließlich Folgendes aus, um diese einfache Akku-App auszuführen: Klicken Sie im Menü **Debuggen** auf **Debuggen starten**, um die Projektmappe zu testen.

**Tipp**    Um numerische Werte aus dem Objekt " [**batteryreport**](/uwp/api/Windows.Devices.Power.BatteryReport) " zu erhalten, Debuggen Sie die APP auf dem **lokalen Computer** oder einem externen **Gerät** (z. b. einem Windows Phone). Beim Debuggen mit einem Geräteemulator gibt das **BatteryReport**-Objekt für die Kapazitäts- und Rateneigenschaften **null** zurück.

 