---
title: "Utwórz zestawy skalowania maszyny wirtualnej dla systemu Linux na platformie Azure | Dokumentacja firmy Microsoft"
description: "Tworzenie i wdrażanie aplikacji wysokiej dostępności na maszynach wirtualnych systemu Linux przy użyciu zestawu skali maszyny wirtualnej"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: 8703d0c06f2507cc3c21d4280d887a8772145a28
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a>Tworzenie zestawu skali maszyny wirtualnej i wdrażanie aplikacji wysokiej dostępności w systemie Linux
Zestaw skali maszyny wirtualnej umożliwia wdrażanie i zarządzanie nimi zestaw identyczne, automatyczne skalowanie maszyn wirtualnych. Można ręcznie skalować liczbę maszyn wirtualnych w zestawie skalowania lub definiowania reguł do skalowania automatycznego na podstawie użycia zasobów, takie jak procesor CPU, pamięci żądanie lub ruchu sieciowego. W tym samouczku możesz wdrożyć skali maszyny wirtualnej w usłudze Azure. Omawiane kwestie:

> [!div class="checklist"]
> * Umożliwia tworzenie aplikacji do skalowania init chmury
> * Utwórz zestaw skali maszyny wirtualnej
> * Zwiększ lub Zmniejsz liczbę wystąpień w zestawie skalowania
> * Tworzenie reguł skalowania automatycznego
> * Wyświetl informacje o połączeniu dla wystąpień zestawu skali
> * Dysków danych w zestawie skalowania


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.22 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="scale-set-overview"></a>Omówienie zestawu skali
Zestaw skali maszyny wirtualnej umożliwia wdrażanie i zarządzanie nimi zestaw identyczne, automatyczne skalowanie maszyn wirtualnych. Maszyny wirtualne w zestawie skalowania są dystrybuowane w domenach awarii i aktualizacji logikę w co najmniej jednej *umieszczania grupy*. Są to grupy podobnie skonfigurowanych maszyn wirtualnych, podobnie jak [zestawów dostępności](tutorial-availability-sets.md).

Maszyny wirtualne są tworzone zgodnie z potrzebami w zestawie skalowania. Należy zdefiniować regułę automatycznego skalowania do kontrolowania sposobu i czasu maszyny wirtualne są dodawane lub usuwane z zestawu skalowania. Reguły te mogą być wyzwalane w oparciu metryki przykład obciążenia procesora CPU, pamięć lub ruchu sieciowego.

Skala ustawia obsługi maksymalnie 1000 maszyn wirtualnych, korzystając z obrazu platformy Azure. W przypadku obciążeń z instalacją znaczących lub wymagania dotyczące dostosowywania maszyny Wirtualnej, warto [utworzyć niestandardowy obraz maszyny Wirtualnej](tutorial-custom-images.md). Możesz utworzyć maksymalnie 300 maszyn wirtualnych w skali ustawić przy użyciu obrazu niestandardowego.


## <a name="create-an-app-to-scale"></a>Utwórz aplikację do skalowania
W środowisku produkcyjnym, warto [utworzyć niestandardowy obraz maszyny Wirtualnej](tutorial-custom-images.md) zawierającą aplikacji zainstalowane i skonfigurowane. W tym samouczku umożliwia dostosowywanie maszyn wirtualnych po pierwszym uruchomieniu komputera, aby szybko wyświetlić zestaw akcji skalowania.

W poprzednich samouczka przedstawiono [sposobu dostosowywania maszyny wirtualnej systemu Linux, po pierwszym uruchomieniu komputera](tutorial-automate-vm-deployment.md) z inicjowaniem chmury. Można użyć tego samego pliku konfiguracji chmury init NGINX zainstalować i uruchomić prostej aplikacji Node.js "Hello World". 

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


