---
title: "Usługa Azure sieci szkieletowej interfejsu wiersza polecenia skryptu Remove próbki"
description: "Usuń aplikację z klastra usługi sieć szkieletowa usług Azure przy użyciu interfejsu wiersza polecenia Azure Service Fabric"
services: service-fabric
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: 95a6bac7ee79e3490584305e78d410104363b780
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Usuń aplikację z klastra sieci szkieletowej usług

Ten przykładowy skrypt usuwa uruchomione wystąpienie aplikacji sieci szkieletowej usług, wyrejestrowuje typ i wersja aplikacji z klastra.  Usuwanie wystąpienia aplikacji spowoduje również usunięcie wszystkich uruchomionych wystąpień skojarzoną z daną aplikacją usługi. Następnie pliki aplikacji są usuwane z magazynu obrazów. 

Jeśli to konieczne, zainstaluj [interfejsu wiersza polecenia usługi sieć szkieletowa](../service-fabric-cli.md).

## <a name="sample-script"></a>Przykładowy skrypt

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz [dokumentacji interfejsu wiersza polecenia usługi sieć szkieletowa](../service-fabric-cli.md).

Dodatkowe przykłady interfejsu wiersza polecenia usługi sieci szkieletowej dla sieci szkieletowej usług Azure można znaleźć w [przykłady interfejsu wiersza polecenia usługi sieć szkieletowa](../samples-cli.md).
