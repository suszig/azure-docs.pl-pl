---
title: "Ładowanie danych do magazynu danych platformy Azure za pomocą usługi firmy Redgate | Microsoft Docs"
description: "Dowiedz się, jak używać usługi Data Platform Studio firmy Redgate na potrzeby scenariuszy magazynowania danych."
services: sql-data-warehouse
documentationcenter: NA
author: twounder
manager: jhubbard
editor: 
ms.assetid: 670aef98-31f7-4436-86c0-cc989a39fe7f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: mausher;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2548f779767635865daf790d301d86feff573a29
ms.openlocfilehash: cb0b5489ccfabb0e2a4ee412162a18930073b309
ms.lasthandoff: 01/24/2017



---
# <a name="load-data-with-redgate-data-platform-studio"></a>Ładowanie danych za pomocą usługi Data Platform Studio firmy Redgate
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
> * [BCP](sql-data-warehouse-load-with-bcp.md)
> 
> 

Ten samouczek pokazuje, w jaki sposób używać usługi [Data Platform Studio (DPS) firmy Redgate](http://www.red-gate.com/products/azure-development/data-platform-studio/), aby przenieść dane z lokalnego programu SQL Server do usługi Azure SQL Data Warehouse. Usługa Data Platform Studio stosuje najbardziej odpowiednie poprawki zgodności i optymalizacje, więc stanowi najszybszy sposób na rozpoczęcie pracy z usługą SQL Data Warehouse.

> [!NOTE]
> Firma [Redgate](http://www.red-gate.com) to wieloletni partner firmy Microsoft, który dostarcza różne narzędzia programu SQL Server. Ta funkcja w usłudze Data Platform Studio została udostępniona bezpłatnie do użytku komercyjnego i niekomercyjnego.
> 
> 

## <a name="before-you-begin"></a>Przed rozpoczęciem
### <a name="create-or-identify-resources"></a>Tworzenie lub identyfikowanie zasobów
Przed rozpoczęciem tego samouczka trzeba mieć następujące zasoby:

* **Lokalna baza danych programu SQL Server**: dane do zaimportowania do usługi SQL Data Warehouse muszą pochodzić z lokalnego programu SQL Server (wersja 2008 R2 lub nowsza). Usługa Data Platform Studio nie może bezpośrednio importować danych z usługi Azure SQL Database ani z plików tekstowych.
* **Konto usługi Azure Storage**: usługa Data Platform Studio przygotowuje dane w usłudze Azure Blob Storage przed załadowaniem ich do usługi SQL Data Warehouse. Konto magazynu musi używać modelu wdrażania usługi Resource Manager (domyślnie), a nie klasycznego modelu wdrażania. Jeśli nie masz jeszcze konta magazynu, dowiedz się, jak je utworzyć. 
* **Magazyn danych usługi SQL Data Warehouse**: w tym samouczku znajdują się informacje dotyczące przenoszenia danych z lokalnego programu SQL Server do usługi SQL Data Warehouse, więc magazyn danych musi być w trybie online. Jeśli nie masz jeszcze magazynu danych, dowiedz się, jak utworzyć magazyn danych usługi SQL Data Warehouse.

> [!NOTE]
> Można zwiększyć wydajność, jeśli konto magazynu i magazyn danych zostaną utworzone w tym samym regionie.
> 
> 

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a>Krok 1. Logowanie się do usługi Data Platform Studio za pomocą konta platformy Azure
Otwórz przeglądarkę sieci Web i przejdź do witryny sieci Web usługi [Data Platform Studio](https://www.dataplatformstudio.com/). Zaloguj się przy użyciu tego samego konta platformy Azure, które zostało użyte do utworzenia konta magazynu i magazynu danych. Jeśli Twój adres e-mail jest skojarzony zarówno z kontem służbowym, jak i kontem Microsoft, upewnij się, że wybrane zostało konto, które ma dostęp do zasobów.

> [!NOTE]
> Jeśli używasz usługi Data Platform Studio pierwszy raz, zostanie wyświetlona prośba o nadanie aplikacji uprawnień do zarządzania zasobami platformy Azure.
> 
> 

## <a name="step-2-start-the-import-wizard"></a>Krok 2. Uruchamianie kreatora importu
Z poziomu ekranu głównego usługi DPS wybierz link Import to Azure SQL Data Warehouse (Importuj do usługi Azure SQL Data Warehouse), aby uruchomić kreatora importu.

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a>Krok 3. Instalowanie bramy usługi Data Platform Studio
Aby nawiązać połączenie z lokalną bazą danych programu SQL Server, należy zainstalować bramę usługi DPS. Brama to agent klienta, który zapewnia dostęp do środowiska lokalnego, wyodrębnia dane i przekazuje je do używanego konta magazynu. Dane nigdy nie przechodzą przez serwery firmy Redgate. Aby zainstalować bramę:

1. Kliknij link **Create Gateway** (Utwórz bramę)
2. Pobierz i zainstaluj bramę przy użyciu udostępnionego instalatora

![][2]

> [!NOTE]
> Bramę można zainstalować na dowolnej maszynie z dostępem sieciowym do źródłowej bazy danych programu SQL Server. Uzyskuje ona dostęp do bazy danych programu SQL Server przy użyciu uwierzytelniania systemu Windows i poświadczeń bieżącego użytkownika.
> 
> 

Po zainstalowaniu stan bramy zmienia się na wartość Connected (Połączona) i możliwe jest wybranie pozycji Next (Dalej).

## <a name="step-4-identify-the-source-database"></a>Krok 4. Identyfikacja źródłowej bazy danych
W polu tekstowym *Enter Server Name* (Wprowadź nazwę serwera) wprowadź nazwę serwera, na którym znajduje się baza danych, a następnie wybierz przycisk **Next** (Dalej). Następnie z menu rozwijanego wybierz bazę danych, z której chcesz zaimportować dane.

![][3]

Usługa DPS sprawdza wybraną bazę danych pod kątem tabel do zaimportowania. Domyślnie usługa DPS importuje wszystkie tabele znajdujące się w bazie danych. Tabele można wybrać lub anulować ich wybór, rozwijając link All Tables (Wszystkie tabele). Wybierz przycisk Next (Dalej), aby przejść dalej.

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a>Krok 5. Wybieranie konta magazynu do przygotowania danych
Usługa DPS wyświetla monit o lokalizację, w której mają zostać przygotowane dane. Wybierz istniejące konto magazynu z subskrypcji, a następnie wybierz przycisk **Next** (Dalej).

> [!NOTE]
> Usługa DPS utworzy nowy kontener obiektów blob na wybranym koncie magazynu i użyje oddzielnego folderu dla każdego importu.
> 
> 

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Krok 6. Wybieranie magazynu danych
Następnie wybierz bazę danych usługi [Azure SQL Data Warehouse](http://aka.ms/sqldw) w trybie online, do której zostaną zaimportowane dane. Po wybraniu bazy danych wprowadź poświadczenia, aby nawiązać połączenie z bazą danych. Następnie wybierz przycisk **Next** (Dalej).

![][5]

> [!NOTE]
> Usługa DPS scala tabele danych źródłowych w magazyn danych. Usługa DPS wyświetli ostrzeżenie, jeśli ze względu na nazwę tabeli wymagane jest zastąpienie istniejących tabel w magazynie danych. Opcjonalnie można usunąć dowolne istniejące obiekty w magazynie danych, zaznaczając pole wyboru Delete all existing objects (Usuń wszystkie zaznaczone obiekty) przed zaimportowaniem.
> 
> 

## <a name="step-7-import-the-data"></a>Krok 7. Importowanie danych
Usługa DPS wyświetla potwierdzenie, czy dane mają zostać zaimportowane. Po prostu kliknij przycisk Start import (Rozpocznij import), aby rozpocząć import danych.

![][6]

Usługa DPS wyświetla wizualizację, która przedstawia postęp pobierania i przekazywania danych z lokalnego programu SQL Server i postęp importowania do usługi SQL Data Warehouse.

![][7]

Po ukończeniu importowania usługa DPS wyświetla podsumowanie importu danych i raport zmian poprawek zgodności, które zostały wprowadzone.

![][8]

## <a name="next-steps"></a>Następne kroki
Aby eksplorować dane w usłudze SQL Data Warehouse, należy rozpocząć od zapoznania się z następującymi tematami:

* [Tworzenie zapytań względem usługi Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)]
* [Wizualizacja danych przy użyciu usługi Power BI][Visualize data with Power BI]

Aby dowiedzieć się więcej o usłudze Data Platform Studio firmy Redgate:

* [Odwiedź stronę główną usługi DPS](http://www.dataplatformstudio.com/)
* [Obejrzyj pokaz usługi DPS w witrynie Channel9](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Aby uzyskać przegląd innych sposobów migracji i ładowania danych w usłudze SQL Data Warehouse, zobacz:

* [Migracja rozwiązania do usługi SQL Data Warehouse][Migrate your solution to SQL Data Warehouse]
* [Ładowanie danych do usługi Azure SQL Data Warehouse](sql-data-warehouse-overview-load.md)

Aby uzyskać więcej porad programistycznych, zobacz [Omówienie programowania w usłudze SQL Data Warehouse](sql-data-warehouse-overview-develop.md).

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Query Azure SQL Data Warehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Visualize data with Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Migrate your solution to SQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

