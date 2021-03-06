---
title: Häufig gestellte Fragen zu Azure Stack für Windows Server | Microsoft-Dokumentation
description: Liste von häufig gestellten Fragen zum Azure Stack-Marketplace für Windows Server
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2018
ms.author: sethm
ms.reviewer: avishwan
ms.lastreviewed: 11/12/2018
ms.openlocfilehash: 03a6f649f15f6a4905433d6e2ec292a901340929
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55249679"
---
# <a name="windows-server-in-azure-stack-marketplace-faq"></a>Häufig gestellte Fragen zum Azure Stack-Marketplace für Windows Server

In diesem Artikel werden einige häufig gestellte Fragen zu Windows Server-Images im [Azure Stack-Marketplace](azure-stack-marketplace.md) beantwortet.

## <a name="marketplace-items"></a>Marketplace-Elemente

### <a name="how-do-i-update-to-a-newer-windows-image"></a>Wie führe ich eine Aktualisierung auf ein neueres Windows-Image durch?

Finden Sie zunächst heraus, ob Azure Resource Manager-Vorlagen auf bestimmte Versionen verweisen. Wenn dies der Fall ist, aktualisieren Sie diese Vorlagen, oder behalten Sie die älteren Imageversionen bei. Empfohlen wird, die **Version mit der Bezeichnung „latest“** zu verwenden.

Falls Virtual Machine Scale Sets auf eine bestimmte Version verweist, sollten Sie dann überlegen, ob diese später skaliert werden, und entscheiden, ob Sie ältere Versionen beibehalten möchten. Wenn keine dieser Bedingungen zutrifft, löschen Sie vor dem Herunterladen der neueren Images die älteren Images im Marketplace. Verwenden Sie hierfür Marketplace Management, falls das ursprüngliche Image auf diese Weise heruntergeladen wurde. Laden Sie dann die neuere Version herunter.

### <a name="what-are-the-licensing-options-for-windows-server-marketplace-images-on-azure-stack"></a>Welche Lizenzierungsoptionen sind für Windows Server-Marketplace-Images in Azure Stack verfügbar?

Microsoft bietet zwei Versionen von Windows Server-Images über den Azure Stack-Marketplace an:

- **Nutzungsbasierte Bezahlung**: Diese Images führen die Windows-Verbrauchseinheiten zum vollen Preis aus. 
   Zielgruppe: Enterprise Agreement-Kunden (EA), die das *Nutzungsabrechnungsmodell* verwenden, sowie CSPs, die nicht die SPLA-Lizenzierung verwenden möchten.
- **BYOL (Bring-Your-Own-License, Verwendung Ihrer eigenen Lizenz)**: Diese Images führen grundlegende Verbrauchseinheiten aus.
   Zielgruppe: EA-Kunden mit einer Windows Server-Lizenz, und CSPs, die die SPLA-Lizenzierung verwenden.

Der Azure-Hybridnutzungsvorteil (Azure Hybrid Use Benefit, AHUB) wird in Azure Stack nicht unterstützt. Kunden, die ihre Lizenz über das „Kapazitätsmodell“ bezogen haben, müssen das BYOL-Image verwenden. Wenn Sie das Azure Stack Development Kit (ASDK) testen, können Sie eine dieser Optionen verwenden.

### <a name="what-if-i-downloaded-the-wrong-version-to-offer-my-tenantsusers"></a>Was geschieht, wenn ich die falsche Version für meine Mandanten/Benutzer heruntergeladen habe?

Löschen Sie die falsche Version zuerst über Marketplace Management. Warten Sie, bis der Vorgang abgeschlossen ist (achten Sie auf die Benachrichtigungen für den Abschluss, nicht auf das Blatt „Marketplace Management“). Laden Sie dann die richtige Version herunter.

### <a name="what-if-my-user-incorrectly-checked-the-i-have-a-license-box-in-previous-windows-builds-and-they-dont-have-a-license"></a>Was geschieht, wenn mein Benutzer fälschlicherweise das Kontrollkästchen „Ich besitze eine Lizenz“ in den vorherigen Windows-Builds aktiviert hat, obwohl er keine Lizenz besitzt?

