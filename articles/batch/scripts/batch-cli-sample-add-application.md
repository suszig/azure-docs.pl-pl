---
title: "Azure CLI skrypt przykładowy — Dodawanie aplikacji w partii | Dokumentacja firmy Microsoft"
description: "Azure CLI skrypt przykładowy — Dodawanie aplikacji w partii"
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
ms.openlocfilehash: cbfe8ab565ecf7f298a9a6c0f0c8298c675f178c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="adding-applications-to-azure-batch-with-azure-cli"></a>Dodawanie aplikacji do partii zadań Azure z wiersza polecenia platformy Azure

Ten skrypt pokazuje, jak skonfigurować aplikację do użycia z puli partii zadań Azure lub zadania. Aby skonfigurować aplikację, pakiet pliku wykonywalnego, oraz wszelkie zależności, do pliku zip. W tym przykładzie nosi nazwę pliku wykonywalnego zip "Mój-aplikacji-exe.zip".

## <a name="prerequisites"></a>Wymagania wstępne

- Zainstaluj interfejs wiersza polecenia platformy Azure przy użyciu instrukcji w [Przewodnik instalacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/install-azure-cli), jeśli nie ma jeszcze zrobione.
- Tworzenie konta usługi partia zadań, jeśli nie został wcześniej. Zobacz [Tworzenie konta usługi partia zadań z wiersza polecenia platformy Azure](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) przykładowego skryptu, który tworzy konto.

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-application"></a>Oczyszczanie aplikacji

Po uruchomieniu powyższy skrypt przykładowy należy uruchomić następujące polecenia, aby usunąć aplikację i wszystkie jego pakietów przekazano aplikacji.

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do tworzenia aplikacji i przekaż pakiet aplikacji.
Każde polecenie w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie aplikacji partii az](https://docs.microsoft.com/cli/azure/batch/application#az_batch_application_create) | Tworzy aplikację.  |
| [zestaw aplikacji partii az](https://docs.microsoft.com/cli/azure/batch/application#az_batch_application_set) | Aktualizuje właściwości aplikacji.  |
| [Utwórz pakiet aplikacji partii az](https://docs.microsoft.com/cli/azure/batch/application/package#az_batch_application_package_create) | Dodaje pakiet aplikacji dla określonej aplikacji.  |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe przykłady skryptów partii interfejsu wiersza polecenia można znaleźć w [dokumentacji interfejsu wiersza polecenia Azure partii](../batch-cli-samples.md).
