---
title: "Wielodostępne obsługę replikacji maszyny Wirtualnej VMware do platformy Azure (dostawcy usług Kryptograficznych programu) | Dokumentacja firmy Microsoft"
description: "Opisuje sposób wdrażania usługi Azure Site Recovery w środowisku wielodostępnym do organizowania replikacji, trybu failover i odzyskiwania maszyn wirtualnych VMware lokalnymi (VM) na platformie Azure za pośrednictwem dostawcy usług Kryptograficznych programu przy użyciu portalu Azure"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: manayar
ms.openlocfilehash: 9db7e276fbbc064abe16cab2d2df668d2b1c8f7d
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="multi-tenant-support-in-azure-site-recovery-for-replicating-vmware-virtual-machines-to-azure-through-csp"></a>Obsługa wielu dzierżawców w usłudze Azure Site Recovery replikację maszyn wirtualnych VMware do platformy Azure za pośrednictwem dostawcy usług Kryptograficznych

Usługa Azure Site Recovery obsługuje środowiskach wielodostępnych subskrypcji dzierżawcy. Obsługuje ona również wielodostępność dla subskrypcji dzierżawcy, które są tworzone i zarządzane przez program Microsoft Cloud Solution Provider (CSP). W tym artykule szczegółowo wskazówki dotyczące wdrażania i zarządzania nimi wielodostępne scenariusze VMware do platformy Azure. Obejmuje ona również tworzenie i zarządzanie subskrypcjami dzierżawy za pośrednictwem dostawcy usług Kryptograficznych.

W tych wskazówkach pobiera silnie z istniejąca dokumentacja dotycząca replikację maszyn wirtualnych VMware do platformy Azure. Aby uzyskać więcej informacji, zobacz [maszyn wirtualnych VMware replikować do platformy Azure z usługą Site Recovery](site-recovery-vmware-to-azure.md).

## <a name="multi-tenant-environments"></a>Środowiska z wieloma dzierżawcami
Istnieją trzy główne modele wielodostępne:

* **Hosting usług dostawcy (HSP) udostępnionych**: partnera jest właścicielem infrastruktury fizycznej i używa zasobów udostępnionych (vCenter, centrów danych, magazynu fizycznego itd.) do obsługi maszyn wirtualnych wielu dzierżawców w tej samej infrastrukturze. Partner zapewniają odzyskiwania po awarii management jako usługa zarządzana lub dzierżawcy mogą być właścicielami odzyskiwania po awarii, jako rozwiązanie samoobsługi.

* **Dostawcy usług hostingu w wersji dedykowanej**: partnera jest właścicielem infrastruktury fizycznej, ale używa zasobów dedykowanych (wiele Vcenter, datastores fizycznych i tak dalej) do obsługi maszyn wirtualnych każdego dzierżawcy w osobnej infrastruktury. Partner zapewniają odzyskiwania po awarii management jako usługa zarządzana lub dzierżawcy mogą należeć go jako rozwiązaniem samoobsługi.

* **Zarządzane usługi dostawcy (MSP)**: klient jest właścicielem infrastruktury fizycznej, który jest hostem maszyny wirtualnej, a partnera umożliwia włączenie odzyskiwania po awarii i zarządzania.

## <a name="shared-hosting-multi-tenant-guidance"></a>Hosting udostępnione wskazówki wieloma dzierżawcami
W tej sekcji omówiono scenariusz udostępnionych hosting szczegółowo. Dwa scenariusze są podzbiorem scenariusz udostępnionych hosting i używają tych samych zasad. Różnice są opisane na końcu wskazówki udostępniane hosting.

Podstawowe wymagania w przypadku wielu dzierżawców jest odizolowanie różnych dzierżaw. Jeden dzierżawy nie może być można obserwować, co hosted innego dzierżawcę. W środowisku zarządzana przez partnera to wymaganie nie jest równie ważne, ponieważ jest on w środowisku samoobsługi, którym może mieć kluczowe znaczenie. We wskazówkach tych założono, że izolacji dzierżawców, jest wymagana.

Architektura jest przedstawiony na poniższym diagramie:

