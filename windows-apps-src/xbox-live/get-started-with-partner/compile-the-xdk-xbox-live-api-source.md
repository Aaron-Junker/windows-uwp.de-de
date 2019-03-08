---
title: Kompilieren Sie die Quelle xdk-Version Xbox Live-API
description: Erfahren Sie, wie die Xbox Live-API-Quelle kompiliert wird, die mit der Xbox (xdk Developer Kit-Version) geliefert wird.
ms.assetid: 78425e82-c132-4f6b-9db3-2536862f1ce5
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, xdk-Version
ms.localizationpriority: medium
ms.openlocfilehash: 9a98af637c8c60449cd2005c4fc6f83f9b0719cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660995"
---
# <a name="compile-the-xbox-developer-kit-xdk-xbox-live-api-source"></a>Kompilieren Sie die Xbox Developer Kit (xdk-Version) Xbox Live-API-Quelle

Der xdk (Xbox Developer Kit-Version) umfasst die Quelle für die Erstellung der Microsoft.Xbox.Services.dll (XSAPI). Entwickler können diese Anweisungen, um ihre Verwendung ein lokales Builds der DLL-Projekte aktualisieren folgen.

Sie möchten möglicherweise XSAPI selbst wenn erstellen:
1. Wenn Sie ein Problem, um zu verstehen, woher ein Fehlercode stammt debuggen möchten.
1. Wenn wir einen Patch Source Code bieten, um ein Problem für Sie zu beheben, bevor wir eine QFE verteilen können.

## <a name="to-compile-the-xdk-c-xsapi-project-for-yourself"></a>Zum Kompilieren des C++-XSAPI xdk-Version-Projekts für sich selbst

<ol>
  <li> Abrufen der Microsoft.Xbox.Services-Quelle an. Extrahieren Sie zu diesem Zweck alle Dateien aus "%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox Services API\8.0\SourceDist\Xbox.Services.zip" in einen beschreibbaren Ordner außerhalb von "C:\Program Files (x86)", oder Sie können die Quelle, aus Klonen <a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> Wenn Ihr Projekt, die vor dem Erstellen-DLL verweist, müssen Sie den Verweis entfernen</li>
    <ul>
      <li> Für Visual Studio 2012: Wählen Sie "Projekt -> Verweise..." in Visual Studio. Wenn Sie Xbox-Services-API als Verweis aufgeführt ist, wählen sie aus, und klicken Sie auf "Entfernen von Verweisen". Klicken Sie auf "OK", und speichern Sie die Projektdatei.</li>
      <li> Für Visual Studio 2015 oder 2017: Wählen Sie "-> Projekt Verweise hinzufügen" in Visual Studio. Wenn Sie Xbox-Services-API aktiviert ist, deaktivieren Sie es aus. Klicken Sie auf "OK", und speichern Sie die Projektdatei.</li>
    </ul>
  <li> Wenn Sie mit der xdk-Version erstellen, wählen Sie "Datei -> Hinzufügen -> vorhandenes Projekt..." in Visual Studio-Lösung für Ihre Anwendung die folgenden zwei Projekte hinzu. Die Vcxproj-Dateien werden im Ordner befinden, die, denen die Quelle, den Sie extrahiert haben.</li>
Für Visual Studio 2017: <ul>
      <li>\Build\Microsoft.Xbox.Services.141.XDK.Cpp\Microsoft.Xbox.Services.141.XDK.Cpp.vcxproj</li>   <li>\External\cpprestsdk\Release\src\build\vs15.xbox\casablanca141.Xbox.vcxproj</li>
    </ul>
Für Visual Studio 2015: <ul>
      <li>\Build\Microsoft.Xbox.Services.140.XDK.Cpp\Microsoft.Xbox.Services.140.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs14.xbox\casablanca140.Xbox.vcxproj</li>
    </ul>
