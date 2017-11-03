---
title: "Azure CLI przykładowym skrypcie - uruchamiania zadania z instancją | Dokumentacja firmy Microsoft"
description: "Azure CLI przykładowym skrypcie - uruchamiania zadania z partii"
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
ms.openlocfilehash: 73d93622d418359be421e043d0af4e4befc6f4b4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="running-jobs-on-azure-batch-with-azure-cli"></a>Uruchomione zadania w partii zadań Azure z wiersza polecenia platformy Azure

Ten skrypt tworzy zadanie wsadowe i dodaje sekwencję zadań do zadania. On również pokazano, jak monitorować zadania i jego zadań podrzędnych. Na koniec widoczny jest sposób zapytania wydajnie uzyskać informacji o zadaniach zadania usługa partia zadań.

## <a name="prerequisites"></a>Wymagania wstępne

- Zainstaluj interfejs wiersza polecenia platformy Azure przy użyciu instrukcji w [Przewodnik instalacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/install-azure-cli), jeśli nie ma jeszcze zrobione.
- Tworzenie konta usługi partia zadań, jeśli nie został wcześniej. Zobacz [Tworzenie konta usługi partia zadań z wiersza polecenia platformy Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) przykładowego skryptu, który tworzy konto.
- Skonfigurować aplikację do uruchamiania z zadania uruchamiania, jeśli nie zostało to jeszcze zrobione. Zobacz [Dodawanie aplikacji do partii zadań Azure z wiersza polecenia platformy Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) przykładowego skryptu, który tworzy aplikację i przekazuje pakietu aplikacji do platformy Azure.
- Skonfiguruj pulę, w którym zadanie zostanie uruchomione. Zobacz [pule Zarządzanie partii zadań Azure z wiersza polecenia platformy Azure](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) przykładowego skryptu, który powoduje utworzenie puli z konfiguracji usługi chmury lub konfiguracji maszyny wirtualnej.

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]

## <a name="clean-up-job"></a>Oczyszczanie zadania

Po uruchomieniu powyższy skrypt przykładowy należy uruchomić następujące polecenie, aby usunąć zadania i wszystkie jego zadań podrzędnych. Należy pamiętać, że pula będzie trzeba usunąć oddzielnie. Zobacz [pule Zarządzanie partii zadań Azure z wiersza polecenia platformy Azure](./batch-cli-sample-manage-pool.md) Aby uzyskać więcej informacji na temat tworzenia i usuwania pule.

```azurecli
az batch job delete --job-id myjob
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do utworzenia zadania wsadowego i zadania. Każde polecenie w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [AZ logowanie się na koncie usługi partia zadań](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Uwierzytelnić konta usługi partia zadań.  |
| [Utwórz zadanie wsadowe az](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_create) | Tworzy zadanie wsadowe.  |
| [Ustaw zadanie wsadowe az](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_set) | Aktualizuje właściwości zadania wsadowego.  |
| [Pokaż zadania wsadowego az](https://docs.microsoft.com/cli/azure/batch/job#az_batch_job_show) | Pobiera szczegóły określone zadanie wsadowe.  |
| [Utwórz zadanie wsadowe az](https://docs.microsoft.com/cli/azure/batch/task#az_batch_task_create) | Dodaje zadanie do określonego zadania wsadowego.  |
| [Pokaż zadania wsadowego az](https://docs.microsoft.com/cli/azure/batch/task#az_batch_task_show) | Pobiera szczegóły zadania z określonego zadania wsadowego.  |
| [Lista zadań wsadowych az](https://docs.microsoft.com/cli/azure/batch/task#az_batch_task_list) | Zawiera listę zadań skojarzonych z określonego zadania.  |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe przykłady skryptów partii interfejsu wiersza polecenia można znaleźć w [dokumentacji interfejsu wiersza polecenia Azure partii](../batch-cli-samples.md).
