---
description: Hier erfährst du, welche neuesten Ergänzungen an der Dokumentation für Windows-Entwickler vorgenommen wurden.
title: Neueste Aktualisierungen an der Dokumentation für Windows-Entwickler
ms.topic: article
ms.date: 2/10/2021
ms.localizationpriority: medium
ms.author: quradic
author: QuinnRadich
ms.openlocfilehash: 385d6b0a2cb150c13f75183fd3c5592c0f6f9868
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824064"
---
# <a name="latest-updates-to-the-windows-developer-docs"></a>Neueste Aktualisierungen an der Dokumentation für Windows-Entwickler

Die Dokumentation für Windows-Entwickler wird regelmäßig mit neuen und verbesserten Informationen und Inhalten aktualisiert. Hier finden Sie eine Zusammenfassung der Änderungen zum 10. Februar 2021.

Hinweis: Eine spezifische Liste der APIs, die als Teil von Windows 10 Build 19041 (auch als 2004 bezeichnet) hinzugefügt wurden, finden Sie in [dieser Liste](/windows/uwp/whats-new/windows-10-build-19041-api-diff).

Die neuesten Nachrichten zur Dokumentation für Windows-Entwickler oder Informationen dazu, wie Sie uns mit Kommentaren und Fragen erreichen, finden Sie unter unserem Twitter-Handle [@WindowsDocs](https://twitter.com/windowsdocs).

Wir haben in unserer gesamten Dokumentation weiter nach unangemessenen Formulierungen gesucht und diese entfernt.

Informationen dazu, wie Sie zur Microsoft-Dokumenten beitragen können, finden Sie im [Leitfaden für Mitwirkende](/contribute/).

Zu den Highlights dieses Monats gehören:

### <a name="new-content"></a>Neuer Inhalt


* [Portierung aus C++/WinRT nach C#](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp)
* [Exemplarische Vorgehensweise: Erstellen einer C#/WinRT-Komponente und deren Verwenden aus C++/WinRT](/windows/uwp/csharp-winrt/create-windows-runtime-component-cswinrt)
* [Diagnose von C#/WinRT-Komponentenfehlern](/windows/uwp/csharp-winrt/authoring-diagnostics)
* Neue Folge unserer Videoshow [Tabstopps im Vergleich zu Leerzeichen](https://channel9.msdn.com/Shows/Tabs-vs-Spaces)




### <a name="updated-topics"></a>Aktualisierte Themen

* Neufassung der Themen zur Navigationssteuerung, mit C++/ WinRT-Beispielcode: 
    * [Navigationsverlauf und Rückwärtsnavigation für Windows-Apps](/windows/uwp/design/basics/navigation-history-and-backwards-navigation)
    * [Navigationsansicht](/windows/uwp/design/controls-and-patterns/navigationview)
* [High-Level-Shaderprogrammiersprache (HLSL)](/windows/win32/direct3dhlsl/dx-graphics-hlsl)
* [WinHttpQueryDataAvailable](/windows/win32/api/winhttp/nf-winhttp-winhttpquerydataavailable)
* [Tutorial zur Verwendung von Featureflags in einer .NET Core-App | Microsoft-Dokumentation](/azure/azure-app-configuration/use-feature-flags-dotnet-core)
* Fortlaufende Aktualisierungen der Dokumente zur [WinUI 2](../winui/winui2/index.md) und [WinUI 3](../winui/winui3/index.md).
* Regelmäßige Aktualisierungen zu [Einrichten Ihrer Entwicklungsumgebung unter Windows 10](../../dev-environment/overview.md), die WSL und Windows Terminal umfassen.
* Informationen zur neuen [Benutzeroberfläche „Einstellungen“](/windows/terminal/customize-settings/startup) in Windows Terminal.

In den folgenden Themen der Referenz wurden im vergangenen Monat wichtige Aktualisierungen vorgenommen:

## <a name="winrt-conceptual"></a>WinRT-Konzept

<ul>
<li><a href="/windows/uwp/composition/relation-animations">Auf Beziehungen basierende Animationen</a></li>
<li><a href="/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp">Umstellen von C# auf C++/WinRT</a></li>
<li><a href="/windows/uwp/csharp-winrt/authoring-diagnostics">Diagnose von C#/WinRT-Komponentenfehlern</a></li>
<li><a href="/windows/uwp/csharp-winrt/create-windows-runtime-component-cswinrt">Erstellen einer C#/WinRT-Komponente und deren Verwenden aus C++/WinRT</a></li>
<li><a href="/windows/uwp/csharp-winrt/index">C#/WinRT</a></li>
<li><a href="/windows/uwp/debug-test-perf/device-portal-api-core">Referenz zur Kern-API des Windows-Geräteportals</a></li>
<li><a href="/windows/uwp/debug-test-perf/device-portal-desktop">Windows-Geräteportal für Desktop</a></li>
<li><a href="/windows/uwp/debug-test-perf/device-portal-mobile">Windows-Geräteportal für Mobilgeräte</a></li>
<li><a href="/windows/uwp/debug-test-perf/device-portal">Übersicht über das Windows-Geräteportal</a></li>
<li><a href="/windows/uwp/design/app-settings/store-and-retrieve-app-data">Speichern und Abrufen von Einstellungen und anderen App-Daten</a></li>
<li><a href="/windows/uwp/design/basics/navigation-history-and-backwards-navigation">Navigationsverlauf und Rückwärtsnavigation</a></li>
<li><a href="/windows/uwp/design/controls-and-patterns/infobar">InfoBar</a></li>
<li><a href="/windows/uwp/design/controls-and-patterns/media-playback">Media Player</a></li>
<li><a href="/windows/uwp/design/controls-and-patterns/navigationview">Navigationsansicht</a></li>
<li><a href="/windows/uwp/design/input/cortana-deep-link-into-your-app">Deep-Link aus einer Hintergrund-App in Cortana zu einer Vordergrund-App – Cortana UWP-Entwurf und -Entwicklung</a></li>
<li><a href="/windows/uwp/design/input/cortana-design-guidelines">Cortana-Entwurfsrichtlinien – Cortana UWP-Entwurf und -Entwicklung</a></li>
<li><a href="/windows/uwp/design/input/cortana-dynamically-modify-voice-command-definition-vcd-phrase-lists">Dynamisches Ändern der Cortana-VCD-Ausdruckslisten – Cortana UWP-Entwurf und -Entwicklung</a></li>
<li><a href="/windows/uwp/design/input/cortana-interact-with-a-background-app">Interagieren mit einer Hintergrund-App in Cortana – Cortana UWP-Entwurf und -Entwicklung</a></li>
<li><a href="/windows/uwp/design/input/cortana-interactions">Cortana-Interaktionen in Windows-Apps</a></li>
<li><a href="/windows/uwp/design/input/cortana-launch-a-background-app-with-voice-commands">Aktivieren einer Hintergrund-App in Cortana mithilfe von Sprachbefehlen | Cortana UWP-Entwurf und -Entwicklung</a></li>
<li><a href="/windows/uwp/design/input/cortana-launch-a-foreground-app-with-voice-commands">Aktivieren einer Vordergrund-App mit Sprachbefehlen über Cortana – Cortana UWP-Entwurf und -Entwicklung</a></li>
<li><a href="/windows/uwp/design/input/cortana-support-natural-language-voice-commands">Unterstützung für natürlichere Sprachbefehle in Cortana – Cortana UWP-Entwurf und -Entwicklung</a></li>
<li><a href="/windows/uwp/gaming/about-the-uwp-user-interface-and-directx">Das App-Objekt und DirectX</a></li>
<li><a href="/windows/uwp/graphics-concepts/graphics-pipeline">Grafikpipeline</a></li>
<li><a href="/windows/uwp/monetize/get-the-stack-trace-for-an-error-in-your-desktop-application">Abrufen der Stapelüberwachung für einen Fehler in Ihrer Desktopanwendung</a></li>
<li><a href="/windows/uwp/monetize/manage-add-on-submissions">Verwalten von Add-On-Übermittlungen</a></li>
<li><a href="/windows/uwp/monetize/manage-add-ons">Verwalten von Add-Ons</a></li>
<li><a href="/windows/uwp/monetize/manage-app-submissions">Verwalten von App-Übermittlungen</a></li>
<li><a href="/windows/uwp/monetize/manage-flight-submissions">Verwalten von Flight-Paket-Übermittlungen</a></li>
<li><a href="/windows/uwp/monetize/view-and-grant-products-from-a-service">Verwalten von Produktansprüchen aus einem Dienst</a></li>
<li><a href="/windows/uwp/publish/set-custom-permissions-for-account-users">Festlegen von Rollen oder benutzerdefinierten Berechtigungen für Kontobenutzer</a></li>
<li><a href="/windows/uwp/xbox-apps/development-lanes-unity-versioning">Unity – Versionskontrolle für Ihr UWP-Projekt</a></li>
<li><a href="/windows/uwp/xbox-apps/uwp-deployinfo-api">Referenz zur API für Bereitstellungsinformationen des Geräteportals</a></li>
<li><a href="/windows/uwp/xbox-apps/uwp-networkcredentials-api">Referenz zur API für Netzwerkanmeldeinformationen des Geräteportals</a></li>
<li><a href="/windows/uwp/xbox-apps/uwp-remoteinput-api">Geräteportal-Remoteeingabe – API-Referenz</a></li>
<li><a href="/windows/uwp/xbox-apps/uwp-remoteinput-controllers-api">Geräteportal-Controller – API-Referenz</a></li>
<li><a href="/windows/uwp/xbox-apps/uwp-sshpins-api">Geräteportal-SSH-PINs – API-Referenz</a></li>
<li><a href="/windows/uwp/xbox-apps/wdp-media-capture-api">Referenz zur Medienerfassungs-API</a></li>
</ul>

## <a name="win32-conceptual"></a>Win32-Konzept

<ul>
<li><a href="/windows/desktop/Bits/using-windows-powershell-to-create-bits-transfer-jobs">Erstellen von BITS-Übertragungsaufträgen mithilfe von Windows PowerShell</a></li>
<li><a href="/windows/desktop/Bits/using-winrm-windows-powershell-cmdlets-to-manage-bits-transfer-jobs">Verwenden von WinRM Windows PowerShell-Cmdlets zum Verwalten von BITS-Übertragungsaufträgen</a></li>
<li><a href="/windows/desktop/CIMWin32Prov/win32-optionalfeature">Win32_OptionalFeature-Klasse</a></li>
<li><a href="/windows/desktop/CIMWin32Prov/win32-pnpdeviceproperty">Win32_PnPDeviceProperty-Klasse</a></li>
<li><a href="/windows/desktop/Controls/toolbar-standard-button-image-index-values">Abbildung einer Symbolleisten-Standardschaltfläche, Indexwerte (CommCtrl.h)</a></li>
<li><a href="/windows/desktop/Direct2D/border">Rahmen-Effekt</a></li>
<li><a href="/windows/desktop/Direct2D/profiling-directx-applications">Profilerstellung für DirectX-Apps</a></li>
<li><a href="/windows/desktop/DirectWrite/dwritecore-overview">Übersicht über DWriteCore</a></li>
<li><a href="/windows/desktop/DxTechArts/installing-and-using-input-method-editors">Installieren und Verwenden von Eingabemethoden-Editoren</a></li>
<li><a href="/windows/desktop/HyperV_v2/getting-the-virtual-system-dns-name">Abrufen des DNS-Namens des virtuellen Computers</a></li>
<li><a href="/windows/desktop/LearnWin32/appendix--matrix-transforms">Anhang Matrixtransformationen</a></li>
<li><a href="/windows/desktop/Midl/-target">/target-Schalter</a></li>
<li><a href="/windows/desktop/Midl/declare-guid">declare_guid-Attribut</a></li>
<li><a href="/windows/desktop/Midl/sh-composition">sh_composition-Schlüsselwort</a></li>
<li><a href="/windows/desktop/Midl/sh-event">sh_event-Schlüsselwort</a></li>
<li><a href="/windows/desktop/Midl/sh-file">sh_file-Schlüsselwort</a></li>
<li><a href="/windows/desktop/Midl/sh-job">sh_job-Schlüsselwort</a></li>
<li><a href="/windows/desktop/Midl/sh-mutex">sh_mutex-Schlüsselwort</a></li>
<li><a href="/windows/desktop/Midl/sh-pipe">sh_pipe-Schlüsselwort</a></li>
<li><a href="/windows/desktop/Midl/sh-process">sh_process-Schlüsselwort</a></li>
<li><a href="/windows/desktop/Midl/sh-reg-key">sh_reg_key-Schlüsselwort</a></li>
<li><a href="/windows/desktop/Midl/sh-section">sh_section-Schlüsselwort</a></li>
<li><a href="/windows/desktop/Midl/sh-semaphore">sh_semaphore-Schlüsselwort</a></li>
<li><a href="/windows/desktop/Midl/sh-socket">sh_socket-Schlüsselwort</a></li>
<li><a href="/windows/desktop/Midl/sh-thread">sh_thread-Schlüsselwort</a></li>
<li><a href="/windows/desktop/Midl/sh-token">sh_token-Schlüsselwort</a></li>
<li><a href="/windows/desktop/Midl/system-handle">system_handle-Attribut</a></li>
<li><a href="/windows/desktop/Msi/patching-per-user-managed-applications">Patchen von verwalteten Anwendungen pro Benutzer</a></li>
<li><a href="/windows/desktop/Multimedia/buffered-services">Gepufferte Dienste</a></li>
<li><a href="/windows/desktop/NDF/using-network-monitor-to-view-etl-files">Verwenden von Microsoft-Netzwerkmonitor zum Anzeigen von ETL-Dateien</a></li>
<li><a href="/windows/desktop/OpenGL/glfogf">glFogf-Funktion (Gl.h)</a></li>
<li><a href="/windows/desktop/OpenGL/glfogfv">glFogfv-Funktion (Gl.h)</a></li>
<li><a href="/windows/desktop/OpenGL/glfogi">glFogi-Funktion (Gl.h)</a></li>
<li><a href="/windows/desktop/OpenGL/glfogiv">glFogiv-Funktion (Gl.h)</a></li>
<li><a href="/windows/desktop/OpenGL/glmap1d">glMap1d-Funktion (Gl.h)</a></li>
<li><a href="/windows/desktop/OpenGL/glmap1f">glMap1f-Funktion (Gl.h)</a></li>
<li><a href="/windows/desktop/OpenGL/glmap2d">glMap2d-Funktion (Gl.h)</a></li>
<li><a href="/windows/desktop/OpenGL/glmap2f">glMap2f-Funktion (Gl.h)</a></li>
<li><a href="/windows/desktop/ProcThread/using-thread-local-storage">Verwenden von TLS (threadlokaler Speicher)</a></li>
<li><a href="/windows/desktop/SecAuthN/protocols-in-tls-ssl--schannel-ssp-">Protokolle in TLS/SSL (Schannel SSP)</a></li>
<li><a href="/windows/desktop/SecAuthN/schannel-cipher-suites-in-windows-vista">TLS Cipher-Suites in Windows Vista</a></li>
<li><a href="/windows/desktop/SecCrypto/alternatives-to-using-capicom">Alternativen zur Verwendung von CAPICOM</a></li>
<li><a href="/windows/desktop/SecCrypto/cryptuidlgselectcertificate">CryptUIDlgSelectCertificate-Funktion</a></li>
<li><a href="/windows/desktop/Sync/synchronization-and-multiprocessor-issues">Synchronisierungs- und Multiprozessorprobleme</a></li>
<li><a href="/windows/desktop/VDS/volume-and-lun-binding">Volume- und LUN-Bindung</a></li>
<li><a href="/windows/desktop/VSS/setting-vss-restore-methods">Festlegen von VSS-Wiederherstellungsmethoden</a></li>
<li><a href="/windows/desktop/Win7AppQual/preventing-hangs-in-windows-applications">Verhindern von Blockaden in Windows-Anwendungen</a></li>
<li><a href="/windows/desktop/WinProg/large-integer-structures">Large Integer-Strukturen</a></li>
<li><a href="/windows/desktop/WinProg64/shared-registry-keys">Registrierungsschlüssel, die von WOW64 betroffen sind</a></li>
<li><a href="/windows/desktop/WinRM/winrm-powershell-commandlets">Verwaltete Referenz für WinRM Windows PowerShell-Befehlsklassen</a></li>
<li><a href="/windows/desktop/WinSock/ipproto-ip-socket-options">IPPROTO_IP-Socketoptionen</a></li>
<li><a href="/windows/desktop/WinSock/ipproto-ipv6-socket-options">IPPROTO_IPV6-Socketoptionen</a></li>
<li><a href="/windows/desktop/WinSock/winsock-ioctls">Winsock IOCTLs (Winsock2.h)</a></li>
<li><a href="/windows/desktop/application-installing-and-servicing">Anwendungsinstallation und -wartung</a></li>
<li><a href="/windows/desktop/cossdk/configuring-security-for-library-applications">Konfigurieren der Sicherheit für Bibliotheksanwendungen</a></li>
<li><a href="/windows/desktop/delivery_optimization/ibackgroundcopymanager-createjob">IBackgroundCopyManager CreateJob-Methode (Deliveryoptimization.h)</a></li>
<li><a href="/windows/desktop/direct3d12/creating-a-basic-direct3d-12-component">Erstellen einer einfachen Direct3D 12-Komponente</a></li>
<li><a href="/windows/desktop/direct3d12/d3d12decomposesubresource">D3D12DecomposeSubresource-Funktion (D3dx12.h)</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ne-directml-dml_depth_space_order">DML_DEPTH_SPACE_ORDER</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ne-directml-dml_graph_edge_type">DML_GRAPH_EDGE_TYPE</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ne-directml-dml_graph_node_type">DML_GRAPH_NODE_TYPE</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_depth_to_space1_operator_desc">DML_DEPTH_TO_SPACE1_OPERATOR_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_graph_desc">DML_GRAPH_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_graph_edge_desc">DML_GRAPH_EDGE_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_graph_node_desc">DML_GRAPH_NODE_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_max_pooling_grad_operator_desc">DML_MAX_POOLING_GRAD_OPERATOR_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_random_generator_operator_desc">DML_RANDOM_GENERATOR_OPERATOR_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_space_to_depth1_operator_desc">DML_SPACE_TO_DEPTH1_OPERATOR_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/directml/ns-directml-dml_top_k1_operator_desc">DML_TOP_K1_OPERATOR_DESC</a></li>
<li><a href="/windows/desktop/direct3d12/dynamic-indexing-using-hlsl-5-1">Dynamische Indizierung mit HLSL 5.1</a></li>
<li><a href="/windows/desktop/direct3d9/d3dxboxboundprobe">D3DXBoxBoundProbe-Funktion (D3DX9Mesh.h)</a></li>
<li><a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-state-object">Statusobjekte</a></li>
<li><a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl">High-Level-Shaderprogrammiersprache (HLSL)</a></li>
<li><a href="/windows/desktop/directx-sdk--august-2009-">Wo finde ich das DirectX SDK?</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/columnvalueofstruct-t-class">ColumnValueOfStruct(T)-Klasse</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentcorruptionexception-class">EsentCorruptionException-Klasse</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentfatalexception-class">EsentFatalException-Klasse</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentfragmentationexception-class">EsentFragmentationException-Klasse</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentinconsistentexception-class">EsentInconsistentException-Klasse</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentmemoryexception-class">EsentMemoryException-Klasse</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentobsoleteexception-class">EsentObsoleteException-Klasse</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentoperationexception-class">EsentOperationException-Klasse</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentstateexception-class">EsentStateException-Klasse</a></li>
<li><a href="/windows/desktop/extensible-storage-engine/esentusageexception-class">EsentUsageException-Klasse</a></li>
<li><a href="/windows/desktop/gdiplus/-gdiplus-flatapi-flat">GDI+ Flat-API</a></li>
<li><a href="/windows/desktop/inputdev/using-raw-input">Verwenden von Rohdateneingaben</a></li>
<li><a href="/windows/desktop/inputmsg/wm-pointerup">WM_POINTERUP-Meldung</a></li>
<li><a href="/windows/desktop/lwef/animations">Animationen</a></li>
<li><a href="/windows/desktop/lwef/creating-animations">Erstellen von Animationen</a></li>
<li><a href="/windows/desktop/lwef/iagentcharacterex--getanimationnames">IAgentCharacterEx GetAnimationNames</a></li>
<li><a href="/windows/desktop/lwef/toolbar-buttons-">Schaltflächen der Symbolleiste (Microsoft Agent Character Editor)</a></li>
<li><a href="/windows/desktop/lwef/toolbar-buttons">Schaltflächen der Symbolleiste (Linguistic Information Sound Editing Tool)</a></li>
<li><a href="/windows/desktop/medfound/direct3d-video-motion-estimation">Direct3D-Video-Bewegungsschätzung</a></li>
<li><a href="/windows/desktop/shell/band-objects">Erstellen von benutzerdefinierten Explorer-Leisten, Tool-Bändern und Desk-Bändern</a></li>
<li><a href="/windows/desktop/tablet/security-and-trust">Sicherheit und Vertrauenswürdigkeit</a></li>
<li><a href="/windows/desktop/tablet/using-the-managed-library">Verwenden der verwalteten Bibliothek</a></li>
<li><a href="/windows/desktop/updateorchestrator/index">Update Orchestrator-API</a></li>
<li><a href="/windows/desktop/updateorchestrator/universalorchestrator-hasmoratoriumpassed">IUniversalOrchestrator::HasMoratoriumPassed</a></li>
<li><a href="/windows/desktop/updateorchestrator/universalorchestrator-iuniversalorchestrator">IUniversalOrchestrator-Schnittstelle</a></li>
<li><a href="/windows/desktop/updateorchestrator/universalorchestrator-schedulework">IUniversalOrchestrator::ScheduleWork</a></li>
<li><a href="/windows/desktop/updateorchestrator/universalorchestrator-workcompleted">IUniversalOrchestrator::WorkCompleted</a></li>
<li><a href="/windows/desktop/uxguide/cmd-toolbars">Symbolleisten</a></li>
<li><a href="/windows/desktop/uxguide/ctrl-balloons">Ballons</a></li>
<li><a href="/windows/desktop/uxguide/ctrl-command-buttons">Befehlsschaltflächen</a></li>
<li><a href="/windows/desktop/uxguide/ctrl-command-links">Befehlsverknüpfungen</a></li>
<li><a href="/windows/desktop/uxguide/ctrl-drop">Kombinationsfelder für Dropdownlisten</a></li>
<li><a href="/windows/desktop/uxguide/ctrl-group-boxes">Gruppenfelder</a></li>
<li><a href="/windows/desktop/uxguide/ctrl-search-boxes">Suchfelder</a></li>
<li><a href="/windows/desktop/uxguide/glossary">Glossar (Entwurfsgrundlagen)</a></li>
<li><a href="/windows/desktop/uxguide/inter-mouse">Maus und Zeiger</a></li>
<li><a href="/windows/desktop/uxguide/inter-pen">Stift</a></li>
<li><a href="/windows/desktop/uxguide/inter-touch">Toucheingabe</a></li>
<li><a href="/windows/desktop/uxguide/mess-confirm">Bestätigungen</a></li>
<li><a href="/windows/desktop/uxguide/mess-error">Fehlermeldungen (Entwurfsgrundlagen)</a></li>
<li><a href="/windows/desktop/uxguide/vis-animations">Animationen und Übergänge</a></li>
<li><a href="/windows/desktop/uxguide/vis-graphic">Grafische Elemente</a></li>
<li><a href="/windows/desktop/uxguide/vis-icons">Symbole (Entwurfsgrundlagen)</a></li>
<li><a href="/windows/desktop/uxguide/vis-std-icons">Standardsymbole</a></li>
<li><a href="/windows/desktop/uxguide/win-wizards">Assistenten</a></li>
<li><a href="/windows/desktop/uxguide/winenv-notification">Infobereich</a></li>
<li><a href="/windows/desktop/w8cookbook/secured-boot">Antischadsoftware-Frühstart</a></li>
<li><a href="/windows/desktop/wcs/a">A</a></li>
<li><a href="/windows/desktop/wcs/about-windows-color-system-version-1-0">Informationen zu Windows Color System (WCS), Version 1.0</a></li>
<li><a href="/windows/desktop/wcs/additive-primary-colors">Additive Primärfarben</a></li>
<li><a href="/windows/desktop/wcs/advanced-functions-for-use-outside-of-a-device-context">Erweiterte Funktionen zur Verwendung außerhalb eines Gerätekontexts</a></li>
<li><a href="/windows/desktop/wcs/alphabetical-list-of-all-wcs-functions">Alphabetische Liste aller WCS-Funktionen</a></li>
<li><a href="/windows/desktop/wcs/b">B</a></li>
<li><a href="/windows/desktop/wcs/basic-color-management-concepts">Konzepte zur Verwaltung der Basisfarben</a></li>
<li><a href="/windows/desktop/wcs/basic-functions-for-use-within-a-device-context">Grundlegende Funktionen zur Verwendung innerhalb eines Gerätekontexts</a></li>
<li><a href="/windows/desktop/wcs/c">C</a></li>
<li><a href="/windows/desktop/wcs/cmm-transform-creation-flags">CMM-Transformationserstellungsflags</a></li>
<li><a href="/windows/desktop/wcs/cmy-and-cmyk-color-spaces">CMY- und CMYK-Farbraum</a></li>
<li><a href="/windows/desktop/wcs/color-conversion-and-color-matching">Farbkonvertierung und Farbübereinstimmung</a></li>
<li><a href="/windows/desktop/wcs/color-in-imaging">Farbe in der Bildverarbeitung</a></li>
<li><a href="/windows/desktop/wcs/color-space-constants">Farbraumkonstanten</a></li>
<li><a href="/windows/desktop/wcs/color-space-type-identifiers">Farbraum-Typbezeichner</a></li>
<li><a href="/windows/desktop/wcs/color-spaces">Farbräume</a></li>
<li><a href="/windows/desktop/wcs/common-color-messages">Allgemeine Farbmeldungen</a></li>
<li><a href="/windows/desktop/wcs/d">D</a></li>
<li><a href="/windows/desktop/wcs/describing-color">Beschreiben von Farbe</a></li>
<li><a href="/windows/desktop/wcs/device-calibration-and-characterization-functions">Geräterkalibrierung und Charakterisierungsfunktionen</a></li>
<li><a href="/windows/desktop/wcs/device-dependent-color-spaces">Geräteabhängige Farbräume</a></li>
<li><a href="/windows/desktop/wcs/device-independent-color-spaces">Geräteunabhängige Farbräume</a></li>
<li><a href="/windows/desktop/wcs/enumerations">Enumerationen</a></li>
<li><a href="/windows/desktop/wcs/error-codes-specific-to-wcs">WCS-spezifische Fehlercodes</a></li>
<li><a href="/windows/desktop/wcs/functions">Funktionen</a></li>
<li><a href="/windows/desktop/wcs/further-information">Weitere Informationen</a></li>
<li><a href="/windows/desktop/wcs/g">G</a></li>
<li><a href="/windows/desktop/wcs/h">H</a></li>
<li><a href="/windows/desktop/wcs/hls-color-spaces">HLS-Farbräume</a></li>
<li><a href="/windows/desktop/wcs/hsv-color-spaces">HSV-Farbräume</a></li>
<li><a href="/windows/desktop/wcs/human-color-perception">Menschliche Farbwahrnehmung</a></li>
<li><a href="/windows/desktop/wcs/icmprogressproccallback">ICMProgressProcCallback-Rückruffunktion</a></li>
<li><a href="/windows/desktop/wcs/interfaces">Schnittstellen</a></li>
<li><a href="/windows/desktop/wcs/l">L</a></li>
<li><a href="/windows/desktop/wcs/macros-for-cmyk-values-and-colors">Makros für CMYK-Werte und -Farben</a></li>
<li><a href="/windows/desktop/wcs/obsolete-wcs-functions">Veraltete WCS-Funktionen</a></li>
<li><a href="/windows/desktop/wcs/p">P</a></li>
<li><a href="/windows/desktop/wcs/profile-management-functions">Profilverwaltungsfunktionen</a></li>
<li><a href="/windows/desktop/wcs/r">R</a></li>
<li><a href="/windows/desktop/wcs/reference">Referenz</a></li>
<li><a href="/windows/desktop/wcs/rendering-intents">Rendering von Absichten</a></li>
<li><a href="/windows/desktop/wcs/rgb-color-spaces">RGB-Farbräume</a></li>
<li><a href="/windows/desktop/wcs/s">S</a></li>
<li><a href="/windows/desktop/wcs/security-considerations--windows-color-system">Sicherheitsüberlegungen für Windows Color System</a></li>
<li><a href="/windows/desktop/wcs/srgb--a-standard-color-space">sRGB Standardfarbraum</a></li>
<li><a href="/windows/desktop/wcs/structures">Strukturen</a></li>
<li><a href="/windows/desktop/wcs/subtractive-primary-colors">Subtraktive Primärfarben</a></li>
<li><a href="/windows/desktop/wcs/t">T</a></li>
<li><a href="/windows/desktop/wcs/using-color-management-modules--cmm">Verwenden von Farbverwaltungsmodulen (CMM)</a></li>
<li><a href="/windows/desktop/wcs/using-color-management-on-the-internet">Verwenden von Farbverwaltung im Internet</a></li>
<li><a href="/windows/desktop/wcs/using-device-profiles-with-wcs">Verwenden von Geräteprofilen mit WCS</a></li>
<li><a href="/windows/desktop/wcs/using-gdi-functions-with-wcs">Verwenden von GDI-Funktionen mit WCS</a></li>
<li><a href="/windows/desktop/wcs/using-structures-in-wcs-1-0">Verwenden von Strukturen in WCS 1.0</a></li>
<li><a href="/windows/desktop/wcs/using-the-color-mapping-process-with-wcs">Verwenden des Farbzuordnungsprozesses mit WCS</a></li>
<li><a href="/windows/desktop/wcs/using-wcs-1-0">Verwenden von WCS 1.0</a></li>
<li><a href="/windows/desktop/wcs/w">W</a></li>
<li><a href="/windows/desktop/wcs/wcs-1-0-availability">WCS 1.0-Verfügbarkeit</a></li>
<li><a href="/windows/desktop/wcs/wcs-1-0-glossary">WCS 1.0-Glossar</a></li>
<li><a href="/windows/desktop/wcs/wcs-calibration-schema">WCS-Kalibrierungsschema</a></li>
<li><a href="/windows/desktop/wcs/wcs-color-appearance-model-profile-schema-and-algorithm">WCS-Farbdarstellungsmodell: Profilschema und Algorithmus</a></li>
<li><a href="/windows/desktop/wcs/wcs-color-device-model-profile-schema-and-algorithms">WCS-Farbgerätemodell: Profilschema und Algorithmen</a></li>
<li><a href="/windows/desktop/wcs/wcs-constants">WCS-Konstanten</a></li>
<li><a href="/windows/desktop/wcs/wcs-functions-for-color-management-modules--cmms--to-implement">WCS-Funktionen für die zu implementierenden Farbverwaltungsmodule (CMMs)</a></li>
<li><a href="/windows/desktop/wcs/wcs-gamut-map-model-profile-schema-and-algorithms">WCS-Farbauswahlpalette: Profilschema und Algorithmen</a></li>
<li><a href="/windows/desktop/wcs/wcs-registry-keys">WCS-Registrierungsschlüssel</a></li>
<li><a href="/windows/desktop/wcs/wcs-transform-creation-algorithms">WCS-Transformationserstellungsalgorithmen</a></li>
<li><a href="/windows/desktop/wcs/what-s-new-in-version-1-0-of-wcs">Neuerungen in Version 1.0 von WCS</a></li>
<li><a href="/windows/desktop/wcs/what-s-new-in-windows-vista">Neuerungen in Windows Vista</a></li>
<li><a href="/windows/desktop/wcs/windows-color-system-common-profile-types-schema--versioning-and-localization-strategies">Windows Color System: Schema gemeinsamer Profiltypen, Versionierung und Lokalisierungsstrategien</a></li>
<li><a href="/windows/desktop/wcs/windows-color-system-schemas-and-algorithms">Windows Color System: Schemas und Algorithmen</a></li>
<li><a href="/windows/desktop/wcs/windows-color-system">Windows Color System (WCS)</a></li>
<li><a href="/windows/desktop/wmformat/drm-for-more-information">Weitere Informationen (Microsoft Windows Media DRM-Client)</a></li>
<li><a href="/windows/desktop/wmformat/for-more-information">Weitere Informationen (Windows Media Format SDK)</a></li>
</ul>

## <a name="win32-api-reference"></a>Win32-API-Referenz

<ul>
<li><a href="/windows/win32/api/cimfs/index">cimfs </a></li>
<li><a href="/windows/win32/api/commctrl/nf-commctrl-loadiconmetric">LoadIconMetric-Funktion (commctrl.h) </a></li>
<li><a href="/windows/win32/api/d3d12/ne-d3d12-d3d12_pipeline_state_subobject_type">D3D12_PIPELINE_STATE_SUBOBJECT_TYPE (d3d12.h) </a></li>
<li><a href="/windows/win32/api/d3d12/nf-d3d12-id3d12device2-createpipelinestate">ID3D12Device2::CreatePipelineState (d3d12.h) </a></li>
<li><a href="/windows/win32/api/d3d12/nf-d3d12-id3d12pipelinelibrary1-loadpipeline">ID3D12PipelineLibrary1::LoadPipeline (d3d12.h) </a></li>
<li><a href="/windows/win32/api/d3d12/ns-d3d12-d3d12_pipeline_state_stream_desc">D3D12_PIPELINE_STATE_STREAM_DESC (d3d12.h) </a></li>
<li><a href="/windows/win32/api/dxgi/nf-dxgi-idxgifactory-makewindowassociation">IDXGIFactory::MakeWindowAssociation (dxgi.h) </a></li>
<li><a href="/windows/win32/api/icm/nc-icm-pbmcallbackfn">PBMCALLBACKFN </a></li>
<li><a href="/windows/win32/api/icm/nc-icm-pcmscallbacka">PCMSCALLBACKA </a></li>
<li><a href="/windows/win32/api/icm/nc-icm-pcmscallbackw">PCMSCALLBACKW </a></li>
<li><a href="/windows/win32/api/icm/ne-icm-bmformat">BMFORMAT </a></li>
<li><a href="/windows/win32/api/icm/ne-icm-colordatatype">COLORDATATYPE </a></li>
<li><a href="/windows/win32/api/icm/ne-icm-colorprofilesubtype">COLORPROFILESUBTYPE </a></li>
<li><a href="/windows/win32/api/icm/ne-icm-colorprofiletype">COLORPROFILETYPE </a></li>
<li><a href="/windows/win32/api/icm/ne-icm-colortype">COLORTYPE </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-associatecolorprofilewithdevicea">AssociateColorProfileWithDeviceA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-associatecolorprofilewithdevicew">AssociateColorProfileWithDeviceW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-checkbitmapbits">CheckBitmapBits </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-checkcolors">CheckColors </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-closecolorprofile">CloseColorProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcheckcolors">CMCheckColors </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcheckcolorsingamut">CMCheckColorsInGamut </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmconvertcolornametoindex">CMConvertColorNameToIndex </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmconvertindextocolorname">CMConvertIndexToColorName </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcreatedevicelinkprofile">CMCreateDeviceLinkProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcreatemultiprofiletransform">CMCreateMultiProfileTransform </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcreateprofile">CMCreateProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcreateprofilew">CMCreateProfileW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcreatetransform">CMCreateTransform </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmcreatetransformw">CMCreateTransformW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmdeletetransform">CMDeleteTransform </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmgetinfo">CMGetInfo </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmgetnamedprofileinfo">CMGetNamedProfileInfo </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmgetps2colorrenderingdictionary">CMGetPS2ColorRenderingDictionary </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmgetps2colorspacearray">CMGetPS2ColorSpaceArray </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmisprofilevalid">CMIsProfileValid </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmtranslatecolors">CMTranslateColors </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-cmtranslatergbsext">CMTranslateRGBsExt </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-coloradaptergetdisplaycurrentstateid">ColorAdapterGetDisplayCurrentStateID </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-coloradaptergetdisplaytransformdata">ColorAdapterGetDisplayTransformData </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-coloradaptergetsystemmodifywhitepointcaps">ColorAdapterGetSystemModifyWhitePointCaps </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-coloradapterregisteroemcolorservice">ColorAdapterRegisterOEMColorService </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-coloradapterunregisteroemcolorservice">ColorAdapterUnregisterOEMColorService </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-coloradapterupdatedeviceprofile">ColorAdapterUpdateDeviceProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-coloradapterupdatedisplaygamma">ColorAdapterUpdateDisplayGamma </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-colorprofileadddisplayassociation">ColorProfileAddDisplayAssociation </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-colorprofilegetdisplaydefault">ColorProfileGetDisplayDefault </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-colorprofilegetdisplaylist">ColorProfileGetDisplayList </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-colorprofilegetdisplayuserscope">ColorProfileGetDisplayUserScope </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-colorprofileremovedisplayassociation">ColorProfileRemoveDisplayAssociation </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-colorprofilesetdisplaydefaultassociation">ColorProfileSetDisplayDefaultAssociation </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-createcolortransforma">CreateColorTransformA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-createcolortransformw">CreateColorTransformW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-createdevicelinkprofile">CreateDeviceLinkProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-createmultiprofiletransform">CreateMultiProfileTransform </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-createprofilefromlogcolorspacea">CreateProfileFromLogColorSpaceA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-createprofilefromlogcolorspacew">CreateProfileFromLogColorSpaceW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-deletecolortransform">DeleteColorTransform </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-enumcolorprofilesa">EnumColorProfilesA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-enumcolorprofilesw">EnumColorProfilesW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getcmminfo">GetCMMInfo </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getcolorprofileelement">GetColorProfileElement </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getcolorprofileelementtag">GetColorProfileElementTag </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getcolorprofilefromhandle">GetColorProfileFromHandle </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getcolorprofileheader">GetColorProfileHeader </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getcountcolorprofileelements">GetCountColorProfileElements </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getnamedprofileinfo">GetNamedProfileInfo </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getps2colorrenderingdictionary">GetPS2ColorRenderingDictionary </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getps2colorspacearray">GetPS2ColorSpaceArray </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getstandardcolorspaceprofilea">GetStandardColorSpaceProfileA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-getstandardcolorspaceprofilew">GetStandardColorSpaceProfileW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-installcolorprofilea">InstallColorProfileA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-installcolorprofilew">InstallColorProfileW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-iscolorprofiletagpresent">IsColorProfileTagPresent </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-iscolorprofilevalid">IsColorProfileValid </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-opencolorprofilea">OpenColorProfileA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-opencolorprofilew">OpenColorProfileW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-selectcmm">SelectCMM </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-setcolorprofileelement">SetColorProfileElement </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-setcolorprofileelementreference">SetColorProfileElementReference </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-setcolorprofileelementsize">SetColorProfileElementSize </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-setcolorprofileheader">SetColorProfileHeader </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-setupcolormatchinga">SetupColorMatchingA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-setupcolormatchingw">SetupColorMatchingW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-translatebitmapbits">TranslateBitmapBits </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-translatecolors">TranslateColors </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-unregistercmma">UnregisterCMMA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-unregistercmmw">UnregisterCMMW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsassociatecolorprofilewithdevice">WcsAssociateColorProfileWithDevice </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcscheckcolors">WcsCheckColors </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcscreateiccprofile">WcsCreateIccProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsdisassociatecolorprofilefromdevice">WcsDisassociateColorProfileFromDevice </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsenumcolorprofiles">WcsEnumColorProfiles </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsenumcolorprofilessize">WcsEnumColorProfilesSize </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsgetcalibrationmanagementstate">WcsGetCalibrationManagementState </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsgetdefaultcolorprofile">WcsGetDefaultColorProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsgetdefaultcolorprofilesize">WcsGetDefaultColorProfileSize </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsgetdefaultrenderingintent">WcsGetDefaultRenderingIntent </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsgetuseperuserprofiles">WcsGetUsePerUserProfiles </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsopencolorprofilea">WcsOpenColorProfileA </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcsopencolorprofilew">WcsOpenColorProfileW </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcssetcalibrationmanagementstate">WcsSetCalibrationManagementState </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcssetdefaultcolorprofile">WcsSetDefaultColorProfile </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcssetdefaultrenderingintent">WcsSetDefaultRenderingIntent </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcssetuseperuserprofiles">WcsSetUsePerUserProfiles </a></li>
<li><a href="/windows/win32/api/icm/nf-icm-wcstranslatecolors">WcsTranslateColors </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-cmykcolor">CMYKCOLOR </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-displaystateid">DisplayStateID </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-displaytransformlut">DisplayTransformLut </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-enumtypea">ENUMTYPEA </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-enumtypew">ENUMTYPEW </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-generic3channel">GENERIC3CHANNEL </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-graycolor">GRAYCOLOR </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-hificolor">HiFiCOLOR </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-labcolor">LabCOLOR </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-named_profile_info">NAMED_PROFILE_INFO </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-namedcolor">NAMEDCOLOR </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-profile">PROFILE </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-profileheader">PROFILEHEADER </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-rgbcolor">RGBCOLOR </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-xyypoint">XYYPoint </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-xyzcolor">XYZCOLOR </a></li>
<li><a href="/windows/win32/api/icm/ns-icm-yxycolor">YxyCOLOR </a></li>
<li><a href="/windows/win32/api/processthreadsapi/nf-processthreadsapi-terminateprocess">TerminateProcess-Funktion (processthreadsapi.h) </a></li>
<li><a href="/windows/win32/api/projectedfslib/ns-projectedfslib-prj_callbacks">PRJ_CALLBACKS (projectedfslib.h) </a></li>
<li><a href="/windows/win32/api/propidl/ns-propidl-propvariant">PROPVARIANT (propidl.h) </a></li>
<li><a href="/windows/win32/api/shellapi/ns-shellapi-shellexecuteinfoa">SHELLEXECUTEINFOA (shellapi.h) </a></li>
<li><a href="/windows/win32/api/shellapi/ns-shellapi-shellexecuteinfow">SHELLEXECUTEINFOW (shellapi.h) </a></li>
<li><a href="/windows/win32/api/shobjidl_core/nf-shobjidl_core-shassocenumhandlers">SHAssocEnumHandlers-Funktion (shobjidl_core.h) </a></li>
<li><a href="/windows/win32/api/winbase/nf-winbase-createprocesswithlogonw">CreateProcessWithLogonW-Funktion (winbase.h) </a></li>
<li><a href="/windows/win32/api/winbase/nf-winbase-createprocesswithtokenw">CreateProcessWithTokenW-Funktion (winbase.h) </a></li>
<li><a href="/windows/win32/api/winsock2/nf-winsock2-recvfrom">recvfrom-Funktion (winsock2.h) </a></li>
<li><a href="/windows/win32/api/wmp/nf-wmp-iwmpsettings-get_baseurl">IWMPSettings::get_baseURL (wmp.h) </a></li>
</ul>

## <a name="uwp-api-reference"></a>UWP-API-Referenz

<ul>
<li><a href="/uwp/api/windows.ui.xaml.debugsettings.enableframeratecounter">Windows.UI.Xaml.DebugSettings.EnableFrameRateCounter</a></li>
</ul>