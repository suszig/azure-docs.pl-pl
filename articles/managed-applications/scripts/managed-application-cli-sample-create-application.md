---
title: "Azure CLI skrypt przykładowy — wdrażanie zarządzanych aplikacji | Dokumentacja firmy Microsoft"
description: "Azure CLI skrypt przykładowy — wdrażanie definicji zarządzanej aplikacji"
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: 62d0247df3b3d9f242877e4ea27ccc871cf797c0
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2017
---
# <a name="deploy-a-managed-application-for-service-catalog-with-azure-cli"></a>Wdrażanie zarządzanych aplikacji katalogu usług z wiersza polecenia platformy Azure

Ten skrypt wdraża definicję zarządzanych aplikacji z katalogu usług. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli[main](../../../cli_scripts/managed-applications/create-application/create-application.sh "Create application")]


## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujące polecenie do wdrażania aplikacji zarządzanych. Każde polecenie w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [Utwórz az managedapp](https://docs.microsoft.com/cli/azure/managedapp#az_managedapp_create) | Utwórz zarządzaną aplikację. Podaj identyfikator definicji i parametrów szablonu. |


## <a name="next-steps"></a>Następne kroki

* Aby obejrzeć wprowadzenie do aplikacji zarządzanych, zobacz [zarządzanej aplikacji Azure — omówienie](../overview.md).
* Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).
