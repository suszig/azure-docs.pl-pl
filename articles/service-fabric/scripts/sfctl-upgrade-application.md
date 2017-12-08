---
title: "Przykład skryptu interfejsu wiersza polecenia usługi sieci szkieletowej — aktualizacja aplikacji w klastrze"
description: "Usługa sieci szkieletowej interfejsu wiersza polecenia skryptu próbki — aktualizacja aplikacji za pomocą nowej wersji. W tym przykładzie powoduje także uaktualnienie wdrożonej aplikacji z nowej usługi bits."
services: service-fabric
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: adegeo
ms.custom: 
ms.openlocfilehash: f91ea8c2f8c6e9a6ce38cf72be2c61d9e5218b23
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Dodaj certyfikat aplikacji do klastra sieci szkieletowej usług

Ten przykładowy skrypt przekazuje nową wersję istniejącej aplikacji, a następnie uaktualnia wdrożonej aplikacji z nowej usługi bits.

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-sh[main](../../../cli_scripts/service-fabric/upgrade-application/upgrade-application.sh "Upload and update an application on a Service Fabric cluster")]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia usługi sieć szkieletowa](../service-fabric-cli.md).

Dodatkowe przykłady interfejsu wiersza polecenia usługi sieci szkieletowej dla sieci szkieletowej usług Azure można znaleźć w [przykłady interfejsu wiersza polecenia usługi sieć szkieletowa](../samples-cli.md).
