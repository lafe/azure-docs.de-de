---
title: 'Tutorial: Verwenden von Azure Database Migration Service für die Onlinemigration von MongoDB zur Azure Cosmos DB-API für MongoDB | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie mit Azure Database Migration Service eine Onlinemigration von einer lokalen MongoDB-Instanz zur Azure Cosmos DB-API für MongoDB durchführen.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: douglasl
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 02/12/2019
ms.openlocfilehash: 95286b7a63471ee07f76276d8b4b63ca5f2aecce
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56212030"
---
# <a name="tutorial-migrate-mongodb-to-azure-cosmos-dbs-api-for-mongodb-online-using-dms-preview"></a>Tutorial: Onlinemigration von MongoDB zur Azure Cosmos DB-API für MongoDB mit DMS (Vorschauversion)
Sie können Azure Database Migration Service zum Durchführen einer Onlinemigration (minimale Ausfallzeit) von Datenbanken aus einer lokalen oder cloudbasierten MongoDB-Instanz zur Azure Cosmos DB-API für MongoDB verwenden.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen einer Instanz von Azure Database Migration Service
> * Erstellen Sie ein Migrationsprojekt mithilfe von Azure Database Migration Service.
> * Ausführen der Migration
> * Überwachen der Migration
> * Fertigstellen der Migration, wenn Sie dazu bereit sind

In diesem Tutorial migrieren Sie ein Dataset in MongoDB, das auf einem virtuellen Azure-Computer gehostet wird, mit Azure Database Migration Service zur Azure Cosmos DB-API für MongoDB. Hierbei kommt es nur zu minimaler Ausfallzeit. Wenn Sie noch keine MongoDB-Quelle eingerichtet haben, lesen Sie den Artikel [Installieren und Konfigurieren von MongoDB auf einem virtuellen Windows-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/install-mongodb).

> [!NOTE]
> Die Verwendung von Azure Database Migration Service zum Ausführen einer Onlinemigration erfordert das Erstellen einer Instanz auf der Grundlage des Premium-Tarifs.

> [!IMPORTANT]
> Für eine optimale Migration empfiehlt Microsoft die Erstellung einer Azure Database Migration Service-Instanz in derselben Azure-Region, in der sich auch die Zieldatenbank befindet. Die Verschiebung von Daten zwischen Regionen oder Geografien kann den Migrationsvorgang verlangsamen.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

