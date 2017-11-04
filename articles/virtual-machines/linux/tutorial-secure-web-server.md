---
title: "Zabezpieczenia serwera sieci web z certyfikatów SSL na platformie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zabezpieczyć serwer sieci web NGINX z certyfikatów SSL na maszynie Wirtualnej systemu Linux na platformie Azure"
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
ms.date: 07/17/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 4e3ad8a5c08b739d8b2c6e224db0c8f88c1893ba
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="secure-a-web-server-with-ssl-certificates-on-a-linux-virtual-machine-in-azure"></a>Zabezpieczenia serwera sieci web z certyfikatów SSL na maszynie wirtualnej systemu Linux na platformie Azure
Do zabezpieczania serwerów sieci web, certyfikatu później SSL (Secure Sockets) może być używany do szyfrowania ruchu w sieci web. Te certyfikaty SSL mogą być przechowywane w usłudze Azure Key Vault i Zezwalaj wdrożeń zabezpieczonych certyfikatów na maszynach wirtualnych systemu Linux (VM) na platformie Azure. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie usługi Azure Key Vault
> * Generowanie lub Przekaż certyfikat do magazynu kluczy
> * Utwórz maszynę Wirtualną i zainstaluj NGINX serwera sieci web
> * Wstaw certyfikat do maszyny Wirtualnej i skonfigurować NGINX wraz z powiązaniem SSL

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli).  


## <a name="overview"></a>Omówienie
Usługa Azure Key Vault zabezpiecza kluczy kryptograficznych i kluczy tajnych takich certyfikatów lub hasła. Key Vault ułatwia uprościć proces zarządzania certyfikatu i pozwala zachować kontrolę nad kluczami, które uzyskują dostęp do tych certyfikatów. Można utworzyć certyfikatu z podpisem własnym wewnątrz usługi Key Vault, lub Przekaż istniejących, zaufanego certyfikatu, który już następującą.

Zamiast przy użyciu niestandardowego obrazu maszyny Wirtualnej, który zawiera certyfikaty rozszerzania w, wstrzyknąć certyfikatów do uruchomionej maszyny Wirtualnej. Ten proces zapewnia, że najbardziej aktualne certyfikaty są zainstalowane na serwerze sieci web podczas wdrażania. Jeśli odnowić lub Zastąp certyfikat, masz nie można utworzyć nowego niestandardowego obrazu maszyny Wirtualnej. Najnowsze certyfikaty są automatycznie dodane podczas tworzenia dodatkowych maszyn wirtualnych. W trakcie całego nigdy nie certyfikatów pozostaw platformy Azure lub są widoczne w skrypcie, Historia wiersza polecenia lub szablonu.


