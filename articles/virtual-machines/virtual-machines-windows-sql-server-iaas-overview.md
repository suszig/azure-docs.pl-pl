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
    ms.date="08/29/2016"
    ms.author="jroth"/>

# Omówienie programu SQL Server w usłudze Azure Virtual Machines

W tym temacie opisano opcje uruchamiania programu SQL Server na maszynach wirtualnych Azure oraz zamieszczono [linki do obrazów portalu](#option-1-deploy-a-sql-vm-per-minute-licensing) i przegląd [typowych zadań](#manage-your-sql-vm).

>[AZURE.NOTE] Jeśli znasz już program SQL Server i chcesz tylko zobaczyć, jak wdrożyć maszynę wirtualną programu SQL Server, zobacz temat [Aprowizowanie maszyny wirtualnej programu SQL Server w Portalu Azure](virtual-machines-windows-portal-sql-server-provision.md).

## Omówienie
Może jesteś administratorem bazy danych, który chce przenieść obciążenia lokalnego programu SQL Server do chmury lub deweloperem, który rozważa możliwości relacyjnej bazy danych programu SQL Server w kontekście aplikacji platformy Azure. Jakie są zalety uruchamiania obciążeń programu SQL Server na maszynach wirtualnych Azure? Poniższy informacyjny klip wideo omawia zalety i zawiera opis techniczny.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

## Ocena korzyści

Przed rozpoczęciem działań należy ocenić zalety użycia programu SQL Server na maszynach wirtualnych Azure.

Jeśli przenosisz inne obciążenia na platformę Azure, np. aplikację przedsiębiorstwa, dobrym pomysłem będzie również przeniesienie wszelkich zależnych baz danych SQL Server na platformę Azure w celu zwiększenia wydajności. Niemniej hostowanie programu SQL Server na maszynach wirtualnych Azure zapewnia też inne korzyści. Przykładowo automatycznie uzyskujesz dostęp do wielu centrów danych, co zapewnia globalną obecność i odzyskiwanie po awarii. Aby uzyskać pełną listę scenariuszy i korzyści, zobacz stronę produktu [Program SQL Server na maszynach wirtualnych](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [AZURE.NOTE] Podczas oceniania programu SQL Server na maszynach wirtualnych Azure zapoznaj się również z innymi opcjami magazynu i rozwiązań SQL dostępnymi na platformie Azure, takimi jak [SQL Database](../sql-database/sql-database-technical-overview.md), [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) i [baza danych SQL Server Stretch](../sql     -server-stretch-database/sql-server-stretch-database-overview.md). Aby uzyskać jedno szczegółowe porównanie, zobacz [Choose a cloud SQL Server option: Azure SQL (PaaS) Database or SQL Server on Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) (Wybór opcji programu SQL Server w chmurze: Baza danych SQL Azure [PaaS] lub program SQL Server na maszynach wirtualnych Azure [IaaS]).

W przypadku podjęcia decyzji o uruchomieniu programu SQL Server na maszynach wirtualnych Azure jednym z pierwszych wyborów będzie zdecydowanie o tym, czy zostanie użyty obraz maszyny wirtualnej obejmujący koszty licencjonowania programu SQL Server. Drugą opcją jest używanie własnej licencji (BYOL, bring your own license) i ponoszenie kosztów samej maszyny wirtualnej. W dwóch następnych sekcjach opisano te opcje.

## Opcja 1. Wdrożenie maszyny wirtualnej z programem SQL (licencjonowanie za minutę)
Poniższa tabela zawiera macierz dostępnych obrazów programu SQL Server w galerii maszyn wirtualnych. Kliknij dowolny link, aby rozpocząć tworzenie nowej maszyny wirtualnej z programem SQL o określonej wersji, wybranym wydaniu i z danym systemem operacyjnym. Wszystkie obrazy obejmują [koszty licencjonowania programu SQL Server](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Wskazówki krok po kroku są dostępne w samouczku [Aprowizowanie maszyny wirtualnej programu SQL Server w witrynie Azure Portal](virtual-machines-windows-portal-sql-server-provision.md). Sprawdź również temat [Performance best practices for SQL Server VMs](virtual-machines-windows-sql-performance.md) (Najlepsze rozwiązania umożliwiające zwiększenie wydajności maszyn wirtualnych z programem SQL Server), który wyjaśnia, w jaki sposób wybrać odpowiedni rozmiar maszyny i inne funkcje dostępne podczas aprowizacji.

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

## Opcja 2. Wdrożenie maszyny wirtualnej z programem SQL (BYOL)
Druga opcja to użycie własnej licencji (BYOL). W tym scenariuszu płacisz wyłącznie za maszynę wirtualną i nie ponosisz żadnych dodatkowych kosztów licencjonowania programu SQL Server. Aby użyć własnej licencji, skorzystaj z macierzy wersji i wydań programu SQL Server oraz systemów operacyjnych. W portalu nazwy obrazów są poprzedzane prefiksem **{BYOL}**.

> [AZURE.IMPORTANT] Aby używać obrazów maszyn wirtualnych w modelu BYOL, musisz mieć umowę Enterprise Agreement z opcją [Przenośność licencji za pośrednictwem programu Software Assurance na platformie Azure](https://azure.microsoft.com/pricing/license-mobility/). Musisz również mieć ważną licencję dla wersji/wydania programu SQL Server, którego chcesz użyć. W ciągu **10** dni od aprowizacji maszyny wirtualnej musisz [przekazać firmie Microsoft informacje niezbędne do korzystania z modelu BYOL](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf).

Wskazówki w [samouczku dotyczącym aprowizacji](virtual-machines-windows-portal-sql-server-provision.md) mają zastosowanie, ale musisz użyć jednej z następujących opcji obrazu **BYOL**. Sprawdź również temat [Performance best practices for SQL Server VMs](virtual-machines-windows-sql-performance.md) (Najlepsze rozwiązania umożliwiające zwiększenie wydajności maszyn wirtualnych z programem SQL Server), który wyjaśnia, w jaki sposób wybrać odpowiedni rozmiar maszyny i inne funkcje dostępne podczas aprowizacji.

|Wersja|System operacyjny|Wersja|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

## Zarządzanie maszyną wirtualną z programem SQL
Po aprowizacji maszyny wirtualnej z programem SQL Server możesz wykonać kilka opcjonalnych zadań związanych z zarządzaniem. W niektórych aspektach możesz konfigurować program SQL Server i zarządzać nim dokładnie tak, jak w przypadku instalacji lokalnej. Jednak niektóre zadania są specyficzne dla platformy Azure. W poniższych sekcjach omówiono niektóre z tych obszarów i podano linki do dalszych informacji.

### Migrowanie danych

Jeśli masz istniejącą bazę danych, najprawdopodobniej będziesz ją przenosić do nowo aprowizowanej maszyny wirtualnej z programem SQL. Aby uzyskać listę opcji migracji i wskazówki, zobacz [Migrating a Database to SQL Server on an Azure VM](virtual-machines-windows-migrate-sql.md) (Migrowanie bazy danych do programu SQL Server na maszynie wirtualnej portalu Azure).

### Konfigurowanie wysokiej dostępności

Jeśli potrzebujesz wysokiej dostępności, rozważ skonfigurowanie grup dostępności programu SQL Server. Obejmuje to wiele maszyn wirtualnych Azure w sieci wirtualnej. W Portalu Azure znajduje się szablon, który przeprowadzi konfigurację za Ciebie. Aby uzyskać więcej informacji, zobacz [Configure an AlwaysOn availability group in Azure Resource Manager virtual machines](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) (Konfigurowanie zawsze włączonej grupy dostępności na maszynach wirtualnych korzystających z usługi Azure Resource Manager). Jeśli chcesz ręcznie skonfigurować grupę dostępności i skojarzony odbiornik, zobacz [Configure AlwaysOn Availability Groups in Azure VM](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md) (Konfigurowanie zawsze włączonych grup dostępności na maszynie wirtualnej Azure).

Inne zagadnienia dotyczące wysokiej dostępności można znaleźć w temacie [High Availability and Disaster Recovery for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md) (Wysoka dostępność i odzyskiwanie po awarii dla programu SQL Server w usłudze Azure Virtual Machines).

### Tworzenie kopii zapasowej danych
Maszyny wirtualne Azure umożliwiają korzystanie z funkcji [Automatyczne kopie zapasowe](virtual-machines-windows-sql-automated-backup.md), która regularnie tworzy kopie zapasowe bazy danych w usłudze Blob Storage. Tej techniki można również używać ręcznie. Aby uzyskać więcej informacji, zobacz [Use Azure Storage for SQL Server Backup and Restore](virtual-machines-windows-use-storage-sql-server-backup-restore.md) (Używanie usługi Azure Storage do tworzenia kopii zapasowych programu SQL Server i ich przywracania). Omówienie wszystkich opcji tworzenia i przywracania kopii zapasowych znajduje się w temacie [Backup and Restore for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md) (Tworzenie i przywracanie kopii zapasowych programu SQL Server w usłudze Azure Virtual Machines).

### Automatyzowanie aktualizacji
Maszyny wirtualne platformy Azure umożliwiają korzystanie z funkcji [Automatyczne stosowanie poprawek](virtual-machines-windows-sql-automated-patching.md) w celu zaplanowania okna obsługi, w którym mają być automatycznie instalowane ważne aktualizacje systemu Windows i programu SQL Server.

### Program poprawy jakości obsługi klienta
Program poprawy jakości obsługi klienta jest domyślnie włączony. Nie jest to zadanie związane z zarządzaniem, chyba że chcesz wyłączyć program poprawy jakości obsługi klienta po aprowizacji. Program poprawy jakości obsługi klienta możesz dostosować lub wyłączyć, łącząc się z maszyną wirtualną za pomocą pulpitu zdalnego. Następnie uruchom narzędzie **SQL Server Error and Usage Reporting**. Postępuj zgodnie z instrukcjami, aby wyłączyć raportowanie.

## Następne kroki
[Zbadaj ścieżkę szkoleniową](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) dla programu SQL Server na maszynach wirtualnych Azure.

Masz więcej pytań? Najpierw zobacz [SQL Server on Azure Virtual Machines FAQ](virtual-machines-windows-sql-server-iaas-faq.md) (Często zadawane pytania dotyczące programu SQL Server w usłudze Azure Virtual Machines). Możesz też dodać pytania lub komentarze na końcu dowolnego tematu dotyczącego maszyn wirtualnych z programem SQL, aby porozmawiać z przedstawicielem firmy Microsoft i społecznością.



<!--HONumber=sep16_HO1-->


