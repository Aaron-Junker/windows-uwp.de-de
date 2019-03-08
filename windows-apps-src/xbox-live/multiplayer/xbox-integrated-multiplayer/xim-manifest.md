---
title: Konfiguration des XIM-Projekts
description: Beschreibt das Konfigurieren Ihrer UWP-app-Manifeste mit Xbox integrierte Multiplayer-Spiele (XIM) funktioniert.
ms.assetid: 5d0ed7bd-9527-42a3-ae09-8cc9012c1111
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Xbox integrierte Multiplayer-Spiele, manifest
ms.localizationpriority: medium
ms.openlocfilehash: 926cc4495236fc4762f9b3176a5d6a4a579e2e2e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613425"
---
# <a name="xim-project-configuration"></a>Konfiguration des XIM-Projekts

## <a name="required-appxmanifest-capability-content"></a>Erforderlicher Inhalt der AppXManifest-Funktion

Eine Anwendung, die mithilfe von Natur aus XIM erfordert Herstellen einer Verbindung mit und zum Akzeptieren von Verbindungen von Netzwerkressourcen, die sowohl über das Internet und dem lokalen Netzwerk. Außerdem müssen den Zugriff auf Geräte der Mikrofon Voice Chat zu unterstützen. Daher sollten die app die "InternetClientServer" und "PrivateNetworkClientServer"-Funktionen und die Gerätefunktion "Mikrofon", in ihrem AppXManifest deklarieren. Finden Sie in der Dokumentation zur Plattform für die weitere Details für jedes aus. Der folgende Codeausschnitt zeigt die Knoten, die unter dem Knoten Paket/Funktionen vorhanden sind, sollte dies sonst bei Konnektivität und Chat blockiert werden:

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package ...>
   <Identity ... />
   ...
   <Capabilities>
     <Capability Name="internetClientServer" />
     <Capability Name="privateNetworkClientServer" />
     <DeviceCapability Name="microphone" />
   </Capabilities>
 </Package>
```

## <a name="universal-windows-application-xbox-live-multiplayer-invite-protocol-activation-extension"></a>Laden Sie universelle Windows-Anwendung, die Xbox Live Multiplayer-Protokoll-aktivierungserweiterung

Xbox Live-Multiplayer-Anwendungen sollten Spiele Einladungen zu unterstützen. Einladungen, die von lokalen Benutzern akzeptiert in Form von protokollaktivierung der app eingehen (finden Sie in der Dokumentation zur Plattform für Weitere Informationen zur protokollaktivierung). Apps mit XIM außer für Plattformaufruf XIM-Netzwerke, die mittels Out-of-Band-Reservierungen verwaltet die `xim::extract_protocol_activation_information()` Methode zur Unterstützung bei der Verarbeitung von protokollaktivierung Laufzeit-, aber Anwendungen der universellen Windows-Plattform (UWP) muss ebenfalls statisch deklariert, über ein Erweiterung "windows.protocol" AppXManifest, dass sie solche "ms-Xbl-Multiplayer"-Aktivierungen durch das System unterstützen. Dies kann durchgeführt werden, wie im folgenden Codeausschnitt gezeigt:

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10" xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" IgnorableNamespaces="uap">
   <Applications>
     <Application ...>
       <Extensions>
         ...
          <uap:Extension Category="windows.protocol">
             <uap:Protocol Name="ms-xbl-multiplayer"/>
          </uap:Extension>
       </Extensions>
     </Application>
   </Applications>
 </Package>
```

Dies ist nur für universelle Windows-Anwendungen erforderlich sind. Xbox One-Ressource ("xdk-Version")-Anwendungen deklarieren nicht explizit eine Protokoll-aktivierungserweiterung, wie sie automatisch registriert werden, indem ihre Xbox Live Title-ID-Erweiterung deklariert.

## <a name="xbox-one-exclusive-resource-application-network-manifest-templates"></a>Xbox One-Ressource-Anwendungsnetzwerk manifest Vorlagen

Xbox One Ressource Anwendungen, die XIM verwenden, müssen bestimmte Socket-Beschreibung und die Vorlageninhalt wird in der Erweiterung "Netzwerk-Manifest" die appxmanifest-Datei eingeschlossen (finden Sie in der Dokumentation zur Plattform für die weitere Details auf Netzwerk-Manifeste ein). Die app möglicherweise auch andere Vorlagen, aber der folgende Codeausschnitt muss vorhanden sein, andernfalls mit einem Netzwerk XIM verschieben fehl:

