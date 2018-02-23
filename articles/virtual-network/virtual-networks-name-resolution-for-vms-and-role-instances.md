---
title: "Rozpoznawanie nazwy dla maszyn wirtualnych i wystąpień roli"
description: "Scenariusze rozpoznawania nazwy IaaS platformy Azure, rozwiązania hybrydowe między usługi w chmurze różnych, usługi Active Directory i przy użyciu serwera DNS."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: tysonn
ms.assetid: 5d73edde-979a-470a-b28c-e103fcf07e3e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: jdial
ms.openlocfilehash: 5ea3e4ad68fd37ccaa6e081febe4827bdb2e196d
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/22/2018
---
# <a name="name-resolution-for-vms-and-role-instances"></a>Rozpoznawanie nazwy dla maszyn wirtualnych i wystąpień roli

W zależności od tego, jak używasz usługi Azure do hostowania IaaS i PaaS, hybrydowych rozwiązań konieczne może być umożliwiają maszyn wirtualnych i wystąpień ról, które zostało utworzone w celu komunikowania się ze sobą. Mimo że ten komunikat może odbywać się przy użyciu adresów IP, jest znacznie prostsza do użyć nazw, które można łatwo zapamiętać i nie należy zmieniać. 

Jeśli wystąpień roli maszyn wirtualnych hostowanych na platformie Azure wymagane i rozpoznawanie nazw domen do adresów IP, można użyć jednej z dwóch metod:

