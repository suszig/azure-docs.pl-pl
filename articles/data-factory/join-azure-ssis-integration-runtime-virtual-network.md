---
title: "Dołącz do sieci Wirtualnej środowiska uruchomieniowego integracji usług SSIS Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak sprzęgać środowiska uruchomieniowego integracji usług SSIS Azure z sieci wirtualnej platformy Azure."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2017
ms.author: spelluru
ms.openlocfilehash: 8a58f55bd627594145661e1c8d5c1da360cd1e30
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2017
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Dołącz do środowiska uruchomieniowego integracji usług SSIS Azure do sieci wirtualnej
Środowisko uruchomieniowe integracji usług SSIS Azure (IR) należy dołączyć do sieci wirtualnej platformy Azure (VNet), jeśli spełniony jest jeden z następujących warunków: 

- Hostujesz bazę danych katalogu usług SSIS na wystąpieniu zarządzanym programu SQL Server (prywatna wersja zapoznawcza), który jest częścią sieci wirtualnej.
- Chcesz połączyć się z lokalnymi magazynami danych z pakietów usług SSIS działającymi w środowisku Azure SSIS Integration Runtime.

 Usługa Azure Data Factory w wersji 2 (wersja zapoznawcza) umożliwia dołączenie środowiska Azure-SSIS Integration Runtime do klasycznej sieci wirtualnej. Obecnie usługa Azure Resource Manager w sieci wirtualnej nie jest jeszcze obsługiwany. Jednak można nim działać jak pokazano w poniższej sekcji. 

 > [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz [Data Factory version 1 documentation (Dokumentacja usługi Data Factory w wersji 1)](v1/data-factory-introduction.md).

Pakiety usług SSIS dostęp do chmury publicznej tylko magazynów danych, nie trzeba Dołącz IR Azure SSIS do sieci wirtualnej. Jeśli pakiety usług SSIS uzyskać dostęp do lokalnych magazynów danych, IR Azure SSIS należy dołączyć do sieci wirtualnej, która jest połączona z siecią lokalną. Katalog usług SSIS znajduje się w bazie danych SQL Azure, która nie znajduje się w sieci wirtualnej, należy otworzyć odpowiednie porty. Jeśli katalog SSIS jest obsługiwany w Azure zarządzane wystąpienia SQL w klasycznej sieci wirtualnej, IR Azure SSIS można dołączyć do tej samej sieci wirtualnej klasyczny (lub) różnych klasycznego sieci wirtualnej, która ma połączenie klasy do klasycznej sieci wirtualnej z elementem wystąpienia zarządzane SQL Azure. Więcej informacji można znaleźć w poniższych sekcjach.  

## <a name="access-on-premises-data-stores"></a>Dostęp do lokalnych magazynów danych
Jeśli pakiety usług SSIS uzyskać dostęp do lokalnych magazynów danych, Dołącz do Twojego środowiska uruchomieniowego integracji usług SSIS Azure do sieci wirtualnej, która jest połączona z siecią lokalną. Oto kilka ważnych kwestii, które należy pamiętać: 

- Jeśli nie istniejącej sieci wirtualnej podłączonej do sieci lokalnej, najpierw należy utworzyć [klasycznej sieci wirtualnej](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) Twojego środowiska uruchomieniowego integracji Azure SSIS do przyłączenia. Następnie należy skonfigurować site-to-site [połączenie bramy sieci VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md)/[ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) połączenie z tej sieci wirtualnej sieci lokalnej.
- W przypadku istniejącej sieci wirtualnej klasycznego podłączony do sieci lokalnej w tej samej lokalizacji co Twojego środowiska uruchomieniowego integracji usług SSIS Azure, możesz ponadto dołączyć do Twojego środowiska uruchomieniowego integracji usług SSIS Azure do niego.
- W przypadku istniejących klasycznej sieci wirtualnej podłączone do sieci lokalnej w innej lokalizacji z Twojego środowiska uruchomieniowego integracji usług SSIS Azure, najpierw można utworzyć [klasycznej sieci wirtualnej](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) Twojego środowiska uruchomieniowego integracji Azure SSIS do przyłączenia. Następnie należy skonfigurować [klasycznego do klasycznej sieci wirtualnej](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) połączenia.
- Jeśli istniejącej sieci wirtualnej Azure Resource Manager podłączony do sieci lokalnej, najpierw należy utworzyć [klasycznej sieci wirtualnej](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) Twojego środowiska uruchomieniowego integracji Azure SSIS do przyłączenia. Następnie należy skonfigurować [klasycznego do Azure Resource Manager w sieci wirtualnej](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) połączenia.

## <a name="domain-name-services-server"></a>Serwer usługi nazw domeny 
Jeśli należy użyć serwera usługi nazw domen (DNS) w sieci wirtualnej dołączane za pomocą Twojego środowiska uruchomieniowego integracji usług SSIS Azure, należy wykonać, aby [upewnij się, że węzły Twojego środowiska uruchomieniowego integracji usług SSIS Azure w sieci wirtualnej może rozpoznać punkty końcowe systemu Azure](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="network-security-group"></a>Grupy zabezpieczeń sieci
Jeśli musisz wdrożyć grupy zabezpieczeń sieci (NSG) w sieci wirtualnej dołączane za pomocą programu Azure SSIS Runtime integracji, Zezwalaj traffics przychodzącego/wychodzącego przez następujące porty:

| Porty | Kierunek | Protokół transportu | Przeznaczenie | Docelowy dla ruchu przychodzącego źródło/wychodzącego |
| ---- | --------- | ------------------ | ------- | ----------------------------------- |
| 10100<br/>20100<br/>30100  | Przychodzący | TCP | Usług Azure używa tych portów do komunikacji z węzłami Twojego środowiska uruchomieniowego integracji usług SSIS Azure w sieci wirtualnej. | Internet | 
| 443 | Wychodzący | TCP | Węzły sieci środowiska uruchomieniowego integracji usług SSIS Azure w sieci wirtualnej użyć tego portu dostępu do usług Azure, na przykład usługi Azure Storage, Centrum zdarzeń,... itd. | INTERNET | 
| 1433<br/>11000-11999<br/>14000-14999  | Wychodzący | TCP | Węzły sieci środowiska uruchomieniowego integracji usług SSIS Azure w sieci wirtualnej umożliwia dostęp do bazy danych SSISDB obsługiwanych przez serwer bazy danych SQL Azure (nie dotyczy hostowanych przez usługi Azure SQL zarządzane wystąpienie bazy danych SSISDB) tych portów. | Internet | 

## <a name="script-to-configure-vnet"></a>Skrypt w celu skonfigurowania sieci wirtualnej 
Można użyć skryptu programu PowerShell z przewodnikiem z [w tym artykule](create-azure-ssis-integration-runtime.md) zainicjować środowiska uruchomieniowego integracji usług SSIS Azure w sieci wirtualnej. Skrypt automatycznie konfiguruje uprawnień sieci wirtualnej i ustawień, dzięki czemu można środowiska uruchomieniowego integracji usług SSIS Azure możesz dołączyć do sieci wirtualnej.  


## <a name="use-portal-to-configure-vnet"></a>Konfigurowanie sieci wirtualnej za pomocą portalu
Uruchomienie skryptu jest najprostszym sposobem na skonfigurowanie sieci wirtualnej. Jeśli nie masz dostępu do tej sieci wirtualnej / automatyczne konfigurację właściciel tej sieci wirtualnej nie powiedzie się. / Możesz spróbować ręcznie skonfigurować poniższe kroki:

### <a name="find-the-resource-id-for-your-azure-vnet"></a>Znajdź identyfikator zasobu dla sieci wirtualnej platformy Azure.
 
1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).
2. Kliknij przycisk **więcej usług**. Filtruj i wybierz **sieci wirtualnych (klasyczne)**.
3. Filtruj i wybierz z **sieci wirtualnej** na liście. 
4. Na stronie (klasyczne) sieci wirtualnej, wybierz **właściwości**. 

    ![Identyfikator zasobu sieci wirtualnej klasycznego](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)
5. Kliknij przycisk Kopiuj **identyfikator ZASOBU** można skopiować identyfikator zasobu sieci klasycznego do Schowka. Zapisz identyfikator ze Schowka w programie OneNote lub pliku.
6. Kliknij przycisk **podsieci** w menu po lewej stronie i upewnij się, że liczba **dostępnych adresów** jest większa niż w węzłach programu runtime integracji usług SSIS Azure.

    ![Liczba dostępnych adresów w sieci wirtualnej](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)
7. Dołącz **MicrosoftAzureBatch** do **klasycznego współautora maszyny wirtualnej** roli sieci wirtualnej. 
    1. Kliknij przycisk kontroli dostępu (IAM) w menu po lewej stronie, a następnie kliknij przycisk **Dodaj** na pasku narzędzi.
    
        ![Dodaj -> Kontrola dostępu](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png) 
    2. W **dodać uprawnienia** wybierz pozycję **klasycznego współautora maszyny wirtualnej** dla **roli**. Typ **MicrosoftAzureBatch** w **wybierz** pola tekstowego, a następnie wybierz **MicrosoftAzureBatch** z listy wyników wyszukiwania. 
    
        ![Dodaj uprawnienia — wyszukiwanie](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)
    3. Kliknij przycisk Zapisz, aby zapisać ustawienia i zamknąć stronę.
    
        ![Zapisz ustawienia dostępu](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)
    4. Upewnij się, że widoczny **MicrosoftAzureBatch** na liście uczestników.
    
        ![Potwierdź AzureBatch dostępu](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)
5. Sprawdź, czy dostawca tej partii zadań Azure jest zarejestrowany w subskrypcji platformy Azure, która ma sieci wirtualnej lub zarejestruj dostawcę w partii zadań Azure. Jeśli masz już konto partii zadań Azure w ramach subskrypcji, subskrypcja jest zarejestrowana dla partii zadań Azure.
    1. W portalu Azure kliknij **subskrypcje** w menu po lewej stronie. 
    2. Wybierz użytkownika **subskrypcji**. 
    3. Kliknij przycisk **dostawców zasobów** po lewej stronie i upewnij się, że `Microsoft.Batch` zarejestrowanego dostawcy. 
    
        ![zarejestrowany partii potwierdzenia](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

    Jeśli nie widzisz `Microsoft.Batch` znajduje się na liście, aby zarejestrować go, [jest tworzone puste konto partii zadań Azure](../batch/batch-account-create-portal.md) w ramach subskrypcji. Później można usunąć go. 
         


## <a name="next-steps"></a>Następne kroki
Aby uzyskać informacje o środowisku uruchomieniowym usług SSIS Azure zobacz następujące tematy: 

- [Środowisko uruchomieniowe integracji usług SSIS Azure](concepts-integration-runtime.md#azure-ssis-integration-runtime). Ten artykuł zawiera informacje o pojęciach dotyczących środowisk uruchomieniowych integracji ogólnie tym podczerwieni Azure SSIS. 
- [Samouczek: Wdrażanie pakietów usług SSIS na platformie Azure](tutorial-deploy-ssis-packages-azure.md). Ten artykuł zawiera szczegółowe instrukcje dotyczące tworzenia środowiska Azure-SSIS IR i używania bazy danych Azure SQL do hostowania wykazu usług SSIS. 
- [How to: Create an Azure-SSIS integration runtime (Jak: Tworzenie środowiska Azure SSIS Integration Runtime)](create-azure-ssis-integration-runtime.md). Ten artykuł stanowi rozszerzenie samouczka i zawiera instrukcje dotyczące używania wystąpienia zarządzanego Azure SQL (prywatna wersja zapoznawcza) i dołączania środowiska IR do sieci wirtualnej. 
- [Monitor an Azure-SSIS IR (Monitorowanie środowiska Azure-SSIS IR)](monitor-integration-runtime.md#azure-ssis-integration-runtime). W tym artykule przedstawiono sposób pobierania informacji o środowisku Azure-SSIS IR i opisów stanów w pobranych informacjach. 
- [Manage an Azure-SSIS IR (Zarządzanie środowiskiem Azure-SSIS IR)](manage-azure-ssis-integration-runtime.md). W tym artykule przedstawiono sposób zatrzymywania, uruchamiania lub usuwania środowiska Azure-SSIS IR. Zawiera on również instrukcje skalowania środowiska Azure-SSIS IR do wewnątrz za pomocą dodawania do niego węzłów. 
