---
Description: Befolgen Sie diese Richtlinien, um die Pakete der APP für die Übermittlung an den Microsoft Store vorzubereiten.
title: App-Paketanforderungen
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, Paket Anforderungen, Pakete, Paketformat, unterstützte Version, übermitteln
ms.localizationpriority: medium
ms.openlocfilehash: 848adbab20765a65ef4673219c55dfbf076e47e0
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219773"
---
# <a name="app-package-requirements"></a>App-Paketanforderungen

Befolgen Sie diese Richtlinien, um die Pakete der APP für die Übermittlung an den Microsoft Store vorzubereiten.

## <a name="before-you-build-your-apps-package-for-the-microsoft-store"></a>Vor dem Erstellen des App-Pakets für die Microsoft Store

Denken Sie daran, [Ihre App mit dem Zertifizierungskit für Windows-Apps zu testen](../debug-test-perf/windows-app-certification-kit.md). Außerdem wird empfohlen, Ihre App mit verschiedenen Hardwaretypen zu testen. Beachten Sie, dass die app nur auf Computern mit Entwickler Lizenzen installiert und ausgeführt werden kann, bis Sie Ihre APP zertifizieren und aus dem Microsoft Store verfügbar machen.

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>Erstellen des App-Pakets mit Microsoft Visual Studio

Wenn Sie Microsoft Visual Studio als Entwicklungsumgebung verwenden, verfügen Sie bereits über integrierte Tools zum schnellen und einfachen Erstellen eines App-Pakets. Weitere Informationen finden Sie unter [Verpacken von Apps](../packaging/index.md).

> [!NOTE]
> Stellen Sie sicher, dass alle Dateinamen ANSI verwenden. 

Um Ihr Paket in Visual Studio zu erstellen, müssen Sie sich mit demselben Konto anmelden, das Ihrem Entwicklerkonto zugeordnet ist. Einige Teile des Paketmanifests enthalten spezifische kontobezogene Details. Diese Informationen werden erkannt und automatisch hinzugefügt. Ohne die zusätzlichen Informationen, die dem Manifest hinzugefügt werden, können Fehler beim Hochladen von Paketen auftreten. 

Wenn Sie die UWP-Pakete Ihrer APP erstellen, kann Visual Studio eine msix-oder AppX-Datei oder eine. msixupload-oder. appxupload-Datei erstellen. Für UWP-apps wird empfohlen, dass Sie immer die Datei ". msixupload" oder ". appxupload" auf der Seite " [Pakete](upload-app-packages.md) " hochladen. Weitere Informationen zum Packen von UWP-Apps für den Store finden Sie unter [Packen einer UWP-App mit Visual Studio](/windows/msix/package/packaging-uwp-apps).

App-Pakete müssen nicht mit einem Stammzertifikat einer vertrauenswürdigen Zertifizierungsstelle signiert werden.


### <a name="app-bundles"></a>App-Bündel

Für UWP-apps kann Visual Studio eine APP Bundle (. msixbundle oder. appxbundle) generieren, um die Größe der APP zu reduzieren, die von den Benutzern heruntergeladen wird. Dieser Schritt ist in der Regel sinnvoll, wenn Sie sprachspezifische Ressourcen, mehrere Ressourcen für die Bildgröße oder Ressourcen für bestimmte Versionen von Microsoft DirectX definiert haben.

> [!NOTE]
> Ein App-Bundle kann Pakete für alle Architekturen umfassen.

Bei einem App-Bündel lädt der Benutzer nicht alle vorhandenen Ressourcen, sondern nur relevante Dateien herunter. Weitere Informationen zu app-Paketen finden Sie unter [Packen von apps](../packaging/index.md) und [Verpacken einer UWP-App mit Visual Studio](/windows/msix/package/packaging-uwp-apps).


## <a name="building-the-app-package-manually"></a>Manuelles Erstellen des App-Pakets

Wenn Sie Ihr Paket nicht mit Visual Studio erstellen, müssen Sie Ihr [Paketmanifest manuell erstellen](/uwp/schemas/appxpackage/how-to-create-a-package-manifest-manually).

