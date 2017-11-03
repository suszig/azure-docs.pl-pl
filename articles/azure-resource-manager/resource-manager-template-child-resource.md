---
title: "Definicja zasobu podrzędnego w szablonie Azure | Dokumentacja firmy Microsoft"
description: "Pokazuje, jak ustawić typ zasobu i nazwę zasobu podrzędnego w szablonie usługi Azure Resource Manager"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: tomfitz
ms.openlocfilehash: 5b6ce5526f354008eb4a697deec737876f22391f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a>Ustaw nazwę i typ zasobu podrzędnego w szablonie usługi Resource Manager
Podczas tworzenia szablonu, często konieczne jest uwzględnienie zasobu podrzędnego, która jest powiązana z zasobem nadrzędnej. Na przykład do szablonu może zawierać programu SQL server i bazy danych. Serwer SQL jest zasobem nadrzędnej, a baza danych jest zasobu podrzędnego. 

Format podrzędny typ zasobu jest:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

Format podrzędny Nazwa zasobu jest:`{parent-resource-name}/{child-resource-name}`

Jednak należy określić typ i nazwa w szablonie inaczej na podstawie tego, czy jest zagnieżdżony w obrębie zasobu nadrzędnego, lub na jego własnej na najwyższym poziomie. W tym temacie przedstawiono sposób obsługi obu podejść.

Podczas konstruowania pełni kwalifikowane odwołanie do zasobu, aby połączyć segmenty z typem i nazwą nie jest po prostu składa się z dwóch.  Po obszarze nazw, użyj sekwencji *typ/nazwa* pary z najmniej specyficznych, które specyficzny:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Na przykład:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`jest poprawna `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` jest nieprawidłowy

## <a name="nested-child-resource"></a>Zasób zagnieżdżonych elementów podrzędnych
Najprostszym sposobem definiowania zasobu podrzędnego jest zagnieździć w obrębie zasobu nadrzędnego. W poniższym przykładzie przedstawiono bazy danych SQL zagnieżdżone w obrębie w programie SQL Server.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

Dla zasobu podrzędnego ustawiono typ `databases` , ale jego typ zasobu pełne jest `Microsoft.Sql/servers/databases`. Nie podawaj `Microsoft.Sql/servers/` ponieważ zakłada się z nadrzędnego typu zasobu. Ustawiono nazwę zasobu podrzędnego `exampledatabase` , ale imię i nazwisko zawiera nazwę nadrzędnej. Nie podawaj `exampleserver` ponieważ zakłada się od zasobu nadrzędnego.

## <a name="top-level-child-resource"></a>Zasobu podrzędnego najwyższego poziomu
Można zdefiniować zasobu podrzędnego na najwyższym poziomie. Tej metody może użyć, jeśli zasobu nadrzędnego nie została wdrożona w tym samym szablonie, lub jeśli chcesz użyć `copy` utworzyć wiele podrzędnych zasobów. Z tej metody należy udostępniają typ zasobu pełne i nazwy zasobu podrzędne zawierają nazwę zasobu nadrzędnego.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

Baza danych jest zasobu podrzędnego z serwerem, nawet jeśli są one zdefiniowane w tym samym poziomie w szablonie.

## <a name="next-steps"></a>Następne kroki
* Aby uzyskać zalecenia dotyczące tworzenia szablonów, zobacz [najlepszych rozwiązań dotyczących tworzenia szablonów usługi Azure Resource Manager](resource-manager-template-best-practices.md).
* Przykład tworzenia wielu podrzędnych zasobów, zobacz [wdrażanie wielu wystąpień zasobów w szablonach usługi Azure Resource Manager](resource-group-create-multiple.md).
