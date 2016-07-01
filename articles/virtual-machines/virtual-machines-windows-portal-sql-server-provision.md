<properties
    pageTitle="Aprowizowanie maszyny wirtualnej programu SQL Server | Microsoft Azure"
    description="Utwórz maszynę wirtualną programu SQL Server i połącz się z nią na platformie Azure przy użyciu portalu. W tym samouczku wykorzystano tryb usługi Resource Manager."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    editor=""
    manager="jhubbard"
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="05/24/2016"
    ms.author="jroth" />

# Aprowizowanie maszyny wirtualnej programu SQL Server w Portalu Azure

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [Program PowerShell](virtual-machines-windows-ps-sql-create.md)

Ten kompleksowy samouczek pokazuje, jak używać Portalu Azure do aprowizowania maszyny wirtualnej programu SQL Server.

Kilka obrazów w galerii maszyn wirtualnych Azure zawiera program Microsoft SQL Server. Za pomocą kilku kliknięć możesz wybrać jeden z obrazów maszyn wirtualnych SQL z galerii i aprowizować go w środowisku Azure.

W tym samouczku zostaną wykonane następujące czynności:

- [Wybieranie obrazu maszyny wirtualnej SQL z galerii](#select-a-sql-vm-image-from-the-gallery)
- [Konfigurowanie i tworzenie maszyny wirtualnej](#configure-the-vm)
- [Otwieranie maszyny wirtualnej przy użyciu pulpitu zdalnego](#open-the-vm-with-remote-desktop)
- [Zdalne ustanawianie połączenia z programem SQL Server](#connect-to-sql-server-remotely)

## Wybieranie obrazu maszyny wirtualnej SQL z galerii

1. Zaloguj się w [Portalu Azure](https://portal.azure.com) przy użyciu swojego konta.

    >[AZURE.NOTE] Jeśli nie masz konta platformy Azure, odwiedź stronę [bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

1. W Portalu Azure kliknij pozycję **Nowe**. Portal otworzy blok **Nowe**. Zasoby maszyn wirtualnych programu SQL znajdują się w grupie **Virtual Machines** witryny Marketplace.

1. W bloku **Nowe** kliknij pozycję **Virtual Machines**.

1. Aby wyświetlić wszystkie dostępne obrazy, kliknij pozycję **Wyświetl wszystko** w bloku **Virtual Machines**.

    ![Blok Azure Virtual Machines](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade.png)

1. W obszarze **Serwery baz danych** kliknij pozycję **SQL Server**. Może być konieczne przewinięcie w dół w celu zlokalizowania pozycji **Serwery baz danych**. Przejrzyj dostępne szablony programu SQL Server.

    ![Obrazy SQL w galerii maszyn wirtualnych](./media/virtual-machines-windows-portal-sql-server-provision/virtual-machine-gallery-sql-server.png)

1. Każdy szablon identyfikuje wersję programu SQL Server i system operacyjny. Wybierz z listy jeden z obrazów. Następnie przejrzyj blok szczegółów zawierający opis obrazu maszyny wirtualnej.

1. W obszarze **Wybierz model wdrożenia** sprawdź, czy pozycja **Resource Manager** została zaznaczona, i kliknij pozycję **Utwórz**.

    ![Tworzenie maszyny wirtualnej SQL przy użyciu usługi Resource Manager](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## Konfigurowanie maszyny wirtualnej
Do konfigurowania maszyny wirtualnej programu SQL Server służy pięć bloków.

| Krok               | Opis                          |
|---------------------|-------------------------------|
| **Podstawy**              | [Konfigurowanie ustawień podstawowych](#1-configure-basic-settings)      |
| **Rozmiar**                | [Wybieranie rozmiaru maszyny wirtualnej](#2-choose-virtual-machine-size)   |
| **Ustawienia**            | [Konfigurowanie funkcji opcjonalnych](#3-configure-optional-features)   |
| **Ustawienia programu SQL Server** | [Konfigurowanie ustawień programu SQL Server](#4-configure-sql-server-settings) |
| **Podsumowanie**             | [Przeglądanie podsumowania](#5-review-the-summary)            |

## 1. Konfigurowanie ustawień podstawowych
W bloku **Podstawowe** podaj następujące informacje:

* Wprowadź unikatową nazwę maszyny wirtualnej w polu **Nazwa**.
* Określ nazwę użytkownika dla konta lokalnego administratora maszyny wirtualnej w polu **Nazwa użytkownika**. To konto jest także dodawane do stałej roli serwera programu SQL Server **sysadmin**.
* Podaj silne hasło w polu **Hasło**.
* Jeśli masz wiele subskrypcji, sprawdź, czy subskrypcja jest poprawna dla nowej maszyny wirtualnej.
* W polu **Grupa zasobów** wpisz nazwę nowej grupy zasobów. Inna możliwość to użycie istniejącej grupy zasobów i kliknięcie pozycji **Wybierz istniejącą**. Grupa zasobów to kolekcja powiązanych zasobów platformy Azure (maszyny wirtualne, konta magazynu, sieci wirtualne itp.).

    >[AZURE.NOTE] Nowa grupa zasobów jest przydatna, jeśli tylko testujesz lub poznajesz wdrożenia programu SQL Server na platformie Azure. Po zakończeniu testu usuń grupę zasobów, aby automatycznie usunąć maszynę wirtualną i wszystkie skojarzone z nią zasoby. Aby uzyskać więcej informacji na temat grup zasobów, zobacz [Omówienie usługi Azure Resource Manager](../resource-group-overview.md).

* Wybierz ustawienie **Lokalizacja** dla tego wdrożenia.
* Kliknij pozycję **OK**, aby zapisać ustawienia.

    ![Blok podstawowych ustawień SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## 2. Wybieranie rozmiaru maszyny wirtualnej
W kroku **Rozmiar** wybierz rozmiar maszyny wirtualnej w bloku **Wybierz rozmiar**. W tym bloku początkowo wyświetlane są rozmiary zalecane zgodnie z wybranym szablonem. Szacowany jest również miesięczny koszt działania maszyny wirtualnej.

![Opcje rozmiaru maszyny wirtualnej SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

W przypadku obciążeń produkcyjnych zalecane jest wybranie rozmiaru maszyny wirtualnej, która obsługuje [usługę Premium Storage](../storage/storage-premium-storage.md). Jeśli nie potrzebujesz tego poziomu wydajności, użyj przycisku **Wyświetl wszystkie**, który umożliwia wyświetlenie wszystkich opcji rozmiaru maszyny. Na przykład możesz użyć mniejszego rozmiaru maszyny dla wdrożenia lub środowiska testowego.

>[AZURE.NOTE] Aby uzyskać więcej informacji na temat rozmiarów maszyny wirtualnej, zobacz [Sizes for virtual machines](virtual-machines-windows-sizes.md) (Rozmiary maszyn wirtualnych). Aby zapoznać się z zagadnieniami związanymi z rozmiarami maszyn wirtualnych programu SQL Server, zobacz [Performance best practices for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md) (Najlepsze rozwiązania umożliwiające zwiększenie wydajności programu SQL Server w usłudze Azure Virtual Machines).

Wybierz rozmiar maszyny wirtualnej, a następnie kliknij pozycję **Wybierz**.

## 3. Konfigurowanie funkcji opcjonalnych
W bloku **Ustawienia** skonfiguruj magazyn Azure, sieć i monitorowanie dla maszyny wirtualnej.

- W obszarze **Usługa Storage** określ wartość **Typ dysku** jako standardowy lub Premium (SSD). Magazyn w warstwie Premium jest zalecany w przypadku obciążeń produkcyjnych.

>[AZURE.NOTE] Wybranie opcji Premium (SSD) dla rozmiaru maszyny, która nie obsługuje usługi Premium Storage, powoduje automatyczną zmianę rozmiaru maszyny.  

- W obszarze **Konto usługi Storage** możesz zaakceptować nazwę konta automatycznie aprowizowanego magazynu. Możesz również kliknąć pozycję **Konto usługi Storage**, aby wybrać istniejące konto i skonfigurować typ konta magazynu. Domyślnie platforma Azure tworzy nowe konto magazynu z magazynem lokalnie nadmiarowym. Aby uzyskać więcej informacji na temat opcji magazynu, zobacz [Replikacja usługi Azure Storage](../storage/storage-redundancy.md).

- W obszarze **Sieć** możesz zaakceptować automatycznie wypełnione wartości. Możesz również kliknąć poszczególne funkcje, aby ręcznie skonfigurować ustawienia **Sieć wirtualna**, **Podsieć**, **Publiczny adres IP** i **Grupa zabezpieczeń sieci**. Na potrzeby tego samouczka zachowaj wartości domyślne.

- Platforma Azure domyślnie umożliwia **monitorowanie** za pomocą tego samego konta magazynu wyznaczonego dla maszyny wirtualnej. Możesz zmienić te ustawienia tutaj.

- W obszarze **Zestaw dostępności** określ zestaw dostępności. Na potrzeby tego samouczka możesz wybrać pozycję **Brak**. Jeśli planujesz konfigurowanie zawsze włączonych grup dostępności SQL, skonfiguruj dostępność, aby zapobiec ponownemu utworzeniu maszyny wirtualnej.  Aby uzyskać więcej informacji, zobacz [Manage the Availability of Virtual Machines](virtual-machines-windows-manage-availability.md) (Zarządzanie dostępnością usługi Virtual Machines).

Po zakończeniu konfigurowania tych ustawień kliknij pozycję **OK**.

## 4. Konfigurowanie ustawień programu SQL Server
W bloku **Ustawienia programu SQL Server** skonfiguruj określone ustawienia i optymalizacje dla programu SQL Server. Możesz skonfigurować dla programu SQL Server poniższe ustawienia.

| Ustawienie               |
|---------------------|
| [Łączność](#connectivity)              |
| [Uwierzytelnianie](#authentication)                |
| [Konfiguracja usługi Storage](#storage-configuration)            |
| [Automatyczne stosowanie poprawek](#automated-patching) |
| [Automatyczne usługa Backup](#automated-backup)             |
| [Integracja magazynu kluczy Azure](#azure-key-vault-integration)             |

### Łączność
W obszarze **Łączność z serwerem SQL** określ żądany typ dostępu do wystąpienia programu SQL Server na tej maszynie wirtualnej. Na potrzeby tego samouczka wybierz pozycję **Publiczne (Internet)**, aby zezwolić na połączenia z programem SQL Server z komputerów lub usług w Internecie. Jeśli ta opcja zostanie wybrana, platforma Azure automatycznie skonfiguruje zaporę i sieciową grupę zabezpieczeń do zezwalania na ruch przez port 1433.  

![Opcje łączności z serwerem SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

Aby ustanowić połączenie z programem SQL Server za pośrednictwem Internetu, musisz również włączyć funkcję Uwierzytelnianie programu SQL Server opisaną w następnej sekcji.

>[AZURE.NOTE] Można dodać więcej ograniczeń komunikacji sieciowej z maszyną wirtualną programu SQL Server. Możesz to zrobić, edytując grupę zabezpieczeń sieci po utworzeniu maszyny wirtualnej. Aby uzyskać więcej informacji, zobacz [What is a Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md) (Co to jest sieciowa grupa zabezpieczeń?).

Jeśli nie chcesz zezwolić na połączenia z aparatem bazy danych za pośrednictwem Internetu, wybierz jedną z następujących opcji:

- **Lokalne (tylko wewnątrz maszyny wirtualnej)**, aby zezwolić na połączenia z programem SQL Server tylko z maszyny wirtualnej.
- **Prywatne (wewnątrz usługi Virtual Network)**, aby zezwolić na połączenia z programem SQL Server tylko z maszyn wirtualnych lub usług w tej samej sieci wirtualnej.

Ogólnie rzecz biorąc, można ulepszyć zabezpieczenia, wybierając łączność z najbardziej restrykcyjnymi ograniczeniami akceptowanymi w danym scenariuszu. Jednak wszystkie opcje można zabezpieczyć przy użyciu reguł sieciowej grupy zabezpieczeń i uwierzytelniania SQL/Windows.

Domyślnie wybierane jest ustawienie **Port** 1433. Możesz określić inny numer portu.
Aby uzyskać więcej informacji, zobacz [Connect to a SQL Server Virtual Machine (Resource Manager) | Microsoft Azure](virtual-machines-windows-sql-connect.md) (Ustanawianie połączenia z maszyną wirtualną programu SQL Server (usługa Resource Manager) | Microsoft Azure).

### Uwierzytelnianie
Jeśli wymagasz uwierzytelniania programu SQL Server, kliknij pozycję **Włącz** w obszarze **Uwierzytelnianie SQL**.

![Uwierzytelnianie programu SQL Server](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

>[AZURE.NOTE] Jeśli planujesz dostęp do programu SQL Server za pośrednictwem Internetu (tzn. opcja łączności publicznej), musisz w tym miejscu włączyć uwierzytelnianie SQL. Publiczny dostęp do programu SQL Server wymaga użycia uwierzytelniania SQL.

Jeśli włączasz opcję uwierzytelniania programu SQL Server, podaj informacje w polach **Nazwa logowania** i **Hasło**. Ta nazwa użytkownika jest skonfigurowana jako identyfikator logowania uwierzytelniania programu SQL Server i członek stałej roli serwera **sysadmin**. Aby uzyskać więcej informacji na temat trybów uwierzytelniana, zobacz [Choose an Authentication Mode](http://msdn.microsoft.com/library/ms144284.aspx) (Wybieranie trybu uwierzytelniania).

Jeśli nie włączysz opcji uwierzytelniania programu SQL Server, możesz użyć konta administratora lokalnego na maszynie wirtualnej do ustanawiania połączeń z wystąpieniem programu SQL Server.

### Konfiguracja usługi Storage
Kliknij pozycję **Konfiguracja usługi Storage**, aby określić wymagania dotyczące magazynu.

![Konfiguracja usługi SQL Storage](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

>[AZURE.NOTE] Ta opcja jest niedostępna po wybraniu magazynu standardowego. Automatyczna optymalizacja magazynu jest dostępna tylko dla usługi Premium Storage.

Możesz określić wymagania dotyczące na przykład liczby operacji wejścia/wyjścia na sekundę (IOPs), przepustowości w MB/s i całkowitego rozmiaru magazynu. Skonfiguruj te wartości przy użyciu suwaków. Portal automatycznie oblicza liczbę dysków na podstawie tych wymagań.

Domyślnie platforma Azure optymalizuje magazyn dla 5000 IOPs, 200 MB i 1 TB miejsca do magazynowania. Możesz zmienić te ustawienia magazynu zależnie od obciążenia. W obszarze **Usługa Storage zoptymalizowana dla** wybierz jedną z następujących opcji:

- **Ogólne** to ustawienie domyślne obsługujące większość obciążeń.
- **Przetwarzanie transakcyjne** optymalizuje magazyn dla tradycyjnych obciążeń baz danych OLTP.
- **Magazynowanie danych** optymalizuje magazyn dla tradycyjnych obciążeń związanych z analizą i raportami.

>[AZURE.NOTE] Górne limity na suwakach są zależne od wybranego rozmiaru maszyny wirtualnej.

### Automatyczne stosowanie poprawek
Opcja **Automatyczne stosowanie poprawek** jest domyślnie włączona. Automatyczne stosowanie poprawek umożliwia platformie Azure automatyczne stosowanie poprawek programu SQL Server i systemu operacyjnego. Określ dzień tygodnia, godzinę i czas trwania okna obsługi. Platforma Azure stosuje poprawki w tym oknie obsługi. Harmonogram okna obsługi korzysta z ustawień regionalnych godziny maszyny wirtualnej. Jeśli nie chcesz, aby platforma Azure automatycznie stosowała poprawki programu SQL Server i systemu operacyjnego, kliknij pozycję **Wyłącz**.  

![Automatyczne stosowanie poprawek SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Aby uzyskać więcej informacji, zobacz [Automated Patching for SQL Server in Azure Virtual Machines](virtual-machines-windows-classic-sql-automated-patching.md) (Automatyczne stosowanie poprawek programu SQL Server w usłudze Azure Virtual Machines).

### Automatyczne kopie zapasowe
Automatyczną obsługę kopii zapasowych baz danych możesz włączyć dla wszystkich baz danych w obszarze **Automatyczne kopie zapasowe**. Opcja Automatyczne kopie zapasowe jest domyślnie wyłączona.

Po włączeniu automatycznej obsługi kopii zapasowych SQL możesz skonfigurować poniższe ustawienia:

- Okres przechowywania (dni) kopii zapasowych
- Konto usługi Storage używane dla kopii zapasowych
- Opcja szyfrowania i hasło dla kopii zapasowych

Aby szyfrować kopie zapasowe, kliknij pozycję **Włącz**. Następnie określ ustawienie **Hasło**. Platforma Azure tworzy certyfikat do szyfrowania kopii zapasowych i używa określonego hasła do ochrony tego certyfikatu.

![Automatyczna usługa Backup SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup.png)

 Aby uzyskać więcej informacji, zobacz [Automated Backup for SQL Server in Azure Virtual Machines](virtual-machines-windows-classic-sql-automated-backup.md) (Automatyczna usługa Backup dla programu SQL Server w usłudze Azure Virtual Machines).

### Integracja magazynu kluczy Azure
Aby przechowywać klucze tajne zabezpieczeń do szyfrowania na platformie Azure, kliknij pozycję **Integracja magazynu kluczy Azure**, a następnie kliknij pozycję **Włącz**.

![Integracja magazynu kluczy Usług SQL Azure](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

Poniższa tabela zawiera listę parametrów wymaganych do skonfigurowania integracji magazynu kluczy Azure.

|PARAMETR|OPIS|PRZYKŁAD|
|----------|----------|-------|
|**Adres URL magazynu kluczy** |Lokalizacja magazynu kluczy.|https://contosokeyvault.vault.azure.net/ |
|**Nazwa główna** |Nazwa główna usługi Azure Active Directory. Ta nazwa jest również nazywana identyfikatorem klienta.  |fde2b411-33d5-4e11-af04eb07b669ccf2|
| **Główny klucz tajny**|Główny klucz tajny usługi Azure Active Directory. Ten klucz tajny jest również nazywany kluczem tajnym klienta. | 9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM=|
|**Nazwa poświadczenia**|**Nazwa poświadczenia**: integracja magazynu kluczy Azure powoduje utworzenie poświadczenia w programie SQL Server, co umożliwia maszynie wirtualnej dostęp do magazynu kluczy. Wybierz nazwę tego poświadczenia.| moje_poświadczenie_1|

Aby uzyskać więcej informacji, zobacz [Configure Azure Key Vault Integration for SQL Server on Azure VMs](virtual-machines-windows-classic-ps-sql-keyvault.md) (Konfigurowanie integracji magazynu kluczy Azure dla programu SQL Server na maszynach wirtualnych Azure).

Po zakończeniu konfigurowania tych ustawień programu SQL Server kliknij pozycję **OK**.

## 5. Przeglądanie podsumowania
W bloku **Podsumowanie** przejrzyj podsumowanie i kliknij pozycję **OK**, aby utworzyć wystąpienie programu SQL Server, grupę zasobów i zasoby określone dla tej maszyny wirtualnej.

Możesz monitorować wdrożenie z Portalu Azure. Przycisk **Powiadomienia** w górnej części ekranu służy do wyświetlania podstawowych informacji dotyczących stanu wdrożenia.

>[AZURE.NOTE] Aby podać przykład czasu wymaganego do przeprowadzenia wdrożenia, wdrożono maszynę wirtualną SQL dla regionu Wschodnie stany USA z ustawieniami domyślnymi. Przeprowadzenie tego wdrożenia testowego trwało łącznie 26 minut. Wdrażanie może jednak trwać krócej lub dłużej zależnie od regionu i wybranych ustawień.

## Otwieranie maszyny wirtualnej przy użyciu pulpitu zdalnego

Wykonaj następujące kroki, aby ustanowić połączenie z maszyną wirtualną przy użyciu pulpitu zdalnego:

1. Po utworzeniu maszyny wirtualnej Azure ikona maszyny wirtualnej pojawi się na pulpicie nawigacyjnym platformy Azure. Możesz również znaleźć ją, przeglądając istniejące maszyny wirtualne. Kliknij maszynę wirtualną SQL. W bloku **Maszyna wirtualna** zostaną wyświetlone szczegóły maszyny wirtualnej.
1. W górnej części bloku **Maszyna wirtualna** kliknij pozycję **Połącz**.
1. Przeglądarka pobierze plik RDP dla maszyny wirtualnej. Otwórz plik RDP.
    ![Pulpit zdalny dla maszyny wirtualnej SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
1. Funkcja Podłączanie pulpitu zdalnego powiadomi, że nie można zidentyfikować wydawcy tego połączenia zdalnego. Kliknij pozycję **Połącz**, aby kontynuować.
1. W oknie dialogowym **Zabezpieczenia systemu Windows** kliknij pozycję **Użyj innego konta**.
1. W polu **Nazwa użytkownika** wpisz **\<nazwę użytkownika>**, gdzie <user name> jest nazwą użytkownika określoną podczas konfigurowania maszyny wirtualnej. Przed nazwą musisz dodać początkowy ukośnik odwrotny.
1. W polu **Hasło** wpisz hasło uprzednio skonfigurowane dla tej maszyny wirtualnej, a następnie kliknij pozycję **OK**, aby ustanowić połączenie.
1. Jeśli w kolejnym oknie dialogowym **Podłączanie pulpitu zdalnego** zostanie wyświetlony monit o potwierdzenie zamiaru połączenia, kliknij pozycję **Tak**.

Po ustanowieniu połączenia z maszyną wirtualną programu SQL Server możesz uruchomić narzędzie SQL Server Management Studio i ustanowić połączenie przy użyciu uwierzytelniania systemu Windows i poświadczeń administratora lokalnego. Jeśli opcja Uwierzytelnianie programu SQL Server została włączona, możesz też ustanowić połączenie przy użyciu uwierzytelniania SQL oraz identyfikatora logowania i hasła skonfigurowanego podczas aprowizowania.

Korzystając z dostępu do maszyny, możesz bezpośrednio zmienić ustawienia maszyny i programu SQL Server, w zależności od wymagań. Na przykład możesz skonfigurować ustawienia zapory lub zmienić ustawienia konfiguracji programu SQL Server.

## Zdalne ustanawianie połączenia z programem SQL Server

W tym samouczku wybrano opcję dostępu **Publiczne** dla maszyny wirtualnej i opcję **Uwierzytelnianie programu SQL Server**. Te ustawienia powodują automatyczne skonfigurowanie maszyny wirtualnej do zezwalania na połączenia z programem SQL Server z dowolnego klienta za pośrednictwem Internetu (zakładając, że ma on poprawny identyfikator logowania SQL).

>[AZURE.NOTE] Jeśli nie wybrano opcji Publiczne podczas aprowizowania, uzyskanie dostępu do wystąpienia programu SQL Server za pośrednictwem Internetu wymaga wykonania dodatkowych kroków. Aby uzyskać więcej informacji, zobacz [Connect to a SQL Server Virtual Machine](virtual-machines-windows-sql-connect.md) (Nawiązywanie połączenia z maszyną wirtualną programu SQL Server).

Poniższe sekcje pokazują, jak nawiązać połączenie z wystąpieniem programu SQL Server na maszynie wirtualnej z innego komputera za pośrednictwem Internetu.

> [AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## Następne kroki
Aby uzyskać inne informacje na temat użycia programu SQL Server na platformie Azure, zobacz [SQL Server on Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) (Program SQL Server w usłudze Azure Virtual Machines) i [Frequently Asked Questions](virtual-machines-windows-sql-server-iaas-faq.md) (Często zadawane pytania).

Obejrzyj film poglądowy dotyczący programu SQL Server w usłudze Azure Virtual Machines: [Azure VM is the best platform for SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016) (Maszyna wirtualna Azure jest najlepszą platformą dla programu SQL Server 2016).



<!--HONumber=Jun16_HO2-->


