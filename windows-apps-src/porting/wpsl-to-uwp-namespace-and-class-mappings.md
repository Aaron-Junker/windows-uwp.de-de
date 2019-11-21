---
description: This topic provides a comprehensive mapping of Windows Phone Silverlight APIs to their Universal Windows Platform (UWP) equivalents.
title: Windows Phone Silverlight to UWP namespace and class mappings
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1af36b43e02c9ee08373098f57ba29c10badb6c8
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259066"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone Silverlight to UWP API mappings


This topic provides a comprehensive mapping of Windows Phone Silverlight APIs to their Universal Windows Platform (UWP) equivalents. Im Allgemeinen erfolgt keine 1: 1-Zuordnung von Funktionen, jedoch gilt: Jede Plattform kann ggf. mehr oder weniger Funktionalität bieten als ihr Gegenstück in einem Namespace oder einer Klasse.

The mapping table will help you when you're working in a UWP project and you're re-using source code from a Windows Phone Silverlight project. Zwischen den beiden Plattformen gibt es Unterschiede bei den Namen von Namespaces und Klassen (einschließlich UI-Steuerelemente). In vielen Fällen ist es einfach: Sie ändern z. B. einen Namespacenamen, und der Code wird kompiliert Manchmal wird neben dem Namespacenamen auch der Name einer Klasse oder API geändert In anderen Fällen ist die Zuordnung etwas schwieriger, und in seltenen Fällen muss der Ansatz geändert werden.

**How to use the table:  ** First, search for the name of the class you're using. Klassen werden aufgelistet, wenn es sich um eine kompliziertere Zuordnung als eine Änderung des Namespacenamens handelt. Wenn Ihre Klasse nicht aufgeführt ist, handelt es sich bei der Zuordnung nur um eine Namespaceänderung. Wenn Sie den Namespacenamen Ihrer Klasse finden, finden Sie auch den entsprechenden Namen des UWP-Namespaces. Ihre Klasse ist in diesem Namespace enthalten. Wenn der Namespace nicht aufgeführt ist, hat sich dessen Name nicht geändert.

**Note**  Windows 10 supports much more of the .NET Framework than a Windows Phone Store app does. For example, Windows 10 has several System.ServiceModel.\* namespaces as well as System.Net, System.Net.NetworkInformation, and System.Net.Sockets.
Also, in a Windows 10 app, you will benefit from .NET Native, which an ahead-of-time compilation technology that converts MSIL into natively-runnable machine code. .NET Native-Apps starten schneller, verbrauchen weniger Arbeitsspeicher und benötigen weniger Akkuenergie als ihre MSIL-Gegenstücke.