Ausführliche Informationen und die Anforderungen für Manifeste finden Sie in der Dokumentation zum [App-Paketmanifest](/uwp/schemas/appxpackage/appx-package-manifest). Es werden nur Apps zertifiziert, deren Manifest dem Paketmanifestschema entspricht.

Ihr Manifest muss spezifische konto- und App-bezogene Informationen enthalten. Sie finden diese Informationen unter [Anzeigen von Details zur App-Identität](view-app-identity-details.md) im Abschnitt **App-Verwaltung** der App-Übersichtsseite im Dashboard.

> [!NOTE]
> Bei Werten im Manifest wird die Groß-/Kleinschreibung beachtet. Leerzeichen und Satzzeichen müssen ebenfalls übereinstimmen. Geben Sie die Werte richtig ein, und überprüfen Sie sie anschließend auf ihre Korrektheit.


App-Bündel (. msixbundle oder. appxbundle) verwenden ein anderes Manifest. Ausführliche Informationen und die Anforderungen für App-Bündel finden Sie in der Dokumentation zum [Bündelmanifest](/uwp/schemas/bundlemanifestschema/bundle-manifest). Beachten Sie, dass das Manifest jedes enthaltenen Pakets in einer msixbundle-oder appxbundle-Datei dieselben Elemente und Attribute verwenden muss, mit Ausnahme des Attributs **ProcessorArchitecture** des [Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) -Elements.

> [!TIP]
> Stellen Sie sicher, dass Sie das [zertifizierungskit für Windows-apps](../debug-test-perf/windows-app-certification-kit.md) ausführen, bevor Sie Ihre Pakete So können Sie feststellen, ob es mit Ihrem Manifest Probleme gibt, die Zertifizierungs- oder Einreichungsfehler verursachen können.


## <a name="package-format-requirements"></a>Paketformatanforderungen

Ihre App-Pakete müssen die folgenden Anforderungen erfüllen:

| App-Paketeigenschaft | Anforderung                                                          |
|----------------------|----------------------------------------------------------------------|
| Paketgröße         | . msixbundle oder. appxbundle: maximal 25 GB pro Bundle <br>msix-oder AppX-Pakete für Windows 10:25 GB maximal pro Paket<br>APPX-Pakete für Windows 8.1: maximal 8 GB pro Paket <br> APPX-Pakete für Windows 8: maximal 2 GB pro Paket <br> APPX-Pakete für Windows Phone 8.1: maximal 4 GB pro Paket <br> XAP-Pakete: maximal 1 GB pro Paket                                                                           |
| Hashes für Blockzuordnung     | SHA2-256-Algorithmus                                                   |

> [!IMPORTANT]
> Sie können keine neuen XAP-Pakete mehr hochladen, die mit den Windows Phone 8. x SDK (s) erstellt wurden. Apps, die bereits mit XAP-Paketen im Speicher gespeichert sind, funktionieren weiterhin auf Windows 10 Mobile-Geräten. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="supported-versions"></a>Unterstützte Versionen

Für UWP-apps müssen alle Pakete eine Version von Windows 10 als Ziel haben, die vom Speicher unterstützt wird. Die Versionen, die Ihr Paket unterstützt, müssen in den Attributen **MinVersion** und **maxversiongetedes** [targetdevicefamily](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) -Elements des App-Manifests angegeben werden.

Die derzeit unterstützten Versionen reichen von folgenden Versionen: 
- Minimal: 10.0.10240.0
- Maximum: 10.0.17763.1


## <a name="storemanifest-xml-file"></a>Datei „StoreManifest.xml“

„StoreManifest.xml“ ist eine optionale Konfigurationsdatei, die in App-Pakete aufgenommen werden kann. Der Zweck besteht darin, Features zu aktivieren, wie z. b. das Deklarieren der App als Microsoft Store Geräte-APP oder das Deklarieren von Anforderungen, von denen ein Paket abhängig ist, damit es auf ein Gerät anwendbar ist. Wenn Sie verwendet wird, wird StoreManifest.xml mit dem App-Paket übermittelt und muss sich im Stamm Ordner des Hauptprojekts Ihrer APP befinden. Weitere Informationen finden Sie unter [StoreManifest-Schema](/uwp/schemas/storemanifest/store-manifest-schema-portal).

 

 