## <a name="create-an-azure-key-vault"></a>Tworzenie usługi Azure Key Vault
Przed utworzeniem magazyn kluczy i certyfikatów, Utwórz nową grupę zasobów o [Tworzenie grupy az](/cli/azure/group#create). Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroupSecureWeb* w *eastus* lokalizacji:

```azurecli-interactive 
az group create --name myResourceGroupSecureWeb --location eastus
```

Następnie należy utworzyć magazyn kluczy o [az keyvault utworzyć](/cli/azure/keyvault#create) i włącz ją do użycia podczas wdrażania maszyny Wirtualnej. Każdy magazyn kluczy wymaga unikatowej nazwy i powinna być małe litery. Zastąp  *<mykeyvault>*  w poniższym przykładzie z własną unikatową nazwę usługi Key Vault:

```azurecli-interactive 
keyvault_name=<mykeyvault>
az keyvault create \
    --resource-group myResourceGroupSecureWeb \
    --name $keyvault_name \
    --enabled-for-deployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Wygeneruj certyfikat i przechowywania w magazynie kluczy
W środowisku produkcyjnym, należy zaimportować prawidłowy certyfikat podpisane przez zaufanego dostawcę z [importu certyfikatów keyvault az](/cli/azure/certificate#import). W tym samouczku, w poniższym przykładzie pokazano, jak można wygenerować certyfikatu z podpisem własnym z [utworzenia certyfikatu keyvault az](/cli/azure/certificate#create) używającą domyślne zasady certyfikatu:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```

### <a name="prepare-a-certificate-for-use-with-a-vm"></a>Przygotowanie certyfikat do użycia z maszyny Wirtualnej
Do używania certyfikatu podczas maszyny Wirtualnej utworzyć procesu, Uzyskaj identyfikator certyfikatu z [az keyvault wersje klucza tajnego listy-](/cli/azure/keyvault/secret#list-versions). Konwertuj certyfikatu z [az wirtualna format klucz tajny](/cli/azure/vm#format-secret). Poniższy przykład przypisuje dane wyjściowe tych poleceń zmienne łatwość użycia w następnych krokach:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```

### <a name="create-a-cloud-init-config-to-secure-nginx"></a>Tworzenie konfiguracji chmury init, aby zabezpieczyć NGINX
[Init chmury](https://cloudinit.readthedocs.io) jest powszechnie używaną podejście, aby dostosować Maszynę wirtualną systemu Linux, ponieważ jest on uruchamiany po raz pierwszy. Init chmury można użyć, aby zainstalować pakiety i zapisywać pliki, lub aby skonfigurować użytkowników i zabezpieczeń. Podczas inicjowania chmury działania podczas początkowego procesu rozruchu, nie ma, nie dodatkowe kroki lub agentów wymaganych do zastosowania konfiguracji.

Podczas tworzenia maszyny Wirtualnej, certyfikaty i klucze są przechowywane w chronionej */var/lib/agentawaagent/* katalogu. Aby zautomatyzować Dodawanie certyfikatu do maszyny Wirtualnej i skonfigurowanie serwera sieci web, użyj init chmury. W tym przykładzie mamy Instalowanie i konfigurowanie serwera sieci web NGINX. Ten sam proces służy do instalowania i konfigurowania Apache. 

Utwórz plik o nazwie *chmurze init-web-server.txt* i wklej następującą konfigurację:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
      }
runcmd:
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
```

### <a name="create-a-secure-vm"></a>Tworzenie bezpiecznej maszyny Wirtualnej
Teraz Utwórz maszynę Wirtualną z [tworzenia maszyny wirtualnej az](/cli/azure/vm#create). Dane certyfikatu jest wprowadzonym z magazynu kluczy o `--secrets` parametru. Przekazywane w konfiguracji chmury init z `--custom-data` parametru:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-web-server.txt \
    --secrets "$vm_secret"
```

Trwa kilka minut, aż do utworzenia maszyny Wirtualnej, pakietów do zainstalowania i aplikacji, aby rozpocząć. Po utworzeniu maszyny Wirtualnej, zanotuj `publicIpAddress` wyświetlanych przez wiersza polecenia platformy Azure. Ten adres jest używany do dostępu do witryny w przeglądarce sieci web.

Aby zezwolić na ruch bezpieczną internetową nawiązać połączenie z maszyną Wirtualną, otwórz port 443 z Internetu z [port Otwórz az maszyny wirtualnej](/cli/azure/vm#open-port):

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --port 443
```


### <a name="test-the-secure-web-app"></a>Testowanie aplikacji sieci web bezpieczne
Teraz możesz otworzyć przeglądarkę sieci web i wprowadź *https://<publicIpAddress>*  na pasku adresu. Podaj własny publicznego adresu IP z maszyny Wirtualnej utworzyć procesu. Jeśli używasz certyfikatu z podpisem własnym, zaakceptuj ostrzeżenie o zabezpieczeniach:

![Zaakceptuj ostrzeżenie o zabezpieczeniach przeglądarki sieci web](./media/tutorial-secure-web-server/browser-warning.png)

Następnie wyświetleniem zabezpieczonej witrynie NGINX jak w poniższym przykładzie:

![Uruchamianie zabezpieczoną witryną NGINX widoku](./media/tutorial-secure-web-server/secured-nginx.png)


## <a name="next-steps"></a>Następne kroki

W tym samouczku NGINX serwera sieci web jest zabezpieczony za pomocą certyfikatu SSL, przechowywane w usłudze Azure Key Vault. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie usługi Azure Key Vault
> * Generowanie lub Przekaż certyfikat do magazynu kluczy
> * Utwórz maszynę Wirtualną i zainstaluj NGINX serwera sieci web
> * Wstaw certyfikat do maszyny Wirtualnej i skonfigurować NGINX wraz z powiązaniem SSL

Wykonaj to łącze, aby wyświetlić przykłady skryptów wbudowanych maszyny wirtualnej.

> [!div class="nextstepaction"]
> [Przykłady skryptów maszyny wirtualnej systemu Windows](./cli-samples.md)