---
title: "Błąd RequestDisallowedByPolicy z zasadami zasobów platformy Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano przyczyny tego błędu RequestDisallowedByPolicy."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: c950fe5444d4eacf3568d355a8dc9c5699645229
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Błąd RequestDisallowedByPolicy z zasadami zasobów platformy Azure

W tym artykule opisano przyczyny tego błędu RequestDisallowedByPolicy, umożliwia także rozwiązania dla tego błędu.

## <a name="symptom"></a>Objaw

Podczas próby wykonania akcji podczas wdrażania, może zostać wyświetlony **RequestDisallowedByPolicy** błąd, który uniemożliwia wykonanie tej akcji. W poniższym przykładzie pokazano kod błędu:

```json
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Aby uzyskać szczegółowe informacje dotyczące zasad zablokowane wdrożenia, użyj następujących metod:

### <a name="method-1"></a>1 — metoda

W programie PowerShell, podaj identyfikator zasad jako `Id` parametr, aby pobrać szczegółowe informacje dotyczące zasad zablokowane wdrożenia.

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="method-2"></a>2 — metoda 

W programie Azure CLI 2.0 należy podać nazwę definicji zasad: 

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Rozwiązanie

Zabezpieczeń i zgodności dział IT może wymusić zasady zasobów, które uniemożliwia tworzenie adresów publicznego adresu IP, grupy zabezpieczeń sieci, trasy zdefiniowane przez użytkownika lub tabele tras. Komunikat o błędzie w **objawy** sekcji przedstawiono zasady o nazwie **regionPolicyDefinition**. Zasady mogą mieć inną nazwę.
Aby rozwiązać ten problem, pracować ze swoim działem IT, aby przejrzeć zasad zasobów i określić, w jaki można wykonać żądanej akcji zgodnie z tymi zasadami.

Aby uzyskać więcej informacji zobacz następujące artykuły:

- [Przegląd zasad zasobów](resource-manager-policy.md)
- [Wyświetl przypisania zasad za pośrednictwem portalu](resource-manager-policy-portal.md#view-policy-assignments)
