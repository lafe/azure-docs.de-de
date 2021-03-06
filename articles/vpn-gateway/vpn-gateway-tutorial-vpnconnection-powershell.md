---
title: Erstellen und Verwalten von Azure-S2S-VPN-Verbindungen mit PowerShell | Microsoft-Dokumentation
description: 'Tutorial: Erstellen und Verwalten von S2S-VPN-Verbindungen mit dem Azure PowerShell-Modul'
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 05/08/2018
ms.author: yushwang
ms.custom: mvc
ms.openlocfilehash: 0c71062bded65f8aa7c259c0678ee6675e2dab38
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55509472"
---
# <a name="create-and-manage-s2s-vpn-connections-with-the-azure-powershell-module"></a>Erstellen und Verwalten von S2S-VPN-Verbindungen mit dem Azure PowerShell-Modul

Azure-S2S-VPN-Verbindungen ermöglichen sichere, standortübergreifende Konnektivität zwischen lokalen Kundenumgebungen und Azure. Dieses Tutorial führt Sie durch Lebenszyklen von IPsec-S2S-VPN-Verbindungen wie etwa das Erstellen und Verwalten einer S2S-VPN-Verbindung. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen einer S2S-VPN-Verbindung
> * Aktualisieren der Verbindungseigenschaft: vorinstallierter Schlüssel, BGP, IPsec-/IKE-Richtlinie
> * Hinzufügen weiterer VPN-Verbindungen
> * Löschen einer VPN-Verbindung

In der folgenden Abbildung wird die Topologie für dieses Tutorial veranschaulicht:

![Abbildung einer Site-to-Site-VPN-Verbindung](./media/vpn-gateway-tutorial-vpnconnection-powershell/site-to-site-diagram.png)

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und nutzen möchten, müssen Sie für dieses Tutorial mindestens Version 5.3 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/azurerm/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="requirements"></a>Requirements (Anforderungen)

Absolvieren Sie das erste Tutorial [Erstellen eines VPN-Gateways mit Azure PowerShell](vpn-gateway-tutorial-create-gateway-powershell.md), um die folgenden Ressourcen zu erstellen:

1. Ressourcengruppe (TestRG1), virtuelles Netzwerk (VNet1) und GatewaySubnet
2. VPN-Gateway (VNet1GW)

Die Parameterwerte des virtuellen Netzwerks sind unten aufgeführt. Beachten Sie die zusätzlichen Werte für das Gateway des lokalen Netzwerks zur Darstellung Ihres lokalen Netzwerks. Ändern Sie die Werte gemäß Ihrer Umgebung und Netzwerkeinrichtung.

```azurepowershell-interactive
# Virtual network
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "East US"
$VNet1Prefix = "10.1.0.0/16"
$VNet1ASN    = 65010
$Gw1         = "VNet1GW"

# On-premises network
$LNG1        = "VPNsite1"
$LNGprefix1  = "10.101.0.0/24"
$LNGprefix2  = "10.101.1.0/24"
$LNGIP1      = "YourDevicePublicIP"

# Optional - on-premises BGP properties
$LNGASN1     = 65011
$BGPPeerIP1  = "10.101.1.254"

# Connection
$Connection1 = "VNet1ToSite1"
```

Der Workflow zum Erstellen einer S2S-VPN-Verbindung ist einfach:

1. Erstellen eines Gateways des lokalen Netzwerks zur Darstellung Ihres lokalen Netzwerks
2. Erstellen einer Verbindung zwischen Ihrem Azure-VPN-Gateway und dem Gateway des lokalen Netzwerks

## <a name="create-a-local-network-gateway"></a>Erstellen eines Gateways für das lokale Netzwerk

Ein lokales Netzwerkgateways stellt Ihr lokales Netzwerk dar. Sie können die Eigenschaften des lokalen Netzwerks im Gateway des lokalen Netzwerk angeben, wie etwa Folgende:

* Öffentliche IP-Adresse Ihres VPN-Geräts
* Lokaler Adressraum
* (Optional) BGP-Attribute (IP-Adresse und AS-Nummer des BGP-Peers)

Erstellen Sie mit dem Befehl [New-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermlocalnetworkgateway?view=azurermps-6.8.1) ein Gateway des lokalen Netzwerks.

```azurepowershell-interactive
New-AzureRmLocalNetworkGateway -Name $LNG1 -ResourceGroupName $RG1 `
  -Location 'East US' -GatewayIpAddress $LNGIP1 -AddressPrefix $LNGprefix1,$LNGprefix2
