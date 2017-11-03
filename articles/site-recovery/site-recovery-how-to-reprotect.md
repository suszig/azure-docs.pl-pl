---
title: "Włącz ponownie ochronę z platformy Azure do lokacji lokalnej | Dokumentacja firmy Microsoft"
description: "Po przełączeniu maszyn wirtualnych na platformie Azure można zainicjować powrotu po awarii, aby przywrócić maszyn wirtualnych do lokalnego. Dowiedz się, jak i włącz ponownie ochronę przed powrotu po awarii."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: 3644b41c3e3293a263bd9ff996d4e3d26417aeed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="reprotect-from-azure-to-an-on-premises-site"></a>Włącz ponownie ochronę z platformy Azure do lokacji lokalnej



## <a name="overview"></a>Omówienie
W tym artykule opisano, jak i włącz ponownie ochronę maszyn wirtualnych platformy Azure z platformy Azure do lokacji lokalnej. Wykonaj instrukcje w tym artykule, kiedy zechcesz się nie powieść z powrotem z maszyn wirtualnych VMware lub systemem Windows lub Linux, serwerów fizycznych, po zostały one przejścia w tryb failover z lokalnej lokacji do platformy Azure (zgodnie z opisem w [VMware replikować maszyny wirtualne i serwerów fizycznych do platformy Azure z usługą Azure Site Recovery](site-recovery-failover.md)).

> [!WARNING]
> Nie można wykonać powrotu po awarii po jednej [ukończone migracji](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), przenieść maszynę wirtualną do innej grupy zasobów lub usunąć maszyny wirtualnej platformy Azure. Wyłączenie ochrony maszyny wirtualnej, nie możesz powrotu po awarii.


Po zakończeniu przełączonej i replikowania chronionych maszyn wirtualnych, można zainicjować powrotu po awarii w przypadku maszyn wirtualnych, aby dostosować je do lokacji lokalnej.

> [!NOTE]
> Można tylko ponownej ochrony i powrotu po awarii do hosta ESXi. Nie można wykonać powrotu po awarii maszyny wirtualnej do hostów funkcji Hyper-v lub stacji roboczych VMware lub dowolnej platformie wirtualizacji.

Opublikuj komentarze lub pytania na końcu tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Aby szybko zapoznać Obejrzyj film następujący temat do trybu failover z platformy Azure do lokacji lokalnej.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="prerequisites"></a>Wymagania wstępne

> [!IMPORTANT]
> Podczas pracy w trybie failover na platformie Azure lokalnej lokacji może nie być dostępna i dlatego serwer konfiguracji mogą być dostępne Cofanie lub zamknięcie. Podczas ponownej ochrony i powrotu po awarii lokalnego serwera konfiguracji powinna być uruchomiona i jest niepodłączony OK.


Podczas przygotowywania Włącz ponownie ochronę maszyny wirtualnej, wykonać lub należy wziąć pod uwagę następujące akcje wymagań wstępnych:

* Jeśli serwer vCenter zarządza maszynami wirtualnymi, które chcesz powrócić po awarii, upewnij się, że masz [wymagane uprawnienia](site-recovery-vmware-to-azure-classic.md) odnajdywania maszyn wirtualnych na serwer vCenter.

  > [!WARNING]
  > Migawki są obecne na główny cel na lokalnym lub maszynie wirtualnej, ponowne włączenie ochrony kończy się niepowodzeniem. Przed kontynuowaniem Włącz ponownie ochronę, można usunąć migawek w głównym celu. Migawki maszyny wirtualnej są scalane automatycznie podczas wykonywania zadania ponownej ochrony.

