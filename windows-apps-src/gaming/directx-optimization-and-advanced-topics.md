---
title: Optimierung und erweiterte Themen für DirectX-Spiele
description: Weitere Informationen zur Optimierung der DirectX-Spielleistung und anderen erweiterten Themen finden Sie in den Artikeln.
ms.assetid: b5f29fb2-3bcf-43b2-9a68-f8819473bf62
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Game, DirectX, optimieren, Multisampling, Austausch Ketten
ms.localizationpriority: medium
ms.openlocfilehash: f19081a2618ea683c7790db3cd7517078f19269c
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053650"
---
# <a name="optimization-and-advanced-topics-for-directx-games"></a>Optimierung und erweiterte Themen für DirectX-Spiele

Dieser Abschnitt enthält Informationen zum Optimieren der Leistung von DirectX-spielen und anderen erweiterten Themen.

Im Thema asynchronen Programming for Games werden die verschiedenen Punkte erläutert, die Sie beachten sollten, wenn Sie die asynchrone Programmierung verwenden möchten, um einige der Komponenten zu parallelisieren und Multithreading zu verwenden, um die Verwendung einer leistungsfähigen GPU zu maximieren.

Behandeln von mit dem Gerät entfernten Szenarios in Direct3D 11 in einer exemplarischen Vorgehensweise wird erläutert, wie Spiele, die mithilfe von Direct3D 11 entwickelt wurden, Situationen erkennen und reagieren können, in denen der Grafikadapter zurückgesetzt, entfernt oder geändert wird.

Im Thema Multisampling in UWP-apps finden Sie eine Übersicht über die Verwendung von Multisample-Antialiasing, eine Grafik Technik, mit der die Darstellung von Rand Kanten in UWP-spielen, die mit Direct3D erstellt wurden, reduziert wird.

Thema zur Optimierung der Eingabe-und Renderingschleife enthält Anleitungen zum Auswählen der richtigen Eingabe Ereignis Verarbeitungsoption zum Verwalten der Eingabe Latenz und zum Optimieren der Renderingschleife.

Reduzieren der Latenzzeit mit DXGI 1,3-Auslagerungs Ketten erläutert, wie Sie die effektive Frame Latenz verringern, indem Sie darauf warten, dass die SwapChain die entsprechende Zeit zum Rendern eines neuen Frames signalisiert.

Im Thema austauschen von Verlaufs-und Überlagerungen in der Gruppe wird erläutert, wie Sie Renderingzeiten mithilfe von skalierten Austausch Ketten verbessern können, um Echtzeitspiel Inhalte mit einer niedrigeren Auflösung zu rendern, als die Anzeigesystem Außerdem wird erläutert, wie Überlagerungs Austausch Ketten für Geräte mit der Hardware Überlagerungs Funktion erstellt werden. Diese Technik kann verwendet werden, um das Problem einer herunterskalierten Benutzeroberfläche zu verringern, die sich aus der Verwendung der Auslagerungs Ketten Skalierung ergibt.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="asynchronous-programming-directx-and-cpp.md">Asynchrone Programmierung für Spiele</a></p></td>
<td align="left"><p>Grundlegendes zum asynchronen programmieren und Threading mit DirectX.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="handling-device-lost-scenarios.md">Behandeln von entfernten Geräte Szenarien in Direct3D 11</a></p></td>
<td align="left"><p>Erstellen Sie die Geräteschnittstellen Kette Direct3D und DXGI neu, wenn der Grafikadapter entfernt oder erneut initialisiert wird.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md">Multisampling in UWP-Apps</a></p></td>
<td align="left"><p>Verwenden Sie multisamplinggrad in UWP-spielen, die mit Direct3D erstellt wurden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md">Optimieren von Eingabe und Renderschleife</a></p></td>
<td align="left"><p>Verringern Sie die Eingabe Latenz und optimieren Sie die Renderingschleife.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="reduce-latency-with-dxgi-1-3-swap-chains.md">Reduzieren der Latenz mit DXGI 1.3-Swapchains</a></p></td>
<td align="left"><p>Verwenden Sie DXGI 1,3, um die effektive Frame Latenz zu verringern.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multisampling--scaling--and-overlay-swap-chains.md">Swapchainskalierung und Überlagerungen</a></p></td>
<td align="left"><p>Erstellen Sie skalierte Austausch Ketten für schnelleres Rendering auf mobilen Geräten, und verwenden Sie Überlagerungs Austausch Ketten, um die visuelle Qualität zu erhöhen.</p></td>
</tr>
</tbody>
</table>