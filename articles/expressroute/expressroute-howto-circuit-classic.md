---
title: "Tworzenie i modyfikowanie obwodu usługi ExpressRoute: środowiska PowerShell: klasyczny Portal Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule przedstawiono kroki tworzenia i inicjowania obsługi administracyjnej obwodu usługi ExpressRoute. W tym artykule przedstawiono również sposób sprawdzania stanu, update lub delete i anulowanie zastrzeżenia obwodu."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0134d242-6459-4dec-a2f1-4657c3bc8b23
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 93ddc2975db34053c6a776d1c3b931536f3f8ec7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell-classic"></a>Tworzenie i modyfikowanie obwodu usługi ExpressRoute, przy użyciu programu PowerShell (klasyczne)
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Interfejs wiersza polecenia platformy Azure](howto-circuit-cli.md)
> * [Video - portalu Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasyczny)](expressroute-howto-circuit-classic.md)
>

W tym artykule przedstawiono kroki, aby utworzyć obwodu usługi Azure ExpressRoute przy użyciu poleceń cmdlet programu PowerShell i klasycznym modelu wdrażania. W tym artykule przedstawiono również sposób sprawdzania stanu, update lub delete i anulowanie zastrzeżenia obwodu usługi ExpressRoute.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]


**Modele wdrażania Azure — informacje**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Przed rozpoczęciem
### <a name="step-1-review-the-prerequisites-and-workflow-articles"></a>Krok 1. Przejrzyj wymagania wstępne i artykuły przepływu pracy
Upewnij się, że użytkownik przejrzał [wymagania wstępne](expressroute-prerequisites.md) i [przepływy pracy](expressroute-workflows.md) przed rozpoczęciem konfigurowania.  

### <a name="step-2-install-the-latest-versions-of-the-azure-service-management-sm-powershell-modules"></a>Krok 2. Zainstaluj najnowsze wersje moduły programu PowerShell Azure usługi zarządzania (ko)
Postępuj zgodnie z instrukcjami [wprowadzenie do poleceń cmdlet programu Azure PowerShell](/powershell/azure/overview) wskazówki krok po kroku dotyczące sposobu konfigurowania komputera do modułów programu Azure PowerShell.

### <a name="step-3-log-in-to-your-azure-account-and-select-a-subscription"></a>Krok 3. Zaloguj się do konta platformy Azure i wybierz subskrypcję
1. Otwórz konsolę programu PowerShell z podwyższonym poziomem uprawnień i połącz się ze swoim kontem. Użyj poniższego przykładu w celu łatwiejszego nawiązania połączenia:

        Login-AzureRmAccount

2. Sprawdź subskrypcje dostępne na koncie.

        Get-AzureRmSubscription

3. Jeśli masz więcej niż jedną subskrypcję, wybierz tę, której chcesz użyć.

        Select-AzureSubscription -SubscriptionId "Replace_with_your_subscription_id"

4. Upewnij się, czy dla wybranego identyfikatora subskrypcji jest ustawiony jako domyślny.

        Get-AzureSubscription -default

## <a name="create-and-provision-an-expressroute-circuit"></a>Tworzenie i przydzielanie obwodu usługi ExpressRoute
### <a name="step-1-import-the-powershell-modules-for-expressroute"></a>Krok 1. Zaimportuj moduły programu PowerShell dla usługi ExpressRoute
 Jeśli jeszcze nie, aby rozpocząć korzystanie z poleceń cmdlet programu ExpressRoute należy zaimportować moduły Azure i usługi ExpressRoute w sesji programu PowerShell. Moduły importowania w lokalizacji, w jakiej zostały wcześniej zainstalowane na komputerze lokalnym. W zależności od metody użytej do instalowania modułów lokalizacja może być inna niż w poniższym przykładzie pokazano. W razie potrzeby zmodyfikować przykładzie.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="step-2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>Krok 2. Pobierz listę obsługiwanych dostawców, lokalizacji i przepustowości
Przed utworzeniem obwodu usługi ExpressRoute, należy listę dostawców obsługiwanych łączności, lokalizacji i opcji przepustowości.

