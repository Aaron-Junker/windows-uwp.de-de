---
title: Überprüfen der Einstellungen für Benutzerberechtigungen in Unity
description: Erfahren Sie, wie Berechtigungen-Einstellungen für eine signierte überprüfen in Xbox Live-Konto.
ms.assetid: ''
ms.date: 10/26/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Konten, Testkonten, Jugendschutz, Benutzerberechtigungen, Erzwingung gesperrt, Zusatzverkäufe
ms.openlocfilehash: b55ebf9b53cadf2e57317347adce19c3578f9d56
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620405"
---
# <a name="check-user-privilege-settings-in-unity"></a>Überprüfen der Einstellungen für Benutzerberechtigungen in Unity
Auf Xbox Live sind alle authentifizierten Benutzerkonto Berechtigungen zugeordnet. Berechtigungen steuern, welche Funktionen von Xbox Live, ein Benutzer zu einem bestimmten Zeitpunkt rechtzeitig zugreifen kann. Einige dieser Berechtigungen sind für die System-gesteuerten Features, während andere bestimmte Spiele oder Erweiterung Abonnements zugeordnet sein können. Darüber hinaus können Jugendschutz und sperren, die ausgegeben wird, durch das Team für die Xbox Live-Erzwingung die Berechtigungen eines Benutzers einschränken. Diese Berechtigungen werden eine Anzahl von Szenarios, einschließlich Multiplayer-, den Zugriff auf vom Benutzer generierte Inhalte, die per Chat verbunden sind, oder bis hin zu Streamingvideos behandelt. Spiele verwenden diese Informationen, um Access Control und Personalisierung Entscheidungen zu treffen.

## <a name="prerequisites"></a>Voraussetzungen
Um Berechtigungen benutzereinstellungen zu bestimmen, müssen Sie Ihr Spiel für die Authentifizierung mit Xbox Live konfiguriert und einen Benutzer sich erfolgreich angemeldet haben.

>[!IMPORTANT]
> Wenn Sie Ihr Spiel im Unity-Editor testen, Ihr Spiel nicht mit der Xbox Live-Dienst verbunden ist und wird gefälschte Daten verwenden, um eine Verbindung zu simulieren. Dadurch wird ein null-Wert, wenn Sie für Berechtigungen überprüfen. Um mit echten Daten zu testen, führen Sie einen universelle Windows-Plattform-Build von Ihr Unity-Spiel, und öffnen Sie die generierte Projektdatei in Visual Studio.

In den folgenden Artikeln wird beschrieben, die Schritte, die Sie ergreifen können:

* [Melden Sie sich beim Xbox Live in Unity (Build und Test-Anmeldung)](unity-prefabs-and-sign-in.md#build-and-test-sign-in)
* [Testen Sie Ihr Unity-spielen-Build in Visual Studio](test-visual-studio-build.md)

## <a name="determine-privileges"></a>Ermitteln von Berechtigungen
Die Berechtigungen eines Benutzers werden in die bei der Authentifizierung erhaltene Token übertragen. In Unity können Sie zugreifen, die Liste der Berechtigungen, die ein Benutzer in hat der `XboxLiveUser` Klasse, nachdem der Benutzer erfolgreich angemeldet wurde. Berechtigungen werden als eine einzelne Zeichenfolge, die durch ein Leerzeichen getrennt gespeichert. Beispielsweise können Sie ein Benutzer mit den folgenden Berechtigungen finden Sie unter:

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

Debug.Log(XboxLiveUser.User.Privileges);

//Console would read:
// Privileges: "188 191 192 193 194 195 196 198 199 200 201 203 204 205 206 207 208 211 214 215 216 217 220 224 227 228 235 238 245 247 249 252 254 255"
```

Wenn Sie für eine bestimmte Berechtigung suchen möchten, sehen Sie um finden Sie unter den `Privileges` Eigenschaft enthält den zugehörigen Code:

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

if (XboxLiveUser.User.Privileges.Contains("247"))
{
    Debug.Log("User has the user_created_content privilege");
}
```

## <a name="privilege-codes"></a>Berechtigung-Codes
Folgendes ist eine Liste der möglichen Berechtigungen-Codes, die zurückgegeben werden können.

| Code  | Berechtigung  | Beschreibung   |
|------ |-----------------------------  |-------------------    |
| 190   | broadcast             | Live-Spiels können übertragen werden.     |
| 197   | view_friends_list     | Können Freunde-Liste eines anderen Benutzers anzeigen.   |
| 198   | game_dvr              | Upload kann erfasst in-Game-Videos in der Cloud.      |
| 199   | share_kinect_content          | Kinect aufgezeichneten Inhalt in die Cloud für den Benutzer hochgeladen und für alle Benutzer verfügbar gemacht werden kann. |
| 203   | multiplayer_parties           | Kann eine Partei beitreten.     |
| 205   | communication_voice_ingame    | Kann während der Parteien und Multiplayer-Spiele-Sitzungen an Voice Chat teilnehmen.    |
| 206   | communication_voice_skype     | Können die Sprachkommunikation mit Skype für Xbox One.   |
| 207   | cloud_gaming_manage_session   | Zuordnen und einen Cloud-Compute-Cluster für eine gehostete game-Sitzung verwalten können.    |
| 208   | cloud_gaming_join_session     | Können eine Cloud-Computing-Sitzung beitreten.     |
| 209   | cloud_saved_games     | Können Spiele im Titel Cloud-Speicher speichern.    |
| 211   | share_content     | Freigeben von Inhalten für andere Benutzer können.    |
| 214   | premium_content   | Kann zu erwerben, herunterladen und starten Premiuminhalte mit der Xbox Live Gold-Abonnement verfügbar sind.     |
| 219   | subscription_content  | Können erwerben und Herunterladen von Inhalten für Premium-Abonnement und verwenden die Features der Premium-Abonnement.     |
| 220   | social_network_sharing    | Statusinformationen in sozialen Netzwerken können freigeben werden.    |
| 224   | premium_video     | Können Premium-video-Dienste zugreifen.    |
| 235   | purchase_content  | Kann Inhalte zu erwerben.     |
| 247   | user_created_content  | Herunterladen und Anzeigen von online-Benutzer Inhalte erstellt.    |
| 249   | profile_viewing   | Können andere Benutzerprofilen anzeigen.   |
| 252   | Kommunikation    | Asynchrone Textnachrichten mit beliebigen Personen können verwenden werden.    |
| 254   | multiplayer_sessions  | Können Multiplayer-Sitzungen bei einem Spiel verknüpfen.   |
| 255   | add_friend    | Führen Sie andere Xbox Live-Benutzer und Xbox Live-Freunde hinzufügen können.   |
