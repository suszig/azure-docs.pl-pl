---
title: "Rozwiązania w Operations Management Suite (OMS) | Dokumentacja firmy Microsoft"
description: "Rozwiązania rozszerzyć funkcjonalność z Operations Management Suite (OMS) przez zapewnienie scenariuszy pakietów zarządzania, dodawanych przez klientów do ich obszarem roboczym pakietu OMS.  Ten artykuł zawiera szczegółowe informacje na temat niestandardowych rozwiązań utworzonych przez klientom i partnerom."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 1f054a4e-6243-4a66-a62a-0031adb750d8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2443dd73fdf441721bd6f6f340da515d9f5a22a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="working-with-management-solutions-in-operations-management-suite-oms-preview"></a>Praca z rozwiązaniami do zarządzania w Operations Management Suite (OMS) (wersja zapoznawcza)
> [!NOTE]
> Jest to wstępne dokumentacji dla rozwiązań do zarządzania w OMS, które są obecnie w wersji zapoznawczej.    
> 
> 

Rozwiązania do zarządzania rozszerzyć funkcjonalność z Operations Management Suite (OMS) podając scenariuszy pakietów zarządzania, dodawanych przez klientów do ich środowiska.  Oprócz [rozwiązań dostarczonych przez firmę Microsoft](../log-analytics/log-analytics-add-solutions.md), partnerów i klientów można tworzyć rozwiązania do zarządzania do użycia w środowisku własnych lub udostępniona użytkownikom za pośrednictwem przez społeczność.

## <a name="finding-and-installing-management-solutions"></a>Znajdowanie i instalowanie rozwiązania do zarządzania
Istnieje wiele metod do lokalizowania i instalowanie rozwiązania do zarządzania, zgodnie z opisem w poniższych sekcjach.

### <a name="azure-marketplace"></a>Azure Marketplace
Rozwiązania do zarządzania są dostarczane przez firmę Microsoft i partnerów zaufanych może być zainstalowana z portalu Azure Marketplace w portalu Azure.

