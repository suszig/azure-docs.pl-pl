---
title: "Omówienie zestawów skalowania maszyn wirtualnych platformy Azure | Microsoft Docs"
description: "Dowiedz się więcej o zestawach skalowania maszyn wirtualnych platformy Azure"
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
ms.date: 03/10/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 2071debe24ef2705b2ee05ce1210a813234cf277
ms.lasthandoff: 03/10/2017


---
# <a name="what-are-virtual-machine-scale-sets-in-azure"></a>Co to są zestawy skalowania maszyn wirtualnych na platformie Azure?
Zestawy skalowania maszyn wirtualnych to zasób dostępny w ramach usługi Azure Compute, który umożliwia wdrożenie zestawu identycznych maszyn wirtualnych oraz zarządzanie nim. Wszystkie maszyny wirtualne mają jednakową konfigurację, dzięki czemu zestawy skalowania umożliwiają rzeczywiste skalowanie automatyczne (bez wstępnej aprowizacji maszyn wirtualnych). To umożliwia łatwiejsze tworzenie wielkoskalowych usług z myślą o intensywnych operacjach obliczeniowych oraz obsłudze danych big data i obciążeń konteneryzowanych.

W przypadku aplikacji skalujących zasoby komputerowe w poziomie operacje skalowania są niejawnie równoważone w domenach błędów i aktualizacji. Aby zapoznać się z zagadnieniami dotyczącymi zestawów skalowania, zobacz [Ogłoszenia na blogu platformy Azure](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Aby dowiedzieć się więcej o zestawach skalowania, obejrzyj te klipy wideo:

* [Mark Russinovich omawia zestawy skalowania na platformie Azure](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Zestawy skalowania maszyn wirtualnych według Guya Bowermana](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-scale-sets"></a>Tworzenie zestawów skalowania i zarządzanie nimi
Zestaw skalowania można utworzyć w witrynie [Azure Portal](https://portal.azure.com), wybierając pozycję *nowy* i wpisując na pasku wyszukiwania hasło „skalowania”. Na liście wyników wyszukiwania pojawi się hasło „zestaw skalowania maszyn wirtualnych”. Z tego miejsca można wypełnić wymagane pola w celu dostosowania i wdrożenia zestawu skalowania. Warto zauważyć, że w portalu dostępne są również opcje umożliwiające konfigurację podstawowych reguł automatycznego skalowania na podstawie użycia procesora CPU.

Zestawy skalowania można także definiować i wdrażać za pomocą szablonów JSON oraz [interfejsów API REST](https://msdn.microsoft.com/library/mt589023.aspx) — tak jak poszczególne maszyny wirtualne w ramach usługi Azure Resource Manager. Można zatem użyć dowolnej standardowej metody wdrażania za pomocą usługi Azure Resource Manager. Aby uzyskać więcej informacji na temat szablonów, zobacz [Tworzenie szablonów usługi Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

Zestaw przykładowych szablonów dla zestawów skalowania znajduje się w repozytorium szablonów z pakietu Azure Quickstart w witrynie GitHub [tutaj](https://github.com/Azure/azure-quickstart-templates) (poszukaj szablonów z ciągiem *vmss* w nazwie).

Na stronach szczegółów tych szablonów jest dostępny przycisk, który prowadzi do funkcji wdrażania w portalu. Aby wdrożyć zestaw skalowania, kliknij ten przycisk, a następnie wypełnij wymagane parametry w portalu. Jeśli nie wiesz, czy dany zasób obsługuje wielkie lub małe i wielkie litery, lepiej użyć wartości parametrów pisanych małymi literami i cyframi. Dostępna jest również przydatna analiza wideo szablonu zestawu skalowania:

[Analiza szablonu zestawu skalowania maszyn wirtualnych](https://channel9.msdn.com/Blogs/Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-scale-set-out-and-in"></a>Skalowanie zestawu skalowania do wewnątrz i na zewnątrz
Pojemność zestawu skalowania można zmienić w witrynie Azure Portal, klikając sekcję _Skalowanie_ w obszarze _Ustawienia_. 

[Interfejs wiersza polecenia platformy Azure](https://github.com/Azure/azure-cli) umożliwia zmianę pojemności zestawu skalowania w wierszu polecenia za pomocą polecenia _scale_. Aby na przykład ustawić zestaw skalowania na pojemność 10 maszyn wirtualnych, użyj polecenia:

```bash
az vmss scale -g resourcegroupname -n scalesetname --new-capacity 10 
```

Aby ustawić liczbę maszyn wirtualnych w zestawie skalowania za pomocą programu PowerShell, użyj polecenia _Update-AzureRmVmss_:

```PowerShell
$vmss = Get-AzureRmVmss -ResourceGroupName resourcegroupname -VMScaleSetName scalesetname  
$vmss.Sku.Capacity = 10
Update-AzureRmVmss -ResourceGroupName resourcegroupname -Name scalesetname -VirtualMachineScaleSet $vmss
```

Aby zwiększyć lub zmniejszyć liczbę maszyn wirtualnych w zestawie skalowania przy użyciu szablonu usługi Azure Resource Manager, zmień właściwość *capacity* i ponownie wdróż szablon. Ta prosta metoda umożliwia zintegrowanie zestawów skalowania z funkcją automatycznego skalowania na platformie Azure lub utworzenie własnej niestandardowej warstwy skalowania w celu zdefiniowania niestandardowych zdarzeń skalowania, które nie są obsługiwane przez automatyczne skalowanie na platformie Azure. 

W przypadku ponownego wdrażania szablonu usługi Azure Resource Manager w celu zmiany pojemności można zdefiniować dużo mniejszy szablon obejmujący jedynie pakiet właściwości „SKU” ze zaktualizowaną pojemnością. Przykład można znaleźć [tutaj](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing).

## <a name="autoscale"></a>Automatyczne skalowanie

Jeśli zestaw skalowania utworzono w witrynie Azure Portal, można go opcjonalnie skonfigurować przy użyciu ustawień automatycznego skalowania, co umożliwi zwiększanie lub zmniejszanie liczby maszyn wirtualnych na podstawie średniego użycia procesora CPU. Wiele szablonów zestawów skalowania w repozytorium [Szablony szybkiego startu platformy Azure](https://github.com/Azure/azure-quickstart-templates) definiuje ustawienia automatycznego skalowania. Ustawienia automatycznego skalowania można także dodać do istniejącego zestawu skalowania. Oto przykładowy skrypt programu Azure PowerShell dodający automatyczne skalowanie na podstawie użycia procesora CPU do zestawu skalowania:

```PowerShell

$subid = "yoursubscriptionid"
$rgname = "yourresourcegroup"
$vmssname = "yourscalesetname"
$location = "yourlocation" # e.g. southcentralus

$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Increase -ScaleActionValue 1
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator LessThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Decrease -ScaleActionValue 1
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "autoprofile1"
Add-AzureRmAutoscaleSetting -Location $location -Name "autosetting1" -ResourceGroup $rgname -TargetResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -AutoscaleProfiles $profile1
```

 Listę prawidłowych metryk skalowania można znaleźć tutaj: [Metryki obsługiwane z usługą Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md) pod nagłówkiem _Microsoft.Compute/virtualMachineScaleSets_. Dostępne są również bardziej zaawansowane opcje automatycznego skalowania, w tym automatyczne skalowanie na podstawie harmonogramu oraz integracja z systemami alertów przy użyciu elementów webhook.

## <a name="monitoring-your-scale-set"></a>Monitorowanie zestawu skalowania
W witrynie [Azure Portal](https://portal.azure.com) znajduje się lista zestawów skalowania oraz ich właściwości. Witryna obsługuje również operacje zarządzania, które można wykonywać zarówno na zestawach skalowania, jak i na poszczególnych maszynach wirtualnych w zestawie skalowania. Witryna zawiera także dostosowywalny wykres użycia zasobów. Jeśli chcesz wyświetlić lub edytować podstawową definicję JSON zasobu platformy Azure, możesz także użyć [Eksploratora zasobów Azure](https://resources.azure.com). Zestawy skalowania są zasobami dostępnymi w ramach dostawcy zasobów Microsoft.Compute platformy Azure. Z poziomu tej witryny można je wyświetlić, rozwijając następujące linki:

**Subskrypcje -> Twoja subskrypcja -> resourceGroups -> dostawcy -> Microsoft.Compute -> virtualMachineScaleSets -> zestaw skalowania -> itd.**

## <a name="scale-set-scenarios"></a>Scenariusze dotyczące zestawów skalowania
W tej sekcji przedstawiono niektóre typowe scenariusze dotyczące zestawów skalowania. Scenariusze te mają zastosowanie w przypadku niektórych usług platformy Azure wyższego poziomu (np. Batch, Service Fabric, Azure Container Service).

* **Połączenia RDP/SSH z wystąpieniami zestawów skalowania** — zestaw skalowania jest tworzony w sieci wirtualnej, a poszczególnym maszynom wirtualnym w zestawie skalowania nie są przydzielane publiczne adresy IP. Dzięki tej zasadzie unika się wydatków i nakładów pracy związanych z przydzielaniem osobnych publicznych adresów IP wszystkim węzłom w sieci obliczeniowej. Połączenie z tymi maszynami wirtualnymi możesz nawiązać z innych zasobów w sieci wirtualnej, na przykład modułów równoważenia obciążenia i autonomicznych maszyn wirtualnych, którym można przydzielić publiczne adresy IP.
* **Nawiązywanie połączenia z maszynami wirtualnymi za pomocą reguł NAT** — można utworzyć publiczny adres IP, przypisać go do modułu równoważenia obciążenia i zdefiniować pulę NAT dla ruchu przychodzącego mapującą porty pod adresem IP na port maszyny wirtualnej w zestawie skalowania. Na przykład:
  
  | Element źródłowy | Port źródłowy | Element docelowy | Port docelowy |
  | --- | --- | --- | --- |
  |  Publiczny adres IP |Port 50000 |vmss\_0 |Port 22 |
  |  Publiczny adres IP |Port 50001 |vmss\_1 |Port 22 |
  |  Publiczny adres IP |Port 50002 |vmss\_2 |Port 22 |
  
   W tym przykładzie reguły NAT zdefiniowano w celu udostępnienia połączenia SSH każdej maszynie wirtualnej w zestawie skalowania przy użyciu jednego publicznego adresu IP: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)
  
   Tu natomiast pokazano, jak wykonać powyższe działania przy użyciu połączenia RDP i systemu Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)
* **Łączenie się z maszynami wirtualnymi za pomocą „rampy”** — jeśli utworzysz zestaw skalowania i autonomiczną maszynę wirtualną w tej samej sieci wirtualnej, autonomiczna maszyna wirtualna i maszyny wirtualne z zestawu skalowania mogą się łączyć ze sobą za pomocą wewnętrznych adresów IP zgodnie z definicją w sieci wirtualnej/podsieci. W przypadku utworzenia publicznego adresu IP i przypisania go do autonomicznej maszyny wirtualnej można nawiązać połączenie RDP lub SSH z autonomiczną maszyną wirtualną, a następnie z jej poziomu połączyć się z wystąpieniami danego zestawu skalowania. Można tutaj zauważyć, że prosty zestaw skalowania jest z natury bezpieczniejszy niż prosta autonomiczna maszyna wirtualna z publicznym adresem IP w konfiguracji domyślnej.
  
   Na przykład ten szablon wdraża prosty zestaw skalowania z autonomiczną maszyną wirtualną: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox)
* **Równoważenie obciążenia wystąpień zestawów skalowania** — aby przekazywać zadania do klastra obliczeniowego maszyn wirtualnych w trybie okrężnym, można odpowiednio skonfigurować moduł równoważenia obciążenia platformy Azure za pomocą reguł równoważenia obciążenia w warstwie&4;. Definiując sondy do weryfikowania działania aplikacji, można wysyłać polecenia ping do portów przy użyciu określonego protokołu, interwału i ścieżki żądania. Usługa Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/) obsługuje również zestawy skalowania, a także warstwę&7; oraz bardziej zaawansowane scenariusze równoważenia obciążenia.
  
   Oto przykład tworzenia zestawu skalowania z uruchomionymi serwerami sieci Web Apache, który używa modułu równoważenia obciążenia do zrównoważenia obciążenia każdej maszyny wirtualnej: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl) (zapoznaj się z typem zasobu Microsoft.Network/loadBalancers oraz profilem networkProfile i extensionProfile w zestawie virtualMachineScaleSet)

   W tym przykładzie użyto usługi Application Gateway. Linux:  [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway). Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway) 

* **Wdrażanie zestawu skalowania jako klastra obliczeniowego w menedżerze klastra w środowisku PaaS** — zestawy skalowania są czasami określane jako rola procesu roboczego następnej generacji. Mimo iż jest to trafny opis, wiąże się z nim ryzyko pomylenia funkcji zestawu skalowania z funkcjami usług Azure Cloud Services. Zestawy skalowania pełnią rolę procesu roboczego lub stanowią zasób procesu roboczego w tym sensie, że są ogólnym zasobem obliczeniowym, który jest niezależny od platformy/środowiska uruchomieniowego oraz możliwy do dostosowania i zintegrowania z infrastrukturą IaaS usługi Azure Resource Manager.
  
   Rola procesu roboczego usług Cloud Services ma wprawdzie ograniczenia w zakresie obsługi platform/środowiska uruchomieniowego (obsługuje wyłącznie obrazy platformy Windows), ale obejmuje usługi, takie jak wymiana wirtualnego adresu IP, konfigurowane ustawienia uaktualniania i określone ustawienia wdrażania środowiska uruchomieniowego/aplikacji, które nie są *jeszcze* dostępne w zestawach skalowania lub zostaną zapewnione przez inne usługi PaaS wyższego poziomu, takie jak Service Fabric. Zestawy skalowania można rozumieć jako infrastrukturę obsługującą rozwiązanie PaaS. Rozwiązania PaaS takie jak [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) są oparte na tej infrastrukturze.
  
   Przykładem takiego podejścia jest wdrożenie klastra usługi [Azure Container Service](https://azure.microsoft.com/services/container-service/) opartego na zestawach skalowania z koordynatorem kontenera: [https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos).

## <a name="scale-set-performance-and-scale-guidance"></a>Wytyczne dotyczące wydajności i skalowania zestawów skalowania
* Zestawy skalowania obsługują maksymalnie 1000 maszyn wirtualnych w zestawie skalowania. W przypadku utworzenia i przekazania własnych niestandardowych obrazów maszyn wirtualnych limit wynosi 100. Uwagi dotyczące korzystania z dużych zestawów skalowania znajdziesz w temacie [Praca z dużymi zestawami skalowania maszyn wirtualnych](virtual-machine-scale-sets-placement-groups.md).
* Aby używać zestawów skalowania, nie trzeba wstępnie tworzyć kont magazynu platformy Azure. Zestawy skalowania obsługują usługę Azure Managed Disks, dzięki czemu nie trzeba martwić się o problemy z wydajnością dotyczące liczby dysków na konto magazynu. Aby uzyskać więcej informacji, zobacz [Zestawy skalowania maszyn wirtualnych i dyski zarządzane](virtual-machine-scale-sets-managed-disks.md).
* Aby uzyskać krótsze i bardziej przewidywalne czasy aprowizacji maszyny wirtualnej oraz lepszą wydajność operacji we/wy, rozważ użycie magazynu Premium Storage platformy Azure zamiast magazynu w warstwie Standardowa.
* Liczba maszyn wirtualnych, które można utworzyć, jest ograniczona przez przydział rdzeni w regionie wdrażania. Nawet jeśli obecnie masz wysoki limit rdzeni na użytek usług w chmurze platformy Azure, może być konieczne skontaktowanie się z działem pomocy technicznej w celu zwiększenia limitu przydziału obliczeniowego. Aby wysłać zapytanie dotyczące limitu przydziału, uruchom następujące polecenie w interfejsie wiersza polecenia platformy Azure: `azure vm list-usage` lub następujące polecenie programu PowerShell: `Get-AzureRmVMUsage` (w przypadku programu PowerShell w wersji starszej niż 1.0 użyj polecenia `Get-AzureVMUsage`).

## <a name="scale-set-frequently-asked-questions"></a>Często zadawane pytania dotyczące zestawu skalowania
**PYTANIE** Ile maszyn wirtualnych może się znajdować w zestawie skalowania?

**ODPOWIEDŹ** Zestaw skalowania może zawierać od 0 do 1000 maszyn wirtualnych opartych na obrazach platformy lub od 0 do 100 maszyn wirtualnych opartych na obrazach niestandardowych. 

**PYTANIE** Czy zestawy skalowania obsługują dyski danych?

**ODPOWIEDŹ** Tak. Zestaw skalowania może definiować konfigurację dołączonych dysków danych, która jest stosowana do wszystkich maszyn wirtualnych w zestawie. Aby uzyskać więcej informacji, zobacz (Dołączone dyski danych i zestawy skalowania na platformie Azure)[virtual-machine-scale-sets-attached-disks.md]. Oto przykłady innych opcji magazynowania danych:

* Pliki platformy Azure (dyski udostępnione za pośrednictwem protokołu SMB)
* Dysk systemu operacyjnego
* Dysk tymczasowy (lokalny, bez kopii zapasowej w usłudze Azure Storage)
* Usługa danych platformy Azure (na przykład tabele platformy Azure, obiekty blob platformy Azure)
* Zewnętrzne usługi danych (na przykład zdalna baza danych)

**PYTANIE** Które regiony świadczenia usługi Azure obsługują zestawy skalowania?

**ODPOWIEDŹ** Wszystkie regiony obsługują zestawy skalowania.

**PYTANIE** Jak utworzyć zestaw skalowania za pomocą obrazu niestandardowego?

**ODPOWIEDŹ** Utwórz dysk zarządzany na podstawie wirtualnego dysku twardego z obrazem niestandardowym i odwołaj się do niego w szablonie zestawu skalowania. Oto przykład: [https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os](https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os).

**PYTANIE** Jeśli zmniejszę pojemność zestawu skalowania z 20 do 15, które maszyny wirtualne zostaną usunięte?

**ODPOWIEDŹ** W celu zapewnienia maksymalnej dostępności maszyny wirtualne są usuwane z zestawu skalowania równomiernie w domenach błędów i domenach uaktualniania. Najpierw są usuwane maszyny wirtualne o największym identyfikatorze.

**PYTANIE** Co się stanie, jeśli następnie zwiększę pojemność z 15 do 18?

**ODPOWIEDŹ** Zwiększenie pojemności do 18 spowoduje utworzenie 3 nowych maszyn wirtualnych. Za każdym razem następuje zwiększenie identyfikatora wystąpienia maszyny wirtualnej względem poprzedniej najwyższej wartości (na przykład 20, 21, 22). Maszyny wirtualne są równoważone w domenach błędów i domenach uaktualniania.

**PYTANIE** Czy mogę wymusić sekwencję wykonywania w zestawie skalowania w przypadku korzystania z wielu rozszerzeń?

**ODPOWIEDŹ** Nie można tego zrobić bezpośrednio, ale w przypadku rozszerzenia customScript skrypt może poczekać na ukończenie działania innego rozszerzenia ([na przykład przez monitorowanie dziennika rozszerzeń](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)). Dodatkowe wytyczne dotyczące sekwencjonowania rozszerzeń można znaleźć w tym wpisie w blogu: [Extension Sequencing in Azure VM Scale Sets](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/) (Sekwencjonowanie rozszerzeń w zestawach skalowania maszyn wirtualnych platformy Azure).

**PYTANIE** Czy zestawy skalowania współdziałają z zestawami dostępności platformy Azure?

**ODPOWIEDŹ** Tak. Zestaw skalowania to niejawny zestaw dostępności z 5 domenami błędów i 5 domenami uaktualniania. Zestawy skalowania składające się z ponad 100 maszyn wirtualnych obejmują wiele „grup umieszczania”, które są równoważne wielu zestawom dostępności. Aby uzyskać więcej informacji na temat grup umieszczania, zobacz [Praca z dużymi zestawami skalowania maszyn wirtualnych](virtual-machine-scale-sets-placement-groups.md). Zestaw dostępności maszyn wirtualnych może znajdować się w tej samej sieci wirtualnej co zestaw skalowania maszyn wirtualnych. Typowa konfiguracja polega na umieszczeniu maszyn wirtualnych węzła kontrolnego (często wymagających unikatowej konfiguracji) w zestawie dostępności, a węzłów danych w zestawie skalowania.

Więcej często zadawanych pytań dotyczących zestawów skalowania można znaleźć w artykule [Zestawy skalowania maszyn wirtualnych platformy Azure — często zadawane pytania](virtual-machine-scale-sets-faq.md).

