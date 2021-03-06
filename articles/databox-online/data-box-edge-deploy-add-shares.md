---
title: Übertragen von Daten mit Azure Data Box Edge | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Freigaben auf einem Data Box Edge-Gerät hinzufügen und eine Verbindung mit diesen Freigaben herstellen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to add and connect to shares on Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 6810818e48329d883961c840fa83857d84b98fd4
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56112868"
---
# <a name="tutorial-transfer-data-with-azure-data-box-edge-preview"></a>Tutorial: Übertragen von Daten mit Azure Data Box Edge (Vorschauversion)

In diesem Tutorial erfahren Sie, wie Sie auf Ihrem Data Box Edge-Gerät Freigaben hinzufügen und eine Verbindung mit diesen Freigaben herstellen. Nachdem die Freigaben hinzugefügt wurden, kann Data Box Edge Daten an Azure übertragen.

Dieser Vorgang kann bis zu zehn Minuten dauern. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen einer Freigabe
> * Herstellen einer Verbindung mit der Freigabe

> [!IMPORTANT]
> Data Box Edge befindet sich in der Vorschauphase. Lesen Sie die [Azure-Vertragsbedingungen für Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bestellen und bereitstellen. 
 
## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich, dass Folgendes erfüllt ist, bevor Sie Ihrem Data Box Edge-Gerät Freigaben hinzufügen:
* Sie haben Ihr physisches Gerät gemäß der Anleitung unter [Installieren von Azure Data Box Edge](data-box-edge-deploy-install.md) installiert. 

* Das physische Gerät ist aktiviert, wie in [Verbinden, Einrichten und Aktivieren von Azure Data Box Edge](data-box-edge-deploy-connect-setup-activate.md) beschrieben. 

* Das Gerät ist für die Erstellung von Freigaben und die Übertragung von Daten bereit.


## <a name="add-a-share"></a>Hinzufügen einer Freigabe

Gehen Sie wie folgt vor, um eine Freigabe zu erstellen:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zu **Alle Ressourcen**, und suchen Sie nach Ihrer Data Box Edge-Ressource.
    
1. Wählen Sie in der gefilterten Ressourcenliste Ihre Data Box Edge-Ressource aus.

1. Wählen Sie im linken Bereich **Übersicht** und dann **Freigabe hinzufügen** aus.
   
   ![Hinzufügen einer Freigabe](./media/data-box-edge-deploy-add-shares/click-add-share.png)

1. Gehen Sie im Bereich **Freigabe hinzufügen** wie folgt vor:

    a. Geben Sie im Feld **Name** einen eindeutigen Namen für die Freigabe an.  
    Der Freigabename darf nur Kleinbuchstaben, Ziffern und Bindestriche enthalten. Er muss aus 3 bis 63 Zeichen bestehen und mit einem Buchstaben oder einer Ziffer beginnen. Bindestriche müssen vorangestellt und von einem Buchstaben oder einer Ziffer gefolgt werden.
    
    b. Wählen Sie einen **Typ** für die Freigabe aus.  
    Zur Auswahl stehen **SMB** und **NFS** (Standardeinstellung: SMB). „SMB“ ist die Standardeinstellung für Windows-Clients, und „NFS“ wird für Linux-Clients verwendet.  
    Je nachdem, ob Sie sich für SMB- oder NFS-Freigaben entscheiden, variiert der Rest der Optionen geringfügig. 

    c. Geben Sie ein Speicherkonto an, in dem die Freigabe gespeichert wird.  
    Wenn ein Container noch nicht vorhanden ist, wird er im Speicherkonto mit dem neu erstellten Freigabenamen angelegt. Wenn er bereits vorhanden ist, wird der vorhandene Container verwendet. 
    
    d. Wählen Sie in der Dropdownliste **Speicherdienst** **Blockblob**, **Seitenblob** oder **Dateien** aus.  
    Der ausgewählte Diensttyp hängt von dem Format ab, in dem die Daten in Azure verwendet werden sollen. In diesem Beispiel sollen die Daten als Blobblöcke in Azure gespeichert. Daher wählen wir **Blockblob** aus. Bei Verwendung von „Seitenblob“ müssen Ihre Daten ganzzahlige Vielfache von 512 Bytes sein. VHDX-Daten sind beispielsweise immer ganzzahlige Vielfache von 512 Bytes.
   
    e. Je nachdem, ob Sie eine SMB-Freigabe oder eine NFS-Freigabe erstellt haben, führen Sie einen der folgenden Schritte aus: 
     
    - **SMB-Freigabe**: Wählen Sie unter **Alle lokalen Benutzer mit Berechtigungen** die Option **Neu erstellen** oder **Vorhandene verwenden**. Wenn Sie einen neuen lokalen Benutzer anlegen, geben Sie einen Benutzernamen und ein Kennwort ein, und bestätigen Sie dann das Kennwort. Dadurch werden die Berechtigungen dem lokalen Benutzer zugewiesen. Nachdem Sie hier die Berechtigungen zugewiesen haben, können Sie den Datei-Explorer verwenden, um diese zu ändern.

        Wenn Sie für diese Freigabedaten das Kontrollkästchen **Nur Lesevorgänge zulassen** aktivieren, können Sie Benutzer angeben, die nur über Lesezugriff verfügen.

        ![Hinzufügen einer SMB-Freigabe](./media/data-box-edge-deploy-add-shares/add-share-smb-1.png)
   
    - **NFS-Freigabe**: Geben Sie die IP-Adressen der zulässigen Clients ein, die auf die Freigabe zugreifen können.

        ![Hinzufügen einer NFS-Freigabe](./media/data-box-edge-deploy-add-shares/add-share-nfs-1.png)
   
1. Wählen Sie **Erstellen** aus, um die Freigabe zu erstellen. 
    
    Sie werden benachrichtigt, wenn die Freigabe erstellt wird. Nachdem die Freigabe mit den angegebenen Einstellungen erstellt wurde, wird der Abschnitt **Freigaben** mit den neuen Freigabeinformationen aktualisiert. 
    
    ![Aktualisierte Liste der Freigaben](./media/data-box-edge-deploy-add-shares/updated-list-of-shares.png) 

## <a name="connect-to-the-share"></a>Herstellen einer Verbindung mit der Freigabe

Nun können Sie eine Verbindung mit den Freigaben herstellen, die Sie im vorherigen Schritt erstellt haben. Die auszuführenden Schritte sind ggf. abhängig von der verwendeten Freigabe (SMB oder NFS). 

### <a name="connect-to-an-smb-share"></a>Herstellen einer Verbindung mit einer SMB-Freigabe

Verbinden Sie sich auf Ihrem Windows Server-Client, der mit Ihrem Data Box Edge-Gerät verbunden ist, mit einer SMB-Freigabe, indem Sie die Befehle eingeben:


1. Geben Sie in einem Befehlsfenster den folgenden Befehl ein:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

1. Wenn Sie dazu aufgefordert werden, geben Sie das Kennwort für die Freigabe ein.  
   Hier sehen Sie eine Beispielausgabe des Befehls.

    ```powershell
    Microsoft Windows [Version 10.0.16299.192) 
    (c) 2017 Microsoft Corporation. All rights reserved. 
    
    C: \Users\DataBoxEdgeUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser 
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60': 
    The command completed successfully. 
    
    C: \Users\DataBoxEdgeUser>
    ```   


1. Wählen Sie auf der Tastatur „Windows + R“ aus. 

1. Geben Sie im Fenster **Ausführen** `\\<device IP address>` an, und wählen Sie anschließend **OK** aus.  
   Der Datei-Explorer wird geöffnet. Die von Ihnen erstellten Freigaben sollten jetzt als Ordner angezeigt werden. Doppelklicken Sie im Datei-Explorer auf eine Freigabe (einen Ordner), um den Inhalt anzuzeigen.
 
    ![Herstellen einer Verbindung mit einer SMB-Freigabe](./media/data-box-edge-deploy-add-shares/connect-to-share2.png)

    Die Daten werden bei ihrer Generierung in diese Freigaben geschrieben, und das Gerät pusht die Daten in die Cloud.

### <a name="connect-to-an-nfs-share"></a>Herstellen einer Verbindung mit einer NFS-Freigabe

Führen Sie auf dem mit Ihrem Data Box Edge-Gerät verbundenen Linux-Client die folgenden Schritte aus:

1. Stellen Sie sicher, dass der NFSv4-Client auf dem Client installiert ist. Verwenden Sie zum Installieren des NFS-Clients den folgenden Befehl:

   `sudo apt-get install nfs-common`

    Weitere Informationen finden Sie unter [Install NFSv4 client](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client) (Installieren des NFSv4-Clients).

1. Führen Sie nach der Installation des NFS-Clients den folgenden Befehl aus, um die NFS-Freigabe einzubinden, die Sie auf Ihrem Data Box Edge-Gerät erstellt haben:

   `sudo mount -t nfs -o sec=sys,resvport <device IP>:/<NFS shares on device> /home/username/<Folder on local Linux computer>`

    > [!IMPORTANT]
    > Vergewissern Sie sich vor dem Einbinden der Freigaben, dass die Verzeichnisse, die als Bereitstellungspunkte auf Ihrem lokalen Computer fungieren, bereits erstellt wurden. Diese Verzeichnisse dürfen keine Dateien oder Unterordner enthalten.

    Das folgende Beispiel zeigt, wie Sie über NFS eine Verbindung mit einer Freigabe auf Ihrem Data Box Edge-Gerät herstellen. Die Geräte-IP lautet `10.10.10.60`. Die Freigabe `mylinuxshare2` wird in „ubuntuVM“ eingebunden. Der Bereitstellungspunkt der Freigabe ist `/home/databoxubuntuhost/edge`.

    `sudo mount -t nfs -o sec=sys,resvport 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/Edge`

> [!NOTE] 
> Für die Vorschauversion gelten die folgenden Einschränkungen:
> - Das Umbenennen einer Datei, die in den Freigaben erstellt wurde, wird nicht unterstützt. 
> - Durch das Löschen einer Datei aus einer Freigabe wird der Eintrag im Speicherkonto nicht gelöscht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden unter anderem folgende Data Box Edge-Themen behandelt:

> [!div class="checklist"]
> * Hinzufügen einer Freigabe
> * Herstellen einer Verbindung mit einer Freigabe

Um zu erfahren, wie Sie Ihre Daten mit Data Box Edge transformieren, fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Transformieren von Daten mit Data Box Edge](./data-box-edge-deploy-configure-compute.md)


