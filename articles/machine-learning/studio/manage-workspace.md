---
title: Verwalten eines Machine Learning Studio-Arbeitsbereichs und Verbindungsaufbau
titleSuffix: Azure Machine Learning Studio
description: Verwalten des Zugriffs auf Azure Machine Learning-Arbeitsbereiche sowie Bereitstellen und Verwalten von ML-API-Webdiensten
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 02/27/2017
ms.openlocfilehash: f1904552b6c43cadd611659ed544b9f4bafcb465
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55487273"
---
# <a name="manage-an-azure-machine-learning-studio-workspace"></a>Verwalten eines Azure Machine Learning Studio-Arbeitsbereichs

> [!NOTE]
> Informationen zum Verwalten von Webdiensten im Machine Learning Web Services-Portal finden Sie unter [Verwalten eines Webdiensts im Azure Machine Learning Web Services-Portal](manage-new-webservice.md).
> 
> 

Sie können Machine Learning-Arbeitsbereiche im Azure-Portal verwalten.



## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

So verwalten Sie einen Arbeitsbereich im Azure-Portal

1. Melden Sie sich über das Administratorkonto eines Azure-Abonnements beim [Azure-Portal](https://portal.azure.com/) an.
2. Geben Sie im Suchfeld oben auf der Seite „Machine Learning-Arbeitsbereiche“ ein, und wählen Sie dann **Machine Learning-Arbeitsbereiche** aus.
3. Klicken Sie auf den Arbeitsbereich, den Sie verwalten möchten.

Neben den Standardinformationen und -optionen zur Ressourcenverwaltung stehen folgende Optionen zur Verfügung:

- **Eigenschaften anzeigen:** Auf dieser Seite werden Informationen zum Arbeitsbereich und der Ressource angezeigt. Zudem können Sie hier das Abonnement und die Ressourcengruppe ändern, mit denen der Arbeitsbereich verbunden ist.
- **Speicherschlüssel neu synchronisieren:** Im Arbeitsbereich werden die Schlüssel für das Speicherkonto verwaltet. Wenn sich die Schlüssel für das Speicherkonto ändern, können Sie auf **Schlüssel neu synchronisieren** klicken, um die Schlüssel mit dem Arbeitsbereich zu synchronisieren.

Verwenden Sie zum Verwalten der dem Arbeitsbereich zugeordneten Webdienste das Portal für Machine Learning-Webdienste. Ausführliche Informationen finden Sie unter [Verwalten eines Webdiensts im Azure Machine Learning Web Services-Portal](manage-new-webservice.md).

> [!NOTE]
> Zum Bereitstellen oder Verwalten neuer Webdienste muss Ihnen in dem Abonnement, für das der Webdienst bereitgestellt wird, eine Teilnehmer- oder Administratorrolle zugewiesen sein. Wenn Sie andere Benutzer in einen Machine Learning-Arbeitsbereich einladen, müssen Sie ihnen eine Teilnehmer- oder Administratorrolle im Abonnement zuweisen, damit sie Webdienste bereitstellen oder verwalten können. 
> 
>Weitere Informationen zum Festlegen von Zugriffsberechtigungen finden Sie unter [Verwalten des Zugriffs mit RBAC und dem Azure-Portal](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über das [Bereitstellen von Machine Learning mithilfe von Azure Resource Manager-Vorlagen](deploy-with-resource-manager-template.md). 
