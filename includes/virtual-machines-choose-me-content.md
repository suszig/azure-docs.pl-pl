<a name="tellmevm"></a>

## <a name="tell-me-about-virtual-machines"></a>Opowiedz mi o maszynach wirtualnych
Usługa Azure Virtual Machines umożliwia tworzenie i używanie maszyn wirtualnych w chmurze. Z technologii maszyn wirtualnych zapewniającej dostęp do *infrastruktury jako usługi (IaaS)* można korzystać na różne sposoby. Przykłady to:

* **Maszyny wirtualne na potrzeby tworzenia i testowania.** Grupy programistów często używają maszyn wirtualnych, ponieważ oferują one szybki i łatwy sposób tworzenia komputerów o określonej konfiguracji wymaganej do kodowania i testowania aplikacji. Usługa Azure Virtual Machines zapewnia prosty i ekonomiczny sposób tworzenia maszyn wirtualnych, korzystania z nich, a następnie usuwania ich, gdy nie są już potrzebne.
* **Uruchamianie aplikacji w chmurze.** Uruchamianie niektórych aplikacji w chmurze publicznej jest z punktu widzenia ekonomii prawidłowym podejściem. Przykładem jest aplikacja, która ma duże wartości szczytowe zapotrzebowania. Mimo że można wyposażyć centrum danych z wystarczającą ilość sprzętu do obsługi szczytowego zapotrzebowania, sprzęt ten może być niewystarczająco wykorzystywany przez większość czasu. Uruchomienie takiej aplikacji na platformie Azure pozwala na zapłacenie za dodatkowe maszyny wirtualny tylko wtedy, gdy są potrzebne, a następnie na ich zamknięcie. Załóżmy z kolei, że masz nową firmę, która szybko potrzebuje zasobów obliczeniowych dostępnych na żądanie i bez zobowiązań. I w tym przypadku warto wybrać platformę Azure.
* **Rozszerzanie własnego centrum danych do chmury publicznej.** W przypadku korzystania z usługi Azure Virtual Network organizacja może utworzyć sieć wirtualną (VNET) stanowiącą rozszerzenie sieci lokalnej i dodać maszyny wirtualne do tej sieci VNET. Umożliwia to uruchamianie aplikacji, takich jak [SharePoint](../articles/virtual-machines/virtual-machines-windows-sharepoint-farm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), [SQL Server](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) i inne, na maszynie wirtualnej platformy Azure. Takie podejście może być łatwiejsze do wdrożenia lub mniej kosztowne niż uruchamianie ich na maszynach wirtualnych we własnych centrum danych.   
* **Odzyskiwanie po awarii.** Zamiast bez przerwy płacić za zapasowe, rzadko używane centrum danych, dzięki funkcji odzyskiwania sprawności po awarii korzystająca z usługi IaaS można płacić za zasoby obliczeniowe tylko wtedy, gdy są naprawdę potrzebne.  Na przykład w razie awarii głównego centrum danych można utworzyć działające na platformie Azure maszyny wirtualne, które pozwalają na uruchomienie najważniejszych aplikacji. Gdy przestaną one być potrzebne, można je wyłączyć.

Podobnie jak inne maszyny wirtualne, maszyny wirtualne platformy Azure są wyposażone w system operacyjny, oferują możliwości sieciowe i magazynowe oraz umożliwiają uruchamianie wielu różnych aplikacji. Możesz użyć obrazu dostarczonego przez zespół Azure lub partnera Azure bądź skorzystać z własnego obrazu. Przykłady obejmują różne wersje i konfiguracje elementów wymienionych poniżej:

* Serwery systemu Linux, takie jak Suse, Ubuntu i CentOS
* Oprogramowanie Windows Server 
* Oprogramowanie SQL Server
* Platforma BizTalk Server 
* Oprogramowanie SharePoint Server

Maszyny wirtualne wykorzystują wirtualne dyski twarde do przechowywania ich systemu operacyjnego oraz danych. Wirtualne dyski twarde są również używane do obsługi obrazów, spośród których można wybierać, chcąc zainstalować system operacyjny. Pokazano to na poniższej ilustracji, na której widoczne są także dwa spośród narzędzi służących do tworzenia maszyn wirtualnych i zarządzania nimi.

<a name="fig_createvms"></a>
![vm_diagram](./media/virtual-machines-choose-me-content/diagram.png)

**Rysunek: Maszyny wirtualne platformy Azure zapewniają dostęp do infrastruktury jako usługi.**

Maszynami wirtualnymi można zarządzać z poziomu portalu w przeglądarce internetowej, a także za pomocą narzędzi wiersza polecenia z obsługą skryptów lub bezpośrednio za pomocą interfejsu API REST. Partnerzy firmy Microsoft, tacy jak RightScale i ScaleXtreme, także dostarczają usługi z zakresu zarządzania, które opierają się na interfejsie API REST. 

Poza systemem operacyjnym dostępne są także inne parametry konfiguracji maszyn wirtualnych:

* Rozmiar, który określa m.in. liczbę dysków możliwych do podłączenia oraz moc obliczeniową. W ramach platformy Azure dostępna jest szeroka gama rozmiarów umożliwiających wykorzystanie produktu do wielu różnych zastosowań. Aby uzyskać szczegółowe informacje, zobacz [Sizes for virtual machines](../articles/virtual-machines/virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Rozmiary maszyn wirtualnych).  
* Region platformy Azure, w którym będzie hostowana nowa maszyna wirtualna, taki jak USA, Europa lub Azja. 
* Rozszerzenia maszyn wirtualnych, które zapewniają im dodatkowe możliwości, takie jak uruchamianie oprogramowania antywirusowego lub korzystanie z funkcji konfiguracji żądanego stanu programu Windows PowerShell.

Inne korzyści z maszyn wirtualnych, które należy wziąć pod uwagę, obejmują:

**Płatność zgodnie z rzeczywistym użyciem** — użytkownicy platformy Azure płacą cenę godzinową określoną na podstawie rozmiaru i systemu operacyjnego maszyny wirtualnej. Za kolejne rozpoczęte godziny na użytkowników platformy Azure są nakładane opłaty wyłącznie za wykorzystane minuty. Magazyn jest wyceniany oddzielnie; związane z nim opłaty są także pobierane osobno. Aby uzyskać szczegółowe informacje, zobacz [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/) (Cennik usługi Virtual Machines).

**Odporność** — w ramach platformy Azure jest monitorowany sprzęt fizyczny wykorzystywany do hostowania poszczególnych uruchomionych maszyn wirtualnych. Ewentualna awaria serwera fizycznego z uruchomioną maszyną wirtualną zostaje wykryta przez zespół Azure, po czym maszyna wirtualna zostaje przeniesiona na nowy sprzęt oraz ponownie uruchomiona. Ten proces jest niekiedy nazywany naprawą usługi. Zespół Azure chroni także dane maszyny wirtualnej, przechowując nadmiarowe kopie dysków VHD w magazynie obiektów blob. 



<!--HONumber=Jan17_HO2-->


