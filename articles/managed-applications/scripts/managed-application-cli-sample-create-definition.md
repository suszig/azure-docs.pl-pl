---
title: "Azure CLI skrypt przykładowy — Tworzenie definicji aplikacji zarządzanej | Dokumentacja firmy Microsoft"
description: "Azure CLI skrypt przykładowy — Tworzenie definicji aplikacji zarządzanej"
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
ms.openlocfilehash: 6cfc51e5787ad6ab100638d0965b69232cda070a
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2017
---
# <a name="create-a-managed-application-definition-with-azure-cli"></a>Utwórz definicję zarządzanej aplikacji z wiersza polecenia platformy Azure

Ten skrypt publikuje definicję aplikacji zarządzanych do katalogu usług. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli[main](../../../cli_scripts/managed-applications/create-definition/create-definition.sh "Create definition")]


## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następującego polecenia do tworzenia definicji zarządzanej aplikacji. Każde polecenie w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [Utworzenie definicji managedapp az](https://docs.microsoft.com/cli/azure/managedapp/definition#az_managedapp_definition_create) | Utwórz definicję zarządzanej aplikacji. Podaj pakiet, który zawiera pliki wymagane. |


## <a name="next-steps"></a>Następne kroki

* Aby obejrzeć wprowadzenie do aplikacji zarządzanych, zobacz [zarządzanej aplikacji Azure — omówienie](../overview.md).
* Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).
