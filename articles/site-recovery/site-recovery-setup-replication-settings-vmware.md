---
title: "Konfigurowanie ustawień replikacji dla usługi Azure Site Recovery | Microsoft Docs"
description: "Opisuje sposób wdrożenia usługi Site Recovery w celu organizowania replikacji, pracy w trybie failover i odzyskiwania maszyn wirtualnych funkcji Hyper-V w chmurach VMM do platformy Azure."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: rayne-wiselman
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/19/2017
ms.author: sutalasi
translationtype: Human Translation
ms.sourcegitcommit: 74d51269cdd55e39eaa2963fec2b409decb47d0a
ms.openlocfilehash: a279fdc35c62c76e6f0858d52c64240032669f15


---
# <a name="manage-replication-policy-for-vmware-to-azure"></a>Zarządzanie zasadami replikacji oprogramowania VMware na platformie Azure


## <a name="create-a-new-replication-policy"></a>Tworzenie nowych zasad replikacji

1. W menu po lewej stronie kliknij pozycję „Zarządzaj -> Infrastruktura usługi Site Recovery”. 
2. Wybierz pozycję „Zasady replikacji” w sekcji „Oprogramowanie VMware i maszyny fizyczne”.
3. Wybierz pozycję „+Zasady replikacji” u góry.

    ![Tworzenie zasad replikacji](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. Wprowadź nazwę zasad.

5. W polu progu celu punktu odzyskiwania określ limit celu punktu odzyskiwania. Przekroczenie tego limitu przez replikację ciągłą będzie powodować generowanie alertów.
6. W obszarze Przechowywanie punktu odzyskiwania określ w godzinach, jak długie będzie okno przechowywania dla każdego punktu odzyskiwania. Chronione maszyny można odzyskać do dowolnego punktu w tym oknie. 

    > [!NOTE] 
    > W przypadku maszyn replikowanych do magazynu w warstwie Premium jest obsługiwany czas przechowywania do 24 godzin, natomiast w przypadku maszyn replikowanych do magazynu w warstwie Standardowa jest obsługiwany czas przechowywania do 72 godzin.
    
    > [!NOTE] 
    > Zostaną automatycznie utworzone zasady replikacji na potrzeby powrotu po awarii.

7. W obszarze Częstotliwość migawek spójności aplikacji określ, jak często (w minutach) będą tworzone punkty odzyskiwania zawierające migawki spójne z aplikacjami.

8. Kliknij przycisk OK. Tworzenie zasad powinno potrwać od około 30 sekund do 1 minuty.

![Tworzenie zasad replikacji](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-configuration-server-with-replication-policy"></a>Kojarzenie serwera konfiguracji z zasadami replikacji
1. Kliknij zasady replikacji, z którymi chcesz skojarzyć serwer konfiguracji.
2. Kliknij pozycję „Skojarz” u góry.
![Tworzenie zasad replikacji](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)

3. Z listy serwerów wybierz pozycję „Serwer konfiguracji”.
4. Kliknij przycisk OK. Kojarzenie serwera konfiguracji powinno potrwać od około 1 do 2 minut.

![Tworzenie zasad replikacji](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-replication-policy"></a>Edytowanie zasad replikacji
1. Kliknij zasady replikacji, dla których chcesz edytować ustawienia replikacji.
![Tworzenie zasad replikacji](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)

2. Kliknij pozycję „Edytuj ustawienia” u góry.
![Tworzenie zasad replikacji](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)

3. Zmień ustawienia zgodnie z Twoimi wymaganiami.
4. Kliknij pozycję „Zapisz” u góry. Zapisywanie zasad powinno potrwać od około 2 do 5 minut w zależności od tego, ile maszyn wirtualnych korzysta z tych zasad replikacji.

![Tworzenie zasad replikacji](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-configuration-server-from-replication-policy"></a>Usuwanie skojarzenia serwera konfiguracji z zasad replikacji
1. Kliknij zasady replikacji, z którymi chcesz skojarzyć serwer konfiguracji.
2. Kliknij pozycję „Usuń skojarzenie” u góry.
3. Z listy serwerów wybierz pozycję „Serwer konfiguracji”.
4. Kliknij przycisk OK. Usuwanie skojarzenia serwera konfiguracji powinno potrwać od około 1 do 2 minut.
    
    > [!NOTE] 
    > Nie można usunąć skojarzenia serwera konfiguracji, jeśli co najmniej jeden zreplikowany element korzysta z zasad. Przed usunięciem skojarzenia serwera konfiguracji upewnij się, że żadne zreplikowane elementy nie korzystają z zasad.

## <a name="delete-replication-policy"></a>Usuwanie zasad replikacji 

1. Kliknij zasady replikacji, które chcesz usunąć.
2. Kliknij pozycję Usuń. Usuwanie zasad powinno potrwać od około 30 sekund do 1 minuty.

    > [!NOTE] 
    > Nie można usunąć zasad replikacji, jeśli jest z nimi skojarzony co najmniej 1 serwer konfiguracji. Przed usunięciem zasad upewnij się, że żadne zreplikowane elementy nie korzystają z zasad, i usuń wszystkie skojarzone serwery konfiguracji.


<!--HONumber=Jan17_HO4-->