In diesem Artikel wird eine Onlinemigration von MongoDB zur Azure Cosmos DB-API für MongoDB beschrieben. Informationen zu einer Offlinemigration finden Sie unter [Offlinemigration von MongoDB zur Azure Cosmos DB-API für MongoDB mit DMS](tutorial-mongodb-cosmos-db.md).

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:
- [Erstellen einer Azure Cosmos DB-API für das MongoDB-Konto.](https://ms.portal.azure.com/#create/Microsoft.DocumentDB)
- Erstellen Sie ein virtuelles Azure-Netzwerk (VNET) für Azure Database Migration Service, indem Sie das Azure Resource Manager-Bereitstellungsmodell verwenden, das entweder über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder über [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) Standort-zu-Standort-Konnektivität für Ihre lokalen Quellserver bereitstellt.
- Stellen Sie sicher, dass die Netzwerksicherheitsgruppen-Regeln des VNET nicht die folgenden Kommunikationsports blockieren: 443, 53, 9354, 445 und 12000. Weitere Details zur Datenverkehrsfilterung mit NSG in Azure VNET finden Sie im Artikel [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Ändern Sie die Firewall des Quellservers, damit Azure Database Migration Service auf die MongoDB-Server-Quellinstanz zugreifen kann (standardmäßig TCP-Port 27017).
- Wenn Sie eine Firewall-Appliance vor Ihren Quelldatenbanken verwenden, müssen Sie möglicherweise Firewallregeln hinzufügen, damit Azure Database Migration Service auf die Quelldatenbanken für die Migration zugreifen kann.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registrieren des Ressourcenanbieters „Microsoft.DataMigration“
1. Melden Sie sich beim Azure-Portal an, und klicken Sie auf **Alle Dienste** und anschließend auf **Abonnements**.

   ![Abonnements im Portal anzeigen](media/tutorial-mongodb-to-cosmosdb-online/portal-select-subscription1.png)
       
2. Wählen Sie das Abonnement aus, in dem Sie die Azure Database Migration Service-Instanz erstellen möchten, und klicken Sie auf **Ressourcenanbieter**.
 
    ![Ressourcenanbieter anzeigen](media/tutorial-mongodb-to-cosmosdb-online/portal-select-resource-provider.png)
    
3.  Suchen Sie nach „Migration“, und wählen Sie rechts neben **Microsoft.DataMigration** die Option **Registrieren** aus.
 
    ![Registrieren des Ressourcenanbieters](media/tutorial-mongodb-to-cosmosdb-online/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Erstellen einer Instanz
1.  Wählen Sie im Azure-Portal die Option **+ Ressource erstellen**, suchen Sie nach Azure Database Migration Service, und wählen Sie dann **Azure Database Migration Service** aus der Dropdownliste aus.

    ![Azure Marketplace](media/tutorial-mongodb-to-cosmosdb-online/portal-marketplace.png)

2.  Wählen Sie auf dem Bildschirm **Azure Database Migration Service** die Schaltfläche **Erstellen** aus.
 
    ![Erstellen einer Instanz von Azure Database Migration Service](media/tutorial-mongodb-to-cosmosdb-online/dms-create1.png)
  
3.  Geben Sie auf dem Bildschirm **Migrationsdienst erstellen** einen Namen für den Dienst, das Abonnement und eine neue oder vorhandene Ressourcengruppe an.

4. Wählen Sie den Standort aus, an dem Sie die Azure Database Migration Service-Instanz erstellen möchten. 

5. Wählen Sie ein vorhandenes virtuelles Netzwerk (VNET) aus, oder erstellen Sie ein neues.

    Das VNET erteilt Azure Database Migration Service Zugriff auf die MongoDB-Quellinstanz und auf das Azure Cosmos DB-Zielkonto.

    Weitere Informationen zum Erstellen des VNET im Azure-Portal finden Sie im Artikel [Erstellen eines virtuellen Netzwerks im Azure Portal](https://aka.ms/DMSVnet).

6. Wählen Sie eine SKU aus dem Premium-Tarif aus.

    > [!NOTE]
    > Onlinemigrationen werden nur bei Verwendung des Premium-Tarifs unterstützt. Weitere Informationen zu Kosten und Tarifen finden Sie in der [Preisübersicht](https://aka.ms/dms-pricing).

    Beachten Sie die Empfehlungen in [diesem Blogbeitrag](https://go.microsoft.com/fwlink/?linkid=861067), falls Sie Hilfe bei der Wahl des richtigen Azure Database Migration Service-Tarifs benötigen.  

     ![Konfigurieren der Einstellungen einer Azure Database Migration Service-Instanz](media/tutorial-mongodb-to-cosmosdb-online/dms-settings3.png)

7.  Wählen Sie **Erstellen**, um den Dienst zu erstellen.

## <a name="create-a-migration-project"></a>Erstellen eines Migrationsprojekts
Nachdem der Dienst erstellt wurde, suchen Sie diesen im Azure-Portal, öffnen Sie ihn, und erstellen Sie anschließend ein neues Migrationsprojekt.

1. Wählen Sie im Azure-Portal **Alle Dienste**, suchen Sie nach Azure Database Migration Service, und wählen Sie dann **Azure Database Migration Service** aus.
 
    ![Suchen aller Instanzen von Azure Database Migration Service](media/tutorial-mongodb-to-cosmosdb-online/dms-search.png)

2. Suchen Sie im Bildschirm **Azure Database Migration Services** nach dem Namen der von Ihnen erstellten Azure Database Migration Service-Instanz, und wählen Sie die Instanz aus.

    Alternativ hierzu können Sie die Azure Database Migration Service-Instanz auch über den Suchbereich im Azure-Portal ermitteln.

    ![Verwenden des Suchbereichs im Azure-Portal](media/tutorial-mongodb-to-cosmosdb-online/dms-search-portal.png)

3. Wählen Sie **+ Neues Migrationsprojekt** aus.

4. Geben Sie im Fenster **Neues Migrationsprojekt** einen Projektnamen ein, und wählen Sie im Textfeld **Quellservertyp** die Option **MongoDB** und im Textfeld **Zielservertyp** die Option **CosmosDB (MongoDB-API)**. Wählen Sie außerdem für **Aktivitätstyp auswählen** die Option **Onlinedatenmigration [Vorschauversion]**.

    ![Erstellen eines Database Migration Service-Projekts](media/tutorial-mongodb-to-cosmosdb-online/dms-create-project1.png)

5.  Klicken Sie auf **Speichern**, und wählen Sie dann **Aktivität erstellen und ausführen**, um das Projekt zu erstellen und die Migrationsaktivität auszuführen.

## <a name="specify-source-details"></a>Angeben von Quelldetails
1. Geben Sie im Bildschirm **Source details** (Quelldetails) die Verbindungsdetails für die MongoDB-Server-Quellinstanz an.

    Es gibt drei Modi, um eine Verbindung mit einer Quelle herzustellen:
       * **Standardmodus**, in dem ein vollqualifizierter Domänenname oder eine IP-Adresse, eine Portnummer und die Anmeldeinformationen für die Verbindung akzeptiert werden.
       * **Verbindungszeichenfolgen-Modus**, in dem eine MongoDB-Verbindungszeichenfolge akzeptiert wird. Dies ist im Artikel [Connection String URI Format](https://docs.mongodb.com/manual/reference/connection-string/) (URI-Format von Verbindungszeichenfolgen) beschrieben.
       * **Daten aus Azure-Speicher**, wobei eine Blobcontainer-SAS-URL akzeptiert wird. Aktivieren Sie die Option **Das Blob enthält BSON-Speicherabbilder**, wenn für den Blobcontainer vom MongoDB-Tool [bsondump](https://docs.mongodb.com/manual/reference/program/bsondump/) erstellte BSON-Speicherabbilder vorhanden sind. Deaktivieren Sie sie, wenn der Container JSON-Dateien enthält.

    Sie können die IP-Adresse für Situationen verwenden, in denen eine DNS-Namensauflösung nicht möglich ist.

   ![Angeben von Quelldetails](media/tutorial-mongodb-to-cosmosdb-online/dms-specify-source1.png)

2. Wählen Sie **Speichern** aus.

   > [!NOTE]
   > Die Quellserveradresse sollte die Adresse des primären Replikats sein, wenn die Quelle eine Replikatgruppe ist, und die Adresse des Routers, wenn es sich bei der Quelle um einen MongoDB-Cluster mit Sharding handelt. Für einen MongoDB-Cluster mit Sharding muss Azure Database Migration Service eine Verbindung mit den einzelnen Shards im Cluster herstellen können, und hierzu kann es erforderlich sein, die Firewall auf weiteren Computern zu öffnen.          

## <a name="specify-target-details"></a>Angeben von Zieldetails
1. Geben Sie auf dem Bildschirm **Zieldetails für die Migration** die Verbindungsdetails für das Azure Cosmos DB-Zielkonto an (also für die vorab bereitgestellte Azure Cosmos DB-API für das MongoDB-Konto, zu dem Sie die MongoDB-Daten migrieren).

    ![Angeben von Zieldetails](media/tutorial-mongodb-to-cosmosdb-online/dms-specify-target1.png)

2. Wählen Sie **Speichern** aus.

## <a name="map-to-target-databases"></a>Zuordnen zu Zieldatenbanken
1. Ordnen Sie im Bildschirm **Map to target databases** (Zu Zieldatenbanken zuordnen) die Quell- und die Zieldatenbank für die Migration einander zu.

    Wenn die Zieldatenbank denselben Datenbanknamen wie die Quelldatenbank enthält, wählt Azure Database Migration Service die Zieldatenbank standardmäßig aus.

    Wenn die Zeichenfolge **Erstellen** neben dem Datenbanknamen angezeigt wird, hat der Azure Database Migration Service die Zieldatenbank nicht gefunden, und der Dienst erstellt die Datenbank für Sie.

    Geben Sie einen RU-Wert für den Durchsatz an, falls an diesem Punkt der Migration die Freigabe des Durchsatzes gewünscht ist. In Cosmos DB können Sie den Durchsatz entweder auf Datenbankebene oder für jede Sammlung individuell bereitstellen. Der Durchsatz wird in [Anforderungseinheiten](https://docs.microsoft.com/azure/cosmos-db/request-units) (RUs) gemessen. Weitere Informationen: [Azure Cosmos DB – Preise](https://azure.microsoft.com/pricing/details/cosmos-db/).

    ![Zuordnen zu Zieldatenbanken](media/tutorial-mongodb-to-cosmosdb-online/dms-map-target-databases1.png)

2. Wählen Sie **Speichern** aus.

3. Erweitern Sie auf dem Einstellungsbildschirm **Sammlung** die Auflistung der Sammlungen, und überprüfen Sie dann die Liste der Sammlungen, die migriert werden.

    Beachten Sie, dass der Azure Database Migration Service automatisch alle Sammlungen auswählt, die in der MongoDB-Quellinstanz vorhanden sind, jedoch nicht auf dem Azure Cosmos DB-Zielkonto. Wenn Sie Sammlungen, die bereits Daten enthalten, erneut migrieren möchten, müssen Sie die Sammlungen in diesem Fenster explizit auswählen.

    Sie können die Anzahl von RUs angeben, die die Sammlungen verwenden sollen. In den meisten Fällen ist ein Wert zwischen 500 (mindestens 1.000 für Sammlungen mit Sharding) und 4.000 ausreichend. Der Azure Database Migration Service schlägt intelligente Standardwerte basierend auf der Sammlungsgröße vor.

    Sie können auch einen Shardschlüssel angeben, um die [Partitionierung in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview) für eine optimale Skalierbarkeit zu nutzen. Überprüfen Sie unbedingt die [bewährten Methoden für die Auswahl eines Shard-/Partitionsschlüssels](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey). Wenn Sie nicht über einen Partitionsschlüssel verfügen, können Sie immer **_id** als Shardschlüssel verwenden, um den Durchsatz zu verbessern.

    ![Auswählen von Sammlungstabellen](media/tutorial-mongodb-to-cosmosdb-online/dms-collection-setting1.png)

4. Wählen Sie **Speichern** aus.

5. Geben Sie auf dem Bildschirm **Migrationszusammenfassung** im Textfeld **Aktivitätsname** einen Namen für die Migrationsaktivität an.

    ![Migrationszusammenfassung](media/tutorial-mongodb-to-cosmosdb-online/dms-migration-summary1.png)

## <a name="run-the-migration"></a>Ausführen der Migration
- Wählen Sie **Migration ausführen** aus.

   Das Fenster „Migrationsaktivität“ wird angezeigt, und der **Status** der Aktivität wird angegeben.

   ![Aktivitätsstatus](media/tutorial-mongodb-to-cosmosdb-online/dms-activity-status1.png)

## <a name="monitor-the-migration"></a>Überwachen der Migration
- Klicken Sie auf dem Bildschirm „Migrationsaktivität“ auf **Aktualisieren**, um die Anzeige zu aktualisieren, bis der **Status** der Migration **Wird wiedergegeben** lautet.

   > [!NOTE]
   > Sie können die Aktivität zum Abrufen von Details der Migrationsmetriken auf Datenbank- und Sammlungsebene auswählen.

   ![Status „Wird wiedergegeben“ der Aktivität](media/tutorial-mongodb-to-cosmosdb-online/dms-activity-replaying.png)

## <a name="verify-data-in-cosmos-db"></a>Überprüfen von Daten in Cosmos DB

1. Nehmen Sie Änderungen an Ihrer MongoDB-Quelldatenbank vor.
2. Stellen Sie eine Verbindung mit COSMOS DB her, um zu überprüfen, ob die Daten vom MongoDB-Quellserver repliziert werden.

    ![Status „Wird wiedergegeben“ der Aktivität](media/tutorial-mongodb-to-cosmosdb-online/dms-verify-data.png)
 
## <a name="complete-the-migration"></a>Fertigstellen der Migration

* Nachdem alle Dokumente von der Quelle auf dem COSMOS DB-Ziel verfügbar sind, können Sie im Kontextmenü der Migrationsaktivität die Option **Fertig stellen** wählen, um die Migration abzuschließen.

    Mit dieser Aktion wird die Wiedergabe aller ausstehenden Änderungen durchgeführt und die Migration fertiggestellt.

    ![Status „Wird wiedergegeben“ der Aktivität](media/tutorial-mongodb-to-cosmosdb-online/dms-finish-migration.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

 * [Cosmos DB-Dienstinformationen](https://azure.microsoft.com/services/cosmos-db/)

## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich in der Migrationsanleitung im [Leitfaden zur Datenbankmigration von Microsoft](https://datamigration.microsoft.com/) über weitere Szenarios.