* [Rozpoznawanie nazw platformy Azure](#azure-provided-name-resolution)
* [Rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server) (która może przesłać kwerendy do serwerów DNS platformy Azure) 

Rodzaj rozpoznawania nazw, których używasz, zależy od tego, jak maszyn wirtualnych i wystąpień roli muszą komunikować się ze sobą. W poniższej tabeli przedstawiono scenariusze i odpowiedniego rozwiązania rozpoznawania nazwy:

| **Scenariusz** | Rozwiązania | **Suffix** |
| --- | --- | --- |
| Nazwa rozwiązania między wystąpień roli lub maszyn wirtualnych znajdujących się w tej samej usługi w chmurze lub sieci wirtualnej. |[Rozpoznawanie nazw platformy Azure](#azure-provided-name-resolution) |Nazwa hosta lub nazwa FQDN |
| Rozpoznawanie nazw w usłudze Azure App Service (aplikacja sieci Web, funkcji lub Bot) przy użyciu integracji sieci wirtualnej do wystąpień roli lub maszyn wirtualnych znajdujących się w tej samej sieci wirtualnej. |Zarządzany przez klienta DNS serwerów przekazujących zapytań między sieciami wirtualnymi rozpoznanie przez platformę Azure (serwer proxy DNS). Zobacz [rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server). |Tylko nazwa FQDN |
| Rozpoznawanie nazw między wystąpień roli lub maszyn wirtualnych znajdujących się w różnych sieciach wirtualnych. |Zarządzany przez klienta DNS serwerów przekazujących zapytań między sieciami wirtualnymi rozpoznanie przez platformę Azure (serwer proxy DNS). Zobacz [rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server). |Tylko nazwa FQDN |
| Rozpoznawanie nazw z aplikacji sieci Web usługi aplikacji do maszyn wirtualnych znajdujących się w tej samej sieci wirtualnej. |Zarządzany przez klienta DNS serwerów przekazujących zapytań między sieciami wirtualnymi rozpoznanie przez platformę Azure (serwer proxy DNS). Zobacz [rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server). |Tylko nazwa FQDN |
| Rozpoznawanie nazw z aplikacji sieci Web usługi aplikacji do maszyn wirtualnych znajdujących się w innej sieci wirtualnej. |Zarządzany przez klienta DNS serwerów przekazujących zapytań między sieciami wirtualnymi rozpoznanie przez platformę Azure (serwer proxy DNS). Zobacz [rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server-for-web-apps). |Tylko nazwa FQDN |
| Rozpoznawanie nazwy komputerów i usług lokalnych z wystąpień roli lub maszyn wirtualnych na platformie Azure. |Zarządzany przez klienta z serwerów DNS (lokalnego kontrolera domeny, kontrolera domeny tylko do odczytu lokalnej lub zsynchronizowane, na przykład przy użyciu transferów stref DNS dodatkowej). Zobacz [rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server). |Tylko nazwa FQDN |
| Rozpoznawanie nazwy Azure hostów z komputerów lokalnych. |Do przodu zapytań do serwera proxy DNS zarządzany przez klienta w odpowiedniej sieci wirtualnej, serwer proxy przekazuje zapytania na platformie Azure do rozpoznania. Zobacz [rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server). |Tylko nazwa FQDN |
| Reverse DNS wewnętrznych adresów IP. |[Rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server). |Nie dotyczy |
| Rozpoznawanie nazw między maszynami wirtualnymi lub wystąpień roli znajduje się w różnych usług w chmurze, nie znajduje się w sieci wirtualnej. |Nie dotyczy. Łączność między maszyn wirtualnych i wystąpień roli w innej chmurze usługi nie jest obsługiwana poza siecią wirtualną. |Nie dotyczy|

## <a name="azure-provided-name-resolution"></a>Rozpoznawanie nazw platformy Azure

Wraz z rozpoznawanie publicznej nazwy DNS platforma Azure udostępnia rozpoznawania nazw wewnętrznych dla maszyn wirtualnych i wystąpień ról, które znajdują się w tej samej sieci wirtualnej lub usługi w chmurze. Maszyny wirtualne/wystąpienia w usłudze w chmurze udostępnić tym samym sufiksem DNS (tylko nazwa hosta jest wystarczająca), ale w innej chmurze klasycznych sieci wirtualnych usługi mają inne sufiksy DNS, więc nazwa FQDN nie jest wymagane do rozpoznawania nazw między usługami inną chmurę. W sieciach wirtualnych w modelu wdrażania usługi Resource Manager, sufiks DNS jest spójna sieci wirtualnych (tak, aby nazwa FQDN nie jest potrzebny) i nazwy DNS można przypisać do karty sieciowe i maszyn wirtualnych. Mimo że rozpoznawanie nazw platformy Azure nie wymaga żadnej konfiguracji, nie jest odpowiednim wyborem w przypadku wszystkich scenariuszy wdrożenia w poprzedniej tabeli.

> [!NOTE]
> Przy użyciu ról sieci web i proces roboczy, można także przejść wewnętrznych adresów IP na podstawie liczby nazwę i wystąpienie roli przy użyciu interfejsu API REST zarządzania usługi Azure wystąpień roli. Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST zarządzania usługi](https://msdn.microsoft.com/library/azure/ee460799.aspx).
> 
> 

### <a name="features"></a>Funkcje

* Łatwość użycia: Aby można było używać rozpoznawania nazw platformy Azure jest wymagana żadna konfiguracja.
* Usługa rozpoznawania nazw platformy Azure jest wysokiej dostępności, co pozwala zaoszczędzić konieczność tworzenia i zarządzania klastrami serwerów DNS.
* Można w połączeniu z serwerem DNS do rozpoznawania zarówno lokalnie, jak i nazwy hostów Azure.
* Rozpoznawanie nazw są udostępniane między wystąpieniami/maszyn wirtualnych roli w ramach tej samej usługi w chmurze bez konieczności nazwy FQDN.
* Rozpoznawanie nazw są udostępniane między maszynami wirtualnymi w sieciach wirtualnych, które używają modelu wdrażania usługi Resource Manager, bez potrzeby wprowadzania nazwy FQDN. Sieci wirtualne w klasycznym modelu wdrażania wymagają nazwy FQDN podczas rozpoznawania nazw w innej chmurze usługi. 
* Można użyć nazwy hostów, które najlepiej opisują wdrożeń, a nie Praca z nazwami wygenerowany automatycznie.

### <a name="considerations"></a>Zagadnienia do rozważenia

* Nie można zmodyfikować sufiks DNS utworzony Azure.
* Nie można ręcznie zarejestrować własnych.
* Usługa WINS i NetBIOS nie są obsługiwane (nie widać maszyn wirtualnych w Eksploratorze Windows).
* Nazwy hostów musi być zgodny z DNS. Nazwy musi zawierać tylko 0-9, a-z i "-" i nie może zaczynać się ani kończyć "-". Zobacz dokument RFC 3696 sekcji 2.
* Ruch zapytanie DNS jest ograniczenie dla każdej maszyny Wirtualnej. Ograniczanie nie powinny mieć wpływ na większości aplikacji. Ograniczanie żądań zaobserwowano, upewnij się, że włączone jest buforowanie po stronie klienta. Aby uzyskać więcej informacji, zobacz [wprowadzenie w pełni wykorzystać możliwości rozpoznawania nazw platformy Azure](#Getting-the-most-from-Azure-provided-name-resolution).
* W pierwszej usługi w chmurze 180 tylko maszyny wirtualne są zarejestrowane dla każdej sieci wirtualnej w klasycznym modelu wdrażania. To ograniczenie nie ma zastosowania do sieci wirtualnych w modelach wdrażania Menedżera zasobów.

## <a name="dns-client-configuration"></a>Konfiguracja klienta DNS

### <a name="client-side-caching"></a>Buforowanie po stronie klienta

Nie wszystkie zapytania DNS musi być przesyłane przez sieć. Buforowanie po stronie klienta pomaga zmniejszenia opóźnienia i poprawy odporności na blips sieci rozwiązując cyklicznego zapytania DNS z lokalnej pamięci podręcznej. Rekordy DNS zawierają time to live (TTL) umożliwiającą pamięci podręcznej do przechowywania bez wpływania na świeżości rekordów, tak po stronie klienta jest odpowiedni dla większości sytuacji rekordu tak długo, jak to możliwe.

Domyślnie klienta DNS systemu Windows ma wbudowane pamięci podręcznej DNS. Niektóre dystrybucjach systemu Linux nie dołączaj buforowanie domyślnie. Dodawanie pamięci podręcznej DNS do każdej maszyny Wirtualnej systemu Linux (po sprawdzeniu, że nie ma lokalnej pamięci podręcznej już), jest zalecane.

Istnieje szereg różnych DNS buforowania dostępnych pakietów. Na przykład dnsmasq. Poniższe kroki listy instalowanie dnsmasq na najbardziej typowych dystrybucjach:

* **Ubuntu (używa resolvconf)**:
  * Zainstaluj pakiet dnsmasq z `sudo apt-get install dnsmasq`.
* **SUSE (używa netconf)**:
  * Zainstaluj pakiet dnsmasq z `sudo zypper install dnsmasq`.
  * Włącz usługę dnsmasq z `systemctl enable dnsmasq.service`. 
  * Uruchom usługę dnsmasq z `systemctl start dnsmasq.service`. 
  * Edytuj **/etc/sysconfig/network/config** i zmienić *NETCONFIG_DNS_FORWARDER = ""* do *dnsmasq*.
  * Zaktualizuj resolv.conf z `netconfig update` można ustawić jako program rozpoznawania nazw DNS lokalnej pamięci podręcznej.
* **OpenLogic (używa NetworkManager)**:
  * Zainstaluj pakiet dnsmasq z `sudo yum install dnsmasq`.
  * Włącz usługę dnsmasq z `systemctl enable dnsmasq.service`.
  * Uruchom usługę dnsmasq z `systemctl start dnsmasq.service`.
  * Dodaj *dołączy domeny nazwy serwerów 127.0.0.1;* do **/etc/dhclient-eth0.conf**.
  * Uruchom ponownie usługę sieci z `service network restart` można ustawić jako program rozpoznawania nazw DNS lokalnej pamięci podręcznej.

> [!NOTE]
> Pakiet "dnsmasq" jest tylko jeden z wielu pamięci podręcznej DNS, dostępne dla systemu Linux. Przed użyciem go, sprawdź jego przydatności do określonych potrzeb, a nie pamięci podręcznej jest zainstalowany.
> 
> 
    
### <a name="client-side-retries"></a>Ponownych prób po stronie klienta

Usługa DNS jest głównie protokołu UDP. Jak protokół UDP nie gwarantuje dostarczanie komunikatów, obsługi logiki ponawiania próby w samego protokołu DNS. Każdy klient DNS (systemu operacyjnego) może mieć Logika ponawiania różne w zależności od preferencji twórcy:

* Systemów operacyjnych Windows ponów próbę po jednym drugi, a następnie ponownie po innym 2, 4 i inny cztery sekundy. 
* Domyślne Linux Instalatora ponownych prób po 5 sekund. Zmiana retry na pięć razy w odstępach czasu 1 sekundę, jest zalecane.

Sprawdź bieżące ustawienia na Maszynę wirtualną systemu Linux z `cat /etc/resolv.conf`. Przyjrzyj się *opcje* linii, na przykład:

```bash
options timeout:1 attempts:5
```

Plik resolv.conf jest zazwyczaj generowanych automatycznie i nie można edytować. Wykonania określonych kroków w celu dodania *opcje* wiersza różnią się distro:

* **Ubuntu** (używa resolvconf):
  * Dodaj wiersz opcje do **/etc/resolveconf/resolv.conf.d/head**.
  * Uruchom `resolvconf -u` do aktualizacji.
* **SUSE** (używa netconf):
  * Dodaj *timeout:1 prób: 5* do **NETCONFIG_DNS_RESOLVER_OPTIONS = ""** parametru w **/etc/sysconfig/network/config**. 
  * Uruchom `netconfig update` do aktualizacji.
* **OpenLogic** (używa NetworkManager):
  * Dodaj *echa "Opcje timeout:1 prób: 5"* do **/etc/NetworkManager/dispatcher.d/11-dhclient**. 
  * Aktualizacja z `service network restart`.

## <a name="name-resolution-using-your-own-dns-server"></a>Rozpoznawanie nazw przy użyciu serwera DNS

### <a name="vms-and-role-instances"></a>Maszyn wirtualnych i wystąpień roli

Istnieją różne sytuacje, w przypadku, gdy potrzeb rozpoznawania nazw może go poza funkcje udostępniane przez platformę Azure, na przykład gdy przy użyciu domen usługi Active Directory lub gdy wymagają rozpoznawania nazw DNS między sieciami wirtualnymi. Tak, aby pokrywał tych scenariuszy, platforma Azure oferuje możliwości do użycia serwery DNS.

Serwery DNS w sieci wirtualnej można przekazywać zapytań DNS do rozpoznawania nazw cykliczne platformy Azure do rozpoznania nazwy hostów w tej sieci wirtualnej. Na przykład kontroler domeny (DC) działają na platformie Azure można odpowiada na zapytania DNS dla swojej domeny i przesyła inne zapytania do usługi Azure. Przekazywanie zapytań umożliwia maszyn wirtualnych zobaczyć, zarówno lokalnych zasobów (za pośrednictwem kontrolera domeny), jak i nazwy hostów platformy Azure (za pośrednictwem usługi przesyłania dalej). Dostęp do platformy Azure cykliczne rozwiązujący zostaną przekazane za pośrednictwem wirtualnego adresu IP 168.63.129.16.

Przesyłanie dalej DNS również umożliwia rozpoznawanie nazw DNS w sieci wirtualnej między oraz pozwala na maszynach lokalnych do rozpoznania nazwy hostów platformy Azure. Aby można było rozpoznać nazwy hosta maszyny Wirtualnej, serwer DNS maszyny Wirtualnej musi znajdować się w tej samej sieci wirtualnej i można skonfigurować do zapytań do przodu nazwy hosta Azure. Sufiks DNS jest różna w każdej sieci wirtualnej, można użyć zasady warunkowego przesyłania dalej do wysyłania zapytań DNS do odpowiedniej sieci wirtualnych do rozpoznania. Na poniższej ilustracji przedstawiono dwie sieci wirtualne i sieć lokalną tej sieci wirtualnej między komputerami za pomocą tej metody rozpoznawania nazw DNS. Usługi przesyłania dalej DNS na przykład jest dostępny w [galerię szablonów Szybki Start Azure](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) i [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

> [!NOTE]
> Wystąpienia roli można wykonać rozpoznawanie nazw maszyn wirtualnych w tej samej sieci wirtualnej przy użyciu nazwy FQDN, która używa nazwy maszyny Wirtualnej wraz z sufiksem DNS "internal.cloudapp.net". Jednak, w tym przypadku rozpoznawanie nazw tylko zakończy się pomyślnie, jeśli wystąpienia roli ma nazwę maszyny Wirtualnej, zdefiniowane w [roli schematu (plik .cscfg)](https://msdn.microsoft.com/library/azure/jj156212.aspx). 
>    <Role name="<role-name>" vmName="<vm-name>">
> 
> Wystąpienia roli, trzeba będzie wykonać rozpoznawanie nazw maszyn wirtualnych w innej sieci wirtualnej (przy użyciu nazwy FQDN **internal.cloudapp.net** sufiks) ma to zrobić za pośrednictwem przesyłania dalej między dwiema sieciami wirtualnymi, zgodnie z opisem w niestandardowych serwerów DNS w tej sekcji.
>

![Sieć wirtualna między DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Podczas rozpoznawania nazwy platformy Azure, wewnętrzny sufiks DNS (`*.internal.cloudapp.net`) znajduje się na każdej maszynie Wirtualnej na platformie Azure przez DHCP. Ten sufiks umożliwia rozpoznawania nazwy hosta, jak rekordów nazwa hosta jest *internal.cloudapp.net* strefy. Korzystając z własnych rozwiązania rozpoznawania nazw, ten sufiks nie jest dostarczony do maszyn wirtualnych, ponieważ zakłócać innych architektur DNS (na przykład scenariusze przyłączonych do domeny). Zamiast tego, platforma Azure udostępnia symbol zastępczy niedziałającej (*reddog.microsoft.com*).

W razie potrzeby sufiks DNS wewnętrzny może zostać określony przy użyciu programu PowerShell lub interfejsu API:

* Dla sieci wirtualnych w modelach wdrażania usługi Resource Manager, jest dostępny za pośrednictwem sufiks [karty interfejsu sieciowego](virtual-network-network-interface.md) zasobów lub za pośrednictwem [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) polecenia cmdlet.
* W klasycznych modeli wdrażania, jest dostępna za pośrednictwem sufiks [uzyskać interfejsu API wdrożenia](https://msdn.microsoft.com/library/azure/ee460804.aspx) wywołania lub za pośrednictwem [Get AzureVM-debugowania](/powershell/module/azure/get-azurevm) polecenia cmdlet.

Jeśli przekazywanie zapytań na platformie Azure nie własnych potrzeb, musisz podać rozwiązania DNS. Rozwiązania DNS musi:

* Podaj rozpoznawania odpowiedniej nazwy hosta, za pomocą [DDNS](virtual-networks-name-resolution-ddns.md), np. Uwaga: Jeśli przy użyciu DDNS może być konieczne wyłączenie oczyszczanie rekordów DNS dzierżaw DHCP platformy Azure są długie i oczyszczania może usunąć DNS przedwcześnie rekordy. 
* Podaj odpowiednie rekursywnego rozpoznawania zezwalająca na rozpoznawanie nazw domen zewnętrznych.
* Być dostępny (TCP i UDP na porcie 53) od klientów, które służy ona i mieć możliwość uzyskania dostępu do Internetu.
* Być zabezpieczony przed dostępu z Internetu, ograniczających zagrożenia powodowanego przez czynników zewnętrznych.

> [!NOTE]
> Aby uzyskać najlepszą wydajność, korzystając z maszyn wirtualnych platformy Azure jako serwery DNS, należy wyłączyć IPv6 i [publiczny adres IP na poziomie wystąpienia](virtual-networks-instance-level-public-ip.md) powinny zostać przypisane do każdego serwera DNS maszyny Wirtualnej. Analiza wydajności dodatkowe i optymalizacje przy użyciu systemu Windows Server jako serwer DNS, zobacz [Nazwa rozwiązania wydajność cykliczne Windows DNS Server 2012 R2](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx).
> 
> 

### <a name="web-apps"></a>Web Apps
Jeśli zachodzi potrzeba wykonania rozpoznawania nazw z aplikacji sieci Web usługi aplikacji połączone siecią wirtualną na maszynach wirtualnych w tej samej sieci wirtualnej, a następnie Oprócz konfigurowania niestandardowego serwera DNS, który ma usługi przesyłania dalej DNS który przesyła dalej zapytania do platformy Azure (wirtualny adres IP 168.63.129.16) , należy również wykonać następujące czynności:
* Włącz integrację sieci wirtualnej dla aplikacji sieci Web usługi aplikacji, jeśli nie jeszcze zrobione, zgodnie z opisem w [integracji aplikacji z siecią wirtualną](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* W portalu Azure wybierz hosting aplikacji sieci Web dla planu usług aplikacji **sieci synchronizacji** w obszarze **sieci**, **integracji sieci wirtualnej**, jak pokazano poniżej obraz:

    ![Rozpoznawanie nazw sieci wirtualnej aplikacji sieci Web](./media/virtual-networks-name-resolution-for-vms-and-role-instances/webapps-dns.png)

Rozpoznawanie nazw z aplikacji sieci Web usługi aplikacji połączony z siecią wirtualną, do maszyn wirtualnych w innej sieci wirtualnej wymaga użycia niestandardowych serwerów DNS w sieci zarówno wirtualnych, w następujący sposób:
* Konfigurowanie serwera DNS w sieci wirtualnej docelowego na maszynie Wirtualnej, która również może przekazywać kwerendy do platformy Azure cyklicznego programu rozpoznawania nazw (wirtualny adres IP 168.63.129.16). Usługi przesyłania dalej DNS na przykład jest dostępny w [galerię szablonów Szybki Start Azure](https://azure.microsoft.com/documentation/templates/301-dns-forwarder) i [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder). 
* Konfigurowanie usługi przesyłania dalej DNS w sieci wirtualnej źródła na maszynie Wirtualnej. Konfigurowanie tej usługi przesyłania dalej DNS do przesyłania zapytań do serwera DNS w sieci wirtualnej docelowej.
* Skonfiguruj serwer DNS źródła w ustawieniach sieci wirtualnej źródła.
* Włącz integrację sieci wirtualnej dla aplikacji sieci Web usługi aplikacji, tak utworzyć link do źródła sieci wirtualnej, zgodnie z instrukcjami zawartymi w [integracji aplikacji z siecią wirtualną](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* W portalu Azure wybierz hosting aplikacji sieci web dla planu usług aplikacji **sieci synchronizacji** w obszarze **sieci**, **integracji sieci wirtualnej**. 

## <a name="specifying-dns-servers"></a>Określanie serwerów DNS
Korzystając z serwerów DNS, Azure pozwala, aby określić wiele serwerów DNS dla sieci wirtualnej lub dla interfejsu sieciowego (Resource Manager) / (klasyczne) usługa w chmurze. Serwerów DNS określonych dla interfejsu sieci usługi chmury Pobierz pierwszeństwo nad serwerów DNS określonych dla sieci wirtualnej.

> [!NOTE]
> Właściwości połączenia sieci, takich jak serwer DNS adresów IP, nie należy edytować bezpośrednio z poziomu maszyn wirtualnych systemu Windows, jak mogą uzyskać wymazane podczas usługi poprawianego po wymianie pobiera karty sieci wirtualnej. 
> 
> 

Podczas korzystania z modelu wdrażania usługi Resource Manager, serwerów DNS można określić w portalu, interfejsu API/szablonów: [sieci wirtualnej](https://msdn.microsoft.com/library/azure/mt163661.aspx) i [interfejsu sieciowego](https://msdn.microsoft.com/library/azure/mt163668.aspx), lub środowiska PowerShell: [sieci wirtualnej ](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetwork) i [interfejsu sieciowego](/powershell/module/azurerm.network/new-azurermnetworkinterface).

Korzystając z klasycznym modelu wdrażania, serwery DNS dla sieci wirtualnej można określić w portalu lub [ *konfiguracji sieci* pliku](https://msdn.microsoft.com/library/azure/jj157100). Dla usług w chmurze, serwery DNS są określone za pomocą [ *konfiguracji usługi* pliku](https://msdn.microsoft.com/library/azure/ee758710) lub za pomocą programu PowerShell, [AzureVM nowy](/powershell/module/azure/new-azurevm).

> [!NOTE]
> W przypadku zmiany ustawień DNS dla wirtualnych sieci/wirtualnej maszyny, która została już wdrożona, musisz ponownie uruchomić każdej dotyczy maszyny Wirtualnej, aby zmiany zaczęły obowiązywać.
> 
> 

## <a name="next-steps"></a>Kolejne kroki

Model wdrażania Menedżera zasobów:

* [Utwórz lub zaktualizuj sieć wirtualną](https://msdn.microsoft.com/library/azure/mt163661.aspx)
* [Utwórz lub zaktualizuj karty interfejsu sieciowego](https://msdn.microsoft.com/library/azure/mt163668.aspx)
* [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)
* [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface)

Klasycznego modelu wdrażania:

* [Schemat konfiguracji usługi Azure](https://msdn.microsoft.com/library/azure/ee758710)
* [Schemat konfiguracji sieci wirtualnej](https://msdn.microsoft.com/library/azure/jj157100)
* [Skonfiguruj sieć wirtualną przy użyciu pliku konfiguracji sieci](virtual-networks-using-network-configuration-file.md)
