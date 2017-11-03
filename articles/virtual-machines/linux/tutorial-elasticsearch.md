---
title: "Wdrażanie ElasticSearch na maszynie wirtualnej programowanie na platformie Azure"
description: "Samouczek — instalacja stosu elastycznych na komputerze projektowym maszyny Wirtualnej systemu Linux na platformie Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rloutlaw
manager: justhe
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 10/11/2017
ms.author: routlaw
ms.openlocfilehash: 5b0b51504478cc0d501a89760ccd60808a69ccbd
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="install-the-elastic-stack-on-an-azure-vm"></a>Zainstaluj stosu elastycznych na maszynie Wirtualnej platformy Azure

W tym artykule przedstawiono sposób wdrażania [Elasticsearch](https://www.elastic.co/products/elasticsearch), [Logstash](https://www.elastic.co/products/logstash), i [Kibana](https://www.elastic.co/products/kibana), na maszynie Wirtualnej systemu Ubuntu na platformie Azure. Aby zobaczyć stos elastycznej w akcji, można opcjonalnie nawiązać Kibana i pracować z niektórych przykładowych danych rejestrowania. 

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie maszyny Wirtualnej systemu Ubuntu w grupie zasobów platformy Azure
> * Zainstaluj w maszynie Wirtualnej Elasticsearch, Logstash i Kibana
> * Wyślij przykładowych danych do Elasticsearch z Logstash 
> * Otwieranie portów i pracować z danymi w konsoli Kibana


 To wdrożenie jest odpowiednia podstawowego środowiska deweloperskiego przy użyciu elastycznej stosu. Aby uzyskać więcej informacji na temat elastycznej stosu, w tym zalecenia w środowisku produkcyjnym, zobacz [elastycznej dokumentacji](https://www.elastic.co/guide/index.html) i [Centrum architektura Azure](/azure/architecture/elasticsearch/).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Tworzenie maszyny wirtualnej

Utwórz maszynę wirtualną za pomocą polecenia [az vm create](/cli/azure/vm#create). 

Następujący przykład umożliwia utworzenie maszyny wirtualnej o nazwie *myVM* i kluczy SSH, jeśli jeszcze nie istnieją w domyślnej lokalizacji kluczy. Aby użyć określonego zestawu kluczy, użyj opcji `--ssh-key-value`.  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Po utworzeniu maszyny wirtualnej w interfejsie wiersza polecenia platformy Azure zostanie wyświetlona informacja podobna do następującej. Zwróć uwagę na element `publicIpAddress`. Ten adres jest używany na potrzeby uzyskiwania dostępu do maszyny wirtualnej.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="ssh-into-your-vm"></a>Łączenie z maszyną wirtualną za pośrednictwem protokołu SSH

Jeśli nie znasz już publicznego adresu IP maszyny Wirtualnej, należy uruchomić [az sieci ip publicznego listy](/cli/azure/network/public-ip#list) polecenia:

```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

Użyj następującego polecenia, aby utworzyć sesję SSH z maszyną wirtualną. Zastąp poprawne publiczny adres IP maszyny wirtualnej. W tym przykładzie adres IP jest *40.68.254.142*.

```bash
ssh azureuser@40.68.254.142
```

## <a name="install-the-elastic-stack"></a>Zainstaluj elastycznej stosu

Importowanie klucza podpisywania Elasticsearch i zaktualizować listę źródeł stanie uwzględnienie repozytorium pakietów elastycznej:

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
```

Instalowanie wirtualnej Java na maszynie Wirtualnej i konfigurowanie JAVA_HOME ta zmienna jest niezbędne składniki stosu elastycznych do uruchomienia.

```bash
sudo apt update && sudo apt install openjdk-8-jre-headless
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

Uruchom następujące polecenia, aby zaktualizować źródła pakietu Ubuntu i zainstaluj Elasticsearch, Kibana i Logstash.

```bash
sudo apt update && sudo apt install elasticsearch kibana logstash   
```

> [!NOTE]
> Szczegółowe instrukcje dotyczące instalacji, w tym katalogu układów i konfiguracji początkowej, są obsługiwane w [elastyczna w dokumentacji](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html)

## <a name="start-elasticsearch"></a>Uruchom Elasticsearch 

Uruchom Elasticsearch na maszynie Wirtualnej za pomocą następującego polecenia:

```bash
sudo systemctl start elasticsearch.service
```

To polecenie powoduje żadnych danych wyjściowych, dlatego Sprawdź, czy Elasticsearch jest uruchomiona na maszynie Wirtualnej z tym `curl` polecenia:

```bash
curl -XGET 'localhost:9200/'
```

Jeśli Elasticsearch jest uruchomiona, zapoznaj się dane wyjściowe podobne do następujących:

```json
{
  "name" : "w6Z4NwR",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "SDzCajBoSK2EkXmHvJVaDQ",
  "version" : {
    "number" : "5.6.3",
    "build_hash" : "1a2f265",
    "build_date" : "2017-10-06T20:33:39.012Z",
    "build_snapshot" : false,
    "lucene_version" : "6.6.1"
  },
  "tagline" : "You Know, for Search"
}
```

## <a name="start-logstash-and-add-data-to-elasticsearch"></a>Uruchom Logstash i dodać dane do Elasticsearch

Uruchom Logstash przy użyciu następującego polecenia:

```bash 
sudo systemctl start logstash.service
```

W trybie interakcyjnym, aby upewnić się, że działa prawidłowo, należy przetestować Logstash:

```bash
sudo /usr/share/logstash/bin/logstash -e 'input { stdin { } } output { stdout {} }'
```

Jest to podstawowy logstash [potoku](https://www.elastic.co/guide/en/logstash/5.6/pipeline.html) który echa standardowe dane wejściowe do wyjścia standardowego. 

```output
The stdin plugin is now waiting for input:
hello azure
2017-10-11T20:01:08.904Z myVM hello azure
```

Konfigurowanie Logstash do przekazywania komunikatów jądra z tej maszyny Wirtualnej do Elasticsearch. Utwórz nowy plik w pusty katalog o nazwie `vm-syslog-logstash.conf` i Wklej w następującej konfiguracji Logstash:

```Logstash
input {
    stdin {
        type => "stdin-type"
    }

    file {
        type => "syslog"
        path => [ "/var/log/*.log", "/var/log/*/*.log", "/var/log/messages", "/var/log/syslog" ]
        start_position => "beginning"
    }
}

output {

    stdout {
        codec => rubydebug
    }
    elasticsearch {
        hosts  => "localhost:9200"
    }
}
```

Przetestować tę konfigurację i wysyłanie danych z serwera syslog do Elasticsearch:

```bash
sudo /usr/share/logstash/bin/logstash -f vm-syslog-logstash.conf
```

W terminalu powtarzana, ponieważ są one wysyłane do Elasticsearch pojawić się wpisy dziennika systemowego. Użyj `CTRL+C` aby wyjść poza Logstash po wysłanych do niektórych danych.

## <a name="start-kibana-and-visualize-the-data-in-elasticsearch"></a>Uruchom Kibana i wizualizacji danych w Elasticsearch

Edytuj `/etc/kibana/kibana.yml` i zmień adres IP Kibana nasłuchuje umożliwi dostęp z przeglądarki sieci web.

```bash
server.host:"0.0.0.0"
```

Uruchom Kibana przy użyciu następującego polecenia:

```bash
sudo systemctl start kibana.service
```

Otwórz port 5601 z wiersza polecenia platformy Azure, aby umożliwić uzyskiwanie zdalnego dostępu do konsoli Kibana:

```azurecli-interactive
az vm open-port --port 5601 --resource-group myResourceGroup --name myVM
```

Otwarcie konsoli Kibana i wybierz **Utwórz** do wygenerowania na podstawie danych syslog wysłane do Elasticsearch wcześniej indeks domyślny. 

![Przeglądaj zdarzenia dziennika systemowego w Kibana](media/elasticsearch-install/kibana-index.png)

Wybierz **odnajdowania** na konsoli Kibana wyszukiwania Przeglądaj i odfiltrować zdarzenia dziennika systemowego.

![Przeglądaj zdarzenia dziennika systemowego w Kibana](media/elasticsearch-install/kibana-search-filter.png)

## <a name="next-steps"></a>Następne kroki

W tym samouczku wdrożono stosu elastycznych do tworzenia maszyny Wirtualnej na platformie Azure. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie maszyny Wirtualnej systemu Ubuntu w grupie zasobów platformy Azure
> * Zainstaluj w maszynie Wirtualnej Elasticsearch, Logstash i Kibana
> * Wysyłanie przykładowych danych do Elasticsearch z Logstash 
> * Otwieranie portów i pracować z danymi w konsoli Kibana