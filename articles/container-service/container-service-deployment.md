<properties
   pageTitle="Wdrażanie klastra usługi kontenera platformy Azure | Microsoft Azure"
   description="Wdrażanie klastra usługi kontenera Azure za pomocą portalu Azure, interfejsu wiersza polecenia platformy Azure lub programu PowerShell."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/16/2016"
   ms.author="rogardle"/>

# Wdrażanie klastra usługi kontenera platformy Azure

Usługa kontenera platformy Azure zapewnia szybkie wdrażanie popularnych rozwiązań typu open source służących do aranżowania i klastrowania kontenerów. Za pomocą usługi kontenera platformy Azure można wdrażać klastry DC/OS i Docker Swarm przy użyciu szablonów usługi Azure Resource Manager i portalu Azure. Klastry te są wdrażane za pomocą zestawów skali maszyny wirtualnej Azure, a podczas pracy korzystają z ofert magazynu i pracy w sieci na platformie Azure. Do uzyskania dostępu do usługi kontenera platformy Azure niezbędna jest subskrypcja platformy Azure. Jeśli jej nie masz, możesz [utworzyć konto bezpłatnej wersji próbnej](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

Ten dokument zawiera opis kroków wdrażania klastra usługi kontenera platformy Azure przy użyciu [portalu Azure](#creating-a-service-using-the-azure-portal), [interfejsu wiersza polecenia platformy Azure](#creating-a-service-using-the-azure-cli) i [modułu Azure PowerShell](#creating-a-service-using-powershell).  

## Tworzenie usługi przy użyciu portalu Azure

Zaloguj się do portalu Azure, wybierz pozycję Nowe i wyszukaj platformę handlową platformy Azure dla **usługi kontenera platformy Azure**.

![Tworzenie wdrożenia](media/acs-portal1.png)  <br /> 

Wybierz pozycję **Usługa kontenera platformy Azure** i kliknij przycisk **Utwórz**.

![Tworzenie wdrożenia](media/acs-portal2.png)  <br /> 

Wprowadź następujące informacje:

- Nazwa użytkownika — jest to nazwa użytkownika, która będzie używana dla konta na poszczególnych maszynach wirtualnych i zestawach skali maszyn wirtualnych w klastrze usługi kontenera platformy Azure.
- Subskrypcja — wybierz subskrypcję platformy Azure.
- Grupa zasobów — wybierz istniejącą grupę zasobów lub utwórz nową.
- Lokalizacja — wybierz region platformy Azure dla wdrożenia usługi kontenera platformy Azure.
- Klucz publiczny SSH — dodaj klucz publiczny, który będzie używany do uwierzytelniania względem usługi Virtual Machines w kontenerze usługi na platformie Azure. Bardzo ważne jest, aby klucz ten nie zawierał podziałów wierszy i miał prefiks „ssh-rsa” i postfiks „nazwa_użytkownika@domena”. Powinien on wyglądać podobnie do następującego: „**AAAAB3Nz ssh-rsa... WYCINEK_ZAWARTOŚCI... UcyupgH użytkownik_Azure@linuxvm**”. Aby uzyskać wskazówki dotyczące tworzenia kluczy SSH, zobacz artykuły związane z systemami [Linux]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) i [Windows]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/).

Aby kontynuować, kliknij przycisk **OK**.

![Tworzenie wdrożenia](media/acs-portal3.png)  <br /> 

Wybierz typ aranżacji. Dostępne opcje to:

- DC/OS — wdraża klaster DC/OS.
- Swarm — wdraża klaster Docker Swarm.

Aby kontynuować, kliknij przycisk **OK**.

![Tworzenie wdrożenia](media/acs-portal4.png)  <br /> 

Wprowadź następujące informacje:

