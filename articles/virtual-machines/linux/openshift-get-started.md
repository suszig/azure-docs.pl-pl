---
title: "Wdrażanie pochodzenia OpenShift na platformie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się wdrożyć pochodzenia OpenShift maszyn wirtualnych platformy Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: jbinder
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: jbinder
ms.openlocfilehash: e03da05625e440eab29ccc28a2343d3433fc7607
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-openshift-origin-to-azure-virtual-machines"></a>Wdrażanie pochodzenia OpenShift na maszynach wirtualnych platformy Azure 

[Pochodzenie OpenShift](https://www.openshift.org/) to platforma kontenera typu open source oparty na [Kubernetes](https://kubernetes.io/). Takie rozwiązanie upraszcza proces wdrażania, skalowanie i działania aplikacji wielodostępnej. 

W tym przewodniku opisano sposób wdrażania pochodzenia OpenShift na maszynach wirtualnych platformy Azure przy użyciu wiersza polecenia platformy Azure i szablonów Menedżera zasobów Azure. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz KeyVault do zarządzania kluczy SSH dla klastra OpenShift.
> * Wdrażanie klastra OpenShift na maszynach wirtualnych Azure. 
> * Instalowanie i konfigurowanie [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) do zarządzania klastrem.
> * Dostosowywanie wdrożenia OpenShift.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

To szybki start wymaga wiersza polecenia platformy Azure w wersji 2.0.8 lub nowszej. Aby dowiedzieć się, jaka wersja jest używana, uruchom polecenie `az --version`. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure. 
Zaloguj się do Twojej subskrypcji platformy Azure z [az logowania](/cli/azure/#login) polecenia i wykonaj na ekranie kliknij lub kierunki **wypróbuj** użycia chmury powłoki.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-key-vault"></a>Tworzenie magazynu kluczy
Utwórz KeyVault do przechowywania kluczy SSH dla klastra z [az keyvault utworzyć](/cli/azure/keyvault#create) polecenia.  

```azurecli 
az keyvault create --resource-group myResourceGroup --name myKeyVault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>Tworzenie klucza SSH 
Klucz SSH jest wymagany do bezpiecznego dostępu do klastra OpenShift pochodzenia. Utwórz parę kluczy SSH przy użyciu `ssh-keygen` polecenia. 
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> Pary kluczy SSH, którą utworzysz nie może zawierać hasło.

Aby uzyskać więcej informacji na temat kluczy SSH w systemie Windows [tworzenie SSH kluczy w systemie Windows](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-ssh-private-key-in-key-vault"></a>Przechowywanie klucza prywatnego SSH w magazynie kluczy
Wdrożenie OpenShift używa klucza SSH, utworzenia bezpiecznego dostępu do głównego OpenShift. Aby włączyć wdrożenie w celu bezpiecznego pobierania klucza SSH, należy przechowywać klucz w Key Vault za pomocą następującego polecenia.

# <a name="enabled-for-template-deployment"></a>Włączony do wdrożenia szablonu
```azurecli
az keyvault secret set --vault-name KeyVaultName --name OpenShiftKey --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a>Tworzenie nazwy głównej usługi 
OpenShift komunikuje się z platformy Azure przy użyciu nazwy użytkownika i hasła lub nazwy głównej usługi. Podmiot zabezpieczeń usługi Azure jest tożsamość zabezpieczeń korzystających z aplikacji, usługami i automatyzacja takich narzędzi jak OpenShift. Kontrolowanie i definiowanie uprawnień określające, jakie operacje nazwy głównej usługi można wykonać na platformie Azure. Zwiększające bezpieczeństwo za pośrednictwem tylko podanie nazwy użytkownika i hasła, w tym przykładzie tworzy podstawowe usługę podmiotu zabezpieczeń.

Utwórz usługę podmiotu zabezpieczeń z [az ad sp utworzyć do rbac](/cli/azure/ad/sp#create-for-rbac) i poświadczenia, które wymaga OpenShift wyjściowe:

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {strong password} \
          --scopes $(az group show --name myResourceGroup --query id)
```
Zwróć uwagę na właściwość appId zwrócone przez polecenie.
```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {strong password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > Nie twórz niezabezpieczonego hasła.  Postępuj zgodnie ze wskazówkami zawartymi w [ograniczeniach i regułach dotyczących hasła usługi Azure AD](/azure/active-directory/active-directory-passwords-policy).

Aby uzyskać więcej informacji dotyczących nazwy główne usług, zobacz [Tworzenie nazwy głównej usługi platformy Azure z 2.0 interfejsu wiersza polecenia platformy Azure](/cli/azure/create-an-azure-service-principal-azure-cli)

## <a name="deploy-the-openshift-origin-template"></a>Wdrażanie szablonu OpenShift źródła
Następnie wdrożyć pochodzenia OpenShift przy użyciu szablonu usługi Azure Resource Manager. 

> [!NOTE] 
> Polecenie wymaga az CLI 2.0.8 lub nowszym. Az interfejsu wiersza polecenia można sprawdzić wersji z `az --version` polecenia. Aby zaktualizować wersję interfejsu wiersza polecenia, zobacz [zainstalować Azure CLI 2.0]( /cli/azure/install-azure-cli).

Użyj `appId` wartości z nazwy głównej usługi utworzony wcześniej dla `aadClientId` parametru.

```azurecli 
az group deployment create --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --params \ 
        openshiftMasterPublicIpDnsLabel=myopenshiftmaster \
        infraLbPublicIpDnsLabel=myopenshiftlb \
        openshiftPassword=Pass@word!
        sshPublicKey=~/.ssh/openshift_rsa.pub \
        keyVaultResourceGroup=myResourceGroup \
        keyVaultName=myKeyVault \
        keyVaultSecret=OpenShiftKey \
        aadClientId={appId} \
        aadClientSecret={strong password} 
```
Wdrażanie może potrwać maksymalnie 20 minut. Adres URL konsoli OpenShift i nazwa DNS wzorca OpenShift drukowania na terminalu po zakończeniu wdrożenia.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ocpadmin@myopenshiftmaster.cloudapp.azure.com"
}
```
## <a name="connect-to-the-openshift-cluster"></a>Połącz się z klastrem OpenShift
Po zakończeniu wdrożenia łączenie z konsolą OpenShift przy użyciu przeglądarki `OpenShift Console Uri`. Alternatywnie można nawiązać wzorca OpenShift przy użyciu następującego polecenia.

```bash
$ ssh ocpadmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Gdy nie są już potrzebne, można użyć [usunięcie grupy az](/cli/azure/group#delete) polecenia, aby usunąć grupę zasobów, OpenShift klastra, a wszystkie powiązane zasoby.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

W przypadku tego samouczka, zapamiętanych jak:
> [!div class="checklist"]
> * Utwórz KeyVault do zarządzania kluczy SSH dla klastra OpenShift.
> * Wdrażanie klastra OpenShift na maszynach wirtualnych Azure. 
> * Instalowanie i konfigurowanie [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) do zarządzania klastrem.

Po wdrożeniu klastra OpenShift pochodzenia. Możesz wykonać OpenShift samouczkami, aby dowiedzieć się, jak wdrażanie pierwszej aplikacji i korzystania z narzędzi OpenShift. Zobacz [wprowadzenie pochodzenia OpenShift](https://docs.openshift.org/latest/getting_started/index.html) rozpocząć pracę. 
