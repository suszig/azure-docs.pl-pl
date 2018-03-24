---
title: Zainstaluj lasu usługi Active Directory w sieci wirtualnej platformy Azure | Dokumentacja firmy Microsoft
description: Samouczek, który wyjaśnia sposób tworzenia nowego lasu usługi Active Directory na maszynie wirtualnej (VM) w sieci wirtualnej platformy Azure.
services: active-directory, virtual-network
keywords: 'Maszyna wirtualna usługi Active directory, instalacja lasu usługi active directory, filmy wideo z usługi azure active directory '
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
tags: ''
ms.assetid: eb7170d0-266a-4caa-adce-1855589d65d1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2017
ms.author: joflore
ms.openlocfilehash: 0b98b475f49847589398ca743fc3ac3b8bcbb381
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Instalowanie nowego lasu usługi Active Directory w sieci wirtualnej platformy Azure
W tym artykule przedstawiono sposób tworzenia nowego środowiska usługi Active Directory systemu Windows Server na maszynie wirtualnej (VM) na [sieci wirtualnej platformy Azure](../virtual-network/virtual-networks-overview.md). W takim przypadku sieci wirtualnej platformy Azure nie jest połączony z siecią lokalną.

Być może zainteresuje te pokrewne artykuły:

* Film przedstawiający tych kroków, zobacz [sposobu instalowania nowego lasu usługi Active Directory w sieci wirtualnej platformy Azure](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
* Można opcjonalnie [skonfigurowania sieci VPN lokacja lokacja](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) , a następnie zainstalować nowy las lub rozszerzyć lasu lokalnego do sieci wirtualnej platformy Azure. Te kroki opisane w artykule [Instalowanie repliki kontrolera domeny Active Directory w sieci wirtualnej platformy Azure](active-directory-install-replica-active-directory-domain-controller.md).
* Aby uzyskać ogólne wskazówki dotyczące instalowania usług domenowych w usłudze Active Directory (AD DS) w sieci wirtualnej platformy Azure, zobacz [wskazówki dotyczące wdrażania systemu Windows Server Active Directory na maszynach wirtualnych Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Diagram scenariusza
W tym scenariuszu użytkownicy zewnętrzni trzeba uzyskiwać dostęp do aplikacji działających na serwerach przyłączonych do domeny. Maszyn wirtualnych działających na serwery aplikacji i maszyn wirtualnych kontrolerów domeny z systemem są zainstalowane w ich własnych usługi w chmurze w sieci wirtualnej platformy Azure. Są one również uwzględniona w obrębie zbiór dostępności dla ulepszone odporność na uszkodzenia.

![Las usługi Active Directory na maszyny wirtualne w sieci wirtualnej Azure ][1] 7

## <a name="how-does-this-differ-from-on-premises"></a>Jak jest to różnica między lokalnymi?
Nie jest znacznie różnica między instalowania kontrolera domeny na platformie Azure lub lokalnie. W poniższej tabeli wymieniono główne różnice.

| Aby skonfigurować... | Lokalnie | Sieć wirtualna Azure |
| --- | --- | --- |
| **Adres IP dla kontrolera domeny** |Przypisz statyczny adres IP we właściwościach karty sieciowej |Uruchom polecenie cmdlet Set-AzureStaticVNetIP można przypisać statycznego adresu IP |
| **Program rozpoznawania nazw DNS klienta** |Ustaw adres serwera preferowany i alternatywny serwer DNS w sieci karty właściwości elementów członkowskich domeny |Ustaw adres serwera DNS na właściwości sieci wirtualnej |
| **Magazyn baz danych w usłudze Active Directory** |Opcjonalnie zmienić domyślną lokalizację przechowywania z C:\ |Aby zmienić domyślną lokalizację przechowywania z C:\ |

## <a name="create-an-azure-virtual-network"></a>Tworzenie sieci wirtualnej platformy Azure
1. Zaloguj się do Portalu Azure.
2. Utwórz sieć wirtualną. Kliknij przycisk **sieci** > **utworzyć sieć wirtualną**. Użyj wartości w tabeli poniżej, aby zakończyć pracę kreatora.

   | Na tej stronie kreatora... | Określ te wartości |
   | --- | --- |
   |  **Szczegóły sieci wirtualnej** |<p>Nazwa: Wprowadź nazwę sieci wirtualnej</p><p>Region: Wybierz region najbliższy</p> |
   |  **DNS i sieci VPN** |<p>Pozostaw puste serwera DNS</p><p>Nie należy wybierać opcji albo sieci VPN</p> |
   |  **Przestrzeniami adresów sieci wirtualnej** |<p>Nazwa podsieci: Wprowadź nazwę dla podsieci</p><p>Rozpoczynanie IP: <b>10.0.0.0</b></p><p>CIDR: <b>PREFIKSIE/24 (256)</b></p> |

## <a name="create-vms-to-run-the-domain-controller-and-dns-server-roles"></a>Tworzenie maszyn wirtualnych do uruchomienia kontrolera domeny i ról serwera DNS
Powtórz następujące kroki, aby utworzyć maszyn wirtualnych do obsługi roli kontrolera domeny, zgodnie z potrzebami. Należy wdrożyć co najmniej dwóch wirtualnych kontrolerów domeny w celu zapewnienia odporności na uszkodzenia i nadmiarowość. Jeśli sieć wirtualna platformy Azure zawiera co najmniej dwa kontrolery domeny, podobnie skonfigurowanych (to, że są one zarówno wykazów globalnych, uruchom serwer DNS, i nie zawiera żadnych ról FSMO i tak dalej) umieścić maszyn wirtualnych, które Uruchom te kontrolery domeny zbiór dostępności dla ulepszone odporność na uszkodzenia.

Aby utworzyć maszyn wirtualnych przy użyciu programu Windows PowerShell zamiast interfejsu użytkownika, zobacz [Utwórz maszynę wirtualną przy użyciu programu PowerShell](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-quick.md) próbki.

1. W portalu Azure wybierz **nowy** > **obliczeniowe**, a następnie wybierz maszynę wirtualną. Użyj następujących wartości, aby zakończyć pracę kreatora. Zaakceptuj wartość domyślną ustawienia, chyba że inną wartość jest sugerowane lub wymagane.

   | Na tej stronie kreatora... | Określ te wartości |
   | --- | --- |
   |  **Wybierz obraz** |Windows Server 2012 R2 Datacenter |
   |  **Konfiguracja maszyny wirtualnej** |<p>Nazwa maszyny wirtualnej: Wpisz nazwy o pojedynczej etykiecie (na przykład AzureDC1).</p><p>Nowa nazwa użytkownika: Wpisz nazwę użytkownika. Ten użytkownik będzie członkiem lokalnej grupy Administratorzy na maszynie Wirtualnej. Konieczne będzie tę nazwę, aby zalogować się do maszyny Wirtualnej po raz pierwszy. Wbudowane konto o nazwie Administrator nie będzie działać.</p><p>Nowe hasło/Potwierdź: Wpisz hasło</p> |
   |  **Konfiguracja maszyny wirtualnej** |<p>Usługi w chmurze: Wybierz <b>Utwórz nową usługę w chmurze</b> pierwsza maszyna wirtualna i wybierz tej samej nazwy usługi chmury podczas tworzenia więcej maszyn wirtualnych, który będzie obsługiwać rolę kontrolera domeny.</p><p>Nazwa DNS usługi w chmurze: Określ globalnie unikatowa nazwa</p><p>Region/grupy koligacji/sieci wirtualnej: Określ nazwę sieci wirtualnej (np. WestUSVNet).</p><p>Konto magazynu: Wybierz <b>użyć konta magazynu automatycznie generowanych</b> dla pierwszej maszyny Wirtualnej, a następnie wybierz tej samej nazwy konta magazynu podczas tworzenia więcej maszyn wirtualnych, który będzie obsługiwać rolę kontrolera domeny.</p><p>Zestaw dostępności: Wybierz <b>tworzenia zestawu dostępności</b>.</p><p>Nazwa zbioru dostępności: wpisz nazwę zestawu dostępności podczas tworzenie pierwszej maszyny Wirtualnej, a następnie wybierz takie same nazwy podczas tworzenia więcej maszyn wirtualnych.</p> |
   |  **Konfiguracja maszyny wirtualnej** |<p>Wybierz <b>Zainstaluj agenta maszyny Wirtualnej</b> oraz innych rozszerzeń należy.</p> |
2. Dołączanie dysku do każdej maszyny Wirtualnej, które zostanie uruchomione rolę serwera Kontroler domeny. Dodatkowy dysk jest niezbędne do przechowywania bazy danych, dzienników i folderu SYSVOL usługi AD. Określ rozmiar dysku (np. 10 GB) i pozostawić **hosta pamięci podręcznej preferencji** ustawioną **Brak**. Aby uzyskać instrukcje, zobacz [jak dołączyć dysku danych do maszyny wirtualnej systemu Windows](../virtual-machines/windows/attach-managed-disk-portal.md).
3. Po pierwszy raz logujesz się do maszyny Wirtualnej, otwórz **Menedżera serwera** > **usług plików i magazynowania** Aby utworzyć wolumin na dysku w systemie plików NTFS.
4. Zarezerwuj statyczny adres IP dla maszyn wirtualnych, które będą uruchamiane roli kontrolera domeny. Aby Zarezerwuj statyczny adres IP, Pobierz Instalator platformy sieci Web firmy Microsoft i [zainstalować program Azure PowerShell](/powershell/azure/overview) i uruchom polecenie cmdlet Set-AzureStaticVNetIP. Na przykład:

    `Get-AzureVM -ServiceName AzureDC1 -Name AzureDC1 | Set-AzureStaticVNetIP -IPAddress 10.0.0.4 | Update-AzureVM`

Aby uzyskać więcej informacji na temat ustawiania statycznego adresu IP, zobacz [skonfigurować statyczny adres IP wewnętrznego dla maszyny Wirtualnej,](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-windows-server-active-directory"></a>Zainstaluj usługi Active Directory systemu Windows Server
Użyj tej samej procedury, aby [zainstalowania usług AD DS](https://technet.microsoft.com/library/jj574166.aspx) Używanie lokalnego (oznacza to, należy użyć interfejsu użytkownika, pliku odpowiedzi lub środowiska Windows PowerShell). Musisz podać poświadczenia administratora, aby zainstalować nowy las. Aby określić lokalizację bazy danych usługi Active Directory, dzienników i folderu SYSVOL, Zmień domyślną lokalizację przechowywania dysk systemu operacyjnego, dysku dodatkowych danych, dołączony do maszyny Wirtualnej.

Po zakończeniu instalacji kontrolera domeny, ponownie połączenie z maszyną Wirtualną, a następnie zaloguj się do kontrolera domeny. Pamiętaj podać poświadczenia domeny.

## <a name="reset-the-dns-server-for-the-azure-virtual-network"></a>Resetuj serwera DNS dla sieci wirtualnej platformy Azure
1. Zresetować ustawienie usługi przesyłania dalej DNS na nowym serwerze DNS kontrolera domeny.
   1. W Menedżerze serwera kliknij **narzędzia** > **DNS**.
   2. W **Menedżera DNS**, kliknij prawym przyciskiem myszy nazwę serwera DNS i kliknij przycisk **właściwości**.
   3. Na **usług przesyłania dalej** , kliknij adres IP usługi przesyłania dalej i kliknij **Edytuj**.  Wybierz adres IP, a następnie kliknij przycisk **usunąć**.
   4. Kliknij przycisk **OK** do Zamknij Edytor i **Ok** ponownie, aby zamknąć właściwości serwera DNS.
2. Zaktualizuj ustawienia serwera DNS dla sieci wirtualnej.
   1. Kliknij przycisk **sieci wirtualnych** > kliknij dwukrotnie sieć wirtualną utworzono > **Konfiguruj** > **serwerów DNS**, wpisz nazwę i adres IP z jednej z maszyn wirtualnych którym działa rola serwera DNS kontrolera domeny i kliknij przycisk **zapisać**.
   2. Wybierz maszynę Wirtualną, a następnie kliknij przycisk **ponowne uruchomienie** do wyzwolenia maszyny Wirtualnej, aby skonfigurować ustawienia programu rozpoznawania nazw DNS przy użyciu adresu IP nowego serwera DNS.

## <a name="create-vms-for-domain-members"></a>Tworzenie maszyn wirtualnych dla członków domeny
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

## <a name="see-also"></a>Zobacz też
* [Jak zainstalować nowy las usługi Active Directory w sieci wirtualnej platformy Azure](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
* [Wskazówki dotyczące wdrażania systemu Windows serwer usługi Active Directory na maszynach wirtualnych Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Konfigurowanie sieci VPN lokacja lokacja](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Instalowanie repliki kontrolera domeny Active Directory w sieci wirtualnej platformy Azure](active-directory-install-replica-active-directory-domain-controller.md)
* [Microsoft Azure IaaS specjalista IT: (01) maszyny wirtualnej podstawy](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IaaS specjalista IT: (05) tworzenie sieci wirtualnych i łączności między lokalizacjami](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Omówienie usługi Virtual Network](../virtual-network/virtual-networks-overview.md)
* [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Dokumentacja poleceń cmdlet platformy Azure](/powershell/azure/get-started-azureps)
* [Ustaw adres statyczny adres IP maszyny Wirtualnej platformy Azure](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
* [Jak przypisać statyczny adres IP do maszyny Wirtualnej Azure](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
* [Instalowanie nowego lasu usługi Active Directory](https://technet.microsoft.com/library/jj574166.aspx)
* [Wprowadzenie do wirtualizacji usług (AD DS) domeny usługi Active Directory (poziom 100)](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
