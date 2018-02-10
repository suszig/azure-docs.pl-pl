---
title: "Nieprawidłowy szablon Azure błędy | Dokumentacja firmy Microsoft"
description: "Opisuje sposób rozwiązania błędów szablonu jest nieprawidłowa."
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
ms.openlocfilehash: 87bc6e4def624785c5052a9a25f579b022c940ec
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="resolve-errors-for-invalid-template"></a>Rozwiązywanie błędów dla szablonu jest nieprawidłowa

W tym artykule opisano sposób rozwiązania błędów szablonu jest nieprawidłowa.

## <a name="symptom"></a>Objaw

W przypadku wdrażania szablonu, pojawi się o błędzie z informacją:

```
Code=InvalidTemplate
Message=<varies>
```

Komunikat o błędzie zależy od typu błędu.

## <a name="cause"></a>Przyczyna

Ten błąd może wynikać z kilku różnych typów błędów. Wymagają one zazwyczaj strukturalnych lub składnia błąd w szablonie.

## <a name="solution"></a>Rozwiązanie

### <a name="solution-1---syntax-error"></a>Rozwiązanie 1 — błąd składni

Jeśli zostanie wyświetlony komunikat o błędzie wskazuje szablonu nie powiodło się sprawdzanie poprawności, mogą mieć problem składni w szablonie.

```
Code=InvalidTemplate
Message=Deployment template validation failed
```

Ten błąd jest proste wyrażenia szablonu mogą być skomplikowanych. Na przykład następujące przypisanie nazwy dla konta magazynu zawiera jeden zestaw nawiasów, trzy funkcje trzy zestawy nawiasów, jeden zestaw apostrofy i jedną właściwość:

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
```

Jeśli nie podasz Składnia dopasowania, szablon tworzy wartość, która różni się od masz zamiar.

Po otrzymaniu tego typu błędu, należy dokładnie przejrzeć składni wyrażeń. Należy wziąć pod uwagę przy użyciu edytora JSON, takich jak [programu Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) lub [Visual Studio Code](resource-manager-vs-code.md), który może zostać wyświetlone ostrzeżenie dotyczące błędy składniowe.

### <a name="solution-2---incorrect-segment-lengths"></a>Rozwiązanie 2 - niepoprawną długość

Inny błąd nieprawidłowego szablonu występuje, gdy nazwa zasobu nie jest w nieprawidłowym formacie.

```
Code=InvalidTemplate
Message=Deployment template validation failed: 'The template resource {resource-name}'
for type {resource-type} has incorrect segment lengths.
```

Zasób poziomu głównego musi mieć jeden mniej segment w nazwie niż w typie zasób. Każdy z segmentów odróżnia się kreską ułamkową. W poniższym przykładzie Typ zawiera dwa segmenty, a nazwa ma jeden segment, dzięki czemu **prawidłową nazwę**.

```json
{
  "type": "Microsoft.Web/serverfarms",
  "name": "myHostingPlanName",
  ...
}
```

W następnym przykładzie, ale **Nieprawidłowa nazwa** ponieważ ma taką samą liczbę segmentów jako typu.

```json
{
  "type": "Microsoft.Web/serverfarms",
  "name": "appPlan/myHostingPlanName",
  ...
}
```

Typ i nazwa dla zasoby podrzędne mają taką samą liczbę segmentów. Liczba segmentów ma sens, ponieważ Pełna nazwa i typ dla obiekt podrzędny zawiera nadrzędnego nazwę i typ. W związku z tym pełną nazwę nadal ma jeden segment mniej niż pełny typ.

```json
"resources": [
    {
        "type": "Microsoft.KeyVault/vaults",
        "name": "contosokeyvault",
        ...
        "resources": [
            {
                "type": "secrets",
                "name": "appPassword",
                ...
            }
        ]
    }
]
```

Pobieranie segmenty prawo można trudnych z typami Menedżera zasobów, które są stosowane przez dostawców zasobów. Na przykład stosowania Blokada zasobu do witryny sieci web wymaga typu z czterech segmentów. W związku z tym nazwa jest trzy segmenty:

```json
{
    "type": "Microsoft.Web/sites/providers/locks",
    "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
    ...
}
```

### <a name="solution-3---parameter-is-not-valid"></a>Rozwiązanie 3 — parametr jest nieprawidłowy

Jeśli szablon służy do dozwolonych wartości dla parametru, a następnie podaj wartość, która nie jest jednym z tych wartości, pojawi się komunikat podobny do następującego błędu:

```
Code=InvalidTemplate;
Message=Deployment template validation failed: 'The provided value {parameter value}
for the template parameter {parameter name} is not valid. The parameter value is not
part of the allowed values
```

Dokładnie sprawdź dozwolone wartości w szablonie i podaj podczas wdrażania.

### <a name="solution-4---too-many-target-resource-groups"></a>Rozwiązanie 4 - zbyt wiele grup zasobów obiektu docelowego

Jeśli określono więcej niż pięć grup zasobów obiektu docelowego w jednym wdrożeniu, ten błąd jest wyświetlany. Należy wziąć pod uwagę konsolidowanie liczbę grup zasobów w danym wdrożeniu albo wdrażanie niektóre szablony jako oddzielnych wdrożeń. Aby uzyskać więcej informacji, zobacz [zasobów Azure wdrożyć więcej niż jedną subskrypcję lub grupy zasobów](resource-manager-cross-resource-group-deployment.md).

### <a name="solution-5---circular-dependency-detected"></a>Wykryto rozwiązania 5 - zależność cykliczną

Ten błąd jest wyświetlany, gdy zasoby są zależne od siebie nawzajem w sposób uniemożliwiający wdrożenia uruchamianiu. Kombinacja zależnościami sprawia, że dwa lub więcej zasobów, poczekaj, aż inne zasoby, które są również oczekiwania. Na przykład zasób1 zależy od resource3 zasób2 zależy od zasób1 i resource3 zależy od zasób2. Zazwyczaj można rozwiązać ten problem, usuwając zbędne zależności.