Polecenia cmdlet programu PowerShell `Get-AzureDedicatedCircuitServiceProvider` zwraca informację, która będzie używana w dalszych krokach:

    Get-AzureDedicatedCircuitServiceProvider

Sprawdź, czy dostawca połączenia znajduje się tam. Ponieważ będzie on potrzebny później podczas tworzenia obwód, zwróć uwagę następujące informacje:

* Nazwa
* PeeringLocations
* BandwidthsOffered

Teraz możesz utworzyć obwodu usługi ExpressRoute.         

### <a name="step-3-create-an-expressroute-circuit"></a>Krok 3. Create an ExpressRoute circuit (Tworzenie obwodu usługi ExpressRoute)
Poniższy przykład przedstawia sposób tworzenia obwodu ExpressRoute za pośrednictwem Equinix 200-MB/s w Dolinie Krzemowej. Jeśli używasz innego dostawcy i inne ustawienia, należy zastąpić te informacje po wprowadzeniu żądania.

> [!IMPORTANT]
> Od momentu jego wygenerowaniu klucza usługi będą naliczane obwodu usługi ExpressRoute. Upewnij się, gdy dostawca łączności jest gotowy do udostępniania obwodu podczas wykonywania tej operacji.
> 
> 

Poniżej zamieszczono przykładowe żądanie dla nowego klucza usługi:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Lub, jeśli chcesz utworzyć obwodu usługi ExpressRoute z dodatkiem premium, skorzystaj z następującego przykładu. Zapoznaj się [ExpressRoute — często zadawane pytania](expressroute-faqs.md) więcej szczegółów dotyczących dodatek w warstwie premium.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Odpowiedź będzie zawierać klucz usługi. Szczegółowy opis wszystkich parametrów można uzyskać, uruchamiając następujące czynności:

    get-help new-azurededicatedcircuit -detailed

### <a name="step-4-list-all-the-expressroute-circuits"></a>Krok 4. Wyświetl listę wszystkich obwody usługi ExpressRoute
Można uruchomić `Get-AzureDedicatedCircuit` polecenie, aby uzyskać listę wszystkich obwody usługi ExpressRoute, które zostały utworzone:

    Get-AzureDedicatedCircuit

Odpowiedź będzie miała podobny do poniższego przykładu:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Te informacje w dowolnym momencie można pobrać za pomocą `Get-AzureDedicatedCircuit` polecenia cmdlet. Wywołania bez parametrów wyświetla listę wszystkich obwodów. Klucz usługi będzie wyświetlane w *bindingTemplate* pola.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Szczegółowy opis wszystkich parametrów można uzyskać, uruchamiając następujące czynności:

    get-help get-azurededicatedcircuit -detailed