* Przed powrotem nie, należy utworzyć dwa dodatkowe składniki:

  * **Serwer przetwarzania**: [serwera przetwarzania](site-recovery-vmware-setup-azure-ps-resource-manager.md) odbiera dane z chronioną maszynę wirtualną na platformie Azure i wysyła dane do lokacji lokalnej. Sieciach z małym opóźnieniami jest wymagany między serwerem przetwarzania i chronionej maszyny wirtualnej. W związku z tym może mieć lokalny serwer przetwarzania, jeśli używasz połączenia Azure ExpressRoute lub z serwera opartego na platformie Azure i sieci VPN.
  
  * **Główny serwer docelowy**: główny serwer docelowy odbiera dane powrotu po awarii. Serwer zarządzania lokalnymi utworzony ma głównego serwera docelowego instalowane domyślnie. Jednak w zależności od wielkości ruchu Wstecz nie powiodło się, konieczne może być tworzenie oddzielnych głównego serwera docelowego do powrotu po awarii.
    * [Maszyny wirtualnej systemu Linux wymaga Linux głównego serwera docelowego](site-recovery-how-to-install-linux-master-target.md).
    * Maszyny wirtualnej systemu Windows wymaga systemu Windows głównego serwera docelowego. Lokalny proces serwera i wzorzec komputerów docelowych można użyć ponownie.

    Główny element docelowy ma inne wymagania wstępne wymienione w [typowych czynności do wykonania w głównym celu przed ponownej ochrony](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server).

* Serwer konfiguracji jest wymagana lokalnymi przechodzenia wstecz. Podczas powrotu po awarii maszyna wirtualna musi istnieć w bazie danych konfiguracji serwera. W przeciwnym razie powrotu po awarii zakończy się niepowodzeniem. 

> [!IMPORTANT]
> Upewnij się, wykonanie zaplanowanego tworzenia kopii zapasowych serwera konfiguracji. W przypadku awarii, należy przywrócić działanie serwera przy użyciu tego samego adresu IP, dzięki czemu działa w przypadku powrotu po awarii.

* Ustaw `disk.EnableUUID=true` ustawienie parametrów konfiguracji maszyny wirtualnej główny serwer docelowy w środowisku programu VMware. Jeśli ten wiersz nie istnieje, dodaj ją. To ustawienie jest wymagane do zapewnia spójny UUID dysku maszyny wirtualnej (VMDK), dzięki czemu jego instaluje poprawnie.

* *Nie można użyć narzędzia Storage vMotion na głównym serwerze docelowym*. Może to spowodować powrót po awarii zakończyć się niepowodzeniem. Nie można uruchomić maszyny wirtualnej, ponieważ dyski nie są dostępne. Aby zapobiec temu problemowi, należy wykluczyć z listy vMotion serwery docelowe głównej.

* Dodaj nowy dysk do głównego serwera docelowego: dysk przechowywania. Dodaj nowy dysk i sformatuj dysk.


### <a name="frequently-asked-questions"></a>Często zadawane pytania

#### <a name="why-do-i-need-a-s2s-vpn-or-an-expressroute-connection-to-replicate-data-back-to-the-on-premises-site"></a>Dlaczego należy S2S sieci VPN lub połączenia ExpressRoute, do replikacji danych do lokacji lokalnej?
Replikacji z lokalnych do platformy Azure może zdarzyć się przez Internet lub połączenia ExpressRoute publicznej komunikacji równorzędnej, ponowne włączenie ochrony i powrotu po awarii wymaga sieci VPN lokacja lokacja (S2S) do replikacji danych. Podaj sieci, dzięki czemu maszyny wirtualne przełączona w tryb failover na platformie Azure ma dostęp (ping) konfiguracji lokalnego serwera. Można także wdrożyć serwera przetwarzania w sieci platformy Azure przełączona w tryb failover maszyny wirtualnej. Ten serwer przetwarzania również będą już mogli nawiązać połączenia z lokalnego serwera konfiguracji.

#### <a name="when-should-i-install-a-process-server-in-azure"></a>Podczas instalowania serwera przetwarzania na platformie Azure?


Maszyny wirtualne na platformie Azure, który chcesz włączyć ją ponownie wysyłanie danych replikacji do serwera przetwarzania. Konfigurowanie sieci tak, aby maszyny wirtualne na platformie Azure mogą uzyskać dostęp do procesu serwera.

Można wdrożyć serwer przetwarzania na platformie Azure lub użyć istniejącego serwera przetwarzania, który został użyty podczas pracy awaryjnej. Istotne wziąć pod uwagę jest czas oczekiwania do wysyłania danych z maszyn wirtualnych na platformie Azure do serwera przetwarzania.