![Udostępniony HSP z jednego vCenter](./media/site-recovery-multi-tenant-support-vmware-using-csp/shared-hosting-scenario.png)  
**Hosting udostępnionych scenariusza z jednym vCenter**

Jak pokazano na powyższym diagramie, każdy klient ma na serwerze zarządzania oddzielne. Ta konfiguracja ogranicza dzierżawcy dostęp do maszyn wirtualnych specyficznego dla dzierżawy i umożliwia odizolowania dzierżawców. Scenariusz replikacji maszyn wirtualnych VMware używa serwer konfiguracji do zarządzania kontami odnajdywanie maszyn wirtualnych i instalowania agentów. Firma Microsoft, wykonaj te same zasady w środowiskach wielodostępnych, z uwzględnieniem ograniczenie odnajdywania maszyny Wirtualnej za pośrednictwem kontroli dostępu vCenter.

Wymaganie izolacji danych wymaga, że wszystkie informacje poufne infrastruktury (na przykład dostęp do poświadczeń) ujawnione dzierżawcom. Z tego powodu zaleca się pozostawienie wszystkie składniki serwera zarządzania, w obszarze wyłączną kontrolę partnera. Składniki serwera zarządzania są:
* Serwer konfiguracji (CS)
* Serwer przetwarzania (PS)
* Główny serwer docelowy (MT)

PS skalowalnego w poziomie jest również pod kontrolą partnera.

### <a name="every-cs-in-the-multi-tenant-scenario-uses-two-accounts"></a>Każdy CS w scenariuszu wielodostępne używa dwóch kont

- **konto dostępu do vCenter**: Odkryj maszyny wirtualne dzierżawców za pomocą tego konta. Ma uprawnienia dostępu vCenter przypisane do niej (zgodnie z opisem w następnej sekcji). Aby zapobiec przypadkowemu dostępu przecieki, zaleca się wprowadzenia przez partnerów tych samych poświadczeń narzędzia konfiguracji.

- **Konto dostępu do maszyny wirtualnej**: Użyj tego konta do zainstalowania agenta mobilności w ramach dzierżawy maszyn wirtualnych za pomocą wypychania automatycznego. Zazwyczaj jest kontem domeny, czy dzierżawcy może zawierać partnerowi także, czy też partnera może zarządzać bezpośrednio. Jeśli dzierżawa nie chcą udostępniać szczegóły partnera bezpośrednio, użytkownik można zezwolić na wprowadź poświadczenia do ograniczonej czasowo dostępu do CS lub, przy pomocy partnera, zainstaluj agentów mobilności ręcznie.

### <a name="requirements-for-a-vcenter-access-account"></a>Wymagania dotyczące konta dostępu do vCenter

Jak wspomniano w poprzedniej sekcji, należy skonfigurować CS z konta, które ma specjalne uprawnienia przypisane do niej. Przypisania roli należy zastosować do konta dostępu vCenter dla każdego obiektu vCenter i nie są propagowane do obiektów podrzędnych. Ta konfiguracja zapewnia izolacji dzierżawców, ponieważ propagacja dostępu może spowodować przypadkowe dostęp do innych obiektów.

![Propaguj opcji obiektów podrzędnych](./media/site-recovery-multi-tenant-support-vmware-using-csp/assign-permissions-without-propagation.png)

Alternatywą jest przypisanie konta użytkownika i roli obiektu centrum danych i propagować je do obiektów podrzędnych. Następnie nadaj kontu *dostępu* roli do wszystkich obiektów (np. maszyny wirtualne z innymi dzierżawcami) powinna być niedostępne dla konkretnego dzierżawy. Ta konfiguracja jest skomplikowane i udostępnia ona kontroli dostępu przypadkowe, ponieważ każdy nowy obiekt podrzędny automatycznie udzielono dostępu, który jest dziedziczone z obiektu nadrzędnego. Dlatego zaleca się, że używasz pierwszego podejścia.

Procedura dostępu do konta vCenter jest następująca:

1. Utwórz nową rolę przez klonowanie wstępnie zdefiniowanych *tylko do odczytu* roli, a następnie nadaj nazwę wygodny (na przykład Azure_Site_Recovery, jak pokazano w tym przykładzie).

