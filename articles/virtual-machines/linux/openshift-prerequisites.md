---
title: "OpenShift w wymagania wstępne platformy Azure | Dokumentacja firmy Microsoft"
description: "Wymagania wstępne dotyczące wdrażania OpenShift na platformie Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 0c90b8a6d17fa293b6708d942afd35e1333623cb
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="common-prerequisites-for-openshift-in-azure"></a>Typowe wymagania wstępne dotyczące OpenShift na platformie Azure

W przypadku wdrażania OpenShift na platformie Azure, istnieje kilka typowych wstępne niezależnie od tego, czy wdrażanie pochodzenia OpenShift lub OpenShift kontenera platformy.

Instalacja OpenShift odbywa się za pośrednictwem Ansible playbooks. Ansible używa SSH do nawiązania połączenia wszystkich hostów, które będzie częścią klastra, aby wykonać kroki instalacji.
Po zainicjowaniu połączenia SSH do hostów zdalnych, nie istnieje sposób wprowadzić hasło. Z tego powodu klucza prywatnego nie może mieć hasło skojarzone z nim lub wdrożenie zakończy się niepowodzeniem.
Ponieważ wszystkie maszyny wirtualne są wdrażane za pomocą szablonów Resource Manager, sam klucz publiczny jest używana do uzyskiwania dostępu do wszystkich maszyn wirtualnych. Potrzebujemy do dodania do maszyny Wirtualnej, który wykonuje wszystkie playbooks również odpowiedniego klucza prywatnego.
W tym celu bezpiecznie używamy magazynu kluczy Azure do przekazania klucza prywatnego do maszyny Wirtualnej.

Jeśli istnieje potrzeba magazynu trwałego dla kontenerów, potrzebne są trwałe woluminów (PV). Odwiedziny te należy wykonać kopię postaci magazynu trwałego. OpenShift obsługuje dyskach platformy Azure (VHD) dla tej funkcji, ale Azure musi być skonfigurowany jako dostawcy chmury. W tym modelu OpenShift następujące czynności:

- Utwórz obiekt wirtualnego dysku twardego na koncie magazynu Azure
- Instalowanie wirtualnego dysku twardego do maszyny Wirtualnej i formatowanie woluminu
- Zainstalować woluminu na Pod

Aby to zrobić OpenShift wymaga uprawnień do wykonania poprzednich zadań na platformie Azure. Aby to osiągnąć, nazwy głównej usługi jest wymagana. Główna usługi (SP) jest kontem zabezpieczeń w usłudze Azure Active Directory, któremu przyznano uprawnienia do zasobów.
Nazwy głównej usługi musi mieć dostęp do konta magazynu i maszyny wirtualne wchodzące w skład klastra. W przypadku wdrożenia wszystkich zasobów klastra OpenShift do jednej grupy zasobów PS można udzielać uprawnień do tej grupy zasobów.

Ten przewodnik zawiera opis sposobu tworzenia artefaktów związanych z wstępne.

