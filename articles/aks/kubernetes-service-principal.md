---
title: "Jednostka usługi klastra Azure Kubernetes"
description: "Tworzenie jednostki usługi Azure Active Directory dla klastra Kubernetes w usłudze AKS i zarządzanie nią"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: get-started-article
ms.date: 11/30/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 9814dca53f1a410f4d1e95cc18b98373f27f9802
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="service-principals-with-azure-container-service-aks"></a>Jednostka usługi Azure Container Service (AKS)

Klaster AKS wymaga [jednostki usługi Azure Active Directory][aad-service-principal] do współpracy z interfejsami API platformy Azure. Jednostka usługi jest potrzebna do dynamicznego zarządzania zasobami, takimi jak [trasy zdefiniowane przez użytkownika][user-defined-routes] i narzędzie [Azure Load Balancer dla warstwy 4][azure-load-balancer-overview].

W tym artykule przedstawiono różne sposoby konfigurowania jednostki usługi dla własnego klastra Kubernetes w usłudze AKS.

## <a name="before-you-begin"></a>Przed rozpoczęciem


Aby utworzyć jednostkę usługi Azure AD, musisz mieć uprawnienia do zarejestrowania aplikacji w swojej dzierżawie usługi Azure AD i przypisania aplikacji do roli w swojej subskrypcji. Jeśli nie masz niezbędnych uprawnień, może być konieczne zwrócenie się z prośbą do administratora usługi Azure AD lub subskrypcji, aby przyznał niezbędne uprawnienia, lub o wstępne utworzenie jednostki usługi dla klastra Kubernetes.

Musisz również mieć zainstalowany i skonfigurowany interfejs wiersza polecenia platformy Azure w wersji 2.0.21 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure][install-azure-cli].

## <a name="create-sp-with-aks-cluster"></a>Utwórz jednostkę usługi z klastrem usługi AKS

Podczas wdrażania klastra usługi AKS za pomocą polecenia `az aks create` masz możliwość automatycznego wygenerowania jednostki usługi.

W następującym przykładzie tworzony jest klaster AKS. Ponieważ nie podano istniejącej jednostki usługi, dla klastra jest tworzona jednostka usługi. Aby można było ukończyć tę operację, Twoje konto musi mieć odpowiednie uprawnienia do tworzenia jednostki usługi.

```azurecli
az aks create --name myK8SCluster --resource-group myResourceGroup --generate-ssh-keys
```

## <a name="use-an-existing-sp"></a>Używanie istniejącej jednostki usługi

Można użyć istniejącej jednostki usługi Azure AD lub wstępnie ją utworzyć do użycia z klastrem usługi AKS. Jest to przydatne w przypadku wdrażania klastra z witryny Azure Portal, gdzie jest wymagane podanie informacji o jednostce usługi. W przypadku korzystania z istniejącej jednostki usługi jako hasło należy skonfigurować klucz tajny klienta.

## <a name="pre-create-a-new-sp"></a>Wstępne tworzenie nowej jednostki usługi

Użyj polecenia [az ad sp create-for-rbac][az-ad-sp-create], aby utworzyć jednostkę usługi przy użyciu interfejsu wiersza polecenia platformy Azure.

```azurecli
az ad sp create-for-rbac --skip-assignment
```

Dane wyjściowe będą podobne do następujących. Zwróć uwagę na elementy `appId` i `password`. Te wartości są używane podczas tworzenia klastra usługi AKS.

```json
{
  "appId": "7248f250-0000-0000-0000-dbdeb8400d85",
  "displayName": "azure-cli-2017-10-15-02-20-15",
  "name": "http://azure-cli-2017-10-15-02-20-15",
  "password": "77851d2c-0000-0000-0000-cb3ebc97975a",
  "tenant": "72f988bf-0000-0000-0000-2d7cd011db47"
}
```

## <a name="use-an-existing-sp"></a>Używanie istniejącej jednostki usługi

Korzystając ze wstępnie utworzonej jednostki usługi, podaj `appId` i `password` jako wartości argumentu w poleceniu `az aks create`.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myK8SCluster --service-principal <appId> --client-secret <password>
```

W przypadku wdrażania klastra AKS przy użyciu witryny Azure Portal wprowadź wartość `appId` w polu **Identyfikator klienta jednostki usługi** i wartość `password` w polu **Klucz tajny klienta jednostki usługi** w formie konfiguracji klastra AKS.

![Obraz przedstawiający przechodzenie do aplikacji Azure Vote](media/container-service-kubernetes-service-principal/sp-portal.png)

## <a name="additional-considerations"></a>Dodatkowe zagadnienia

Podczas pracy z jednostkami usług AKS i Azure AD należy pamiętać o poniższym.

* Jednostka usługi dla rozwiązania Kubernetes jest częścią konfiguracji klastra. Nie należy jednak używać tożsamości do wdrażania klastra.
* Każda jednostka usługi jest skojarzona z aplikacją usługi Azure AD. Jednostka usługi dla klastra Kubernetes może zostać skojarzona z dowolną prawidłową nazwą aplikacji usługi Azure AD (np. `https://www.contoso.org/example`). Adres URL dla aplikacji nie musi być rzeczywistym punktem końcowym.
* Podczas określania **identyfikatora klienta** jednostki usługi można użyć wartości `appId` (jak pokazano w tym artykule) lub odpowiedniej jednostki usługi `name` (na przykład `https://www.contoso.org/example`).
* Na głównej maszynie wirtualnej i maszynach wirtualnych węzłów w klastrze Kubernetes poświadczenia jednostki usługi są przechowywane w pliku `/etc/kubernetes/azure.json`.
* Gdy używasz polecenia `az aks create`, aby automatycznie wygenerować jednostkę usługi, poświadczenia jednostki usługi są zapisywane w pliku `~/.azure/acsServicePrincipal.json` na maszynie użytej do uruchomienia polecenia.
* Kiedy używasz polecenia `az aks create` do automatycznego wygenerowania jednostki usługi, jednostka usługi może także uwierzytelnić się za pomocą [rejestru Azure Container Registry][acr-intro] utworzonego w tej samej subskrypcji.
* Usunięcie klastra AKS utworzonego za pomocą polecenia `az aks create` nie powoduje usunięcia automatycznie utworzonej jednostki usługi. Można ją usunąć za pomocą polecenia `az ad sp delete --id $clientID`.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat jednostek usługi Azure Active Directory, zobacz dokumentację aplikacji usługi Azure AD.

> [!div class="nextstepaction"]
> [Application and service principal objects (Obiekty aplikacji i jednostki usługi)][service-principal]

<!-- LINKS - internal -->
[aad-service-principal]: ../active-directory/develop/active-directory-application-objects.md
[acr-intro]: ../container-registry/container-registry-intro.md
[az-ad-sp-create]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[azure-load-balancer-overview]: ../load-balancer/load-balancer-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[service-principal]: ../active-directory/develop/active-directory-application-objects.md
[user-defined-routes]: ../load-balancer/load-balancer-overview.md