2. Przypisz następujące uprawnienia do tej roli:

    * **Magazyn danych**: przydzielić miejsca, Magazyn danych przeglądania, operacje na plikach niskiego poziomu, usuń plik, pliki aktualizacji maszyny wirtualnej

    * **Sieci**: przypisywanie sieci

    * **Zasób**: przypisanie maszyny Wirtualnej do puli zasobów, migracji wyłączyć maszynę Wirtualną, migracji wyłączyć na maszynie Wirtualnej

    * **Zadania**: Tworzenie zadania, zadania aktualizacji

    * **Maszyna wirtualna**:
        * Konfiguracja > wszystkie
        * Interakcja > odpowiedzi na pytanie, połączenie z urządzeniem, skonfiguruj dysków CD, konfigurowanie dyskietka, wyłącz zasilanie, zainstaluj narzędzia VMware
        * Spis > Utwórz na podstawie istniejących, Utwórz nowy, zarejestruj, wyrejestrować
        * Inicjowanie obsługi administracyjnej > Zezwalaj na pobieranie maszyny wirtualnej, przekazywanie plików maszyny wirtualnej Zezwalaj
        * Zarządzanie migawkami > Usuń migawki

    ![Okno dialogowe Edytuj rolę](./media/site-recovery-multi-tenant-support-vmware-using-csp/edit-role-permissions.png)

3. Przypisz poziomy dostępu do konta vCenter (używane w dzierżawie CS) dla różnych obiektów, w następujący sposób:

>| Obiekt | Rola | Uwagi |
>| --- | --- | --- |
>| vCenter | Tylko do odczytu | Konieczne jest tylko dostęp vCenter do zarządzania różnych obiektów. Można usunąć to uprawnienie, jeśli konto nigdy nie będą dostarczane przez dzierżawcę lub używane dla wszystkich operacji zarządzania na serwerze vCenter. |
>| Centrum danych | Azure_Site_Recovery |  |
>| Hosta i hostów klastra. | Azure_Site_Recovery | Ponownie zapewnia, że dostęp jest na poziomie obiektu, dzięki czemu tylko dostępne hosty muszą dzierżawione maszyny wirtualne przed trybu failover i po powrotu po awarii. |
>| Magazyn danych i magazynu danych klastra | Azure_Site_Recovery | Taka sama jak poprzedzających. |
>| Sieć | Azure_Site_Recovery |  |
>| Serwer zarządzania | Azure_Site_Recovery | Obejmuje dostęp do wszystkich składników (CS PS i MT), jeśli są poza maszyny CS. |
>| Maszyny wirtualne dzierżawcy | Azure_Site_Recovery | Zapewnia, że dowolnej dzierżawy nowych maszyn wirtualnych dzierżawcy określonego także uzyskać dostęp, lub nie będzie wykrywalny za pośrednictwem portalu Azure. |

Dostęp do konta vCenter jest teraz ukończona. Ten krok spełnia wymagania minimalne uprawnienia, aby ukończyć operacji powrotu po awarii. Można również używać tych uprawnień dostępu z istniejących zasad. Po prostu zmodyfikować istniejące uprawnienia zawiera uprawnienia roli w kroku 2, szczegółowe wcześniej.

Aby ograniczyć operacje odzyskiwania po awarii do stanu trybu failover (oznacza to, bez możliwości powrotu po awarii), wykonaj poprzednią procedurę, z wyjątkiem: zamiast przypisywać *Azure_Site_Recovery* rolę vCenter konta dostępu należy przypisywać tylko *tylko do odczytu* roli do tego konta. Ten zestaw uprawnień pozwala na replikację maszyny Wirtualnej i trybu failover, a nie zezwala na powrót po awarii. Wszystkie inne poprzedniego procesu pozostaje jest. Do zapewnienia izolacji dzierżawy i ograniczyć odnajdywanie maszyny Wirtualnej, co uprawnienie jest ciągle przypisany tylko na poziomie obiektu i nie są propagowane do obiektów podrzędnych.

