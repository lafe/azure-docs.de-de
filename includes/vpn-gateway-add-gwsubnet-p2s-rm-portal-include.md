---
title: Includedatei
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5642533fe1015e88c3b27e83139bfd26cb0b1a53
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444142"
---
1. Navigieren Sie im [Portal](http://portal.azure.com) zum virtuellen Resource Manager-Netzwerk, für das Sie ein virtuelles Netzwerkgateway erstellen möchten.
2. Klicken Sie auf der VNET-Seite im Abschnitt **Einstellungen** auf **Subnetze**, um die Seite **Subnetze** zu erweitern.
3. Klicken Sie auf der Seite **Subnetze** auf **+Gatewaysubnetz**, um die Seite **Subnetz hinzufügen** zu öffnen. 

   ![Hinzufügen des Gatewaysubnetzes](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "Hinzufügen des Gatewaysubnetzes")
4. Als **Name** für Ihr Subnetz wird automatisch der Wert „GatewaySubnet“ eingefügt. Dieser Wert ist erforderlich, damit Azure das Subnetz als Gatewaysubnetz erkennt. Passen Sie die automatisch ausgefüllten Werte für **Adressbereich** an Ihre Konfigurationsanforderungen an. Konfigurieren Sie keine Routingtabelle und keine Dienstendpunkte.

   ![Hinzufügen des Subnetzes](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "Hinzufügen des Subnetzes")
5. Klicken Sie unten auf der Seite auf **OK**, um das Subnetz zu erstellen.