- Liczba wzorców — liczba wzorców w klastrze.
- Liczba agentów — w przypadku opcji Docker Swarm jest to początkowa liczba agentów w zestawie skali agenta. W przypadku opcji DC/OS jest to początkowa liczba agentów w prywatnym zestawie skali. Ponadto jest tworzony publiczny zestaw skali zawierający wstępnie określoną liczbę agentów. Liczba agentów w tym publicznym zestawie skali zależy od liczby serwerów głównych utworzonych w klastrze: 1 agent publiczny dla 1 serwera głównego i 2 agentów publicznych dla 3 lub 5 serwerów głównych.
- Rozmiar maszyny wirtualnej agenta — rozmiar maszyn wirtualnych agenta.
- Prefiks DNS — unikatowa w zakresie globalnym nazwa, która będzie służyć jako prefiks kluczowych części w pełni kwalifikowanych nazw domen dla usługi. 

Aby kontynuować, kliknij przycisk **OK**.

![Tworzenie wdrożenia](media/acs-portal5.png)  <br /> 

Kliknij przycisk **OK** po zakończeniu weryfikacji usługi.

![Tworzenie wdrożenia](media/acs-portal6.png)  <br /> 

Kliknij przycisk **Utwórz**, aby rozpocząć proces wdrażania.

![Tworzenie wdrożenia](media/acs-portal7.png)  <br /> 

Jeśli wybrano opcję przypięcia wdrożenia do portalu Azure, stan wdrożenia może być widoczny.

![Tworzenie wdrożenia](media/acs-portal8.png)  <br /> 

Po zakończeniu wdrażania klaster usługi kontenera platformy Azure jest gotowy do użycia.

## Tworzenie usługi przy użyciu interfejsu wiersza polecenia platformy Azure

Do utworzenia wystąpienia usługi kontenera platformy Azure przy użyciu wiersza polecenia niezbędna jest subskrypcja platformy Azure. Jeśli jej nie masz, możesz [utworzyć konto bezpłatnej wersji próbnej](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). Musisz również zainstalować i skonfigurować interfejs wiersza polecenia platformy Azure.

Aby wdrożyć klaster DC/OS lub Docker Swarm, wybierz jeden z poniższych szablonów usługi GitHub. Zwróć uwagę na to, że oba te szablony są takie same — różnią się tylko domyślnie wybraną aranżacją.

