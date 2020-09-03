---
title: Bekannte Probleme mit UWP im Zusammenhang mit dem Xbox-Entwicklerprogramm
description: Erfahren Sie mehr über bekannte Probleme mit der UWP in Xbox One Developer Program, und erfahren Sie, wie Sie auf andere Hilfe Ressourcen zugreifen.
ms.date: 03/29/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: a7b82570-1f99-4bc3-ac78-412f6360e936
ms.localizationpriority: medium
ms.openlocfilehash: 7867b26849019c517c0e3d2e3ad9aa0cf86158fc
ms.sourcegitcommit: efa5f793607481dcae24cd1b886886a549e8d6e5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89411994"
---
# <a name="known-issues-with-uwp-on-xbox-developer-program"></a>Bekannte Probleme mit UWP im Zusammenhang mit dem Xbox-Entwicklerprogramm

Dieses Thema beschreibt bekannte Probleme im Zusammenhang mit dem Xbox One-Entwicklerprogramm. Weitere Informationen zu diesem Programm finden Sie unter [UWP auf Xbox](index.md). 

\[Wenn Sie von einem Link in einem API-Referenz Thema stammen und nach universellen Gerätefamilien-API-Informationen suchen, finden Sie weitere Informationen [unter UWP-Funktionen, die auf Xbox nicht unterstützt](/uwp/extension-sdks/uwp-limitations-on-xbox)werden.\]

In der folgenden Liste werden einige bekannte Probleme aufgelistet, die auftreten können. Diese Liste ist jedoch nicht vollständig.

