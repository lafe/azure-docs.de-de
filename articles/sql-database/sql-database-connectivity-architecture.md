---
title: Weiterleiten des Azure-Datenverkehrs an Azure SQL-Datenbank und SQL Data Warehouse | Microsoft-Dokumentation
description: In diesem Artikel wird die Verbindungsarchitektur von Azure SQL-Datenbank und SQL Data Warehouse von innerhalb und von außerhalb von Azure erläutert.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab
manager: craigg
ms.date: 02/06/2019
ms.openlocfilehash: 5ce8464de552fb228b961af199e4b03e645478a2
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55809979"
---
# <a name="azure-sql-connectivity-architecture"></a>Verbindungsarchitektur von Azure SQL

In diesem Artikel wird die Verbindungsarchitektur von Azure SQL-Datenbank und SQL Data Warehouse erläutert. Zudem wird dargelegt, wie die verschiedenen Komponenten Datenverkehr an Ihre Instanz von Azure SQL leiten. Diese Verbindungskomponenten leiten Netzwerkdatenverkehr mithilfe von Clients, die von innerhalb und von außerhalb von Azure verbunden sind, an Azure SQL-Datenbank und SQL Data Warehouse. Dieser Artikel bietet auch Skriptbeispiele zum Ändern der Verbindungsart und Überlegungen, die beim Ändern der Standardverbindungseinstellungen berücksichtigt werden können.

