---
title: "Aprowizowanie środowiska Azure SSIS Integration Runtime przy użyciu portalu | Microsoft Docs"
description: "W tym artykule opisano sposób tworzenia środowiska Azure SSIS Integration Runtime przy użyciu witryny Azure Portal."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: hero-article
ms.date: 01/09/2018
ms.author: spelluru
ms.openlocfilehash: b6a795f8a26340f24f9e09aea371ba90afe50101
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2018
---
# <a name="provision-an-azure-ssis-integration-runtime-by-using-the-data-factory-ui"></a>Aprowizowanie środowiska Azure SSIS Integration Runtime przy użyciu interfejsu użytkownika usługi Data Factory
Ten samouczek zawiera instrukcje aprowizacji środowiska Azure SSIS Integration Runtime (IR) w usłudze Azure Data Factory przy użyciu witryny Azure Portal. Następnie możesz użyć programu SQL Server Data Tools (SSDT) lub SQL Server Management Studio (SSMS) do wdrożenia pakietów usług SQL Server Integration Services (SSIS) w tym środowisku uruchomieniowym na platformie Azure. Aby uzyskać koncepcyjne informacje dotyczące środowiska Azure SSIS IR, zobacz [Omówienie środowiska Azure-SSIS Integration Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime).

W tym samouczku wykonasz następujące czynności:

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Tworzenie i uruchamianie środowiska Azure-SSIS Integration Runtime

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz [dokumentację dotyczącą usługi Data Factory w wersji 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="prerequisites"></a>Wymagania wstępne
- **Subskrypcja platformy Azure**. Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto. 
- **Serwer usługi Azure SQL Database**. Jeśli nie masz jeszcze serwera bazy danych, utwórz go w witrynie Azure Portal, zanim zaczniesz. Usługa Azure Data Factory tworzy bazę danych wykazu usług SSIS (SSISDB) na tym serwerze bazy danych. Zaleca się utworzenie serwera bazy danych w tym samym regionie platformy Azure, co środowisko Integration Runtime. Ta konfiguracja pozwala środowisku Integration Runtime zapisywać dzienniki wykonywania SSISDB bez wykraczania poza granice regionów świadczenia usług platformy Azure. 
    - Upewnij się, że ustawienie „**Zezwalaj na dostęp do usług platformy Azure**” jest włączone dla serwera bazy danych. Aby uzyskać więcej informacji, zobacz artykuł [Secure your Azure SQL database (Zabezpieczenia bazy danych Azure SQL)](../sql-database/sql-database-security-tutorial.md#create-a-server-level-firewall-rule-in-the-azure-portal). Aby włączyć to ustawienie za pomocą programu PowerShell, zobacz polecenie [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule?view=azurermps-4.4.1).
    - Do listy adresów IP klienta w ustawieniach zapory dla serwera bazy danych dodaj adres IP maszyny klienta lub zakres adresów IP, który zawiera adres IP maszyny klienta. Aby uzyskać więcej informacji, zobacz [Azure SQL Database server-level and database-level firewall rules (Reguły zapory na poziomie serwera i na poziomie bazy danych usługi Azure SQL Database)](../sql-database/sql-database-firewall-configure.md). 
 
## <a name="create-a-data-factory"></a>Tworzenie fabryki danych

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).    
2. Kliknij przycisk **Nowy** w lewym menu, kliknij pozycję **Dane + analiza**, a następnie kliknij pozycję **Data Factory**. 
   
   ![Nowy-> Fabryka danych](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. Na stronie **Nowa fabryka danych** wprowadź jako **nazwę** wartość **MyAzureSsisDataFactory**. 
      
     ![Strona Nowa fabryka danych](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   Nazwa fabryki danych platformy Azure musi być **globalnie unikatowa**. Jeśli wystąpi poniższy błąd, zmień nazwę fabryki danych (np. twojanazwaMyAzureSsisDataFactory) i spróbuj utworzyć ją ponownie. Artykuł [Data Factory — Naming Rules (Usługa Data Factory — reguły nazewnictwa)](naming-rules.md) zawiera reguły nazewnictwa artefaktów usługi Data Factory.
  
       `Data factory name “MyAzureSsisDataFactory” is not available`
3. Wybierz **subskrypcję** Azure, w której chcesz utworzyć fabrykę danych. 
4. Dla opcji **Grupa zasobów** wykonaj jedną z następujących czynności:
     
      - Wybierz pozycję **Użyj istniejącej**, a następnie wybierz istniejącą grupę zasobów z listy rozwijanej. 
      - Wybierz pozycję **Utwórz nową**, a następnie wprowadź nazwę grupy zasobów.   
         
      Informacje na temat grup zasobów znajdują się w artykule [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Używanie grup zasobów do zarządzania zasobami platformy Azure).  
4. Wybierz wartość **V2 (wersja zapoznawcza)** dla **wersji**.
5. Na liście **lokalizacja** wybierz lokalizację fabryki danych. Na liście są wyświetlane tylko lokalizacje obsługiwane na potrzeby tworzenia fabryk danych.
6. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**.     
7. Kliknij przycisk **Utwórz**.
8. Na pulpicie nawigacyjnym jest widoczny następujący kafelek ze stanem: **Wdrażanie fabryki danych**. 

    ![kafelek Wdrażanie fabryki danych](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Po zakończeniu tworzenia zostanie wyświetlona strona **Fabryka danych**, jak pokazano na poniższej ilustracji.
   
   ![Strona główna fabryki danych](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Kliknij pozycję **Tworzenie i monitorowanie**, aby w osobnej karcie uruchomić interfejs użytkownika usługi Data Factory. 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Aprowizowanie środowiska Azure SSIS Integration Runtime

1. Na stronie wprowadzenia kliknij kafelek **Konfigurowanie środowiska SSIS Integration Runtime**. 

   ![Kafelek Konfigurowanie środowiska SSIS Integration Runtime](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. Na stronie **Ustawienia ogólne** kreatora **konfiguracji środowiska Integration Runtime** wykonaj następujące czynności: 

   ![Ustawienia ogólne](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    1. Określ **nazwę** środowiska Integration Runtime.
    2. Wybierz **lokalizację** środowiska Integration Runtime. Wyświetlane są tylko obsługiwane lokalizacje.
    3. Wybierz **rozmiar węzła**, który ma zostać skonfigurowany ze środowiskiem uruchomieniowym usług SSIS.
    4. Określ **liczbę węzłów** w klastrze.
    5. Kliknij przycisk **Dalej**. 
1. W obszarze **Ustawienia SQL** wykonaj następujące czynności: 

    ![Ustawienia SQL](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    1. Określ **subskrypcję** platformy Azure, która ma serwer usługi SQL Azure. 
    2. Wybierz serwer usługi SQL Azure dla **punktu końcowego serwera bazy danych katalogu**.
    3. Wprowadź nazwę użytkownika **administratora**.
    4. Wprowadź **hasło** dla administratora.  
    5. Wybierz **warstwę usług** dla bazy danych SSISDB. Wartość domyślna to Podstawowa.
    6. Kliknij przycisk **Dalej**. 
1.  Na stronie **Ustawienia zaawansowane** wybierz wartość dla ustawienia **Maximum Parallel Executions Per Node** (Maksymalna liczba równoległych wykonań na węzeł).   

    ![Ustawienia zaawansowane](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Ten krok jest **opcjonalny**. Jeśli masz klasyczną sieć wirtualną (VNet), do której chcesz dołączyć środowisko Integration Runtime, wybierz opcję **Select a VNet for your Azure-SSIS integration runtime to join and allow Azure services to configure VNet permissions/settings** (Wybierz sieć wirtualną dla środowiska Azure-SSIS Integration Runtime do dołączenia i zezwalaj usługom platformy Azure na konfigurowanie uprawnień/ustawień sieci wirtualnej) i wykonaj następujące czynności: 

    ![Ustawienia zaawansowane za pomocą sieci wirtualnej](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

    1. Określ **subskrypcję**, która ma klasyczną sieć wirtualną. 
    2. Wybierz **sieć wirtualną**. <br/>
    4. Wybierz **podsieć**.<br/> 
1. Kliknij pozycję **Zakończ**, aby rozpocząć tworzenie środowiska Azure-SSIS Integration Runtime. 

    > [!IMPORTANT]
    > - Ukończenie tego procesu zajmuje około 20 minut
    > - Usługa Data Factory łączy się z usługą Azure SQL Database w celu przygotowania bazy danych wykazu usług SSIS (SSISDB). Skrypt konfiguruje również uprawnienia i ustawienia Twojej sieci wirtualnej, jeśli zostanie określona, i dołącza nowe wystąpienie środowiska Azure SSIS Integration Runtime do sieci wirtualnej.
7. W oknie **Połączenia** w razie potrzeby przełącz do pozycji **Produkty Integration Runtime**. Kliknij przycisk **Odśwież**, aby odświeżyć status. 

    ![Stan tworzenia](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Użyj linków w kolumnie **Akcje**, aby monitorować, zatrzymywać/uruchamiać, edytować lub usunąć środowisko Integration Runtime. Użyj ostatniego linku, aby wyświetlić kod JSON dla środowiska Integration Runtime. Przyciski edytowania i usuwania są włączone tylko wtedy, gdy środowisko IR jest zatrzymane. 

    ![Środowisko IR Azure SSIS — akcje](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        
9. Kliknij link **Monitorowanie** w obszarze **Akcje**.  

    ![Środowisko IR Azure SSIS — szczegóły](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-details.png)
10. Jeśli wystąpił **błąd** skojarzony ze środowiskiem Azure SSIS IR, na tej stronie będzie wyświetlana liczba błędów oraz link umożliwiający wyświetlenie szczegółowych informacji o błędzie. Na przykład jeśli wykaz usług SSIS już istnieje na serwerze bazy danych, zostanie wyświetlony błąd wskazujący, że baza danych SSISDB już istnieje.  
11. Kliknij pozycję **Środowiska Integration Runtime** u góry, aby przejść z powrotem do poprzedniej strony i wyświetlić wszystkie środowiska Integration Runtime skojarzone z fabryką danych.  

## <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Środowiska Azure SSIS Integration Runtime w portalu

1. W interfejsie użytkownika usługi Azure Data Factory przejdź na kartę **Edycja**, kliknij pozycję **Połączenia**, a następnie przejdź na kartę **Środowiska Integration Runtime**, aby wyświetlić istniejące środowiska Integration Runtime w fabryce danych. 
    ![Wyświetlanie istniejących środowisk IR](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
1. Kliknij pozycję **Nowe**, aby utworzyć środowisko IR Azure-SSIS. 

    ![Środowisko Integration Runtime za pomocą menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
2. Aby utworzyć środowisko Azure-SSIS Integration Runtime, kliknij przycisk **Nowe**, jak pokazano na ilustracji. 
3. W oknie Integration Runtime Setup (Konfiguracja środowiska Integration Runtime) wybierz pozycję **Lift-and-shift existing SSIS packages to execute in Azure** (Migruj metodą „lift-and-shift” istniejące pakiety usług SSIS do wykonywania na platformie Azure), a następnie kliknij przycisk **Dalej**.

    ![Określanie typu środowiska Integration Runtime](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Zobacz sekcję [Aprowizowanie środowiska Azure SSIS Integration Runtime](#provision-an-azure-ssis-integration-runtime), zawierającą pozostałe kroki konfigurowania środowiska IR Azure-SSIS. 

    
## <a name="deploy-ssis-packages"></a>Wdrażanie pakietów usług SSIS
Teraz użyj programu SQL Server Data Tools (SSDT) lub SQL Server Management Studio (SSMS), aby wdrożyć pakiety usług SSIS na platformie Azure. Połącz się ze swoim serwerem Azure SQL, który hostuje katalog usług SSIS (SSISDB). Nazwa serwera Azure SQL jest w formacie: `<servername>.database.windows.net`(dla usługi Azure SQL Database). 

Zobacz następujące artykuły wchodzące w skład usług SSIS: 

- [Deploy, run, and monitor an SSIS package on Azure (Wdrażanie, uruchamianie i monitorowanie pakietu usług SSIS na platformie Azure)](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Connect to SSIS catalog on Azure (Łączenie z wykazem usług SSIS na platformie Azure)](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Schedule package execution on Azure (Planowanie wykonywania pakietów na platformie Azure)](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Connect to on-premises data sources with Windows authentication (Łączenie z lokalnymi źródłami danych przy użyciu uwierzytelniania systemu Windows)](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności: 

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Tworzenie i uruchamianie środowiska Azure-SSIS Integration Runtime

Przejdź do następującego samouczka, aby dowiedzieć się więcej o kopiowaniu danych lokalnych do chmury: 

> [!div class="nextstepaction"]
>[Kopiowanie danych w chmurze](tutorial-copy-data-portal.md)
