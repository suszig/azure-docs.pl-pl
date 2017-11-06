---
title: "Utwórz przypisanie zasad, aby zidentyfikować niezgodnych zasobów w środowisku platformy Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule przedstawiono kroki, aby utworzyć definicję zasad w celu identyfikacji niezgodnych zasobów."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: db5112c858d2a2c54813d9c9a3670a45fcbdb993
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment"></a>Utwórz przypisanie zasad, aby zidentyfikować niezgodnych zasobów w środowisku platformy Azure
Pierwszym etapem opis zgodności w usłudze Azure jest znajomość, gdzie autonomiczna z zasobami bieżącej. Ta opcja szybkiego startu przeprowadza użytkownika przez proces tworzenia przypisanie zasad do identyfikacji maszyn wirtualnych, które nie korzystają z dysków zarządzanych.

Po zakończeniu tego procesu zostanie pomyślnie zidentyfikowano maszyn wirtualnych, które nie są używane dyski zarządzanych i dlatego *niezgodnych*.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="opt-in-to-azure-policy"></a>Zgódź się na Azure zasad

Zasady usługi Azure jest teraz dostępna w publicznej wersji zapoznawczej i należy zarejestrować, aby zażądać dostępu.

1. Przejdź do zasad Azure https://aka.ms/getpolicy i wybierz **Utwórz konto** w okienku po lewej stronie.

   ![Wyszukaj zasady](media/assign-policy-definition/sign-up.png)

2. Wyrazić zgodę na zasady Azure, wybierając subskrypcje w **subskrypcji** chcesz pracować z listy. Następnie wybierz **zarejestrować**.

   ![Zezwól na przy użyciu zasad usługi Azure](media/assign-policy-definition/preview-opt-in.png)

   Żądanie zostało automatycznie zatwierdzone podglądu. Może potrwać do 30 minut, systemu przetwarzania rejestracji.

## <a name="create-a-policy-assignment"></a>Utwórz przypisanie zasad

W tego przewodnika Szybki Start, możemy utworzyć przypisania zasad i przypisać *inspekcji maszyny wirtualne bez dysków zarządzanych* definicji zasad.

1. Wybierz **przypisania** w okienku po lewej stronie Azure zasad.
2. Wybierz **przypisać zasady** od góry **przypisania** okienka.

   ![Przypisz definicji zasad](media/assign-policy-definition/select-assign-policy.png)

3. Na **przypisać zasady** kliknij przycisk ![przycisk definicji zasad](media/assign-policy-definition/definitions-button.png) obok **zasad** pole, aby otworzyć listę dostępnych definicji.

   ![Definicje Otwórz dostępnych zasad](media/assign-policy-definition/open-policy-definitions.png)

   Zasady Azure zawiera już wbudowanych w definicji zasad można użyć. Wbudowane zasady definicje znaleźć takich jak:

   - Wymuszanie tagu i jego wartość
   - Stosuje tag i jego wartość
   - Wymaga programu SQL Server w wersji 12.0

4. Wyszukiwanie w definicji zasad można znaleźć *inspekcji maszyn wirtualnych, które nie używają dysków zarządzanych* definicji. Kliknięcie tej zasady, a następnie kliknij przycisk **przypisać**.

   ![Definicja prawidłowe zasady](media/assign-policy-definition/select-available-definition.png)