* [Szablon DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-mesos)
* [Szablon Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Następnie upewnij się, że interfejs wiersza polecenia platformy Azure został połączony z subskrypcją platformy Azure. Możesz to zrobić za pomocą następującego polecenia:

```bash
azure account show
```
Jeśli konto platformy Azure nie zostanie zwrócone, użyj poniższego polecenia, aby zalogować się do platformy Azure za pomocą interfejsu wiersza polecenia.

```bash
azure login -u user@domain.com
```

Następnie skonfiguruj narzędzia interfejsu wiersza polecenia platformy Azure, aby umożliwić korzystanie z usługi Azure Resource Manager.

```bash
azure config mode arm
```

Utwórz grupę zasobów platformy Azure oraz klaster usługi kontenera przy użyciu poniższego polecenia, gdzie:

- **RESOURCE_GROUP** (grupa_zasobów) to nazwa grupy zasobów, której chcesz użyć dla tej usługi.
- **LOCATION** (lokalizacja) to region platformy Azure, w którym zostanie utworzone wdrożenie grupy zasobów i usługi kontenera platformy Azure.
- **TEMPLATE_URI** (identyfikator_URI_szablonu) to lokalizacja pliku wdrożenia. **Uwaga:** musi to być plik RAW, a nie wskaźnik do interfejsu użytkownika usługi GitHub. Aby znaleźć ten adres URL, wybierz plik azuredeploy.json w usłudze GitHub i kliknij przycisk RAW:

> Uwaga: w przypadku uruchamiania tego polecenia powłoka wyświetli monit o wprowadzenie wartości parametrów wdrożenia.
 
```bash
# sample deployment

azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### Wprowadzanie parametrów szablonu

Ta wersja polecenia wymaga interaktywnego zdefiniowania parametrów. Jeśli chcesz podać parametry, takie jak ciąg sformatowany przy użyciu notacji JSON, możesz to zrobić za pomocą przełącznika `-p`. Na przykład:

 ```bash
 # sample deployment

azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
 ```

Możesz również udostępnić plik z parametrami sformatowanymi przy użyciu notacji JSON, korzystając z przełącznika `-e`:

 ```bash
 # sample deployment

azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
 ```

Aby wyświetlić przykładowy plik parametrów o nazwie `azuredeploy.parameters.json`, wyszukaj go w serwisie GitHub przy użyciu szablonów usługi kontenera platformy Azure.

## Tworzenie usługi przy użyciu programu PowerShell

Klaster usługi kontenera platformy Azure można również wdrożyć przy użyciu programu PowerShell. Ten dokument jest oparty na wersji 1.0 [modułu Azure PowerShell](https://azure.microsoft.com/blog/azps-1-0/).

Aby wdrożyć klaster DC/OS lub Docker Swarm, wybierz jeden z poniższych szablonów. Zwróć uwagę na to, że oba te szablony są takie same — różnią się tylko domyślnie wybraną aranżacją.

* [Szablon DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-mesos)
* [Szablon Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Przed utworzeniem klastra w ramach subskrypcji platformy Azure sprawdź, czy nastąpiło zalogowanie do platformy Azure z poziomu sesji programu PowerShell. W tym celu możesz użyć polecenia `Get-AzureRMSubscription`:

```powershell
Get-AzureRmSubscription
```

Jeśli musisz zalogować się do platformy Azure, użyj polecenia `Login-AzureRMAccount`:

```powershell
Login-AzureRmAccount
```

Jeśli wdrażasz do nowej grupy zasobów, musisz najpierw utworzyć grupę zasobów. Aby utworzyć nową grupę zasobów, użyj polecenia `New-AzureRmResourceGroup`, określając nazwę grupy zasobów i region docelowy:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Po utworzeniu grupy zasobów możesz utworzyć klaster przy użyciu poniższego polecenia. Identyfikator URI żądanego szablonu zostanie określony dla parametru `-TemplateUri`. W przypadku uruchamiania tego polecenia program PowerShell wyświetli monit o wprowadzenie wartości parametrów wdrożenia.

```powershell
# sample deployment

New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
 ```

### Wprowadzanie parametrów szablonu

Jeśli znasz program PowerShell, wiesz, że możesz przechodzić przez dostępne parametry polecenia cmdlet, wpisując znak minus (-), a następnie naciskając klawisz TAB. Ta funkcja działa również w przypadku parametrów zdefiniowanych w szablonie. Po wpisaniu nazwy szablonu polecenie cmdlet pobiera szablon, analizuje parametry i dynamicznie dodaje parametry szablonu do polecenia. Dzięki temu można bardzo łatwo określić wartości parametrów szablonu. A jeśli zapomnisz o podaniu wartości wymaganego parametru, w programie PowerShell zostanie wyświetlony monit dotyczący tej wartości.

Poniżej znajduje się pełne polecenie z uwzględnionymi parametrami. Możesz podać własne wartości nazw zasobów.

```powershell
# sample deployment

New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## Następne kroki
 
Teraz, gdy masz działający klaster, możesz zapoznać się z tymi dokumentami, aby uzyskać szczegółowe informacje na temat połączeń i zarządzania.
 
[Łączenie z klastrem usługi kontenera platformy Azure](container-service-connect.md)
[Praca z usługą kontenera platformy Azure i klastrem DC/OS](container-service-mesos-marathon-rest.md)
[Praca z usługą kontenera platformy Azure i klastrem Docker Swarm](container-service-docker-swarm.md)



<!--HONumber=Jun16_HO2-->


