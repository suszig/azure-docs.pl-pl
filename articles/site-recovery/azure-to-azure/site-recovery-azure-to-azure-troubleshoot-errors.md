---
title: "Usługa Azure Site Recovery Rozwiązywanie problemów z błędów i problemów z replikacją Azure do platformy Azure | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów z błędów i problemów podczas replikowania maszyn wirtualnych platformy Azure dla odzyskiwania po awarii"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: sujayt
ms.openlocfilehash: 7231c5d58dfd2d6eddd40e9003b63206f4c7f859
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Rozwiązywanie problemów z replikacją maszyn wirtualnych Azure do platformy Azure

W tym artykule opisano typowych problemów w usłudze Azure Site Recovery podczas replikacji i odzyskiwania maszyn wirtualnych platformy Azure z jednego regionu do innego regionu oraz opisano sposoby ich rozwiązywania. Aby uzyskać więcej informacji o obsługiwanych konfiguracjach, zobacz [macierz obsługi replikowania maszyn wirtualnych platformy Azure](site-recovery-support-matrix-azure-to-azure.md).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Problemy przydziału zasobów platformy Azure (kod błędu 150097)
Subskrypcja powinny zostać włączone do tworzenia maszyn wirtualnych platformy Azure w region docelowy, który ma być używany jako regionu odzyskiwania po awarii. Ponadto subskrypcji powinny mieć wystarczający przydział umożliwia tworzenie maszyn wirtualnych o określonym rozmiarze. Domyślnie usługa Site Recovery wybiera ten sam rozmiar dla docelowej maszyny Wirtualnej jako źródło maszyny Wirtualnej. Jeśli dopasowania rozmiaru nie jest dostępna, najbliższy rozmiar możliwe jest pobierany automatycznie. Jeśli rozmiar pasującego obsługującego źródła konfiguracji maszyny Wirtualnej nie jest, zostanie wyświetlony ten komunikat o błędzie:

**Kod błędu:** | **Możliwe przyczyny** | **Zalecenia**
--- | --- | ---
150097<br></br>**Komunikat**: nie można włączyć replikacji dla maszyny wirtualnej VmName. | — Identyfikator subskrypcji nie może być włączone do utworzenia żadnej maszyny wirtualnej w miejscu docelowym regionie.</br></br>— Identyfikator subskrypcji nie może być włączone lub nie ma wystarczającego przydziału do utworzenia określonych rozmiarów maszyn wirtualnych w miejscu docelowym regionie.</br></br>-Odpowiedniego elementu docelowego rozmiar maszyny Wirtualnej, odpowiadający źródła liczba kart interfejsu Sieciowego maszyny Wirtualnej (2) nie zostanie znaleziona dla Identyfikatora subskrypcji w lokalizacji docelowej regionu.| Skontaktuj się z [Azure pomocy dotyczącej rozliczeń](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) umożliwiające tworzenie maszyny Wirtualnej wymagana rozmiarów maszyn wirtualnych w lokalizacji docelowej dla Twojej subskrypcji. Po włączeniu, ponów operację zakończoną niepowodzeniem.

### <a name="fix-the-problem"></a>Rozwiąż problem
Możesz skontaktować się [Azure pomocy dotyczącej rozliczeń](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) umożliwiające tworzenie maszyn wirtualnych o wymaganym rozmiarze w lokalizacji docelowej dla swojej subskrypcji.

Jeśli lokalizacja docelowa ma ograniczenie pojemności, wyłącz replikację i włącz ją w innej lokalizacji, gdzie subskrypcji ma wystarczającego przydziału do tworzenia maszyn wirtualnych z wymaganym rozmiarze.

## <a name="trusted-root-certificates-error-code-151066"></a>Zaufane certyfikaty główne (kod błędu 151066)

Jeśli wszystkie najnowsze zaufanych certyfikatów głównych nie są dostępne na maszynie Wirtualnej, zadanie "Włącz replikację" może zakończyć się niepowodzeniem. Bez certyfikatów uwierzytelniania i autoryzacji wywołań usługi Site Recovery z maszyny Wirtualnej się nie powieść. Zostanie wyświetlony komunikat o błędzie dotyczący nieudanego zadania usługi Site Recovery "Włącz replikację":