```

## <a name="create-a-s2s-vpn-connection"></a>Erstellen einer S2S-VPN-Verbindung

Erstellen Sie als Nächstes mit [New-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworkgatewayconnection?view=azurermps-6.8.1) eine Site-to-Site-VPN-Verbindung zwischen dem Gateway Ihres virtuellen Netzwerks und Ihrem VPN-Gerät. Beachten Sie, dass „-ConnectionType“ für „Site-to-Site-VPN“ *IPsec* lautet.

```azurepowershell-interactive
$vng1 = Get-AzureRmVirtualNetworkGateway -Name $GW1  -ResourceGroupName $RG1
$lng1 = Get-AzureRmLocalNetworkGateway   -Name $LNG1 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection1 -ResourceGroupName $RG1 `
  -Location $Location1 -VirtualNetworkGateway1 $vng1 -LocalNetworkGateway2 $lng1 `
  -ConnectionType IPsec -SharedKey "Azure@!b2C3"
```

Wenn Sie BGP verwenden, fügen Sie die optionale Eigenschaft **-EnableBGP $True** hinzu, um BGP für die Verbindung zu aktivieren. Diese Einstellung ist standardmäßig deaktiviert.

## <a name="update-the-vpn-connection-pre-shared-key-bgp-and-ipsecike-policy"></a>Aktualisieren der VPN-Verbindung: vorinstallierter Schlüssel, BGP und IPsec/IKE-Richtlinie

### <a name="view-and-update-your-pre-shared-key"></a>Anzeigen und Aktualisieren des vorinstallierten Schlüssels

Die Azure-S2S-VPN-Verbindung verwendet einen vorinstallierten Schlüssel (Geheimnis), um zwischen Ihrer lokalen VPN-Gerät und dem Azure-VPN-Gateway zu authentifizieren. Sie können den vorinstallierten Schlüssel für eine Verbindung mit [Get-AzureRmVirtualNetworkGatewayConnectionSharedKey](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworkgatewayconnectionsharedkey?view=azurermps-6.8.1) und [Set-AzureRmVirtualNetworkGatewayConnectionSharedKey](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgatewayconnectionsharedkey?view=azurermps-6.8.1) anzeigen bzw. aktualisieren.

> [!IMPORTANT]
> Der vorinstallierte Schlüssel ist eine Zeichenfolge mit max. 128 **druckbaren ASCII-Zeichen**.

Dieser Befehl zeigt den vorinstallierten Schlüssel für die Verbindung an:

```azurepowershell-interactive
Get-AzureRmVirtualNetworkGatewayConnectionSharedKey `
  -Name $Connection1 -ResourceGroupName $RG1
```

Die Ausgabe lautet **Azure@!b2C3** entsprechend des oben angeführten Beispiels. Verwenden Sie den folgenden Befehl, um den Wert des vorinstallierten Schlüssels in **Azure@!_b2=C3** zu ändern:

```azurepowershell-interactive
Set-AzureRmVirtualNetworkGatewayConnectionSharedKey `
  -Name $Connection1 -ResourceGroupName $RG1 `
  -Value "Azure@!_b2=C3"
```

### <a name="enable-bgp-on-vpn-connection"></a>Aktivieren von BGP für VPN-Verbindungen

Das Azure-VPN-Gateway unterstützt das dynamische BGP-Routingprotokoll. Sie können BGP für jede einzelne Verbindung aktivieren, abhängig davon, ob Sie BGP in Ihren lokalen Netzwerken und Geräten verwenden. Geben Sie die folgenden BGP-Eigenschaften an, bevor Sie BGP für die Verbindung aktivieren:

* ASN (Autonomous System Number) des Azure-VPN
* ASN des Gateways des lokalen Netzwerks
* BGP-Peer-IP-Adresse des Gateways des lokalen Netzwerks

Falls Sie die BGP-Eigenschaften nicht konfiguriert haben, fügen Sie Ihrem VPN-Gateway und dem Gateway des lokalen Netzwerks diese Eigenschaften mithilfe folgender Befehle hinzu: [Set-AzureRmVirtualNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgateway?view=azurermps-6.8.1) und [ Set-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermlocalnetworkgateway?view=azurermps-6.8.1).

```azurepowershell-interactive
$vng1 = Get-AzureRmVirtualNetworkGateway -Name $GW1  -ResourceGroupName $RG1
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $vng1 -Asn $VNet1ASN

$lng1 = Get-AzureRmLocalNetworkGateway   -Name $LNG1 -ResourceGroupName $RG1
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $lng1 `
  -Asn $LNGASN1 -BgpPeeringAddress $BGPPeerIP1
```

Aktivieren Sie BGP mit [Set-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgatewayconnection?view=azurermps-6.8.1).

```azurepowershell-interactive
$connection = Get-AzureRmVirtualNetworkGatewayConnection `
  -Name $Connection1 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection `
  -EnableBGP $True
