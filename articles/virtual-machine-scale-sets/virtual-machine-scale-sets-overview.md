---
title: "Przegląd zestawów skalowania maszyn wirtualnych | Microsoft Docs"
description: "Dowiedz się więcej o zestawach skalowania maszyn wirtualnych"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 1/25/2017
ms.author: guybo
translationtype: Human Translation
ms.sourcegitcommit: 868d65642ab9ca3e1c35d33a7cb9e9dd8c31c430
ms.openlocfilehash: f945ac0357c11c70780dea8e62f094457a213d96


---
# <a name="virtual-machine-scale-sets-overview"></a>Omówienie zestawów skalowania maszyn wirtualnych
Zestawy skalowania maszyn wirtualnych to zasób dostępny w ramach usługi Azure Compute, który umożliwia wdrożenie zestawu identycznych maszyn wirtualnych oraz zarządzanie nimi. Wszystkie maszyny wirtualne mają jednakową konfigurację, dzięki czemu zestawy skalowania maszyn wirtualnych umożliwiają rzeczywiste skalowanie automatyczne (bez wstępnej aprowizacji maszyn wirtualnych). Dzięki temu można łatwiej tworzyć wielkoskalowe usługi z myślą o intensywnych operacjach obliczeniowych, obsłudze danych big data oraz obciążeń konteneryzowanych.