## <a name="other-multi-tenant-environments"></a>Innych środowiskach wielodostępnych

Poprzedniej sekcji opisano sposób konfigurowania środowiska z wieloma dzierżawami udostępnionego rozwiązania hostingu. Inne dwa główne rozwiązania są przeznaczone wyłącznie i zarządzane usługi hostingu. W poniższych sekcjach opisano architekturę tych rozwiązań.

### <a name="dedicated-hosting-solution"></a>Dedykowane rozwiązania do hostingu

Jak pokazano na poniższym diagramie, architektury różnica w dedykowane rozwiązania hostingu jest, że infrastruktura każdego dzierżawcy jest skonfigurowany dla tej dzierżawy tylko. Ponieważ dzierżaw są izolowane za pomocą osobnych Vcenter, dostawcy hostingu musi nadal zawiera procedura dostawcy usług Kryptograficznych dla udostępniona Usługa hostingu, ale nie trzeba martwić izolacji dzierżawców. Instalator dostawcy usług Kryptograficznych pozostaje niezmieniona.

![Architektura udostępnione hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/dedicated-hosting-scenario.png)  
**Dedykowany hostingu scenariusza z wieloma Vcenter**

### <a name="managed-service-solution"></a>Usługi zarządzane przez rozwiązanie

Jak pokazano na poniższym diagramie, architektury różnica w rozwiązaniu zarządzanych usług jest infrastruktury każdego dzierżawcy jest również fizycznie oddzielona od innych dzierżaw infrastruktury. W tym scenariuszu zwykle występuje, gdy dzierżawa jest właścicielem infrastruktury i chce dostawcę rozwiązania do zarządzania odzyskiwania po awarii. Ponownie ponieważ dzierżaw są fizycznie izolowane za pośrednictwem różnych infrastruktur, konieczność partnera postępuj zgodnie z instrukcjami dostawcy usług Kryptograficznych przewidzianych udostępniona Usługa hostingu, ale nie trzeba martwić izolacji dzierżawców. Inicjowanie obsługi administracyjnej dostawcy usług Kryptograficznych pozostaje niezmieniona.

![Architektura udostępnione hsp](./media/site-recovery-multi-tenant-support-vmware-using-csp/managed-service-scenario.png)  
**Zarządzane usługi scenariusza z wieloma Vcenter**