```xml

 <?xml version="1.0" encoding="utf-8"?>
 <Package xmlns="http://schemas.microsoft.com/appx/2010/manifest" xmlns:mx="http://schemas.microsoft.com/appx/2013/xbox/manifest" IgnorableNamespaces="mx">
   <Applications>
     <Application ...>
       <Extensions>
         ...
         <mx:Extension Category="windows.xbox.networking">
           <mx:XboxNetworkingManifest>
             <mx:SocketDescriptions>
               <mx:SocketDescription Name="Xim_client_v1" SecureIpProtocol="Udp" BoundPort="17181-17183">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceSocketUsage Type="Initiate" />
                   <mx:SecureDeviceSocketUsage Type="Accept" />
                   <mx:SecureDeviceSocketUsage Type="SendChat" />
                   <mx:SecureDeviceSocketUsage Type="SendGameData" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveChat" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveGameData" />
                 </mx:AllowedUsages>
               </mx:SocketDescription>
               <mx:SocketDescription Name="Xim_relay_acceptor_v1" SecureIpProtocol="Udp" BoundPort="17191-17390">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceSocketUsage Type="Accept" />
                   <mx:SecureDeviceSocketUsage Type="SendChat" />
                   <mx:SecureDeviceSocketUsage Type="SendGameData" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveChat" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveGameData" />
                 </mx:AllowedUsages>
               </mx:SocketDescription>
             </mx:SocketDescriptions>
             <mx:SecureDeviceAssociationTemplates>
               <mx:SecureDeviceAssociationTemplate Name="Xim_relay_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_relay_acceptor_v1" MultiplayerSessionRequirement="Required">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
                   <mx:SecureDeviceAssociationUsage Type="AcceptOnXboxLiveCompute" />
                 </mx:AllowedUsages>
               </mx:SecureDeviceAssociationTemplate>
               <mx:SecureDeviceAssociationTemplate Name="Xim_peer_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_client_v1" MultiplayerSessionRequirement="Required">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
                   <mx:SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
                   <mx:SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
                 </mx:AllowedUsages>
               </mx:SecureDeviceAssociationTemplate>
             </mx:SecureDeviceAssociationTemplates>
           </mx:XboxNetworkingManifest>
         </mx:Extension>
       </Extensions>
     </Application>
   </Applications>
 </Package>
```

## <a name="universal-windows-application-uwp-network-manifest-templates"></a>Universelle Windows-Anwendung (UWP) Netzwerk manifest Vorlagen

Alle universelle Windows-Anwendung, die XIM verwendet, müssen bestimmte Socket-Beschreibung und Vorlageninhalt befindet sich in der Datei "Netzwerk-Manifest" (Siehe networkmanifest.xml-Datei im Stammverzeichnis Pakets, in der Dokumentation zur Plattform für die weitere Details auf Netzwerk ausgedrückt). Die app möglicherweise auch andere Vorlagen, aber der folgende Codeausschnitt muss vorhanden sein, andernfalls mit einem Netzwerk XIM verschieben fehl:

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <NetworkManifest xmlns="http://schemas.microsoft.com/xbox/2012/networkmanifest">
   <SocketDescriptions>
     <SocketDescription Name="Xim_client_v1" SecureIpProtocol="Udp" BoundPort="17181-17183">
       <AllowedUsages>
         <SecureDeviceSocketUsage Type="Initiate" />
         <SecureDeviceSocketUsage Type="Accept" />
         <SecureDeviceSocketUsage Type="SendChat" />
         <SecureDeviceSocketUsage Type="SendGameData" />
         <SecureDeviceSocketUsage Type="ReceiveChat" />
         <SecureDeviceSocketUsage Type="ReceiveGameData" />
       </AllowedUsages>
     </SocketDescription>
     <SocketDescription Name="Xim_relay_acceptor_v1" SecureIpProtocol="Udp" BoundPort="17191-17390">
       <AllowedUsages>
         <SecureDeviceSocketUsage Type="Accept" />
         <SecureDeviceSocketUsage Type="SendChat" />
         <SecureDeviceSocketUsage Type="SendGameData" />
         <SecureDeviceSocketUsage Type="ReceiveChat" />
         <SecureDeviceSocketUsage Type="ReceiveGameData" />
       </AllowedUsages>
     </SocketDescription>
   </SocketDescriptions>
   <SecureDeviceAssociationTemplates>
     <SecureDeviceAssociationTemplate Name="Xim_relay_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_relay_acceptor_v1" MultiplayerSessionRequirement="Required">
       <AllowedUsages>
         <SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
         <SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
         <SecureDeviceAssociationUsage Type="AcceptOnXboxLiveCompute" />
       </AllowedUsages>
     </SecureDeviceAssociationTemplate>
     <SecureDeviceAssociationTemplate Name="Xim_peer_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_client_v1" MultiplayerSessionRequirement="Required">
       <AllowedUsages>
         <SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
         <SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
         <SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
         <SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
       </AllowedUsages>
     </SecureDeviceAssociationTemplate>
   </SecureDeviceAssociationTemplates>
 </NetworkManifest>
```
