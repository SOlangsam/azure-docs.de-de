---
title: Einschränkungen – Azure Database for MySQL
description: Dieser Artikel beschreibt die Einschränkungen in Azure Database for MySQL, z.B. die Anzahl der Verbindungs- und Speichermoduloptionen.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/9/2019
ms.openlocfilehash: 757a061bff72ca9fc34d408cd94cec9966d1157f
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77191114"
---
# <a name="limitations-in-azure-database-for-mysql"></a>Beschränkungen in Azure Database for MySQL
In den folgenden Abschnitten werden die Kapazitäts- und funktionalen Beschränkungen sowie Beschränkungen bei der Unterstützung der Speicher-Engine und von Datenmanipulationsanweisungen im Datenbankdienst beschrieben. Sehen Sie sich auch die [allgemeinen Einschränkungen](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) an, die für die MySQL-Datenbank-Engine gelten.

## <a name="maximum-connections"></a>Maximale Anzahl der Verbindungen
Die folgende Tabelle enthält die maximale Anzahl von Verbindungen nach Tarif und V-Kernen: 

|**Tarif**|**vCore(s)**| **Max. Anzahl von Verbindungen**|
|---|---|---|
|Basic| 1| 50|
|Basic| 2| 100|
|Universell| 2| 600|
|Universell| 4| 1250|
|Universell| 8| 2500|
|Universell| 16| 5\.000|
|Universell| 32| 10000|
|Universell| 64| 20000|
|Arbeitsspeicheroptimiert| 2| 1250|
|Arbeitsspeicheroptimiert| 4| 2500|
|Arbeitsspeicheroptimiert| 8| 5\.000|
|Arbeitsspeicheroptimiert| 16| 10000|
|Arbeitsspeicheroptimiert| 32| 20000|

Wenn Verbindungen den Grenzwert übersteigen, erhalten Sie möglicherweise den folgenden Fehler:
> FEHLER 1040 (08004): Zu viele Verbindungen

> [!IMPORTANT]
> Für eine optimale Erfahrung empfehlen wir, dass Sie einen Verbindungsspooler wie ProxySQL verwenden, um Verbindungen effizient zu verwalten.

Das Erstellen neuer Clientverbindungen mit MySQL nimmt Zeit in Anspruch, und nach der Herstellung belegen diese Verbindungen Datenbankressourcen, auch wenn Sie sich im Leerlauf befinden. Die meisten Anwendungen fordern viele kurzlebige Verbindungen an, was diese Situation erschwert. Das Ergebnis sind weniger Ressourcen, die für ihre tatsächliche Workload verfügbar sind, was zu verringerter Leistung führt. Ein Verbindungspooler, der Verbindungen im Leerlauf reduziert und vorhandene Verbindungen wiederverwendet, hilft dabei, dies zu vermeiden. Weitere Informationen zum Einrichten von ProxySQL finden Sie in unserem [Blogbeitrag](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042).

## <a name="storage-engine-support"></a>Speicher-Engine-Unterstützung

### <a name="supported"></a>Unterstützt
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [MEMORY](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Nicht unterstützt
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [BLACKHOLE](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARCHIVE](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERATED](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Berechtigungsunterstützung

### <a name="unsupported"></a>Nicht unterstützt
- DBA-Rolle: Viele Serverparameter und -einstellungen können unbeabsichtigterweise die Serverleistung beeinträchtigen oder ACID-Eigenschaften des DBMS verschlechtern. Um die Dienstintegrität und die SLA auf Produktebene aufrechtzuerhalten, stellt dieser Dienst die DBA-Rolle nicht zur Verfügung. Das Standardbenutzerkonto, das zusammen mit einer neuen Datenbankinstanz erstellt wird, gestattet dem Benutzer die Ausführung der meisten DDL- und DML-Anweisungen in der verwalteten Datenbankinstanz. 
- SUPER-Berechtigung: Auf ähnliche Weise ist die [SUPER-Berechtigung](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) ebenfalls eingeschränkt.
- DEFINER: Erfordert erhöhte Berechtigungen zum Erstellen und ist beschränkt. Entfernen Sie beim Importieren von Daten mithilfe einer Sicherung die `CREATE DEFINER`-Befehle manuell oder mithilfe des Befehls `--skip-definer`, wenn Sie einen mysqldump ausführen.

## <a name="data-manipulation-statement-support"></a>Unterstützung von Datenmanipulationsanweisungen

### <a name="supported"></a>Unterstützt
- `LOAD DATA INFILE` wird unterstützt, jedoch muss der Parameter `[LOCAL]` angegeben und an einen UNC-Pfad (über das SMB-Protokoll eingebundene Azure Storage-Instanz) weitergeleitet werden.

### <a name="unsupported"></a>Nicht unterstützt
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>Funktionale Beschränkungen

### <a name="scale-operations"></a>Skalierungsvorgänge
- Die dynamische Skalierung auf den oder vom Basic-Tarif aus wird zurzeit nicht unterstützt.
- Die Verringerung der Größe des Serverspeichers wird nicht unterstützt.

### <a name="server-version-upgrades"></a>Upgrades von Serverversionen
- Die automatisierte Migration zwischen Hauptversionen von Datenbank-Engines wird derzeit nicht unterstützt. Wenn Sie auf die nächste Hauptversion upgraden möchten, führen Sie eine [Sicherung und Wiederherstellung](./concepts-migrate-dump-restore.md) auf einem Server aus, der mit der neuen Engine-Version erstellt wurde.

### <a name="point-in-time-restore"></a>Point-in-Time-Wiederherstellung
- Wenn Sie das PITR-Feature verwenden, wird der neue Server mit den gleichen Konfigurationen erstellt wie der Server, auf dem er basiert.
- Die Wiederherstellung eines gelöschten Servers wird nicht unterstützt.

### <a name="vnet-service-endpoints"></a>VNET-Dienstendpunkte
- VNET-Dienstendpunkte werden nur für Server vom Typ „Universell“ und „Arbeitsspeicheroptimiert“ unterstützt.

### <a name="storage-size"></a>Speichergröße
- Informationen zu den Beschränkungen der Speichergröße pro Tarif finden Sie unter [Tarife](concepts-pricing-tiers.md).

## <a name="current-known-issues"></a>Aktuelle bekannte Probleme
- Die MySQL-Serverinstanz zeigt die falsche Serverversion an, nachdem die Verbindung hergestellt wurde. Um die richtige Engine-Version der Serverinstanzen abzurufen, verwenden Sie den Befehl `select version();`.

## <a name="next-steps"></a>Nächste Schritte
- [Verfügbaren Funktionen auf den einzelnen Dienstebenen](concepts-pricing-tiers.md)
- [Unterstützte MySQL-Datenbankversionen](concepts-supported-versions.md)
