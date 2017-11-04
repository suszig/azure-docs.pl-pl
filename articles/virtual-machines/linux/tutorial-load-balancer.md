---
title: "Jak załadować saldo maszyn wirtualnych systemu Linux na platformie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać usługi równoważenia obciążenia Azure do tworzenia aplikacji wysokiej dostępności i bezpieczne w trzech maszyn wirtualnych systemu Linux"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 25e2538e220327a078a6527e667dfcd6cb838b1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-load-balance-linux-virtual-machines-in-azure-to-create-a-highly-available-application"></a>Jak załadować saldo maszyn wirtualnych systemu Linux na platformie Azure, aby utworzyć aplikację wysokiej dostępności
Równoważenie obciążenia sieciowego zapewnia wyższy poziom dostępności dzięki rozproszeniu przychodzące żądania między wieloma maszynami wirtualnymi. W tym samouczku opisano różne składniki usługi równoważenia obciążenia Azure dystrybucji ruchu, które zapewniają wysoką dostępność. Omawiane kwestie:

> [!div class="checklist"]
> * Tworzenie usługi równoważenia obciążenia Azure
> * Utwórz kondycji sondę modułu równoważenia obciążenia
> * Tworzenie reguły ruchu usługi równoważenia obciążenia
> * Umożliwia utworzenie podstawowej aplikacji Node.js init chmury
> * Tworzenie maszyn wirtualnych i dołączanie do usługi równoważenia obciążenia
> * Wyświetl modułu równoważenia obciążenia w akcji
> * Dodawanie i usuwanie maszyny wirtualne z modułem równoważenia obciążenia


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="azure-load-balancer-overview"></a>Omówienie usługi równoważenia obciążenia Azure
Moduł równoważenia obciążenia Azure jest równoważenia obciążenia warstwy 4 (TCP, UDP), który zapewnia wysoką dostępność, przekazując przychodzący ruch między maszynami wirtualnymi w dobrej kondycji. Badanie kondycji modułu równoważenia obciążenia monitoruje danego portu na każdej maszynie Wirtualnej i tylko dystrybuuje ruch do operacyjnej maszyny Wirtualnej.

Należy zdefiniować frontonu konfiguracji adresu IP, który zawiera co najmniej jeden publiczny adres IP. Ta frontonu konfiguracji IP umożliwia Twojej usługi równoważenia obciążenia i aplikacje mają być dostępne za pośrednictwem Internetu. 

Maszyny wirtualne połączenia z modułem równoważenia obciążenia przy użyciu ich karty interfejsu sieci wirtualnej (NIC). Aby rozpowszechnić ruch do maszyn wirtualnych, puli adresów zaplecza zawiera adres IP adresów wirtualnych (NIC) połączony z usługą równoważenia obciążenia.

Aby sterowanie przepływem ruchu, należy zdefiniować reguły modułu równoważenia obciążenia dla określonych portów i protokołów, które mapują do maszyn wirtualnych.

Po wykonaniu poprzednich w samouczku [utworzyć zestaw skali maszyny wirtualnej](tutorial-create-vmss.md), usługi równoważenia obciążenia został utworzony. Wszystkie te składniki zostały skonfigurowane dla Ciebie jako część zestawu skali.


