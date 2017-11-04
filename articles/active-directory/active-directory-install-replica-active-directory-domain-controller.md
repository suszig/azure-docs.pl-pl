---
title: "Instalowanie repliki kontrolera domeny usługi Active Directory na platformie Azure | Dokumentacja firmy Microsoft"
description: "Samouczek, który objaśnia, jak zainstalować kontroler domeny w lesie usługi Active Directory lokalnych na maszynie wirtualnej platformy Azure."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8c9ebf1b-289a-4dd6-9567-a946450005c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: fb0bacac346445e6bde9df22f3355419e3162a3c
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Instalowanie repliki kontrolera domeny usługi Active Directory w sieci wirtualnej platformy Azure
W tym temacie przedstawiono sposób instalowania dodatkowych kontrolerów domeny (znanej także jako replik kontrolerów domeny) do domeny usługi Active Directory lokalnego na maszynach wirtualnych Azure (maszyny wirtualne) w sieci wirtualnej platformy Azure.

> [!IMPORTANT]
> Firma Microsoft zaleca zarządzanie usługą Azure AD przy użyciu [centrum administracyjnego usługi Azure AD](https://aad.portal.azure.com) w witrynie Azure Portal zamiast korzystania z klasycznej witryny Azure Portal przywołanej w niniejszym artykule.

Być może zainteresuje te tematy pokrewne:

* Można opcjonalnie zainstalować nowy las usługi Active Directory w sieci wirtualnej platformy Azure. Te kroki opisane w artykule [zainstalować nowy las usługi Active Directory w sieci wirtualnej platformy Azure](active-directory-new-forest-virtual-machine.md).
* Aby uzyskać ogólne wskazówki dotyczące instalowania usług domenowych w usłudze Active Directory (AD DS) w sieci wirtualnej platformy Azure, zobacz [wskazówki dotyczące wdrażania systemu Windows Server Active Directory na maszynach wirtualnych Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Diagram scenariusza
W tym scenariuszu użytkownicy zewnętrzni trzeba uzyskiwać dostęp do aplikacji działających na serwerach przyłączonych do domeny. Maszyny wirtualne z systemem serwerów aplikacji, kontrolery domeny repliki są zainstalowane w sieci wirtualnej platformy Azure. Sieci wirtualnej można podłączyć do sieci lokalnej przez [sieci VPN typu lokacja lokacja](../vpn-gateway/vpn-gateway-site-to-site-create.md) połączenia, jak pokazano na poniższym diagramie, lub można użyć [ExpressRoute](../expressroute/expressroute-locations-providers.md) szybsze połączenia.

Serwery aplikacji i kontrolery domeny są wdrażane w ramach usługi w chmurze oddzielne do dystrybucji przetwarzania obliczeń i w obrębie [zestawów dostępności](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ulepszone odporności na uszkodzenia.
Kontrolery domen replikacji ze sobą i z kontrolerów domeny lokalnej za pomocą replikacji usługi Active Directory. Nie są konieczne żadne narzędzia synchronizacji.

![Diagram sieci wirtualnej platformy Azure pf repliki kontrolera domeny usługi Active Directory][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Tworzenie lokacji usługi Active Directory dla sieci wirtualnej platformy Azure
Jest dobrym rozwiązaniem, aby utworzyć witrynę w usłudze Active Directory reprezentujące region sieci odpowiadającego do sieci wirtualnej. Czy pozwala zoptymalizować uwierzytelniania, replikacji i innych operacji lokalizacji kontrolera domeny. W poniższych krokach opisano sposób tworzenia lokacji i w tle więcej, zobacz [Dodawanie nowej lokacji](https://technet.microsoft.com/library/cc781496.aspx).

1. Otwórz Lokacje i usługi Active Directory: **Menedżera serwera** > **narzędzia** > **Lokacje i usługi Active Directory**.
2. Utwórz witrynę do reprezentowania regionu, w którym utworzono sieci wirtualnej platformy Azure: kliknij **witryny** > **akcji** > **nowej lokacji** > wpisz nazwę nowej lokacji, takich jak Azure nam zachodnie > Wybierz link lokacji > **OK**.
3. Utwórz podsieć i skojarzyć z nowej lokacji: kliknij dwukrotnie **witryny** > kliknij prawym przyciskiem myszy **podsieci** > **nową podsieć** > wpisz zakres adresów IP sieci wirtualnej (np. 10.1.0.0/16 na diagramie scenariusz) > Wybierz nową witrynę Azure > **OK**.

## <a name="create-an-azure-virtual-network"></a>Tworzenie sieci wirtualnej platformy Azure
1. W [klasycznego portalu Azure](https://manage.windowsazure.com), kliknij przycisk **nowy** > **usług sieciowych** > **sieci wirtualnej** > **Utwórz niestandardowy** i użyj następujących wartości, aby zakończyć pracę kreatora.

   | Na tej stronie kreatora... | Określ te wartości |
   | --- | --- |
   |  **Szczegóły sieci wirtualnej** |<p>Nazwa: Nazwa sieci wirtualnej, takie jak WestUSVNet.</p><p>Region: Wybierz region najbliższy.</p> |
   |  **Połączenie usługi DNS i sieci VPN** |<p>Serwery DNS: Określ nazwę i adres IP co najmniej jeden lokalnych serwerów DNS.</p><p>Połączenie: Wybierz **skonfigurowania sieci VPN lokacja lokacja**.</p><p>Sieć lokalna: Określ nowe sieci lokalnej.</p><p>Jeśli korzystasz z usługi ExpressRoute zamiast sieci VPN, zobacz [skonfigurować połączenie ExpressRoute za pośrednictwem dostawcy usług Exchange](../expressroute/expressroute-locations-providers.md).</p> |
   |  **Połączenie lokacja lokacja** |<p>Nazwa: Nazwa sieci lokalnej.</p><p>Adres IP urządzenia sieci VPN: Określ publiczny adres IP urządzenia, które będą łączyć się z sieci wirtualnej. Urządzenie sieci VPN nie może znajdować się za translatorem adresów sieciowych.</p><p>Adres: Określ zakresy adresów w sieci lokalnej (np. 192.168.0.0/16 na diagramie scenariusz).</p> |
   |  **Przestrzeniami adresów sieci wirtualnej** |<p>Przestrzeń adresowa: Określ zakres adresów IP dla maszyn wirtualnych, które mają być uruchamiane w sieci wirtualnej platformy Azure (np. 10.1.0.0/16 na diagramie scenariusz). Ten zakres adresów nie może nakładać się na zakresy adresów sieci lokalnej.</p><p>Podsieci: Określ nazwę i adres dla podsieci dla serwerów aplikacji (np. serwera sieci Web, 10.1.1.0/24) i dla kontrolerów domeny (np. wewnętrznej bazy danych, 10.1.2.0/24).</p><p>Kliknij przycisk **Dodaj podsieć bramy**.</p> |
2. Następnie należy skonfigurować bramę sieci wirtualnej do utworzenia bezpiecznego połączenia sieci VPN typu lokacja lokacja. Zobacz [należy skonfigurować bramę sieci wirtualnej](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) dla instrukcji.
3. Utwórz połączenie sieci VPN lokacja lokacja między nową sieć wirtualną i lokalnego urządzenia sieci VPN. Zobacz [należy skonfigurować bramę sieci wirtualnej](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) dla instrukcji.

## <a name="create-azure-vms-for-the-dc-roles"></a>Tworzenie maszyn wirtualnych platformy Azure dla ról kontrolera domeny
Powtórz następujące kroki, aby utworzyć maszyn wirtualnych do obsługi roli kontrolera domeny, zgodnie z potrzebami. Należy wdrożyć co najmniej dwóch wirtualnych kontrolerów domeny w celu zapewnienia odporności na uszkodzenia i nadmiarowość. Jeśli sieć wirtualna platformy Azure zawiera co najmniej dwa kontrolery domeny, podobnie skonfigurowanych (to, że są one zarówno wykazów globalnych, uruchom serwer DNS, i nie zawiera żadnych ról FSMO i tak dalej) umieścić maszyn wirtualnych, które Uruchom te kontrolery domeny zbiór dostępności dla ulepszone odporność na uszkodzenia.
Aby utworzyć maszyn wirtualnych przy użyciu programu Windows PowerShell zamiast interfejsu użytkownika, zobacz [użycia programu Azure PowerShell do tworzenia i wstępnie skonfigurować maszyn wirtualnych z systemem Windows](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

1. W [klasycznego portalu Azure](https://manage.windowsazure.com), kliknij przycisk **nowy** > **obliczeniowe** > **maszyny wirtualnej** > **z galerii**. Użyj następujących wartości, aby zakończyć pracę kreatora. Zaakceptuj wartość domyślną ustawienia, chyba że inną wartość jest sugerowane lub wymagane.

   | Na tej stronie kreatora... | Określ te wartości |
   | --- | --- |
   |  **Wybierz obraz** |Windows Server 2012 R2 Datacenter |
   |  **Konfiguracja maszyny wirtualnej** |<p>Nazwa maszyny wirtualnej: Wpisz nazwy o pojedynczej etykiecie (na przykład AzureDC1).</p><p>Nowa nazwa użytkownika: Wpisz nazwę użytkownika. Ten użytkownik będzie członkiem lokalnej grupy Administratorzy na maszynie Wirtualnej. Konieczne będzie tę nazwę, aby zalogować się do maszyny Wirtualnej po raz pierwszy. Wbudowane konto o nazwie Administrator nie będzie działać.</p><p>Nowe hasło/Potwierdź: Wpisz hasło</p> |
   |  **Konfiguracja maszyny wirtualnej** |<p>Usługi w chmurze: Wybierz <b>Utwórz nową usługę w chmurze</b> pierwsza maszyna wirtualna i wybierz tej samej nazwy usługi chmury podczas tworzenia więcej maszyn wirtualnych, który będzie obsługiwać rolę kontrolera domeny.</p><p>Nazwa DNS usługi w chmurze: Określ globalnie unikatowa nazwa</p><p>Region/grupy koligacji/sieci wirtualnej: Określ nazwę sieci wirtualnej (np. WestUSVNet).</p><p>Konto magazynu: Wybierz <b>użyć konta magazynu automatycznie generowanych</b> dla pierwszej maszyny Wirtualnej, a następnie wybierz tej samej nazwy konta magazynu podczas tworzenia więcej maszyn wirtualnych, który będzie obsługiwać rolę kontrolera domeny.</p><p>Zestaw dostępności: Wybierz <b>tworzenia zestawu dostępności</b>.</p><p>Nazwa zbioru dostępności: wpisz nazwę zestawu dostępności podczas tworzenie pierwszej maszyny Wirtualnej, a następnie wybierz takie same nazwy podczas tworzenia więcej maszyn wirtualnych.</p> |
   |  **Konfiguracja maszyny wirtualnej** |<p>Wybierz <b>Zainstaluj agenta maszyny Wirtualnej</b> oraz innych rozszerzeń należy.</p> |
2. Dołączanie dysku do każdej maszyny Wirtualnej, które zostanie uruchomione rolę serwera Kontroler domeny. Dodatkowy dysk jest niezbędne do przechowywania bazy danych, dzienników i folderu SYSVOL usługi AD. Określ rozmiar dysku (np. 10 GB) i pozostawić **hosta pamięci podręcznej preferencji** ustawioną **Brak**. Aby uzyskać instrukcje, zobacz [jak dołączyć dysku danych do maszyny wirtualnej systemu Windows](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. Po pierwszy raz logujesz się do maszyny Wirtualnej, otwórz **Menedżera serwera** > **usług plików i magazynowania** Aby utworzyć wolumin na dysku w systemie plików NTFS.
4. Zarezerwuj statyczny adres IP dla maszyn wirtualnych, które będą uruchamiane roli kontrolera domeny. Aby Zarezerwuj statyczny adres IP, Pobierz Instalator platformy sieci Web firmy Microsoft i [zainstalować program Azure PowerShell](/powershell/azure/overview) i uruchom polecenie cmdlet Set-AzureStaticVNetIP. Na przykład:

    "AzureDC1 - ServiceName get AzureVM-Name AzureDC1 | Zestaw AzureStaticVNetIP - IPAddress 10.0.0.4 | AzureVM aktualizacji

Aby uzyskać więcej informacji na temat ustawiania statycznego adresu IP, zobacz [skonfigurować statyczny adres IP wewnętrznego dla maszyny Wirtualnej,](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Instalowanie usług AD DS na maszynach wirtualnych Azure
Zaloguj się do maszyny Wirtualnej i sprawdź, czy istnieje połączenie przez połączenie sieci VPN i ExpressRoute lokacja lokacja z zasobami w sieci lokalnej. Następnie należy zainstalować usługi AD DS na maszynach wirtualnych Azure. Można użyć tego samego procesu, który umożliwia instalowanie dodatkowego kontrolera domeny w sieci lokalnej (interfejsu użytkownika, środowiska Windows PowerShell lub pliku odpowiedzi). Jak zainstalować usługi AD DS, upewnij się, że określenie nowego woluminu dla lokalizacji bazy danych, dzienników i folderu SYSVOL usługi AD. Jeśli potrzebujesz odświeżacz w instalacji usług AD DS, zobacz [Instalowanie usług Active Directory domeny (poziom 100)](https://technet.microsoft.com/library/hh472162.aspx) lub [instalacji kontrolera domeny repliki systemu Windows Server 2012 w istniejącej domenie (poziom 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>Ponownie skonfiguruj serwer DNS dla sieci wirtualnej
1. W [portalu Azure](https://portal.azure.com)w **wyszukiwania zasobów** wprowadź *sieci wirtualnych*, następnie kliknij przycisk **sieci wirtualnych (klasyczne)** w wynikach wyszukiwania. Kliknij nazwę sieci wirtualnej, a następnie [zmienić konfigurację adresów IP serwerów DNS dla sieci wirtualnej](../virtual-network/virtual-network-manage-network.md#dns-servers) do używania statycznych adresów IP przypisane do replik kontrolerów domeny zamiast adresy IP lokalnych serwerów DNS.
2. Aby upewnić się, że wszystkie repliki kontrolera domeny maszyn wirtualnych w sieci wirtualnej skonfigurowano do korzystania z serwerów DNS w sieci wirtualnej, kliknij przycisk **maszyn wirtualnych**, kliknij kolumnę stan każdej maszyny wirtualnej, a następnie kliknij przycisk **ponownego uruchomienia**. Poczekaj, aż maszyny Wirtualnej zawiera **systemem** stanu przed podjęciem próby Zaloguj się do niego.

## <a name="create-vms-for-application-servers"></a>Tworzenie maszyn wirtualnych dla serwerów aplikacji
1. Powtórz następujące kroki, aby utworzyć maszyn wirtualnych do uruchamiania jako serwery aplikacji. Zaakceptuj wartość domyślną ustawienia, chyba że inną wartość jest sugerowane lub wymagane.

   | Na tej stronie kreatora... | Określ te wartości |
   | --- | --- |
   |  **Wybierz obraz** |Windows Server 2012 R2 Datacenter |
   |  **Konfiguracja maszyny wirtualnej** |<p>Nazwa maszyny wirtualnej: Wpisz nazwy o pojedynczej etykiecie (na przykład AppServer1).</p><p>Nowa nazwa użytkownika: Wpisz nazwę użytkownika. Ten użytkownik będzie członkiem lokalnej grupy Administratorzy na maszynie Wirtualnej. Konieczne będzie tę nazwę, aby zalogować się do maszyny Wirtualnej po raz pierwszy. Wbudowane konto o nazwie Administrator nie będzie działać.</p><p>Nowe hasło/Potwierdź: Wpisz hasło</p> |
   |  **Konfiguracja maszyny wirtualnej** |<p>Usługi w chmurze: Wybierz **Utwórz nową usługę w chmurze** dla pierwsza maszyna wirtualna, a następnie wybierz tego samego nazwa usługi w chmurze podczas tworzenia maszyn wirtualnych więcej będzie hosta aplikacji.</p><p>Nazwa DNS usługi w chmurze: Określ globalnie unikatowa nazwa</p><p>Region/grupy koligacji/sieci wirtualnej: Określ nazwę sieci wirtualnej (np. WestUSVNet).</p><p>Konto magazynu: Wybierz **użyć konta magazynu automatycznie generowanych** dla pierwszej maszyny Wirtualnej, a następnie wybierz tej samej nazwy konta magazynu podczas tworzenia więcej maszyn wirtualnych, które będą hosta aplikacji.</p><p>Zestaw dostępności: Wybierz **tworzenia zestawu dostępności**.</p><p>Nazwa zbioru dostępności: wpisz nazwę zestawu dostępności podczas tworzenie pierwszej maszyny Wirtualnej, a następnie wybierz takie same nazwy podczas tworzenia więcej maszyn wirtualnych.</p> |
   |  **Konfiguracja maszyny wirtualnej** |<p>Wybierz <b>Zainstaluj agenta maszyny Wirtualnej</b> oraz innych rozszerzeń należy.</p> |
2. Po udostępnieniu każdej maszyny Wirtualnej, zaloguj się i przyłączyć go do domeny. W **Menedżera serwera**, kliknij przycisk **lokalnego serwera** > **grupy roboczej** > **zmian...** a następnie wybierz **domeny** i wpisz nazwę domeny lokalnej. Podaj poświadczenia użytkownika domeny i uruchom ponownie maszyny Wirtualnej, aby ukończyć przyłączania do domeny.

Aby utworzyć maszyn wirtualnych przy użyciu programu Windows PowerShell zamiast interfejsu użytkownika, zobacz [użycia programu Azure PowerShell do tworzenia i wstępnie skonfigurować maszyn wirtualnych z systemem Windows](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Aby uzyskać więcej informacji o korzystaniu z programu Windows PowerShell, zobacz [wprowadzenie do poleceń cmdlet Azure](/powershell/azure/overview) i [dokumentacji poleceń Cmdlet Azure](/powershell/azure/get-started-azureps).

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Wskazówki dotyczące wdrażania systemu Windows serwer usługi Active Directory na maszynach wirtualnych Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Jak przekazywać istniejących kontrolerów domeny funkcji Hyper-V lokalnej na platformie Azure przy użyciu programu Azure PowerShell](http://support.microsoft.com/kb/2904015)
* [Instalowanie nowego lasu usługi Active Directory w sieci wirtualnej platformy Azure](active-directory-new-forest-virtual-machine.md)
* [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* [Microsoft Azure IaaS specjalista IT: (01) maszyny wirtualnej podstawy](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IaaS specjalista IT: (05) tworzenie sieci wirtualnych i łączności między lokalizacjami](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Azure PowerShell](/powershell/azure/overview)
* [Polecenia cmdlet do zarządzania platformy Azure](/powershell/module/azurerm.compute/#virtual_machines)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
