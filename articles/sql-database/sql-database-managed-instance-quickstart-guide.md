---
title: 'Schnellstart: Verwaltete Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation'
description: Hier werden die ersten Schritte mit verwalteten Azure SQL-Datenbank-Instanzen beschrieben.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: 3940c2f239a4354cfb44a499f7375f4ba34f8aa8
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55892026"
---
# <a name="getting-started-with-azure-sql-database-managed-instance"></a>Erste Schritte mit verwalteten Azure SQL-Datenbank-Instanzen

Die Bereitstellungsoption [verwaltete Instanz](sql-database-managed-instance-index.yml) erstellt eine Datenbank mit nahezu uneingeschränkter Kompatibilität mit der aktuellen lokalen SQL Server-Datenbank-Engine (Enterprise Edition). Darüber hinaus bietet sie eine native Implementierung eines [virtuellen Netzwerks (VNET)](../virtual-network/virtual-networks-overview.md) zur Behebung allgemeiner Sicherheitsrisiken sowie ein vorteilhaftes [Geschäftsmodell](https://azure.microsoft.com/pricing/details/sql-database/) für Kunden mit lokaler SQL Server-Instanz. In diesem Abschnitt erfahren Sie, wie Sie schnell eine verwaltete Instanz konfigurieren und erstellen und Ihre Datenbanken migrieren.

## <a name="quickstart-overview"></a>Schnellstartübersicht

In den folgenden Schnellstarts erfahren Sie, wie Sie schnell eine verwaltete Instanz erstellen, eine VM oder eine Point-to-Site-VPN-Verbindung für Clientanwendungen konfigurieren und eine Datenbank mithilfe einer `.bak`-Datei in Ihrer neuen verwalteten Instanz wiederherstellen:

- [Erstellen einer verwalteten Instanz im Azure-Portal](sql-database-managed-instance-get-started.md). Im Azure-Portal können Sie die erforderlichen Parameter (Benutzername/Kennwort, Anzahl von Kernen, maximaler Speicher) konfigurieren und automatisch eine Azure-Netzwerkumgebung erstellen, ohne sich mit Netzwerkdetails oder Infrastrukturanforderungen zu befassen. Sie müssen lediglich sicherstellen, dass Sie über einen [Abonnementtyp](sql-database-managed-instance-resource-limits.md#supported-subscription-types) verfügen, der die Erstellung einer verwalteten Instanz zulässt. Wenn Sie Ihr eigenes Netzwerk verwenden oder das Netzwerk anpassen möchten, lesen Sie die Dokumentation zum Konfigurieren der Netzwerkumgebung für eine verwaltete Instanz.
- Eine verwaltete Instanz wird in einem eigenen VNET ohne öffentlichen Endpunkt erstellt. Für den Clientanwendungszugriff können Sie wie in einem der folgenden Schnellstarts beschrieben eine VM im gleichen VNET (in einem anderen Subnetz) oder eine Point-to-Site-VPN-Verbindung mit dem VNET auf Ihrem Clientcomputer erstellen.
  - Für die Konnektivität von Clientanwendungen (einschließlich SQL Server Management Studio) erstellen Sie eine [Azure-VM im VNET der verwalteten Instanz](sql-database-managed-instance-configure-vm.md).
  - Richten Sie auf dem Clientcomputer, auf dem Sie SQL Server Management Studio und andere Anwendungen mit Clientkonnektivität ausführen, eine [Point-to-Site-VPN-Verbindung mit Ihrer verwalteten Instanz](sql-database-managed-instance-configure-p2s.md) ein. Dies ist eine der zwei Optionen für die Konnektivität mit Ihrer verwalteten Instanz und dem zugehörigen VNET.

  > [!NOTE]
  > Sie können auch ExpressRoute oder eine Site-to-Site-Verbindung über Ihr lokales Netzwerk verwenden. Diese Methoden werden in den Schnellstarts jedoch nicht behandelt.

Nachdem Sie eine verwaltete Instanz erstellt und den Zugriff konfiguriert haben, können Sie mit der Migration Ihrer Datenbanken in der lokalen SQL Server-Instanz oder auf Azure-VMs beginnen. Die Migration schlägt fehl, wenn die zu migrierende Quelldatenbank nicht unterstützte Features enthält. Um Fehler zu vermeiden und die Kompatibilität zu überprüfen, können Sie den [Datenmigrations-Assistenten (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) installieren. Dieser analysiert Ihre Datenbanken in SQL Server und ermittelt sämtliche Probleme, die die Migration zu einer verwalteten Instanz verhindern können (beispielsweise das Vorhandensein von [FileStream](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) oder mehreren Protokolldateien). Nachdem Sie diese Probleme behoben haben, sind Ihre Datenbanken bereit für die Migration zur verwalteten Instanz. Der [Assistent für Datenbankexperimente](https://blogs.msdn.microsoft.com/datamigration/2018/08/06/release-database-experimentation-assistant-dea-v2-6/) ist ein nützliches Tool, das Ihre Workload in SQL Server aufzeichnen und in der verwalteten Instanz wiedergeben kann, um zu ermitteln, ob nach der Migration zu einer verwalteten Instanz Leistungsprobleme zu erwarten sind.

Sobald Sie sicher sind, dass Ihre Datenbank zu einer verwalteten Instanz migriert werden kann, können Sie die nativen Wiederherstellungsfunktionen von SQL Server verwenden, um eine Datenbank aus einer `.bak`-Datei in einer verwalteten Instanz wiederherzustellen. Einen Schnellstart finden Sie unter [Wiederherstellen einer Datenbank aus einer Sicherung in einer verwalteten Instanz](sql-database-managed-instance-get-started-restore.md). In diesem Schnellstart stellen Sie eine Datenbank mit dem Transact-SQL-Befehl `RESTORE` aus einer in Azure Blob Storage gespeicherten `.bak`-Datei wieder her. 

> [!TIP]
> Informationen zum Erstellen einer Sicherung Ihrer Datenbank in Azure Blob Storage mit dem Transact-SQL-Befehl `BACKUP` finden Sie unter [SQL Server-Sicherung über URLs](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url).

In diesen Schnellstarts erfahren Sie, wie Sie schnell eine Datenbanksicherung erstellen, konfigurieren und in einer verwalteten Instanz wiederherstellen. In einigen Szenarien müssen Sie die Bereitstellung von verwalteten Instanzen und der erforderlichen Netzwerkumgebung anpassen oder automatisieren. Diese Szenarien werden im Anschluss beschrieben.

## <a name="customize-network-environment"></a>Anpassen der Netzwerkumgebung

Wenn die Instanz über das Azure-Portal erstellt wird, kann das VNET/Subnetz automatisch konfiguriert werden. Sie können das VNET/Subnetz aber auch vor der Erstellung verwalteter Instanzen erstellen, damit Sie die Parameter des VNETs/Subnetzes konfigurieren können. Die einfachste Methode zum Erstellen und Konfigurieren der Netzwerkumgebung besteht darin, das Netzwerk und das Subnetz für die verwaltete Instanz mit einer [Azure-Vorlage für die Ressourcenbereitstellung](sql-database-managed-instance-create-vnet-subnet.md) zu erstellen und zu konfigurieren. Sie müssen lediglich auf die Bereitstellungsschaltfläche von Azure Resource Manager klicken und das Formular mit Parametern ausfüllen. 

Alternativ können Sie die Erstellung des Netzwerks auch mit diesem [PowerShell-Skript](https://www.powershellmagazine.com/2018/07/23/configuring-azure-environment-to-set-up-azure-sql-database-managed-instance-preview/) automatisieren.

Wenn Sie bereits über ein VNET und ein Subnetz verfügen, in dem Sie Ihre verwaltete Instanz bereitstellen möchten, müssen diese die [Netzwerkanforderungen](sql-database-managed-instance-connectivity-architecture.md#network-requirements) erfüllen. Verwenden Sie dieses [PowerShell-Skript, um zu überprüfen, ob Ihr Subnetz korrekt konfiguriert ist](sql-database-managed-instance-configure-vnet-subnet.md). Das Skript überprüft Ihr Netzwerk, meldet Probleme, informiert Sie über erforderliche Änderungen und bietet anschließend an, die nötigen Änderungen an Ihrem VNET/Subnetz vorzunehmen. Führen Sie dieses Skript aus, wenn Sie Ihr VNET/Subnetz nicht manuell konfigurieren möchten. Sie können es auch nach jeder größeren Neukonfigurationen Ihrer Netzwerkinfrastruktur ausführen. Wenn Sie Ihr eigenes Netzwerk erstellen und konfigurieren möchten, finden Sie unter [Verbindungsarchitektur](sql-database-managed-instance-connectivity-architecture.md) und [The Ultimate guide for creating and configuring Azure SQL Managed Instance environment](https://medium.com/azure-sqldb-managed-instance/the-ultimate-guide-for-creating-and-configuring-azure-sql-managed-instance-environment-91ff58c0be01) (Ultimativer Leitfaden für das Erstellen und Konfigurieren einer Umgebung für die verwalteten Azure SQL-Instanz) weitere Informationen.

## <a name="automating-creation-of-a-managed-instance"></a>Automatisieren der Erstellung einer verwalteten Instanz

 Falls Sie die Netzwerkumgebung noch nicht wie im vorherigen Schritt beschrieben erstellt haben, kann das Azure-Portal dies für Sie erledigen. Dabei wird die Umgebung allerdings mit einigen Standardparametern konfiguriert, die Sie später nicht mehr ändern können. Alternativ können Sie folgende Optionen nutzen:

- [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/)
- [PowerShell mit Resource Manager-Vorlage](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md)
- [Azure-Befehlszeilenschnittstelle](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/11/14/create-azure-sql-managed-instance-using-azure-cli/).

## <a name="migrating-to-a-managed-instance-with-minimal-downtime"></a>Migrieren zu einer verwalteten Instanz mit minimaler Downtime

In den Artikeln in diesen Schnellstarts erfahren Sie, wie Sie schnell eine verwaltete Instanz einrichten und Ihre Datenbanken mithilfe der nativen `RESTORE`-Funktion migrieren. Bei der Verwendung der nativen `RESTORE`-Funktion müssen Sie jedoch warten, bis die Datenbanken wiederhergestellt wurden (und in Azure Blob Storage kopiert wurden, sofern sie dort noch nicht gespeichert sind). Dies führt insbesondere bei größeren Datenbanken zu einer Downtime Ihrer Anwendung. Verwenden Sie für Ihre Produktionsdatenbank [Data Migration Service (DMS)](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance?toc=/azure/sql-database/toc.json), um die Datenbank mit minimaler Downtime zu migrieren. DMS pusht die in Ihrer Quelldatenbank vorgenommenen Änderungen inkrementell in die Datenbank der verwalteten Instanz, die wiederhergestellt wird, und minimiert so die Downtime. Dadurch können Sie Ihre Anwendung schnell und mit minimaler Downtime von der Quell- auf die Zieldatenbank umstellen.

## <a name="next-steps"></a>Nächste Schritte

- Machen Sie sich mit einer [allgemeinen Liste unterstützter Features in verwalteten Instanzen](sql-database-features.md) sowie mit [Details und bekannten Problemen](sql-database-managed-instance-transact-sql-information.md) vertraut.
- Informieren Sie sich über [technische Merkmale der verwalteten Instanz](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits). 
- Weiterführende Schrittanleitungen finden Sie unter [Verwenden einer verwalteten Instanz in Azure SQL-Datenbank](sql-database-howto-managed-instance.md). 
