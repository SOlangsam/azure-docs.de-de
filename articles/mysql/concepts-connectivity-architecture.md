---
title: Verbindungsarchitektur – Azure Database for MySQL
description: Hier wird die Verbindungsarchitektur für Ihren Azure Database for MySQL-Server beschrieben.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: e520c9a1e9d40c318b8e2b69801bf5cc600e16bb
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76772043"
---
# <a name="connectivity-architecture-in-azure-database-for-mysql"></a>Verbindungsarchitektur in Azure Database for MySQL
In diesem Artikel wird die Verbindungsarchitektur von Azure Database for MySQL beschrieben, und Sie erfahren, wie Datenverkehr von Clients innerhalb und außerhalb von Azure an Ihre Azure Database for MySQL-Instanz weitergeleitet wird.

## <a name="connectivity-architecture"></a>Verbindungsarchitektur
Die Verbindung mit Ihrer Azure Database for MySQL-Instanz wird über ein Gateway hergestellt, das eingehende Verbindungen an den physischen Ort weiterleitet, an dem sich Ihr Server in unseren Clustern befindet. Das folgende Diagramm veranschaulicht den Datenverkehrsfluss.

![Übersicht über die Verbindungsarchitektur](./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png)

Ein Client, der eine Verbindung mit der Datenbank herstellt, erhält eine mit dem Gateway verknüpfte Verbindungszeichenfolge. Dieses Gateway besitzt eine öffentliche IP-Adresse, die an Port 3306 lauscht. Innerhalb des Datenbankclusters wird der Datenverkehr an die entsprechende Azure Database for MySQL-Instanz weitergeleitet. Für die Verbindungsherstellung mit Ihrem Server (etwa von Unternehmensnetzwerken aus) muss daher die clientseitige Firewall geöffnet werden, damit ausgehender Datenverkehr unsere Gateways erreichen kann. Weiter unten finden Sie eine vollständige, nach Region aufgeschlüsselte Liste der IP-Adressen, die von unseren Gateways verwendet werden.

## <a name="azure-database-for-mysql-gateway-ip-addresses"></a>IP-Adressen des Azure Database for MySQL-Gateways
Die folgende Tabelle enthält die primären und sekundären IP-Adressen des Azure Database for MySQL-Gateways für alle Datenregionen. Die primäre IP-Adresse ist die aktuelle IP-Adresse des Gateways. Bei der zweiten IP-Adresse handelt es sich um eine Failover-IP-Adresse für den Fall, dass die primäre Adresse ausfällt. Kunden sollten wie bereits erwähnt ausgehenden Datenverkehr für beide IP-Adressen zulassen. Die zweite IP-Adresse lauscht an keinen Diensten, bis sie von Azure Database for MySQL aktiviert wird, um Verbindungen zu akzeptieren.

| **Regionsname** | **Gateway-IP-Adressen** |
|:----------------|:-------------|
| Australien, Mitte| 20.36.105.0     |
| Australien, Mitte 2     | 20.36.113.0   |
| Australien (Osten) | 13.75.149.87, 40.79.161.1     |
| Australien, Südosten |191.239.192.109, 13.73.109.251   |
| Brasilien Süd | 104.41.11.5, 191.233.201.8, 191.233.200.16  |
| Kanada, Mitte |40.85.224.249  |
| Kanada, Osten | 40.86.226.166    |
| USA (Mitte) | 23.99.160.139, 13.67.215.62   |
| China, Osten | 139.219.130.35    |
| China, Osten 2 | 40.73.82.1  |
| China, Norden | 139.219.15.17    |
| China, Norden 2 | 40.73.50.0     |
| Asien, Osten | 191.234.2.139, 52.175.33.150, 13.75.33.20, 13.75.33.21     |
| East US | 40.121.158.30, 191.238.6.43  |
| USA (Ost) 2 |40.79.84.180, 191.239.224.107, 52.177.185.181   |
| Frankreich, Mitte | 40.79.137.0, 40.79.129.1  |
| Deutschland, Mitte | 51.4.144.100     |
| Deutschland, Nordosten | 51.5.144.179  |
| Indien, Mitte | 104.211.96.159     |
| Indien, Süden | 104.211.224.146  |
| Indien, Westen | 104.211.160.80    |
| Japan, Osten | 13.78.61.196, 191.237.240.43  |
| Japan, Westen | 104.214.148.156, 191.238.68.11, 40.74.96.7, 40.74.96.6 |
| Korea, Mitte | 52.231.32.42   |
| Korea, Süden | 52.231.200.86    |
| USA Nord Mitte | 23.96.178.199, 23.98.55.75, 52.162.104.35, 52.162.104.36    |
| Nordeuropa | 40.113.93.91, 191.235.193.75    |
| Südafrika, Norden  | 102.133.152.0    |
| Südafrika, Westen | 102.133.24.0   |
| USA Süd Mitte |13.66.62.124, 23.98.162.75, 104.214.16.39, 20.45.120.0   |
| Südostasien | 104.43.15.0, 23.100.117.95, 40.78.233.2, 23.98.80.12     |
| VAE, Mitte | 20.37.72.64  |
| Vereinigte Arabische Emirate, Norden | 65.52.248.0    |
| UK, Süden | 51.140.184.11   |
| UK, Westen | 51.141.8.11  |
| USA, Westen-Mitte | 13.78.145.25     |
| Europa, Westen | 40.68.37.158, 191.237.232.75     |
| USA (Westen) | 104.42.238.205, 23.99.34.75  |
| USA, Westen 2 | 13.66.226.202  |
||||

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Verwalten von Firewallregeln für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./howto-manage-firewall-using-portal.md)
* [Erstellen und Verwalten von Firewallregeln für Azure Database for MySQL mithilfe der Azure CLI](./howto-manage-firewall-using-cli.md)