**Wir möchten gerne Ihr Feedback erhalten.** Melden Sie daher alle festgestellten Probleme im Forum für das [Entwickeln von Apps für die Universelle Windows-Plattform (UWP)](https://social.msdn.microsoft.com/forums/windowsapps/home?forum=wpdevelop). 

Wenn Sie Probleme haben, lesen Sie die Informationen in diesem Thema, informieren Sie sich in den [häufig gestellten Fragen](frequently-asked-questions.md), und nutzen Sie die Foren, um Hilfe zu erhalten.

 
## <a name="deploying-from-vs-fails-with-parental-controls-turned-on"></a>Fehler bei der Bereitstellung aus VS bei aktiviertem Jugendschutz

Beim Starten Ihrer App in VS tritt ein Fehler auf, wenn in den Einstellungen der Konsole der Jugendschutz aktiviert ist.

Um dieses Problem zu umgehen, deaktivieren Sie vorübergehend den Jugendschutz. Alternative:
1. Stellen Sie Ihre App in der Konsole mit deaktiviertem Jugendschutz bereit.
2. Aktivieren Sie den Jugendschutz.
3. Starten Sie die App von der Konsole.
4. Geben Sie eine PIN oder ein Kennwort ein, um das Starten der App zuzulassen.
5. Die App wird gestartet.
6. Schließen Sie die App.
7. Starten Sie die App in VS mithilfe von F5. Sie wird ohne Benutzeraufforderung gestartet.

Zu diesem Zeitpunkt bleibt die Berechtigung so lange _bestehen_, bis Sie den Benutzer abmelden, auch wenn Sie die App deinstallieren und neu installieren.
 
Es gibt eine weitere Ausnahme, die nur für Kinderkonten verfügbar ist. Bei einem Kinderkonto muss sich ein Elternteil anmelden, um die Berechtigung zu erteilen. In diesem Fall kann der Elternteil die Option **Immer** auswählen, um dem Kind das Starten der App zu erlauben. Diese Ausnahme wird in der Cloud gespeichert und bleibt bestehen, auch wenn sich das Kind abmeldet und wieder anmeldet.

## <a name="storagefilecopyasync-fails-to-copy-encrypted-files-to-unencrypted-destination"></a>Storagefile. copyasync kann verschlüsselte Dateien nicht in ein unverschlüsseltes Ziel kopieren. 

Wenn storagefile. copyasync zum Kopieren einer Datei verwendet wird, die in ein nicht verschlüsseltes Ziel verschlüsselt ist, schlägt der-Befehl mit folgender Ausnahme fehl:

```
System.UnauthorizedAccessException: Access is denied. (Excep_FromHResult 0x80070005)
```

Dies kann sich auf Xbox-Entwickler auswirken, die Dateien, die als Teil des App-Pakets bereitgestellt werden, an einen anderen Speicherort kopieren möchten. Der Grund hierfür ist, dass der Paket Inhalt auf einer Xbox im Einzelhandels Modus verschlüsselt ist, aber nicht im Entwicklungsmodus. Infolgedessen funktioniert die APP während der Entwicklung und Tests möglicherweise erwartungsgemäß, schlägt jedoch fehl, sobald Sie veröffentlicht und auf einer Einzelhandel-Xbox installiert wurde.
 

## <a name="blocked-networking-ports-on-xbox-one"></a>Gesperrte Netzwerkports auf Xbox One

Bindungen an Ports im Bereich [57344, 65535] (einschließlich) sind für Apps für die universelle Windows-Plattform (UWP) auf Xbox One-Geräten nicht möglich. Obwohl die Bindung an diese Ports während der Laufzeit erfolgreich zu sein scheint, kann Netzwerkdatenverkehr im Hintergrund gelöscht werden, bevor er Ihre App erreicht. Ihre App sollte an den Port 0 gebunden werden, wann immer möglich. Dieser ermöglicht dem System die Auswahl des lokalen Ports. Wenn Sie einen bestimmten Port verwenden müssen, muss sich die Portnummer im Bereich [1025, 49151] befinden, und Sie sollten Konflikte mit der IANA-Registrierung überprüfen und vermeiden. Weitere Informationen finden Sie in [Dienstname und Transportprotokoll-Portnummer-Registrierung](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml).

## <a name="windows-runtime-api-coverage"></a>Windows-Runtime API-Abdeckung

Nicht alle Windows-Runtime-APIs werden auf der Xbox unterstützt. Die Liste der APIs, die nicht funktionieren, finden Sie unter [UWP-Funktionen, die auf Xbox nicht unterstützt](/uwp/extension-sdks/uwp-limitations-on-xbox)werden. Wenn Sie Probleme mit anderen APIs feststellen, melden Sie dies bitte in den Foren.

## <a name="navigating-to-wdp-causes-a-certificate-warning"></a>Navigieren zu WDP führt zu einer Zertifikatwarnung

Sie erhalten eine Warnung zum Zertifikat, das bereitgestellt wurde, ähnlich wie folgender Screenshot, dass das von Ihrer Xbox One-Konsole signierte Sicherheitszertifikat nicht als bekannter vertrauenswürdiger Herausgeber betrachtet wird. Klicken Sie auf **Laden dieser Website fortsetzen**, um auf das Windows Device Portal zuzugreifen.

![Warnung zum Sicherheitszertifikat der Website](images/security_cert_warning.jpg)


## <a name="knownfoldersmediaserverdevices-caveat-on-xbox"></a>KnownFolders. mediaserverdevices (Vorbehalte) auf Xbox

Auf dem Desktop werden Medienserver mit dem PC "gekoppelt", und der Geräte Zuordnungs Dienst verfolgt ständig, welche der Server zurzeit online sind, sodass eine anfängliche Dateisystem Abfrage sofort eine Liste der gekoppelten Server zurückgeben kann, die zurzeit online sind.

Auf der Xbox gibt es keine Benutzeroberfläche zum Hinzufügen oder Entfernen von Servern. Daher gibt die anfängliche Dateisystem Abfrage immer leer zurück. Sie müssen eine Abfrage erstellen und das Ereignis "contentschge" abonnieren und die Abfrage jedes Mal aktualisieren, wenn Sie eine Benachrichtigung erhalten. Die Server werden in und die meisten innerhalb von 3 Sekunden ermittelt.

Einfacher Beispielcode:

```
namespace TestDNLA {

    public sealed partial class MainPage : Page {
        public MainPage() {
            this.InitializeComponent();
        }

        private async void FindFiles_Click(object sender, RoutedEventArgs e) {
            try {
                StorageFolder library = KnownFolders.MediaServerDevices;
                var folderQuery = library.CreateFolderQuery();
                folderQuery.ContentsChanged += FolderQuery_ContentsChanged;
                IReadOnlyList<StorageFolder> rootFolders = await folderQuery.GetFoldersAsync();
                if (rootFolders.Count == 0) {
                    Debug.WriteLine("No Folders found");
                } else {
                    Debug.WriteLine("Folders found");
                }
            } catch (Exception ex) {
                Debug.WriteLine("Error: " + ex.Message);
            } finally {
                Debug.WriteLine("Done");
            }
        }

        private async void FolderQuery_ContentsChanged(Windows.Storage.Search.IStorageQueryResultBase sender, object args) {
            Debug.WriteLine("Folder added " + sender.Folder.Name);
            IReadOnlyList<StorageFolder> topLevelFolders = await sender.Folder.GetFoldersAsync();
            foreach (StorageFolder topLevelFolder in topLevelFolders) {
                Debug.WriteLine(topLevelFolder.Name);
            }
        }
    }
}
```

## <a name="see-also"></a>Siehe auch
- [Häufig gestellte Fragen](frequently-asked-questions.md)
- [UWP auf Xbox One](index.md)