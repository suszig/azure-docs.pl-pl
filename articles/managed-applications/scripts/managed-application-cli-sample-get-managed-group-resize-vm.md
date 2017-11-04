---
title: "Azure CLI skrypt przykładowy — Pobierz grupę zasobów zarządzanych i zmień rozmiar maszyn wirtualnych | Dokumentacja firmy Microsoft"
description: "Azure CLI przykładowym skrypcie — Pobierz grupę zasobów zarządzanych i zmień rozmiar maszyny wirtualne"
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
ms.openlocfilehash: c78d2646471e40d60972cf91cb5bbd351f71a66c
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2017
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-azure-cli"></a>Pobieranie zasobów w grupie zasobów zarządzanych, a następnie zmień rozmiar maszyny wirtualne z wiersza polecenia platformy Azure

Ten skrypt pobiera zasoby w grupie zasobów zarządzanych i zmienia rozmiar maszyn wirtualnych w danej grupie zasobów.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli[main](../../../cli_scripts/managed-applications/get-application/get-application.sh "Get application")]


## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do wdrażania aplikacji zarządzanych. Każde polecenie w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [Lista managedapp az](https://docs.microsoft.com/cli/azure/managedapp#az_managedapp_list) | Wyświetl listę zarządzanych aplikacji. Podaj wartości zapytania, aby skoncentrować się wyniki. |
| [Lista zasobów az](https://docs.microsoft.com/cli/azure/resource#az_resource_list) | Lista zasobów. Podaj zasób wartości grupy i zapytania, aby skoncentrować się wynik. |
| [Zmień rozmiar maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm#az_vm_resize) | Zaktualizować rozmiaru maszyny wirtualnej. |


## <a name="next-steps"></a>Następne kroki

* Aby obejrzeć wprowadzenie do aplikacji zarządzanych, zobacz [zarządzanej aplikacji Azure — omówienie](../overview.md).
* Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).