> [!div class="checklist"]
> * Utwórz KeyVault do zarządzania kluczy SSH dla klastra OpenShift.
> * Tworzenie nazwy głównej usługi do użytku przez dostawcę chmury Azure.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure. 
Zaloguj się do Twojej subskrypcji platformy Azure z [az logowania](/cli/azure/#login) polecenia i wykonaj na ekranie kliknij lub kierunki **wypróbuj** użycia chmury powłoki.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. Zaleca się, że grupa zasobów dedykowanych służy do obsługiwania Key Vault — niezależnie od zasobów klastra OpenShift zostaną wdrożone do grupy zasobów. 

Poniższy przykład tworzy grupę zasobów o nazwie *keyvaultrg* w *eastus* lokalizacji.

```azurecli 
az group create --name keyvaultrg --location eastus
```

## <a name="create-a-key-vault"></a>Tworzenie magazynu kluczy
Utwórz KeyVault do przechowywania kluczy SSH dla klastra z [az keyvault utworzyć](/cli/azure/keyvault#create) polecenia. Nazwa magazynu kluczy musi być globalnie unikatowa.

Poniższy przykład tworzy keyvault, o nazwie *keyvault* w *keyvaultrg* grupy zasobów.

```azurecli 
az keyvault create --resource-group keyvaultrg --name keyvault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>Tworzenie klucza SSH 
Klucz SSH jest wymagany do bezpiecznego dostępu do klastra OpenShift pochodzenia. Utwórz parę kluczy SSH przy użyciu `ssh-keygen` polecenia (w systemie Linux lub Mac.).
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> Pary kluczy SSH, którą utworzysz nie może zawierać hasło.

Aby uzyskać więcej informacji na temat kluczy SSH w systemie Windows [tworzenie SSH kluczy w systemie Windows](/azure/virtual-machines/linux/ssh-from-windows).

## <a name="store-ssh-private-key-in-key-vault"></a>Przechowywanie klucza prywatnego SSH w magazynie kluczy
Wdrożenie OpenShift używa klucza SSH, utworzenia bezpiecznego dostępu do głównego OpenShift. Aby włączyć wdrożenie w celu bezpiecznego pobierania klucza SSH, należy przechowywać klucz w Key Vault za pomocą następującego polecenia:

```azurecli
az keyvault secret set --vault-name keyvault --name keysecret --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a>Tworzenie nazwy głównej usługi 
OpenShift komunikuje się z platformy Azure przy użyciu nazwy użytkownika i hasła lub nazwy głównej usługi. Podmiot zabezpieczeń usługi Azure jest tożsamość zabezpieczeń korzystających z aplikacji, usługami i automatyzacja takich narzędzi jak OpenShift. Kontrolowanie i definiowanie uprawnień określające, jakie operacje nazwy głównej usługi można wykonać na platformie Azure. Zwiększające bezpieczeństwo za pośrednictwem tylko podanie nazwy użytkownika i hasła, w tym przykładzie tworzy podstawowe usługę podmiotu zabezpieczeń.

Utwórz usługę podmiotu zabezpieczeń z [az ad sp utworzyć do rbac](/cli/azure/ad/sp#create-for-rbac) i poświadczenia, które wymaga OpenShift output.

Poniższy przykład tworzy usługę podmiotu zabezpieczeń i przypisuje go uprawnienia współautora do grupy zasobów o nazwie myResourceGroup. Jeśli przy użyciu systemu Windows, należy wykonać ```az group show --name myResourceGroup --query id``` oddzielnie i użyć danych wyjściowych ze źródłem opcja zakresów.

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {Strong Password} \
          --scopes $(az group show --name myResourceGroup --query id)
```

Zwróć uwagę na właściwość appId zwrócone przez polecenie.
```json
{
  "appId": "11111111-abcd-1234-efgh-111111111111",            
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {Strong Password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > Nie twórz niezabezpieczonego hasła.  Postępuj zgodnie ze wskazówkami zawartymi w [ograniczeniach i regułach dotyczących hasła usługi Azure AD](/azure/active-directory/active-directory-passwords-policy).

Aby uzyskać więcej informacji dotyczących nazwy główne usług, zobacz [Tworzenie nazwy głównej usługi platformy Azure z 2.0 interfejsu wiersza polecenia platformy Azure](/cli/azure/create-an-azure-service-principal-azure-cli)

## <a name="next-steps"></a>Następne kroki

Ten artykuł obejmuje następujące tematy:
> [!div class="checklist"]
> * Utwórz KeyVault do zarządzania kluczy SSH dla klastra OpenShift.
> * Tworzenie nazwy głównej usługi do użytku przez dostawcę chmury Azure.

Teraz przejdź wdrożenie klastra OpenShift

- [Wdrażanie OpenShift źródła](./openshift-origin.md)
- [Wdrażanie OpenShift kontenera platformy](./openshift-container-platform.md)

