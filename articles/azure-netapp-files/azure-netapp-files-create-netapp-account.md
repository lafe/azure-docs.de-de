---
title: Erstellen eines NetApp-Kontos für den Zugriff auf Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt das Zugreifen auf Azure NetApp Files sowie das Erstellen eines NetApp-Kontos, um einen Kapazitätspool einrichten und ein Volume erstellen zu können.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 47b9d25f8db2241bb578528780e28f43d56371e5
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55963079"
---
# <a name="create-a-netapp-account"></a>Erstellen eines NetApp-Kontos
Nach der Erstellung eines NetApp-Kontos können Sie einen Kapazitätspool einrichten und anschließend ein Volume erstellen. Neue NetApp-Konten werden über das Blatt „Azure NetApp Files“ erstellt.

## <a name="before-you-begin"></a>Voraussetzungen
Sie müssen Ihr Abonnement registriert haben, um den NetApp-Ressourcenanbieter und das Public Preview-Feature zu nutzen.

[Registrieren für Azure NetApp Files](azure-netapp-files-register.md)

## <a name="steps"></a>Schritte 

1. Melden Sie sich beim Azure-Portal an. 
2. Greifen Sie mithilfe einer der folgenden Methoden auf das Blatt „Azure NetApp Files“ zu:  
  * Suchen Sie im Suchfeld des Azure-Portals nach **Azure NetApp Files**.  
  * Klicken Sie im Navigationsbereich auf **Alle Dienste**, und filtern Sie nach „Azure NetApp Files“.  

  Sie können das Blatt „Azure NetApp Files“ als Favorit speichern, indem Sie daneben auf das Sternsymbol klicken. 

3. Klicken Sie auf **+ Hinzufügen**, um ein neues NetApp-Konto zu erstellen.  
  Das Fenster für das neue NetApp-Konto wird angezeigt.  

4. Geben Sie für Ihr NetApp-Konto folgende Informationen an: 
  * **Kontoname**  
    Geben Sie einen eindeutigen Namen für das Abonnement an.
  *  **Abonnement**  
    Wählen Sie eines der vorhandenen Abonnements aus.
  * **Ressourcengruppe**   
    Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue.
  * **Location**  
    Wählen Sie die Region aus, in der sich das Konto und dessen untergeordnete Ressourcen befinden sollen.  

    ![Neues NetApp-Konto](../media/azure-netapp-files/azure-netapp-files-new-netapp-account.png)


5. Klicken Sie auf **Create**.     
  Das erstellte NetApp-Konto wird nun auf dem Blatt „Azure NetApp Files“ angezeigt. 

## <a name="next-steps"></a>Nächste Schritte  

[Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)