Für Visual Studio 2012: <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.Cpp\Microsoft.Xbox.Services.110.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110.Xbox.vcxproj</li>
    </ul>
    <li> Fügen Sie, dass die Source-Projekte als Verweis durch Auswählen des Projekt-Verweise >... und wählen Sie "Verweis hinzufügen". Klicken Sie unter "Projektmappen Projekte ->" Überprüfen Sie die Einträge für beide Projekte, die oben genannten klicken Sie dann auf OK klicken.</li>
    <li> Die Props-Datei zu Ihrem Projekt hinzufügen, indem Sie auf "Ansicht -> andere Windows-Eigenschaften-Manager >", Rechtsklick auf das Projekt, "Vorhandenes Eigenschaftenblatt hinzufügen" auswählen und dann schließlich auswählen xsapi.staticlib.props in Sourch SDK-Stamm.  Wenn das Buildsystem Props-Dateien nicht unterstützt, müssen Sie manuell die Präprozessordefinitionen und -Bibliotheken wie in %XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\ hinzufügen Xbox.Services.API.Cpp.props</li>
    <li> -Fügen Sie die Datei services.h zu Ihrer app hinzufügen, indem Sie mit der rechten Maustaste auf das Projekt hinzufügen > Vorhandenes Element, und die {SDK root}\Include\xsapi\services.h Quelldatei auswählen</li>
    <li> Stellen Sie sicher, dass es sich bei "Ordner" sowohl das Anwendungsprojekt und das Xbox-Services-Projekt identisch sind. Mit dieser Einstellung finden Sie in Visual Studio-Projekt die-Eigenschaften > Konfigurationseigenschaften -> Allgemein -> Ausgabeverzeichnis.</li>
    <li> Ihre Visual Studio-Projektmappe neu erstellen</li>
</ol>

## <a name="to-compile-the-xdk-winrt-xsapi-project-for-yourself"></a>Zum Kompilieren des xdk-Version WinRT XSAPI-Projekts für sich selbst

<ol>
  <li> Abrufen der Microsoft.Xbox.Services-Quelle an. Zu diesem Zweck, extrahieren Sie alle Dateien aus "%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox Services API\8.0\SourceDist\Xbox.Services.zip" in einen beschreibbaren Ordner außerhalb von "C:\Program Files (x86)", oder Sie können die Quelle, aus Klonen <a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> Wenn Ihr Projekt, die vor dem Erstellen-DLL verweist, müssen Sie den Verweis entfernen</li>
    <ul>
      <li> Für Visual Studio 2012: Wählen Sie "Projekt -> Verweise..." in Visual Studio. Wenn Sie Xbox-Services-API als Verweis aufgeführt ist, wählen sie aus, und klicken Sie auf "Entfernen von Verweisen". Klicken Sie auf "OK", und speichern Sie die Projektdatei.</li>
      <li> Für Visual Studio 2015 oder 2017: Wählen Sie "-> Projekt Verweise hinzufügen" in Visual Studio. Wenn Sie Xbox-Services-API aktiviert ist, deaktivieren Sie es aus. Klicken Sie auf "OK", und speichern Sie die Projektdatei.</li>
    </ul>
  <li> Wenn Sie mit der xdk-Version erstellen, wählen Sie "Datei -> Hinzufügen -> vorhandenes Projekt..." in Visual Studio-Lösung für Ihre Anwendung die folgenden zwei Projekte hinzu. Die Vcxproj-Dateien werden im Ordner befinden, die, denen die Quelle, den Sie extrahiert haben.  Für Visual Studio 2015 aktualisiert die Projekte automatisch auf das Visual Studio 2015-Format.</li>
    <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.WinRT\Microsoft.Xbox.Services.110.XDK.WinRT.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110.Xbox.vcxproj</li>
    </ul>
  <li> Fügen Sie in Visual Studio die Verweise hinzu:</li>
    <ul>
      <li> Für Visual Studio 2012: Wählen Sie "Projekt -> Verweise..." und wählen Sie "Verweis hinzufügen" in Visual Studio. In Projektmappe Projekte Chceck-Einträge für beide Projekte, die oben genannten >, und klicken Sie auf OK.</li>
      <li> Für Visual Studio 2015 oder 2017: Wählen Sie "-> Projekt Verweise hinzufügen" in Visual Studio. Klicken Sie unter Projekte überprüfen Sie die Einträge für beide Projekte, die oben genannten, und klicken Sie auf OK.</li>
    </ul>
  <li> Stellen Sie sicher, dass es sich bei "Ordner" sowohl das Anwendungsprojekt und das Xbox-Services-Projekt identisch sind. Mit dieser Einstellung finden Sie in Visual Studio-Projekt die-Eigenschaften > Konfigurationseigenschaften -> Allgemein -> Ausgabeverzeichnis.</li>
  <li> Ihre Visual Studio-Projektmappe neu erstellen</li>
</ol>
