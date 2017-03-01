---
title: "Nazwa główna usługi klastra Azure Kubernetes | Dokumentacja firmy Microsoft"
description: "Tworzenie i zarządzanie nazwą główną usługi Azure Active Directory w klastrze usługi Azure Container Service z funkcją Kubernetes"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/21/2017
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: 71c6c5ffacf49b907e3e9f488789f31928b25823
ms.openlocfilehash: e01a9ef7d223e7a5a06475cf419b73959baa803f


---

# <a name="about-the-azure-active-directory-service-principal-for-a-kubernetes-cluster-in-azure-container-service"></a>Temat dotyczy nazwy głównej usługi Azure Active Directory dla klastra Kubernetes w usłudze Azure Container Service



W usłudze Azure Container Service klaster Kubernetes wymaga [nazwy głównej usługi Azure Active Directory](../active-directory/active-directory-application-objects.md) jako konta usługi do współpracy z interfejsami API systemu Azure. Nazwa główna usługi jest potrzebna do dynamicznego zarządzania zasobami, takimi jak trasy zdefiniowane przez użytkownika i narzędzie Azure Load Balancer dla warstwy 4.

W tym artykule przedstawiono różne sposoby określania nazwy głównej usługi dla klastra Kubernetes. Na przykład jeśli zainstalowano i skonfigurowano [interfejs wiersza polecenia platformy Azure w wersji 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2), można uruchomić polecenie [`az acs create`](https://docs.microsoft.com/en-us/cli/azure/acs#create), aby jednocześnie utworzyć klaster Kubernetes i nazwę główną usługi.



## <a name="requirements-for-the-service-principal"></a>Wymagania dotyczące nazwy głównej usługi

Poniżej przedstawiono wymagania dotyczące nazwy głównej usługi Azure Active Directory dla klastra Kubernetes w usłudze Azure Container Service. 

* **Zakres**: grupa zasobów, w której jest wdrażany klaster

* **Rola**: **Współautor**

* **Wpis tajny klienta**: musi to być hasło. Obecnie nie można używać nazwy głównej usługi do uwierzytelniania certyfikatu.

> [!NOTE]
> Każda nazwa główna usługi jest skojarzona z aplikacją usługi Azure Active Directory. Nazwa główna usługi klastra Kubernetes może zostać skojarzona z dowolną prawidłową nazwą aplikacji usługi Azure Active Directory.
> 


## <a name="service-principal-options-for-a-kubernetes-cluster"></a>Opcje nazwy głównej usługi dla klastra Kubernetes

### <a name="option-1-pass-the-service-principal-client-id-and-client-secret"></a>Opcja 1: Przekaż identyfikator klienta nazwy głównej usługi i klucz tajny klienta

Podaj **identyfikator klienta** (nazywany również `appId`, identyfikator aplikacji) i **wpis tajny klienta** (`password`) istniejącej nazwy głównej usługi jako parametry podczas tworzenia klastra Kubernetes. Jeśli korzystasz z istniejącej nazwy głównej usługi, upewnij się, że spełnione są wymagania z poprzedniej sekcji. Aby utworzyć nazwę główną usługi, odwiedź stronę [Tworzenie nazwy głównej usługi](#create-a-service-principal-in-azure-active-directory) w dalszej części tego artykułu.

Te parametry można określić podczas [wdrażanie klastra Kubernetes](./container-service-deployment.md) za pomocą portalu, interfejsu wiersza polecenia platformy Azure w wersji 2.0, programu Azure PowerShell lub innymi metodami.

>[!TIP] 
>Podczas określania **identyfikatora klienta** należy użyć identyfikatora `appId`, a nie `ObjectId` nazwy głównej usługi.
>

Poniższy przykład przedstawia sposób przekazania parametrów przez interfejs wiersza polecenia platformy Azure w wersji 2.0 (zobacz [instrukcje instalacji i konfiguracji](/cli/azure/install-az-cli2)). W tym przykładzie użyto [szablonu Kubernetes quickstart](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).

1. [Pobierz](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) plik parametrów szablonu `azuredeploy.parameters.json` z usługi GitHub.

2. Aby określić nazwę główną usługi, wprowadź wartości dla `servicePrincipalClientId` i `servicePrincipalClientSecret` w pliku. (Należy również podać własne wartości dla `dnsNamePrefix` i `sshRSAPublicKey`. Ta ostatnia jest kluczem publicznym SSH umożliwiającym dostęp do klastra). Zapisz plik.

    ![Przekazywanie parametrów nazwy głównej usługi](./media/container-service-kubernetes-service-principal/service-principal-params.png)

3. Przy użyciu opcji `--parameters` uruchom następujące polecenie, aby ustawić ścieżkę do pliku azuredeploy.parameters.json. To polecenie wdraża klaster w utworzonej grupie zasobów o nazwie `myResourceGroup` w regionie Zachodnie stany USA.

    ```azurecli
    az login

    az account set --subscription "mySubscriptionID"

    az group create --name "myResourceGroup" --location "westus" 
    
    az group deployment create -g "myResourceGroup" --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.json" --parameters @azuredeploy.parameters.json
    ```


### <a name="option-2-generate-the-service-principal-when-creating-the-cluster-with-the-azure-cli-20"></a>Opcja 2: Wygeneruj nazwę główną usługi podczas tworzenia klastra przy użyciu interfejsu wiersza polecenia platformy Azure 2.0

Jeśli zainstalowano i skonfigurowano [interfejs wiersza polecenia platformy Azure w wersji 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2), można uruchomić polecenie [`az acs create`](https://docs.microsoft.com/en-us/cli/azure/acs#create) w celu [utworzenia klastra](./container-service-create-acs-cluster-cli.md).

Tak jak w przypadku innych opcji tworzenia klastra Kubernetes, parametry istniejącej nazwy głównej usługi można określić po uruchomieniu polecenia `az acs create`. Jednak w przypadku pominięcia tych parametrów usługa Azure Container Service tworzy automatycznie nazwę główną usługi. Podczas wdrażania dzieje się to w sposób niewidoczny dla użytkownika. 

Następujące polecenie tworzy klaster Kubernetes i generuje zarówno klucze SSH, jak i poświadczenia nazwy głównej usługi:

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

## <a name="create-a-service-principal-in-azure-active-directory"></a>Tworzenie nazwy głównej usługi w usłudze Azure Active Directory

Jeśli chcesz utworzyć nazwę główną usługi w usłudze Azure Active Directory przeznaczoną do używania w klastrze Kubernetes, system Azure udostępnia kilka metod. 

Następujące przykładowe polecenia pokazują, jak wykonać to za pomocą interfejsu [wiersza polecenia platformy Azure w wersji 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2). Można też utworzyć nazwę główną usługi za pomocą programu [Azure PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md), [portalu klasycznego](../azure-resource-manager/resource-group-create-service-principal-portal.md) lub innej metody.

> [!IMPORTANT]
> Upewnij się, że zostały spełnione wymagania dotyczące nazwy głównej usługi przedstawione wcześniej w tym artykule.
>

```azurecli
az login

az account set --subscription "mySubscriptionID"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID"
```

Zwrócone dane wyjściowe powinny przypominać następujące (w postaci zredagowanej):

![Tworzenie nazwy głównej usługi](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

Wyróżniono **identyfikator klienta** (`appId`) i **klucz tajny klienta** (`password`), używane jako parametry nazwy głównej usługi przy wdrażaniu klastra.


Potwierdź swoją nazwę główną usługi przez otwarcie nowej powłoki i uruchomienie następującego polecenia, zastępując pozycje `appId`, `password` i `tenant`:

```azurecli 
az login --service-principal -u yourClientID -p yourClientSecret --tenant yourTenant

az vm list-sizes --location westus
```

## <a name="additional-considerations"></a>Dodatkowe zagadnienia


* Podczas określania **identyfikatora klienta** nazwy głównej usługi można użyć wartości `appId` (jak pokazano w tym artykule) lub odpowiedniej nazwy głównej usługi `name` (na przykład `https://www.contoso.org/example`).

* Jeśli używasz polecenia `az acs create`, aby automatycznie wygenerować nazwę główną usługi, poświadczenia nazwy głównej usługi są zapisywane w pliku ~/.azure/acsServicePrincipal.json na komputerze użytym do uruchomienia polecenia.

* Na głównej maszynie wirtualnej i maszynach wirtualnych węzłów w klastrze Kubernetes poświadczenia nazwy głównej usługi są przechowywane w pliku /etc/kubernetes/azure.json.

## <a name="next-steps"></a>Następne kroki

* [Rozpocznij pracę z klastrem Kubernetes](container-service-kubernetes-walkthrough.md) w klastrze usług kontenera.



<!--HONumber=Feb17_HO4-->


