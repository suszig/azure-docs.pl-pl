---
title: "Szybki start — Tworzenie pierwszego kontenera usługi Azure Container Instances"
description: "Wdrażaj i rozpocznij pracę z usługą Azure Container Instances"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: quickstart
ms.date: 02/22/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: f8fe53f834e4fcf7f16174222cb51d89e40305ec
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/24/2018
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Tworzenie pierwszego kontenera w usłudze Azure Container Instances
Usługa Azure Container Instances ułatwia tworzenie kontenerów Docker na platformie Azure oraz zarządzanie nimi bez konieczności inicjowania obsługi maszyn wirtualnych czy adoptowania usługi wyższego poziomu. Podczas pracy z tym przewodnikiem Szybki start utworzysz kontener na platformie Azure i ujawnisz go w Internecie przy użyciu w pełni kwalifikowanej nazwy domeny. Ta operacja jest wykonywana za pomocą jednego polecenia. W ciągu kilku sekund w przeglądarce zobaczysz następujący wynik:

![Widziana w przeglądarce aplikacja wdrożona za pomocą usługi Azure Container Instances][aci-app-browser]

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto][azure-account].

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Podczas pracy z tym przewodnikiem Szybki start możesz użyć usługi Azure Cloud Shell lub lokalnej instalacji interfejsu wiersza polecenia platformy Azure. Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten przewodnik Szybki start będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.27 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0][azure-cli-install].

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Wystąpienia Azure Container Instances są zasobami platformy Azure i należy je umieścić w grupie zasobów platformy Azure, czyli logicznej kolekcji, w której zasoby platformy Azure są wdrażane i zarządzane.

Utwórz grupę zasobów za pomocą polecenia [az group create][az-group-create].

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Tworzenie kontenera

Kontener można utworzyć, podając nazwę obrazu usługi Docker i grupy zasobów platformy Azure w poleceniu [az container create][az-container-create]. Opcjonalnie można ujawnić kontener w Internecie, określając etykietę nazwy DNS. W tym przewodniku Szybki start wdrożysz kontener hostujący małą aplikację internetową napisaną w języku [Node.js][node-js].

Wykonaj poniższe polecenie, aby uruchomić wystąpienie kontenera. Wartość `--dns-name-label` musi być unikatowa w regionie platformy Azure, w którym tworzysz wystąpienie, dlatego być może trzeba będzie zmodyfikować tę wartość w celu zapewnienia unikatowości.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --dns-name-label aci-demo --ports 80
```

W ciągu kilku sekund powinna pojawić się odpowiedź na żądanie. Początkowo kontener ma stan **Tworzenie**, ale powinien zostać uruchomiony w ciągu kilku sekund. Stan możesz sprawdzić przy użyciu polecenia [az container show][az-container-show]:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
```

Po uruchomieniu polecenia zostanie wyświetlona w pełni kwalifikowana nazwa domeny (FQDN) kontenera i stan jego aprowizacji:

```console
$ az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
FQDN                               ProvisioningState
---------------------------------  -------------------
aci-demo.eastus.azurecontainer.io  Succeeded
```

Po przejściu kontenera do stanu **Powodzenie** możesz uzyskiwać dostęp go niego w przeglądarce, przechodząc do jego nazwy FQDN:

![Zrzut ekranu przedstawiający aplikację uruchomioną w wystąpieniu kontenera platformy Azure][aci-app-browser]

## <a name="pull-the-container-logs"></a>Ściąganie dzienników kontenera

Dzienniki dla utworzonego kontenera możesz ściągnąć przy użyciu polecenia [az container logs][az-container-logs]:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Dane wyjściowe:

```bash
listening on port 80
::ffff:10.240.255.107 - - [29/Nov/2017:20:48:50 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36"
::ffff:10.240.255.107 - - [29/Nov/2017:20:48:50 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://52.224.178.107/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36"
```

## <a name="delete-the-container"></a>Usuwanie kontenera

Po zakończeniu pracy z kontenerem możesz go usunąć przy użyciu polecenia [az container delete][az-container-delete]:

```azurecli-interactive
az container delete --resource-group myResourceGroup --name mycontainer
```

Aby się upewnić, że kontener został usunięty, uruchom polecenie [az container list](/cli/azure/container#az_container_list):

```azurecli-interactive
az container list --resource-group myResourceGroup --output table
```

W danych wyjściowych polecenia nie powinien pojawić się kontener **mycontainer**. Jeśli nie ma innych kontenerów w grupie zasobów, nie powinny zostać wyświetlone żadne dane wyjściowe.

## <a name="next-steps"></a>Następne kroki

Cały kod dla kontenera, użyty w tym przewodniku Szybki start, jest dostępny [w serwisie GitHub][app-github-repo] wraz z odpowiednim plikiem Dockerfile. Jeśli chcesz spróbować samodzielnie skompilować kontener i wdrożyć go w usłudze Azure Container Instances za pomocą usługi Azure Container Registry, przejdź do samouczka dotyczącego usługi Azure Container Instances.

> [!div class="nextstepaction"]
> [Samouczki dotyczące usługi Azure Container Instances](./container-instances-tutorial-prepare-app.md)

Aby wypróbować opcje uruchamiania kontenerów w systemie aranżacji na platformie Azure, zobacz przewodnik Szybki start [Service Fabric][service-fabric] lub [Azure Container Service (AKS)][container-service].

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - External -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[azure-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[node-js]: http://nodejs.org

<!-- LINKS - Internal -->
[az-group-create]: /cli/azure/group?view=azure-cli-latest#az_group_create
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create
[az-container-delete]: /cli/azure/container?view=azure-cli-latest#az_container_delete
[az-container-list]: /cli/azure/container?view=azure-cli-latest#az_container_list
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az_container_logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show
[azure-cli-install]: /cli/azure/install-azure-cli
[container-service]: ../aks/kubernetes-walkthrough.md
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
