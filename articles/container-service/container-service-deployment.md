---
title: "Wdrażanie klastra kontenera platformy Docker na platformie Azure | Microsoft Docs"
description: "Wdrażanie klastra usługi Azure Container Service za pomocą witryny Azure Portal lub szablonu usługi Azure Resource Manager."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Mesos, Azure, dcos, swarm, kubernetes, azure container service, acs
ms.assetid: 696a736f-9299-4613-88c6-7177089cfc23
ms.service: container-service
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/21/2017
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 2a381431acb6436ddd8e13c69b05423a33cd4fa6
ms.openlocfilehash: b9be92498f9daf1d2f964cc689bacb2358b237be


---
# <a name="deploy-an-azure-container-service-cluster"></a>Wdrażanie klastra usługi kontenera platformy Azure



Usługa kontenera platformy Azure zapewnia szybkie wdrażanie popularnych rozwiązań typu open source służących do aranżowania i klastrowania kontenerów. Ten dokument zawiera opis kroków wdrażania klastra usługi Azure Container Service przy użyciu witryny Azure Portal lub szablonu szybkiego startu usługi Azure Resource Manager. 

Wdrożenie klastra usługi Azure Container Service jest również możliwe za pomocą [interfejsu wiersza polecenia Azure w wersji 2.0](container-service-create-acs-cluster-cli.md) lub interfejsów API usługi Azure Container Service.



## <a name="prerequisites"></a>Wymagania wstępne

