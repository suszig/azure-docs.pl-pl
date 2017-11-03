---
title: "Użyj portalu Azure do zarządzania zasobami Azure | Dokumentacja firmy Microsoft"
description: "Użyj portalu Azure i usługi Azure Resource Manager do zarządzania zasobami. Pokazuje, jak pracować z pulpitów nawigacyjnych do monitorowania zasobów."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0725bbf2-5913-4c07-af6e-24e11d957fbc
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: 7a94fd5065de93384460e851627a9813d439956b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-resources-through-portal"></a>Zarządzania zasobami Azure za pośrednictwem portalu
> [!div class="op_single_selector"]
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [Interfejs wiersza polecenia platformy Azure](xplat-cli-azure-resource-manager.md)
> * [Portal](resource-group-portal.md) 
> * [Interfejs API REST](resource-manager-rest-api.md)
> 
> 

W tym temacie przedstawiono sposób użycia [portalu Azure](https://portal.azure.com) z [usługi Azure Resource Manager](resource-group-overview.md) do zarządzania zasobami platformy Azure. Aby dowiedzieć się więcej o wdrażaniu zasobów za pośrednictwem portalu, zobacz [wdrożenie zasobów z szablonami usługi Resource Manager i portalu Azure](resource-group-template-deploy-portal.md).

Obecnie nie wszystkie usługi obsługuje portalu lub Menedżera zasobów. W przypadku tych usług należy użyć [klasyczny portal](https://manage.windowsazure.com). Aby stan każdej usługi, zobacz [wykresu dostępności portalu Azure](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="manage-resource-groups"></a>Zarządzanie grupami zasobów

Grupa zasobów jest kontenerem, który zawiera powiązane zasoby Azure rozwiązania. Grupa zasobów może zawierać wszystkie zasoby dla rozwiązania lub tylko te zasoby, które mają być zarządzane jako grupa. Użytkownik decyduje o sposobie przydziału zasobów do grup zasobów pod kątem tego, co jest najbardziej odpowiednie dla danej organizacji. Ogólnie rzecz biorąc można dodać zasobów, które mają taki sam cykl życia do tej samej grupy zasobów, dlatego łatwo wdrożyć, aktualizować i usuwać je jako grupa. 

Grupa zasobów przechowuje metadane dotyczące zasobów. Z tego powodu określając lokalizację dla grupy zasobów, określasz miejsce, w którym przechowywane są metadane. Dla zachowania zgodności może być konieczne upewnienie się, że dane są przechowywane w odpowiednim regionie.

1. Aby wyświetlić wszystkie grupy zasobów w ramach subskrypcji, wybierz **grup zasobów**.
   
    ![Przeglądaj grup zasobów](./media/resource-group-portal/browse-groups.png)
2. Aby utworzyć pustej grupy zasobów, wybierz **Dodaj**.
   
    ![Dodaj grupę zasobów](./media/resource-group-portal/add-resource-group.png)
3. Podaj nazwę i lokalizację dla nowej grupy zasobów. Wybierz pozycję **Utwórz**.
   
    ![Utwórz grupę zasobów](./media/resource-group-portal/create-empty-group.png)
4. Musisz wybrać **Odśwież** Aby wyświetlić grupy ostatnio utworzony zasób.
   
    ![Odśwież grupę zasobów](./media/resource-group-portal/refresh-resource-groups.png)
5. Aby dostosować informacje wyświetlane dla grupy zasobów, wybierz opcję **kolumn**.
   
    ![Dostosowywanie kolumn](./media/resource-group-portal/select-columns.png)
6. Wybierz kolumny, które chcesz dodać, a następnie wybierz **aktualizacji**.
   
    ![Dodawanie kolumn](./media/resource-group-portal/add-columns.png)
7. Aby dowiedzieć się więcej o wdrażaniu zasobów do nowej grupy zasobów, zobacz [wdrożenie zasobów z szablonami usługi Resource Manager i portalu Azure](resource-group-template-deploy-portal.md).
8. Aby uzyskać szybki dostęp do grupy zasobów można przypiąć blok do pulpitu nawigacyjnego.
   
    ![Grupa zasobów numeru PIN](./media/resource-group-portal/pin-group.png)
9. Pulpit nawigacyjny Wyświetla grupy zasobów i jego zasoby. Można wybrać grupy zasobów albo jego zasobach można przejść do elementu.
   
    ![Grupa zasobów numeru PIN](./media/resource-group-portal/show-resource-group-dashboard.png)

## <a name="tag-resources"></a>Tag zasobów
Możliwość dodawania tagów do grup zasobów i zasobów w celu logicznego uporządkowania zasobów. Informacje o pracy z tagami, zobacz [organizowania zasobów na platformie Azure przy użyciu tagów](resource-group-using-tags.md).

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="monitor-resources"></a>Monitorowanie zasobów
Po wybraniu zasobu bloku zasobów przedstawia domyślne wykresów i tabel do monitorowania tego typu zasobu.

1. Wybierz zasób i zwróć uwagę, **monitorowanie** sekcji. Zawiera wykresy, które mają zastosowanie do tego typu zasobu. Na poniższej ilustracji przedstawiono domyślne monitorowania danych dla konta magazynu.
   
    ![Pokaż monitorowania](./media/resource-group-portal/show-monitoring.png)
2. Części bloku można przypiąć do pulpitu nawigacyjnego, wybierając wielokropek (...) powyżej sekcji. Można również dostosować rozmiar sekcji w bloku lub usuń go całkowicie. Na poniższej ilustracji przedstawiono sposób numer pin, dostosowywanie lub usunąć sekcji Procesora i pamięci.
   
    ![sekcja numeru PIN](./media/resource-group-portal/pin-cpu-section.png)
3. Po przypięciu sekcji do pulpitu nawigacyjnego, zobaczysz podsumowania na pulpicie nawigacyjnym. A, wybierając ją od razu przejście do szczegółów na temat danych.
   
    ![widok pulpitu nawigacyjnego](./media/resource-group-portal/view-startboard.png)
4. Aby całkowicie dostosować danych można monitorować za pośrednictwem portalu, przejdź do pulpitu nawigacyjnego domyślne, a następnie wybierz **nowego pulpitu nawigacyjnego**.
   
    ![pulpit nawigacyjny](./media/resource-group-portal/dashboard.png)
5. Nadaj nazwę nowego pulpitu nawigacyjnego i przeciągnij Kafelki na pulpicie nawigacyjnym. Kafelki są filtrowane według różnych opcji.
   
    ![pulpit nawigacyjny](./media/resource-group-portal/create-dashboard.png)
   
     Aby dowiedzieć się więcej o pracy z pulpitów nawigacyjnych, zobacz [tworzenie i udostępnianie pulpitów nawigacyjnych w portalu Azure](../azure-portal/azure-portal-dashboards.md).

## <a name="manage-resources"></a>Zarządzanie zasobami
W bloku zasobu można zobaczyć opcje służące do zarządzania zasobu. Portal Wyświetla opcje zarządzania dla tego typu zasobu. Dostępne polecenia zarządzania u góry bloku zasobów i po lewej stronie.

![Zarządzanie zasobami](./media/resource-group-portal/manage-resources.png)

Z tych opcji można wykonać operacji, takich jak uruchamianie i zatrzymywanie maszyny wirtualnej lub ponownej konfiguracji właściwości maszyny wirtualnej.

## <a name="move-resources"></a>Przenoszenie zasobów
Jeśli potrzebujesz przenieść zasoby do innej grupy zasobów lub inną subskrypcję, zobacz [przenoszenia zasobów do nowej grupy zasobów lub subskrypcji](resource-group-move-resources.md).

## <a name="lock-resources"></a>Blokowanie zasobów
Można zablokować subskrypcji, grupy zasobów lub zasobów uniemożliwić innym użytkownikom w organizacji przypadkowo usuwanie i modyfikowanie kluczowych zasobów. Aby uzyskać więcej informacji, zobacz [Lock resources with Azure Resource Manager](resource-group-lock-resources.md) (Blokowanie zasobów w usłudze Azure Resource Manager).

[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="view-your-subscription-and-costs"></a>Wyświetlanie Twoich subskrypcji i kosztów
Możesz przeglądać informacje o Twojej subskrypcji i zestawienia kosztów dla wszystkich zasobów. Wybierz **subskrypcje** i subskrypcję, którą chcesz wyświetlić. Może mieć tylko jedną subskrypcję, aby wybrać.

![subskrypcja](./media/resource-group-portal/select-subscription.png)

W bloku subskrypcji zostanie wyświetlony ocena wydajności.

![współczynnik spalania](./media/resource-group-portal/burn-rate.png)

I podziału kosztów według typów zasobów.

![koszt zasobów](./media/resource-group-portal/cost-by-resource.png)

## <a name="export-template"></a>Eksportowanie szablonu
Po skonfigurowaniu grupy zasobów, można wyświetlić szablonu usługi Resource Manager dla grupy zasobów. Eksportowanie szablonu oferuje dwie korzyści:

1. Można łatwo automatyzować przyszłych wdrożeń rozwiązania, ponieważ zawiera on całej infrastruktury.
2. Należy zaznajomić o składni szablonu przez wyszukiwanie w notacji obiektu JavaScript (JSON), który reprezentuje rozwiązania.

Aby uzyskać instrukcje, zobacz [szablonu eksportu usługi Azure Resource Manager z istniejących zasobów](resource-manager-export-template.md).

## <a name="delete-resource-group-or-resources"></a>Usuń grupę zasobów lub zasobów
Usunięcie grupy zasobów powoduje usunięcie wszystkich zasobów w nim zawarte. Można również usunąć pojedynczych zasobów w grupie zasobów. Należy zachować ostrożność podczas usuwania grupy zasobów, ponieważ może być zasobów w innych grup zasobów, które są z nim połączone. Menedżer zasobów nie powoduje usunięcia połączonych zasobów, ale ich nie może działać poprawnie bez oczekiwanych zasobów.

![Usuwanie grupy](./media/resource-group-portal/delete-group.png)

## <a name="next-steps"></a>Następne kroki
* Aby wyświetlić dzienniki aktywności, zobacz [inspekcji operacji za pomocą Menedżera zasobów](resource-group-audit.md).
* Aby wyświetlić szczegółowe informacje o wdrożeniu, zobacz [wyświetlić operacje wdrażania](resource-manager-deployment-operations.md).
* Aby wdrożyć zasobów za pośrednictwem portalu, zobacz [wdrożenie zasobów z szablonami usługi Resource Manager i portalu Azure](resource-group-template-deploy-portal.md).
* Aby zarządzać dostępem do zasobów, zobacz [zarządzanie dostępem do zasobów subskrypcji platformy Azure za pomocą przypisań ról](../active-directory/role-based-access-control-configure.md).
* Aby uzyskać instrukcje dla przedsiębiorstw dotyczące użycia usługi Resource Manager w celu efektywnego zarządzania subskrypcjami, zobacz [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Szkielet platformy Azure dla przedsiębiorstwa — narzucony nadzór subskrypcji).