Jeśli masz Konfigurowanie połączenia ExpressRoute, można użyć serwera przetwarzania lokalnego można wysłać danych, ponieważ brakuje opóźnienia między maszyną wirtualną a serwerem przetwarzania.

 ![Diagram architektury usługi ExpressRoute](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)



Jednak jeśli VPN S2S, zalecamy wdrożenie serwera przetwarzania na platformie Azure.

 ![Diagram architektury dla sieci VPN](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)


Należy pamiętać, że replikacja z platformy Azure do środowiska lokalnego może się zdarzyć, tylko za pośrednictwem połączenia VPN S2S lub prywatnej komunikacji równorzędnej ExpressRoute sieci. Upewnij się, że wystarczającą przepustowość jest dostępny za pośrednictwem tego kanału sieci.

Aby uzyskać informacje o instalowaniu serwera opartego na platformie Azure, zobacz [zarządzać serwerem procesów działających na platformie Azure](site-recovery-vmware-setup-azure-ps-resource-manager.md).

> [!TIP]
> Zalecamy używanie serwera opartego na platformie Azure podczas powrotu po awarii. Wydajność replikacji jest większe, jeśli serwer przetwarzania jest bliżej replikacji maszyny wirtualnej (maszyna przełączona w tryb failover na platformie Azure). Jednak podczas fazy weryfikacji koncepcji lub demonstracji, służy serwer przetwarzania lokalnego wraz z usługi ExpressRoute z prywatnej komunikacji równorzędnej przeprowadzenie weryfikacji Koncepcji szybsze.

> [!NOTE]
> Replikacja z lokalnego do platformy Azure może się zdarzyć tylko za pośrednictwem Internetu lub usługi ExpressRoute z publicznej komunikacji równorzędnej. Replikacja z platformy Azure do środowiska lokalnego może się zdarzyć tylko za pośrednictwem połączenia VPN S2S lub ExpressRoute z prywatnej komunikacji równorzędnej


#### <a name="what-ports-should-i-open-on-different-components-so-that-reprotection-can-work"></a>Jakie porty należy otworzyć w różnych składników, dzięki czemu można pracować przełączonej?