```

Deaktivieren Sie BGP, indem Sie den Eigenschaftswert „-EnableBGP“ in **$False** ändern. Ausführlichere Erläuterungen zu BGP für Azure-VPN-Gateways finden Sie unter [Konfigurieren von BGP für Azure VPN Gateways mithilfe von PowerShell](vpn-gateway-bgp-resource-manager-ps.md).

### <a name="apply-a-custom-ipsecike-policy-on-the-connection"></a>Anwenden einer benutzerdefinierten IPsec-/IKE-Richtlinie für die Verbindung

Sie können eine optionale IPsec-/IKE-Richtlinie anwenden, um die exakte Kombination aus kryptografischen IPsec-/IKE-Algorithmen und Schlüssellängen für die Verbindung anzugeben, statt die [Standardvorschläge](vpn-gateway-about-vpn-devices.md#ipsec) zu verwenden. Im folgenden Beispielskript wird eine andere IPsec/IKE-Richtlinie mit den folgenden Algorithmen und Parametern erstellt:

* IKEv2: AES256, SHA256, DHGroup14
* IPsec: AES128, SHA1, PFS14, Lebensdauer der Sicherheitszuordnung: 14.400 Sekunden und 102.400.000 KB

```azurepowershell-interactive
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection1 `
                -ResourceGroupName $RG1
$newpolicy  = New-AzureRmIpsecPolicy `
                -IkeEncryption AES256 -IkeIntegrity SHA256 -DhGroup DHGroup14 `
                -IpsecEncryption AES128 -IpsecIntegrity SHA1 -PfsGroup PFS2048 `
                -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection `
  -IpsecPolicies $newpolicy
```

Eine vollständige Liste von Algorithmen und Anweisungen finden Sie unter [Konfigurieren der IPsec/IKE-Richtlinie für S2S-VPN- oder VNET-zu-VNET-Verbindungen](vpn-gateway-ipsecikepolicy-rm-powershell.md).

## <a name="add-another-s2s-vpn-connection"></a>Hinzufügen einer weiteren S2S-VPN-Verbindung

Um eine weitere S2S-VPN-Verbindung zum gleichen VPN-Gateway hinzuzufügen, erstellen Sie ein weiteres Gateway des lokalen Netzwerks und dann eine neue Verbindung zwischen dem neuen Gateway des lokalen Netzwerks und dem VPN-Gateway. Folgen Sie dem Beispiel in diesem Artikel.

```azurepowershell-interactive
# On-premises network
$LNG2        = "VPNsite2"
$Location2   = "West US"
$LNGprefix21 = "10.102.0.0/24"
$LNGprefix22 = "10.102.1.0/24"
$LNGIP2      = "YourDevicePublicIP"
$Connection2 = "VNet1ToSite2"

New-AzureRmLocalNetworkGateway -Name $LNG2 -ResourceGroupName $RG1 `
  -Location $Location2 -GatewayIpAddress $LNGIP2 -AddressPrefix $LNGprefix21,$LNGprefix22

$vng1 = Get-AzureRmVirtualNetworkGateway -Name $GW1  -ResourceGroupName $RG1
$lng2 = Get-AzureRmLocalNetworkGateway   -Name $LNG2 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection2 -ResourceGroupName $RG1 `
  -Location $Location1 -VirtualNetworkGateway1 $vng1 -LocalNetworkGateway2 $lng2 `
  -ConnectionType IPsec -SharedKey "AzureA1%b2_C3+"
```

Nun bestehen zwei S2S-VPN-Verbindungen mit Ihrem Azure-VPN-Gateway.

![VPN-Verbindungen zwischen mehreren Standorten](./media/vpn-gateway-tutorial-vpnconnection-powershell/multisite-connections.png)

## <a name="delete-a-s2s-vpn-connection"></a>Löschen einer S2S-VPN-Verbindung

Löschen Sie eine S2S-VPN-Verbindung mit [Remove-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermvirtualnetworkgatewayconnection?view=azurermps-6.8.1).

```azurepowershell-interactive
Remove-AzureRmVirtualNetworkGatewayConnection -Name $Connection2 -ResourceGroupName $RG1
```

Löschen Sie das Gateway des lokalen Netzwerks, wenn Sie es nicht mehr benötigen. Ein Gateway des lokalen Netzwerks kann nicht gelöscht werden, wenn diesem andere Verbindungen zugeordnet sind.

```azurepowershell-interactive
Remove-AzureRmVirtualNetworkGatewayConnection -Name $LNG2 -ResourceGroupName $RG1
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie S2S-VPN-Verbindungen erstellt und verwaltet werden, wie etwa Folgende:

> [!div class="checklist"]
> * Erstellen einer S2S-VPN-Verbindung
> * Aktualisieren der Verbindungseigenschaft: vorinstallierter Schlüssel, BGP, IPsec-/IKE-Richtlinie
> * Hinzufügen weiterer VPN-Verbindungen
> * Löschen einer VPN-Verbindung

Fahren Sie mit den folgenden Tutorials fort, um sich über S2S-, VNET-zu-VNET- und P2S-Verbindungen zu informieren.

> [!div class="nextstepaction"]
> * [Erstellen von VNET-zu-VNET-Verbindungen](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [Erstellen von P2S-Verbindungen](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
