<properties
    pageTitle="Tworzenie oferty w usłudze Azure Stack | Microsoft Azure"
    description="Jako administrator usługi dowiedz się, jak utworzyć ofertę dla dzierżawców w usłudze Azure Stack."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/25/2016"
    ms.author="erikje"/>


# Tworzenie oferty w usłudze Azure Stack

[Oferty](azure-stack-key-features.md#services-plans-offers-and-subscriptions) to grupy obejmujące co najmniej jeden plan, które dostawcy przedstawiają dzierżawcom do zasubskrybowania (zakupienia). W tym przykładzie zostanie utworzona oferta obejmująca [plan utworzony](azure-stack-create-plan.md) w ostatnim kroku. Umożliwi to subskrybentom tej oferty aprowizację maszyn wirtualnych.

1.  [Zaloguj się](azure-stack-connect-azure-stack.md#log-in-as-a-service-administrator) w witrynie portalu jako administrator usługi.
    ![](media/azure-stack-create-offer/image1.png)

2.  Kliknij przycisk **Nowy**.

3.  Kliknij pozycję **Oferty i plany dzierżawy**, następnie kliknij pozycję **Oferta**.
    ![](media/azure-stack-create-offer/image2.png)

4.  W bloku **Nowa oferta** wykonaj następujące czynności:

    1.  Wypełnij pola **Nazwa wyświetlana** i **Nazwa zasobu**. Nazwa wyświetlana jest przyjazną nazwą oferty. Nazwa zasobu jest widoczna tylko dla administratora. Jest to nazwa używana przez administratorów do pracy z ofertą jako zasobem usługi Azure Resource Manager.

    2.  Wybierz nową lub istniejącą **grupę zasobów**.

        ![](media/azure-stack-create-offer/image3.png)

5.  Kliknij pozycję **Plany bazowe** i w bloku **Plan** wybierz plany, które chcesz uwzględnić w ofercie, a następnie kliknij przycisk **Wybierz**. Kliknij pozycję **Utwórz**, aby utworzyć ofertę.

    ![](media/azure-stack-create-offer/image4.png)

6.  Kliknij pozycję **Zmień stan**, a następnie kliknij pozycję **Publiczne**.
Plany i oferty muszą być publiczne, aby dzierżawcy mieli ich pełny widok podczas subskrybowania. Jeśli plan jest prywatny, a oferta publiczna, dzierżawcy będą mogli uzyskać dostęp do oferty, ale nie będą mogli wyświetlić szczegółów planu. Plany i oferty mogą być:

    -   **Publiczne**: widoczne dla dzierżawców.

    -   **Prywatne**: widoczne tylko dla administratorów usługi. Ustawienie przydatne podczas opracowywania planu lub oferty, lub jeśli administrator usługi chce zatwierdzać każdą subskrypcję.

    -   **Zlikwidowane**: zamknięte dla nowych subskrybentów. Administrator usługi może użyć stanu Zlikwidowane, aby uniemożliwić przyszłe subskrypcje, ale pozostawić bieżących subskrybentów bez zmian.

    ![](media/azure-stack-create-offer/image6.png)

Zmiany w planie lub ofercie nie są natychmiast widoczne dla dzierżawcy. Aby zmiany stały się widoczne, subskrypcja musi być zsynchronizowana, a następnie dzierżawca musi odświeżyć portal lub wylogować się/zalogować ponownie.

Nawet kiedy dodatkowa subskrypcja została utworzona i jest zsynchronizowana, może być konieczne wylogowanie/zalogowanie się, aby zobaczyć nową subskrypcję w selektorze subskrypcji podczas tworzenia nowych zasobów lub grup zasobów.

## Następne kroki

[Subskrybowanie oferty i aprowizacja maszyny wirtualnej](azure-stack-subscribe-plan-provision-vm.md)



<!--HONumber=Sep16_HO3-->


