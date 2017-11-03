---
title: "Azure CLI skrypt przykładowy — Tworzenie konta usługi partia zadań | Dokumentacja firmy Microsoft"
description: "Azure CLI skrypt przykładowy — Tworzenie konta usługi partia zadań"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: fd2f4682a04c557b69bbfce115f41c54a96d462c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-batch-account-with-the-azure-cli"></a>Tworzenie konta usługi partia zadań z wiersza polecenia platformy Azure

Ten skrypt tworzy konto partii zadań Azure i pokazuje, jak różne właściwości konta można zbadać i aktualizacji.

## <a name="prerequisites"></a>Wymagania wstępne

Zainstaluj interfejs wiersza polecenia platformy Azure przy użyciu instrukcji w [Przewodnik instalacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/install-azure-cli), jeśli nie ma jeszcze zrobione.

## <a name="batch-account-sample-script"></a>Skrypt przykładowy konta

Podczas tworzenia konta usługi partia zadań domyślnie jego węzły obliczeniowe są przypisywane wewnętrznie przez usługi partia zadań. Węzły obliczeniowe przydzielone podlegają przydziału rdzeni oddzielne i konta mogą być uwierzytelniane za pośrednictwem klucza wspólnego poświadczeń lub Azure Active Dirctory token.

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]

## <a name="batch-account-using-user-subscription-sample-script"></a>Przy użyciu skryptu próbki subskrypcji użytkownika konta usługi partia zadań

Można również zdecydować się na ma partii utworzyć jego węzłów obliczeniowych w ramach własnej subskrypcji platformy Azure.
Konta, które przydzielić obliczeniowe węzłów w ramach subskrypcji, musi zostać uwierzytelniony za pośrednictwem tokenu programu Azure Active Directory i węzły obliczeniowe przydzielone są zliczane limitu przydziału subskrypcji. Aby utworzyć konto w tym trybie, jeden Określ odwołanie magazynu kluczy podczas tworzenia konta.

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "Create Account using User Subscription")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia

Po uruchomieniu dowolnego z powyższych przykładowe skrypty, uruchom następujące polecenie, aby usunąć grupę zasobów i wszystkie powiązane zasoby (w tym konta wsadowego, konta usługi Azure Storage i Azure magazynów kluczy).

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń w celu utworzenia grupy zasobów konta usługi partia zadań i wszystkie powiązane zasoby. Każde polecenie w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Tworzenie konta usługi partia zadań az](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_create) | Tworzy konto usługi partia zadań.  |
| [Ustaw konto wsadowe az](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_set) | Aktualizuje właściwości konta usługi partia zadań.  |
| [Pokaż konto wsadowe az](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_show) | Pobiera szczegóły określonego konta usługi partia zadań.  |
| [Lista kluczy konta zadań wsadowych az](https://docs.microsoft.com/cli/azure/batch/account/keys#az_batch_account_keys_list) | Pobiera klucze dostępu do określonego konta usługi partia zadań.  |
| [AZ logowanie się na koncie usługi partia zadań](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Uwierzytelnia określony konta usługi partia zadań dla dalszego interakcja interfejsu wiersza polecenia.  |
| [Tworzenie konta magazynu az](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Tworzy konto magazynu. |
| [Utwórz az keyvault](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_create) | Tworzy magazyn kluczy. |
| [keyvault az set-policy.](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_set_policy) | Aktualizacja zasad zabezpieczeń określonego magazynu kluczy. |
| [Usuwanie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe przykłady skryptów partii interfejsu wiersza polecenia można znaleźć w [dokumentacji interfejsu wiersza polecenia Azure partii](../batch-cli-samples.md).
