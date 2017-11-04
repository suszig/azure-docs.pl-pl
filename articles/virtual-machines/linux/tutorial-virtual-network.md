---
title: Sieci wirtualnych platformy Azure i maszyn wirtualnych systemu Linux | Dokumentacja firmy Microsoft
description: "Samouczek — Zarządzanie sieciami wirtualnymi platformy Azure i maszyn wirtualnych systemu Linux z interfejsu wiersza polecenia platformy Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: a49b4c2d4ddd6d686675cee53d46cd4dd6ad3811
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-the-azure-cli"></a>Zarządzanie sieciami wirtualnymi platformy Azure i maszyn wirtualnych systemu Linux z interfejsu wiersza polecenia platformy Azure

Maszyny wirtualne platformy Azure używania sieci platformy Azure do komunikacji sieciowej wewnętrznych i zewnętrznych. Ten samouczek przeprowadzi Cię przez wdrożenie dwóch maszyn wirtualnych i konfigurowanie sieci platformy Azure dla tych maszyn wirtualnych. Przykłady w tym samouczku założono, że maszyny wirtualne obsługujące aplikacji sieci web z bazy danych zaplecza, jednak aplikacja nie zostanie wdrożona w samouczku. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie sieci wirtualnej i podsieci
> * Tworzenie publicznego adresu IP
> * Tworzenie frontonu maszyny Wirtualnej
> * Zabezpieczanie ruchu sieciowego
> * Tworzenie maszyny Wirtualnej zaplecza

Podczas wykonywania tego samouczka, można wyświetlić te zasoby utworzone:

![Sieci wirtualnej z dwoma podsieciami](./media/tutorial-virtual-network/networktutorial.png)

- *myVNet* -sieć wirtualna, która maszyn wirtualnych używają do komunikowania się ze sobą i z Internetu.
- *myFrontendSubnet* -podsieci w *myVNet* używany przez zasoby frontonu.
- *myPublicIPAddress* -publiczny adres IP używany do dostępu *myFrontendVM* z Internetu.
- *myFrontentNic* -interfejs sieciowy używany przez *myFrontendVM* do komunikowania się z *myBackendVM*.
- *myFrontendVM* -maszyny Wirtualnej, używany do komunikacji między z Internetem i *myBackendVM*.
- *myBackendNSG* -grupy zabezpieczeń sieci, która kontroluje komunikację między *myFrontendVM* i *myBackendVM*.
- *myBackendSubnet* -podsieci skojarzone z *myBackendNSG* i używany przez zasoby zaplecza.
- *myBackendNic* -interfejs sieciowy używany przez *myBackendVM* do komunikowania się z *myFrontendVM*.
- *myBackendVM* -maszynę Wirtualną, która korzysta z portu 22 i 3306 do komunikowania się z *myFrontendVM*.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="vm-networking-overview"></a>Omówienie sieci maszyny Wirtualnej

Sieci wirtualnych platformy Azure Włącz bezpiecznych połączeń sieci między maszynami wirtualnymi, internet i innymi usługami Azure, takich jak bazy danych Azure SQL. Sieci wirtualne są podzielone na segmentach logicznej podsieci. Podsieci są używane do sterowania przepływem sieci oraz funkcję granicy zabezpieczeń. Podczas wdrażania maszyny Wirtualnej, zwykle obejmuje interfejs sieci wirtualnej, który jest podłączony do podsieci.

## <a name="create-a-virtual-network-and-subnet"></a>Tworzenie sieci wirtualnej i podsieci

W tym samouczku jednej sieci wirtualnej jest tworzony z dwoma podsieciami. Podsieci frontonu do obsługi aplikacji sieci web, a podsieć zaplecza dla hostingu serwera bazy danych.

