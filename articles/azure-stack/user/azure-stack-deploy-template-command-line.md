---
title: "Wdrażanie szablonów przy użyciu wiersza polecenia Azure stosu | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak użyć interfejsu wiersza polecenia (CLI) i platform do wdrażania szablonów Azure stosu."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: 9584177f-4af3-4834-864d-930b09ae0995
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: d58d80d89bb2544c4d4a34f608177a96760406ba
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Wdrażanie szablonów w stosie Azure przy użyciu wiersza polecenia

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Użyj wiersza polecenia do wdrażania usługi Azure Resource Manager szablonów w zestawie Azure stosu. Szablony usługi Azure Resource Manager wdrażania i obsługi administracyjnej wszystkie zasoby aplikacji w jednej, skoordynowanej operacji.

## <a name="before-you-begin"></a>Przed rozpoczęciem
 - [Zainstaluj i połącz](azure-stack-connect-cli.md) stos Azure z wiersza polecenia platformy Azure
 - Pobierz pliki *azuredeploy.json* i *azuredeploy.parameters.json* z [Utwórz szablon przykład konta magazynu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).
 
## <a name="deploy-template"></a>Wdrażanie szablonu
Przejdź do folderu, w którym te pliki zostały pobrane i uruchom następujące polecenie, aby wdrożyć szablon:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

To polecenie wdraża szablonu w grupie zasobów **cliRG** w lokalizacji domyślnej POC stosu Azure.

## <a name="validate-template-deployment"></a>Weryfikowanie szablonu wdrożenia
Aby wyświetlić ten zasób grupy i konto magazynu, użyj następujących poleceń:

    azure group list

    azure storage account list