![Porty dla trybu failover i powrotu po awarii](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

#### <a name="which-master-target-server-should-i-use-for-reprotection"></a>Które główny serwer docelowy powinien używać przełączonej?
Lokalny główny serwer docelowy jest wymagany do odbierania danych z serwera przetwarzania, a następnie zapisać do pliku VMDK na lokalnej maszynie wirtualnej. W przypadku ochrony maszyn wirtualnych systemu Windows, należy Windows głównego serwera docelowego. Można użyć ponownie docelowego serwera i głównego procesu lokalnymi<!-- !todo component -->. Dla maszyn wirtualnych systemu Linux należy ustawić dodatkowych lokalnych Linux głównego celu.


Informacje o instalowaniu głównego serwera docelowego zobacz:

* [Jak zainstalować serwer główny serwer docelowy z systemem Windows](site-recovery-vmware-to-azure.md)
* [Jak zainstalować serwer główny cel systemu Linux](site-recovery-how-to-install-linux-master-target.md)


#### <a name="what-datastore-types-are-supported-on-the-on-premises-esxi-host-during-failback"></a>Jakie typy magazynu danych są obsługiwane na hoście ESXi lokalnych podczas powrotu po awarii?

Obecnie usługi Azure Site Recovery obsługuje awaria, powrót tylko do systemu plików maszyny wirtualnej (VMFS) lub sieci vSAN magazynu danych. Magazyn danych systemu plików NFS nie jest obsługiwane. Z powodu tego ograniczenia wybór magazynu danych wejściowych na ponownej ochrony ekranu jest puste w przypadku datastores systemu plików NFS lub pokazuje vSAN magazynu danych, ale nie powiedzie się podczas wykonywania zadania. Jeśli zamierzasz wykonaj powrót po awarii, można tworzyć VMFS magazyn danych lokalnych i powrót po awarii do jej. Pełnego pobierania VMDK spowoduje, że ta powrotu po awarii.

### <a name="common-things-to-check-after-completing-installation-of-the-master-target-server"></a>Typowe czynności do wykonania po zakończeniu instalacji główny serwer docelowy

* Jeśli maszyna wirtualna jest obecny lokalnie na serwerze vCenter, główny serwer docelowy wymaga dostępu do pliku VMDK na lokalnej maszynie wirtualnej. Wymagany jest dostęp do zapisu dysków maszyny wirtualnej replikowane dane. Upewnij się, że magazyn danych na lokalnej maszynie wirtualnej jest zainstalowany na hoście główny cel z dostępem do odczytu/zapisu.

* Jeśli maszyna wirtualna nie jest obecny lokalnie na serwerze vCenter, Usługa Site Recovery potrzebne do utworzenia nowej maszyny wirtualnej podczas zastosowania. Ta maszyna wirtualna jest tworzony na hoście ESX, na którym utworzyć głównego celu. Wybierz hosta ESX dokładnie, dzięki czemu powrotu po awarii maszyny wirtualnej jest tworzona na hoście, który ma.

* *Nie można użyć narzędzia Storage vMotion dla głównego serwera docelowego*. Może to spowodować powrót po awarii zakończyć się niepowodzeniem. Nie można uruchomić maszyny wirtualnej, ponieważ dyski nie są dostępne.

  > [!WARNING]
  > Jeśli główny cel ulega zadań vMotion magazynu po przełączonej, dysków chronionych maszyn wirtualnych, które są dołączone do głównego celu migracji w miejscu docelowym zadania vMotion. Jeśli spróbujesz się niepowodzeniem po tym, oderwania dysku nie powiedzie się, ponieważ nie znaleziono dysków. Następnie staje się on trudne do znalezienia dyski kont magazynu. Należy je znaleźć ręcznie i dołącz je do maszyny wirtualnej. Następnie można uruchomić na lokalnej maszynie wirtualnej.

* Dodaj dysk przechowywania do istniejącego serwera głównego celu systemu Windows. Dodaj nowy dysk i sformatuj dysk. Dysk przechowywania służy do zatrzymania punkty w czasie, gdy maszyny wirtualnej są replikowane do lokacji lokalnej. Poniżej przedstawiono niektóre kryteria dysk przechowywania. Bez tych kryteriów napędu nie są wyświetlane na głównym serwerze docelowym.

   * Wolumin nie jest używany do innych celów, takich jak element docelowy replikacji.

   * Wolumin nie jest w trybie blokady.

   * Wolumin nie jest woluminem pamięci podręcznej. Główny cel nie należy zainstalować na tym woluminie. Wolumin instalacji niestandardowej dla docelowego serwera i głównego procesu nie jest uprawniony do woluminu przechowywania. Gdy serwer przetwarzania i główny serwer docelowy są zainstalowane na woluminie, wolumin jest woluminu pamięci podręcznej głównego elementu docelowego.

   * Typ systemu plików woluminu nie jest FAT lub FAT32.

   * Pojemność woluminu jest różna od zera.

   * Wolumin przechowywania domyślne dla systemu Windows jest R.

   * Wolumin przechowywania domyślne dla systemu Linux jest /mnt/retention.

  > [!IMPORTANT]
  > Należy dodać nowy dysk, jeśli używasz istniejącej maszyny serwera konfiguracji serwera/procesu lub skali lub proces serwera/główną maszynę docelową, na serwerze. Nowy dysk powinna spełniać wymagania poprzedniej. Jeśli nie ma dysk przechowywania, nie zostanie wyświetlone na liście rozwijanej wyboru w portalu. Po dodaniu dysku do głównego celu lokalnymi potrzebny do 15 minut dysku pojawią się w zaznaczenia w portalu. Jeśli dysk nie jest wyświetlany po 15 minutach, można również odświeżyć serwera konfiguracji.

* Maszyny wirtualne systemu Linux przełączona w tryb failover wymaga Linux głównego serwera docelowego. Przełączona w tryb failover maszyny wirtualnej systemu Windows wymaga systemu Windows głównego serwera docelowego.

* Zainstaluj narzędzia VMware na głównym serwerze docelowym. Bez narzędzia VMware nie można wykryć datastores na hoście ESXi głównego celu.

* Włącz `disk.EnableUUID=true` parametru na maszynie wirtualnej główny serwer docelowy przy użyciu właściwości vCenter. <!-- !todo Needs link. -->

* Główny cel powinien mieć co najmniej jeden datastore VMFS dołączony. Jeśli brak, **Datastore** dane wejściowe na stronie ponownej ochrony jest pusta, i nie może kontynuować.

* Główny serwer docelowy nie może mieć migawek na dyskach. W przypadku migawki nie przełączonej i powrotu po awarii.

* Główny cel nie może mieć zdefiniowany kontroler Paravirtual SCSI. Kontroler może być tylko kontrolera LSI Logic. Bez kontrolera LSI Logic przełączonej kończy się niepowodzeniem.

<!--
### Failback policy
To replicate back to on-premises, you will need a failback policy. This policy get automatically created when you create a forward direction policy. Note that

1. This policy gets auto associated with the configuration server during creation.
2. This policy is not editable.
3. The set values of the policy are (RPO Threshold = 15 Mins, Recovery Point Retention = 24 Hours, App Consistency Snapshot Frequency = 60 Mins)
   ![](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

-->

## <a name="steps-to-reprotect"></a>Kroki, aby Wznów

> [!NOTE]
> Po uruchomieniu maszyny wirtualnej na platformie Azure, dopiero po pewnym czasie dla agenta zarejestrować serwer konfiguracji (maksymalnie 15 minut). W tym czasie ponowne włączenie ochrony kończy się niepowodzeniem, a komunikat o błędzie wskazuje, że agent nie jest zainstalowany. Poczekaj kilka minut, a następnie spróbuj ponownie zastosowania.



1. W **magazynu** > **elementy replikowane**, kliknij prawym przyciskiem myszy maszynę wirtualną, która jest Failover, a następnie wybierz **ponownego włączenia ochrony**. Możesz również kliknij maszynę i wybierz **ponownego włączenia ochrony** z przyciski poleceń.
2. W bloku, zwróć uwagę, że kierunek ochrony, **Azure do środowiska lokalnego**, jest już wybrana.
3. W **główny serwer docelowy** i **serwera przetwarzania**, wybierz lokalny główny serwer docelowy i serwera przetwarzania.
4. Aby uzyskać **Datastore**, wybierz magazyn danych, do której chcesz odzyskać dyski lokalną. Ta opcja jest używana, gdy na lokalnej maszynie wirtualnej zostanie usunięta, a następnie musisz utworzyć nowe dyski. Ta opcja jest ignorowana, jeśli dyski już istnieje, ale nadal należy określić wartość.
5. Wybierz dysk przechowywania.
6. Zasady powrotu po awarii jest automatycznie wybierany.
7. Kliknij przycisk **OK** zacząć zastosowania. Zadanie rozpoczyna się replikacja maszyny wirtualnej z platformy Azure do lokacji lokalnej. Postęp można śledzić na **zadania** kartę.

Jeśli chcesz odzyskać do lokalizacji alternatywnej (po usunięciu na lokalnej maszynie wirtualnej), wybierz dysk przechowywania i magazynu danych skonfigurowanego dla głównego serwera docelowego. Gdy użytkownik powrót po awarii do lokacji lokalnej, w przypadku powrotu po awarii plan ochrony maszyn wirtualnych VMware Użyj tego samego magazynu danych jako główny serwer docelowy. W programie vCenter zostanie utworzona nowa maszyna wirtualna.

Jeśli chcesz odzyskać maszynę wirtualną na platformie Azure do istniejących na lokalnej maszynie wirtualnej Zainstaluj datastores na lokalnej maszynie wirtualnej z dostępem do odczytu/zapisu na hoście ESXi serwera głównego celu.
    ![Okno dialogowe ponownego włączenia ochrony](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

Można również Obejmij ochroną na poziomie planu odzyskiwania. Grupa replikacji może przełączona do trybu tylko do planu odzyskiwania. Gdy użytkownik zacznij ponownie chronić przy użyciu planu odzyskiwania, musisz podać wartości dla każdego chronionego komputera.

> [!NOTE]
> Użyj tego samego serwera głównego celu włączyć ją ponownie do grupy replikacji. Użycie innego głównego serwera docelowego do Włącz ponownie ochronę grupy replikacji, serwer nie może dostarczyć wspólnego punktu w czasie.

> [!NOTE]
> Na lokalnej maszynie wirtualnej jest wyłączony podczas zastosowania. Pomaga to zapewnić spójność danych podczas replikacji. Nie należy włączać maszyny wirtualnej, po zakończeniu zastosowania.

Po przełączonej zakończy się powodzeniem, maszyny wirtualnej przechodzą w stan chronionych.

## <a name="next-steps"></a>Następne kroki

Po wprowadzeniu chronionych stan maszyny wirtualnej można [zainicjować powrotu po awarii](site-recovery-how-to-failback-azure-to-vmware.md#steps-to-fail-back). 

Powrót po awarii Zamknij maszynę wirtualną na platformie Azure, a następnie uruchomić na lokalnej maszynie wirtualnej. Oczekiwane przestój dla aplikacji. Wybierz czas powrotu po awarii, gdy aplikacja może tolerować przestoju.

## <a name="common-problems"></a>Typowe problemy

* Jeśli szablon jest używany do tworzenia maszyn wirtualnych, upewnij się, że każda maszyna wirtualna ma własny identyfikator UUID dysków. Identyfikator UUID na lokalnej maszynie wirtualnej powoduje konflikt z z głównego celu, ponieważ zarówno utworzone przy użyciu tego samego szablonu, ponowne włączenie ochrony kończy się niepowodzeniem. Wdróż innego główny cel, który nie został utworzony z tego samego szablonu.

* Jeśli odnajdywanie vCenter użytkownika tylko do odczytu i ochrony maszyn wirtualnych, ochrona zakończy się pomyślnie i tryb failover działa prawidłowo. Podczas zastosowania trybu failover nie powiedzie się, ponieważ nie można odnaleźć datastores. Objawem jest, że datastores nie są wyświetlane podczas zastosowania. Aby rozwiązać ten problem, można zaktualizować poświadczeń vCenter przy użyciu odpowiedniego konta, które ma uprawnienia i spróbuj ponownie wykonać zadanie. Aby uzyskać więcej informacji, zobacz [VMware replikowanie maszyn wirtualnych i serwerów fizycznych do platformy Azure z usługą Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).

* Po wykonaj powrót po awarii maszyny wirtualnej systemu Linux i uruchomić ją lokalnie, można zobaczyć, że pakiet Menedżera sieci został odinstalowany z komputera. Ta dezinstalacja odbywa się, ponieważ pakiet Menedżera sieci jest usuwany po odzyskaniu maszyna wirtualna na platformie Azure.

* Maszyny wirtualnej systemu Linux jest skonfigurowane za pomocą statycznego adresu IP i przeszła w tryb failover na platformie Azure, adres IP są uzyskiwane z serwera DHCP. Gdy przełączyć się na lokalnym maszyny wirtualnej w dalszym ciągu używać protokołu DHCP w celu pobrania adresu IP. Ręcznie Zaloguj się do komputera, a ustawienie adresu IP do statycznego adresu w razie potrzeby. Maszyny wirtualnej systemu Windows można ponownie uzyskać jego statyczny adres IP.

* Jeśli używany jest bezpłatna wersja ESXi 5.5 lub bezpłatna wersja funkcji Hypervisor vSphere 6, trybu failover zakończy się pomyślnie, ale powrotu po awarii nie powiedzie się. Aby umożliwić powrót po awarii, uaktualnienie do licencji ewaluacyjnej albo program.

* Jeśli nie można osiągnąć serwera konfiguracji z serwera przetwarzania, należy użyć Telnet, aby sprawdzić połączenie z serwerem konfiguracji na porcie 443. Można też spróbować wykonać polecenie ping z serwera przetwarzania serwera konfiguracji. Serwer przetwarzania powinny mieć również pulsu, gdy jest podłączone do serwera konfiguracji.

* Jeśli próbujesz powrót po awarii do alternatywnego vCenter, upewnij się, że Twoje nowe vCenter i główny serwer docelowy są wykrywane. Typowe objaw jest, że datastores nie są dostępne lub widoczne w **Wznów** okno dialogowe.

* Serwer systemu Windows Server 2008 R2 z dodatkiem SP1, który jest chroniony jako serwer fizyczny lokalnymi nie nieudane platformy Azure z lokacjami lokalnymi.

### <a name="common-error-codes"></a>Typowe kody błędów

#### <a name="error-code-95226"></a>Kod błędu 95226

*Ponownej ochrony nie powiodła się, ponieważ maszyny wirtualnej platformy Azure nie mógł uzyskać dostęp do lokalnego serwera konfiguracji.*

Dzieje się tak podczas 
1. Maszyny wirtualnej platformy Azure nie może skontaktować się z lokalnego serwera konfiguracji i dlatego nie można być wykryte i zarejestrowany na serwerze konfiguracji. 
2. Usługa InMage Scout aplikacji na maszynie wirtualnej Azure, która musi działać do komunikowania się do lokalnego serwera konfiguracji może nie działać po pracy awaryjnej.

Aby rozwiązać ten problem
1. Należy się upewnić, że sieć maszyny wirtualnej platformy Azure jest skonfigurowany w taki sposób, że maszyna wirtualna może się komunikować z lokalnego serwera konfiguracji. W tym celu należy skonfigurować sieci VPN między lokacjami do lokalnego centrum danych lub skonfiguruj połączenie ExpressRoute z prywatnej komunikacji równorzędnej w sieci wirtualnej platformy Azure maszyny wirtualnej. 
2. Jeśli masz już skonfigurowane w taki sposób, że maszyny wirtualne Azure mogą komunikować się z lokalnego serwera konfiguracji sieci, następnie zaloguj się do maszyny wirtualnej i sprawdź "Usługa aplikacji InMage Scout". Jeśli zauważysz, że nie jest uruchomiona usługa aplikacji InMage Scout ręcznie uruchom usługę i sprawdź, czy typ uruchamiania usługi jest ustawiony na automatyczny.

### <a name="error-code-78052"></a>Kod błędu 78052
Ponownej ochrony kończy się niepowodzeniem z komunikatem o błędzie: *maszyny wirtualnej nie można ukończyć włączania ochrony.*

Może to nastąpić z dwóch przyczyn
1. Maszyny wirtualnej, które są ponownej ochrony jest Windows Server 2016. Aktualnie ten system operacyjny nie jest obsługiwana w przypadku powrotu po awarii, ale będzie wkrótce obsługiwany.
2. Istnieje już maszyny wirtualnej o tej samej nazwie na głównym serwerze docelowym kończy się niepowodzeniem do.

Aby rozwiązać ten problem można wybrać innego głównego serwera docelowego na innego hosta, dzięki czemu ponownej ochrony utworzy komputera na inny host, gdzie nie powodują konfliktu nazw. Można również vMotion główny cel do innego hosta, na którym nie nastąpi kolizję nazw. Jeśli istniejącej maszyny wirtualnej jest stray maszyny, można po prostu ją zmienić tak, aby na tym samym hoście ESXi można utworzyć nowej maszyny wirtualnej.

### <a name="error-code-78093"></a>Kod błędu 78093

*Maszyna wirtualna nie jest uruchomiona, zawiesić lub jest niedostępny.*

Włącz ponownie ochronę nieudanej przez maszynę wirtualną do lokalnej, należy uruchamiania maszyny wirtualnej platformy Azure. Jest to, aby usługa mobilności rejestruje z serwerem konfiguracji lokalnych i rozpocząć replikację komunikując się z serwerem przetwarzania. Jeśli komputer jest niepoprawna sieci lub nie działa (zawiesić lub zamknięcie), serwer konfiguracji nie można osiągnąć usługi mobilności na maszynie wirtualnej, aby rozpocząć ponownej ochrony. Można ponownie uruchomić maszynę wirtualną tak, aby umożliwić komunikację wstecz lokalnymi. Uruchom ponownie zadanie ponownej ochrony po uruchomieniu maszyny wirtualnej platformy Azure

### <a name="error-code-8061"></a>Kod błędu 8061

*Magazyn danych jest niedostępny z hosta ESXi.*

Zapoznaj się [wzorca wstępne docelowej](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server) i [obsługuje datastores](site-recovery-how-to-reprotect.md#what-datastore-types-are-supported-on-the-on-premises-esxi-host-during-failback) powrotu po awarii

