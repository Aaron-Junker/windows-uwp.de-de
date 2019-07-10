---
title: Verwenden einer MySQL-Datenbank in einer UWP-App
description: Verwenden einer MySQL-Datenbank in einer UWP-App
ms.date: 3/28/2019
ms.topic: article
keywords: Windows 10, UWP, MySQL, Datenbank
ms.localizationpriority: medium
ms.openlocfilehash: a7708ca082647aef6bbf2261922d2ebd6723923e
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "63785491"
---
# <a name="use-a-mysql-database"></a>Verwenden einer MySQL-Datenbank
Dieser Artikel enthält die erforderlichen Schritte zur Verwendung einer MySQL-Datenbank in einer UWP-App. Er enthält auch einen kleinen Codeausschnitt zur Veranschaulichung, wie Sie im Code mit der Datenbank interagieren können.

## <a name="set-up-your-solution"></a>Einrichten der Lösung

Um Ihre App direkt mit einer MySQL-Datenbank zu verbinden, stellen Sie sicher, dass für die Mindestversion Ihres Projekts das Fall Creators Update (Build 16299) festgelegt ist.  Sie finden diese Informationen auf der Eigenschaftenseite des UWP-Projekts.

![Abbildung des Eigenschaftenbereichs in Visual Studio mit dem Fall Creators Update als festgelegter Ziel- und Mindestversion](images/min-version-fall-creators.png)

Öffnen Sie die **Paket-Manager-Konsole** („Ansicht“ -> „Weitere Fenster“ -> „Paket-Manager-Konsole“). Verwenden Sie den Befehl **Install-Package MySql.Data**, um den Treiber für MySQL zu installieren. Dadurch können Sie programmgesteuert auf MySQL-Datenbanken zugreifen.

## <a name="test-your-connection-using-sample-code"></a>Testen der Verbindung mit Beispielcode
Nachfolgend finden Sie ein Beispiel für die Herstellung einer Verbindung mit einer MySQL-Remotedatenbank und dem Lesen von Daten aus dieser Datenbank. Beachten Sie, dass die IP-Adresse, die Anmeldeinformationen und der Datenbankname angepasst werden müssen.

```csharp
string M_str_sqlcon = "server=10.xxx.xx.xxx;user id=foo;password=bar;database=baz";
MySqlConnection mysqlcon = new MySqlConnection(M_str_sqlcon);
MySqlCommand mysqlcom = new MySqlCommand("select * from table1", mysqlcon);
mysqlcon.Open();
MySqlDataReader mysqlread = mysqlcom.ExecuteReader(CommandBehavior.CloseConnection);
while (mysqlread.Read())
{
    Debug.WriteLine(mysqlread.GetString(0)+":"+mysqlread.GetString(1));
}
mysqlcon.Close();
```