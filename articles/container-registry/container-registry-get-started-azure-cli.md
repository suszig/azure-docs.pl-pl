---
title: "Tworzenie prywatnego rejestru kontenerów platformy Docker — interfejs wiersza polecenia platformy Azure | Microsoft Doc"
description: "Rozpoczynanie pracy z tworzeniem prywatnych rejestrów kontenerów platformy Docker za pomocą interfejsu wiersza polecenia platformy Azure w wersji 2.0 i zarządzaniem nimi"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/03/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: e37a3194bb65ccf3bb6168a2f456902a9c48edc5
ms.lasthandoff: 03/28/2017

---
# <a name="create-a-private-docker-container-registry-using-the-azure-cli-20"></a>Tworzenie prywatnego rejestru kontenerów platformy Docker za pomocą interfejsu wiersza polecenia platformy Azure w wersji 2.0
Polecenia [interfejsu wiersza polecenia platformy Azure w wersji 2.0](https://github.com/Azure/azure-cli) służą do tworzenia rejestru kontenera i zarządzania jego ustawieniami z komputera z systemem Linux, Mac lub Windows. Tworzenie i zarządzanie rejestrami kontenerów jest także możliwe przy użyciu [witryny Azure Portal](container-registry-get-started-portal.md) lub programowo przy użyciu interfejsu [API REST](https://go.microsoft.com/fwlink/p/?linkid=834376) usługi Container Registry.


* Podstawy oraz pojęcia zostały przedstawione w części [ — omówienie](container-registry-intro.md)
* Aby uzyskać pomoc dotyczącą poleceń interfejsu wiersza polecenia usługi Container Registry (polecenia `az acr`), przekaż parametr `-h` do dowolnego polecenia.


## <a name="prerequisites"></a>Wymagania wstępne
* **Interfejs wiersza polecenia platformy Azure w wersji 2.0**: aby zainstalować interfejs wiersza polecenia platformy Azure w wersji 2.0 i rozpocząć pracę z nim, zobacz [instrukcje dotyczące instalacji](/cli/azure/install-azure-cli). Zaloguj się do subskrypcji platformy Azure, uruchamiając polecenie `az login`. Aby uzyskać więcej informacji, zobacz [Get started with the CLI 2.0](/cli/azure/get-started-with-azure-cli) (Rozpoczynanie pracy z interfejsem wiersza polecenia w wersji 2.0).
* **Grupa zasobów**: utwórz [grupę zasobów](../azure-resource-manager/resource-group-overview.md#resource-groups) przed utworzeniem rejestru kontenerów lub użyj istniejącej grupy zasobów. Upewnij się, że grupa zasobów znajduje się w lokalizacji, w której usługa Container Registry jest [dostępna](https://azure.microsoft.com/regions/services/). Aby utworzyć grupę zasobów przy użyciu interfejsu wiersza polecenia w wersji 2.0, zobacz [dokumentację dotyczącą interfejsu wiersza polecenia w wersji 2.0](/cli/azure/group).
* **Konto magazynu** (opcjonalnie): utwórz standardowe [konto magazynu](../storage/storage-introduction.md) platformy Azure, aby obsługiwać rejestr kontenerów w tej samej lokalizacji. Jeśli nie określisz konta magazynu podczas tworzenia rejestru przy użyciu polecenia `az acr create`, polecenie spowoduje utworzenie go dla Ciebie. Aby utworzyć konto magazynu przy użyciu interfejsu wiersza polecenia w wersji 2.0, zobacz [dokumentację dotyczącą interfejsu wiersza polecenia w wersji 2.0](/cli/azure/storage/account). Usługa Premium Storage nie jest obecnie obsługiwana.
* **Nazwa główna usługi** (opcjonalnie): rejestr utworzony za pomocą interfejsu wiersza polecenia domyślnie nie jest konfigurowany do uzyskiwania dostępu. W zależności od potrzeb możesz przypisać istniejącą nazwę główną usługi Azure Active Directory do rejestru (lub utworzyć i przypisać nową) albo włączyć konto administratora w rejestrze. Zobacz sekcje w dalszej części tego artykułu. Więcej informacji dotyczących dostępu do rejestru znajduje się w temacie [Authenticate with a container registry](container-registry-authentication.md) (Uwierzytelnianie za pomocą rejestru kontenera).

## <a name="create-a-container-registry"></a>Tworzenie rejestru kontenerów
Uruchom polecenie `az acr create`, aby utworzyć rejestr kontenera.

> [!TIP]
> Podczas tworzenia rejestru określ globalnie unikatową nazwę domeny najwyższego poziomu, która będzie zawierać tylko litery i cyfry. W przykładach nazwa rejestru to `myRegistry1`, ale możesz ją zastąpić własną, unikatową nazwą.
>
>

Następujące polecenie używa minimalnej liczby parametrów do utworzenia kontenera rejestru `myRegistry1` w grupie zasobów `myResourceGroup` w lokalizacji Południowo-środkowe stany USA:

```azurecli
az acr create -n myRegistry1 -g myResourceGroup -l southcentralus
```

* Parametr `--storage-account-name` jest opcjonalny. Jeśli nie określono inaczej, nazwa konta magazynu utworzonego we wskazanej grupie zasobów składa się z nazwy rejestru i sygnatury czasowej.

Dane wyjściowe będą podobne do następujących:

![az acr — tworzenie danych wyjściowych](./media/container-registry-get-started-azure-cli/acr_create.png)


Zwróć szczególną uwagę na następujące elementy:

* `id` — identyfikator rejestru w ramach subskrypcji, którego potrzebujesz do przypisania nazwy głównej usługi.
* `loginServer` — w pełni kwalifikowana nazwa służąca do [logowania do rejestru](container-registry-authentication.md). W tym przykładzie nazwa to `myregistry1.exp.azurecr.io` (tylko małe litery).

## <a name="assign-a-service-principal"></a>Przypisywanie nazwy głównej usługi
Aby przypisać nazwę główną usługi Azure Active Directory do rejestru, użyj poleceń interfejsu wiersza polecenia w wersji 2.0. W tych przykładach do nazwy głównej usługi przypisano rolę Właściciel, ale możesz przypisywać [inne role](../active-directory/role-based-access-control-configure.md).

### <a name="create-a-service-principal-and-assign-access-to-the-registry"></a>Tworzenie nazwy głównej usługi i przypisywanie dostępu do rejestru
W poniższym poleceniu do nowej nazwy głównej usługi jest przypisywany dostęp roli Właściciel do identyfikatora rejestru przy użyciu parametru `--scopes`. Określ silne hasło przy użyciu parametru `--password`.

```azurecli
az ad sp create-for-rbac --scopes /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --password myPassword
```



### <a name="assign-an-existing-service-principal"></a>Przypisywanie istniejącej nazwy głównej usługi
Jeśli już masz nazwę główną usługi i chcesz do niej przypisać dostęp roli Właściciel do rejestru, uruchom polecenie podobne do poniższego przykładu. Identyfikator aplikacji nazwy głównej usługi jest przekazywany przy użyciu parametru `--assignee`:

```azurecli
az role assignment create --scope /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --assignee myAppId
```



## <a name="manage-admin-credentials"></a>Zarządzanie poświadczeniami administratora
Konto administratora jest automatycznie tworzone dla każdego rejestru kontenera i jest domyślnie wyłączone. W poniższych przykładach pokazano polecenia interfejsu wiersza polecenia `az acr` służące do zarządzania poświadczeniami administratora dla rejestru kontenera.

### <a name="obtain-admin-user-credentials"></a>Uzyskiwanie poświadczeń użytkownika administratora
```azurecli
az acr credential show -n myRegistry1
```

### <a name="enable-admin-user-for-an-existing-registry"></a>Włączanie użytkownika administratora dla istniejącego rejestru
```azurecli
az acr update -n myRegistry1 --admin-enabled true
```

### <a name="disable-admin-user-for-an-existing-registry"></a>Wyłączanie użytkownika administratora dla istniejącego rejestru
```azurecli
az acr update -n myRegistry1 --admin-enabled false
```

## <a name="list-images-and-tags"></a>Tworzenie listy obrazów i tagów
Użyj poleceń interfejsu wiersza polecenia `az acr` do tworzenia zapytań dotyczących obrazów i tagów w repozytorium.

> [!NOTE]
> Obecnie usługa Container Registry nie obsługuje tworzenia zapytań dotyczących obrazów i tagów przy użyciu polecenia `docker search`.


### <a name="list-repositories"></a>Tworzenie listy repozytoriów
W poniższym przykładzie przedstawiono listę repozytoriów w rejestrze w formacie JSON (JavaScript Object Notation):

```azurecli
az acr repository list -n myRegistry1 -o json
```

### <a name="list-tags"></a>Tworzenie listy tagów
W poniższym przykładzie przedstawiono listę tagów w repozytorium **samples/nginx** w formacie JSON:

```azurecli
az acr repository show-tags -n myRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Następne kroki
* [Push your first image using the Docker CLI](container-registry-get-started-docker-cli.md) (Wypychanie pierwszego obrazu za pomocą interfejsu wiersza polecenia platformy Docker)

