---
title: "Usługa Azure sieci szkieletowej interfejsu wiersza polecenia skryptu wdrażania próbki"
description: "Wdrażanie aplikacji do klastra usługi sieć szkieletowa usług Azure przy użyciu interfejsu wiersza polecenia Azure Service Fabric"
services: service-fabric
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 08/22/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: 2bbf689820a92cfa01b26fdbacb4526ade8956ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Wdrażanie aplikacji do klastra sieci szkieletowej usług

Ten przykładowy skrypt umożliwia skopiowanie pakietu aplikacji do magazynu obrazu klastra, rejestruje typ aplikacji w klastrze i tworzy wystąpienie aplikacji na podstawie typu aplikacji. Wszystkie usługi domyślne są również tworzone w tym momencie.

Jeśli to konieczne, zainstaluj [interfejsu wiersza polecenia usługi sieć szkieletowa](../service-fabric-cli.md).

## <a name="sample-script"></a>Przykładowy skrypt

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia

Po zakończeniu [Usuń](cli-remove-application.md) skryptu można użyć, aby usunąć aplikację. Skrypt Usuń Usuwa wystąpienie aplikacji, wyrejestrowuje typ aplikacji i usuwa pakiet aplikacji w sklepie obrazu.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia usługi sieć szkieletowa](../service-fabric-cli.md).

Dodatkowe przykłady interfejsu wiersza polecenia usługi sieci szkieletowej dla sieci szkieletowej usług Azure można znaleźć w [przykłady interfejsu wiersza polecenia usługi sieć szkieletowa](../samples-cli.md).
