---
description: Dieses Thema enthält eine umfassende Zuordnung der Windows Phone Silverlight-APIs zu ihren Entsprechungen in der universellen Windows-Plattform (UWP).
title: WPSL-zu-UWP-Namespace und Klassen Zuordnungen
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ab6bdace41041f03bd4316b1f65de1a5aeb60835
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167494"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone von Silverlight zu UWP-API-Zuordnungen


Dieses Thema enthält eine umfassende Zuordnung der Windows Phone Silverlight-APIs zu ihren Entsprechungen in der universellen Windows-Plattform (UWP). Im Allgemeinen erfolgt keine 1: 1-Zuordnung von Funktionen, jedoch gilt: Jede Plattform kann ggf. mehr oder weniger Funktionalität bieten als ihr Gegenstück in einem Namespace oder einer Klasse.

Die Zuordnungstabelle ist hilfreich, wenn Sie in einem UWP-Projekt arbeiten und Quellcode aus einem Windows Phone Silverlight-Projekt erneut verwenden. Zwischen den beiden Plattformen gibt es Unterschiede bei den Namen von Namespaces und Klassen (einschließlich UI-Steuerelemente). In vielen Fällen ist es einfach: Sie ändern z. B. einen Namespacenamen, und der Code wird kompiliert Manchmal wird neben dem Namespacenamen auch der Name einer Klasse oder API geändert In anderen Fällen ist die Zuordnung etwas schwieriger. In seltenen Fällen muss der Ansatz geändert werden.

**Verwenden der Tabelle:  ** Suchen Sie zuerst nach dem Namen der Klasse, die Sie verwenden. Klassen werden aufgelistet, wenn es sich um eine kompliziertere Zuordnung als eine Änderung des Namespacenamens handelt. Wenn Ihre Klasse nicht aufgeführt ist, handelt es sich bei der Zuordnung nur um eine Namespaceänderung. Wenn Sie den Namespacenamen Ihrer Klasse finden, finden Sie auch den entsprechenden Namen des UWP-Namespaces. Ihre Klasse ist in diesem Namespace enthalten. Wenn der Namespace nicht aufgeführt ist, hat sich dessen Name nicht geändert.

**Hinweis**    Windows 10 unterstützt viel mehr .NET Framework als eine Windows Phone Store-App. Windows 10 hat z. b. mehrere System. Service Model. \* Namespaces sowie System.net, System .net. NetworkInformation und System .net. Sockets.
Außerdem profitieren Sie in einer Windows 10-App von .NET Native. Dabei handelt es sich um eine fortschrittliche Kompilierungstechnologie, mit der MSIL-Code in Computercode für die systemeigene Ausführung konvertiert wird. .NET Native-Apps starten schneller, verbrauchen weniger Arbeitsspeicher und benötigen weniger Akkuenergie als ihre MSIL-Gegenstücke.

