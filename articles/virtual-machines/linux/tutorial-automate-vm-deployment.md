---
title: Dostosowywanie maszyny Wirtualnej systemu Linux, po pierwszym uruchomieniu komputera na platformie Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak na potrzeby chmury init i Key Vault customze maszyn wirtualnych systemu Linux rozruchu Azure po raz pierwszy"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 4e2d07a03902a8c837150da8d50ab9abec8d1c95
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-customize-a-linux-virtual-machine-on-first-boot"></a>Dostosowywanie maszyny wirtualnej systemu Linux, po pierwszym uruchomieniu komputera
W poprzednich samouczka przedstawiono sposób, aby SSH z maszyną wirtualną (VM) oraz ręcznie zainstalować NGINX. Do tworzenia maszyn wirtualnych w sposób szybki i spójny, wymagane jest zwykle jakiegoś automatyzacji. Typowym podejściem dostosować Maszynę wirtualną po pierwszym uruchomieniu komputera jest użycie [init chmury](https://cloudinit.readthedocs.io). Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz plik konfiguracji init chmury
> * Utwórz maszynę Wirtualną, która używa pliku init chmury
> * Wyświetlanie działającej aplikacji Node.js po utworzeniu maszyny Wirtualnej
> * Użyj magazynu kluczy bezpiecznie przechowywać certyfikatów
> * Automatyzacja bezpiecznego wdrażania z NGINX z inicjowaniem chmury


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli).  