## <a name="csp-program-overview"></a>Omówienie programu dostawcy usług Kryptograficznych
[Dostawcy usług Kryptograficznych programu](https://partner.microsoft.com/en-US/cloud-solution-provider) zaznajomić razem lepiej scenariuszy, które oferują partnerów wszystkich usług chmurowych firmy Microsoft, w tym usługi Office 365, Enterprise Mobility Suite i Microsoft Azure. Z dostawcy usług Kryptograficznych naszych partnerów właścicielem end-to-end relacji z klientami i stają się punkt kontaktu relacji głównej. Partnerzy można wdrożyć subskrypcji platformy Azure dla klientów i łączenie subskrypcji z własnych wartości dodanej, dostosowane oferty.

Z usługą Azure Site Recovery partnerzy zarządzać kompletne rozwiązanie odzyskiwania po awarii dla klientów bezpośrednio za pośrednictwem dostawcy usług Kryptograficznych. Lub mogą przy użyciu dostawcy usług Kryptograficznych Konfigurowanie środowisk usługi Site Recovery i umożliwić klientom zarządzanie potrzeb odzyskiwania po awarii w sposób samoobsługi. W obu przypadkach partnerzy to łączności między odzyskiwania lokacji i klientów. Partnerzy relacji z klientem usługi i obciążania klientów do użycia usługi Site Recovery.

## <a name="create-and-manage-tenant-accounts"></a>Tworzenie i zarządzanie kontami dzierżawy

### <a name="step-0-prerequisite-check"></a>Krok 0: Sprawdzanie wymagań wstępnych

Wymagania wstępne maszyny Wirtualnej są takie same, jak opisano w [dokumentacji usługi Azure Site Recovery](site-recovery-vmware-to-azure.md). Oprócz tych wymagań wstępnych powinien mieć kontrolę dostępu wymienione wcześniej w miejscu przed przystąpieniem do zarządzania dzierżawy za pośrednictwem dostawcy usług Kryptograficznych. Dla każdego dzierżawcy należy utworzyć na serwerze zarządzania oddzielne, który może komunikować się z dzierżawą maszyn wirtualnych i vCenter partnera. Tylko partner ma prawa dostępu do tego serwera.

### <a name="step-1-create-a-tenant-account"></a>Krok 1: Tworzenie konta dzierżawy

1. Za pomocą [Microsoft Partner Center](https://partnercenter.microsoft.com/), zaloguj się do swojego konta dostawcy usług Kryptograficznych.

2. Na **pulpitu nawigacyjnego** menu, wybierz opcję **klientów**.

    ![Łącze klientów Centrum partnerskiego firmy Microsoft](./media/site-recovery-multi-tenant-support-vmware-using-csp/csp-dashboard-display.png)

3. Na stronie zostanie otwarty, kliknij przycisk **klienta Dodaj** przycisku.

    ![Przycisk Dodaj klienta](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-new-customer.png)

4. Na **nowego klienta** , wypełnij szczegółowe informacje dotyczące konta dzierżawy, a następnie kliknij przycisk **dalej: subskrypcji**.

    ![Strona informacje o koncie](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-add-filled.png)

5. Na stronie wybierania subskrypcji wybierz **Microsoft Azure** pole wyboru. Można dodać inne subskrypcje teraz lub w dowolnym momencie.

    ![Pole wyboru subskrypcji Microsoft Azure](./media/site-recovery-multi-tenant-support-vmware-using-csp/azure-subscription-selection.png)

6. Na **przeglądu** Potwierdź szczegóły dzierżawy, a następnie kliknij przycisk **przesyłania**.

    ![Strona przeglądu](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-summary-page.png)  

    Po utworzeniu konta dzierżawy, zostanie wyświetlona strona potwierdzenia, wyświetlanie szczegółów domyślnego konta i hasła dla tej subskrypcji.

7. Zapisz informacje i Zmień hasło później w razie potrzeby za pomocą usługi Azure logowania strony portalu.  

    Te informacje można udostępniać dzierżawy, ponieważ jest lub można tworzyć i udostępniać oddzielne konto, jeśli jest to konieczne.

### <a name="step-2-access-the-tenant-account"></a>Krok 2: Dostęp do konta dzierżawy

Subskrypcji dzierżawcy mogą dostęp za pośrednictwem pulpitu nawigacyjnego Centrum partnerskiego firmy Microsoft, zgodnie z opisem w temacie "krok 1: utworzenie konta dzierżawy."

1. Przejdź do **klientów** strony, a następnie kliknij nazwę konta dzierżawy.

2. Na **subskrypcje** strony konta dzierżawy, można monitorować istniejące subskrypcje konta i dodawać więcej subskrypcji, zgodnie z wymaganiami. Aby zarządzać dzierżawcy operacji odzyskiwania po awarii, wybierz **wszystkie zasoby (Azure portal)**.

    ![Łącze wszystkie zasoby](./media/site-recovery-multi-tenant-support-vmware-using-csp/all-resources-select.png)  

    Kliknięcie przycisku **wszystkie zasoby** udziela dostępu do subskrypcji platformy Azure dzierżawcy. Dostęp można sprawdzić, klikając łącze usługi Azure Active Directory u góry po prawej z portalu Azure.

    ![Azure Active Directory łącza](./media/site-recovery-multi-tenant-support-vmware-using-csp/aad-admin-display.png)

Można teraz wykonywać wszystkie operacje usługi site recovery dla dzierżawcy za pośrednictwem portalu Azure i zarządzanie operacjami odzyskiwania po awarii. Dostęp subskrypcji dzierżawcy za pośrednictwem dostawcy usług Kryptograficznych dla odzyskiwania po awarii zarządzanych, wykonaj proces opisany wcześniej.

### <a name="step-3-deploy-resources-to-the-tenant-subscription"></a>Krok 3: Wdrażanie zasobów do subskrypcji dzierżawcy
1. W portalu Azure Utwórz grupę zasobów, a następnie wdrożyć na zwykły proces magazynu usług odzyskiwania.

2. Pobierz klucz rejestracji magazynu.

3. Zarejestruj CS dla dzierżawcy przy użyciu klucza rejestracji magazynu.

4. Wprowadź poświadczenia dla konta dostępu dwóch: maszyny Wirtualnej i vCenter konto dostępu do uzyskania dostępu do konta.

    ![Kont serwera configuration Manager](./media/site-recovery-multi-tenant-support-vmware-using-csp/config-server-account-display.png)

### <a name="step-4-register-site-recovery-infrastructure-to-the-recovery-services-vault"></a>Krok 4: Zarejestruj Site Recovery infrastruktury w magazynie usług odzyskiwania
1. W portalu Azure w magazynie, który został utworzony wcześniej, należy zarejestrować serwer vCenter do CS, który został zarejestrowany w "krok 3: Wdrażanie zasobów w subskrypcji dzierżawcy." W tym celu należy użyć konta dostępu do vCenter.
2. Zakończ proces "Przygotowanie infrastruktury" Site Recovery na zwykły proces.
3. Maszyny wirtualne są teraz gotowe do replikacji. Sprawdź, czy tylko maszyny wirtualne dzierżawcy są wyświetlane na **wybierz maszyny wirtualne** bloku w obszarze **replikować** opcji.

    ![Lista maszyn wirtualnych dzierżawcy w bloku wybierz maszyny wirtualne](./media/site-recovery-multi-tenant-support-vmware-using-csp/tenant-vm-display.png)

### <a name="step-5-assign-tenant-access-to-the-subscription"></a>Krok 5: Przypisywanie dzierżawy dostępu do subskrypcji

Samoobsługowego odzyskiwania, podaj dzierżawcy szczegóły konta, jak wspomniano w kroku 6 procedury "krok 1: utworzenie konta dzierżawy" sekcji. Tę akcję można wykonać po partnera konfiguruje infrastruktury odzyskiwania po awarii. Czy typu odzyskiwania po awarii jest zarządzane lub samoobsługi, partnerzy muszą uzyskać dostęp do subskrypcji dzierżawcy za pośrednictwem portalu dostawcy usług Kryptograficznych. Konfigurowanie magazynu należących do partnera, a zarejestrować infrastruktury do subskrypcji dzierżawcy.

Partnerów można również dodać nowego użytkownika do subskrypcji dzierżawcy za pośrednictwem portalu dostawcy usług Kryptograficznych w następujący sposób:

1. Przejdź do strony subskrypcji CSP dzierżawcy, a następnie wybierz **użytkownicy i licencje** opcji.

    ![Stronę subskrypcji dzierżawcy dostawcy usług Kryptograficznych](./media/site-recovery-multi-tenant-support-vmware-using-csp/users-and-licences.png)

    Teraz można utworzyć nowego użytkownika, wprowadzając odpowiednie dane i wybierając uprawnienia lub przekazując listy użytkowników w pliku CSV.

2. Po utworzeniu nowego użytkownika, przejdź do portalu Azure, a następnie na **subskrypcji** bloku, wybierz odpowiednie subskrypcji.

3. W bloku, który zostanie otwarty, wybierz **kontroli dostępu (IAM)**, a następnie kliknij przycisk **Dodaj** można dodać użytkownika z poziomu odpowiedniego dostępu.      
    Użytkownicy, które zostały utworzone za pośrednictwem portalu dostawcy usług Kryptograficznych są automatycznie wyświetlane w bloku, który zostanie otwarty po kliknięciu poziom dostępu.

    ![Dodawanie użytkownika](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-user-subscription.png)

    Dla większości operacji zarządzania *współautora* roli jest wystarczająca. Użytkownicy z poziomu tego dostępu może robić wszystko to w przypadku subskrypcji, z wyjątkiem zmiana poziomów dostępu (dla którego *właściciela*— wymagany jest dostęp do poziomu). Można również dostosować poziomów dostępu zgodnie z wymaganiami.