* **Subskrypcja platformy Azure**: jeśli jej nie masz, możesz utworzyć konto [bezpłatnej wersji próbnej](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

* **Klucz publiczny SSH RSA**: podczas wdrażania za pośrednictwem portalu lub jednego z szablonów szybkiego startu platformy Azure musisz podać klucz publiczny na potrzeby uwierzytelniania na maszynach wirtualnych usługi Azure Container Service. Aby utworzyć klucze RSA Secure Shell (SSH), zobacz wskazówki dla systemów [OS X i Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) lub [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md). 

* **Klucz tajny i identyfikator klienta jednostki usługi** (tylko rozwiązanie Kubernetes): aby uzyskać więcej informacji i wskazówek dotyczących tworzenia jednostki usługi, zobacz [About the service principal for a Kubernetes cluster](container-service-kubernetes-service-principal.md) (Informacje o nazwie głównej usługi dla klastra Kubernetes).



## <a name="create-a-cluster-by-using-the-azure-portal"></a>Tworzenie klastra przy użyciu witryny Azure Portal
1. Zaloguj się w witrynie Azure Portal, wybierz pozycję **Nowy** i w portalu Azure Marketplace wyszukaj usługę **Azure Container Service**.

    ![Usługa Azure Container Service w portalu Marketplace](media/container-service-deployment/acs-portal1.png)  <br />

2. Wybierz pozycję **Azure Container Service** i kliknij przycisk **Utwórz**.

    ![Tworzenie usługi kontenera](media/container-service-deployment/acs-portal2.png)  <br />

3. Wprowadź następujące informacje:

    * **Nazwa użytkownika**: nazwa użytkownika dla konta na poszczególnych maszynach wirtualnych i zestawach skalowania maszyn wirtualnych w klastrze usługi Azure Container Service.
    * **Subskrypcja**: wybierz subskrypcję platformy Azure.
    * **Grupa zasobów**: wybierz istniejącą grupę zasobów lub utwórz nową. Najlepszym rozwiązaniem jest użycie dla każdego wdrożenia nowej grupy zasobów.
    * **Lokalizacja**: wybierz region platformy Azure dla wdrożenia usługi Azure Container Service.
    * **Klucz publiczny SSH RSA**: dodaj klucz publiczny, który będzie używany do uwierzytelniania względem maszyn wirtualnych usługi Azure Container Service. Ważne jest, aby ten klucz nie zawierał podziałów wierszy i miał prefiks `ssh-rsa`. Postfiks `username@domain` jest opcjonalny. Klucz powinien wyglądać podobnie do poniższego: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**. 

4. Aby kontynuować, kliknij przycisk **OK**.

    ![Ustawienia podstawowe](media/container-service-deployment/acs-portal3.png)  <br />

5. W bloku **Konfiguracja platformy** wybierz pozycję **Konfiguracja programu Orchestrator**. Dostępne są następujące opcje:

  * **DC/OS**: wdraża klaster DC/OS.
  * **Swarm**: wdraża klaster Docker Swarm.
  * **Kubernetes**: wdraża klaster Kubernetes.


6. Aby kontynuować, kliknij przycisk **OK**.

    ![Wybór koordynatora](media/container-service-deployment/acs-portal4-new.png)  <br />

7. Jeśli na liście rozwijanej wybrano wartość **Kubernetes**, należy wprowadzić identyfikator klienta nazwy głównej usługi (zwany również appId) oraz klucz tajny klienta jednostki usługi (hasło). Aby uzyskać więcej informacji, zobacz [About the service principal for a Kubernetes cluster](container-service-kubernetes-service-principal.md) (Informacje o nazwie głównej usługi dla klastra Kubernetes).

    ![Wprowadzanie nazwy głównej usługi Kubernetes](media/container-service-deployment/acs-portal10.png)  <br />

7. W bloku **ustawień usługi Azure Container Service** wprowadź następujące informacje:

    * **Liczba serwerów głównych**: liczba serwerów głównych w klastrze.
    * **Liczba agentów**: w przypadku opcji Docker Swarm lub Kubernetes ta wartość to początkowa liczba agentów w zestawie skalowania agenta. W przypadku opcji DC/OS jest to początkowa liczba agentów w prywatnym zestawie skalowania. Ponadto w przypadku koordynatora DC/OS jest tworzony publiczny zestaw skalowania zawierający wstępnie określoną liczbę agentów. Liczba agentów w tym publicznym zestawie skalowania zależy od liczby serwerów głównych utworzonych w klastrze: jeden agent publiczny dla jednego serwera głównego i dwa agenty publiczne dla trzech lub pięciu serwerów głównych.
    * **Rozmiar maszyny wirtualnej agenta**: rozmiar maszyn wirtualnych agenta.
    * **Prefiks DNS**: unikatowa w zakresie globalnym nazwa, która służy jako prefiks kluczowych części w pełni kwalifikowanych nazw domen dla usługi.
    * **Diagnostyka maszyn wirtualnych**: w przypadku niektórych koordynatorów można wybrać opcję włączenia diagnostyki maszyn wirtualnych.

8. Aby kontynuować, kliknij przycisk **OK**.

    ![Ustawienia usługi Container Service](media/container-service-deployment/acs-portal5.png)  <br />

9. Kliknij przycisk **OK** po zakończeniu weryfikacji usługi.

    ![Walidacja](media/container-service-deployment/acs-portal6.png)  <br />

10. Zapoznaj się z warunkami. Aby rozpocząć proces wdrażania, kliknij przycisk **Zakup**.

    ![Zakup](media/container-service-deployment/acs-portal7.png)  <br />

    Jeśli wybrano opcję przypięcia wdrożenia do witryny Azure Portal, widoczny jest stan wdrożenia.

    ![Stan wdrożenia](media/container-service-deployment/acs-portal8.png)  <br />

Przeprowadzenie wdrożenia zajmuje kilka minut. Następnie klaster usługi Azure Container Service jest gotowy do użycia.



## <a name="create-a-cluster-by-using-a-quickstart-template"></a>Tworzenie klastra przy użyciu szablonu szybkiego startu
Na potrzeby wdrożenia klastra w usłudze Azure Container Service są dostępne szablony szybkiego startu platformy Azure. Udostępnione szablony szybkiego startu można modyfikować w celu włączenia dodatkowej lub zaawansowanej konfiguracji platformy Azure. Aby utworzyć klaster usługi Azure Container Service za pomocą szablonu szybkiego startu platformy Azure, potrzebna jest subskrypcja platformy Azure. Jeśli jej nie masz, utwórz konto [bezpłatnej wersji próbnej](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). 

Wykonaj następujące kroki, aby wdrożyć klaster przy użyciu szablonu oraz interfejsu wiersza polecenia platformy Azure w wersji 2.0 (zobacz [instrukcje instalacji i konfigurowania](/cli/azure/install-az-cli2.md)).

> [!NOTE] 
> Jeśli korzystasz z systemu Windows, możesz użyć podobnych kroków, aby wdrożyć szablon przy użyciu programu Azure PowerShell. Zobacz kroki w dalszej części tej sekcji. Szablon możesz także wdrożyć za pośrednictwem [portalu](../azure-resource-manager/resource-group-template-deploy-portal.md) lub innych metod.

1. Aby wdrożyć klaster DC/OS, Docker Swarm lub Kubernetes, wybierz jeden z dostępnych szablonów szybkiego startu usługi GitHub. Częściowa lista została przedstawiona poniżej. Zwróć uwagę na to, że szablony DC/OS i Swarm są takie same — różnią się tylko domyślnie wybranym koordynatorem.

    * [Szablon DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [Szablon Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [Szablon Kubernetes](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. Zaloguj się do konta platformy Azure (`az login`) i upewnij się, że interfejs wiersza polecenia platformy Azure jest połączony z subskrypcją platformy Azure. Domyślną subskrypcję możesz wyświetlić przy użyciu następującego polecenia:

    ```azurecli
    az account show
    ```
    
    Jeśli masz więcej niż jedną subskrypcję i chcesz ustawić inną subskrypcję domyślną, uruchom polecenie `az account set --subscription` i określ nazwę lub identyfikator subskrypcji.

3. Najlepszym rozwiązaniem jest użycie dla wdrożenia nowej grupy zasobów. Aby utworzyć grupę zasobów, użyj polecenia `az group create`, określając lokalizację i nazwę grupy zasobów: 

    ```azurecli
    az group create --name "RESOURCE_GROUP" --location "LOCATION"
    ```

4. Utwórz plik JSON zawierający wymagane parametry szablonu. Pobierz plik parametrów o nazwie `azuredeploy.parameters.json`, który towarzyszy szablonowi usługi Azure Container Service `azuredeploy.json` w witrynie GitHub. Wprowadź wymagane wartości parametrów dla klastra. 

    Aby na przykład użyć [szablonu DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos), podaj wartości parametrów `dnsNamePrefix` i `sshRSAPublicKey`. W pliku `azuredeploy.json` można znaleźć opis oraz opcje dla innych parametrów.  
 

5. Utwórz klaster usługi Container Service przez przekazanie pliku parametrów wdrożenia przy użyciu następującego polecenia, gdzie:

    * **RESOURCE_GROUP** to nazwa grupy zasobów utworzonej w poprzednim kroku.
    * **DEPLOYMENT_NAME** (opcjonalnie) to nazwa nadawana wdrożeniu.
    * **TEMPLATE_URI** to lokalizacja pliku wdrożenia `azuredeploy.json`. Ten identyfikator URI musi wskazywać plik nieprzetworzony. Nie może to być wskaźnik do interfejsu użytkownika witryny GitHub. Aby znaleźć ten identyfikator URI, wybierz plik `azuredeploy.json` w witrynie GitHub i kliknij przycisk **Raw** (Nieprzetworzone).  

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters @azuredeploy.parameters.json
    ```

    Parametry możesz również podać jako ciąg w formacie JSON w wierszu polecenia. Użyj polecenia podobnego do następującego:

    ```azurecli
    az group deployment create -g RESOURCE_GROUP -n DEPLOYMENT_NAME --template-uri TEMPLATE_URI --parameters "{ \"param1\": {\"value1\"} … }"
    ```

    > [!NOTE]
    > Przeprowadzenie wdrożenia zajmuje kilka minut.
    > 

### <a name="equivalent-powershell-commands"></a>Równoważne polecenia programu PowerShell
Szablon klastra usługi Azure Container Service można również wdrożyć przy użyciu programu PowerShell. Ten dokument jest oparty na wersji 1.0 [modułu Azure PowerShell](https://azure.microsoft.com/blog/azps-1-0/).

1. Aby wdrożyć klaster DC/OS, Docker Swarm lub Kubernetes, wybierz jeden z dostępnych szablonów szybkiego startu usługi GitHub. Częściowa lista została przedstawiona poniżej. Zwróć uwagę na to, że szablony DC/OS i Swarm są takie same — różnią się tylko domyślnie wybranym koordynatorem.

    * [Szablon DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [Szablon Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [Szablon Kubernetes](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. Przed utworzeniem klastra w ramach subskrypcji platformy Azure sprawdź, czy nastąpiło zalogowanie do platformy Azure z poziomu sesji programu PowerShell. W tym celu możesz użyć polecenia `Get-AzureRMSubscription`:

    ```powershell
    Get-AzureRmSubscription
    ```

3. Jeśli musisz zalogować się do platformy Azure, użyj polecenia `Login-AzureRMAccount`:

    ```powershell
    Login-AzureRmAccount
    ```

4. Najlepszym rozwiązaniem jest użycie dla wdrożenia nowej grupy zasobów. Aby utworzyć grupę zasobów, użyj polecenia `New-AzureRmResourceGroup`, określając nazwę grupy zasobów i region docelowy:

    ```powershell
    New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
    ```

5. Po utworzeniu grupy zasobów możesz utworzyć klaster przy użyciu poniższego polecenia. Identyfikator URI żądanego szablonu zostanie określony dla parametru `-TemplateUri`. Po uruchomieniu tego polecenia program PowerShell wyświetli monit o wprowadzenie wartości parametrów wdrożenia.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
    ```

#### <a name="provide-template-parameters"></a>Wprowadzanie parametrów szablonu
Jeśli znasz program PowerShell, wiesz, że możesz przechodzić przez dostępne parametry polecenia cmdlet, wpisując znak minus (-), a następnie naciskając klawisz TAB. Ta funkcja działa również w przypadku parametrów zdefiniowanych w szablonie. Po wpisaniu nazwy szablonu polecenie cmdlet pobiera szablon, analizuje parametry i dynamicznie dodaje parametry szablonu do polecenia. Dzięki temu można bardzo łatwo określić wartości parametrów szablonu. A jeśli zapomnisz o podaniu wartości wymaganego parametru, w programie PowerShell zostanie wyświetlony monit dotyczący tej wartości.

Poniżej znajduje się pełne polecenie z uwzględnionymi parametrami. Możesz podać własne wartości nazw zasobów.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Następne kroki
Teraz, gdy masz działający klaster, możesz zapoznać się z tymi dokumentami, aby uzyskać szczegółowe informacje na temat połączeń i zarządzania:

* [Łączenie z klastrem usługi Azure Container Service](container-service-connect.md)
* [Współpraca z usługą Azure Container Service i rozwiązaniem DC/OS](container-service-mesos-marathon-rest.md)
* [Współpraca z usługą Azure Container Service i rozwiązaniem Docker Swarm](container-service-docker-swarm.md)
* [Współpraca z usługą Azure Container Service i rozwiązaniem Kubernetes](container-service-kubernetes-walkthrough.md)



<!--HONumber=Feb17_HO4-->