## <a name="cloud-init-overview"></a>Init chmury — omówienie
[Init chmury](https://cloudinit.readthedocs.io) jest powszechnie używaną podejście, aby dostosować Maszynę wirtualną systemu Linux, ponieważ jest on uruchamiany po raz pierwszy. Init chmury można użyć, aby zainstalować pakiety i zapisywać pliki, lub aby skonfigurować użytkowników i zabezpieczeń. Podczas inicjowania chmury działania podczas początkowego procesu rozruchu, nie ma, nie dodatkowe kroki lub agentów wymaganych do zastosowania konfiguracji.

Init chmury działa także w dystrybucji. Na przykład nie używaj **instalacji stanie get** lub **yum zainstalować** do zainstalowania pakietu. Zamiast tego można zdefiniować listę pakietów do zainstalowania. Init chmury automatycznie używa narzędzia do zarządzania natywnego pakietu dla distro, którą wybierzesz.

Pracujemy nad z naszych partnerów uzyskanie init chmury uwzględnione i Praca w obrazach, zapewniające na platformie Azure. W poniższej tabeli przedstawiono bieżącej dostępności init chmury na obrazy platformy Azure:

| Alias | Wydawca | Oferta | SKU | Wersja |
|:--- |:--- |:--- |:--- |:--- |:--- |
| UbuntuLTS |Canonical |UbuntuServer |16.04 LTS |najnowsza |
| UbuntuLTS |Canonical |UbuntuServer |14.04.5-LTS |najnowsza |
| CoreOS |CoreOS |CoreOS |Stable |najnowsza |


## <a name="create-cloud-init-config-file"></a>Utwórz plik konfiguracji init chmury
Aby wyświetlić inicjowania chmury w akcji, utwórz maszynę Wirtualną, która instaluje NGINX i uruchamia prosty "Hello World" aplikacji Node.js. Następującą konfigurację chmury init instaluje wymagane pakiety, tworzy aplikacji Node.js, a następnie zainicjować i uruchamia aplikację.

W bieżącym powłoki, Utwórz plik o nazwie *init.txt chmury* i wklej następującą konfigurację. Na przykład utworzyć plik, w powłoce chmury nie na komputerze lokalnym. Można użyć dowolnego edytora, którego chcesz. Wprowadź `sensible-editor cloud-init.txt` do tworzenia pliku i wyświetlić listę dostępnych edytory. Upewnij się, że poprawnie skopiować pliku całego init chmury szczególnie pierwszy wiersz:

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

Aby uzyskać więcej informacji o opcjach konfiguracji chmury init, zobacz [przykłady konfiguracji chmury init](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).

## <a name="create-virtual-machine"></a>Tworzenie maszyny wirtualnej
Przed utworzeniem maszyny Wirtualnej, Utwórz nową grupę zasobów o [Tworzenie grupy az](/cli/azure/group#create). Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroupAutomate* w *eastus* lokalizacji:

```azurecli-interactive 
az group create --name myResourceGroupAutomate --location eastus
```

Teraz Utwórz maszynę Wirtualną z [tworzenia maszyny wirtualnej az](/cli/azure/vm#create). Użyj `--custom-data` parametr do przekazania w pliku config init chmury. Podaj pełną ścieżkę do *init.txt chmury* konfiguracji, jeśli plik został zapisany poza istnieje katalog roboczy. Poniższy przykład tworzy Maszynę wirtualną o nazwie *myAutomatedVM*:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

Trwa kilka minut, aż do utworzenia maszyny Wirtualnej, pakietów do zainstalowania i aplikacji, aby rozpocząć. Istnieją zadania w tle, które nadal działać po interfejsu wiersza polecenia Azure powrót do wiersza polecenia. Może to być inny kilka minut, w celu uzyskania dostępu do aplikacji. Po utworzeniu maszyny Wirtualnej, zanotuj `publicIpAddress` wyświetlanych przez wiersza polecenia platformy Azure. Ten adres jest używany na dostęp do aplikacji Node.js za pośrednictwem przeglądarki sieci web.

Aby zezwolić na ruch w sieci web do maszyny Wirtualnej, należy otworzyć port 80 z Internetu z [port Otwórz az maszyny wirtualnej](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroupAutomate --name myVM
```

## <a name="test-web-app"></a>Przetestuj aplikację sieci web
Teraz możesz otworzyć przeglądarkę sieci web i wprowadź *http://<publicIpAddress>*  na pasku adresu. Podaj własny publicznego adresu IP z maszyny Wirtualnej utworzyć procesu. Aplikacja Node.js jest wyświetlana, jak w poniższym przykładzie:

![Wyświetl uruchomione NGINX lokacji](./media/tutorial-automate-vm-deployment/nginx.png)


## <a name="inject-certificates-from-key-vault"></a>Wstaw certyfikaty z magazynu kluczy
W tej sekcji opcjonalne przedstawiono, jak można bezpiecznie certyfikaty są przechowywane w usłudze Azure Key Vault i wstrzyknąć podczas wdrażania maszyny Wirtualnej. Zamiast przy użyciu niestandardowego obrazu, który zawiera certyfikaty rozszerzania programu, ten proces zapewnia, że najbardziej aktualne certyfikaty są wstrzykiwane do maszyny Wirtualnej po pierwszym uruchomieniu komputera. W trakcie nigdy nie opuszcza platformy Azure lub certyfikatu jest widoczna w skryptu, Historia wiersza polecenia lub szablonu.

Usługa Azure Key Vault zabezpiecza kluczy kryptograficznych i kluczy tajnych, takich jak certyfikaty lub hasła. Key Vault ułatwia uprościć proces zarządzania kluczami i pozwala zachować kontrolę nad kluczami, które dostępu i szyfrowania danych. W tym scenariuszu niektóre pojęcia Key Vault do utworzenia i użycia certyfikatu, jednak nie jest wyczerpujący Przegląd sposobu użycia usługi Key Vault.

W poniższej procedurze pokazano, jak można:

- Tworzenie usługi Azure Key Vault
- Generowanie lub Przekaż certyfikat do magazynu kluczy
- Utwórz klucz tajny z certyfikatów do dodania w z maszyną wirtualną
- Utwórz maszynę Wirtualną i wprowadzić certyfikat

### <a name="create-an-azure-key-vault"></a>Tworzenie usługi Azure Key Vault
Najpierw utwórz magazyn kluczy o [az keyvault utworzyć](/cli/azure/keyvault#create) i włącz ją do użycia podczas wdrażania maszyny Wirtualnej. Każdy magazyn kluczy wymaga unikatowej nazwy i powinna być małe litery. Zastąp *mykeyvault* w poniższym przykładzie z własną unikatową nazwę usługi Key Vault:

```azurecli-interactive 
keyvault_name=mykeyvault
az keyvault create \
    --resource-group myResourceGroupAutomate \
    --name $keyvault_name \
    --enabled-for-deployment
```

### <a name="generate-certificate-and-store-in-key-vault"></a>Generowanie certyfikatu i przechowywania w magazynie kluczy
W środowisku produkcyjnym, należy zaimportować prawidłowy certyfikat podpisane przez zaufanego dostawcę z [importu certyfikatów keyvault az](/cli/azure/keyvault/certificate#import). W tym samouczku, w poniższym przykładzie pokazano, jak można wygenerować certyfikatu z podpisem własnym z [utworzenia certyfikatu keyvault az](/cli/azure/keyvault/certificate#create) używającą domyślne zasady certyfikatu:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```


### <a name="prepare-certificate-for-use-with-vm"></a>Przygotowywanie certyfikatów do użytku z maszyną Wirtualną.
Do używania certyfikatu podczas maszyny Wirtualnej utworzyć procesu, Uzyskaj identyfikator certyfikatu z [az keyvault wersje klucza tajnego listy-](/cli/azure/keyvault/secret#list-versions). Maszyna wirtualna wymaga certyfikatu w określonym formacie do wprowadzenia jej podczas rozruchu, więc konwertowanie certyfikatu z [az wirtualna format klucz tajny](/cli/azure/vm#format-secret). Poniższy przykład przypisuje dane wyjściowe tych poleceń zmienne łatwość użycia w następnych krokach:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```


### <a name="create-cloud-init-config-to-secure-nginx"></a>Tworzenie konfiguracji chmury init do zabezpieczania NGINX
Podczas tworzenia maszyny Wirtualnej, certyfikaty i klucze są przechowywane w chronionej */var/lib/agentawaagent/* katalogu. Aby zautomatyzować Dodawanie certyfikatu do maszyny Wirtualnej i konfigurowanie NGINX, można użyć konfiguracji zaktualizowane init chmury z poprzedniego przykładu.

Utwórz plik o nazwie *chmurze init-secured.txt* i wklej następującą konfigurację. Ponownie korzystając z powłoki chmury, Utwórz plik konfiguracji init chmury, występują, a nie na komputerze lokalnym. Użyj `sensible-editor cloud-init-secured.txt` do tworzenia pliku i wyświetlić listę dostępnych edytory. Upewnij się, że poprawnie skopiować pliku całego init chmury szczególnie pierwszy wiersz:

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
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
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
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-secure-vm"></a>Tworzenie bezpiecznej maszyny Wirtualnej
Teraz Utwórz maszynę Wirtualną z [tworzenia maszyny wirtualnej az](/cli/azure/vm#create). Dane certyfikatu jest wprowadzonym z magazynu kluczy o `--secrets` parametru. Co w poprzednim przykładzie, możesz również przekazać w konfiguracji chmury init z `--custom-data` parametru:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-secured.txt \
    --secrets "$vm_secret"
```

Trwa kilka minut, aż do utworzenia maszyny Wirtualnej, pakietów do zainstalowania i aplikacji, aby rozpocząć. Istnieją zadania w tle, które nadal działać po interfejsu wiersza polecenia Azure powrót do wiersza polecenia. Może to być inny kilka minut, w celu uzyskania dostępu do aplikacji. Po utworzeniu maszyny Wirtualnej, zanotuj `publicIpAddress` wyświetlanych przez wiersza polecenia platformy Azure. Ten adres jest używany na dostęp do aplikacji Node.js za pośrednictwem przeglądarki sieci web.

Aby zezwolić na ruch bezpieczną internetową nawiązać połączenie z maszyną Wirtualną, otwórz port 443 z Internetu z [port Otwórz az maszyny wirtualnej](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --port 443
```

### <a name="test-secure-web-app"></a>Przetestuj aplikację sieci web bezpiecznego
Teraz możesz otworzyć przeglądarkę sieci web i wprowadź *https://<publicIpAddress>*  na pasku adresu. Podaj własny publicznego adresu IP z maszyny Wirtualnej utworzyć procesu. Jeśli używasz certyfikatu z podpisem własnym, zaakceptuj ostrzeżenie o zabezpieczeniach:

![Zaakceptuj ostrzeżenie o zabezpieczeniach przeglądarki sieci web](./media/tutorial-automate-vm-deployment/browser-warning.png)

Zabezpieczonej witrynie NGINX i Node.js aplikacji zostaną wyświetlone jak w poniższym przykładzie:

![Uruchamianie zabezpieczoną witryną NGINX widoku](./media/tutorial-automate-vm-deployment/secured-nginx.png)


## <a name="next-steps"></a>Następne kroki
W tym samouczku należy skonfigurować po pierwszym uruchomieniu komputera z inicjowaniem chmury maszyn wirtualnych. W tym samouczku omówiono:

> [!div class="checklist"]
> * Utwórz plik konfiguracji init chmury
> * Utwórz maszynę Wirtualną, która używa pliku init chmury
> * Wyświetlanie działającej aplikacji Node.js po utworzeniu maszyny Wirtualnej
> * Użyj magazynu kluczy bezpiecznie przechowywać certyfikatów
> * Automatyzacja bezpiecznego wdrażania z NGINX z inicjowaniem chmury

Przejdź do samouczka dalej informacje na temat tworzenia niestandardowych obrazów maszyn wirtualnych.

> [!div class="nextstepaction"]
> [Tworzenie niestandardowych obrazów maszyn wirtualnych](./tutorial-custom-images.md)
