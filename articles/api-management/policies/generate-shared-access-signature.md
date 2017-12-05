---
title: "Azure próbce interfejsu API zarządzania zasad - Generowanie sygnatura dostępu współdzielonego | Dokumentacja firmy Microsoft"
description: "Przykładowe zasady zarządzania Azure API - pokazano, jak Generowanie sygnatura dostępu współdzielonego, za pomocą wyrażeń i przekazuje żądanie do magazynu Azure z identyfikatora uri ponownego zapisywania zasad."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: e1f17f9f4e17a3eebb55e4ec1905aec19a2165a5
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="generate-shared-access-signature"></a>Generowanie sygnatury dostępu współdzielonego

W tym artykule przedstawiono przykład zasad interfejsu API Azure zarządzania, który demonstruje sposób generowania [sygnatura dostępu współdzielonego](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-shared-access-signature-part-1) przy użyciu wyrażeń i przekazuje żądanie do magazynu Azure z identyfikatora uri ponownego zapisywania zasad. Aby ustawić lub zmodyfikować kod zasad, wykonaj czynności opisane w [zestawu lub Edytuj zasady](../set-edit-policies.md). Aby wyświetlić inne przykłady, zobacz [Przykłady zasad](../policy-samples.md).

## <a name="policy"></a>Zasady

Wklej kod do **przychodzących** bloku.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Generate Shared Access Signature and forward request to Azure storage.policy.xml)]

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o zasadach APIM:

+ [Zasady transformacji](../api-management-transformation-policies.md)
+ [Przykłady zasad](../policy-samples.md)