**Kod błędu:** | **Możliwa przyczyna** | **Zalecenia**
--- | --- | ---
151066<br></br>**Komunikat**: Konfiguracja usługi Site Recovery nie powiodła się. | Wymagane zaufanych certyfikatów głównych używany do autoryzacji i uwierzytelniania nie są dostępne na komputerze. | — Dla maszyny Wirtualnej z systemem operacyjnym Windows upewnij się, że zaufanych certyfikatów głównych znajdują się na komputerze. Aby uzyskać informacje, zobacz [Konfigurowanie zaufanych certyfikatów głównych i certyfikatów niedozwolonych](https://technet.microsoft.com/library/dn265983.aspx).<br></br>— Dla maszyny Wirtualnej z systemem operacyjnym Linux postępuj zgodnie ze wskazówkami dla zaufanych certyfikatów głównych opublikowanych przez dystrybutora wersji systemu operacyjnego Linux.

### <a name="fix-the-problem"></a>Rozwiąż problem
**Windows**

Zainstalować wszystkie najnowsze aktualizacje systemu Windows na maszynie Wirtualnej tak, aby wszystkie zaufane certyfikaty główne są obecne na komputerze. Jeśli pracujesz w środowisku bez połączenia, należy wykonać standardowego procesu aktualizacji systemu Windows w Twojej organizacji, aby uzyskać certyfikaty. Jeśli wymagane certyfikaty nie są dostępne na maszynie Wirtualnej, połączenia z usługą Site Recovery nie ze względów bezpieczeństwa.

Wykonaj typowe Windows update zarządzania lub certyfikat proces zarządzania aktualizacjami w organizacji, aby uzyskać najnowsze certyfikaty główne i listy odwołania certyfikatów zaktualizowane na maszynach wirtualnych.

Aby sprawdzić, czy problem został rozwiązany, przejdź do login.microsoftonline.com z przeglądarki w maszynie Wirtualnej.

**Linux**

Postępuj zgodnie ze wskazówkami pochodzącymi z dystrybutora Linux można pobrać najnowsze zaufanych certyfikatów głównych i najnowszej listy odwołania certyfikatów na maszynie Wirtualnej.

Ponieważ SuSE Linux używa łączy symbolicznych, aby zachować lista certyfikatów, wykonaj następujące kroki:

1.  Zaloguj się jako użytkownik root.

2.  Uruchom następujące polecenie:

      ``# cd /etc/ssl/certs``

3.  Aby sprawdzić, czy certyfikat firmy Symantec głównego urzędu certyfikacji znajduje się, uruchom następujące polecenie:

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4.  Jeśli plik nie zostanie odnaleziony, uruchom następujące polecenia:

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

      ``# c_rehash``

5.  Aby utworzyć łącza symbolicznego z b204d74a.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem, uruchom to polecenie:

      ``# ln -s  VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

6.  Sprawdź, czy następujące dane wyjściowe tego polecenia. Jeśli nie, należy utworzyć łącza symbolicznego:

      ``# ls -l | grep Baltimore
      -rw-r--r-- 1 root root   1303 Apr  7  2016 Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 04:47 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 05:01 653b494a.0 -> Baltimore_CyberTrust_Root.pem``

7. Jeśli 653b494a.0 łącza symbolicznego nie jest obecny, użyj tego polecenia, aby utworzyć łącza symbolicznego:

      ``# ln -s Baltimore_CyberTrust_Root.pem 653b494a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Łączność wychodząca dla zakresów adresów URL odzyskiwania lokacji lub adres IP (kod błędu 151037 lub 151072)

Replikacja usługi Site Recovery do pracy, łączność wychodząca określonych adresów URL lub adres IP zakresów wymagany jest z maszyny Wirtualnej. Jeśli maszyna wirtualna znajduje się za zaporą, używa reguł grupa zabezpieczeń sieci do kontrolowania łączność wychodząca może pojawić się jeden z tych komunikatów o błędzie:

**Kody błędów** | **Możliwe przyczyny** | **Zalecenia**
--- | --- | ---
151037<br></br>**Komunikat**: nie można zarejestrować maszyny wirtualnej platformy Azure z usługą Site Recovery. | -Używasz NSG do kontrolowania dostępu ruchu wychodzącego na Maszynie wirtualnej i wymagany adres IP nie są białej dla wychodzących do zakresów.</br></br>-Używasz narzędzia zapory innych firm i nie są wymagane zakresy IP/URL białej.</br>| — Jeśli używasz zapory serwera proxy do kontrolowania łączności sieciowej wychodzących na maszynie Wirtualnej, upewnij się, że adresy URL wymagań wstępnych lub zakresy IP centrum danych są białej. Aby uzyskać informacje, zobacz [zapory wskazówki dotyczące serwera proxy](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>— Jeśli używasz reguły NSG do sterowania łączności sieciowej wychodzących na maszynie Wirtualnej, upewnij się, że zakresy IP centrum danych wymagań wstępnych białej. Aby uzyskać informacje, zobacz [wskazówki dotyczące grupy zabezpieczeń sieci](https://aka.ms/a2a-nsg-guidance).
151072<br></br>**Komunikat**: Konfiguracja usługi Site Recovery nie powiodła się. | Nie można ustanowić połączenia z punktami końcowymi usługi Site Recovery. | — Jeśli używasz zapory serwera proxy do kontrolowania łączności sieciowej wychodzących na maszynie Wirtualnej, upewnij się, że adresy URL wymagań wstępnych lub zakresy IP centrum danych są białej. Aby uzyskać informacje, zobacz [zapory wskazówki dotyczące serwera proxy](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>— Jeśli używasz reguły NSG do sterowania łączności sieciowej wychodzących na maszynie Wirtualnej, upewnij się, że zakresy IP centrum danych wymagań wstępnych białej. Aby uzyskać informacje, zobacz [wskazówki dotyczące grupy zabezpieczeń sieci](https://aka.ms/a2a-nsg-guidance).

### <a name="fix-the-problem"></a>Rozwiąż problem
Do listy dozwolonych [odpowiednie adresy URL](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-urls) lub [wymagane zakresy IP](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-ip-ranges), postępuj zgodnie z instrukcjami [sieci wytycznych](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Nie znaleziono na komputerze (kod błędu 150039) dysku

Nowy dysk dołączony do maszyny Wirtualnej musi zostać zainicjowany.

**Kod błędu:** | **Możliwe przyczyny** | **Zalecenia**
--- | --- | ---
150039<br></br>**Komunikat**: dysk danych platformy Azure (DiskName) (DiskURI) numer jednostki logicznej (LUN) (LUNValue) nie został zamapowany do odpowiedniego dysku zgłaszane z poziomu maszyny Wirtualnej, który ma taką samą wartość jednostki LUN. | -Nowy dysk danych, był dołączony do maszyny Wirtualnej, ale nie został on zainicjowany.</br></br>-Dysk z danymi w ramach maszyny Wirtualnej nie jest poprawnie zgłaszają wartość jednostki LUN, jaką dysk został dołączony do maszyny Wirtualnej.| Upewnij się, są inicjowane dyski danych, a następnie spróbuj ponownie wykonać operację.</br></br>W systemie Windows: [Attach i zainicjować nowego dysku](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-portal#option-1-attach-and-initialize-a-new-disk).</br></br>Dla systemu Linux: [inicjowania dysku danych w systemie Linux](https://docs.microsoft.com/azure/virtual-machines/linux/classic/attach-disk-classic#initialize-a-new-data-disk-in-linux).

### <a name="fix-the-problem"></a>Rozwiąż problem
Upewnij się, została zainicjowana dysków z danymi, a następnie spróbuj ponownie wykonać operację:

- W systemie Windows: [Attach i zainicjować nowego dysku](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-portal#option-1-attach-and-initialize-a-new-disk).
- Dla systemu Linux: [inicjowania dysku danych w systemie Linux](https://docs.microsoft.com/azure/virtual-machines/linux/classic/attach-disk-classic#initialize-a-new-data-disk-in-linux).

Jeśli problem będzie się powtarzać, skontaktuj się z pomocy technicznej.


## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>Nie można znaleźć maszyny Wirtualnej platformy Azure dla wyboru "Włącz replikacji"

Jeśli nie widzisz maszyny Wirtualnej platformy Azure dla zaznaczenia po włączeniu replikacji, to może z powodu przestarzałą konfigurację usługi Site Recovery pozostać na maszynie Wirtualnej Azure. Przestarzałą konfigurację może pozostać na maszynie Wirtualnej platformy Azure w następujących przypadkach:

- Włączyć replikację dla maszyny Wirtualnej platformy Azure przy użyciu usługi Site Recovery, a następnie usunąć magazyn usługi Site Recovery bez jawnie wyłączenie replikacji na maszynie Wirtualnej.
- Włączyć replikację dla maszyny Wirtualnej platformy Azure przy użyciu usługi Site Recovery, a następnie usunąć grupę zasobów, zawierającą magazyn usługi Site Recovery bez jawnie wyłączenie replikacji na maszynie Wirtualnej.

### <a name="fix-the-problem"></a>Rozwiąż problem

Można użyć [Usuń stare skryptu konfiguracji ASR](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) i usuwanie starych konfiguracji usługi Site Recovery na maszynie Wirtualnej Azure. Maszyna wirtualna powinna zostać wyświetlona po włączeniu replikacji po usunięciu przestarzałą konfigurację.

## <a name="vms-provisioning-state-is-not-valid-error-code-150019"></a>Stan inicjowania obsługi administracyjnej maszyny Wirtualnej jest nieprawidłowy (kod błędu 150019)

Aby włączyć replikację na maszynie Wirtualnej, stan inicjowania powinien być **zakończyło się pomyślnie**. Można sprawdzić stan maszyny Wirtualnej, wykonując poniższe kroki.

1.  Wybierz **Eksploratora zasobów** z **wszystkie usługi** w portalu Azure.
2.  Rozwiń węzeł **subskrypcje** listę i wybierz subskrypcję.
3.  Rozwiń węzeł **ResourceGroups** listę i wybierz grupę zasobów maszyny wirtualnej.
4.  Rozwiń węzeł **zasobów** listę i wybierz maszynę wirtualną
5.  Sprawdź **provisioningState** w widoku wystąpienia po prawej stronie.

### <a name="fix-the-problem"></a>Rozwiąż problem

- Jeśli **provisioningState** jest **niepowodzenie**, skontaktuj się z pomocą techniczną, podając szczegóły, aby rozwiązać.
- Jeśli **provisioningState** jest **aktualizacji**, inne rozszerzenie mogą być pobieranie wdrożone. Sprawdź, czy są wszystkie trwających operacji na maszynie Wirtualnej, poczekaj na ich zakończenie i ponów próbę odzyskania lokacji nie powiodło się **włączyć replikację** zadania.

## <a name="next-steps"></a>Kolejne kroki
[Replikowanie maszyn wirtualnych platformy Azure](azure-to-azure-quickstart.md)
