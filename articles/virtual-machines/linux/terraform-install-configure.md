---
title: "Instalowanie i konfigurowanie Terraform do obsługi administracyjnej maszyn wirtualnych i innych infrastruktury na platformie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zainstalować i skonfigurować Terraform tworzenie zasobów Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: echuvyrov
ms.openlocfilehash: da567097be38ac649c6bf1de1508de24d21cb877
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="install-and-configure-terraform-to-provision-vms-and-other-infrastructure-into-azure"></a>Instalowanie i konfigurowanie Terraform do obsługi administracyjnej maszyn wirtualnych i innych infrastruktury na platformie Azure 
W tym artykule opisano kroki niezbędne do zainstalowania i skonfigurowania Terraform do udostępniania zasobów, takich jak maszyny wirtualne na platformie Azure. Zostanie sposób tworzenia i używania Azure poświadczenia umożliwiające Terraform do udostępniania zasobów w chmurze w bezpieczny sposób.

HashiCorp Terraform zapewnia prosty sposób definiowania i wdrażania infrastruktury chmurowej przy użyciu języka tworzenia szablonów niestandardowych o nazwie HashiCorp konfiguracji języka (HCL). Ten język niestandardowy jest [ułatwia pisanie i łatwy do zrozumienia](terraform-create-complete-vm.md). Ponadto przy użyciu `terraform plan` polecenia, można zwizualizować zmian w infrastrukturze przed je zatwierdzić. Wykonaj następujące kroki, aby rozpocząć korzystanie z platformy Azure, Terraform.