Weitere Informationen finden Sie unter [Konvertieren von Windows Server-VMs mit Hybridvorteil zurück zur nutzungsbasierten Bezahlung](../virtual-machines/windows/hybrid-use-benefit-licensing.md#powershell-1).

### <a name="what-if-i-have-an-older-image-and-my-user-forgot-to-check-the-i-have-a-license-box-or-we-use-our-own-images-and-we-do-have-enterprise-agreement-entitlement"></a>Was geschieht, wenn wir ein älteres Image besitzen und mein Benutzer vergessen hat, das Kontrollkästchen „Ich besitze eine Lizenz“ zu aktivieren, oder wir unsere eigenen Images verwenden und über Enterprise Agreement-Berechtigung verfügen?

Weitere Informationen finden Sie unter [Konvertieren einer vorhandenen VM mit Azure-Hybridvorteil für Windows Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md#convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server). Beachten Sie, dass der Azure-Hybridvorteil zwar nicht für Azure Stack gilt, diese Einstellung sich jedoch auf diesen Dienst auswirkt.

### <a name="what-about-other-vms-that-use-windows-server-such-as-sql-or-machine-learning-server"></a>Was passiert mit anderen VMs, die Windows Server verwenden, z.B. SQL Server oder Machine Learning Server?

Diese Images wenden den **licenseType**-Parameter an, weshalb das nutzungsbasierte Modell angewendet wird. Sie können diesen Parameter festlegen (siehe hierzu die Antwort zur vorherigen häufig gestellten Frage). Dies gilt nur für die Windows Server-Software, nicht für sich überlagernde Produkte wie SQL, für das Bring Your Own License erforderlich ist. Die benutzerdefinierte Lizenzierung gilt nicht für sich überlagernde Softwareprodukte.

### <a name="i-have-an-enterprise-agreement-ea-and-will-be-using-my-ea-windows-server-license-how-do-i-make-sure-images-are-billed-correctly"></a>Ich besitze ein Enterprise Agreement (EA) und verwende meine Windows Server-EA-Lizenz. Wie stelle ich sicher, dass die Images ordnungsgemäß abgerechnet werden?

Sie können **licenseType: Windows_Server** in einer Azure Resource Manager-Vorlage hinzufügen. Diese Einstellung muss jedem Ressourcenblock eines virtuellen Computers hinzugefügt werden.

## <a name="activation"></a>Aktivierung

Um einen virtuellen Windows Server-Computer in Azure Stack zu aktivieren, müssen die folgenden Bedingungen erfüllt sein:

- Der OEM hat den entsprechenden BIOS-Marker in jedem Hostsystem in Azure Stack festgelegt.
- Windows Server 2012 R2 und Windows Server 2016 müssen die [automatische Aktivierung virtueller Computer](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)) verwenden. Der Schlüsselverwaltungsdienst (Key Management Service, KMS) und andere Aktivierungsdienste werden nicht in Azure Stack unterstützt.

### <a name="how-can-i-verify-that-my-virtual-machine-is-activated"></a>Wie kann ich sicherstellen, dass mein virtueller Computer aktiviert ist?

Führen Sie den folgenden Befehl in einer Eingabeaufforderung mit erhöhten Rechten aus: 

```shell
slmgr /dlv
``` 

Wenn er richtig aktiviert ist, wird dies klar und deutlich mit dem Hostnamen in der `slmgr`-Ausgabe angezeigt. Verlassen Sie sich nicht auf Wasserzeichen in der Anzeige, da sie möglicherweise nicht auf dem neuesten Stand sind oder von einem anderen hinter Ihrem geschalteten virtuellen Computer angezeigt werden.

### <a name="my-vm-is-not-set-up-to-use-avma-how-can-i-fix-it"></a>Meine VM ist nicht für die Verwendung von AVMA konfiguriert. Wie kann ich dies beheben?

Führen Sie den folgenden Befehl in einer Eingabeaufforderung mit erhöhten Rechten aus: 

```shell
slmgr /ipk <AVMA key> 
```

Die für das Image zu verwendenden Schlüssel werden im Artikel [Automatische Aktivierung virtueller Computer](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)) aufgeführt.

### <a name="i-create-my-own-windows-server-images-how-can-i-make-sure-they-use-avma"></a>Ich erstelle meine eigenen Windows Server-Images. Wie kann ich sicherstellen, dass diese AVMA verwenden?

Es wird empfohlen, die Befehlszeile `slmgr /ipk` mit dem entsprechenden Schlüssel auszuführen, bevor Sie den Befehl `sysprep` ausführen. Alternativ können Sie den AVMA-Schlüssel in einer Setupdatei namens „Unattend.exe“ einschließen.

### <a name="i-am-trying-to-use-my-windows-server-2016-image-created-on-azure-and-it-is-not-activating-or-using-kms-activation"></a>Ich möchte mein in Azure erstelltes Windows Server 2016-Image verwenden, doch es kann nicht aktiviert werden oder die KMS-Aktivierung funktioniert nicht.

Führen Sie den Befehl `slmgr /ipk` aus. Für Azure-Images wird das Fallback auf AVMA möglicherweise nicht richtig ausgeführt, aber wenn eine Verbindung mit dem Azure KMS-System hergestellt werden kann, werden sie aktiviert. Es wird empfohlen, sicherzustellen, dass diese VMs für die Verwendung von AVMA konfiguriert sind.

### <a name="i-have-performed-all-of-these-steps-but-my-virtual-machines-are-still-not-activating"></a>Ich habe all diese Schritte ausgeführt, aber meine virtuellen Computer sind noch nicht aktiviert.

Wenden Sie sich an Ihren Hardwareanbieter, um sicherzustellen, dass die richtigen BIOS-Marker installiert wurden.

### <a name="what-about-earlier-versions-of-windows-server"></a>Was ist mit früheren Versionen von Windows Server?

[Automatische Aktivierung virtueller Computer](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)) wird nicht älteren Versionen von Windows Server unterstützt. Sie müssen die VMs manuell aktivieren.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Der Azure Stack-Marketplace – Übersicht](azure-stack-marketplace.md)
- [Herunterladen von Marketplace-Elementen von Azure in Azure Stack](azure-stack-download-azure-marketplace-item.md)