### <a name="step-5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>Krok 5. Wyślij klucz usługi do dostawcą połączenia do inicjowania obsługi
*ServiceProviderProvisioningState* zawiera informacje o bieżącym stanie inicjowania obsługi administracyjnej po stronie dostawcy usług. *Stan* zapewnia stanu po stronie firmy Microsoft. Aby uzyskać więcej informacji na temat obwodu inicjowania obsługi administracyjnej stanów zobacz [przepływy pracy](expressroute-workflows.md#expressroute-circuit-provisioning-states) artykułu.

Podczas tworzenia nowego obwodu ExpressRoute obwodu będą w następującym stanie:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Gdy dostawca połączenia trwa jej włączanie, obwodu przejdzie na następujący:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Obwodu usługi ExpressRoute musi być w następującym stanie dla Ciebie można było użyć go:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="step-6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>Krok 6. Okresowo sprawdzać stan i stan klucz obwodu
Dzięki temu można dowiedzieć się, gdy dostawca jest włączony obwodu. Po skonfigurowaniu obwodu *ServiceProviderProvisioningState* będą wyświetlane jako *obsługiwane administracyjnie* jak pokazano w poniższym przykładzie:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="step-7-create-your-routing-configuration"></a>Krok 7. Tworzenie konfiguracji routingu
Zapoznaj się [obwodu ExpressRoute konfiguracji routingu (tworzenia i modyfikowania obwodu komunikacji równorzędnych)](expressroute-howto-routing-classic.md) artykułu, aby uzyskać instrukcje krok po kroku.

> [!IMPORTANT]
> Te instrukcje dotyczą tylko obwody, które zostały utworzone z dostawców usług, które oferują warstwy 2 łączności usługi. Jeśli używasz usługodawcy, który oferuje zarządzanych warstwy 3 usługi (zazwyczaj IP sieci VPN, takie jak MPLS), dostawca połączenia będzie Konfigurowanie i zarządzanie nimi routingu dla Ciebie.
> 
> 

### <a name="step-8-link-a-virtual-network-to-an-expressroute-circuit"></a>Krok 8. Łączenie sieci wirtualnej z obwodem usługi ExpressRoute
Następnie połączyć sieć wirtualną obwodu usługi ExpressRoute. Zapoznaj się [łączenie obwody usługi ExpressRoute z siecią wirtualną](expressroute-howto-linkvnet-classic.md) instrukcje krok po kroku. Jeśli chcesz utworzyć sieć wirtualną przy użyciu klasycznego modelu wdrażania w przypadku połączeń ExpressRoute, zobacz [utworzyć sieć wirtualną w przypadku połączeń ExpressRoute](expressroute-howto-vnet-portal-classic.md).

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Pobieranie stanu obwodu usługi ExpressRoute
Te informacje w dowolnym momencie można pobrać za pomocą `Get-AzureCircuit` polecenia cmdlet. Wywołania bez parametrów wyświetla listę wszystkich obwodów.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Można uzyskać informacji o określonych obwodu ExpressRoute przez przekazanie klucza usługi jako parametru do wywołania.

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Szczegółowy opis wszystkich parametrów można uzyskać, uruchamiając w poniższym przykładzie:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Modyfikowanie obwodu usługi ExpressRoute
Można zmodyfikować niektórych właściwości obwodu usługi ExpressRoute, bez wywierania wpływu na łączność.

Można wykonać następujące czynności bez przestojów:

* Włącz lub Wyłącz dodatek premium usługi ExpressRoute dla obwodu usługi ExpressRoute.
* Zwiększyć przepustowość obwodu ExpressRoute dostarczane na port jest dostępna pojemność. Należy pamiętać, że przepustowość obwodu zmiana wersji na starszą nie jest obsługiwany. 
* Zmiana zliczania planu naliczane danych nieograniczone danych. Należy pamiętać, że zmiana zliczania planu z dane nieograniczone naliczane danych nie jest obsługiwana.
* Można włączyć lub wyłączyć *operacje klasycznego*.

Zapoznaj się [ExpressRoute — często zadawane pytania](expressroute-faqs.md) Aby uzyskać więcej informacji na temat limity i ograniczenia.

### <a name="to-enable-the-expressroute-premium-add-on"></a>Aby włączyć dodatek usługi ExpressRoute w warstwie premium
Dodatek usługi ExpressRoute w warstwie premium dla istniejącego obwodu można włączyć za pomocą następującego polecenia cmdlet programu PowerShell:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Obwodu mają teraz włączonymi funkcjami dodatek premium usługi ExpressRoute. Należy pamiętać, że firma Microsoft będzie uruchamiane rozliczeń dla funkcji dodatku premium, jak polecenie zostało pomyślnie uruchomione.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Aby wyłączyć dodatek usługi ExpressRoute w warstwie premium
> [!IMPORTANT]
> Ta operacja może zakończyć się niepowodzeniem, jeśli używasz zasobów, które są większe niż co to jest dozwolone dla standardowych obwodu.
> 
> 

#### <a name="considerations"></a>Zagadnienia do rozważenia

* Należy się upewnić, liczba sieci wirtualnych powiązany obwodu jest mniejsza niż 10, aby obniżyć z premium standard. Jeśli nie zrobisz, żądanie aktualizacji zakończy się niepowodzeniem i będzie rozliczane stawki premium.
* Należy odłączyć wszystkie sieci wirtualne w różnych regionach geograficznymi. Jeśli nie zrobisz, żądanie aktualizacji zakończy się niepowodzeniem i będzie rozliczane stawki premium.
* Tabela tras musi być mniejsza niż 4000 tras dla prywatnej komunikacji równorzędnej. Jeśli rozmiar tabeli tras jest większa niż 4000 tras, sesji BGP spowoduje porzucenie i nie będzie reenabled, aż przejdzie liczby prefiksów anonsowanych poniżej 4000.

#### <a name="disable-the-premium-add-on"></a>Wyłącz dodatek w warstwie premium
Dodatek usługi ExpressRoute w warstwie premium dla istniejącego obwodu można wyłączyć za pomocą następującego polecenia cmdlet programu PowerShell:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Aby zaktualizować przepustowości obwodu ExpressRoute
Sprawdź [ExpressRoute — często zadawane pytania](expressroute-faqs.md) obsługiwane opcje przepustowości dla dostawcy. Można wybrać żadnych ma rozmiar większy niż rozmiar z istniejącym obwodem tak długo, jak zezwala na fizyczny port (na którym jest tworzony obwodu).

> [!IMPORTANT]
> Może być konieczne ponowne utworzenie obwodu usługi expressroute w przypadku niewystarczającego pojemności na istniejącego portu. Nie można uaktualnić obwodu, jeśli nie bez dodatkowej pojemności dostępnej w tej lokalizacji.
>
> Nie można zmniejszyć przepustowość obwodu usługi ExpressRoute bez zakłóceń. Zmiana wersji na starszą przepustowości wymaga anulowanie zastrzeżenia obwodu ExpressRoute, a następnie Udostępnij ponownie nowy obwód usługi ExpressRoute.
> 
> 

#### <a name="resize-a-circuit"></a>Zmień rozmiar obwodu

Po wybraniu rozmiar, jaki należy można Użyj następującego polecenia, aby zmienić rozmiar obwodu:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Obwodu będzie zostały wielkości po stronie firmy Microsoft. Należy skontaktuj się z dostawcą połączenia można zaktualizować konfiguracji na bok odpowiadające tej zmiany. Należy pamiętać, że firma Microsoft będzie uruchamiane rozliczeń można opcji przepustowości zaktualizowane z tego punktu na.

Jeśli zostanie wyświetlony następujący błąd podczas zwiększyć przepustowość obwodu, to pozostaje nie wystarczającą przepustowość na fizyczny port, na której jest tworzony z istniejącym obwodem. Należy usunąć ten obwód i utworzyć nowy obwód rozmiaru, które są potrzebne. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Anulowania obsługi i usuwania obwodu usługi ExpressRoute

### <a name="considerations"></a>Zagadnienia do rozważenia

* Należy odłączyć wszystkie sieci wirtualne z obwodem usługi ExpressRoute, ta operacja powiodła się. Sprawdź, czy masz żadnych sieci wirtualnych, które są połączone z obwodem, jeśli ta operacja nie powiedzie się.
* Jeśli dostawca usługi obwodu ExpressRoute stan inicjowania obsługi jest **inicjowania obsługi administracyjnej** lub **obsługiwane administracyjnie** należy skontaktować się z dostawcą usług na anulowanie zastrzeżenia obwód w bok. Firma Microsoft będzie zarezerwować zasobów i obciążać Cię do czasu dostawcy usług wykonuje anulowania obsługi obwodu i powiadomienia NAS.
* Jeśli usługodawca została anulowana obwodu (ustawiono dostawcę usługi stan inicjowania obsługi **nieudostępniane**) następnie można usunąć obwodu. Spowoduje to zatrzymanie rozliczeń dla obwodu.

#### <a name="delete-a-circuit"></a>Usunąć obwód

Można usunąć obwodu usługi ExpressRoute, uruchamiając następujące polecenie:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Następne kroki
Po utworzeniu obwodu, upewnij się, należy wykonać następujące czynności:

* [Tworzenie i modyfikowanie routingu dla obwodu usługi ExpressRoute](expressroute-howto-routing-classic.md)
* [Link sieci wirtualnej do obwodu usługi ExpressRoute](expressroute-howto-linkvnet-classic.md)