## <a name="create-a-scale-set"></a>Utwórz zestaw skali
Przed utworzeniem zestaw skali, Utwórz grupę zasobów o [Tworzenie grupy az](/cli/azure/group#create). Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroupScaleSet* w *eastus* lokalizacji:

```azurecli-interactive 
az group create --name myResourceGroupScaleSet --location eastus
```

Teraz Utwórz zestaw o skali maszyny wirtualnej [az vmss utworzyć](/cli/azure/vmss#create). Poniższy przykład tworzy zestaw o nazwie skalowania *myScaleSet*, używa pliku init chmury do dostosowywania maszyny Wirtualnej i generuje klucze SSH, jeśli nie istnieją:

```azurecli-interactive 
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys
```

Trwa kilka minut, aby utworzyć i skonfigurować zasoby zestaw skali i maszyn wirtualnych. Istnieją zadania w tle, które nadal działać po interfejsu wiersza polecenia Azure powrót do wiersza polecenia. Może to być inny kilka minut, w celu uzyskania dostępu do aplikacji.


## <a name="allow-web-traffic"></a>Zezwalaj na ruch w sieci web
Moduł równoważenia obciążenia został utworzony automatycznie jako część zestawu skali maszyny wirtualnej. Moduł równoważenia obciążenia dystrybuuje ruch zestawu zdefiniowanego maszyn wirtualnych przy użyciu reguły modułu równoważenia obciążenia. Dowiedz się więcej o pojęcia dotyczące usługi równoważenia obciążenia i konfiguracji w następnym samouczku [jak równoważyć obciążenie maszyn wirtualnych na platformie Azure](tutorial-load-balancer.md).

Aby zezwolić na ruch do aplikacji sieci web, należy utworzyć regułę z [utworzyć regułę równoważeniem obciążenia sieciowego az](/cli/azure/network/lb/rule#create). Poniższy przykład tworzy reguły o nazwie *myLoadBalancerRuleWeb*:

```azurecli-interactive 
az network lb rule create \
  --resource-group myResourceGroupScaleSet \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```

## <a name="test-your-app"></a>Testowanie aplikacji
Aby wyświetlić aplikację Node.js w sieci web, należy uzyskać publicznego adresu IP z usługi równoważenia obciążenia z [az sieci ip publicznego Pokaż](/cli/azure/network/public-ip#show). Poniższy przykład uzyskuje adres IP dla *myScaleSetLBPublicIP* utworzona w ramach zestawu skali:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

Wprowadź publicznego adresu IP w przeglądarce sieci web. Aplikacja jest wyświetlana, łącznie z nazwą hosta maszyny Wirtualnej, ruch do dystrybucji modułu równoważenia obciążenia:

![Uruchomionej aplikacji Node.js](./media/tutorial-create-vmss/running-nodejs-app.png)

Aby wyświetlić zestaw akcji skalowania, możesz można życie odświeżania przeglądarki sieci web, aby zobaczyć Dystrybuuj ruch na wszystkich maszynach wirtualnych z tą aplikacją usługi równoważenia obciążenia.


## <a name="management-tasks"></a>Zadania zarządzania
W całym cyklu życia zestawu skalowania konieczne może być Uruchom jedno lub więcej zadań zarządzania. Ponadto można tworzenia skryptów automatyzujących różnych zadań cyklu życia. Azure CLI 2.0 umożliwia szybkie do wykonywania tych zadań. Poniżej przedstawiono kilka typowych zadań.

### <a name="view-vms-in-a-scale-set"></a>Maszyny wirtualne widoku w zestawie skalowania
Aby wyświetlić listę uruchomionych w zestawie skalowania maszyn wirtualnych, użyj [wystąpienia listy az vmss](/cli/azure/vmss#list-instances) w następujący sposób:

```azurecli-interactive 
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

Dane wyjściowe są podobne do poniższego przykładu:

```azurecli-interactive 
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a>Zwiększanie lub zmniejszanie wystąpień maszyn wirtualnych
Aby wyświetlić liczbę wystąpień aktualnie zainstalowana w zestawie skalowania, użyj [Pokaż vmss az](/cli/azure/vmss#show) i wykonywać zapytania na *sku.capacity*:

```azurecli-interactive 
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

Możesz ręcznie zwiększyć lub zmniejszyć liczbę maszyn wirtualnych w skali ustawiony za pomocą [skali vmss az](/cli/azure/vmss#scale). Poniższy przykład ustawia liczbę maszyn wirtualnych w skali, z ustawioną *3*:

```azurecli-interactive 
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 3
```


### <a name="configure-autoscale-rules"></a>Konfigurowanie reguł automatycznego skalowania
Zamiast ręcznie skalowanie liczby wystąpień w skali sieci, można zdefiniować reguły automatycznego skalowania. Te reguły monitorować wystąpienia w zestawie skali i Odpowiedz, odpowiednio na podstawie metryk i progów, które należy zdefiniować. Limit liczby wystąpień programu przez jedną skaluje poniższy przykład, gdy średni obciążenie procesora CPU jest większy niż 60% w okresie 5 minut. Jeśli w okresie 5-minutowy średnie obciążenie procesora CPU spadnie poniżej 30%, wystąpienia są skalowane w przez jedno wystąpienie. Identyfikator subskrypcji jest używany do tworzenia zasobu identyfikatorów URI różnych skali zestaw składników. Aby utworzyć reguły z [utworzyć ustawienia skalowania automatycznego az monitora](/cli/azure/monitor/autoscale-settings#create), skopiuj i wklej poniższy profil polecenia skalowania automatycznego:

```azurecli-interactive 
sub=$(az account show --query id -o tsv)

az monitor autoscale-settings create \
    --resource-group myResourceGroupScaleSet \
    --name autoscale \
    --parameters '{"autoscale_setting_resource_name": "autoscale",
      "enabled": true,
      "location": "East US",
      "notifications": [],
      "profiles": [
        {
          "name": "Auto created scale condition",
          "capacity": {
            "minimum": "2",
            "maximum": "10",
            "default": "2"
          },
          "rules": [
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet",
                "metricResourceLocation": "eastus",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT5M",
                "timeAggregation": "Average",
                "operator": "GreaterThan",
                "threshold": 70
              },
              "scaleAction": {
                "direction": "Increase",
                "type": "ChangeCount",
                "value": "1",
                "cooldown": "PT5M"
              }
            },
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet",
                "metricResourceLocation": "eastus",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT5M",
                "timeAggregation": "Average",
                "operator": "LessThan",
                "threshold": 30
              },
              "scaleAction": {
                "direction": "Decrease",
                "type": "ChangeCount",
                "value": "1",
                "cooldown": "PT5M"
              }
            }
          ]
        }
      ],
      "tags": {},
      "target_resource_uri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet"
    }'
```

Ponowne użycie profilów skalowania automatycznego, Utwórz plik JSON (JavaScript Object Notation) i że w celu przekazania `az monitor autoscale-settings create` z `--parameters @autoscale.json` parametru. Aby uzyskać więcej informacji projektu na korzystanie z automatycznego skalowania, zobacz [najlepsze rozwiązania w zakresie skalowania automatycznego](/azure/architecture/best-practices/auto-scaling).


### <a name="get-connection-info"></a>Pobierz informacje o połączeniu
Aby uzyskać informacje o połączeniu o w Twojej zestawy skalowania maszyn wirtualnych, użyj [az vmss listy--połączenia — informacje o wystąpieniu](/cli/azure/vmss#list-instance-connection-info). To polecenie wyświetla publicznego adresu IP i portu dla każdej maszyny Wirtualnej, która umożliwia nawiązanie połączenia przy użyciu protokołu SSH:

```azurecli-interactive 
az vmss list-instance-connection-info \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet
```


## <a name="use-data-disks-with-scale-sets"></a>Dyski danych za pomocą zestawów skali
Można tworzyć i dysków z danymi za pomocą zestawów skali. W poprzednich samouczka przedstawiono sposób [dysków Azure zarządzanie](tutorial-manage-disks.md) który przedstawiono najlepsze rozwiązania i ulepszenia wydajności umożliwiające tworzenie aplikacji dla dysków z danymi, a nie na dysku systemu operacyjnego.

### <a name="create-scale-set-with-data-disks"></a>Utwórz zestaw skali z dysków z danymi
Aby utworzyć zestaw skali i dołączyć dysk danych, należy dodać `--data-disk-sizes-gb` parametr [az vmss utworzyć](/cli/azure/vmss#create) polecenia. Poniższy przykład tworzy zestaw o skali *50*do niej dołączone dyski danych Gb na każde wystąpienie:

```azurecli-interactive 
az vmss create \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetDisks \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --custom-data cloud-init.txt \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50
```

Usunięcie wystąpienia z zestawu skalowania żadnych dysków dołączonych danych są również usuwane.

### <a name="add-data-disks"></a>Dodawanie dysków z danymi
Aby dodać dysk z danymi do wystąpień w zestawie skali, użyj [dołączyć dysku vmss az](/cli/azure/vmss/disk#attach). W poniższym przykładzie dodano *50*dysk Gb na każde wystąpienie:

```azurecli-interactive 
az vmss disk attach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --size-gb 50 \
    --lun 2
```

### <a name="detach-data-disks"></a>Odłączanie dysku z danymi
Aby usunąć dysk z danymi do wystąpień w zestawie skali, użyj [odłączyć dysku vmss az](/cli/azure/vmss/disk#detach). Poniższy przykład umożliwia usunięcie dysk z danymi o numerze LUN *2* z każde wystąpienie:

```azurecli-interactive 
az vmss disk detach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --lun 2
```


## <a name="next-steps"></a>Kolejne kroki
W tym samouczku utworzony zestaw skali maszyny wirtualnej. W tym samouczku omówiono:

> [!div class="checklist"]
> * Umożliwia tworzenie aplikacji do skalowania init chmury
> * Utwórz zestaw skali maszyny wirtualnej
> * Zwiększ lub Zmniejsz liczbę wystąpień w zestawie skalowania
> * Tworzenie reguł skalowania automatycznego
> * Wyświetl informacje o połączeniu dla wystąpień zestawu skali
> * Dysków danych w zestawie skalowania

Przejdź do następnego samouczek, aby dowiedzieć się więcej na temat pojęć dla maszyn wirtualnych równoważenia obciążenia.

> [!div class="nextstepaction"]
> [Równoważyć obciążenie maszyn wirtualnych](tutorial-load-balancer.md)
