---
title: "Typowe chmurze Usługa zarządzania zadania (klasyczne) | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie zarządzania usługami w chmurze w klasycznym portalu Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 41a30e50-067c-485b-96fd-434a6d057abc
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 2ee76dfcb579e53975b1f61a6590f8d78dc0961b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-manage-cloud-services"></a>Jak zarządzać usługi w chmurze
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](cloud-services-how-to-manage-portal.md)
> * [Klasyczna witryna Azure Portal](cloud-services-how-to-manage.md)
>
>

W **usługi w chmurze** obszaru klasyczny Portal Azure portalu, można zaktualizować roli usługi lub wdrożenie, podwyższyć poziom wdrożenia etapowego do środowiska produkcyjnego, połączyć zasoby z usługi w chmurze, tak aby widoczne zależności między zasobami i zasoby są skalowane ze sobą i usunąć usługi w chmurze lub wdrożenia.

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Porady: aktualizowanie roli usługi w chmurze lub wdrożenia
Jeśli musisz zaktualizować kod aplikacji dla usługi w chmurze, użyj **aktualizacji** na pulpicie nawigacyjnym **usługi w chmurze** strony, lub **wystąpień** strony. Można aktualizować jedną rolę lub wszystkich ról. Musisz przekazać nowy pakiet usługi i pliku konfiguracji usługi.

