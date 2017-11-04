---
title: "Samouczek zestawy dostępności dla maszyn wirtualnych systemu Linux na platformie Azure | Dokumentacja firmy Microsoft"
description: "Informacje na temat zestawów dostępności dla maszyn wirtualnych systemu Linux na platformie Azure."
documentationcenter: 
services: virtual-machines-linux
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: tutorial
ms.date: 10/05/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: e7780a29f6633b444608d96012fabe67b9b6d924
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-availability-sets"></a>Jak używać zestawów dostępności


W tym samouczku Dowiedz się jak zwiększyć dostępność i niezawodność rozwiązań maszyny wirtualnej na platformie Azure przy użyciu funkcji o nazwie zestawów dostępności. Zestawy dostępności upewnij się, czy maszyn wirtualnych, należy wdrożyć na platformie Azure są rozproszone na wielu klastrów izolowanego sprzętu. W ten sposób zapewnia, że jeśli awarii sprzętu lub oprogramowania w systemie Azure wykonywany, wpływ na podzbiorze maszyn wirtualnych i który rozwiązania ogólną pozostaje dostępny i działa.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie zestawu dostępności
> * Tworzenie maszyny Wirtualnej w zestawie dostępności
> * Sprawdź dostępne rozmiary maszyny Wirtualnej


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="availability-set-overview"></a>Omówienie zestawu dostępności

Zestawu dostępności jest możliwość grupę logiczną, która na platformie Azure umożliwia Sprawdź, czy zasobów maszyny Wirtualnej, który można umieścić w nim są odizolowane od siebie, gdy są wdrożone w centrum danych Azure. Azure zapewnia, że maszyny wirtualne, umieść w ramach zestawu dostępności uruchamiania na wielu serwerach fizycznych, obliczeniowe stojakami jednostek magazynowych i przełączniki sieciowe. W przypadku sprzętu lub oprogramowania Azure awarii, ma wpływ tylko podzestaw maszyn wirtualnych, a ogólną aplikacji utrzyma się i nadal będzie dostępna dla klientów. Zestawy dostępności są podstawowych możliwości do tworzenia rozwiązań chmur wiarygodne.

Zastanówmy typowe rozwiązania oparte na Maszynach którym może mieć 4 serwery frontonu sieci web i korzystać z 2 maszyny wirtualne zaplecza, które hostują bazy danych. Przy użyciu platformy Azure, należy zdefiniować dwa zestawy dostępności, przed wdrożeniem maszyn wirtualnych: jeden zbiór dostępności dla warstwy "web" i jeden zbiór dostępności dla warstwy "baza danych". Podczas tworzenia nowej maszyny Wirtualnej można określić polecenia Utwórz zestaw jako parametr do maszyny wirtualnej az dostępności i Azure automatycznie upewnia się, że maszyny wirtualne utworzone w zestawie dostępne są izolowane między wieloma zasobami sprzętu fizycznego. Jeśli sprzęt fizyczny wykorzystywany do jednego serwera sieci Web lub maszyn wirtualnych z serwera bazy danych jest uruchomiona na ma problem, wiadomo, że inne wystąpienia serwera sieci Web i bazy danych maszyn wirtualnych będą nadal działać, ponieważ są one na inny komputer.

Użyj zbiorów dostępności Jeśli chcesz wdrożyć niezawodne rozwiązania na podstawie maszyny Wirtualnej w systemie Azure.


## <a name="create-an-availability-set"></a>Tworzenie zestawu dostępności

Możesz utworzyć zestaw przy użyciu danych o dostępności [tworzenia maszyny wirtualnej az zestawu dostępności](/cli/azure/vm/availability-set#create). W tym przykładzie umieszczania liczba domen aktualizacji i odporność na *2* dla zestawu o nazwie dostępności *myAvailabilitySet* w *myResourceGroupAvailability* grupy zasobów.

Utwórz grupę zasobów.

```azurecli-interactive 
az group create --name myResourceGroupAvailability --location eastus
```


```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

Zestawy dostępności pozwalają w celu odizolowania zasobów w domenach awarii i Aktualizacja domeny. A **domeny błędów** reprezentuje kolekcję izolowanego server + sieć i Magazyn zasobów. W powyższym przykładzie mamy wskazać, że firma Microsoft ma naszych dostępności ustawioną być rozproszone na co najmniej dwóch domen błędów, gdy naszych maszyny wirtualne są wdrażane. Również wskazywać czy chcemy naszego zestawu rozproszonych w dwóch dostępności **zaktualizować domen**.  Dwie domeny aktualizacji Sprawdź, czy w gdy platforma Azure stosuje aktualizacje oprogramowania naszych zasobów maszyny Wirtualnej są izolowane, co uniemożliwia całe oprogramowanie uruchomione poniżej naszych maszyny Wirtualnej z aktualizacji w tym samym czasie.


## <a name="create-vms-inside-an-availability-set"></a>Tworzenie maszyn wirtualnych w zestawie dostępności

Maszyny wirtualne muszą być tworzone dostępność ustawioną upewnij się, że są one poprawnie dystrybuowana sprzętu. Nie można dodać istniejącej maszyny Wirtualnej do dostępności po jego utworzeniu. 

Po utworzeniu maszyny Wirtualnej przy użyciu [tworzenia maszyny wirtualnej az](/cli/azure/vm#create) określ zestaw, za pomocą dostępności `--availability-set` parametr, aby określić nazwę zestawu dostępności.

```azurecli-interactive 
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --size Standard_DS1_v2  \
     --image Canonical:UbuntuServer:14.04.4-LTS:latest \
     --admin-username azureuser \
     --generate-ssh-keys \
     --no-wait
done 
```

Mamy teraz dwie maszyny wirtualne w naszym zestawie dostępności nowo utworzony. Ponieważ są one w tym samym zestawie dostępności, Azure zapewnia, że maszyn wirtualnych i ich zasobów (w tym dysków z danymi) są rozmieszczane izolowanego sprzętu fizycznego. Tej dystrybucji zapewnia znacznie wyższą dostępność naszych ogólnego rozwiązania maszyny Wirtualnej.

Jeśli przyjrzymy się zestawem dostępności w portalu, przechodząc do grupy zasobów > myResourceGroupAvailability > myAvailabilitySet, należy się w temacie jak maszyny wirtualne rozproszone na 2 błędów i aktualizować domen.

![Zestawem dostępności w portalu](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Sprawdź, czy dostępne rozmiary maszyny Wirtualnej 

Możesz dodać więcej maszyn wirtualnych do później zestaw dostępności, ale musisz wiedzieć, jaki rozmiarów maszyn wirtualnych są dostępne na sprzęcie.  Użyj [az listy rozmiarów maszyn wirtualnych zestawu dostępności](/cli/azure/availability-set#list-sizes) Aby wyświetlić listę wszystkich dostępnych rozmiarów na sprzęcie klastra dla zestawu dostępności.

```azurecli-interactive 
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table  
```

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie zestawu dostępności
> * Tworzenie maszyny Wirtualnej w zestawie dostępności
> * Sprawdź dostępne rozmiary maszyny Wirtualnej

Przejdź do następnego samouczkiem, aby poznać zestawy skalowania maszyny wirtualnej.

> [!div class="nextstepaction"]
> [Tworzenie zestawu skalowania maszyn wirtualnych](tutorial-create-vmss.md)