1. Zaloguj się do portalu Azure.
2. W okienku po lewej stronie wybierz **więcej usług**.
3. Albo przewiń w dół do **rozwiązań** lub typ *rozwiązań* do **filtru** okna dialogowego.
4. Kliknij przycisk **+ Dodaj** przycisku.
5. Wyszukaj rozwiązania, które interesują Cię przez przeglądanie, klikając pozycję **filtru** przycisk lub wprowadzając w **Everthing wyszukiwania** pole.
6. Kliknij element marketplace, aby wyświetlić szczegółowe informacje.
7. Kliknij przycisk **Utwórz** otworzyć **Dodaj rozwiązanie** okienka.
8. Użytkownik otrzyma monit wymaganych informacji takich jak [OMS obszaru roboczego i konto automatyzacji](#oms-workspace-and-automation-account) oprócz wartości parametrów w rozwiązaniu.
9. Kliknij przycisk **Utwórz** zainstalować rozwiązania.

### <a name="oms-portal"></a>Portalu OMS
Rozwiązania do zarządzania firmy Microsoft mogą być zainstalowane z galerii rozwiązań w portalu OMS.

1. Zaloguj się do portalu OMS.
2. Kliknij przycisk **galerii rozwiązań** kafelka.
3. Na stronie galerii rozwiązań OMS informacje na temat każdego z dostępnych rozwiązań. Kliknij nazwę rozwiązanie, które mają zostać dodane do pakietu OMS.
4. Na stronie dla wybranego rozwiązania są wyświetlane szczegółowe informacje o rozwiązaniu. Kliknij pozycję **Dodaj**.
5. Nowy Kafelek dla rozwiązania, które zostały dodane, pojawi się na przegląd strony w OMS i rozpocząć korzystanie z niej po przetworzeniu danych przez usługę.

### <a name="azure-quickstart-templates"></a>Szablony szybkiego startu platformy Azure
Członków społeczności można przesłać do szablonów Szybki Start Azure rozwiązania do zarządzania.  Możesz pobrać te szablony do nowszej instalacji lub sprawdzić je, aby dowiedzieć się jak [tworzenia własnych rozwiązań](#creating-a-solution).

1. Wykonaj czynności opisane w [OMS obszaru roboczego i konto automatyzacji](#oms-workspace-and-automation-account) połączyć obszaru roboczego i konta.
2. Przejdź do [szablonów Szybki Start Azure](https://azure.microsoft.com/documentation/templates/).  
3. Wyszukiwanie rozwiązania, które Cię interesują.
4. Wybierz rozwiązanie z wyników, aby wyświetlić jego szczegóły.
5. Kliknij przycisk **wdrażanie na platformie Azure** przycisku.
6. Pojawi się monit o podanie informacji takich jak grupy zasobów i lokalizacji oprócz wartości parametrów w rozwiązaniu.
7. Kliknij przycisk **zakupu** zainstalować rozwiązania.

### <a name="deploy-azure-resource-manager-template"></a>Wdrażanie szablonu usługi Azure Resource Manager
Rozwiązania, które można uzyskać od społeczności lub [samodzielnie utworzyć](#creating-a-solution) są zaimplementowane jako szablon Menedżera zasobów i można użyć dowolnego z standardowe metody [wdrażania szablonu](../azure-resource-manager/resource-group-template-deploy-portal.md).  Należy pamiętać, że przed zainstalowaniem rozwiązania, musisz utworzyć i połączyć [OMS obszaru roboczego i konto automatyzacji](#oms-workspace-and-automation-account).

## <a name="oms-workspace-and-automation-account"></a>Obszar roboczy OMS i konta automatyzacji
Większość rozwiązań do zarządzania wymagają [obszarem roboczym pakietu OMS](../log-analytics/log-analytics-manage-access.md) zawiera widoki i [konto automatyzacji](../automation/automation-security-overview.md#automation-account-overview) zawiera elementy runbook i powiązanych zasobów. Obszar roboczy i konto musi spełniać następujące wymagania.

* Rozwiązanie można używać tylko jeden obszar roboczy OMS i jedno konto automatyzacji.  
* Obszar roboczy OMS i automatyzacji konta używanego przez rozwiązanie muszą być połączone z sobą. Obszar roboczy OMS może odnosić się tylko do jednego konta automatyzacji, a konto usługi Automatyzacja może odnosić się tylko do jednego obszarem roboczym pakietu OMS.
* Połączone, obszar roboczy OMS i konto automatyzacji muszą być w tej samej grupie zasobów i region.  Wyjątkiem jest obszar roboczy OMS w regionie wschodnie stany USA i i konto usługi Automatyzacja w wschodnie stany USA 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Tworzenie połączenia między obszarem roboczym pakietu OMS i konta automatyzacji
Sposób Określ obszar roboczy OMS i konto automatyzacji jest zależna od metody instalacji dla rozwiązania.

* Po zainstalowaniu rozwiązanie firmy Microsoft za pośrednictwem portalu OMS jest zainstalowana w bieżącym obszarze roboczym pakietu OMS i żadne konto automatyzacji nie jest wymagane.
* Po zainstalowaniu rozwiązania za pośrednictwem portalu Azure Marketplace, zostanie wyświetlony monit o obszarem roboczym pakietu OMS i konto usługi Automatyzacja i jest tworzone łącze między nimi.  
* Rozwiązania poza portalu Azure Marketplace możesz połączyć obszarem roboczym pakietu OMS i konto automatyzacji przed zainstalowaniem rozwiązania.  Można to zrobić, wybranie dowolnego rozwiązania w portalu Azure Marketplace i wybierając obszar roboczy OMS i konta automatyzacji.  Nie masz faktycznie zainstalować rozwiązania, ponieważ łącze zostanie utworzony, jak zaznaczono obszar roboczy OMS i konta automatyzacji.  Po utworzeniu łącza, a następnie można użyć tego obszarem roboczym pakietu OMS i konto automatyzacji dla dowolnego rozwiązania. 

### <a name="verifying-the-link-between-an-oms-workspace-and-automation-account"></a>Sprawdzanie połączenia między obszarem roboczym pakietu OMS i konta automatyzacji
Można sprawdzić połączenie między obszarem roboczym pakietu OMS i konto usługi Automatyzacja, korzystając z następującej procedury.

1. Wybierz konto usługi Automatyzacja w portalu Azure.
2. Przewiń do dołu **ustawienia** okienka.
3. Jeśli jest sekcji o nazwie **zasobów OMS** w **ustawienia** okienka, a następnie to konto jest powiązany z obszarem roboczym pakietu OMS.
4. Wybierz **obszaru roboczego** Aby wyświetlić szczegółowe informacje o obszarze roboczym pakietu OMS połączone z tego konta automatyzacji.

## <a name="listing-management-solutions"></a>Lista rozwiązań do zarządzania
Umożliwia poniższej procedury do wyświetlanie rozwiązania do zarządzania w obszary robocze połączone z subskrypcją platformy Azure.

1. Zaloguj się do portalu Azure.
2. W okienku po lewej stronie wybierz **więcej usług**.
3. Albo przewiń w dół do **rozwiązań** lub typ *rozwiązań* do **filtru** okna dialogowego.
4. Rozwiązania zainstalowane w wszystkie obszary robocze zostaną wyświetlone.

Należy pamiętać, że można wyświetlić tylko rozwiązania firmy Microsoft zainstalowane w bieżącym obszarze roboczym przy użyciu portalu OMS.

## <a name="removing-a-management-solution"></a>Usuwanie rozwiązania do zarządzania
Rozwiązania do zarządzania zostanie usunięty, również zostaną usunięte wszystkie zasoby w rozwiązaniu.  

1. Znajdź rozwiązania w portalu Azure, korzystając z procedury w [listę rozwiązań](#listing-solutions).
2. Wybierz rozwiązanie, które chcesz usunąć.
3. Kliknij przycisk **usunąć** przycisku.

## <a name="creating-a-management-solution"></a>Tworzenie rozwiązania do zarządzania
Pełne wskazówki dotyczące tworzenia rozwiązań do zarządzania są dostępne pod adresem [tworzenie rozwiązań w Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md). 

## <a name="next-steps"></a>Następne kroki
* Wyszukiwanie [szablonów Szybki Start Azure](https://azure.microsoft.com/documentation/templates) przykłady różnych szablonów usługi Resource Manager.
* Utwórz swój własny [rozwiązań do zarządzania](operations-management-suite-solutions-creating.md).

