---
title: "Azure CLI przykładowym skrypcie - Get nazwa hosta, portów i kluczy dla pamięci podręcznej Redis Azure | Dokumentacja firmy Microsoft"
description: "Azure CLI przykładowym skrypcie - Get nazwa hosta, portów i klucze wystąpienia pamięci podręcznej Redis Azure"
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 6cfff74ed037835abb6d3e3975fe33d71b9730a3
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="get-the-hostname-ports-and-keys-for-azure-redis-cache"></a>Pobierz nazwę hosta, portów i klucze dla pamięci podręcznej Redis Azure

W tym scenariuszu należy dowiedzieć się, jak można pobrać nazwy hosta, portów i klucze służące do nawiązywania połączenia z wystąpieniem usługi pamięć podręczna Redis Azure.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt można pobrać nazwy hosta, kluczy i porty wystąpienia pamięci podręcznej Redis Azure korzysta z następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Pokaż redis az](https://docs.microsoft.com/cli/azure/redis#az_redis_show) | Pobieranie szczegółów wystąpienia pamięci podręcznej Redis Azure. |
| [klucze listy az redis](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys) | Pobrać klucze dostępu dla wystąpienia pamięci podręcznej Redis Azure. |


## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe przykłady skryptów w pamięci podręcznej Redis Azure CLI znajdują się w [dokumentacji pamięć podręczna Redis Azure](../cli-samples.md).