Przed utworzeniem sieci wirtualnej, Utwórz nową grupę zasobów o [Tworzenie grupy az](/cli/azure/group#create). Poniższy przykład tworzy grupę zasobów o nazwie *myRGNetwork* w lokalizacji eastus.

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a>Tworzenie sieci wirtualnej

Użyj [tworzenie sieci wirtualnej sieci az](/cli/azure/network/vnet#create) polecenie, aby utworzyć sieć wirtualną. W tym przykładzie sieci o nazwie *mvVNet* i podano prefiksu adresu *10.0.0.0/16*. Tworzona jest również podsieć o nazwie *myFrontendSubnet* i prefiksem *10.0.1.0/24*. W dalszej części tego samouczka frontonu maszyny Wirtualnej jest podłączona do tej podsieci. 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVNet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myFrontendSubnet \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a>Utwórz podsieć

Nowa podsieć zostanie dodany do sieci wirtualnej przy użyciu [Utwórz podsieć sieci wirtualnej sieci az](/cli/azure/network/vnet/subnet#create) polecenia. W tym przykładzie podsieć o nazwie *myBackendSubnet* i podano prefiksu adresu *10.0.2.0/24*. Ta podsieć jest używany ze wszystkimi usługami zaplecza.

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVNet \
  --name myBackendSubnet \
  --address-prefix 10.0.2.0/24
```

W tym momencie sieci zostało utworzone i podzielone na dwie podsieci, jeden dla usługi frontonu i drugi dla usług zaplecza. W następnej sekcji maszyny wirtualne są tworzone i połączone z tych podsieci.

## <a name="create-a-public-ip-address"></a>Tworzenie publicznego adresu IP

Publiczny adres IP umożliwia zasobów platformy Azure jako dostępny w Internecie. Metoda alokacji publicznego adresu IP można skonfigurować jako dynamicznej lub statycznej. Domyślnie jest dynamicznie przydzielane publicznego adresu IP. Dynamiczne adresy IP są wydawane po cofnięciu przydziału maszyny Wirtualnej. Ten problem powoduje, że adres IP zmienić podczas żadnej operacji, która obejmuje dezalokacji maszyny Wirtualnej.

Metoda alokacji można ustawić jako statyczny, który zapewnia, że adres IP jest przypisana do maszyny Wirtualnej, nawet podczas deallocated stanu. Korzystając z statycznie przydzielony adres IP, nie można określić adres IP. Zamiast tego jest ona przydzielone z puli dostępnych adresów.

```azurecli-interactive
az network public-ip create --resource-group myRGNetwork --name myPublicIPAddress
```

Podczas tworzenia maszyny Wirtualnej z [tworzenia maszyny wirtualnej az](/cli/azure/vm#create) polecenia domyślnego publicznego adresu IP adres metodę alokacji jest dynamiczny. Podczas tworzenia maszyny wirtualnej przy użyciu [tworzenia maszyny wirtualnej az](/cli/azure/vm#create) polecenia, obejmują `--public-ip-address-allocation static` argument można przypisać statycznego publicznego adresu IP. Ta operacja nie jest prezentowana w tym samouczku, jednak w następnej sekcji dynamicznie przydzielonego adresu IP została zmieniona na statycznie przydzielonego adresu. 

### <a name="change-allocation-method"></a>Zmień metodę alokacji

Metoda alokacji adresów IP można zmienić za pomocą [az sieci ip publicznego aktualizacji](/cli/azure/network/public-ip#update) polecenia. W tym przykładzie metoda alokacji adresu IP frontonu maszyny wirtualnej zostanie zmieniona na statyczne.

Po pierwsze Cofnij Przydział maszyny Wirtualnej.

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontendVM
```

Użyj [az sieci ip publicznego aktualizacji](/cli/azure/network/public-ip#update) polecenie, aby zaktualizować metodę alokacji. W takim przypadku `--allocation-method` jest ustawiany *statycznych*.

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myPublicIPAddress --allocation-method static
```

Uruchom maszynę Wirtualną.

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontendVM --no-wait
```

### <a name="no-public-ip-address"></a>Nie publicznego adresu IP

Często maszyny Wirtualnej nie musi być dostępny za pośrednictwem Internetu. Aby utworzyć Maszynę wirtualną bez publicznego adresu IP, należy użyć `--public-ip-address ""` argumentu z pustego zestawu cudzysłowów. Ta konfiguracja jest przedstawiona w dalszej części tego samouczka.

## <a name="create-a-front-end-vm"></a>Tworzenie frontonu maszyny Wirtualnej

Użyj [tworzenia maszyny wirtualnej az](/cli/azure/vm#create) polecenie, aby utworzyć maszynę Wirtualną o nazwie *myFrontendVM* przy użyciu *myPublicIPAddress*.

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myFrontendVM \
  --vnet-name myVNet \
  --subnet myFrontendSubnet \
  --nsg myFrontendNSG \
  --public-ip-address myPublicIPAddress \
  --image UbuntuLTS \
  --generate-ssh-keys
```

## <a name="secure-network-traffic"></a>Zabezpieczanie ruchu sieciowego

Sieciowa grupa zabezpieczeń (NSG, network security group) zawiera listę reguł zabezpieczeń, które blokują lub zezwalają na ruch sieciowy do zasobów połączonych z usługami Azure Virtual Network (VNet). Grupy NSG można skojarzyć z podsieci lub sieci poszczególnych interfejsów. Grupa NSG jest skojarzona z karty sieciowej, ma zastosowanie tylko skojarzonego VM. Jeśli sieciowa grupa zabezpieczeń jest skojarzona z podsiecią, te reguły są stosowane do wszystkich zasobów połączonych z tą podsiecią. 

### <a name="network-security-group-rules"></a>Reguły grupy zabezpieczeń sieci

Reguły NSG definiują portów sieciowych za pośrednictwem których ruch jest dozwolony lub niedozwolony. Zasady mogą obejmować źródłowe i docelowe zakresów adresów IP tak, aby ruch jest kontrolowany między konkretnych systemów lub podsieci. Reguły NSG obejmują także priorytet (od 1 — i 4096). Reguły są sprawdzane według ważności. Reguła o priorytecie 100 jest oceniane przed regułą z priorytetem 200.

Wszystkie sieciowe grupy zabezpieczeń zawierają zestaw reguł domyślnych. Reguł domyślnych nie można usunąć, ale ponieważ mają przypisany najniższy priorytet, mogą być zastąpione przez tworzone zasady.

- **Sieć wirtualna** — ruch pochodzący i kończenie w sieci wirtualnej jest dozwolony zarówno w kierunkach przychodzących i wychodzących.
- **Internet** — ruch wychodzący jest dozwolone, ale ruch przychodzący jest zablokowany.
- **Moduł równoważenia obciążenia** — umożliwia modułowi równoważenia obciążenia Azure badanie kondycji maszyn wirtualnych i wystąpień ról. Tę zasadę można zastąpić, jeśli nie używasz zestawu o zrównoważonym obciążeniu.

### <a name="create-network-security-groups"></a>Utwórz grupy zabezpieczeń sieci

Można utworzyć grupy zabezpieczeń sieci w tym samym czasie jako maszynę Wirtualną przy użyciu [tworzenia maszyny wirtualnej az](/cli/azure/vm#create) polecenia. Gdy tak się grupa NSG jest skojarzona z interfejsu sieciowego maszyn wirtualnych i reguły NSG jest automatycznie utworzona zezwalająca na ruch na porcie *22* z dowolnego źródła. Wcześniej w tym samouczku frontonu NSG został automatycznie utworzony z maszyny Wirtualnej frontonu. Reguły NSG również został automatycznie utworzony dla portu 22. 

W niektórych przypadkach może być przydatne do wstępnego tworzenia grupy NSG, takie jak kiedy nie należy tworzyć reguły domyślne SSH, lub gdy musi być podłączona grupa NSG do podsieci. 

Użyj [utworzyć nsg sieci az](/cli/azure/network/nsg#create) polecenie, aby utworzyć grupę zabezpieczeń sieci.

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myBackendNSG
```

Kojarzenie grupy NSG do interfejsu sieciowego, a nie jest skojarzony z podsiecią. W tej konfiguracji żadnej maszyny Wirtualnej, który jest podłączony do podsieci dziedziczy reguły NSG.

Aktualizuj istniejącą podsieć o nazwie *myBackendSubnet* z Nowa grupa NSG.

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVNet \
  --name myBackendSubnet \
  --network-security-group myBackendNSG
```

### <a name="secure-incoming-traffic"></a>Bezpieczny ruch przychodzący

Podczas tworzenia maszyny Wirtualnej frontonu, reguły NSG utworzono zezwalająca na ruch przychodzący na porcie 22. Ta zasada umożliwia połączeń SSH z maszyną wirtualną. Na przykład ruch powinno być dozwolone na porcie *80*. Ta konfiguracja pozwala na maszynie Wirtualnej dostęp do aplikacji sieci web.

Użyj [Tworzenie reguły nsg sieci az](/cli/azure/network/nsg/rule#create) polecenie, aby utworzyć regułę dla portu *80*.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myFrontendNSG \
  --name http \
  --access allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 80
```

Frontonu maszyny Wirtualnej jest dostępny tylko na porcie *22* i port *80*. Cały ruch przychodzący jest zablokowany na grupę zabezpieczeń sieci. Może to być przydatne do wizualizacji konfiguracjach reguły NSG. Zwraca konfigurację reguły NSG z [listy reguł sieciowej az](/cli/azure/network/nsg/rule#list) polecenia. 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myFrontendNSG --output table
```

### <a name="secure-vm-to-vm-traffic"></a>Bezpiecznej maszyny Wirtualnej do ruchu maszyny Wirtualnej

Reguły grupy zabezpieczeń sieci można także zastosować między maszynami wirtualnymi. Na przykład frontonu maszyny Wirtualnej musi łączyć się z zaplecza maszyny Wirtualnej na porcie *22* i *3306*. Ta konfiguracja umożliwia połączeń SSH z frontonu maszyny Wirtualnej, a także zezwolić aplikacji na Maszynie wirtualnej frontonu do komunikowania się z wewnętrznej bazy danych MySQL. Powinien zostać zablokowany cały ruch między maszynami wirtualnymi frontonu i zaplecza.

Użyj [Tworzenie reguły nsg sieci az](/cli/azure/network/nsg/rule#create) polecenie, aby utworzyć regułę dla portu 22. Zwróć uwagę, że `--source-address-prefix` argument określa wartość *10.0.1.0/24*. Taka konfiguracja powoduje, że za pośrednictwem grupa NSG jest dozwolone tylko na ruch z podsieci frontonu.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name SSH \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "22"
```

Teraz Dodaj regułę dla ruchu danych MySQL na porcie 3306.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name MySQL \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "3306"
```

Ponadto ponieważ grup NSG domyślną regułę zezwalającą na cały ruch między maszynami wirtualnymi w tej samej sieci wirtualnej, regułę można utworzyć dla grup NSG zaplecza zablokować cały ruch. Zauważ, że `--priority` znajduje się wartość *300*, która jest niższy tej reguły NSG i MySQL. Taka konfiguracja powoduje, że ruch SSH i MySQL nadal uzyskuje zezwolenie za pośrednictwem grupy NSG.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name denyAll \
  --access Deny \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "*"
```

## <a name="create-back-end-vm"></a>Tworzenie maszyny Wirtualnej z zaplecza

Teraz Utwórz maszynę wirtualną, która jest dołączona do *myBackendSubnet*. Zwróć uwagę, że `--nsg` argument ma wartość pustą cudzysłowów. Grupy NSG nie muszą zostać utworzone z maszyną Wirtualną. Maszyna wirtualna jest podłączony do podsieci wewnętrznej, które są chronione za pomocą wstępnie utworzone NSG zaplecza. Ta grupa NSG stosuje się do maszyny Wirtualnej. Ponadto Zauważ, że `--public-ip-address` argument ma wartość pustą cudzysłowów. Ta konfiguracja tworzy Maszynę wirtualną bez publicznego adresu IP. 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myBackendVM \
  --vnet-name myVNet \
  --subnet myBackendSubnet \
  --public-ip-address "" \
  --nsg "" \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Maszyna wirtualna zaplecza jest dostępny tylko na porcie *22* i port *3306* z podsieci frontonu. Cały ruch przychodzący jest zablokowany na grupę zabezpieczeń sieci. Może to być przydatne do wizualizacji konfiguracjach reguły NSG. Zwraca konfigurację reguły NSG z [listy reguł sieciowej az](/cli/azure/network/nsg/rule#list) polecenia. 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myBackendNSG --output table
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostało utworzone i zabezpieczonej sieci platformy Azure w odniesieniu do maszyn wirtualnych. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie sieci wirtualnej i podsieci
> * Tworzenie publicznego adresu IP
> * Tworzenie frontonu maszyny Wirtualnej
> * Zabezpieczanie ruchu sieciowego
> * Tworzenie maszyny Wirtualnej z zaplecza

Przejdź do następnego samouczka, aby dowiedzieć się więcej na temat zabezpieczania danych w przypadku maszyn wirtualnych przy użyciu kopii zapasowej systemu Azure. 

> [!div class="nextstepaction"]
> [Tworzenie kopii zapasowych maszyn wirtualnych systemu Linux na platformie Azure](./tutorial-backup-vms.md)