5. Podaj wyświetlenie **nazwa** dla przypisania zasad. W takim przypadku można użyć *inspekcji maszyn wirtualnych, które nie używają dysków zarządzanych*. Można również dodać opcjonalny **opis**. Opis zawiera szczegóły dotyczące sposobu przypisania zasad identyfikuje wszystkie maszyny wirtualne utworzone w tym środowisku, które nie korzystają z dysków zarządzanych.
6. Zmiana warstwy cenowej do **standardowe** aby upewnić się, że zasady stosowany do istniejących zasobów.

   Istnieją dwie warstwy cenowej w ramach zasad usługi Azure — *wolne* i *standardowe*. Z warstwę bezpłatna, może tylko wymuszać zasady na przyszłe zasoby, podczas gdy w przypadku Standard, można również wymusić je na istniejących zasobów, aby lepiej zrozumieć swój stan zgodności. Ponieważ firma Microsoft są ograniczone w wersji zapoznawczej, firma Microsoft ma nie zostało jeszcze udostępnione modelu cenowego, więc nie otrzymają rachunek za wybranie *standardowe*. Aby dowiedzieć się więcej o cenach, obejrzyj: [cennik zasadami Azure](https://acom-milestone-ignite.azurewebsites.net/pricing/details/azure-policy/).

7. Wybierz **zakres** chcesz zasad, który ma zostać zastosowany do.  Zakres Określa, jakie zasoby lub grupowanie zasobów przypisania zasad pobiera wymuszane na. Mogą obejmować z subskrypcji z grupami zasobów.
8. Wybierz subskrypcję (lub grupy zasobów) wcześniej zarejestrowane podczas zgłoszono do zasad usługi Azure. W tym przykładzie używamy tej subskrypcji — **Azure Analytics pojemności deweloperów**, ale opcje będą się różnić.

   ![Definicja prawidłowe zasady](media/assign-policy-definition/assign-policy.png)

9. Wybierz **przypisać**.

Teraz możesz zidentyfikować niezgodnych zasobów, aby zrozumieć stan zgodności danego środowiska.

## <a name="identify-non-compliant-resources"></a>Zidentyfikuj zasoby niezgodnych

Wybierz **zgodności** w okienku po lewej stronie i poszukaj przypisanie zasady utworzone.

![Zasady zgodności](media/assign-policy-definition/policy-compliance.png)

W przypadku istniejących zasobów, które nie są zgodne z tym nowe przypisanie one będą widoczne w obszarze **niezgodne zasoby** kartę.

Jeśli warunek jest obliczane w istniejących zasobów i pochodzi się wartość true dla niektórych z nich, te zasoby są oznaczone jako niezgodne z zasadami. W tym miejscu znajduje się tabela jak działają różne akcje mamy obecnie dostępne z wynik oceny warunku i stan zgodności zasobów.

|Zasób  |Jeśli w zasadach wyrażenie  |Akcja w zasadach   |Stan zgodności  |
|-----------|---------|---------|---------|
|Exists     |True     |Zablokuj     |Niezgodne |
|Exists     |False    |Zablokuj     |Zgodność     |
|Exists     |True     |Append   |Niezgodne |
|Exists     |False    |Append   |Zgodność     |
|Exists     |True     |Inspekcja    |Niezgodne |
|Exists     |False    |Inspekcja    |Niezgodne |

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Przewodnikach w tej kolekcji zależą od tego przewodnika Szybki Start. Jeśli zamierzasz kontynuować pracę z kolejnych samouczkach nie wyczyścić zasoby utworzone w tym Szybki Start. Jeśli nie planujesz kontynuować pracy, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego przewodnika Szybki start.
1. Wybierz **przypisania** w lewym okienku.
2. Wyszukiwanie przydział, do którego został utworzony.

   ![Usuwanie przypisania](media/assign-policy-definition/delete-assignment.png)

3.  Wybierz **usunąć przypisanie**.

## <a name="next-steps"></a>Następne kroki

W tym Szybki Start definicji zasad jest przypisany do zakresu, upewnij się, że wszystkie zasoby w tym zakresie są zgodne i zidentyfikuj te, które nie są.

Aby dowiedzieć się więcej o przypisanie zasad w celu zapewnienia, że **przyszłych** zasobów, które tworzone są zgodne, w dalszym ciągu samouczek dotyczący:

> [!div class="nextstepaction"]
> [Tworzenie i zarządzanie zasadami](./create-manage-policy.md)
