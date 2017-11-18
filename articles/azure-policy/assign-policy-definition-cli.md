---
title: "Użyj interfejsu wiersza polecenia Azure, aby utworzyć przypisanie zasad, aby zidentyfikować niezgodnych zasobów w środowisku platformy Azure | Dokumentacja firmy Microsoft"
description: "Utwórz przypisanie zasad Azure do identyfikacji niezgodnych zasobów za pomocą programu PowerShell."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 6ea39618a24249d92b77afdf5cb0ea284b180223
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/18/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Utwórz przypisanie zasad w celu identyfikacji niezgodnych zasobów w środowisku platformy Azure z wiersza polecenia platformy Azure

Pierwszym etapem opis zgodności w usłudze Azure jest znajomość, gdzie autonomiczna z zasobami bieżącej. Ta opcja szybkiego startu przeprowadza użytkownika przez proces tworzenia przypisanie zasad do identyfikacji maszyn wirtualnych, które nie korzystają z dysków zarządzanych.

Po zakończeniu tego procesu zostanie pomyślnie zidentyfikowano maszyn wirtualnych, które nie korzystają z dysków zarządzanych i dlatego *niezgodnych*.
.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten przewodnik szybkiego startu będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli).

## <a name="opt-in-to-azure-policy"></a>Zgódź się na Azure zasad

Zasady usługi Azure jest teraz dostępna w publicznej wersji zapoznawczej i należy zarejestrować, aby zażądać dostępu.

1. Przejdź do zasad Azure https://aka.ms/getpolicy i wybierz **Utwórz konto** w okienku po lewej stronie.

   ![Wyszukaj zasady](media/assign-policy-definition/sign-up.png)

2. Zapoznać się zasadami Azure po wybraniu subskrypcji w **subskrypcji** chcesz pracować z listy. Następnie wybierz **zarejestrować**.

   ![Zezwól na przy użyciu zasad usługi Azure](media/assign-policy-definition/preview-opt-in.png)

   Żądanie zostało automatycznie zatwierdzone podglądu. Może potrwać do 30 minut, systemu przetwarzania rejestracji.

## <a name="create-a-policy-assignment"></a>Utwórz przypisanie zasad

W tego przewodnika Szybki Start możemy utworzyć przypisanie zasad i przypisanie inspekcji maszyny wirtualne bez dysków zarządzanych definicji. Ta definicja zasad identyfikuje zasoby, które nie spełniają warunków ustawionych w definicji zasad.

Wykonaj następujące kroki, aby utworzyć nowe przypisanie zasad.

Wyświetl wszystkie definicje zasad i znaleźć definicji zasad "Inspekcji maszyn wirtualnych bez zarządzania dyskami":

```azurecli
az policy definition list
```

Zasady Azure zawiera już wbudowanych w definicji zasad można użyć. Definicje wbudowanych zasad będą widoczne takie jak:

- Wymuszanie tagu i jego wartość
- Stosuje tag i jego wartość
- Wymaga programu SQL Server w wersji 12.0

Następnie podaj następujące informacje i uruchom następujące polecenie, aby przypisać definicji zasad:

- Wyświetl **nazwa** dla przypisania zasad. W takim przypadku można użyć *inspekcji maszyny wirtualne bez dysków zarządzanych*.
- **Zasady** — jest to definicja zasad, na podstawie off, którego używasz utworzyć przypisanie. W takim przypadku jest definicji zasad — *inspekcji maszyny wirtualne bez dysków zarządzanych*
- A **zakres** — zakres Określa, jakie zasoby lub grupowanie zasobów przypisania zasad pobiera wymuszane na. Mogą obejmować z subskrypcji z grupami zasobów.

  Użyj subskrypcji (lub grupy zasobów), zostały wcześniej zarejestrowane podczas zgłoszono do zasad usługi Azure w tym przykładzie używamy ten identyfikator subskrypcji - **bc75htn-a0fhsi-349b-56gh-4fghti-f84852** i nazwę grupy zasobów - **FabrikamOMS**. Pamiętaj zmienić identyfikator subskrypcji i nazwę grupy zasobów, której pracujesz.

Jest to, jak powinna wyglądać polecenia:

```azurecli
az policy assignment create --name Audit Virtual Machines without Managed Disks Assignment --policy Audit Virtual Machines without Managed Disks --scope /subscriptions/
bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

Przypisania zasad jest zasada, która została przypisana do odbywać się w określonym zakresie. Ten zakres można również należeć do zakresu od grupy zarządzania do grupy zasobów.

## <a name="identify-non-compliant-resources"></a>Zidentyfikuj zasoby niezgodnych

Aby wyświetlić zasoby, które nie są zgodne, w tym nowe przypisanie:

1. Przejdź z powrotem do strony zasad Azure.
2. Wybierz **zgodności** w okienku po lewej stronie i poszukaj **przypisania zasad** utworzony.

   ![Zasady zgodności](media/assign-policy-definition/policy-compliance.png)

   W przypadku istniejących zasobów, które nie są zgodne z tym nowe przypisanie one widoczne w obszarze **niezgodne zasoby** karcie, jak pokazano powyżej.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Przewodnikach w tej kolekcji zależą od tego przewodnika Szybki Start. Jeśli zamierzasz kontynuować pracę z kolejnych samouczkach nie wyczyścić zasoby utworzone w tym Szybki Start. Jeśli nie planujesz kontynuować, Usuń przypisanie utworzone przez uruchomienie tego polecenia:

```azurecli
az policy assignment delete –name  Assignment --scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852 resourceGroups/ FabrikamOMS
```

## <a name="next-steps"></a>Następne kroki

Tego przewodnika Szybki Start możesz przypisać definicję zasad w celu identyfikacji niezgodnych zasobów w środowisku platformy Azure.

Aby dowiedzieć się więcej o przypisanie zasad, aby upewnić się, że zasoby, należy utworzyć w **przyszłych** są zgodne, w dalszym ciągu samouczek dotyczący:

> [!div class="nextstepaction"]
> [Tworzenie i zarządzanie zasadami](./create-manage-policy.md)
