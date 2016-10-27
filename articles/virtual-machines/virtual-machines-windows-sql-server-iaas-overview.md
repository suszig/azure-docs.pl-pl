<properties
    pageTitle="Omówienie programu SQL Server w usłudze Azure Virtual Machines | Microsoft Azure"
    description="Dowiedz się, jak uruchomić pełne wersje programu SQL Server na maszynach wirtualnych platformy Azure. Pobierz bezpośrednie linki do wszystkich obrazów maszyn wirtualnych programu SQL Server i powiązanej zawartości."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="jroth"/>


# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Omówienie programu SQL Server w usłudze Azure Virtual Machines

W tym temacie opisano opcje uruchamiania programu SQL Server na maszynach wirtualnych Azure oraz zamieszczono [linki do obrazów portalu](#option-1-create-a-sql-vm-with-per-minute-licensing) i przegląd [typowych zadań](#manage-your-sql-vm).

>[AZURE.NOTE] Jeśli znasz już program SQL Server i chcesz tylko zobaczyć, jak wdrożyć maszynę wirtualną programu SQL Server, zobacz temat [Aprowizowanie maszyny wirtualnej programu SQL Server w witrynie Azure Portal](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="overview"></a>Omówienie
Jeśli jesteś administratorem bazy danych lub projektantem, możesz skorzystać z oferowanych przez maszyny wirtualne platformy Azure możliwości przenoszenia lokalnych obciążeń i aplikacji programu SQL Server do chmury. Następujący film wideo obejmuje omówienie techniczne maszyn wirtualnych serwera SQL Server platformy Azure.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

Flm wideo obejmuje następujące zagadnienia:

|Time|Obszar|
|---|---|
| 00:21 | Co to są maszyny wirtualne platformy Azure? |
| 01:45 | Bezpieczeństwo |
| 02:50 | Łączność |
| 03:30 | Wydajność i niezawodność magazynu |
| 05:20 | Rozmiary maszyn wirtualnych |
| 05:54 | Wysoka dostępność i umowy SLA |
| 07:30 | Pomoc techniczna w zakresie konfiguracji |
| 08:00 | Monitorowanie |
| 08:32 | Pokaz: tworzenie maszyny wirtualnej programu SQL Server 2016 |

>[AZURE.NOTE] Film wideo opiera się na programie SQL Server 2016, ale platforma Azure udostępnia obrazy maszyny wirtualnej dla wielu wersji programu SQL Server, w tym dla wersji 2008, 2012, 2014 i 2016. 

## <a name="understand-your-options"></a>Opis opcji
Istnieje wiele powodów, dla których warto zdecydować się na hosting danych w usłudze Azure. Jeśli aplikacja zostanie przeniesiona na platformę Azure, poprawi się również wydajność przenoszenia danych. Są jednak także inne korzyści. Automatycznie uzyskujesz dostęp do wielu centrów danych, co zapewnia globalną obecność i odzyskiwanie po awarii. Dane są również objęte zaawansowanymi zabezpieczeniami, co wpływa także na ich trwałość.

Program SQL Server uruchomiony na maszynach wirtualnych platformy Azure stanowi jedną z opcji przechowywania danych relacyjnych w systemie Azure. Poniższa tabela zawiera krótkie podsumowanie możliwości serwera SQL platformy Azure.

|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Możliwości serwera SQL | Opis |
|---:|---|---|
|![Program SQL Server na maszynach wirtualnych platformy Azure](./media/virtual-machines-windows-sql-server-iaas-overview/sql-server-virtual-machine.png)|[Program SQL Server na maszynach wirtualnych platformy Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/)|Uruchom program SQL Server na maszynach wirtualnych platformy Azure (główne zagadnienie z tego tematu). Bezpośrednie zarządzanie maszyną wirtualną i uruchomienie bazy danych w wersjach handlowych programu SQL Server. |
|![SQL Database](./media/virtual-machines-windows-sql-server-iaas-overview/azure-sql-database.png)|[SQL Database](https://azure.microsoft.com/services/sql-database/)|Korzystanie z usługi SQL Database w celu uzyskania dostępu do bazy danych i jej skalowania bez konieczności zarządzania podstawową infrastrukturą.|
|![Magazyn danych SQL](./media/virtual-machines-windows-sql-server-iaas-overview/azure-sql-data-warehouse.png)|[Magazyn danych SQL](https://azure.microsoft.com/en-us/services/sql-data-warehouse/)|Usługa Azure SQL Data Warehouse służy do przetwarzania dużych ilości danych relacyjnych i nierelacyjnych. Dostarcza ona skalowalne możliwości magazynowania danych.|
|![SQL Server Stretch Database](./media/virtual-machines-windows-sql-server-iaas-overview/sql-server-stretch-database.png)|[SQL Server Stretch Database](https://azure.microsoft.com/en-us/services/sql-server-stretch-database/)|Dynamicznie rozciąganie lokalnych danych transakcyjnych z programu Microsoft SQL Server 2016 na platformie Azure.|

Dostępne opcje powodują, że program SQL Server uruchomiony na maszynach wirtualnych platformy Azure stanowi dobre rozwiązanie w przypadku kilku scenariuszy. Można na przykład skonfigurować maszyny wirtualne platformy Azure w sposób najbardziej zbliżony do lokalnej maszyny z programem SQL Server. Można też uruchamiać dodatkowe aplikacje i usługi na tym samym serwerze bazy danych. Istnieją dwa zasoby, które pozwalają lepiej przeanalizować korzyści z podjęcia danej decyzji:

 - Witryna [Program SQL Server na maszynach wirtualnych platformy Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/) zawiera omówienie najlepszych scenariuszy użycia programu SQL Server na maszynach wirtualnych platformy Azure. 
 - Artykuł [Wybieranie opcji programu SQL Server w chmurze: usługa Azure SQL Database (PaaS) lub program SQL Server na maszynach wirtualnych Azure (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) zawiera szczegółowe porównanie usługi SQL Database oraz programu SQL Server uruchomionego na maszynie wirtualnej.

## <a name="create-a-new-sql-vm"></a>Tworzenie nowej maszyny wirtualnej SQL
Poniższe sekcje zawierają bezpośrednie linki do obrazów maszyn wirtualnych programu SQL Server w galerii witryny Azure Portal. Zależnie od wybranego obrazu można płacić za koszty licencji programu SQL Server naliczane proporcjonalnie do liczby minut lub skorzystać z modelu dostarczania własnej licencji (Bring Your Own License, BYOL).

Samouczek [Aprowizowanie maszyny wirtualnej programu SQL Server w witrynie Azure Portal](virtual-machines-windows-portal-sql-server-provision.md) zawiera wskazówki krok po krok dotyczące tego procesu. Sprawdź również temat [Performance best practices for SQL Server VMs](virtual-machines-windows-sql-performance.md) (Najlepsze rozwiązania umożliwiające zwiększenie wydajności maszyn wirtualnych z programem SQL Server), który wyjaśnia, w jaki sposób wybrać odpowiedni rozmiar maszyny i inne funkcje dostępne podczas aprowizacji.

## <a name="option-1:-create-a-sql-vm-with-per-minute-licensing"></a>Opcja 1. Tworzenie maszyny wirtualnej SQL z licencją płatną według stawki minutowej
Poniższa tabela zawiera macierz dostępnych obrazów programu SQL Server w galerii maszyn wirtualnych. Kliknij dowolny link, aby rozpocząć tworzenie nowej maszyny wirtualnej z programem SQL o określonej wersji, wybranym wydaniu i z danym systemem operacyjnym.

|Wersja|System operacyjny|Wersja|
|---|---|---|
|**SQL 2016**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [Dev](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL 2014 SP1**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL 2014**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2 SP3**|Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 SP3**|Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## <a name="option-2:-create-a-sql-vm-with-an-existing-license"></a>Opcja 2. Tworzenie maszyny wirtualnej SQL przy użyciu istniejącej licencji
Możesz również skorzystać z modelu dostarczania własnej licencji (Bring Your Own License, BYOL). W tym scenariuszu płacisz wyłącznie za maszynę wirtualną i nie ponosisz żadnych dodatkowych kosztów licencjonowania programu SQL Server. Aby użyć własnej licencji, skorzystaj z macierzy wersji i wydań programu SQL Server oraz systemów operacyjnych. W portalu takie nazwy obrazów mają prefiks **{BYOL}**.

|Wersja|System operacyjny|Wersja|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

> [AZURE.IMPORTANT] Aby używać obrazów maszyn wirtualnych w modelu BYOL, musisz mieć umowę Enterprise Agreement z opcją [Przenośność licencji za pośrednictwem programu Software Assurance na platformie Azure](https://azure.microsoft.com/pricing/license-mobility/). Musisz również mieć ważną licencję dla wersji/wydania programu SQL Server, którego chcesz użyć. W ciągu **10** dni od aprowizacji maszyny wirtualnej musisz [przekazać firmie Microsoft informacje niezbędne do korzystania z modelu BYOL](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf).

## <a name="manage-your-sql-vm"></a>Zarządzanie maszyną wirtualną z programem SQL
Po aprowizacji maszyny wirtualnej z programem SQL Server możesz wykonać kilka opcjonalnych zadań związanych z zarządzaniem. Pod wieloma względami możesz konfigurować program SQL Server i zarządzać nim dokładnie tak, jak w przypadku lokalnej instalacji programu SQL Server. Jednak niektóre zadania są specyficzne dla platformy Azure. W poniższych sekcjach omówiono niektóre z tych obszarów i podano linki do dalszych informacji.

### <a name="connect-to-the-vm"></a>Łączenie z maszyną wirtualną
Jedną z podstawowych czynności niezbędnych w celu zarządzania jest połączenie się z maszyną wirtualną z programem SQL Server za pomocą narzędzi takich jak SQL Server Management Studio (SSMS). Aby uzyskać instrukcje dotyczące łączenia się z nową maszyną wirtualną z programem SQL Server, zobacz sekcję [Nawiązywanie połączenia z maszyną wirtualną programu SQL Server w usłudze Azure VM](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Migrowanie danych
Jeśli masz istniejącą bazę danych, najprawdopodobniej będziesz ją przenosić do nowo aprowizowanej maszyny wirtualnej z programem SQL. Aby uzyskać listę opcji migracji i wskazówki, zobacz [Migrating a Database to SQL Server on an Azure VM](virtual-machines-windows-migrate-sql.md) (Migrowanie bazy danych do programu SQL Server na maszynie wirtualnej portalu Azure).

### <a name="configure-high-availability"></a>Konfigurowanie wysokiej dostępności
Jeśli potrzebujesz wysokiej dostępności, rozważ skonfigurowanie grup dostępności programu SQL Server. Obejmuje to wiele maszyn wirtualnych Azure w sieci wirtualnej. W Portalu Azure znajduje się szablon, który przeprowadzi konfigurację za Ciebie. Aby uzyskać więcej informacji, zobacz [Configure an AlwaysOn availability group in Azure Resource Manager virtual machines](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) (Konfigurowanie zawsze włączonej grupy dostępności na maszynach wirtualnych korzystających z usługi Azure Resource Manager). Jeśli chcesz ręcznie skonfigurować grupę dostępności i skojarzony odbiornik, zobacz [Configure AlwaysOn Availability Groups in Azure VM](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md) (Konfigurowanie zawsze włączonych grup dostępności na maszynie wirtualnej Azure).

Inne zagadnienia dotyczące wysokiej dostępności można znaleźć w temacie [High Availability and Disaster Recovery for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md) (Wysoka dostępność i odzyskiwanie po awarii dla programu SQL Server w usłudze Azure Virtual Machines).

### <a name="back-up-your-data"></a>Tworzenie kopii zapasowej danych
Maszyny wirtualne Azure umożliwiają korzystanie z funkcji [Automatyczne kopie zapasowe](virtual-machines-windows-sql-automated-backup.md), która regularnie tworzy kopie zapasowe bazy danych w usłudze Blob Storage. Tej techniki można również używać ręcznie. Aby uzyskać więcej informacji, zobacz [Use Azure Storage for SQL Server Backup and Restore](virtual-machines-windows-use-storage-sql-server-backup-restore.md) (Używanie usługi Azure Storage do tworzenia kopii zapasowych programu SQL Server i ich przywracania). Omówienie wszystkich opcji tworzenia i przywracania kopii zapasowych znajduje się w temacie [Backup and Restore for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md) (Tworzenie i przywracanie kopii zapasowych programu SQL Server w usłudze Azure Virtual Machines).

### <a name="automate-updates"></a>Automatyzowanie aktualizacji
Maszyny wirtualne platformy Azure umożliwiają korzystanie z funkcji [Automatyczne stosowanie poprawek](virtual-machines-windows-sql-automated-patching.md) w celu zaplanowania okna obsługi, w którym mają być automatycznie instalowane ważne aktualizacje systemu Windows i programu SQL Server.

### <a name="customer-experience-improvement-program-(ceip)"></a>Program poprawy jakości obsługi klienta
Program poprawy jakości obsługi klienta jest domyślnie włączony. Wysyła on okresowo raporty do firmy Microsoft w celu umożliwienia doskonalenia programu SQL Server. Program poprawy jakości obsługi klienta nie wymaga przeprowadzenia żadnych czynności związanych z zarządzaniem. Wyjątek stanowi sytuacja, gdy ma on zostać wyłączony po ukończeniu aprowizacji. Program poprawy jakości obsługi klienta możesz dostosować lub wyłączyć, łącząc się z maszyną wirtualną za pomocą pulpitu zdalnego. Następnie uruchom narzędzie **SQL Server Error and Usage Reporting**. Postępuj zgodnie z instrukcjami, aby wyłączyć raportowanie. 

Aby uzyskać więcej informacji, zapoznaj się z poświęconą programowi poprawy jakości obsługi klienta częścią tematu [Accept License Terms](https://msdn.microsoft.com/library/ms143343.aspx) (Akceptacja postanowień licencyjnych). 

## <a name="next-steps"></a>Następne kroki
[Zbadaj ścieżkę szkoleniową](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) dla programu SQL Server na maszynach wirtualnych Azure.

Masz więcej pytań? Najpierw zobacz [SQL Server on Azure Virtual Machines FAQ](virtual-machines-windows-sql-server-iaas-faq.md) (Często zadawane pytania dotyczące programu SQL Server w usłudze Azure Virtual Machines). Możesz też dodać pytania lub komentarze na końcu dowolnego tematu dotyczącego maszyn wirtualnych z programem SQL, aby porozmawiać z przedstawicielem firmy Microsoft i społecznością.



<!--HONumber=Oct16_HO3-->


