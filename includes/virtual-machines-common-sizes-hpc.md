---
title: Includedatei
description: Includedatei
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 07/06/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 16e2a9cfbd9f08428fddade290117b27bc3401f7
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44369570"
---
Virtuelle Azure-Computer der H-Reihe sind High Performing Computing-VMs für Workloads wie Stapelverarbeitung, Analyse, Molekülmodellierung und Strömungsdynamik. Diese virtuellen Computer mit 8 und 16 vCPUs basieren auf der Intel Haswell E5-2667 V3-Prozessortechnologie mit DDR4-Arbeitsspeicher und SSD-basiertem temporärem Speicher. 

Neben beträchtlicher CPU-Leistung bietet die H-Serie verschiedene Optionen für RDMA-Netzwerke mit niedriger Latenz unter Verwendung von FDR InfiniBand sowie verschiedene Speicherkonfigurationen für Berechnungsanforderungen mit hohem Speicherbedarf.



## <a name="h-series"></a>H-Reihe

ACU: 290 - 300

Storage Premium: nicht unterstützt

Storage Premium-Zwischenspeicherung: nicht unterstützt

| Größe | vCPU | Arbeitsspeicher: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Max. Datenträger-Datendurchsatz: IOPS | Maximale Anzahl NICs |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |32 |32 x 500 |2  |
| Standard_H16 |16 |112 |2000 |64 |64 x 500 |4 |
| Standard_H8m |8 |112 |1000 |32 |32 x 500 |2  |
| Standard_H16m |16 |224 |2000 |64 |64 x 500 |4  |
| Standard_H16r <sup>1</sup> |16 |112 |2000 |64 |64 x 500 |4  |
| Standard_H16mr <sup>1</sup> |16 |224 |2000 |64 |64 x 500 |4 |

<sup>1</sup> Für MPI-Anwendungen ist ein dediziertes RDMA-Back-End-Netzwerk durch ein FDR InfiniBand-Netzwerk aktiviert, das eine äußerst geringe Latenz und eine hohe Bandbreite ermöglicht.

<br>