1. W [klasycznego portalu Azure](https://manage.windowsazure.com/), na pulpicie nawigacyjnym **usługi w chmurze** strony, lub **wystąpień** kliknij przycisk **aktualizacji**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. W **etykieta wdrożenia**, wprowadź nazwę identyfikującą wdrażania (na przykład mycloudservice4). Można znaleźć etykiety wdrożenia w obszarze **szybki start** na pulpicie nawigacyjnym.
3. W **pakietu**, użyj **Przeglądaj** przekazać plik pakietu usługi (cspkg).
4. W **konfiguracji**, użyj **Przeglądaj** przekazać plik konfiguracji usługi (cscfg).
5. W **roli**, wybierz pozycję **wszystkie** Jeśli chcesz uaktualnić wszystkie role w usłudze w chmurze. Aby przeprowadzić aktualizację jedną rolę, wybierz rolę, którą chcesz zaktualizować. Nawet w przypadku wybrania konkretnej roli można zaktualizować, aktualizacje w pliku konfiguracji usługi są stosowane do wszystkich ról.
6. W przypadku aktualizacji zmiany liczby ról lub rozmiar żadnych ról, wybierz **zezwolić na aktualizacje w przypadku zmiany rozmiarów ról lub liczby ról** pole wyboru, aby włączyć tę aktualizację, aby kontynuować.

    Należy pamiętać, że w przypadku zmiany rozmiaru roli (oznacza to, że rozmiar maszyny wirtualnej, który jest hostem wystąpienia roli) lub liczby ról, każde wystąpienie roli (maszyny wirtualnej) muszą być ponownym utworzeniu obrazu i wszystkie lokalne dane zostaną utracone.

7. W przypadku ról usługi tylko jedno wystąpienie roli, wybrać **aktualizacji, nawet jeśli co najmniej jeden Rola zawiera pojedyncze wystąpienie pola wyboru** aktualizację kontynuować.

    Każda rola ma co najmniej dwa wystąpienia roli (maszyny wirtualne) Azure tylko można gwarantuje dostępność usługi 99,95% podczas aktualizacji usługi chmury. Umożliwiająca jednej maszyny wirtualnej do przetworzenia żądania klientów podczas innych aktualizacji.

8. Kliknij przycisk **OK** (zaznaczone), aby rozpocząć aktualizacji usługi.

## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Porady: Zamień wdrożeń, aby promować przemieszczanego wdrożenia do produkcji
Użyj **wymiany** wspieranie przemieszczania wdrożenia usługi w chmurze do środowiska produkcyjnego. Jeśli zdecydujesz się wdrożyć nową wersję usługi w chmurze, można przemieścić i przetestować z nową wersją w środowisku przemieszczania usługi chmury, gdy klienci korzystają z bieżącej wersji w środowisku produkcyjnym. Jeśli wszystko jest gotowe do promowania nową wersję w środowisku produkcyjnym, możesz użyć **wymiany** przełączyć adresów URL, według których są opisane dwa wdrożenia.

Można wymienić wdrożenia od **usługi w chmurze** strony lub pulpitu nawigacyjnego.

1. W [klasycznego portalu Azure](https://manage.windowsazure.com/), kliknij przycisk **usługi w chmurze**.
2. Na liście usługi w chmurze kliknij usługę w chmurze wybierz ją.
3. Kliknij przycisk **wymiany**.

    Otwiera następujące monit o potwierdzenie.

    ![Wymiany usługi w chmurze](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Po sprawdzeniu informacji wdrożenia, kliknij przycisk **tak** można zamienić wdrożenia.

    Wymiany wdrożenia odbywa się szybko ponieważ jedynym elementem, który zmienia się wirtualne adresy IP (VIP) dla wdrożenia.

    W celu ograniczenia kosztów obliczeń, można usunąć wdrożenia w środowisku przemieszczania po upewnieniu się, że nowe wdrożenia produkcyjnego działa zgodnie z oczekiwaniami.

### <a name="common-questions-about-swapping-deployments"></a>Często zadawane pytania dotyczące wymiany wdrożenia

**Jakie są wymagania wstępne dotyczące wymiany wdrożenia?**

Istnieją dwa kluczowe wymagania wstępne dotyczące wymiany pomyślne wdrożenie:

- Jeśli chcesz użyć statycznego adresu IP dla Twojego miejsca produkcji, należy zarezerwować jeden dla miejsca przemieszczania również. W przeciwnym razie wymiany zakończy się niepowodzeniem.

- Wszystkie wystąpienia ról musi być uruchomiona, zanim będzie można wykonać wymiany. Można sprawdzić stan swoich wystąpień w klasycznym portalu Azure lub za pomocą [polecenia Get-AzureRole w programie Windows PowerShell](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0).

Należy pamiętać, że aktualizacje systemu operacyjnego gościa i naprawianie operacji usługi powodują zamiany wdrażania zakończyć się niepowodzeniem. Zobacz [Rozwiązywanie problemów z wdrażaniem usługi chmury](cloud-services-troubleshoot-deployment-problems.md) więcej szczegółów.

**Zamiana wpływa negatywnie przestojów w przypadku mojej aplikacji? Jak należy ją obsługuje?**

Zgodnie z opisem w ostatniej sekcji, wymiany wdrożenia zwykle jest bardzo szybko, ponieważ jest on tylko zmiany konfiguracji usługi równoważenia obciążenia Azure. W niektórych przypadkach jednak może potrwać co najmniej 10 sekund i powodować błędy połączeń przejściowej. Aby ograniczyć wpływ na klientów, rozważ zaimplementowanie [Logika ponawiania klienta](../best-practices-retry-general.md).

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Porady: łączenie zasobu usługi w chmurze
Aby pokazać zależności między usługą w chmurze a innymi zasobami, możesz połączyć wystąpienie bazy danych Azure SQL Database lub konto magazynu z usługą chmurze. Możesz połączyć i odłączyć zasobów na **połączonych zasobów** strony, a następnie monitorować ich użycia na pulpicie nawigacyjnym usługi w chmurze. Jeśli monitorowanie włączona połączonym koncie magazynu, można monitorować całkowita liczba żądań na pulpicie nawigacyjnym usługi w chmurze.

Użyj **łącze** do połączenia nowej lub istniejącej bazy danych SQL wystąpienie magazynu konto lub do usługi w chmurze. Następnie można skalować bazy danych wraz z rolą usługi chmury, który jest używany na **skali** strony. (Konto magazynu skaluje automatycznie wraz ze wzrostem użycia.) Aby uzyskać więcej informacji, zobacz [jak skalować usługę w chmurze i połączonych zasobów](cloud-services-how-to-scale.md).

Możesz również można monitorować, zarządzanie i skalowania bazy danych w **baz danych** węzła klasycznego portalu Azure.

"Łączenie" zasób w tym sensie nie łączenie aplikacji do zasobu. W przypadku utworzenia nowej bazy za pomocą **łącze**, musisz dodać parametry połączenia w kodzie aplikacji, a następnie Uaktualnij usługę w chmurze. Należy również dodać parametry połączenia, jeśli aplikacja korzysta z zasobów w połączonym koncie magazynu.

W poniższej procedurze opisano, jak połączyć nowe wystąpienie bazy danych SQL wdrożona na nowym serwerze bazy danych SQL, aby usługa w chmurze.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>Aby połączyć wystąpienie bazy danych SQL z usługą w chmurze
1. W [klasycznego portalu Azure](http://manage.windowsazure.com/), kliknij przycisk **usługi w chmurze**. Następnie kliknij nazwę usługi w chmurze, aby otworzyć pulpit nawigacyjny.
2. Kliknij przycisk **połączone zasobów**.

    **Połączonych zasobów** zostanie otwarta strona.

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Kliknij opcję **Połącz zasób** lub **łącze**.

    **Zasób konsolidacji** zostanie uruchomiony Kreator.

    ![Strona łącze 1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Kliknij przycisk **utworzyć nowy zasób** lub **Połącz istniejący zasób**.
5. Wybierz typ zasobów do połączenia. W [klasycznego portalu Azure](http://manage.windowsazure.com/), kliknij przycisk **bazy danych SQL**. (Tylko klasyczny portal Azure obsługuje łączenie konta magazynu z usługą w chmurze).
6. Aby ukończyć konfigurację bazy danych, postępuj zgodnie z instrukcjami zawartymi w pomocy dla **baz danych SQL** części klasycznego portalu Azure.

    Możesz śledzić postęp operacji łączenia, w obszarze wiadomości.

    Po zakończeniu łączenia można monitorować stan zasobu połączonego na pulpicie nawigacyjnym usługi w chmurze. Aby uzyskać informacji na temat skalowania połączonej bazy danych SQL, zobacz [jak skalować usługę w chmurze i połączonych zasobów](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Aby odłączyć zasobu połączonego
1. W [klasycznego portalu Azure](http://manage.windowsazure.com/), kliknij przycisk **usługi w chmurze**. Następnie kliknij nazwę usługi w chmurze, aby otworzyć pulpit nawigacyjny.
2. Kliknij przycisk **połączonych zasobów**, a następnie wybierz zasób.
3. Kliknij przycisk **odłączyć**. Następnie kliknij przycisk **tak** przy wyświetlaniu monitu o potwierdzenie.

    Odłączanie bazy danych SQL nie ma wpływu na bazie danych lub połączeń z bazą danych aplikacji. Można nadal zarządzać bazy danych w **baz danych SQL** części klasycznego portalu Azure.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Porady: usuwanie wdrożeń i usługi w chmurze
Aby można było usunąć usługi w chmurze, należy usunąć poszczególnych istniejącego wdrożenia.

W celu ograniczenia kosztów obliczeń, możesz usunąć przemieszczania wdrożenia po sprawdzeniu, czy wdrożenia produkcyjnego działa zgodnie z oczekiwaniami. Jesteś kosztów rachunku obliczeniowe dla wystąpień roli, nawet jeśli nie jest uruchomiona usługa w chmurze.

Użyj poniższej procedury można usunąć wdrożenia lub usługi w chmurze.

1. W [klasycznego portalu Azure](http://manage.windowsazure.com/), kliknij przycisk **usługi w chmurze**.
2. Wybierz usługę w chmurze, a następnie kliknij przycisk **usunąć**. (Aby wybrać usługi w chmurze bez konieczności otwierania pulpitu nawigacyjnego, kliknij dowolne miejsce z wyjątkiem nazwy we wpisie usługi w chmurze).

    W przypadku wdrożenia w tymczasowym czy produkcyjnym, zobaczysz menu opcji podobny do następującego w dolnej części okna. Aby można było usunąć usługi w chmurze, należy usunąć wszystkie istniejące wdrożenia.

    ![Usuń Menu](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)

3. Aby usunąć wdrożenia, kliknij przycisk **usunąć wdrożenia produkcyjnego** lub **usunąć wdrożenie przemieszczania**. Następnie w oknie monitu o potwierdzenie kliknij przycisk **tak**.
4. Jeśli zamierzasz usunąć usługi w chmurze, powtórz krok 3, jeśli to konieczne, można usunąć inne wdrożenia.
5. Aby usunąć usługi w chmurze, kliknij przycisk **usługi w chmurze Delete**. Następnie w oknie monitu o potwierdzenie kliknij przycisk **tak**.

> [!NOTE]
> Jeśli pełne monitorowania jest skonfigurowany dla usługi w chmurze, platformy Azure nie powoduje usunięcia danych monitorowania z konta magazynu podczas usuwania usługi w chmurze. Należy ręcznie usunąć dane. Informacje o tym, gdzie można znaleźć w tabelach metryki, zobacz "jak: dostęp do pełnego monitorowania danych poza klasycznego portalu Azure" w [jak usługi w chmurze Monitor](cloud-services-how-to-monitor.md).
>
>

## <a name="next-steps"></a>Następne kroki
* [Konfiguracja ogólna usługi w chmurze](cloud-services-how-to-configure.md).
* Dowiedz się, jak [wdrażania usługi w chmurze](cloud-services-how-to-create-deploy.md).
* Skonfiguruj [niestandardowej nazwy domeny](cloud-services-custom-domain-name.md).
* Skonfiguruj [certyfikaty ssl](cloud-services-configure-ssl-certificate.md).
