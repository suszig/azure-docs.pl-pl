---
title: "Konfigurowanie odzyskiwania po awarii dla maszyn wirtualnych platformy Azure w regionie pomocniczym Azure z usługą Azure Site Recovery (wersja zapoznawcza)"
description: "Dowiedz się, jak skonfigurować odzyskiwania po awarii dla maszyn wirtualnych Azure w innym regionie Azure, przy użyciu usługi Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/08/2017
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 5464eea75c89a95e6bf74b3f24fe92f3652f5db9
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="set-up-disaster-recovery-for-azure-vms-to-a-secondary-azure-region-preview"></a>Konfigurowanie odzyskiwania po awarii dla maszyn wirtualnych platformy Azure do dodatkowej regionu Azure (wersja zapoznawcza)

[Usługi Azure Site Recovery](site-recovery-overview.md) usługi przyczynia się do strategii odzyskiwania po awarii poprzez organizowanie replikacji, trybu failover i powrotu po awarii maszyn lokalnych i Azure maszynach wirtualnych (VM) i zarządzanie nimi.

W tym samouczku przedstawiono sposób konfigurowania odzyskiwania po awarii w regionie pomocniczym Azure dla maszyn wirtualnych platformy Azure. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie magazynu Usług odzyskiwania
> * Sprawdź ustawienia zasobów obiektu docelowego
> * Konfigurowanie wychodzący dostęp do maszyn wirtualnych
> * Włącz replikację dla maszyny Wirtualnej

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

- Upewnij się, że rozumiesz [architektura scenariusza i składniki](concepts-azure-to-azure-architecture.md).
- Przegląd [spełnić wymagania dotyczące](site-recovery-support-matrix-azure-to-azure.md) dla wszystkich składników.

## <a name="create-a-vault"></a>Tworzenie magazynu