## <a name="install-terraform"></a>Zainstaluj Terraform
Aby zainstalować Terraform, [Pobierz](https://www.terraform.io/downloads.html) katalog instalacji odpowiedni dla systemu operacyjnego do oddzielnego pakietu. Pobieranie zawiera pojedynczy plik wykonywalny, dla którego należy także zdefiniować globalnego ścieżki. Aby uzyskać instrukcje dotyczące ustawiania ścieżka w systemie Linux i komputerów Mac, przejdź do [tej strony sieci Web](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux). Aby uzyskać instrukcje dotyczące ustawiania ścieżka w systemie Windows, przejdź do [tej strony sieci Web](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows). Aby zweryfikować instalację, uruchom `terraform` polecenia. Należy wyświetlić listę dostępnych opcji Terraform jako dane wyjściowe.

Następnie należy Terraform dostęp do Twojej subskrypcji platformy Azure w celu obsługi infrastruktury.

## <a name="set-up-terraform-access-to-azure"></a>Konfigurowanie Terraform dostępu do platformy Azure
Aby włączyć Terraform do udostępniania zasobów na platformie Azure, musisz utworzyć dwie jednostki w usłudze Azure Active Directory (Azure AD): aplikacji usługi Azure AD i nazwę główną usługi Azure AD. Następnie należy użyć tych jednostek identyfikatorów w skryptach Terraform. Nazwy głównej usługi jest lokalne wystąpienie programu globalny usługi Azure AD aplikacji. Nazwy głównej usługi umożliwia kontrolę szczegółowego lokalnego dostępu do zasobów globalnych.

Istnieje kilka sposobów, aby utworzyć aplikację usługi Azure AD i usługi Azure AD podmiotu zabezpieczeń. Łatwiejsze i szybsze sposób dzisiaj jest użycie interfejsu wiersza polecenia Azure 2.0, którego [można pobrać i zainstalować w systemie Windows, Linux lub Mac](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli). Możesz również służy programu PowerShell lub interfejsu wiersza polecenia platformy Azure w wersji 1.0 do tworzenia infrastruktury zabezpieczeń wymaganymi. Postępuj zgodnie z instrukcjami pokazują, jak skonfigurować Terraform na platformie Azure przy użyciu wszystkich tych metod.

### <a name="use-azure-cli-20-for-windows-linux-or-mac-users"></a>Użyj 2.0 interfejsu wiersza polecenia platformy Azure (dla użytkowników systemu Windows, Linux lub Mac.) 
Po pobraniu i zainstalowaniu [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), zaloguj się do administrowania subskrypcji platformy Azure, wydając polecenie:

```
az login
```

>[!NOTE]
>Jeśli używasz Chin platformy Azure w Niemczech i chmury Azure dla instytucji rządowych, należy najpierw skonfigurować wiersza polecenia platformy Azure, aby pracować z tej chmury. Można to zrobić, uruchamiając następujące czynności:

```
az cloud set --name AzureChinaCloud|AzureGermanCloud|AzureUSGovernment
```

Jeśli masz wiele subskrypcji Azure, ich szczegóły są zwracane przez `az login` polecenia. Ustaw `SUBSCRIPTION_ID` zmiennej środowiskowej, aby pomieścić wartość zwracana `id` pole z subskrypcji, którego chcesz użyć. 

Ustaw subskrypcji, która ma być używana dla tej sesji.

```
az account set --subscription="${SUBSCRIPTION_ID}"
```

Zapytania względem konta można uzyskać Identyfikatora i dzierżawcy wartości Identyfikatora subskrypcji.

```
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

Następnie należy utworzyć osobne funkcje poświadczeń dla Terraform.

```
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

Identyfikator aplikacji, hasło i sp_name oraz dzierżawy są zwracane. Zanotuj identyfikator appId i hasło.

Aby potwierdzić poświadczeń (nazwy głównej usługi), Otwórz nowe powłokę i uruchom następujące polecenia. Zastąp wartości zwracane sp_name, hasło i dzierżawy:

```
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

### <a name="use-powershell-for-windows-users"></a>Przy użyciu programu PowerShell (w przypadku użytkowników systemu Windows) 
Aby użyć komputera z systemem Windows do zapisu i wykonywania skryptów Terraform i przy użyciu programu PowerShell dla zadań konfiguracji, należy skonfigurować komputer z odpowiednich narzędzi programu PowerShell. 

1. Zainstaluj narzędzia programu PowerShell, wykonując kroki opisane w [Instalowanie i konfigurowanie programu Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps). 

2. Pobieranie i uruchamianie [azure setup.ps1 skryptu](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) z konsoli programu PowerShell.

3. Aby uruchomić skrypt azure setup.ps1, pobierz go i wykonaj `./azure-setup.ps1 setup` polecenie z poziomu konsoli programu PowerShell. Następnie zaloguj się do subskrypcji platformy Azure z uprawnieniami administracyjnymi.

4. Podaj nazwę aplikacji (dowolny ciąg, wymagane) po wyświetleniu monitu. Opcjonalnie wpisz silne hasło po wyświetleniu monitu. Jeśli nie podasz hasła, silne hasło zostanie wygenerowany przy użyciu bibliotek .NET zabezpieczeń.

### <a name="use-azure-cli-10-for-linux-or-mac-users"></a>Użyj interfejsu wiersza polecenia platformy Azure w wersji 1.0 (dla systemu Linux lub Mac. użytkowników)
Aby rozpocząć pracę z Terraform na maszynach z systemem Linux lub Mac z interfejsu wiersza polecenia platformy Azure w wersji 1.0, należy zainstalować właściwego bibliotek na tym komputerze.  

1. Instalowanie narzędzi interfejsu wiersza polecenia xPlat platformy Azure, wykonując kroki opisane w [zainstalować Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

2. Pobierz i zainstaluj procesor JSON zgodnie z instrukcjami w [Pobierz jq](https://stedolan.github.io/jq/download/).

3. Pobieranie i uruchamianie [azure setup.sh skryptu](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash skryptów z poziomu konsoli.

4. Aby uruchomić skrypt azure setup.sh, pobierz go i wykonaj `./azure-setup setup` polecenie z poziomu konsoli. Następnie zaloguj się do subskrypcji platformy Azure z uprawnieniami administracyjnymi.
 
5. Podaj nazwę aplikacji (dowolny ciąg, wymagane) po wyświetleniu monitu. Opcjonalnie wpisz silne hasło po wyświetleniu monitu. Jeśli nie podasz hasła, silne hasło zostanie wygenerowany przy użyciu bibliotek .NET zabezpieczeń.

Wcześniejsze skrypty Utwórz aplikację usługi Azure AD i usługi podmiotu zabezpieczeń. Nazwy głównej usługi pobiera współautora lub dostęp na poziomie właściciela subskrypcji. Z powodu wysokiego poziomu udzielono dostępu zawsze należy chronić informacje zabezpieczeń wygenerowane przez tych skryptów. Zanotuj wszystkie cztery części zabezpieczeń informacji dostarczonych przez tych skryptów: appId, hasła, IDENTYFIKATOR_SUBSKRYPCJI i tenant_id.

## <a name="set-environment-variables"></a>Ustaw zmienne środowiskowe
Po utworzeniu i skonfigurowaniu nazwy głównej usługi Azure AD, należy umożliwić Terraform wiedzieć dzierżawcy ID, identyfikator subskrypcji identyfikator klienta i klucz tajny klienta do użycia. Możesz zrobić to poprzez zastosowanie tych wartości w skryptach Terraform, zgodnie z opisem w [tworzenia podstawowej infrastruktury przy użyciu Terraform](terraform-create-complete-vm.md). Alternatywnie można ustawić następujące zmienne środowiskowe (i w związku z tym uniknąć przypadkowego ewidencjonowanie lub udostępniania poświadczeń):

- ARM_SUBSCRIPTION_ID
- ARM_CLIENT_ID
- ARM_CLIENT_SECRET
- ARM_TENANT_ID

Ten przykładowy skrypt powłoki umożliwia ustawienie tych zmiennych:

```
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id
```

Ponadto jeśli używasz Terraform Azure w Chinach, lub opcję Azure dla instytucji rządowych lub platformy Azure w Niemczech, należy ustawić zmienną ŚRODOWISKOWĄ odpowiednio.

## <a name="next-steps"></a>Następne kroki
Teraz zainstalowaniu Terraform i skonfigurować poświadczenia platformy Azure, dzięki czemu możesz przystąpić do wdrażania infrastruktury do Twojej subskrypcji platformy Azure. Następnie Dowiedz się, jak [tworzenie infrastruktury z Terraform](terraform-create-complete-vm.md).
