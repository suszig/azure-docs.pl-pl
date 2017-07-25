---
title: "Tworzenie prywatnego rejestru platformy Docker — witryna Azure Portal | Microsoft Doc"
description: "Rozpoczynanie pracy z tworzeniem prywatnych rejestrów kontenerów platformy Docker za pomocą witryny Azure Portal i zarządzaniem nimi"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 560aee42b0c5a61c37c594d7937f833ab7183d49
ms.contentlocale: pl-pl
ms.lasthandoff: 07/12/2017

---

# <a name="create-a-managed-container-registry-using-the-azure-portal"></a>Tworzenie zarządzanego rejestru kontenerów przy użyciu witryny Azure Portal

Usługa Azure Container Registry to zarządzana usługa rejestru kontenerów platformy Docker używana do przechowywania prywatnych obrazów kontenerów Docker. W tym przewodniku znajdują się szczegółowe informacje dotyczące tworzenia zarządzanego wystąpienia usługi Azure Container Registry przy użyciu witryny Azure Portal.

Zarządzane rejestry kontenerów platformy Azure są w wersji zapoznawczej i nie są dostępne we wszystkich regionach.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="create-a-container-registry"></a>Tworzenie rejestru kontenerów

1. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.

2. W portalu Marketplace wyszukaj usługę **Azure Container Registry** i wybierz ją.

3. Kliknij pozycję **Utwórz**, aby otworzyć blok tworzenia usługi ACR.

    ![Ustawienia usługi Container Registry](./media/container-registry-get-started-portal/managed-container-registry-settings.png)

4. W bloku usługi **Azure Container Registry** wprowadź następujące informacje. Po zakończeniu kliknij przycisk **Utwórz**.

    a. **Nazwa rejestru**: globalnie unikatowa nazwa domeny najwyższego poziomu dla określonego rejestru. W tym przykładzie nazwa rejestru to *myAzureContainerRegistry1*, ale zastąp ją własną unikatową nazwą. Nazwa może zawierać tylko litery i cyfry.

    b. **Grupa zasobów**: wybierz istniejącą [grupę zasobów](../azure-resource-manager/resource-group-overview.md#resource-groups) lub wprowadź nazwę nowej grupy zasobów.

    c. **Lokalizacja**: wybierz taką lokalizację centrum danych Azure, gdzie usługa jest [dostępna](https://azure.microsoft.com/regions/services/), na przykład **Południowo-środkowe stany USA**.

    d. **Administrator**: jeśli chcesz, umożliw administratorowi dostęp do rejestru. Po utworzeniu rejestru można zmienić to ustawienie.

    e. **Użyj rejestru zarządzanego**: wybierz opcję Tak, aby usługa ACR automatycznie zarządzała magazynem rejestru, korzystała z elementów webhook i używała uwierzytelniania w usłudze AAD.

    f. **Warstwa cenowa**: wybierz warstwę cenową; zobacz cennik usługi ACR, aby uzyskać więcej informacji.

## <a name="log-in-to-acr-instance"></a>Logowanie do wystąpienia usługi ACR

Przed wypychaniem i ściąganiem obrazów kontenerów musisz zalogować się do wystąpienia usługi ACR. 

Aby to zrobić, użyj interfejsu wiersza polecenia platformy Azure 2.0. Najpierw, w razie potrzeby, zaloguj się do platformy Azure za pomocą polecenia [az login](/cli/azure/#login). 

```azurecli
az login
```

Następnie użyj polecenia [az acr login](/cli/azure/acr#login), aby zalogować się do usługi Azure Container Registry.

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

## <a name="use-azure-container-registry"></a>Korzystanie z usługi Azure Container Registry

### <a name="list-container-images"></a>Tworzenie listy obrazów kontenerów

Użyj poleceń interfejsu wiersza polecenia `az acr` do tworzenia zapytań dotyczących obrazów i tagów w repozytorium.

> [!NOTE]
> Obecnie usługa Container Registry nie obsługuje tworzenia zapytań dotyczących obrazów i tagów przy użyciu polecenia `docker search`.

### <a name="list-repositories"></a>Tworzenie listy repozytoriów

W poniższym przykładzie przedstawiono listę repozytoriów w rejestrze w formacie JSON (JavaScript Object Notation):

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a>Tworzenie listy tagów

W poniższym przykładzie przedstawiono listę tagów w repozytorium **samples/nginx** w formacie JSON:

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start utworzono zarządzane wystąpienie usługi Azure Container Registry za pomocą witryny Azure Portal.

> [!div class="nextstepaction"]
> [Push your first image using the Docker CLI](container-registry-get-started-docker-cli.md) (Wypychanie pierwszego obrazu za pomocą interfejsu wiersza polecenia platformy Docker)