| Windows Phone Silverlight | Windows-Runtime |
| ------------------------- | --------------- |
| Werbung | |
| **Microsoft.Advertising.Mobile.UI.AdControl**-Klasse | [AdControl](https://docs.microsoft.com/windows/uwp/monetize/display-ads-using-the-microsoft-advertising-libraries)-Klasse |
| Alarme, Erinnerungen und Hintergrund-Agents | |
| **Microsoft.Phone.BackgroundAgent**-Klasse | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) class |
| **Microsoft.Phone.Scheduler** Namespace | [**Windows.ApplicationModel.Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background) namespace |
| **Microsoft.Phone.Scheduler.Alarm** Klasse | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) and [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) classes |
| **Microsoft.Phone.Scheduler.PeriodicTask**-, **ScheduledAction**-, **ScheduledActionService**-, **ScheduledTask**- und **ScheduledTaskAgent**-Klassen | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) class |
| **Microsoft.Phone.Scheduler.Reminder** Klasse | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) and [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) classes |
| **Microsoft.Phone.PictureDecoder** Klasse | [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) class |
| **Microsoft.Phone.BackgroundAudio** Namespace | [**Windows.Media.Playback**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback) namespace |
| **Microsoft.Phone.BackgroundTransfer**-Namespace | [**Windows.Networking.BackgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) namespace |
| App-Modell und -Umgebung | |
| **System.AppDomain** Klasse | Keine direkte Entsprechung Siehe [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application), [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication), Klassen |
| **System.Environment** Klasse | Keine direkte Entsprechung |
| **System.ComponentModel.Annotations** Klasse  | Keine direkte Entsprechung |
| **System.ComponentModel.BackgroundWorker** Klasse | [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) class |
| **System.ComponentModel.DesignerProperties** Klasse | [**DesignMode**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DesignMode) class |
| **System.Threading.Thread**, **System.Threading.ThreadPool** Klassen | [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) class |
| (ST = **System.Threading**) <br/> **ST.Thread.MemoryBarrier** Methode | (ST = **System.Threading**) <br/> **ST.Interlocked.MemoryBarrier** Methode |
| (ST = **System.Threading**) <br/> **ST.Thread.ManagedThreadId** Eigenschaft | (S = **System**) <br/> **S.Environment.ManagedThreadId** Eigenschaft |
| **System.Threading.Timer** Klasse | [**ThreadPoolTimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer) class |
| (SWT = **System.Windows.Threading**) <br/> **SWT.Dispatcher** Klasse | [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) class |
| (SWT = **System.Windows.Threading**) <br/> **SWT.DispatcherTimer** Klasse | [**DispatcherTimer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer) class |
| Blend für Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> **MEDC.GeometryHelper** Klasse | Keine direkte Entsprechung |
| **Microsoft.Expression.Interactivity** Namespace | [Microsoft.Xaml.Interactivity](https://msdn.microsoft.com/library/windows/apps/microsoft.xaml.interactivity.aspx) Namespace |
| **Microsoft.Expression.Interactivity.Core** Namespace | [Microsoft.Xaml.Interactions.Core](https://msdn.microsoft.com/library/windows/apps/microsoft.xaml.interactions.core.aspx) Namespace |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> **MEIC.ExtendedVisualStateManager** Klasse | Keine direkte Entsprechung |
| **Microsoft.Expression.Interactivity.Input** Namespace | Keine direkte Entsprechung |
| **Microsoft.Expression.Interactivity.Media** Namespace | [Microsoft.Xaml.Interactions.Media](https://msdn.microsoft.com/library/windows/apps/microsoft.xaml.interactions.media.aspx) Namespace |
| **Microsoft.Expression.Shapes** Namespace | Keine direkte Entsprechung |
| (MI = **Microsoft.Internal**) <br/> **MI.IManagedFrameworkInternalHelper** Schnittstelle | Keine direkte Entsprechung |
| Kontakt- und Kalenderdaten | |
| **Microsoft.Phone.UserData** Namespace | [**Windows.ApplicationModel.Contacts**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts), [**Windows.ApplicationModel.Appointments**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments) namespaces |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Account**-, **ContactAddress**-, **ContactCompanyInformation**-, **ContactEmailAddress**-, **ContactPhoneNumber** Klassen | [**Contact**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) class |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Appointments** Klasse | [**AppointmentCalendar**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar) class |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Contacts** Klasse | [**ContactStore**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactStore) class |
| Steuerelemente und UI-Infrastruktur | |
| **ControlTiltEffect.TiltEffect** Klasse | Animationen aus der Windows-Runtime-Animationsbibliothek sind in die Standardstile der allgemeinen Steuerelemente integriert. Siehe [Animation](wpsl-to-uwp-porting-xaml-and-ui.md). |
| **Microsoft.Phone.Controls** Namespace | [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) namespace |
| (MPC = **Microsoft.Phone.Controls**) <br/> **MPC.ContextMenu** Klasse | [**PopupMenu**](https://docs.microsoft.com/uwp/api/Windows.UI.Popups.PopupMenu) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.DatePickerPage** Klasse | [**DatePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.GestureListener** Klasse | [**GestureRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.LongListSelector** Klasse | [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.ObscuredEventArgs** Klasse | [**SystemProtection**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.SystemProtection), [**WindowActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.WindowActivatedEventArgs) classes |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.Panorama** Klasse | [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** Klassen | [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationPage** Klasse | [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TiltEffect** Klasse | [**PointerDownThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TimePickerPage** Klasse | [**TimePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowser** Klasse | [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowserExtensions** Klasse | Keine direkte Entsprechung |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WrapPanel** Klasse | Keine direkte Entsprechung für das allgemeine Layout. [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) and [**WrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) can be used in the items panel template of an items control. |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq** Namespace | Keine direkte Entsprechung |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq.Mapping** Namespace | Keine direkte Entsprechung |
| **Microsoft.Phone.Globalization** Namespace | Keine direkte Entsprechung |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.DeviceExtendedProperties**, **DeviceStatus** Klassen | [**EasClientDeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation), [**MemoryManager**](https://docs.microsoft.com/uwp/api/Windows.System.MemoryManager) classes. Weitere Infos finden Sie unter [Gerätestatus](wpsl-to-uwp-input-and-sensors.md). |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.MediaCapabilities** Klasse | Keine direkte Entsprechung |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.UserExtendedProperties** Klasse | [**AdvertisingManager**](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile.AdvertisingManager) class |
| **System.Windows** Namespace | [**Windows.UI.Xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml) namespace |
| **System.Windows.Automation** Namespace | [**Windows.UI.Xaml.Automation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation) namespace |
| **System.Windows.Controls**, **System.Windows.Input** Namespaces | [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) namespaces |
| **System.Windows.Controls.DrawingSurface**-, **DrawingSurfaceBackgroundGrid** Klassen | [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) class |
| **System.Windows.Controls.RichTextBox** Klasse | [**RichEditBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) class |
| **System.Windows.Controls.WrapPanel** Klasse | Keine direkte Entsprechung für das allgemeine Layout. [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) and [**WrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) can be used in the items panel template of an items control. |
| **System.Windows.Controls.Primitives** Namespace | [**Windows.UI.Xaml.Controls.Primitives**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives) namespace |
| **System.Windows.Controls.Shapes** Namespace | [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) namespace |
| **System.Windows.Data** Namespace | [**Windows.UI.Xaml.Data**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data) namespace |
| **System.Windows.Documents** Namespace | [**Windows.UI.Xaml.Documents**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents) namespace |
| **System.Windows.Ink** Namespace | Keine direkte Entsprechung |
| **System.Windows.Markup** Namespace | [**Windows.UI.Xaml.Markup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup) namespace | 
| **System.Windows.Navigation** Namespace | [**Windows.UI.Xaml.Navigation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation) namespace |
| **System.Windows.UIElement.Tap** Ereignis, **EventHandler&lt;GestureEventArgs&gt;** Delegat | [**Tapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped) event, [**TappedEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.tappedeventhandler) delegate |
| Daten und Dienste |  |
| **System.Data.Linq.DataContext** Klasse | Keine direkte Entsprechung |
| **System.Data.Linq.Mapping.ColumnAttribute** Klasse | Keine direkte Entsprechung |
| **System.Data.Linq.SqlClient.SqlHelpers** Klasse | Keine direkte Entsprechung |
| Geräte | |
| **Microsoft.Devices**, **Microsoft.Devices.Sensors** Namespaces | [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), [**Windows.Devices.Enumeration.Pnp**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.Pnp), [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input), [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) namespaces |
| **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** Klassen | [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) class. Auch [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Klasse (nur Windows). |
| **Microsoft.Devices.CameraButtons** Klasse | [**HardwareButtons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) class |
| **Microsoft.Devices.CameraVideoBrushExtensions** Klasse | [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) class |
| **Microsoft.Devices.Environment** Klasse | Keine direkte Entsprechung Um dieses Problem zu umgehen, verwenden Sie die bedingte Kompilierung und definieren Sie ein benutzerdefiniertes Symbol. Unter Umständen können Sie das Problem auch mit der [IsAttached](https://docs.microsoft.com/dotnet/api/system.diagnostics.debugger.isattached#System_Diagnostics_Debugger_IsAttached) Eigenschaft umgehen. |
| **Microsoft.Devices.MediaHistory** Klasse | Keine direkte Entsprechung |
| **Microsoft.Devices.VibrateController** Klasse | [**VibrationDevice**](https://docs.microsoft.com/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice) class |
| **Microsoft.Devices.Radio.FMRadio** Klasse | Keine direkte Entsprechung |
| **Microsoft.Devices.Sensors.Accelerometer**, **Compass** Klassen | Im [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)-Namespace |
| **Microsoft.Devices.Sensors.Gyroscope** Klasse | [**Gyrometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer) class |
| **Microsoft.Devices.Sensors.Motion** Klasse | [**Inclinometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer) class |
| Globalization | |
| **System.Globalization** Namespace | [**Windows.Globalization**](https://docs.microsoft.com/uwp/api/Windows.Globalization) namespace |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentCulture** Eigenschaft | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentCulture** Eigenschaft |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentUICulture** Eigenschaft | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentUICulture** Eigenschaft |
| Grafiken und Animationen | |
| **Microsoft.Xna.Framework.\*** namespaces, [XNA Framework Class Library](https://msdn.microsoft.com/library/bb203940.aspx), [Content Pipeline Class Library](https://msdn.microsoft.com/library/bb195587(v=XNAGameStudio.40).aspx) | Keine direkte Entsprechung Verwenden Sie im Allgemeinen [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) mit C++. Siehe [Entwickeln von Spielen](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10)) und [Interoperabilität von DirectX und XAML](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10)). |
| **Microsoft.Xna.Framework.Audio.Microphone** Klasse | [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) class |
| **Microsoft.Xna.Framework.Audio.SoundEffect** Klasse | [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) class |
| **Microsoft.Xna.Framework.GamerServices** Namespace | (WPS = **Windows.Phone.System**) <br/> [**WPS.UserProfile.GameServices.Core**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core) namespace |
| **Microsoft.Xna.Framework.GamerServices.Guide** Klasse | Keine direkte Entsprechung |
| **Microsoft.Xna.Framework.Input.GamePad** Klasse | [**HardwareButtons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) class |
| **Microsoft.Xna.Framework.Input.Touch.TouchPanel** Klasse | [**GestureRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) class |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> **MXFM.MediaLibrary**-, **MXFM.PhoneExtensions.MediaLibraryExtensions** Klassen | [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) class |
| **Microsoft.Xna.Framework.Media.MediaQueue** Klasse | [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) class |
| **Microsoft.Xna.Framework.Media.Playlist** Klasse | [**BackgroundMediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer) class |
| **System.Windows.Media** Namespace | [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) namespace |
| **System.Windows.Media.RadialGradientBrush** Klasse | Keine direkte Entsprechung Siehe [Medien und Grafiken](wpsl-to-uwp-porting-xaml-and-ui.md). |
| **System.Windows.Media.Animation** Namespace | [**Windows.UI.Xaml.Media.Animation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation) namespace |
| **System.Windows.Media.Effects** Namespace | Keine direkte Entsprechung |
| **System.Windows.Media.Imaging** Namespace | [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) namespace |
| **System.Windows.Media.Media3D** Namespace | [**Windows.UI.Xaml.Media.Media3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D) namespace |
| **System.Windows.Shapes** Namespace | [**Windows.UI.Xaml.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) namespace |
| Launcher und Chooser | |
| **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask**, **PhoneNumberChooserTask** Klassen | [**ContactPicker**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker) class |
| **Microsoft.Phone.Tasks.AddWalletItemTask**, **AddWalletItemResult** Klassen | [**Windows.ApplicationModel.Wallet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) namespace |
| **Microsoft.Phone.Tasks.BingMapsDirectionsTask**, **BingMapsTask** Klassen | Keine direkte Entsprechung |
| **Microsoft.Phone.Tasks.CameraCaptureTask** Klasse | [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) class. Auch [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Klasse (nur Windows). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) class ([**RequestAppPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) method) |
| **Microsoft.Phone.Tasks.ConnectionSettingsTask**-, **MarketplaceHubTask**-, **MarketplaceReviewTask**-, **MarketplaceSearchTask**-, **MediaPlayerLauncher**-, **SearchTask**-, **SmsComposeTask**-, **WebBrowserTask** Klassen | [**Launcher**](https://docs.microsoft.com/uwp/api/Windows.System.Launcher) class |
| **Microsoft.Phone.Tasks.EmailComposeTask** Klasse | [**EmailMessage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email.EmailMessage) class |
| **Microsoft.Phone.Tasks.GameInviteTask** Klasse | Keine direkte Entsprechung |
| **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask**, **MapUpdaterTask** Klassen | Keine direkte Entsprechung |
| **Microsoft.Phone.Tasks.PhoneCallTask** Klasse | [**PhoneCallManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager) class |
| **Microsoft.Phone.Tasks.PhotoChooserTask** Klasse | [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) class |
| **Microsoft.Phone.Tasks.SaveAppointmentTask** Klasse | [**AppointmentManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager) class |
| **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask**, **SavePhoneNumberTask** Klassen | [**StoredContact**](https://docs.microsoft.com/uwp/api/Windows.Phone.PersonalInformation.StoredContact) class (Windows Phone only) | 
| **Microsoft.Phone.Tasks.SaveRingtoneTask** Klasse | Keine direkte Entsprechung |
| **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask**, **ShareStatusTask** Klassen | [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) class |
| Pfad | |
| **System.Device.Location** Namespace | [**Windows.Devices.Geolocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation) namespace |
| **System.Device.GeoCoordinateWatcher** Klasse | [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) class |
| Karten | |
| **Microsoft.Phone.Maps** Namespaces | [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) namespace |
| **Microsoft.Phone.Maps.Controls** Namespace | [**Windows.UI.Xaml.Controls.Maps**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps) namespace |
| **Microsoft.Phone.Maps.Controls.Map** Klasse | [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) class |
| **Microsoft.Phone.Maps.Services** Namespace | [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) namespace |
| **Microsoft.Phone.Maps.Services.GeocodeQuery**, **ReverseGeocodeQuery** Klassen | [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) class |
| **System.Device.Location.GeoCoordinate** Klasse | [**Geopoint**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint) class |
| **Microsoft.Phone.Maps.Services.Route** Klasse | [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) class |
| **Microsoft.Phone.Maps.Services.RouteQuery** Klasse | [**MapRouteFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder) class |
| Monetisierung | |
| **Microsoft.Phone.Marketplace** Namespace | [**Windows.ApplicationModel.Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) namespace |
| Media | |
| **Microsoft.Phone.Media** Namespace | [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) class |
| -Netzwerk | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.DeviceNetworkInformation** Klasse | [**Hostname**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName), [**NetworkInformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) classes |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterface** Klasse | [**NetworkInformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) class |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceInfo** Klasse | [**ConnectionProfile**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) class |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceList** Klasse | [**NetworkInformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) class |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.SocketExtensions** Klasse | Keine direkte Entsprechung |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.WebRequestExtensions** Klasse | Keine direkte Entsprechung |
| **Microsoft.Phone.Networking.Voip** Namespace | Keine direkte Entsprechung |
| **System.Net.CookieCollection** Klasse | Wird noch unterstützt, aber einige Eigenschaften fehlen (z. B. IsReadOnly) |
| **System.Net.DownloadProgressChangedEventArgs** Klasse und vergleichbare Klassen im Zusammenhang mit **System.Net.WebClient** | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Ableitung von [System.Net.Http.StreamContent](https://docs.microsoft.com/previous-versions/visualstudio/hh138119(v=vs.118)) zum Messen des Fortschritts |
| **System.Net.DnsEndPoint**-, **IPAddress** Klassen | Diese Klassen werden zwar noch unterstützt, aber einige Eigenschaften fehlen. Alternativ dazu ist das Portieren zur [**HostName**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName)-Klasse möglich. |
| **System.Net.HttpUtility** Klasse | [**HtmlFormatHelper**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper) class |
| **System.Net.HttpWebRequest** Klasse | Wird teilweise unterstützt, aber die empfohlene fortschrittliche Alternative ist die [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)-Klasse (oder [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Für diese APIs wird [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) verwendet, um eine HTTP-Anforderung darzustellen. |
| **System.Net.HttpWebResponse** Klasse | Wird weiterhin unterstützt, aber anstelle von „Close()“ wird „Dispose()“ verwendet. Die empfohlene fortschrittliche Alternative ist jedoch die [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)-Klasse (oder [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Für diese APIs wird [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage) verwendet, um eine HTTP-Antwort darzustellen. |
| (SNN = **System.Net.NetworkInformation**) <br/> **SNN.NetworkChange** Klasse | Wird noch unterstützt, mit Ausnahme des Konstruktors |
| **System.Net.OpenReadCompletedEventArgs** Klasse und vergleichbare Klassen im Zusammenhang mit **System.Net.WebClient** | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.Sockets.Socket** Klasse | Wird weiterhin unterstützt, aber anstelle von „Close()“ wird „Dispose()“ verwendet. Alternativ dazu ist das Portieren zur[**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket)-Klasse möglich. |
| **System.Net.Sockets.SocketException** Klasse | Wird zwar noch unterstützt, aber anstelle von ErrorCode wird die SocketErrorCode-Eigenschaft verwendet. |
| **System.Net.Sockets.UdpAnySourceMulticastClient**, **UdpSingleSourceMulticastClient** Klassen | [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket) class |
| **System.Net.UploadProgressChangedEventArgs** Klasse und vergleichbare Klassen im Zusammenhang mit **System.Net.WebClient** | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebClient** Klasse | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebRequest** Klasse | Wird teilweise unterstützt (anderer Eigenschaftensatz), aber die empfohlene modernere Alternative ist die [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)-Klasse (oder [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Für diese APIs wird [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) verwendet, um eine HTTP-Anforderung darzustellen. |
| **System.Net.WebResponse** Klasse | Wird weiterhin unterstützt, aber anstelle von „Close()“ wird „Dispose()“ verwendet. Die empfohlene fortschrittliche Alternative ist jedoch die [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)-Klasse (oder [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). Für diese APIs wird [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage) verwendet, um eine HTTP-Antwort darzustellen. |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventArgs** Klasse | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventHandler** Klasse | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.UriFormatException** Klasse | **System.FormatException** Klasse |
| Benachrichtigungen | |
| MPN = **Microsoft.Phone.Notification**-Namespace | [**Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications), [**Windows.Networking.PushNotifications**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications) namespaces |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotification** Klasse | [**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification) class |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotificationChannel** Klasse | [**PushNotificationChannel**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel) class |
| Programmierung | |
| **System** Namespace | [**Windows.Foundation**](https://docs.microsoft.com/uwp/api/Windows.Foundation) namespace |
| **System.Diagnostics.StackFrame**, **StackTrace** Klassen | Keine direkte Entsprechung |
| **System.Diagnostics** Namespace | [**Windows.Foundation.Diagnostics**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Diagnostics) namespace |
| **System.ICloneable** Schnittstelle | Eine benutzerdefinierte Methode, mit der der passende Typ zurückgegeben wird. |
| **System.Reflection.Emit.ILGenerator** Klasse | Keine direkte Entsprechung |
| Reaktive Erweiterungen | |
| **Microsoft.Phone.Reactive** Namespace | Keine direkte Entsprechung |
| Reflektion | |
| **System.Type** Klasse | **System.Reflection.TypeInfo** Klasse. Siehe [Reflektion in .NET Framework für UWP-Apps](https://docs.microsoft.com/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps). |
| Ressourcen | |
| **System.Resources.ResourceManager** Klasse | (WA = **Windows.ApplicationModel**)<br/>[**WA.Resources.Core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core) and [**WA.Resources**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources) namespaces, [**ResourceManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) class. Siehe [Erstellen und Abrufen von Ressourcen in Windows-Runtime-Apps](https://docs.microsoft.com/previous-versions/windows/apps/hh694557(v=vs.140)). |
| Sicheres Element | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementChannel**, **MPS.SecureElementSession** Klassen | [**SmartCardConnection**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardConnection) class |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementReader** Klasse | [**SmartCardReader**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardReader) class |
| Security | |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.Aes**, **SSC.RSA** Klassen | [**CryptographicEngine**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) class |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.HMACSHA256**, **SSC.SHA256** Klassen | [**HashAlgorithmProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) class |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.ProtectedData** Klasse | [**DataProtectionProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) class |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.RandomNumberGenerator** Klasse | [**CryptographicBuffer**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.CryptographicBuffer) class |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.X509Certificates.X509Certificate** Klasse | [**CertificateEnrollmentManager**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager) class |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBar** Klasse | [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarIconButton** Klasse | [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) class (when used inside the [**PrimaryCommands**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands) property) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarMenuItem** Klasse | [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) class (when used inside the [**SecondaryCommands**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) property) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData**, **MPSh.StandardTileData** Klassen | [**TileTemplateType**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileTemplateType) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.PhoneApplicationService** Klasse | [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication), [**DisplayRequest**](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest) classes |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ProgressIndicator** Klasse | [**StatusBarProgressIndicator**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTile** Klasse | [**SecondaryTile**](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTileSchedule** Klasse | [**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellToast** Klasse | [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.SystemTray** Klasse | [**StatusBar**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBar) class |
| Speicher und E/A | |
| **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile**, **ExternalStorageFolder** Klassen | [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) class |
| **System.IO** Namespace | [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage), [**Windows.Storage.Streams**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams) namespaces |
| **System.IO.Directory** Klasse | [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) class |
| **System.IO.File** Klasse | [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) and [**PathIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.PathIO) classes
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageFile** Klasse |[**ApplicationData.LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) property |
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageSettings** Klasse | [**ApplicationData.LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) property |
| **System.IO.Stream** Klasse | Wird noch unterstützt, aber anstelle von BeginRead()/EndRead() und BeginWrite()/EndWrite() wird ReadAsync() und WriteAsync() verwendet. |
| Brieftasche | |
| **Microsoft.Phone.Wallet** Namespace | [**Windows.ApplicationModel.Wallet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) namespace |
| XML | |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTime** Methode |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTimeOffset** Methode |


Nächstes Thema: [Portieren des Projekts](wpsl-to-uwp-porting-to-a-uwp-project.md).

