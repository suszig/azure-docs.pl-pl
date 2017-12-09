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
ms.date: 12/06/2017
ms.author: adegeo
ms.custom: 
ms.openlocfilehash: 059022a7f447fe2b0decaacf44ef7bc2e9a241d1
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Dodaj certyfikat aplikacji do klastra sieci szkieletowej usług

Ten przykładowy skrypt przekazuje nową wersję istniejącej aplikacji, a następnie uaktualnia wdrożonej aplikacji z nowej usługi bits.

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-sh[main](../../../cli_scripts/service-fabric/upgrade-application/upgrade-application.sh "Upload and update an application on a Service Fabric cluster")]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia usługi sieć szkieletowa](../service-fabric-cli.md).

Dodatkowe przykłady interfejsu wiersza polecenia usługi sieci szkieletowej dla sieci szkieletowej usług Azure można znaleźć w [przykłady interfejsu wiersza polecenia usługi sieć szkieletowa](../samples-cli.md).