> [!IMPORTANT]
> **[Bevorstehende Änderung] Bei Dienstendpunktverbindungen mit Azure SQL-Servern ändert sich das Verbindungsverhalten `Default` in `Redirect`.**
> Kunden wird empfohlen, neue Server zu erstellen und vorhandene Server einzurichten, wobei der Verbindungstyp je nach Konnektivitätsarchitektur explizit auf „Redirect“ (vorzugsweise) oder „Proxy“ festgelegt ist.
>
> Um zu verhindern, dass infolge dieser Änderung Verbindungen über einen Dienstendpunkt in bestehenden Umgebungen unterbrochen werden, setzen wir Telemetrie für folgende Zwecke ein:
> - Bei Servern, bei denen wir feststellen, dass der Zugriff darauf vor der Änderung über Dienstendpunkte erfolgte, wird der Verbindungstyp in `Proxy` geändert.
> - Bei allen anderen Servern wird der Verbindungstyp in `Redirect` geändert.
>
> In folgenden Szenarien können Dienstendpunktbenutzer dennoch betroffen sein:
> - Eine Anwendung stellt nur selten eine Verbindung mit einem vorhandenen Server her, sodass die Informationen zu diesen Anwendungen nicht mithilfe der Telemetrie erfasst wurden.
> - Die automatisierte Bereitstellungslogik erstellt einen SQL-Datenbankserver unter der Annahme, dass das Standardverhalten für Dienstendpunktverbindungen `Proxy` ist.
>
> Wenn Dienstendpunktverbindungen mit Azure SQL-Server nicht hergestellt werden konnten und Sie vermuten, dass Sie von dieser Änderung betroffen sind, stellen Sie sicher, dass der Verbindungstyp explizit auf `Redirect` festgelegt ist. In diesem Fall müssen Sie VM-Firewallregeln und Netzwerksicherheitsgruppen (NSG) für alle Azure-IP-Adressen in der Region öffnen, die zum SQL-[Diensttag](../virtual-network/security-overview.md#service-tags) für Ports 11000 bis 12000 gehören. Wenn dies für Sie keine eine Option ist, schalten Sie den Server explizit auf `Proxy` um.
> [!NOTE]
> Dieses Thema bezieht sich auf Azure SQL-Datenbank-Server, die Einzeldatenbanken und Pools für elastische Datenbanken hosten sowie SQL Data Warehouse-Datenbanken. Der Einfachheit halber wird nur SQL-Datenbank verwendet, wenn sowohl SQL-Datenbank als auch SQL Data Warehouse gemeint sind.

## <a name="connectivity-architecture"></a>Verbindungsarchitektur

Das folgende Diagramm bietet einen allgemeinen Überblick über die Verbindungsarchitektur von Azure SQL-Datenbank.

![Architekturübersicht](./media/sql-database-connectivity-architecture/connectivity-overview.png)

In den folgenden Schritten wird das Herstellen einer Verbindung mit einer Azure SQL-Datenbank beschrieben:

- Clients stellen eine Verbindung mit dem Gateway her, das über eine öffentliche IP-Adresse verfügt und an Port 1433 lauscht.
- Je nach effektiver Verbindungsrichtlinie leitet das Gateway den Datenverkehr an den richtigen Datenbankcluster um oder fungiert als Proxy.
- Innerhalb des Datenbankclusters wird der Datenverkehr an die entsprechende Azure SQL-Datenbank weitergeleitet.

## <a name="connection-policy"></a>Verbindungsrichtlinie

Die Azure SQL-Datenbank unterstützt diese drei Optionen zum Festlegen der Verbindungsrichtlinie für einen SQL-Datenbankserver:

- **Umleiten (empfohlen):** Clients stellen Verbindungen direkt mit dem Knoten her, der die Datenbank hostet. Zum Aktivieren der Konnektivität müssen die Clients ausgehende Firewallregeln für alle Azure-IP-Adressen in der Region mithilfe von Netzwerksicherheitsgruppen (NSG) mit [Diensttags](../virtual-network/security-overview.md#service-tags) für Ports 11000 bis 12000 zulassen – nicht nur für IP-Adressen des Azure SQL-Datenbankgateways an Port 1433. Da Pakete direkt an die Datenbank gesendet werden, haben Latenz und Durchsatz die Leistung verbessert.
- **Proxy:** In diesem Modus werden alle Verbindungen über die Azure SQL-Datenbankgateways hergestellt. Zum Aktivieren der Konnektivität müssen Clients über ausgehende Firewallregeln verfügen, die nur die IP-Adressen des Azure SQL-Datenbankgateways zulassen (i.d.R. zwei IP-Adressen pro Region). Dieser Modus kann je nach Workload höhere Latenzen und geringeren Durchsatz verursachen. Es wird empfohlen, die Verbindungsrichtlinie `Redirect` statt der Verbindungsrichtlinie `Proxy` zu verwenden, um die niedrigste Latenz und den höchsten Durchsatz zu erzielen.
- **Standard:** Diese Verbindungsrichtlinie ist nach dem Erstellen auf allen Servern aktiv, es sei denn, Sie ändern sie explizit in `Proxy` oder `Redirect`. Die effektive Richtlinie hängt davon ab, ob Verbindungen innerhalb von Azure (`Redirect`) oder außerhalb von Azure (`Proxy`) hergestellt werden.

## <a name="connectivity-from-within-azure"></a>Verbindung aus Azure

Wenn Sie eine Verbindung aus Azure herstellen, verfügen Ihre Verbindungen standardmäßig über die Verbindungsrichtlinie `Redirect`. Die Verbindungsrichtlinie `Redirect` bedeutet, dass nach Aufbau der TCP-Sitzung mit Azure SQL-Datenbank die Clientsitzung an den richtigen Datenbankcluster umgeleitet wird. Dabei wird die virtuelle Ziel-IP von der des Gateways für Azure SQL-Datenbank in die des Clusters geändert. Danach fließen alle nachfolgenden Pakete direkt an den Cluster, wobei das Gateway von Azure SQL-Datenbank umgangen wird. Das folgende Diagramm veranschaulicht diesen Datenverkehrfluss.

![Architekturübersicht](./media/sql-database-connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Verbindung von außerhalb von Azure

Wenn Sie von außerhalb von Azure eine Verbindung herstellen, verfügen Ihre Verbindungen standardmäßig über die Verbindungsrichtlinie `Proxy`. Die Richtlinie `Proxy` bedeutet, dass die TCP-Sitzung über das Gateway von Azure SQL-Datenbank hergestellt wird und dass alle nachfolgenden Pakete über das Gateway fließen. Das folgende Diagramm veranschaulicht diesen Datenverkehrfluss.

![Architekturübersicht](./media/sql-database-connectivity-architecture/connectivity-onprem.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>IP-Adressen vom Gateway von Azure SQL-Datenbank

Zur Verbindung mit einer Azure SQL-Datenbank von einer lokalen Ressource aus müssen Sie ausgehenden Netzwerkdatenverkehr auf dem Gateway von Azure SQL-Datenbank für Ihre Azure-Region zulassen. Ihre Verbindungen erfolgen nur über das Gateway, wenn Sie eine Verbindung im `Proxy`-Modus herstellen. Dies ist die Standardeinstellung bei der Verbindung von lokalen Ressourcen aus.

Die folgende Tabelle enthält die primäre und sekundäre IP-Adressen des Gateways von Azure SQL-Datenbank für alle Datenregionen. Für einige Regionen gibt es zwei IP-Adressen. In diesen Regionen ist die primäre IP-Adresse die aktuelle IP-Adresse des Gateways, und die zweite IP-Adresse ist eine Failover-IP-Adresse. Die Failoveradresse ist die Adresse, wohin wir möglicherweise Ihren Server verschieben, um die Verfügbarkeit des Dienst hoch zu halten. Für diese Regionen wird empfohlen, dass Sie ausgehenden Verkehr an beide IP-Adressen zulassen. Die zweite IP-Adresse ist Eigentum von Microsoft und belauscht keine Dienste von Azure SQL-Datenbank. Dies macht sie erst, wenn Sie zulassen, dass Azure SQL-Datenbank Verbindungen annimmt.

| Name der Region | Primäre IP-Adresse | Sekundäre IP-Adresse |
| --- | --- |--- |
| Australien (Osten) | 13.75.149.87 | 40.79.161.1 |
| Australien, Südosten | 191.239.192.109 | 13.73.109.251 |
| Brasilien Süd | 104.41.11.5 | |
| Kanada, Mitte | 40.85.224.249 | |
| Kanada, Osten | 40.86.226.166 | |
| USA (Mitte) | 23.99.160.139 | 13.67.215.62 |
| China, Osten 1 | 139.219.130.35 | |
| China, Osten 2 | 40.73.82.1 | |
| China, Norden 1 | 139.219.15.17 | |
| China, Norden 2 | 40.73.50.0 | |
| Asien, Osten | 191.234.2.139 | 52.175.33.150 |
| US, Osten 1 | 191.238.6.43 | 40.121.158.30 |
| USA (Ost) 2 | 191.239.224.107 | 40.79.84.180 * |
| Frankreich, Mitte | 40.79.137.0 | 40.79.129.1 |
| Deutschland, Mitte | 51.4.144.100 | |
| Deutschland, Nordosten | 51.5.144.179 | |
| Indien, Mitte | 104.211.96.159 | |
| Indien, Süden | 104.211.224.146 | |
| Indien, Westen | 104.211.160.80 | |
| Japan, Osten | 191.237.240.43 | 13.78.61.196 |
| Japan, Westen | 191.238.68.11 | 104.214.148.156 |
| Korea, Mitte | 52.231.32.42 | |
| Korea, Süden | 52.231.200.86 |  |
| USA Nord Mitte | 23.98.55.75 | 23.96.178.199 |
| Nordeuropa | 191.235.193.75 | 40.113.93.91 |
| USA Süd Mitte | 23.98.162.75 | 13.66.62.124 |
| Südostasien | 23.100.117.95 | 104.43.15.0 |
| Vereinigtes Königreich, Norden | 13.87.97.210 | |
| Vereinigtes Königreich, Süden 1 | 51.140.184.11 | |
| Großbritannien, Süden 2 | 13.87.34.7 | |
| UK, Westen | 51.141.8.11 | |
| USA, Westen-Mitte | 13.78.145.25 | |
| Europa, Westen | 191.237.232.75 | 40.68.37.158 |
| USA, Westen 1 | 23.99.34.75 | 104.42.238.205 |
| USA, Westen 2 | 13.66.226.202 | |
||||

\* **HINWEIS:** *USA, Osten 2* verfügt auch über eine tertiäre IP-Adresse von `52.167.104.0`.

## <a name="change-azure-sql-database-connection-policy"></a>Ändern der Verbindungsrichtlinie von Azure SQL-Datenbank

Um die Verbindungsrichtlinie von Azure SQL-Datenbank für einen Azure SQL-Datenbankserver zu ändern, verwenden Sie die [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).

- Wenn Ihre Verbindungsrichtlinie auf `Proxy` festgelegt ist, fließen alle Netzwerkpakete über das Gateway von Azure SQL-Datenbank. Um dies einzustellen, lassen Sie ausgehenden Datenverkehr nur für Gateway-IPs von Azure SQL-Datenbank zu. Die Einstellung `Proxy` hat längere Wartezeiten als die Einstellung `Redirect`.
- Wenn die Einstellung der Verbindungsrichtlinie `Redirect` ist, fließen alle Netzwerkpakete direkt an den Datenbankcluster. Um dies einzustellen, lassen Sie ausgehenden Datenverkehr an mehrere IP-Adressen zu.

## <a name="script-to-change-connection-settings-via-powershell"></a>Skript zum Ändern der Verbindungseinstellungen über PowerShell

> [!IMPORTANT]
> Dieses Skript erfordert das [Azure PowerShell-Modul](/powershell/azure/install-az-ps).

Das folgende PowerShell-Skript veranschaulicht, wie Sie die Verbindungsrichtlinie ändern können.

```powershell
# Get SQL Server ID
$sqlserverid=(Get-AzureRmSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).ResourceId

# Set URI
$id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
(Get-AzureRmResource -ResourceId $id).Properties.connectionType

# Update connection policy
Set-AzureRmResource -ResourceId $id -Properties @{"connectionType" = "Proxy"} -f
```

## <a name="script-to-change-connection-settings-via-azure-cli"></a>Skript zum Ändern der Verbindungseinstellungen über die Azure CLI

> [!IMPORTANT]
> Dieses Skript erfordert die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

Das folgende Skript für die Befehlszeilenschnittstelle veranschaulicht, wie Sie die Verbindungsrichtlinie ändern.

```azurecli-interactive
# Get SQL Server ID
sqlserverid=$(az sql server show -n sql-server-name -g sql-server-group --query 'id' -o tsv)

# Set URI
id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $id

# Update connection policy
az resource update --ids $id --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Ändern der Verbindungsrichtlinie von Azure SQL-Datenbank für einen Azure SQL-Datenbankserver finden Sie unter [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- Weitere Informationen zum Verbindungsverhalten von Azure SQL-Datenbank für Kunden, die ADO.NET 4.5 oder höher verwenden, finden Sie unter [Andere Ports als 1433 für ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).
- Einen allgemeinen Überblick über die Anwendungsentwicklung finden Sie unter [SQL-Datenbankanwendungsentwicklung – Übersicht](sql-database-develop-overview.md).
