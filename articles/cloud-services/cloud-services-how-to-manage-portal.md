---
title: "Typowe zadania zarządzania usługi w chmurze | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie zarządzania usługami w chmurze w portalu Azure. Te przykłady, użyj portalu Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 9af1fdeb5cfe69631cabe13bd341b43319175aae
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="how-to-manage-cloud-services"></a>Jak zarządzać usługi w chmurze
W **usługi w chmurze (klasyczne)** obszaru Azure portalu, można zaktualizować roli usługi lub wdrożenie, podwyższyć poziom wdrożenia etapowego do środowiska produkcyjnego, połączyć zasoby z usługi w chmurze, tak aby widoczne zależności między zasobami i zasoby są skalowane ze sobą i usunąć usługi w chmurze lub wdrożenia.

Więcej informacji na temat skalowania usługi w chmurze [tutaj](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Porady: aktualizowanie roli usługi w chmurze lub wdrożenia
Jeśli musisz zaktualizować kod aplikacji dla usługi w chmurze, użyj **aktualizacji** w bloku usługi w chmurze. Można aktualizować jedną rolę lub wszystkich ról. Aby zaktualizować, możesz przekazać pakiet usługi lub pliku konfiguracji usługi.

1. W [portalu Azure][Azure portal], wybierz usługę w chmurze do zaktualizowania. Ten krok powoduje otwarcie bloku wystąpienia usługi w chmurze.
2. W bloku, kliknij **aktualizacji** przycisku.

    ![Przycisk Aktualizuj](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Aktualizacja wdrożenia przy użyciu nowego pliku pakietu usługi (cspkg) i pliku konfiguracji usługi (cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Opcjonalnie** zaktualizować etykietę wdrożenia i konto magazynu.
5. Jeśli żadnych ról z tylko jednego wystąpienia roli, wybierz **Wdróż, nawet jeśli co najmniej jedna rola zawiera pojedyncze wystąpienie** aktualizację kontynuować.

    Każda rola ma co najmniej dwa wystąpienia roli (maszyny wirtualne) Azure tylko można gwarantuje dostępność usługi 99,95% podczas aktualizacji usługi chmury. Dwa wystąpienia roli jednej maszyny wirtualnej przetwarza żądania klientów podczas innych aktualizacji.

6. Sprawdź **rozpocząć wdrażanie** mają zastosowane po zakończeniu przekazywania pakietu aktualizacji.
7. Kliknij przycisk **OK** zacząć aktualizacji usługi.

## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Porady: Zamień wdrożeń, aby promować przemieszczanego wdrożenia do produkcji
Jeśli zdecydujesz się wdrożyć nową wersję usługi w chmurze, etap i przetestować w środowisku przemieszczania usługi chmury z nową wersją. Użyj **wymiany** przełączyć adresów URL, które są opisane dwa wdrożenia i wspierania nową wersję w środowisku produkcyjnym.

Można wymienić wdrożenia od **usługi w chmurze** strony lub pulpitu nawigacyjnego.

1. W [portalu Azure][Azure portal], wybierz usługę w chmurze do zaktualizowania. Ten krok powoduje otwarcie bloku wystąpienia usługi w chmurze.
2. W bloku, kliknij **wymiany** przycisku.

    ![Wymiany usługi w chmurze](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Otwiera następujące monit o potwierdzenie.

    ![Wymiany usługi w chmurze](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Po sprawdzeniu informacji wdrożenia, kliknij przycisk **OK** można zamienić wdrożenia.

    Wymiany wdrożenia odbywa się szybko ponieważ jedynym elementem, który zmienia się wirtualne adresy IP (VIP) dla wdrożenia.

    W celu ograniczenia kosztów obliczeń, można usunąć tymczasowej wdrożenia po sprawdzeniu, czy wdrożenia produkcyjnego działa zgodnie z oczekiwaniami.

### <a name="common-questions-about-swapping-deployments"></a>Często zadawane pytania dotyczące wymiany wdrożenia

**Jakie są wymagania wstępne dotyczące wymiany wdrożenia?**

Istnieją dwa kluczowe wymagania wstępne dotyczące wymiany pomyślne wdrożenie:

- Jeśli chcesz użyć statycznego adresu IP dla Twojego miejsca produkcji, należy zarezerwować jeden dla miejsca przemieszczania również. W przeciwnym razie wymiany nie powiedzie się.

- Wszystkie wystąpienia ról musi być uruchomiona, zanim będzie można wykonać wymiany. Można sprawdzić stan swoich wystąpień, w bloku omówienie portalu Azure. Alternatywnie można użyć [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) polecenia w programie Windows PowerShell.

Należy pamiętać, że aktualizacje systemu operacyjnego gościa i naprawianie operacji usługi również może spowodować zamiany wdrażania zakończyć się niepowodzeniem. Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów z wdrażaniem usługi chmury](cloud-services-troubleshoot-deployment-problems.md).

**Zamiana wpływa negatywnie przestojów w przypadku mojej aplikacji? Jak należy ją obsługuje?**

Zgodnie z opisem w ostatniej sekcji, wymiany wdrożenia jest zazwyczaj szybkie, ponieważ jest on tylko zmiany konfiguracji usługi równoważenia obciążenia Azure. W niektórych przypadkach jednak może potrwać co najmniej 10 sekund i powodować błędy połączeń przejściowej. Aby ograniczyć wpływ na klientów, rozważ zaimplementowanie [Logika ponawiania klienta](../best-practices-retry-general.md).

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Porady: łączenie zasobu usługi w chmurze
Azure portal nie łączenie zasobów jak w bieżącym portalu klasycznego Azure. Zamiast tego należy wdrożyć dodatkowe zasoby do tej samej grupy zasobów, które są używane przez usługę w chmurze.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Porady: usuwanie wdrożeń i usługi w chmurze
Aby można było usunąć usługi w chmurze, należy usunąć poszczególnych istniejącego wdrożenia.

W celu ograniczenia kosztów obliczeń, można usunąć tymczasowej wdrożenia po sprawdzeniu, czy wdrożenia produkcyjnego działa zgodnie z oczekiwaniami. Rozliczenie jest kosztów obliczeniowe dla wystąpień roli wdrożone, które zostały zatrzymane.

Użyj poniższej procedury można usunąć wdrożenia lub usługi w chmurze.

1. W [portalu Azure][Azure portal], wybierz usługę w chmurze do usunięcia. Ten krok powoduje otwarcie bloku wystąpienia usługi w chmurze.
2. W bloku, kliknij **usunąć** przycisku.

    ![Wymiany usługi w chmurze](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Można usunąć usługi w chmurze całego sprawdzając **Cloud service i jej wdrożenia** lub wybrać opcję **wdrożenia produkcyjnego** lub **przemieszczania — wdrażanie**.

    ![Wymiany usługi w chmurze](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Kliknij przycisk **usunąć** znajdujący się u dołu.
5. Aby usunąć usługi w chmurze, kliknij przycisk **usługi w chmurze Delete**. Następnie w oknie monitu o potwierdzenie kliknij przycisk **tak**.

> [!NOTE]
> Po usunięciu usługi w chmurze, a pełne monitorowania jest skonfigurowana, danych trzeba usunąć ręcznie z konta magazynu. Aby uzyskać informacje o tym, gdzie można znaleźć w tabelach metryki, zobacz [to](cloud-services-how-to-monitor.md) artykułu.


## <a name="how-to-find-more-information-about-failed-deployments"></a>Porady: więcej informacji o wdrożeniach nie powiodło się
**Omówienie** bloku ma pasek stanu u góry. Po kliknięciu przycisku paska otwiera nowy blok i wyświetla informacje o błędzie. Jeśli wdrożenie nie zawiera żadnych błędów, blok informacji jest pusta.

![Wymiany usługi w chmurze](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Następne kroki
* [Konfiguracja ogólna usługi w chmurze](cloud-services-how-to-configure-portal.md).
* Dowiedz się, jak [wdrażania usługi w chmurze](cloud-services-how-to-create-deploy-portal.md).
* Skonfiguruj [niestandardowej nazwy domeny](cloud-services-custom-domain-name-portal.md).
* Skonfiguruj [certyfikaty ssl](cloud-services-configure-ssl-certificate-portal.md).