## <a name="create-azure-load-balancer"></a>Tworzenie usługi równoważenia obciążenia Azure
W tej sekcji Szczegóły, jak można tworzyć i konfigurować poszczególnych składników usługi równoważenia obciążenia. Przed utworzeniem przez moduł równoważenia obciążenia, Utwórz nową grupę zasobów o [Tworzenie grupy az](/cli/azure/group#create). Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroupLoadBalancer* w *eastus* lokalizacji:

```azurecli-interactive 
az group create --name myResourceGroupLoadBalancer --location eastus
```

### <a name="create-a-public-ip-address"></a>Tworzenie publicznego adresu IP
Aby uzyskać dostęp do aplikacji w Internecie, należy publicznego adresu IP usługi równoważenia obciążenia. Utwórz publiczny adres IP z [utworzyć az sieci publicznej ip](/cli/azure/network/public-ip#create). Poniższy przykład tworzy publiczny adres IP o nazwie *myPublicIP* w *myResourceGroupLoadBalancer* grupy zasobów:

```azurecli-interactive 
az network public-ip create \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP
```

### <a name="create-a-load-balancer"></a>Tworzenie modułu równoważenia obciążenia
Tworzenie modułu równoważenia obciążenia z [utworzyć równoważeniem obciążenia sieciowego az](/cli/azure/network/lb#create). Poniższy przykład tworzy moduł równoważenia obciążenia o nazwie *myLoadBalancer* i przypisuje *myPublicIP* adres frontonu konfiguracją protokołu IP:

```azurecli-interactive 
az network lb create \
    --resource-group myResourceGroupLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a>Utworzyć sondy kondycji
Aby zezwolić usłudze równoważenia obciążenia do monitorowania stanu aplikacji, należy użyć sondy kondycji. Sondy kondycji dynamicznie dodaje lub usuwa maszyn wirtualnych z oparte na ich odpowiedzi na sprawdzenie kondycji obrót usługi równoważenia obciążenia. Domyślnie maszyny Wirtualnej zostanie usunięte z dystrybucji modułu równoważenia obciążenia po dwóch kolejnych błędów na 15 sekund. Można utworzyć sondy kondycji, na podstawie protokołu lub stronę wyboru kondycji określonych aplikacji. 

Poniższy przykład tworzy sondowaniem TCP. Można również utworzyć niestandardowe sond HTTP więcej kontroli kondycji szczegółowe. Podczas korzystania z niestandardowego badanie HTTP, należy utworzyć strona sprawdzania kondycji, takich jak *healthcheck.js*. Sonda musi zwracać **HTTP 200 OK** odpowiedzi dla usługi równoważenia obciążenia zachować hosta w obrotu.

Aby utworzyć sondy kondycji TCP, należy użyć [utworzyć sondy równoważeniem obciążenia sieciowego az](/cli/azure/network/lb/probe#create). Poniższy przykład tworzy badanie kondycji o nazwie *myHealthProbe*:

```azurecli-interactive 
az network lb probe create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a>Tworzenie reguły modułu równoważenia obciążenia
Reguły modułu równoważenia obciążenia jest używany do definiowania rozkład ruchu do maszyn wirtualnych. Należy zdefiniować konfiguracji IP frontonu dla ruchu przychodzącego i puli adresów IP zaplecza, aby odbierać ruch, wraz z wymagany port źródłowy i docelowy. Aby upewnij się, że tylko dobrej kondycji maszyn wirtualnych odbieranie ruchu, również zdefiniować sondy kondycji do użycia.

Tworzenie reguły modułu równoważenia obciążenia z [utworzyć regułę równoważeniem obciążenia sieciowego az](/cli/azure/network/lb/rule#create). Poniższy przykład tworzy reguły o nazwie *myLoadBalancerRule*, używa *myHealthProbe* badania kondycji i równoważy ruch na porcie *80*:

```azurecli-interactive 
az network lb rule create \
    --resource-group myResourceGroupLoadBalancer \
    --lb-name myLoadBalancer \
    --name myLoadBalancerRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```


## <a name="configure-virtual-network"></a>Konfigurowanie sieci wirtualnej
Przed wdrożeniem niektórych maszyn wirtualnych i przetestować z usługi równoważenia, Utwórz pomocnicze zasoby sieci wirtualnej. Aby uzyskać więcej informacji o sieciach wirtualnych, zobacz [Zarządzanie sieciami wirtualnymi Azure](tutorial-virtual-network.md) samouczka.

### <a name="create-network-resources"></a>Utwórz zasoby sieciowe
Tworzenie sieci wirtualnej z [tworzenie sieci wirtualnej sieci az](/cli/azure/network/vnet#create). Poniższy przykład tworzy sieć wirtualną o nazwie *myVnet* z podsiecią o nazwie *mySubnet*:

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupLoadBalancer \
    --name myVnet \
    --subnet-name mySubnet
```

Aby dodać grupę zabezpieczeń sieci, należy użyć [utworzyć nsg sieci az](/cli/azure/network/nsg#create). Poniższy przykład tworzy sieciową grupę zabezpieczeń o nazwie *myNetworkSecurityGroup*:

```azurecli-interactive 
az network nsg create \
    --resource-group myResourceGroupLoadBalancer \
    --name myNetworkSecurityGroup
```

Tworzenie reguły grupy zabezpieczeń sieci z [Tworzenie reguły nsg sieci az](/cli/azure/network/nsg/rule#create). Poniższy przykład tworzy regułę grupy zabezpieczeń sieci o nazwie *myNetworkSecurityGroupRule*:

```azurecli-interactive 
az network nsg rule create \
    --resource-group myResourceGroupLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

Wirtualne karty sieciowe są tworzone za pomocą [tworzenie kart interfejsu sieciowego az](/cli/azure/network/nic#create). Poniższy przykład tworzy trzy wirtualne karty sieciowe. (Jedną wirtualną kartę Sieciową dla każdej maszyny Wirtualnej można utworzyć dla aplikacji w poniższych krokach). Można utworzyć dodatkowe wirtualne karty sieciowe i maszyn wirtualnych w dowolnym momencie i dodaj je do usługi równoważenia obciążenia:

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupLoadBalancer \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

### <a name="create-cloud-init-config"></a>Tworzenie konfiguracji init chmury
W poprzednich samouczek dotyczący [sposobu dostosowywania maszyny wirtualnej systemu Linux, po pierwszym uruchomieniu komputera](tutorial-automate-vm-deployment.md), wiesz, jak można zautomatyzować dostosowania maszyny Wirtualnej z inicjowaniem chmury. Można użyć tego samego pliku konfiguracji chmury init NGINX zainstalować i uruchomić prostej aplikacji Node.js "Hello World".

W bieżącym powłoki, Utwórz plik o nazwie *init.txt chmury* i wklej następującą konfigurację. Na przykład utworzyć plik, w powłoce chmury nie na komputerze lokalnym. Wprowadź `sensible-editor cloud-init.txt` do tworzenia pliku i wyświetlić listę dostępnych edytory. Upewnij się, że poprawnie skopiować pliku całego init chmury szczególnie pierwszy wiersz:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych
Aby zwiększyć wysoką dostępność aplikacji, umieść maszyn wirtualnych w zestawie dostępności. Aby uzyskać więcej informacji na temat zestawów dostępności, zobacz poprzedni [sposób tworzenia maszyn wirtualnych o wysokiej dostępności](tutorial-availability-sets.md) samouczka.

Utwórz zestaw o dostępności [tworzenia maszyny wirtualnej az zestawu dostępności](/cli/azure/vm/availability-set#create). Poniższy przykład tworzy zbiór nazwanego dostępności *myAvailabilitySet*:

```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupLoadBalancer \
    --name myAvailabilitySet
```

Teraz można tworzyć maszyn wirtualnych o [tworzenia maszyny wirtualnej az](/cli/azure/vm#create). Poniższy przykład tworzy trzech maszyn wirtualnych i generuje klucze SSH, jeśli jeszcze nie istnieje:

```bash
for i in `seq 1 3`; do
    az vm create \
        --resource-group myResourceGroupLoadBalancer \
        --name myVM$i \
        --availability-set myAvailabilitySet \
        --nics myNic$i \
        --image UbuntuLTS \
        --admin-username azureuser \
        --generate-ssh-keys \
        --custom-data cloud-init.txt \
        --no-wait
done
```

Istnieją zadania w tle, które nadal działać po interfejsu wiersza polecenia Azure powrót do wiersza polecenia. `--no-wait` Parametr bez oczekiwania na ukończenie wszystkich zadań. Może to być inny kilka minut, w celu uzyskania dostępu do aplikacji. Sondy kondycji modułu równoważenia obciążenia automatycznie wykrywa, gdy aplikacja jest uruchomiona na każdej maszynie Wirtualnej. Gdy aplikacja jest uruchomiona, rozpoczyna się reguły modułu równoważenia obciążenia do dystrybucji ruchu.


## <a name="test-load-balancer"></a>Test usługi równoważenia obciążenia
Publiczny adres IP z usługi równoważenia obciążenia z [az sieci ip publicznego Pokaż](/cli/azure/network/public-ip#show). Poniższy przykład uzyskuje adres IP dla *myPublicIP* utworzony wcześniej:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

Następnie można wprowadzić publicznego adresu IP w przeglądarce sieci web. Pamiętaj — zajmuje kilka minut maszyn wirtualnych będzie gotowa, przed rozpoczęciem dystrybucji ruchu do nich usługi równoważenia obciążenia. Aplikacja jest wyświetlana, łącznie z nazwą hosta maszyny Wirtualnej dystrybuowanej usługi równoważenia obciążenia w ruchu, jak w poniższym przykładzie:

![Uruchomionej aplikacji Node.js](./media/tutorial-load-balancer/running-nodejs-app.png)

Aby wyświetlić Dystrybuuj ruch we wszystkich trzech maszyn wirtualnych z tą aplikacją usługi równoważenia obciążenia, możesz można życie odświeżania przeglądarki sieci web.


## <a name="add-and-remove-vms"></a>Dodawanie i usuwanie maszyny wirtualne
Może być konieczne przeprowadzenie konserwacji na maszynach wirtualnych z tą aplikacją, takich jak instalowanie aktualizacji systemu operacyjnego. Aby poradzić sobie z zwiększenie obciążenia do aplikacji, może być konieczne dodanie kolejnych maszyn wirtualnych. W tej sekcji przedstawiono sposób Usuń lub Dodaj Maszynę wirtualną z modułu równoważenia obciążenia.

### <a name="remove-a-vm-from-the-load-balancer"></a>Usuń Maszynę wirtualną z usługi równoważenia obciążenia
Możesz usunąć Maszynę wirtualną z puli adresów zaplecza z [az kart konfiguracji ip puli adresów sieciowych — Usuń](/cli/azure/network/nic/ip-config/address-pool#remove). Poniższy przykład umożliwia usunięcie wirtualnej karty Sieciowej dla **myVM2** z *myLoadBalancer*:

```azurecli-interactive 
az network nic ip-config address-pool remove \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

Aby wyświetlić rozpowszechniają ruchu pozostałych dwóch maszyn wirtualnych z tą aplikacją usługi równoważenia obciążenia można można życie odświeżania przeglądarki sieci web. Teraz można przeprowadzać konserwacji na maszynie Wirtualnej, takie jak instalowanie aktualizacji systemu operacyjnego lub wykonywania ponownego uruchomienia maszyny Wirtualnej.

### <a name="add-a-vm-to-the-load-balancer"></a>Dodaj Maszynę wirtualną z usługą równoważenia obciążenia
Po wykonaniu obsługi maszyny Wirtualnej, lub jeśli trzeba zwiększyć pojemność, można dodać maszyny Wirtualnej do puli adresów zaplecza z [az kart konfiguracji ip puli adresów sieciowych — Dodaj](/cli/azure/network/nic/ip-config/address-pool#add). W poniższym przykładzie dodano wirtualnej karty Sieciowej dla **myVM2** do *myLoadBalancer*:

```azurecli-interactive 
az network nic ip-config address-pool add \
    --resource-group myResourceGroupLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```


## <a name="next-steps"></a>Następne kroki
W tym samouczku utworzony moduł równoważenia obciążenia i dołączone do maszyn wirtualnych. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie usługi równoważenia obciążenia Azure
> * Utwórz kondycji sondę modułu równoważenia obciążenia
> * Tworzenie reguły ruchu usługi równoważenia obciążenia
> * Umożliwia utworzenie podstawowej aplikacji Node.js init chmury
> * Tworzenie maszyn wirtualnych i dołączanie do usługi równoważenia obciążenia
> * Wyświetl modułu równoważenia obciążenia w akcji
> * Dodawanie i usuwanie maszyny wirtualne z modułem równoważenia obciążenia

Przejdź do następnego samouczkiem, aby dowiedzieć się więcej o składnikach sieci wirtualnej platformy Azure.

> [!div class="nextstepaction"]
> [Zarządzanie maszynami wirtualnymi i sieciami wirtualnymi](tutorial-virtual-network.md)
