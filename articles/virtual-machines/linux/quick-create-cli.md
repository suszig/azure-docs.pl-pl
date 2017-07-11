---
title: "Azure: Szybki start — Tworzenie maszyn wirtualnych za pomocą interfejsu wiersza polecenia | Microsoft Docs"
description: "Szybka nauka tworzenia maszyn wirtualnych przy użyciu interfejsu wiersza polecenia platformy Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: nepeters
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: a7cba5b2c43704d92e36d6f808efaa9fc73fdf36
ms.contentlocale: pl-pl
ms.lasthandoff: 06/20/2017

---

<a id="create-a-linux-virtual-machine-with-the-azure-cli" class="xliff"></a>

# Tworzenie maszyny wirtualnej z systemem Linux za pomocą interfejsu wiersza polecenia platformy Azure

Interfejs wiersza polecenia platformy Azure umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. W tym przewodniku zawarto szczegółowe instrukcje korzystania z interfejsu wiersza polecenia platformy Azure w celu wdrożenia maszyny wirtualnej z systemem Ubuntu Server. Po wdrożeniu serwera zostanie utworzone połączenie SSH i zostanie zainstalowany serwer sieci Web NGINX.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten przewodnik szybkiego startu będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

<a id="create-a-resource-group" class="xliff"></a>

## Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

<a id="create-virtual-machine" class="xliff"></a>

## Tworzenie maszyny wirtualnej

Utwórz maszynę wirtualną za pomocą polecenia [az vm create](/cli/azure/vm#create). 

Następujący przykład umożliwia utworzenie maszyny wirtualnej o nazwie *myVM* i kluczy SSH, jeśli jeszcze nie istnieją w domyślnej lokalizacji kluczy. Aby użyć określonego zestawu kluczy, użyj opcji `--ssh-key-value`.  

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

Po utworzeniu maszyny wirtualnej w interfejsie wiersza polecenia platformy Azure zostanie wyświetlona informacja podobna do następującej. Zwróć uwagę na element `publicIpAddress`. Ten adres jest używany na potrzeby uzyskiwania dostępu do maszyny wirtualnej.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

<a id="open-port-80-for-web-traffic" class="xliff"></a>

## Otwieranie portu 80 na potrzeby ruchu w sieci Web 

Domyślnie dozwolone są tylko połączenia SSH z maszynami wirtualnymi z systemem Linux wdrożonymi na platformie Azure. Jeśli ta maszyna wirtualna ma być serwerem sieci Web, port 80 należy otworzyć z Internetu. Otwórz odpowiedni port za pomocą polecenia [az vm open-port](/cli/azure/vm#open-port).  
 
 ```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

<a id="ssh-into-your-vm" class="xliff"></a>

## Łączenie z maszyną wirtualną za pośrednictwem protokołu SSH

Użyj następującego polecenia, aby utworzyć sesję SSH z maszyną wirtualną. Pamiętaj o zamianie elementu *<publicIpAddress>* na prawidłowy publiczny adres IP swojej maszyny wirtualnej.  W naszym powyższym przykładzie adres IP to *40.68.254.142*.

```bash 
ssh <publicIpAddress>
```

<a id="install-nginx" class="xliff"></a>

## Instalowanie serwera NGINX

Użyj poniższego skryptu powłoki systemowej w celu zaktualizowania źródeł pakietów i zainstalowania najnowszego pakietu NGINX. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

<a id="view-the-nginx-welcome-page" class="xliff"></a>

## Wyświetlanie strony powitalnej serwera NGINX

Po zainstalowaniu serwera NGINX i otwarciu portu 80 na maszynie wirtualnej z Internetu możesz użyć wybranej przeglądarki sieci Web, aby wyświetlić domyślną stronę powitalną przeglądarki serwera NGINX. Upewnij się, że w celu odwiedzenia strony domyślnej używasz udokumentowanego powyżej *publicznego adresu IP*. 

![Domyślna witryna serwera NGINX](./media/quick-create-cli/nginx.png) 


<a id="clean-up-resources" class="xliff"></a>

## Oczyszczanie zasobów

Gdy grupa zasobów, maszyna wirtualna i wszystkie pokrewne zasoby nie będą już potrzebne, można je usunąć za pomocą polecenia [az group delete](/cli/azure/group#delete).

```azurecli-interactive 
az group delete --name myResourceGroup
```

<a id="next-steps" class="xliff"></a>

## Następne kroki

W tym przewodniku Szybki start została wdrożona prosta maszyna wirtualna i reguła sieciowej grupy zabezpieczeń oraz zainstalowano serwer sieci Web. Aby dowiedzieć się więcej o maszynach wirtualnych platformy Azure, przejdź do samouczka dla maszyn wirtualnych z systemem Linux.


> [!div class="nextstepaction"]
> [Samouczki dla maszyny wirtualnej platformy Azure z systemem Linux](./tutorial-manage-vm.md)