| Windows Phone Silverlight | Windows-Runtime |
| ------------------------- | --------------- |
| Advertising | |
| **Microsoft.Advertising.Mobile.UI.AdControl**-Klasse | [AdControl](../monetize/display-ads-in-your-app.md)-Klasse |
| Alarme, Erinnerungen und Hintergrund-Agents | |
| **Microsoft.Phone.BackgroundAgent**-Klasse | [**Backgroundtaskbuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) -Klasse |
| **Microsoft.Phone.Scheduler**-Namespace | [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background)-Namespace |
| **Microsoft.Phone.Scheduler.Alarm** Klasse | [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)- und [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager)-Klassen |
| **Microsoft.Phone.Scheduler.PeriodicTask**-, **ScheduledAction**-, **ScheduledActionService**-, **ScheduledTask**- und **ScheduledTaskAgent**-Klassen | [**Backgroundtaskbuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) -Klasse |
| **Microsoft.Phone.Scheduler.Reminder** Klasse | [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)- und [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager)-Klassen |
| **Microsoft.Phone.PictureDecoder** Klasse | [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder)-Klasse |
| **Microsoft.Phone.BackgroundAudio**-Namespace | [**Windows.Media.Playback**](/uwp/api/Windows.Media.Playback)-Namespace |
| **Microsoft.Phone.BackgroundTransfer**-Namespace | [**Windows.Networking.BackgroundTransfer**](/uwp/api/Windows.Networking.BackgroundTransfer) Namespace |
| App-Modell und -Umgebung | |
| **System.AppDomain**-Klasse | Keine direkte Entsprechung. Siehe [**Application**](/uwp/api/Windows.UI.Xaml.Application), [**CoreApplication**](/uwp/api/Windows.ApplicationModel.Core.CoreApplication), Klassen |
| **System.Environment** Klasse | Keine direkte Entsprechung |
| **System.ComponentModel.Annotations** Klasse  | Keine direkte Entsprechung |
| **System.ComponentModel.BackgroundWorker** Klasse | [**ThreadPool**](/uwp/api/Windows.System.Threading.ThreadPool)-Klasse |
| **System.ComponentModel.DesignerProperties**-Klasse | [**DesignMode**](/uwp/api/Windows.ApplicationModel.DesignMode)-Klasse |
| **System.Threading.Thread**-, **System.Threading.ThreadPool**-Klassen | [**ThreadPool**](/uwp/api/Windows.System.Threading.ThreadPool)-Klasse |
| (ST = **System.Threading**) <br/> **ST.Thread.MemoryBarrier**-Methode | (ST = **System.Threading**) <br/> **ST.Interlocked.MemoryBarrier**-Methode |
| (ST = **System.Threading**) <br/> **ST.Thread.ManagedThreadId**-Eigenschaft | (S = **System**) <br/> **S.Environment.ManagedThreadId**-Eigenschaft |
| **System.Threading.Timer**-Klasse | [**ThreadPoolTimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer) Klasse |
| (SWT = **System.Windows.Threading**) <br/> **SWT.Dispatcher**-Klasse | [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) Klasse |
| (SWT = **System.Windows.Threading**) <br/> **SWT.DispatcherTimer** Klasse | [**DispatcherTimer**](/uwp/api/Windows.UI.Xaml.DispatcherTimer)-Klasse |
| Blend for Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> **MEDC.GeometryHelper**-Klasse | Keine direkte Entsprechung |
| **Microsoft.Expression.Interactivity** Namespace | [Microsoft.Xaml.Interactivity](/previous-versions/dn458193(v=vs.120))-Namespace |
| **Microsoft.Expression.Interactivity.Core** Namespace | [Microsoft.Xaml.Interactions.Core](/previous-versions/dn458182(v=vs.120)) Namespace |
| (Meic = **Microsoft. Expression. interactivity. Core**) <br/> **MEIC.ExtendedVisualStateManager** Klasse | Keine direkte Entsprechung |
| **Microsoft.Expression.Interactivity.Input** Namespace | Keine direkte Entsprechung |
| **Microsoft.Expression.Interactivity.Media**-Namespace | [Microsoft.Xaml.Interactions.Media](/previous-versions/dn458186(v=vs.120)) Namespace |
| **Microsoft.Expression.Shapes** Namespace | Keine direkte Entsprechung |
| (MI = **Microsoft.Internal**) <br/> **MI.IManagedFrameworkInternalHelper** Schnittstelle | Keine direkte Entsprechung |
| Kontakt- und Kalenderdaten | |
| **Microsoft.Phone.UserData**-Namespace | [**Windows.ApplicationModel.Contacts**](/uwp/api/Windows.ApplicationModel.Contacts)-, [**Windows.ApplicationModel.Appointments**](/uwp/api/Windows.ApplicationModel.Appointments)-Namespaces |
| (MPU = **Microsoft. Phone. UserData**) <br/> **MPU.Account**-, **ContactAddress**-, **ContactCompanyInformation**-, **ContactEmailAddress**-, **ContactPhoneNumber** Klassen | [**Contact**](/uwp/api/Windows.ApplicationModel.Contacts.Contact) Klasse |
| (MPU = **Microsoft. Phone. UserData**) <br/> **MPU.Appointments**-Klasse | [**AppointmentCalendar**](/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar)-Klasse |
| (MPU = **Microsoft. Phone. UserData**) <br/> **MPU.Contacts** Klasse | [**ContactStore**](/uwp/api/Windows.ApplicationModel.Contacts.ContactStore) Klasse |
| Steuerelemente und UI-Infrastruktur | |
| **ControlTiltEffect.TiltEffect** Klasse | Animationen aus der Windows-Runtime-Animationsbibliothek sind in die Standardstile der allgemeinen Steuerelemente integriert. Siehe [Animation](wpsl-to-uwp-porting-xaml-and-ui.md). |
| **Microsoft.Phone.Controls** Namespace | [**Windows. UI. XAML. Controls**](/uwp/api/Windows.UI.Xaml.Controls) -Namespace |
| (MPC = **Microsoft. Phone. Controls**) <br/> **MPC.ContextMenu**-Klasse | [**PopupMenu**](/uwp/api/Windows.UI.Popups.PopupMenu) Klasse |
| (MPC = **Microsoft. Phone. Controls**) <br/>**MPC.DatePickerPage**-Klasse | [**DatePickerFlyout**](/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout)-Klasse |
| (MPC = **Microsoft. Phone. Controls**) <br/>**MPC.GestureListener**-Klasse | [**GestureRecognizer**](/uwp/api/Windows.UI.Input.GestureRecognizer) Klasse |
| (MPC = **Microsoft. Phone. Controls**) <br/>**MPC.LongListSelector**-Klasse | [**Semanticzoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) -Klasse |
| (MPC = **Microsoft. Phone. Controls**) <br/>**MPC.ObscuredEventArgs** Klasse | [**SystemProtection**](/uwp/api/Windows.Phone.System.SystemProtection)-, [**WindowActivatedEventArgs**](/uwp/api/Windows.UI.Core.WindowActivatedEventArgs)-Klassen |
| (MPC = **Microsoft. Phone. Controls**) <br/>**MPC.Panorama** Klasse | [**Hub**](/uwp/api/Windows.UI.Xaml.Controls.Hub) -Klasse |
| (MPC = **Microsoft. Phone. Controls**) <br/>**MPC.PhoneApplicationFrame**-,<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService**-Klassen | [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) Klasse |
| (MPC = **Microsoft. Phone. Controls**) <br/>**MPC.PhoneApplicationPage**-Klasse | [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page) Klasse |
| (MPC = **Microsoft. Phone. Controls**) <br/>**MPC.TiltEffect**-Klasse | [**Pointerdownpoinmeanimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) -Klasse |
| (MPC = **Microsoft. Phone. Controls**) <br/>**MPC.TimePickerPage** Klasse | [**TimePickerFlyout**](/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout)-Klasse |
| (MPC = **Microsoft. Phone. Controls**) <br/>**MPC.WebBrowser**-Klasse | [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView) -Klasse |
| (MPC = **Microsoft. Phone. Controls**) <br/>**MPC.WebBrowserExtensions**-Klasse | Keine direkte Entsprechung |
| (MPC = **Microsoft. Phone. Controls**) <br/>**MPC.WrapPanel**-Klasse | Keine direkte Entsprechung für das allgemeine Layout. [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) und [**WrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) können in der ItemsPanel-Vorlage eines Elementsteuerelements verwendet werden. |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq**-Namespace | Keine direkte Entsprechung |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq.Mapping** Namespace | Keine direkte Entsprechung |
| **Microsoft.Phone.Globalization** Namespace | Keine direkte Entsprechung |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.DeviceExtendedProperties**-, **DeviceStatus**-Klassen | [**EasClientDeviceInformation**](/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation)-, [**MemoryManager**](/uwp/api/Windows.System.MemoryManager)-Klassen. Weitere Infos finden Sie unter [Gerätestatus](wpsl-to-uwp-input-and-sensors.md). |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.MediaCapabilities** Klasse | Keine direkte Entsprechung |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.UserExtendedProperties** Klasse | [**AdvertisingManager**](/uwp/api/Windows.System.UserProfile.AdvertisingManager)-Klasse |
| **System.Windows**-Namespace | [**Windows.UI.Xaml**](/uwp/api/Windows.UI.Xaml)-Namespace |
| **System.Windows.Automation**-Namespace | [**Windows.UI.Xaml.Automation**](/uwp/api/Windows.UI.Xaml.Automation)-Namespace |
| **System.Windows.Controls**, **System.Windows.Input** Namespaces | [**Windows.UI.Core**](/uwp/api/Windows.UI.Core)-, [**Windows.UI.Input**](/uwp/api/Windows.UI.Input)-, [**Windows.UI.Xaml.Controls**](/uwp/api/Windows.UI.Xaml.Controls)-Namespaces |
| **System.Windows.Controls.DrawingSurface**-, **DrawingSurfaceBackgroundGrid**-Klassen | [**SwapChainPanel**](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)-Klasse |
| **System.Windows.Controls.RichTextBox** Klasse | [**RichEditBox**](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)-Klasse |
| **System.Windows.Controls.WrapPanel** Klasse | Keine direkte Entsprechung für das allgemeine Layout. [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) und [**WrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) können in der ItemsPanel-Vorlage eines Elementsteuerelements verwendet werden. |
| **System.Windows.Controls.Primitives** Namespace | [**Windows.UI.Xaml.Controls.Primitives**](/uwp/api/Windows.UI.Xaml.Controls.Primitives) Namespace |
| **System.Windows.Controls.Shapes**-Namespace | [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) Namespace |
| **System.Windows.Data**-Namespace | [**Windows.UI.Xaml.Data**](/uwp/api/Windows.UI.Xaml.Data) Namespace |
| **System.Windows.Documents**-Namespace | [**Windows.UI.Xaml.Documents**](/uwp/api/Windows.UI.Xaml.Documents)-Namespace |
| **System.Windows.Ink**-Namespace | Keine direkte Entsprechung |
| **System.Windows.Markup** Namespace | [**Windows.UI.Xaml.Markup**](/uwp/api/Windows.UI.Xaml.Markup) Namespace | 
| **System.Windows.Navigation** Namespace | [**Windows.UI.Xaml.Navigation**](/uwp/api/Windows.UI.Xaml.Navigation)-Namespace |
| **System.Windows.UIElement.Tap**-Ereignis, **EventHandler&lt;GestureEventArgs&gt;**-Delegat | [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped) Ereignis, [**TappedEventHandler**](/uwp/api/windows.ui.xaml.input.tappedeventhandler) Delegat |
| Daten und Dienste |  |
| **System.Data.Linq.DataContext** Klasse | Keine direkte Entsprechung |
| **System.Data.Linq.Mapping.ColumnAttribute**-Klasse | Keine direkte Entsprechung |
| **System.Data.Linq.SqlClient.SqlHelpers**-Klasse | Keine direkte Entsprechung |
| Geräte | |
| **Microsoft.Devices**-, **Microsoft.Devices.Sensors**-Namespaces | [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration), [**Windows.Devices.Enumeration.Pnp**](/uwp/api/Windows.Devices.Enumeration.Pnp), [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input), [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors) Namespaces |
| **Microsoft.Devices.Camera**-, **Microsoft.Devices.PhotoCamera**-Klassen | [**Mediacapture**](/uwp/api/Windows.Media.Capture.MediaCapture) -Klasse. Auch [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Klasse (nur Windows). |
| **Microsoft.Devices.CameraButtons**-Klasse | [**HardwareButtons**](/uwp/api/Windows.Phone.UI.Input.HardwareButtons)-Klasse |
| **Microsoft.Devices.CameraVideoBrushExtensions**-Klasse | [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement)-Klasse |
| **Microsoft.Devices.Environment**-Klasse | Keine direkte Entsprechung. Um dieses Problem zu umgehen, verwenden Sie die bedingte Kompilierung und definieren Sie ein benutzerdefiniertes Symbol. Unter Umständen können Sie das Problem auch mit der [IsAttached](/dotnet/api/system.diagnostics.debugger.isattached#System_Diagnostics_Debugger_IsAttached) Eigenschaft umgehen. |
| **Microsoft.Devices.MediaHistory**-Klasse | Keine direkte Entsprechung |
| **Microsoft.Devices.VibrateController**-Klasse | [**VibrationDevice**](/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice) Klasse |
| **Microsoft.Devices.Radio.FMRadio** Klasse | Keine direkte Entsprechung |
| **Microsoft.Devices.Sensors.Accelerometer**, **Compass** Klassen | Im [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)-Namespace |
| **Microsoft.Devices.Sensors.Gyroscope** Klasse | [**Gyrometer**](/uwp/api/Windows.Devices.Sensors.Gyrometer) Klasse |
| **Microsoft.Devices.Sensors.Motion**-Klasse | [**Inclinometer**](/uwp/api/Windows.Devices.Sensors.Inclinometer) Klasse |
| Globalisierung | |
| **System. Globalization** -Namespace | [**Windows. Globalization**](/uwp/api/Windows.Globalization) -Namespace |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentCulture**-Eigenschaft | (SG = **System. Globalization**) <br/> **S.CultureInfo.CurrentCulture**-Eigenschaft |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentUICulture** Eigenschaft | (SG = **System. Globalization**) <br/> **S.CultureInfo.CurrentUICulture**-Eigenschaft |
| Grafiken und Animationen | |
| **Microsoft. XNA. Framework. \* ** Namespaces, [XNA-Framework-Klassenbibliothek](/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)), [Inhalts Pipeline-Klassenbibliothek](/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)) | Keine direkte Entsprechung. Verwenden Sie im Allgemeinen [Microsoft DirectX](/windows/desktop/directx) mit C++. Siehe [Entwickeln von Spielen](/previous-versions/windows/apps/hh452744(v=win.10)) und [Interoperabilität von DirectX und XAML](/previous-versions/windows/apps/hh825871(v=win.10)). |
| **Microsoft.Xna.Framework.Audio.Microphone**-Klasse | [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture)-Klasse |
| **Microsoft.Xna.Framework.Audio.SoundEffect**-Klasse | [**Media Element**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) -Klasse |
| **Microsoft.Xna.Framework.GamerServices**-Namespace | (WPS = **Windows.Phone.System**) <br/> [**WPS.UserProfile.GameServices.Core**](/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core) Namespace |
| **Microsoft.Xna.Framework.GamerServices.Guide** Klasse | Keine direkte Entsprechung |
| **Microsoft.Xna.Framework.Input.GamePad**-Klasse | [**HardwareButtons**](/uwp/api/Windows.Phone.UI.Input.HardwareButtons)-Klasse |
| **Microsoft.Xna.Framework.Input.Touch.TouchPanel**-Klasse | [**GestureRecognizer**](/uwp/api/Windows.UI.Input.GestureRecognizer) Klasse |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> **MXFM.MediaLibrary**-, **MXFM.PhoneExtensions.MediaLibraryExtensions**-Klassen | [**KnownFolders**](/uwp/api/Windows.Storage.KnownFolders)-Klasse |
| **Microsoft.Xna.Framework.Media.MediaQueue** Klasse | [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls)-Klasse |
| **Microsoft.Xna.Framework.Media.Playlist**-Klasse | [**BackgroundMediaPlayer**](/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer) Klasse |
| **System.Windows.Media**-Namespace | [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media)-Namespace |
| **System.Windows.Media.RadialGradientBrush**-Klasse | Keine direkte Entsprechung. Siehe [Medien und Grafiken](wpsl-to-uwp-porting-xaml-and-ui.md). |
| **System.Windows.Media.Animation**-Namespace | [**Windows.UI.Xaml.Media.Animation**](/uwp/api/Windows.UI.Xaml.Media.Animation)-Namespace |
| **System.Windows.Media.Effects**-Namespace | Keine direkte Entsprechung |
| **System.Windows.Media.Imaging**-Namespace | [**Windows.UI.Xaml.Media.Imaging**](/uwp/api/Windows.UI.Xaml.Media.Imaging)-Namespace |
| **System.Windows.Media.Media3D** Namespace | [**Windows.UI.Xaml.Media.Media3D**](/uwp/api/Windows.UI.Xaml.Media.Media3D)-Namespace |
| **System.Windows.Shapes**-Namespace | [**Windows. UI. XAML. Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) -Namespace |
| Launcher und Chooser | |
| **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask**, **PhoneNumberChooserTask** Klassen | [**ContactPicker**](/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker)-Klasse |
| **Microsoft.Phone.Tasks.AddWalletItemTask**-, **AddWalletItemResult**-Klassen | [**Windows.ApplicationModel.Wallet**](/uwp/api/Windows.ApplicationModel.Wallet)-Namespace |
| **Microsoft.Phone.Tasks.BingMapsDirectionsTask**-, **BingMapsTask**-Klassen | Keine direkte Entsprechung |
| **Microsoft.Phone.Tasks.CameraCaptureTask**-Klasse | [**Mediacapture**](/uwp/api/Windows.Media.Capture.MediaCapture) -Klasse. Auch [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Klasse (nur Windows). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp)-Klasse ([**RequestAppPurchaseAsync**](/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync)-Methode) |
| **Microsoft.Phone.Tasks.ConnectionSettingsTask**-, **MarketplaceHubTask**-, **MarketplaceReviewTask**-, **MarketplaceSearchTask**-, **MediaPlayerLauncher**-, **SearchTask**-, **SmsComposeTask**-, **WebBrowserTask** Klassen | [**Launcher**](/uwp/api/Windows.System.Launcher)-Klasse |
| **Microsoft.Phone.Tasks.EmailComposeTask**-Klasse | [**EmailMessage**](/uwp/api/Windows.ApplicationModel.Email.EmailMessage) Klasse |
| **Microsoft.Phone.Tasks.GameInviteTask** Klasse | Keine direkte Entsprechung |
| **Microsoft.Phone.Tasks.MapDownloaderTask**-, **MapsDirectionsTask**-, **MapsTask**-, **MapUpdaterTask**-Klassen | Keine direkte Entsprechung |
| **Microsoft.Phone.Tasks.PhoneCallTask**-Klasse | [**PhoneCallManager**](/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager)-Klasse |
| **Microsoft.Phone.Tasks.PhotoChooserTask** Klasse | [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) Klasse |
| **Microsoft.Phone.Tasks.SaveAppointmentTask** Klasse | [**AppointmentManager**](/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager)-Klasse |
| **Microsoft.Phone.Tasks.SaveContactTask**-, **SaveEmailAddressTask**-, **SavePhoneNumberTask**-Klassen | [**StoredContact**](/uwp/api/Windows.Phone.PersonalInformation.StoredContact)-Klasse (nur Windows Phone) | 
| **Microsoft.Phone.Tasks.SaveRingtoneTask**-Klasse | Keine direkte Entsprechung |
| **Microsoft.Phone.Tasks.ShareLinkTask**-, **ShareMediaTask**-, **ShareStatusTask**-Klassen | [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage)-Klasse |
| Ort | |
| **System.Device.Location**-Namespace | [**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation) Namespace |
| **System.Device.GeoCoordinateWatcher**-Klasse | [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator)-Klasse |
| Karten | |
| **Microsoft.Phone.Maps** Namespaces | [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps)-Namespace |
| **Microsoft.Phone.Maps.Controls**-Namespace | [**Windows.UI.Xaml.Controls.Maps**](/uwp/api/Windows.UI.Xaml.Controls.Maps)-Namespace |
| **Microsoft.Phone.Maps.Controls.Map**-Klasse | [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)-Klasse |
| **Microsoft.Phone.Maps.Services**-Namespace | [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps)-Namespace |
| **Microsoft.Phone.Maps.Services.GeocodeQuery**-, **ReverseGeocodeQuery**-Klassen | [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder)-Klasse |
| **System.Device.Location.GeoCoordinate**-Klasse | [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint)-Klasse |
| **Microsoft.Phone.Maps.Services.Route** Klasse | [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute)-Klasse |
| **Microsoft.Phone.Maps.Services.RouteQuery** Klasse | [**MapRouteFinder**](/uwp/api/Windows.Services.Maps.MapRouteFinder) Klasse |
| Monetisierung | |
| **Microsoft.Phone.Marketplace** Namespace | [**Windows.ApplicationModel.Store**](/uwp/api/Windows.ApplicationModel.Store) Namespace |
| Medien | |
| **Microsoft.Phone.Media**-Namespace | [**Media Element**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) -Klasse |
| Netzwerk | |
| (Mpnn = **Microsoft. Phone .net. NetworkInformation**) <br/> **MPNN.DeviceNetworkInformation**-Klasse | [**Hostname**](/uwp/api/Windows.Networking.HostName)-, [**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation)-Klassen |
| (Mpnn = **Microsoft. Phone .net. NetworkInformation**) <br/> **MPNN.NetworkInterface**-Klasse | [**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation)-Klasse |
| (Mpnn = **Microsoft. Phone .net. NetworkInformation**) <br/> **MPNN.NetworkInterfaceInfo**-Klasse | [**ConnectionProfile**](/uwp/api/Windows.Networking.Connectivity.ConnectionProfile)-Klasse |
| (Mpnn = **Microsoft. Phone .net. NetworkInformation**) <br/> **MPNN.NetworkInterfaceList**-Klasse | [**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation)-Klasse |
| (Mpnn = **Microsoft. Phone .net. NetworkInformation**) <br/> **MPNN.SocketExtensions**-Klasse | Keine direkte Entsprechung |
| (Mpnn = **Microsoft. Phone .net. NetworkInformation**) <br/> **MPNN.WebRequestExtensions** Klasse | Keine direkte Entsprechung |
| **Microsoft.Phone.Networking.Voip**-Namespace | Keine direkte Entsprechung |
| **System.Net.CookieCollection**-Klasse | Wird noch unterstützt, aber einige Eigenschaften fehlen (z. B. IsReadOnly) |
| **System.Net.DownloadProgressChangedEventArgs**-Klasse und ähnliche Klassen in Verbindung mit **System.Net.WebClient** | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) Klasse (oder [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Ableitung von [System.Net.Http.StreamContent](/previous-versions/visualstudio/hh138119(v=vs.118)) zum Messen des Fortschritts |
| **System.Net.DnsEndPoint**-, **IPAddress**-Klassen | Diese Klassen werden zwar noch unterstützt, aber einige Eigenschaften fehlen. Alternativ dazu ist das Portieren zur [**HostName**](/uwp/api/Windows.Networking.HostName)-Klasse möglich. |
| **System.Net.HttpUtility** Klasse | [**HtmlFormatHelper**](/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper)-Klasse |
| **System.Net.HttpWebRequest**-Klasse | Wird teilweise unterstützt, aber die empfohlene fortschrittliche Alternative ist die [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient)-Klasse (oder [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Für diese APIs wird [System.Net.Http.HttpRequestMessage](/previous-versions/visualstudio/hh159020(v=vs.118)) verwendet, um eine HTTP-Anforderung darzustellen. |
| **System.Net.HttpWebResponse**-Klasse | Wird weiterhin unterstützt, aber anstelle von „Close()“ wird „Dispose()“ verwendet. Die empfohlene fortschrittliche Alternative ist jedoch die [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient)-Klasse (oder [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Für diese APIs wird [System.Net.Http.HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) verwendet, um eine HTTP-Antwort darzustellen. |
| (SNN = **System.Net.NetworkInformation**) <br/> **SNN.NetworkChange** Klasse | Wird weiterhin unterstützt, mit Ausnahme des Konstruktors |
| **System.Net.OpenReadCompletedEventArgs** Klasse und vergleichbare Klassen im Zusammenhang mit **System.Net.WebClient** | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) -Klasse (oder [System .net. http. HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.Sockets.Socket**-Klasse | Wird weiterhin unterstützt, aber anstelle von „Close()“ wird „Dispose()“ verwendet. Alternativ dazu ist das Portieren zur[**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket)-Klasse möglich. |
| **System.Net.Sockets.SocketException**-Klasse | Wird zwar noch unterstützt, aber anstelle von ErrorCode wird die SocketErrorCode-Eigenschaft verwendet. |
| **System.Net.Sockets.UdpAnySourceMulticastClient**-, **UdpSingleSourceMulticastClient**-Klassen | [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket)-Klasse |
| **System.Net.UploadProgressChangedEventArgs**-Klasse und ähnliche Klassen mit Bezug auf **System.Net.WebClient** | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) -Klasse (oder [System .net. http. HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebClient** Klasse | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) -Klasse (oder [System .net. http. HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebRequest** Klasse | Wird teilweise unterstützt (anderer Eigenschaftensatz), die empfohlene modernere Alternative ist jedoch die [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient)-Klasse (oder [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Für diese APIs wird [System.Net.Http.HttpRequestMessage](/previous-versions/visualstudio/hh159020(v=vs.118)) verwendet, um eine HTTP-Anforderung darzustellen. |
| **System.Net.WebResponse**-Klasse | Wird weiterhin unterstützt, aber anstelle von „Close()“ wird „Dispose()“ verwendet. Die empfohlene fortschrittliche Alternative ist jedoch die [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient)-Klasse (oder [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Für diese APIs wird [System.Net.Http.HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) verwendet, um eine HTTP-Antwort darzustellen. |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventArgs**-Klasse | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) -Klasse (oder [System .net. http. HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventHandler**-Klasse | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) -Klasse (oder [System .net. http. HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.UriFormatException** Klasse | **System.FormatException**-Klasse |
| Benachrichtigungen | |
| MPN = **Microsoft.Phone.Notification**-Namespace | [**Windows.UI.Notifications**](/uwp/api/Windows.UI.Notifications), [**Windows.Networking.PushNotifications**](/uwp/api/Windows.Networking.PushNotifications) Namespaces |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotification**-Klasse | [**TileNotification**](/uwp/api/Windows.UI.Notifications.TileNotification)-Klasse |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotificationChannel** Klasse | [**PushNotificationChannel**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel)-Klasse |
| Programmieren | |
| **System** -Namespace | [**Windows. Foundation**](/uwp/api/Windows.Foundation) -Namespace |
| **System.Diagnostics.StackFrame**-, **StackTrace**-Klassen | Keine direkte Entsprechung |
| **System.Diagnostics**-Namespace | [**Windows.Foundation.Diagnostics**](/uwp/api/Windows.Foundation.Diagnostics)-Namespace |
| **System.ICloneable**-Schnittstelle | Eine benutzerdefinierte Methode, mit der der passende Typ zurückgegeben wird. |
| **System.Reflection.Emit.ILGenerator** Klasse | Keine direkte Entsprechung |
| Reaktive Erweiterungen | |
| **Microsoft.Phone.Reactive**-Namespace | Keine direkte Entsprechung |
| Spiegelung | |
| **System.Type**-Klasse | **System.Reflection.TypeInfo**-Klasse. Weitere Informationen finden Sie unter [Reflektion im .NET Framework für UWP-apps](/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps). |
| Ressourcen | |
| **System.Resources.ResourceManager**-Klasse | (WA = **Windows. applicationmodel**)<br/>[**WA.Resources.Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) und [**WA.Resources**](/uwp/api/Windows.ApplicationModel.Resources) Namespaces, [**ResourceManager**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) Klasse. Weitere Informationen finden Sie [unter Erstellen und Abrufen von Ressourcen in Windows-Runtime-apps](/previous-versions/windows/apps/hh694557(v=vs.140)). |
| Sicheres Element | |
| (MPS = **Microsoft. Phone. secureelement**) <br/> **MPS.SecureElementChannel**, **MPS.SecureElementSession** Klassen | [**SmartCardConnection**](/uwp/api/Windows.Devices.SmartCards.SmartCardConnection)-Klasse |
| (MPS = **Microsoft. Phone. secureelement**) <br/> **MPS.SecureElementReader**-Klasse | [**SmartCardReader**](/uwp/api/Windows.Devices.SmartCards.SmartCardReader)-Klasse |
| Sicherheit | |
| (SSC = **System. Security. Cryptography**) <br/> **SSC.Aes**-, **SSC.RSA**-Klassen | [**CryptographicEngine**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) Klasse |
| (SSC = **System. Security. Cryptography**) <br/> **SSC.HMACSHA256**, **SSC.SHA256** Klassen | [**HashAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider)-Klasse |
| (SSC = **System. Security. Cryptography**) <br/> **SSC.ProtectedData**-Klasse | [**DataProtectionProvider**](/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) Klasse |
| (SSC = **System. Security. Cryptography**) <br/> **SSC.RandomNumberGenerator**-Klasse | [**CryptographicBuffer**](/uwp/api/Windows.Security.Cryptography.CryptographicBuffer)-Klasse |
| (SSC = **System. Security. Cryptography**) <br/> **SSC.X509Certificates.X509Certificate**-Klasse | [**CertificateEnrollmentManager**](/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager)-Klasse |
| Shell | |
| (MPSH = **Microsoft. Phone. Shell**) <br/> **MPSh.ApplicationBar**-Klasse | [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) -Klasse |
| (MPSH = **Microsoft. Phone. Shell**) <br/> **MPSh.ApplicationBarIconButton** Klasse | [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)-Klasse (bei Verwendung in der [**PrimaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands)-Eigenschaft) |
| (MPSH = **Microsoft. Phone. Shell**) <br/> **MPSh.ApplicationBarMenuItem**-Klasse | [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)-Klasse (bei Verwendung in der [**SecondaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands)-Eigenschaft) |
| (MPSH = **Microsoft. Phone. Shell**) <br/> **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData**, **MPSh.StandardTileData** Klassen | [**TileTemplateType**](/uwp/api/Windows.UI.Notifications.TileTemplateType)-Klasse |
| (MPSH = **Microsoft. Phone. Shell**) <br/> **MPSh.PhoneApplicationService**-Klasse | [**CoreApplication**](/uwp/api/Windows.ApplicationModel.Core.CoreApplication)-, [**DisplayRequest**](/uwp/api/Windows.System.Display.DisplayRequest)-Klassen |
| (MPSH = **Microsoft. Phone. Shell**) <br/> **MPSh.ProgressIndicator**-Klasse | [**StatusBarProgressIndicator**](/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator)-Klasse |
| (MPSH = **Microsoft. Phone. Shell**) <br/> **MPSh.ShellTile**-Klasse | [**SecondaryTile**](/uwp/api/Windows.UI.StartScreen.SecondaryTile)-Klasse |
| (MPSH = **Microsoft. Phone. Shell**) <br/> **MPSh.ShellTileSchedule** Klasse | [**TileUpdater**](/uwp/api/Windows.UI.Notifications.TileUpdater)-Klasse |
| (MPSH = **Microsoft. Phone. Shell**) <br/> **MPSh.ShellToast** Klasse | [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager) Klasse |
| (MPSH = **Microsoft. Phone. Shell**) <br/> **MPSh.SystemTray** Klasse | [**StatusBar**](/uwp/api/Windows.UI.ViewManagement.StatusBar)-Klasse |
| Speicher und E/A | |
| **Microsoft.Phone.Storage.ExternalStorage**-, **ExternalStorageDevice**-, **ExternalStorageFile**-, **ExternalStorageFolder**-Klassen | [**KnownFolders**](/uwp/api/Windows.Storage.KnownFolders)-Klasse |
| **System.IO**-Namespace | [**Windows.Storage**](/uwp/api/Windows.Storage), [**Windows.Storage.Streams**](/uwp/api/Windows.Storage.Streams) Namespaces |
| **System.IO.Directory**-Klasse | [**Storagefolder**](/uwp/api/Windows.Storage.StorageFolder) -Klasse |
| **System.IO.File**-Klasse | [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) und [**PathIO**](/uwp/api/Windows.Storage.PathIO) Klassen
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageFile** Klasse |[**ApplicationData.LocalFolder**](/uwp/api/windows.storage.applicationdata.localfolder)-Eigenschaft |
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageSettings**-Klasse | [**ApplicationData.LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings)-Eigenschaft |
| **System.IO.Stream**-Klasse | Wird noch unterstützt, aber anstelle von BeginRead()/EndRead() und BeginWrite()/EndWrite() wird ReadAsync() und WriteAsync() verwendet. |
| Wallet | |
| **Microsoft.Phone.Wallet**-Namespace | [**Windows.ApplicationModel.Wallet**](/uwp/api/Windows.ApplicationModel.Wallet)-Namespace |
| Xml | |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTime**-Methode |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTimeOffset**-Methode |


Im nächsten Thema wird [das Projekt portieren](wpsl-to-uwp-porting-to-a-uwp-project.md).