W przypadku aplikacji skalujących zasoby komputerowe w poziomie operacje skalowania są niejawnie równoważone w domenach błędów i aktualizacji. Aby zapoznać się z zagadnieniami dotyczącymi zestawów skalowania maszyn wirtualnych, zobacz [Ogłoszenia na blogu platformy Azure](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Aby dowiedzieć się więcej o zestawach skalowania maszyn wirtualnych, obejrzyj te klipy wideo:

* [Mark Russinovich omawia zestawy skalowania na platformie Azure](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Zestawy skalowania maszyn wirtualnych według Guya Bowermana](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Tworzenie zestawów skalowania maszyn wirtualnych i zarządzanie nimi
Zestaw skalowania maszyn wirtualnych można utworzyć w witrynie [Azure Portal](https://portal.azure.com), wybierając pozycję *nowy* i wpisując na pasku wyszukiwania hasło „skalowanie”. Wśród wyników wyszukiwania pojawi się hasło „zestaw skalowania maszyn wirtualnych”. Z tego miejsca można wypełnić wymagane pola w celu dostosowania i wdrożenia zestawu skalowania. Warto zauważyć, że w portalu dostępne są również opcje umożliwiające konfigurację podstawowych reguł skalowania automatycznego na podstawie użycia procesora CPU.

Zestawy skalowania maszyn wirtualnych można także definiować i wdrażać za pomocą szablonów JSON oraz [interfejsów API REST](https://msdn.microsoft.com/library/mt589023.aspx) — tak jak poszczególne maszyny wirtualne w ramach usługi Azure Resource Manager. Można zatem użyć dowolnej standardowej metody wdrażania za pomocą usługi Azure Resource Manager. Aby uzyskać więcej informacji na temat szablonów, zobacz [Tworzenie szablonów usługi Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

Zestaw przykładowych szablonów dla zestawów skalowania maszyn wirtualnych znajduje się w repozytorium szablonów z pakietu Azure Quickstart w witrynie GitHub ([tutaj](https://github.com/Azure/azure-quickstart-templates)). (Poszukaj szablonów z ciągiem *vmss* w nazwie).

Na stronach szczegółów tych szablonów jest dostępny przycisk, który prowadzi do funkcji wdrażania w portalu. Aby wdrożyć zestaw skalowania maszyn wirtualnych, kliknij ten przycisk, a następnie wypełnij wymagane parametry w portalu. Jeśli nie wiesz, czy dany zasób obsługuje wielkie lub małe i wielkie litery, lepiej użyć wartości parametrów pisanych małymi literami i cyframi. Dostępna jest również przydatna analiza wideo szablonu zestawu skalowania maszyn wirtualnych:

[Analiza szablonu zestawu skalowania maszyn wirtualnych](https://channel9.msdn.com/Blogs/Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Skalowanie w poziomie zestawu skalowania maszyny wirtualnej
Aby zwiększyć lub zmniejszyć liczbę maszyn wirtualnych w zestawie skalowania maszyn wirtualnych, wystarczy zmienić właściwość *capacity* i ponownie wdrożyć szablon. Ta prosta metoda umożliwia utworzenie własnej niestandardowej warstwy skalowania w celu zdefiniowania niestandardowych zdarzeń skalowania, które nie są obsługiwane przez skalowanie automatyczne na platformie Azure.

W przypadku ponownego wdrażania szablonu w celu zmiany pojemności można zdefiniować dużo mniejszy szablon obejmujący jedynie pakiet właściwości SKU ze zaktualizowaną pojemnością. Przykład takiego działania można znaleźć [tutaj](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing).

Aby poznać procedurę tworzenia zestawu skalowania, który jest skalowany automatycznie, zobacz [Automatyczne skalowanie maszyn w zestawie skalowania maszyn wirtualnych](virtual-machine-scale-sets-windows-autoscale.md).

## <a name="monitoring-your-vm-scale-set"></a>Monitorowanie zestawu skalowania maszyn wirtualnych
Witryna [Azure Portal](https://portal.azure.com) wyświetla zestawy skalowania i pokazuje podstawowe właściwości i operacje łącznie z wyświetleniem listy maszyn wirtualnych w zestawie i wykresem wykorzystania zasobu. Aby uzyskać więcej szczegółów, można wyświetlić zestawy skalowania maszyn wirtualnych za pomocą [Eksploratora zasobów Azure](https://resources.azure.com). Zestawy skalowania maszyn wirtualnych są zasobami dostępnymi w ramach dostawcy zasobów Microsoft.Compute. Z poziomu tej witryny można je wyświetlić, rozwijając następujące linki:

**Subskrypcje -> Twoja subskrypcja -> resourceGroups -> dostawcy -> Microsoft.Compute -> virtualMachineScaleSets -> zestaw skalowania maszyny wirtualnej -> itp.**

## <a name="vm-scale-set-scenarios"></a>Scenariusze dotyczące zestawów skalowania maszyn wirtualnych
W tej sekcji przedstawiono niektóre typowe scenariusze dotyczące zestawów skalowania maszyn wirtualnych. Scenariusze te mają zastosowanie w przypadku niektórych usług platformy Azure wyższego poziomu (np. Batch, Service Fabric, Azure Container Service).

* **Połączenia RDP/SSH z wystąpieniami zestawów skalowania maszyn wirtualnych** — zestaw skalowania maszyn wirtualnych jest tworzony w sieci wirtualnej, a poszczególne maszyny wirtualne w zestawie skalowania nie uzyskują publicznych adresów IP. Jest to przydatne, ponieważ lepiej unikać wydatków i nakładów pracy związanych z przydzielaniem osobnych publicznych adresów IP wszystkim zasobom bezstanowym w sieci obliczeniowej. Z maszynami wirtualnymi można się łatwo połączyć z poziomu innych zasobów w sieci wirtualnej (także tych, które mają publiczne adresy IP, takich jak moduły równoważenia obciążenia lub pojedyncze maszyny wirtualne).
* **Łączenie się z maszynami wirtualnymi za pomocą reguł NAT** — można utworzyć publiczny adres IP, przypisać go do modułu równoważenia obciążenia i zdefiniować pulę NAT dla ruchu przychodzącego mapującą porty pod adresem IP na port maszyny wirtualnej w zestawie skalowania maszyn wirtualnych. Na przykład:
  
  | Element źródłowy | Port źródłowy | Element docelowy | Port docelowy |
  | --- | --- | --- | --- |
  |  Publiczny adres IP |Port 50000 |vmss\_0 |Port 22 |
  |  Publiczny adres IP |Port 50001 |vmss\_1 |Port 22 |
  |  Publiczny adres IP |Port 50002 |vmss\_2 |Port 22 |
  
   Oto przykład tworzenia zestawu skalowania maszyn wirtualnych korzystającego z reguł NAT w celu udostępnienia połączenia SSH każdej maszynie wirtualnej w zestawie skalowania przy użyciu jednego publicznego adresu IP: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)
  
   Tu natomiast pokazano, jak wykonać powyższe działania przy użyciu połączenia RDP i systemu Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)
* **Łączenie się z maszynami wirtualnymi za pomocą „rampy”** — jeśli utworzysz zestaw skalowania maszyn wirtualnych i autonomiczną maszynę wirtualną w tej samej sieci wirtualnej, autonomiczna maszyna wirtualna i maszyny wirtualne z zestawu skalowania mogą się łączyć ze sobą za pomocą wewnętrznych adresów IP zgodnie z definicją w sieci wirtualnej/podsieci. W przypadku utworzenia publicznego adresu IP i przypisania go do autonomicznej maszyny wirtualnej można nawiązać połączenie RDP lub SSH z autonomiczną maszyną wirtualną, a następnie z jej poziomu połączyć się z wystąpieniami danego zestawu skalowania maszyn wirtualnych. Można tutaj zauważyć, że prosty zestaw skalowania maszyn wirtualnych jest z natury bezpieczniejszy niż prosta autonomiczna maszyna wirtualna z publicznym adresem IP w konfiguracji domyślnej.
  
   Na przykład ten szablon wdraża prosty zestaw skalowania z autonomiczną maszyną wirtualną: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox)
* **Równoważenie obciążenia wystąpień zestawów skalowania maszyn wirtualnych** — aby przekazywać zadania do klastra obliczeniowego maszyn wirtualnych w trybie okrężnym, można odpowiednio skonfigurować moduł równoważenia obciążenia platformy Azure za pomocą reguł równoważenia obciążenia w warstwie&4;. Definiując sondy do weryfikowania działania aplikacji, można wysyłać polecenia ping do portów przy użyciu określonego protokołu, interwału i ścieżki żądania. Usługa Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/) obsługuje również zestawy skalowania, a także warstwę&7; oraz bardziej zaawansowane scenariusze równoważenia obciążenia.
  
   Oto przykład tworzenia zestawu skalowania maszyny wirtualnej z uruchomionymi serwerami sieci Web Apache, która używa modułu równoważenia obciążenia do zrównoważenia obciążenia każdej maszyny wirtualnej: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl) (zapoznaj się z typem zasobu Microsoft.Network/loadBalancers oraz profilem networkProfile i extensionProfile w zestawie virtualMachineScaleSet)
* **Wdrażanie zestawu skalowania maszyn wirtualnych jako klastra obliczeniowego w menedżerze klastra w środowisku PaaS** — zestawy skalowania maszyn wirtualnych są czasami określane jako rola procesu roboczego następnej generacji. Jest to trafny opis, ale niesie ryzyko pomylenia funkcji zestawu skalowania z funkcjami roli procesu roboczego usługi PaaS w wersji&1;. Zestawy skalowania maszyn wirtualnych pełnią rolę procesu roboczego lub stanowią zasób procesu roboczego w tym sensie, że są ogólnym zasobem obliczeniowym, który jest niezależny od platformy/środowiska uruchomieniowego, możliwy do dostosowania i zintegrowania z infrastrukturą IaaS usługi Azure Resource Manager.
  
   Rola procesu roboczego usługi PaaS w wersji&1; ma wprawdzie ograniczenia w zakresie obsługi platform/środowiska wykonawczego (obsługuje wyłącznie obrazy platformy Windows), ale obejmuje usługi, takie jak wymiana wirtualnego adresu IP, konfigurowane ustawienia uaktualniania i określone ustawienia wdrażania środowiska wykonawczego/aplikacji, które nie są *jeszcze* dostępne w zestawach skalowania maszyn wirtualnych lub zostaną zapewnione przez inne usługi PaaS wyższego poziomu, takie jak Service Fabric. Uwzględniając powyższe informacje, można postrzegać zestawy skalowania maszyn wirtualnych jako infrastrukturę obsługującą usługę PaaS. A zatem rozwiązania PaaS (np. Service Fabric) lub menedżery klastrów (np. Mesos) mogą bazować na zestawach skalowania maszyn wirtualnych jako skalowalna warstwa obliczeniowa.
  
   Przykładem takiego podejścia jest wdrożenie klastra usługi Azure Container Service opartego na zestawach skalowania z koordynatorem kontenera: [https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos).

## <a name="vm-scale-set-performance-and-scale-guidance"></a>Wytyczne dotyczące wydajności i skalowania zestawów skalowania maszyn wirtualnych
* Jeśli używasz zestawów skalowania z dyskami niezarządzanymi, nie planuj więcej niż 20 maszyn wirtualnych przypadających na konto magazynu (chyba że dla właściwości *overprovision* ustawisz wartość „false” — wówczas możesz zwiększyć liczbę maszyn wirtualnych do 40). W przypadku dysków zarządzanych te limity dla poszczególnych kont magazynu nie mają zastosowania.
* Podczas korzystania z zestawów skalowania z dyskami niezarządzanymi rozłóż jak najszerzej pierwsze litery nazw kont magazynu. Odpowiednią procedurę można znaleźć, przeglądając przykładowe szablony VMSS w artykule [Szablony z pakietu Azure Quickstart](https://github.com/Azure/azure-quickstart-templates/).
* W przypadku niestandardowych maszyn wirtualnych z dyskami niezarządzanymi w jednym zestawie skalowania maszyn wirtualnych nie powinno znaleźć się więcej niż 40 maszyn wirtualnych w ramach jednego konta magazynu. Przed rozpoczęciem wdrażania zestawu skalowania maszyn wirtualnych trzeba wstępnie skopiować obraz do konta magazynu. Zestawy skalowania z dyskami zarządzanymi obsługują maksymalnie 100 maszyn wirtualnych opartych na obrazach niestandardowych. Zapoznaj się z często zadawanymi pytaniami, aby uzyskać więcej informacji.
* W każdej sieci wirtualnej nie powinno się znajdować więcej niż 4096 maszyn wirtualnych.
* Liczba maszyn wirtualnych, które można utworzyć, jest ograniczona przez przydział rdzeni w regionie wdrażania. Nawet jeśli obecnie masz wysoki limit rdzeni na użytek usług w chmurze lub środowiska IaaS w wersji&1;, może zaistnieć konieczność zwiększenia limitu przydziału obliczeniowego (za pośrednictwem działu pomocy technicznej). Aby wysłać zapytanie dotyczące przydziału, możesz uruchomić następujące polecenie w interfejsie wiersza polecenia platformy Azure: `azure vm list-usage` oraz następujące polecenie programu PowerShell: `Get-AzureRmVMUsage` (w przypadku programu PowerShell w wersji starszej niż 1.0 użyj polecenia `Get-AzureVMUsage`).

## <a name="vm-scale-set-frequently-asked-questions"></a>Często zadawane pytania dotyczące zestawu skalowania maszyn wirtualnych
**PYTANIE** Ile maszyn wirtualnych może być w zestawie skalowania maszyn wirtualnych?

**ODPOWIEDŹ** W przypadku dysków zarządzanych zestaw skalowania może zawierać od 0 do 1000 maszyn wirtualnych opartych na obrazach platformy lub od 0 do 100 maszyn wirtualnych opartych na obrazach niestandardowych. Jeśli chodzi o dyski niezarządzane (gdzie definiuje się własne konta magazynu), limit ten wynosi 100 maszyn wirtualnych dla obrazów platformy i maksymalnie 40 dla obrazów niestandardowych (jeśli właściwość *overprovision* jest ustawiona na wartość „false”, domyślny limit to 20) — jest tak, ponieważ obrazy niestandardowe w przypadku dysków niezarządzanych są ograniczone do pojedynczego konta magazynu.

**PYTANIE** Czy zestawy skalowania maszyn wirtualnych obsługują dyski danych?

**ODPOWIEDŹ** Tak. Zestaw skalowania zdefiniowany z uwzględnieniem magazynu zarządzanego może zdefiniować konfigurację dołączonych dysków danych, która będzie stosowana do wszystkich maszyn wirtualnych w zestawie. Zestawy skalowania, które nie są zdefiniowane z użyciem magazynu zarządzanego, nie mają dołączonych dysków danych. Oto przykłady innych opcji magazynowania danych:

* Pliki platformy Azure (dyski udostępnione za pośrednictwem protokołu SMB)
* Dysk systemu operacyjnego
* Dysk tymczasowy (lokalny, bez kopii zapasowej w usłudze Azure Storage)
* Usługa danych platformy Azure (tabele platformy Azure, obiekty blob platformy Azure)
* Zewnętrzne usługi danych (np. zdalna baza danych)

**PYTANIE** Które regiony świadczenia usługi Azure obsługują zestawy skalowania maszyn wirtualnych?

**ODPOWIEDŹ** Wszystkie regiony obsługują zestawy skalowania maszyn wirtualnych.

**PYTANIE** Jak utworzyć zestaw skalowania maszyn wirtualnych za pomocą obrazu niestandardowego?

**ODPOWIEDŹ** Pozostaw właściwość vhdContainers pustą (lub pomiń ją) i podaj identyfikator URI właściwości obrazu. Na przykład: [201-vmss-windows-customimage].(https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-customimage)


**PYTANIE** Jeśli zmniejszę pojemność zestawu skalowania maszyn wirtualnych z 20 do 15, które maszyny wirtualne zostaną usunięte?

**ODPOWIEDŹ** W celu zapewnienia maksymalnej dostępności maszyny wirtualne są usuwane z zestawu skalowania równomiernie w domenach błędów i domenach uaktualniania. Najpierw są usuwane maszyny wirtualne o największym identyfikatorze.

**PYTANIE** Co się stanie, jeśli następnie zwiększę pojemność z 15 do 18?

**ODPOWIEDŹ** Zwiększenie pojemności do 18 spowoduje utworzenie 3 nowych maszyn wirtualnych. Za każdym razem następuje zwiększenie identyfikatora wystąpienia maszyny wirtualnej względem poprzedniej najwyższej wartości (np. 20, 21, 22). Maszyny wirtualne są równoważone w domenach błędów i domenach uaktualniania.

**PYTANIE** Czy mogę wymusić sekwencję wykonywania w zestawie skalowania maszyn wirtualnych w przypadku korzystania z wielu rozszerzeń?

**ODPOWIEDŹ** Nie można tego zrobić bezpośrednio, ale w przypadku rozszerzenia customScript skrypt może poczekać na ukończenie działania innego rozszerzenia ([na przykład przez monitorowanie dziennika rozszerzeń](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)). Dodatkowe wytyczne dotyczące sekwencjonowania rozszerzeń można znaleźć w tym wpisie w blogu: [Extension Sequencing in Azure VM Scale Sets](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/) (Sekwencjonowanie rozszerzeń w zestawach skalowania maszyn wirtualnych platformy Azure).

**PYTANIE** Czy zestawy skalowania maszyn wirtualnych współdziałają z zestawami dostępności platformy Azure?

**ODPOWIEDŹ** Tak. Zestaw skalowania maszyn wirtualnych to niejawny zestaw dostępności z 5 domenami błędów i 5 domenami uaktualniania. W obszarze virtualMachineProfile nie trzeba nic konfigurować. Zestawy skalowania maszyn wirtualnych składające się z ponad 100 maszyn wirtualnych obejmują wiele „grup umieszczania”, które są równoważne wielu zestawom dostępności. Zestaw dostępności maszyn wirtualnych może znajdować się w tej samej sieci wirtualnej co zestaw skalowania maszyn wirtualnych. Typowa konfiguracja polega na umieszczeniu maszyn wirtualnych węzła kontrolnego (często wymagających unikatowej konfiguracji) w zestawie dostępności, a węzłów danych w zestawie skalowania.



<!--HONumber=Feb17_HO1-->


