<properties
    pageTitle="Usługa SQL Database (PaaS) a program SQL Server w chmurze na maszynach wirtualnych (IaaS) | Microsoft Azure"
    description="Dowiedz się, która opcja programu SQL Server w chmurze jest odpowiednia do używanej aplikacji: usługa Azure SQL Database (PaaS) czy program SQL Server w chmurze na maszynach wirtualnych platformy Azure."
    services="sql-database, virtual-machines"
    keywords="chmura usługi SQL Server, SQL Server w chmurze, baza danych PaaS, chmura SQL Server, DBaaS"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="cjgronlund"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/20/2016"
    ms.author="carlrab"/>

# Wybieranie opcji programu SQL Server w chmurze: usługa Azure SQL Database (PaaS) lub program SQL Server na maszynach wirtualnych Azure (IaaS)

Platforma Azure ma dwie opcje do obsługi obciążeń programu SQL Server na platformie Microsoft Azure:

* [Azure SQL Database](https://azure.microsoft.com/services/sql-database/): baza danych SQL natywna względem chmury, zwana również bazą danych typu platforma jako usługa (PaaS) lub bazą danych jako usługa (DBaaS), zoptymalizowana pod kątem tworzenia aplikacji typu oprogramowanie jako usługa (SaaS). Zapewnia zgodność z większością funkcji programu SQL Server. Aby uzyskać więcej informacji o platformie jako usłudze, zobacz [Co to jest PaaS](https://azure.microsoft.com/overview/what-is-paas/). Aby obejrzeć film, zobacz [SAIIK: PaaS vs IaaS - Navigating the Decision Tree: Azure SQL DB vs SQL Server in a VM](https://channel9.msdn.com/Series/SAIIK-SQL-Server-on-Azure-IaaS-Implementation-Kit/SAIIK-PaaS-vs-IaaS) (SAIIK: PaaS a IaaS — Poruszanie się po drzewie decyzyjnym: Azure SQL DB a SQL Server na maszynie wirtualnej).
* [Program SQL Server w usłudze Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/): program SQL Server zainstalowany i hostowany w chmurze na maszynach wirtualnych systemu Windows Server (VM) na platformie Azure, znany również jako infrastruktura jako usługa (IaaS).

Dowiedz się, jak poszczególne opcje wpisują się w platformę danych firmy Microsoft i uzyskaj pomoc przy dopasowaniu odpowiedniej opcji do wymagań działalności biznesowej. Niezależnie od tego, czy priorytetem jest oszczędność, czy ograniczenie administracji do minimum, artykuł ten może pomóc zdecydować, które rozwiązanie pozwoli spełnić wymagania związane z działalnością biznesową, na których zależy Ci najbardziej.


## Platforma danych firmy Microsoft

Podczas każdej dyskusji dotyczącej baz danych Azure w zestawieniu z lokalnymi bazami danych programu SQL Server należy przede wszystkim pamiętać o tym, że można używać ich wszystkich. Platforma danych firmy Microsoft korzysta z technologii programu SQL Server i udostępnia ją na fizycznych komputerach lokalnych, w prywatnych środowiskach chmury, prywatnych środowiskach chmury udostępnianych przez inne firmy i w chmurze publicznej. Umożliwia to spełnienie unikatowych i różnych potrzeb związanych z prowadzeniem działalności biznesowej poprzez kombinację wdrożeń lokalnych i hostowanych w chmurze, z równoczesnym użyciem w tych środowiskach tego samego zestawu produktów serwerowych, narzędzi projektowych i zakresu wiedzy.

   ![Opcje programu SQL Server w chmurze: program SQL Server w usłudze IaaS lub baza danych SQL w usłudze SaaS w chmurze.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Jak widać to na diagramie, każde rozwiązanie można scharakteryzować według posiadanego poziomu administracji względem infrastruktury (na osi X) oraz według stopnia opłacalności wynikającej z konsolidacji poziomu bazy danych i automatyzacji (na osi Y).

Podczas projektowania aplikacji są dostępne cztery podstawowe opcje obsługi części aplikacji programu SQL Server:

- program SQL Server na niezwirtualizowanych komputerach fizycznych,
- program SQL Server na lokalnych komputerach zwirtualizowanych (w chmurze prywatnej),
- program SQL Server na maszynie wirtualnej platformy Azure (w chmurze publicznej firmy Microsoft),
- usługa Azure SQL Database (w chmurze publicznej firmy Microsoft).

Poniższe sekcje zawierają informacje na temat programu SQL Server w chmurze publicznej firmy Microsoft: usługi Azure SQL Database i programu SQL Server na maszynach wirtualnych platformy Azure. Ponadto przeanalizujemy typową motywację związaną z działalnością biznesową, aby ustalić, która opcja jest najlepsza dla aplikacji.

## Bliższe spojrzenie na usługę Azure SQL Database i program SQL Server na maszynach wirtualnych platformy Azure

**Azure SQL Database** to relacyjna bazą danych dostarczana jako usługa (DBaaS), hostowana w chmurze Azure. Można ją zaliczyć do następujących kategorii branżowych: *oprogramowanie jako usługa (SaaS)* i *platforma jako usługa (PaaS)*. [Baza danych SQL](sql-database-technical-overview.md) działa na ustandaryzowanym sprzęcie i oprogramowaniu, które stanowi własność firmy Microsoft, jest przez nią hostowane i konserwowane. W przypadku usługi SQL Database można programować bezpośrednio w usłudze przy użyciu wbudowanych funkcji i funkcjonalności. Płatność za korzystanie z usługi SQL Database odbywa się zgodnie z rzeczywistym użyciem, a opcje pozwalają na skalowanie w górę lub w poziomie, dzięki czemu klienci mają do dyspozycji większe możliwości bez najmniejszych zakłóceń.

**Program SQL Server w usłudze Azure Virtual Machines** należy do branżowej kategorii *infrastruktura jako usługa (IaaS)* i umożliwia uruchamianie programu SQL Server na maszynie wirtualnej w chmurze. Podobnie jak SQL Database, również ta usługa działa na zgodnym ze standardami sprzęcie, który jest własnością firmy Microsoft, jest przez nią hostowany i konserwowany. W przypadku korzystania z programu SQL Server na maszynie wirtualnej można użyć własnej licencji tego programu lub wstępnie licencjonowanego obrazu z portalu Azure.

Ogólnie rzecz biorąc, te dwie opcje są zoptymalizowane do różnych celów:

- **Usługa SQL Database** jest zoptymalizowana pod kątem ograniczenia do minimum całkowitych kosztów związanych z aprowizacją wielu baz danych i zarządzaniem nimi. Obniża bieżące koszty administracyjne, ponieważ nie jest konieczne zarządzanie maszynami wirtualnymi, systemem operacyjnym ani oprogramowaniem bazy danych. Dotyczy to również uaktualnień, wysokiej dostępności i [kopii zapasowych](sql-database-automated-backups.md). Ogólnie rzecz biorąc, usługa Azure SQL Database może znacząco zwiększyć liczbę baz danych zarządzanych przez pojedynczy zasób informatyczny lub projektowy.
- **Program SQL Server na maszynach wirtualnych platformy Azure** został zoptymalizowany pod kątem rozszerzania istniejących, lokalnych aplikacji SQL Server do chmury w scenariuszu hybrydowym lub pod kątem wdrażania istniejącej aplikacji do platformy Azure w scenariuszu migracji lub projektowania/testowania. Przykładem scenariusza hybrydowego może być utrzymywanie pomocniczych replik bazy danych na platformie Azure z wykorzystaniem [Sieci wirtualnych Azure](../virtual-network/virtual-networks-overview.md). W przypadku programu SQL Server na maszynach wirtualnych platformy Azure masz pełne uprawnienia administracyjne do dedykowanego wystąpienia programu SQL Server oraz maszyny wirtualnej w chmurze. To doskonałe rozwiązanie, gdy firma ma już dostępne zasoby informatyczne do obsługi maszyn wirtualnych. Program SQL Server na maszynach wirtualnych pozwala na stworzenie doskonale dopasowanego systemu, który spełni określone wymagania aplikacji związane z wydajnością i dostępnością.

Poniższa tabela zawiera podsumowanie głównych cech usługi SQL Database i programu SQL Server na maszynach wirtualnych platformy Azure:

|       | SQL Database | SQL Server na maszynie wirtualnej platformy Azure|
| -------------- | ------------ | ---------------------- |
| **Najlepsze dla:** | Nowe aplikacje zaprojektowane dla chmury, które mają ograniczenia czasowe w zakresie projektowania i marketingu. | Istniejące aplikacje, które wymagają szybkiej migracji do chmury przy minimalnych zmianach. Scenariusze szybkiego tworzenia i testowania, gdy nie chcesz kupować lokalnego nieprodukcyjnego sprzętu dla programu SQL Server. |
|| Aplikacje, które wymagają wbudowanej wysokiej dostępności, odzyskiwania po awarii i mechanizmów uaktualniania. | Aplikacje, które nie wymagają wbudowanej wysokiej dostępności, odzyskiwania po awarii i mechanizmów uaktualniania. |
||Zespoły, które nie chcą zarządzać podstawowymi ustawieniami systemu operacyjnego i konfiguracji.| Jeśli potrzebne jest dostosowane środowisko IT z pełnymi prawami administracyjnymi.|
||Bazy danych o rozmiarze do 1 TB lub większe bazy danych, które mogą być [partycjonowane poziomo lub pionowo](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) przy użyciu wzorca skalowalnego w poziomie.|Bazy danych większe niż 1 TB, których nie można [partycjonować poziomo ani pionowo](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling).|
||[Tworzenie aplikacji typu oprogramowanie jako usługa (SaaS)](sql-database-design-patterns-multi-tenancy-saas-applications.md).| Tworzenie aplikacji hybrydowych.|
|||||
|**Zasoby:**|Nie chcesz używać zasobów IT do obsługi i utrzymania podstawowej infrastruktury, ale chcesz skupić się na warstwie aplikacji.|Masz zasoby informatyczne do obsługi i utrzymania.|
|**Całkowity koszt posiadania:**|Eliminuje koszty sprzętu i ogranicza koszty administracyjne.|Eliminuje koszty sprzętu.|
|**Ciągłość działalności biznesowej:**|Oprócz możliwości infrastruktury w zakresie wbudowanej odporności na uszkodzenia usługa Azure SQL Database zapewnia takie funkcje jak [automatyczne kopie zapasowe](sql-database-automated-backups.md), [przywracanie do punktu w czasie](sql-database-recovery-using-backups.md#point-in-time-restore), [przywracanie geograficzne](sql-database-recovery-using-backups.md#geo-restore) i [aktywna replikacja geograficzna](sql-database-active-geo-replication.md), aby zwiększyć ciągłość prowadzenia działalności biznesowej. Więcej informacji znajduje się w temacie [Omówienie ciągłości działalności biznesowej usługi SQL Database](sql-database-business-continuity.md).|Program SQL Server na maszynach wirtualnych platformy Azure umożliwia skonfigurowanie rozwiązania wysokiej dostępności i odzyskiwania po awarii w zależności od określonych potrzeb bazy danych. Można mieć zatem system w znacznym stopniu zoptymalizowany dla aplikacji. Jeśli wystąpi taka potrzeba, tryb failover można przetestować i uruchomić samodzielnie. Więcej informacji znajduje się w temacie [Wysoka dostępność i odzyskiwanie po awarii dla programu SQL Server w usłudze Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).|
|**Chmura hybrydowa:**|Aplikacja lokalna może uzyskiwać dostęp do danych w usłudze Azure SQL Database.|Program SQL Server na maszynach wirtualnych platformy Azure może obejmować aplikacje, które działają częściowo w chmurze i częściowo lokalnie. Można na przykład rozszerzyć sieć lokalną i domenę usługi Active Directory do chmury za pośrednictwem usługi [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Ponadto można przechowywać lokalne pliki danych w usłudze Azure Storage przy użyciu [plików danych programu SQL Server na platformie Azure](http://msdn.microsoft.com/library/dn385720.aspx). Więcej informacji znajduje się w temacie [Wprowadzenie do chmury hybrydowej programu SQL Server 2014](http://msdn.microsoft.com/library/dn606154.aspx).|
||Obsługuje [replikację transakcyjną programu SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) jako subskrybent.|Obsługuje [replikację transakcyjną programu SQL Server](https://msdn.microsoft.com/library/mt589530.aspx), odzyskiwanie po awarii i [zawsze włączone repliki na maszynach wirtualnych platformy Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).|
|||||
|||||

## Względy biznesowe przemawiające za wyborem usługi Azure SQL Database lub programu SQL Server na maszynach wirtualnych Azure

### Koszty

Ograniczone fundusze są często podstawowym czynnikiem decydującym o wyborze sposobu hostowania baz danych — niezależnie od tego, czy Twoja firma to start-up o ograniczonych środkach finansowych, czy należysz do zespołu firmowego, który musi zmieścić się w zaplanowanym budżecie. W tej sekcji poznamy najpierw podstawowe informacje dotyczące rozliczeń i licencjonowania platformy Azure w odniesieniu do dwóch opcji obsługi relacyjnych baz danych: usługi SQL Database i programu SQL Server na maszynach wirtualnych platformy Azure. Następnie zajmiemy się tym, jak należy obliczyć całkowity koszt aplikacji.

#### Podstawowe informacje dotyczące rozliczeń i licencjonowania

Usługa **SQL Database** jest sprzedawana w formie usługi, a nie licencji, podczas gdy [licencjonowanie programu SQL Server na maszynach wirtualnych platformy Azure](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md) wymaga licencjonowania programu SQL Server za minutę lub własnej licencji za pośrednictwem programu Software Assurance.

Obecnie usługa **SQL Database** jest dostępna w kilku warstwach, z których wszystkie są rozliczane godzinowo według stałej stawki ustalanej na podstawie warstwy usługi i wybranego poziomu wydajności. Rozliczany jest również internetowy ruch wychodzący po zwykłych [stawkach transferu danych](https://azure.microsoft.com/pricing/details/data-transfers/). Warstwy usług Podstawowa, Standardowa i Premium zapewniają przewidywalną wydajność i obejmują wiele poziomów wydajności, aby spełnić szczytowe wymagania aplikacji. Warstwy usług i poziomy wydajności można zmieniać, aby odpowiadały one zróżnicowanym potrzebom aplikacji związanym z przepływnością. Jeśli baza danych obejmuje dużą liczbę transakcji i musi obsługiwać wielu użytkowników równocześnie, zalecamy warstwę Premium. Najnowsze informacje dotyczące bieżących obsługiwanych warstw usług można znaleźć w temacie [Warstwy usług w usłudze Azure SQL Database](sql-database-service-tiers.md). Można również utworzyć [elastyczne pule baz danych](sql-database-elastic-pool.md) do współdzielenia zasobów wydajności między wystąpieniami bazy danych.

W przypadku usługi **SQL Database** oprogramowanie bazy danych jest automatycznie konfigurowane, poprawiane i uaktualniane przez firmę Microsoft, co obniża koszty administracyjne. Ponadto [wbudowana funkcja tworzenia kopii zapasowych](sql-database-automated-backups.md) pozwala osiągnąć znaczne oszczędności kosztów, zwłaszcza w przypadku dużej liczby baz danych.

Dzięki **programowi SQL Server na maszynach wirtualnych platformy Azure** można używać udostępnianego na platformie obrazu programu SQL Server (który obejmuje licencję) lub przenieść swoją licencję programu SQL Server. W przypadku użycia obrazów dostarczanych przez platformę Azure koszty operacyjne zależą od rozmiaru maszyny wirtualnej, a także od wybranej wersji programu SQL Server. Niezależnie od rozmiaru maszyny wirtualnej lub wersji programu SQL Server co minutę naliczana jest opłata licencyjna za program SQL Server i system Windows Server, wraz z kosztami korzystania z usługi Azure Storage i dysków maszyny wirtualnej. Opcja rozliczania co minutę pozwala używać programu SQL Server tak długo, jak jest to konieczne bez wykupywania dodatkowych licencji programu SQL Server. W przypadku przeniesienia licencji programu SQL Server na platformę Azure naliczane są tylko opłaty za korzystanie z systemu Windows Server i magazynu. Więcej informacji na temat przenoszenia własnej licencji można znaleźć w temacie [Przenośność licencji za pośrednictwem programu Software Assurance w systemie Azure](https://azure.microsoft.com/pricing/license-mobility/).

#### Obliczanie całkowitego kosztu aplikacji

Po rozpoczęciu pracy z platformą w chmurze koszt obsługi aplikacji obejmuje przede wszystkim koszty programowania i administracji wraz z kosztami usługi publicznej platformy w chmurze.

Oto szczegółowe obliczenie kosztów obsługi aplikacji uruchomionej w usłudze SQL Database i programie SQL Server na maszynach wirtualnych platformy Azure:

**W przypadku korzystania z usługi Azure SQL Database:**

*Całkowity koszt aplikacji = znacznie ograniczone koszty administracyjne + koszty rozwoju oprogramowania + koszty usługi SQL Database*

**W przypadku korzystania z programu SQL Server na maszynach wirtualnych platformy Azure:**

*Całkowity koszt aplikacji = ograniczone do minimum koszty programowania/modyfikacji oprogramowania + koszty administracyjne + koszty licencjonowania programu SQL Server i systemu Windows Server + koszty usługi Azure Storage*

Więcej informacji na temat cen zawierają następujące zasoby:

- [SQL Database — cennik](https://azure.microsoft.com/pricing/details/sql-database/)
- [Maszyny wirtualne — cennik](https://azure.microsoft.com/pricing/details/virtual-machines/) ([SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) i [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows))
- [Kalkulator cen platformy Azure](https://azure.microsoft.com/pricing/calculator/)

> [AZURE.NOTE] W programie SQL Server istnieje niewielki podzbiór funkcji, które nie są używane w usłudze SQL Database lub nie są w niej dostępne. Więcej informacji znajduje się w tematach [SQL Database General Limitations and Guidelines](sql-database-general-limitations.md) (Ogólne ograniczenia i wytyczne dotyczące usługi SQL Database) i [SQL Database Transact-SQL information](sql-database-transact-sql-information.md) (Informacje o programie Transact-SQL w usłudze SQL Database). W przypadku przenoszenia istniejącego rozwiązania programu SQL Server do chmury zobacz temat [Migrating a SQL Server database to Azure SQL Database](sql-database-cloud-migrate.md) (Migrowanie bazy danych programu SQL Server do usługi Azure SQL Database). Podczas migracji istniejącej lokalnej aplikacji SQL Server do usługi SQL Database należy wziąć pod uwagę aktualizację aplikacji, co pozwoli wykorzystać możliwości, które oferują usługi w chmurze. Można na przykład rozważyć użycie usługi [Azure Web App Service](https://azure.microsoft.com/services/app-service/web/) lub [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) do obsługi warstwy aplikacji i zwiększenia oszczędności.

### Administracja

Dla wielu firm decyzja o przeniesieniu usług do usługi w chmurze wiąże się zarówno z ograniczeniem złożoności w zakresie administracji, jak i obniżeniem kosztów. Przy użyciu usługi **SQL Database** firma Microsoft zarządza podstawowym sprzętem, automatycznie replikuje wszystkie dane w celu zapewnienia dużej dostępności, konfiguruje i uaktualnia oprogramowanie bazy danych, zarządza równoważeniem obciążeń, a w przypadku awarii serwera w nieodczuwalny dla klienta sposób obsługuje tryb failover. Możesz samodzielnie administrować swoją bazą danych, ale nie musisz zarządzać aparatem bazy danych, systemem operacyjnym serwera ani sprzętem.  Przykładowe elementy, którymi można dalej zarządzać to bazy danych, identyfikatory logowania, dostrajanie indeksów i zapytań oraz inspekcja i zabezpieczenia.

Z drugiej strony być może dysponujesz wiedzą dostępną tylko w Twojej firmie i chcesz mieć kontrolę nad lokalizacją bazy danych (aż do lokalizacji dysku). **Program SQL Server uruchomiony na maszynach wirtualnych platformy Azure** pozwala na pełną kontrolę nad konfiguracją systemu operacyjnego i wystąpienia programu SQL Server. W przypadku korzystania z maszyny wirtualnej decydujesz o tym, kiedy należy zaktualizować/uaktualnić system operacyjny i oprogramowanie bazy danych i kiedy zainstalować dodatkowe oprogramowanie, np. oprogramowanie antywirusowe czy narzędzia do tworzenia kopii zapasowych. Ponadto możesz kontrolować rozmiar maszyny wirtualnej, liczbę dysków oraz ich konfiguracje magazynu.  Na przykład platforma Azure umożliwia, jeśli wystąpi taka potrzeba, zmianę rozmiaru maszyny wirtualnej zgodnie z potrzebami. Więcej informacji można znaleźć w temacie [Virtual Machine and Cloud Service Sizes for Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Rozmiary maszyn wirtualnych i usług w chmurze na platformie Azure).

### Umowa dotycząca poziomu usług (SLA)

Dla wielu działów IT wypełnienie zobowiązań wynikających z umowy dotyczącej poziomu usług (SLA) ma najwyższy priorytet. W tej sekcji opisano, jakie warunki umowy SLA stosuje się do poszczególnych opcji obsługi bazy danych.

W przypadku warstw usług Podstawowej, Standardowej i Premium usługi **SQL Database** firma Microsoft gwarantuje dostępność na poziomie 99,99%. Najnowsze informacje można znaleźć w artykule [SQL Database — umowa SLA](https://azure.microsoft.com/support/legal/sla/sql-database/). Najnowsze informacje dotyczące warstw usługi SQL Database oraz obsługiwanych planów ciągłości działalności biznesowej znajdują się w temacie [Warstwy usług](sql-database-service-tiers.md).

W przypadku **programu SQL Server uruchomionego na maszynach wirtualnych platformy Azure** firma Microsoft zapewnia dostępność na poziomie 99,95%, która obejmuje tylko maszyny wirtualne. Umowa SLA nie obejmuje procesów (np. programu SQL Server) uruchomionych na maszynie wirtualnej i wymaga obsługi przynajmniej dwóch wystąpień maszyny wirtualnej w zbiorze dostępności. Najnowsze informacje znajdują się w artykule [Maszyny wirtualne — umowa SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Aby uzyskać wysoką dostępność bazy danych w maszynach wirtualnych, należy skonfigurować jedną z obsługiwanych opcji wysokiej dostępności w programie SQL Server, np. [Zawsze włączone grupy dostępności](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx).

### <a name="market"></a>Czas wprowadzenia na rynek

Usługa **SQL Database** to odpowiednie rozwiązanie do obsługi aplikacji przeznaczonych do chmury, gdy wydajność deweloperów i krótki czas wprowadzania na rynek mają decydujące znaczenie. Dzięki funkcjonalności przypominającej model DBA jest doskonała dla architektów i deweloperów chmury, ponieważ zmniejsza potrzebę zarządzania bazowym systemem operacyjnym i bazą danych. Na przykład można użyć [interfejsu API REST](http://msdn.microsoft.com/library/azure/dn505719.aspx) i [poleceń cmdlet programu PowerShell](http://msdn.microsoft.com/library/azure/dn546726.aspx) do automatyzacji operacji administracyjnych dla tysięcy baz danych oraz zarządzania nimi. Funkcje takie jak [elastyczne pule baz danych](sql-database-elastic-pool.md) pozwalają skupić się na warstwie aplikacji i szybciej wprowadzić rozwiązanie na rynek.

**Program SQL Server uruchomiony na maszynach wirtualnych platformy Azure** jest doskonałym rozwiązaniem, jeśli istniejące lub nowe aplikacje wymagają dostępu do wszystkich funkcji wystąpienia programu SQL Server oraz kontroli nad nimi. Jest również odpowiedni, jeśli chcesz migrować istniejące lokalne aplikacje i bazy danych do platformy Azure w niezmienionej postaci. Ponieważ nie musisz wprowadzać zmian w prezentacji, aplikacji i warstwach danych, jeśli rezygnujesz z ponownego projektowania istniejącego rozwiązania, oszczędzasz czas i pieniądze. Zamiast tego możesz skoncentrować się na migracji wszystkich rozwiązań do platformy Azure i przeprowadzeniu optymalizacji wydajności, których może wymagać platforma Azure. Więcej informacje zawiera artykuł [Performance Best Practices for SQL Server on Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-performance.md) (Najlepsze praktyki dotyczące wydajności dla programu SQL Server w usłudze Azure Virtual Machines).

## Podsumowanie

W tym artykule omówiono usługę SQL Database i program SQL Server w usłudze Azure Virtual Machines oraz względy biznesowe, które mogą mieć wpływ na wybór jednego z tych rozwiązań. Poniżej znajduje się podsumowanie sugestii, które należy wziąć pod uwagę:

Wybierz usługę **Azure SQL Database**, jeśli:

- Tworzysz nowe aplikacje oparte na chmurze lub chcesz przeprowadzić migrację istniejącego rozwiązania programu SQL Server w taki sposób, aby skorzystać z oszczędności kosztów i optymalizacji wydajności, które zapewniają usługi w chmurze. Takie rozwiązanie zapewnia korzyści w pełni zarządzanej usługi w chmurze, pomaga skrócić początkowy czas wprowadzenia na rynek i pozwala uzyskać długoterminową optymalizację kosztów.

- Chcesz, aby firma Microsoft wykonywała zwyczajowe operacje zarządzania na bazach danych i potrzebujesz umów SLA gwarantujących wyższą dostępność baz danych.



Wybierz rozwiązanie **SQL Server na maszynach wirtualnych Azure**, jeśli:

- Masz istniejące aplikacje lokalne i chcesz zaprzestać utrzymywania własnego sprzętu lub rozważasz rozwiązania hybrydowe. To rozwiązanie umożliwia szybsze uzyskanie dużej wydajności bazy danych podczas ułatwiania połączenia z aplikacjami lokalnymi za pośrednictwem bezpiecznego tunelu.

- Masz istniejące zasoby IT, potrzebujesz pełnych uprawnień administracyjnych do programu SQL Server i wymagasz pełnej zgodności z lokalnym programem SQL Server. To rozwiązanie umożliwia ograniczenie do minimum kosztów programowania lub modyfikacji istniejących aplikacji, zapewniając elastyczność pozwalającą na uruchamianie większości aplikacji. Ponadto umożliwia pełną kontrolę nad maszyną wirtualną, systemem operacyjnym i konfiguracją bazy danych.


## Następne kroki
- Aby rozpocząć korzystanie z usługi SQL Database, zobacz temat [Samouczek SQL Database: tworzenie bazy danych SQL w kilka minut za pomocą witryny Azure Portal](sql-database-get-started.md).
- Zobacz [Cennik usługi SQL Database] (https://azure.microsoft.com/pricing/details/sql-database/).
- Aby rozpocząć pracę z programem SQL Server na maszynach wirtualnych platformy Azure, zobacz temat [Aprowizowanie maszyny wirtualnej programu SQL Server w Portalu Azure](../virtual-machines/virtual-machines-windows-portal-sql-server-provision.md)
- Zobacz [Program SQL Server na maszynie wirtualnej platformy Azure: ścieżka szkoleniowa](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).



<!--HONumber=sep16_HO1-->


