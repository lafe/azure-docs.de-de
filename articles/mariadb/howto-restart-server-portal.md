---
title: Neustarten eines Azure Database for MariaDB-Servers mithilfe des Azure-Portals
description: In diesem Artikel wird beschrieben, wie Sie einen Azure Database for MariaDB-Server über das Azure-Portal neu starten.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 2/7/2019
ms.openlocfilehash: 4b530ed2ffd32e2433a744b05aaab2219bf2b15a
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55896555"
---
# <a name="restart-azure-database-for-mariadb-server-using-azure-portal"></a>Neustarten eines Azure Database for MariaDB-Servers mithilfe des Azure-Portals
In diesem Thema wird erläutert, wie Sie einen Azure Database for MariaDB-Server neu starten. Es kann vorkommen, dass Sie Ihren Server zu Wartungszwecken neu starten müssen. In diesem Fall kommt es zu einem kurzen Ausfall, weil der Vorgang vom Server ausgeführt wird.

Der Neustart des Servers wird blockiert, wenn der Dienst ausgelastet ist. Beispielsweise kann der Dienst einen zuvor angeforderten Vorgang (z. B. das Skalieren von virtuellen Kernen) verarbeiten.

Die für einen Neustart benötigte Zeit hängt vom MariaDB-Wiederherstellungsprozess ab. Um die Neustartzeit zu verkürzen, empfehlen wir, die Aktivitäten auf dem Server vor dem Neustart auf ein Minimum zu beschränken.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:
- Einen [Azure Database for MariaDB-Server und eine Datenbank](./quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Ausführen des Serverneustarts

Mit den folgenden Schritten wird der MariaDB-Server neu gestartet:

1. Wählen Sie im Azure-Portal Ihren Azure Database for MariaDB-Server aus.

2. Klicken Sie auf der Seite **Übersicht** des Servers auf der Symbolleiste auf **Neu starten**.

   ![Azure Database for MariaDB – Übersicht – Schaltfläche „Neu starten“](./media/howto-restart-server-portal/2-server.png)

3. Klicken Sie auf **Ja**, um den Neustart des Servers zu bestätigen.

   ![Azure Database for MariaDB – Bestätigen des Neustarts ](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Beachten Sie, dass sich der Serverstatus in „Wird neu gestartet“ ändert.

   ![Azure Database for MariaDB – Status „Wird neu gestartet“ ](./media/howto-restart-server-portal/4-restarting-status.png)

5. Vergewissern Sie sich, dass der Neustart des Servers erfolgreich war.

   ![Azure Database for MariaDB – erfolgreicher Neustart ](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Erstellen eines Azure Database for MariaDB-Servers mithilfe des Azure-Portals](./quickstart-create-mariadb-server-database-using-azure-portal.md)