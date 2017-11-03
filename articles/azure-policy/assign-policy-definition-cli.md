---
title: "Użyj interfejsu wiersza polecenia Azure, aby utworzyć przypisanie zasad, aby zidentyfikować niezgodnych zasobów w środowisku platformy Azure | Dokumentacja firmy Microsoft"
description: "Utwórz przypisanie zasad Azure do identyfikacji niezgodnych zasobów za pomocą programu PowerShell."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark
ms.date: 10/06/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 92b532691986e72eca68d9bc3033e20ff8ffef3b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Utwórz przypisanie zasad w celu identyfikacji niezgodnych zasobów w środowisku platformy Azure z wiersza polecenia platformy Azure

Pierwszym etapem opis zgodności w usłudze Azure jest znajomość, gdzie autonomiczna z zasobami bieżącej. Ta opcja szybkiego startu przeprowadza użytkownika przez proces tworzenia zasad przydziału do identyfikacji niezgodnych zasobów o definicji zasad — *wymagają programu SQL Server 12.0*. Po zakończeniu tego procesu zostanie pomyślnie zidentyfikowano serwery są w innej wersji zasadniczo niezgodnych.

Interfejs wiersza polecenia platformy Azure umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. Szczegóły ten przewodnik do utworzenia przypisania zasad, aby zidentyfikować niezgodnych zasobów w środowisku Azure przy użyciu wiersza polecenia platformy Azure.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten przewodnik szybkiego startu będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli).
 
## <a name="opt-in-to-azure-policy"></a>Zgódź się na Azure zasad

Zasady usługi Azure jest teraz dostępna w ograniczony (wersja zapoznawcza), należy zarejestrować, aby zażądać dostępu.

1. Przejdź do zasad Azure https://aka.ms/getpolicy i wybierz **Utwórz konto** w okienku po lewej stronie.

   ![Wyszukaj zasady](media/assign-policy-definition/sign-up.png)

2. Zapoznać się zasadami Azure po wybraniu subskrypcji w **subskrypcji** chcesz pracować z listy. Następnie wybierz **zarejestrować**.

   ![Zezwól na przy użyciu zasad usługi Azure](media/assign-policy-definition/preview-opt-in.png)

   Może upłynąć kilka dni firmie Microsoft zaakceptować żądanie rejestracji, w zależności od zapotrzebowania. Po zaakceptowaniu żądania pobiera za pośrednictwem poczty e-mail są powiadomienie, że możesz rozpocząć korzystanie z usługi.

## <a name="create-a-policy-assignment"></a>Utwórz przypisanie zasad

W tego przewodnika Szybki Start możemy utworzyć przypisania zasad i przypisać definicji wymagają 12.0 wersji serwera SQL. Ta definicja zasad identyfikuje zasoby, które nie spełniają warunków ustawionych w definicji zasad.

Wykonaj następujące kroki, aby utworzyć nowe przypisanie zasad.

Wyświetl wszystkie definicje zasad i znaleźć definicji zasad "Wymagają programu SQL Server w wersji 12.0":

```azurecli
az policy definition list
```

Zasady Azure zawiera już wbudowanych w definicji zasad można użyć. Definicje wbudowanych zasad będą widoczne takie jak:

- Wymuszanie tagu i jego wartość
- Stosuje tag i jego wartość
- Wymaga programu SQL Server w wersji 12.0

Następnie podaj następujące informacje i uruchom następujące polecenie, aby przypisać definicji zasad:

- Wyświetl **nazwa** dla przypisania zasad. W takim przypadku można użyć *wymagają programu SQL Server 12.0 przypisania*.
- **Zasady** — jest to definicja zasad, na podstawie off, którego używasz utworzyć przypisanie. W takim przypadku jest definicji zasad — *wymagają programu SQL Server 12.0*
- A **zakres** — zakres Określa, jakie zasoby lub grupowanie zasobów przypisania zasad pobiera wymuszane na. Mogą obejmować z subskrypcji z grupami zasobów.

  Użyj subskrypcji (lub grupy zasobów), zostały wcześniej zarejestrowane podczas zgłoszono do zasad usługi Azure w tym przykładzie używamy ten identyfikator subskrypcji - **bc75htn-a0fhsi-349b-56gh-4fghti-f84852** i nazwę grupy zasobów - **FabrikamOMS**. Pamiętaj zmienić identyfikator subskrypcji i nazwę grupy zasobów, której pracujesz. 

Jest to, jak powinna wyglądać polecenia:

```azurecli
az policy assignment create --name Require SQL Server version 12.0 Assignment --policy Require SQL Server version 12.0 --scope /subscriptions/ 
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
az policy assignment delete –name Require SQL Server version 12.0 Assignment --scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852 resourceGroups/ FabrikamOMS
```

## <a name="next-steps"></a>Następne kroki

Tego przewodnika Szybki Start możesz przypisać definicję zasad w celu identyfikacji niezgodnych zasobów w środowisku platformy Azure.

Aby dowiedzieć się więcej o przypisanie zasad, aby upewnić się, że zasoby, należy utworzyć w **przyszłych** są zgodne, w dalszym ciągu samouczek dotyczący:

> [!div class="nextstepaction"]
> [Tworzenie i zarządzanie zasadami](./create-manage-policy.md)