Utwórz magazyn w dowolnym regionie, z wyjątkiem region źródła.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Kliknij pozycję **Nowy** > **Monitorowanie i zarządzanie** > **Backup and Site Recovery**.
3. W polu **Nazwa** wprowadź przyjazną nazwę identyfikującą magazyn. Jeśli masz więcej niż jedną subskrypcję, wybierz odpowiedni.
4. Utwórz grupę zasobów lub wybierz istniejący. Określ region platformy Azure. Aby sprawdzić obsługiwane regiony, zobacz sekcję dotyczącą dostępności geograficznej w temacie [Szczegóły cennika usługi Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Aby szybko uzyskać dostęp do magazynu z pulpitu nawigacyjnego, kliknij przycisk **Przypnij do pulpitu nawigacyjnego** , a następnie kliknij przycisk **Utwórz**.

   ![Nowy magazyn](./media/azure-to-azure-tutorial-enable-replication/new-vault-settings.png)

   Nowy magazyn zostanie dodany do **pulpitu nawigacyjnego** w obszarze **wszystkie zasoby**i w głównym **Magazyny usług odzyskiwania** strony.

## <a name="verify-target-resources"></a>Sprawdź zasoby obiektów docelowych

1. Sprawdź, czy subskrypcji platformy Azure umożliwia tworzenie maszyn wirtualnych w regionie docelowej służącej do odzyskiwania po awarii. Skontaktuj się z pomocą techniczną, aby włączyć wymagane przydziału.

2. Upewnij się, że subskrypcja ma za mało zasobów do obsługi maszyn wirtualnych o rozmiarze zgodne źródłowe maszyny wirtualne. Usługa Site Recovery wybiera ten sam rozmiar lub najbliższy rozmiar możliwe w dla docelowej maszyny Wirtualnej.

## <a name="configure-outbound-network-connectivity"></a>Konfiguracja połączenia sieciowego ruchu wychodzącego

Do odzyskania lokacji będzie działać zgodnie z oczekiwaniami należy wprowadzić kilka zmian w łączności wychodzącego z maszyn wirtualnych, które mają być replikowane.

- Usługa Site Recovery nie obsługuje użycia serwera proxy uwierzytelniania do sterowania łączność sieciową.
- Jeśli uwierzytelnianie serwera proxy, nie można włączyć replikacji.

### <a name="outbound-connectivity-for-urls"></a>Łączność wychodząca dla adresu URL

Jeśli używasz serwera proxy zapora oparta na adres URL do sterowania łączność wychodząca, Zezwalaj na dostęp do następujących adresów URL używanych przez usługi Site Recovery.

| **ADRES URL** | **Szczegóły** |
| ------- | ----------- |
| *.blob.core.windows.net | Umożliwia zapisanie z maszyny Wirtualnej do konta magazynu pamięci podręcznej w regionie źródła danych. |
| Login.microsoftonline.com | Umożliwia autoryzację i uwierzytelnianie do adresów URL usługi Site Recovery. |
| *.hypervrecoverymanager.windowsazure.com | Umożliwia maszynie Wirtualnej, aby komunikować się z usługą Site Recovery. |
| *. servicebus.windows.net | Pozwala na maszynie Wirtualnej, aby zapisać monitorowania usługi Site Recovery i danych diagnostycznych. |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>Łączność wychodząca dla zakresów adresów IP

Kontrolowania przy użyciu wszelkie zapory oparte na protokole IP, serwer proxy lub reguły NSG łączność wychodząca, poniższe zakresy adresów IP muszą być białej. Pobierz listę zakresów z następujących łączy:

  - [Zakresy IP centrum danych platformy Microsoft Azure](http://www.microsoft.com/en-us/download/details.aspx?id=41653)
  - [Zakresy IP centrum danych Azure w Niemczech systemu Windows](http://www.microsoft.com/en-us/download/details.aspx?id=54770)
  - [Zakresy IP centrum danych Azure w Chinach systemu Windows](http://www.microsoft.com/en-us/download/details.aspx?id=42064)
  - [Zakresy adresów URL i IP dla usługi Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity)
  - [Adresy IP punktu końcowego usługi odzyskiwania lokacji](https://aka.ms/site-recovery-public-ips)

Użyj tych list, aby skonfigurować kontrolę dostępu do sieci w sieci. Możesz użyć tej funkcji [skryptu](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702) do tworzenia wymaganych reguł NSG.

## <a name="verify-azure-vm-certificates"></a>Sprawdź certyfikaty maszyny Wirtualnej Azure

Sprawdź, czy wszystkie najnowsze certyfikaty główne są obecne w systemie Windows lub maszyn wirtualnych systemu Linux, które mają być replikowane. Jeśli nie są najnowsze certyfikaty główne, maszyna wirtualna nie zarejestrowany usłudze Site Recovery właściwym ograniczeń dotyczących zabezpieczeń.

- Dla maszyn wirtualnych systemu Windows należy zainstalować wszystkie najnowsze aktualizacje systemu Windows na Maszynie wirtualnej, tak aby były wszystkich zaufanych certyfikatów głównych na komputerze. W środowisku bez połączenia wykonaj standardowe usługi Windows Update i procesy aktualizacji certyfikatów dla Twojej organizacji.

- Dla maszyn wirtualnych systemu Linux postępuj zgodnie ze wskazówkami, w dostarczonych przez użytkownika dystrybutor systemu Linux, aby uzyskać najnowsze zaufanych certyfikatów głównych i listy odwołania certyfikatów na maszynie Wirtualnej.

## <a name="set-permissions-on-the-account"></a>Ustawianie uprawnień dla konta

Usługa Azure Site Recovery zawiera trzy role wbudowane do kontrolowania operacji zarządzania usługi Site Recovery.

- **Lokacja odzyskiwania współautora** — ta rola ma wszystkie uprawnienia wymagane do zarządzania operacjami usługi Azure Site Recovery w magazynie usług odzyskiwania. Użytkownika z tą rolą, jednak nie można utworzyć lub usuwanie magazynu usług odzyskiwania lub przypisywanie praw dostępu do innych użytkowników. Ta rola jest najbardziej odpowiednie dla administratorów odzyskiwania po awarii można włączyć i zarządzać odzyskiwania po awarii dla aplikacji lub w całej organizacji.

- **Operator odzyskiwania lokacji** — ta rola ma uprawnienia do wykonania i menedżera operacji trybu Failover i powrotu po awarii. Z tą rolą użytkownika nie może włączyć lub Wyłącz replikację, Utwórz lub usuń magazyny, zarejestrować nową infrastrukturę lub przypisywanie praw dostępu do innych użytkowników. Ta rola jest najbardziej odpowiednie dla operatora odzyskiwania po awarii, który można przełączyć maszyny wirtualne lub aplikacji, gdy zaleceniami właściciele aplikacji i administratorów IT. Post rozpoznawanie po awarii, operator odzyskiwania po awarii można Wznów i powrotu po awarii maszyn wirtualnych.

- **Czytnik odzyskiwania lokacji** — ta rola ma uprawnienia do wyświetlania wszystkich operacji zarządzania usługi Site Recovery. Ta rola jest najbardziej odpowiednie dla zarządu monitorowania IT, który można monitorować bieżący stan ochrony i podnieść biletami pomocy technicznej.

Dowiedz się więcej na [wbudowane role Azure RBAC](../active-directory/role-based-access-built-in-roles.md)

## <a name="enable-replication"></a>Włączanie replikacji

### <a name="select-the-source"></a>Wybierz źródło

1. Magazyny usług odzyskiwania, kliknij nazwę magazynu > **+ Replikuj**.
2. W **źródła**, wybierz pozycję **Azure — wersja ZAPOZNAWCZA**.
3. W **lokalizacja źródłowa**, wybierz źródło region platformy Azure, w którym są uruchomione maszyny wirtualne.
4. Wybierz **modelu wdrażania maszyny wirtualnej platformy Azure** dla maszyn wirtualnych: **Resource Manager** lub **klasycznego**.
5. Wybierz **źródłowa grupa zasobów** dla maszyn wirtualnych Menedżera zasobów lub **usługi w chmurze** klasycznych maszyn wirtualnych.
6. Kliknij pozycję **OK**, aby zapisać ustawienia.

### <a name="select-the-vms"></a>Wybierz maszyny wirtualne

Usługa Site Recovery pobiera listę maszyn wirtualnych, związanych z subskrypcji i zasobu grupy/usługa w chmurze.

1. W **maszyn wirtualnych**, wybierz maszyny wirtualne mają być replikowane.
2. Kliknij przycisk **OK**.

### <a name="configure-replication-settings"></a>Konfigurowanie ustawień replikacji

Usługa Site Recovery tworzy domyślne ustawienia i zasady replikacji dla region docelowy. Możesz zmienić ustawienia, w zależności od wymagań.

1. Kliknij przycisk **ustawienia** Aby wyświetlić ustawienia obiektu docelowego.
2. Aby zastąpić domyślne ustawienia docelowych, kliknij przycisk **Dostosuj**. 

![Konfigurowanie ustawień](./media/azure-to-azure-tutorial-enable-replication/settings.png)


- **Lokalizacja docelowa**: region docelowy używanych na potrzeby odzyskiwania po awarii. Zaleca się, że lokalizacja docelowa odpowiada lokalizacji magazynu usługi Site Recovery.

- **Docelowa grupa zasobów**: grupy zasobów w regionie docelowy przechowujący maszynach wirtualnych Azure po pracy awaryjnej. Domyślnie usługi Site Recovery tworzy nową grupę zasobów w regionie docelowych z sufiksem "asr".

- **Wirtualne sieci docelowej**: sieci w regionie docelowych maszyn wirtualnych znajdują się po pracy awaryjnej.
  Domyślnie usługa Site Recovery tworzy nową sieć wirtualną (i podsieci) w regionie docelowych z sufiksem "asr".

- **Buforowanie kont magazynu**: Usługa Site Recovery używa konta magazynu w regionie źródła. Zmiany źródłowe maszyny wirtualne są wysyłane do tego konta przed replikacji do lokalizacji docelowej.

- **Docelowa kont magazynu**: Domyślnie, Usługa Site Recovery tworzy nowe konto magazynu w regionie docelowym dublowanego źródła konta magazynu maszyny Wirtualnej.

- **Docelowa zestawów dostępności**: Domyślnie, Usługa Site Recovery tworzy nowy zestawem dostępności w region docelowy wraz z sufiksem "asr". Zestawy dostępności można dodawać tylko, jeśli maszyny wirtualne są częścią zestawu w regionie źródła.

- **Nazwa zasad replikacji**: Nazwa zasad.

- **Przechowywania punktu odzyskiwania**: domyślnie Site Recovery przechowuje punkty odzyskiwania przez 24 godziny. Można skonfigurować wartość z zakresu od 1 do 72 godzin.

- **Częstotliwość migawek spójności aplikacji**: domyślnie Site Recovery przyjmuje migawki dotyczącej spójności aplikacji co 4 godziny. Można skonfigurować dowolną wartość z zakresu od 1 do 12 godzin. Migawki dotyczącej spójności aplikacji jest w momencie migawki danych aplikacji wewnątrz maszyny Wirtualnej. Usługa kopiowania woluminów w tle (VSS) zapewnia tej aplikacji na maszynie Wirtualnej są w spójnym stanie podczas wykonywania migawki.

### <a name="track-replication-status"></a>Śledź stan replikacji

1. W **ustawienia**, kliknij przycisk **Odśwież** można pobrać najnowszy stan.

2. Możesz śledzić postęp **Włącz ochronę** zadania w **ustawienia** > **zadania** > **zadania usługi Site Recovery**.

3. W **ustawienia** > **elementy replikowane**, można wyświetlić stan maszyn wirtualnych i postęp replikacji początkowej. Kliknij maszynę Wirtualną, aby przejść do jego ustawień.

## <a name="next-steps"></a>Następne kroki

W tym samouczku odzyskiwania po awarii jest skonfigurowana dla maszyny Wirtualnej platformy Azure. Następnym krokiem jest, aby przetestować konfigurację.

> [!div class="nextstepaction"]
> [Uruchamianie próbnego odzyskiwania po awarii](azure-to-azure-tutorial-dr-drill.md)
