---
title: "Nie znaleziono błędy zasobów platformy Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób naprawić błędy, gdy nie można odnaleźć zasobu."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: c76c965c43ca8217faa9488c01975ce09a21daaf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Rozwiązywanie błędów nie została znaleziona dla zasobów platformy Azure

W tym artykule opisano błędy, które mogą wystąpić, gdy nie można odnaleźć zasobu podczas wdrażania. 

## <a name="symptom"></a>Objaw

Jeśli szablon zawiera nazwę zasobu, którego nie można rozwiązać, komunikat o błędzie podobny do:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Jeśli próba użycia [odwołania](resource-group-template-functions-resource.md#reference) lub [listKeys](resource-group-template-functions-resource.md#listkeys) funkcje z zasobów, których nie można rozpoznać, zostanie wyświetlony następujący błąd:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

## <a name="cause"></a>Przyczyna

Menedżer zasobów musi pobrać właściwości dla zasobu, ale nie można zidentyfikować zasobów w ramach subskrypcji.

## <a name="solution"></a>Rozwiązanie

### <a name="solution-1"></a>Rozwiązanie 1

Jeśli próbujesz wdrożyć brakującego zasobu w szablonie, sprawdź, czy konieczne jest dodanie zależności. Menedżer zasobów optymalizuje wdrożenia przez utworzenie zasobów równoległe, gdy jest to możliwe. Jeśli jeden zasób należy wdrożyć po inny zasób, należy użyć **dependsOn** element do szablonu, aby utworzyć zależność od innego zasobu. Na przykład w przypadku wdrażania aplikacji sieci web, musi istnieć plan usługi aplikacji. Jeśli nie określono czy aplikacji sieci web jest zależny od planu usług aplikacji, usługi Resource Manager tworzy oba zasoby w tym samym czasie. Pojawi się komunikat o błędzie informujący, że nie można odnaleźć zasobu planu usługi aplikacji, ponieważ nie istnieje jeszcze podczas próby ustawienia właściwości w aplikacji sieci web. Ten błąd uniemożliwia ustawiając zależności w aplikacji sieci web.

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

Aby sugestie dotyczące rozwiązywania problemów z błędami zależności, zobacz [Sprawdź wdrożenia sekwencji](resource-manager-troubleshoot-tips.md#check-deployment-sequence).

### <a name="solution-2"></a>Rozwiązanie 2

Gdy zasób istnieje w innej grupie zasobów niż podczas wdrażania, użyj [funkcja resourceId](resource-group-template-functions-resource.md#resourceid) można uzyskać w pełni kwalifikowana nazwa zasobu.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

### <a name="solution-3"></a>Rozwiązanie 3

Wyszukaj wyrażenie, które zawiera [odwołania](resource-group-template-functions-resource.md#reference) funkcji. Wartości podane różnić w zależności od tego, czy zasób jest w szablonie, grupy zasobów i subskrypcji. Sprawdź, że udostępniasz wartości wymaganego parametru dla danego scenariusza. Jeśli zasób znajduje się w innej grupie zasobów, podaj identyfikator zasobu pełna. Na przykład aby odwołać się do konta magazynu w innej grupie zasobów, należy użyć:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```