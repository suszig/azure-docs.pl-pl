---
title: "Zasady zasobów platformy Azure dla konta usługi storage | Dokumentacja firmy Microsoft"
description: "Opisuje zasady usługi Azure Resource Manager do zarządzania wdrażaniem kont magazynu."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2017
ms.author: tomfitz
ms.openlocfilehash: 6612ee61f5c50e743241b92030660cea7ae7094d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="apply-resource-policies-to-storage-accounts"></a>Zastosuj zasady zasobów do kont magazynu
W tym temacie przedstawiono kilka [zasad zasobów](resource-manager-policy.md) można zastosować do kont magazynu Azure. Te zasady zapewnienia spójności w przypadku kont magazynu w organizacji wdrożono. 

## <a name="define-permitted-storage-account-types"></a>Zdefiniuj typy kont magazynu dozwolone

Ogranicza następujące zasady, które [typy kont magazynu](../storage/common/storage-redundancy.md) można wdrożyć:

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/sku.name",
          "in": [
            "Standard_LRS",
            "Standard_GRS"
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Podobne reguły z parametrem akceptowania dozwolonych jednostki SKU jest dostępna jako definicję wbudowanych zasad. Wbudowane zasady ma identyfikator zasobu `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`. 

## <a name="define-permitted-access-tier"></a>Zdefiniuj warstwy dostępu dozwolone

Następujące zasady określa typ [warstwy dostępu](../storage/blobs/storage-blob-storage-tiers.md) które można określić dla kont magazynu:

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="ensure-encryption-is-enabled"></a>Upewnij się, że szyfrowanie jest włączone

Następujące zasady wymagają wszystkich kont magazynu, aby włączyć [szyfrowanie usługi Magazyn](../storage/common/storage-service-encryption.md):

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/enableBlobEncryption",
          "equals": "true"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Ta reguła zasad jest również dostępny jako definicję wbudowanych zasad o identyfikatorze zasobu `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.

## <a name="next-steps"></a>Następne kroki
* Po zdefiniowaniu reguły zasad (jak pokazano w powyższych przykładach), należy utworzyć definicji zasad i przypisać je do zakresu. Zakres może być subskrypcji, grupy zasobów lub zasobów. Aby przypisać zasady za pośrednictwem portalu, zobacz [portal Azure używany do przypisywania i zarządzanie zasadami zasobów](resource-manager-policy-portal.md). Aby przypisać zasady za pomocą interfejsu API REST, programu PowerShell lub interfejsu wiersza polecenia Azure, zobacz [przypisania zasad i zarządzania nimi za pośrednictwem skryptu](resource-manager-policy-create-assign.md). 
* Aby uzyskać instrukcje dla przedsiębiorstw dotyczące użycia usługi Resource Manager w celu efektywnego zarządzania subskrypcjami, zobacz [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Szkielet platformy Azure dla przedsiębiorstwa — narzucony nadzór subskrypcji).

