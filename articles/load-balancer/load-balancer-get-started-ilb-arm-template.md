---
title: "Tworzenie wewnętrznego modułu równoważenia obciążenia przy użyciu szablonu w usłudze Resource Manager | Microsoft Docs"
description: "Dowiedz się, jak utworzyć wewnętrzny moduł równoważenia obciążenia przy użyciu szablonu w usłudze Resource Manager"
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 64150862-6ced-42de-85dc-89d323257d7c
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 482c9cd46902d9e3f4e1e0f001182fdb43ce9367


---
# <a name="create-an-internal-load-balancer-using-a-template"></a>Tworzenie wewnętrznego modułu równoważenia obciążenia przy użyciu szablonu
[!INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

<BR>
[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]

[klasyczny model wdrażania](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Wdrażanie szablonu przy użyciu metody „kliknij, aby wdrożyć”
Przykładowy szablon dostępny w repozytorium publicznym korzysta z pliku parametrów zawierającego wartości domyślne używane do generowania scenariusza opisanego powyżej. Aby wdrożyć ten szablon przy użyciu metody „kliknij, aby wdrożyć”, kliknij [ten link](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), kliknij przycisk **Deploy to Azure** (Wdróż na platformie Azure), w razie potrzeby zastąp wartości domyślne parametrów i postępuj zgodnie z instrukcjami podanymi w portalu.

## <a name="deploy-the-template-by-using-powershell"></a>Wdrażanie szablonu przy użyciu programu PowerShell
Aby wdrożyć pobrany szablon przy użyciu programu PowerShell, wykonaj poniższe kroki.

1. Jeśli nie znasz programu Azure PowerShell, zapoznaj się z artykułem [Instalowanie i konfigurowanie programu Azure PowerShell](../powershell-install-configure.md) i postępuj zgodnie z instrukcjami aż do momentu logowania się w programie Azure i wyboru subskrypcji.
2. Pobierz plik parametrów na lokalny dysk twardy.
3. Edytuj plik i zapisz go.
4. Uruchom polecenie cmdlet **New AzureRmResourceGroupDeployment**, aby utworzyć grupę zasobów za pomocą szablonu.
   
        New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
            -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Wdrażanie szablonu przy użyciu interfejsu wiersza polecenia platformy Azure
Aby wdrożyć szablon przy użyciu interfejsu wiersza polecenia platformy Azure, wykonaj poniższe kroki.

1. Jeśli po raz pierwszy używasz interfejsu wiersza polecenia Azure, zobacz artykuł [Instalowanie i konfigurowania interfejsu wiersza polecenia Azure](../xplat-cli-install.md) i postępuj zgodnie z instrukcjami aż do punktu, w którym należy wybrać konto platformy Azure i subskrypcję.
2. Uruchom polecenie **azure config mode**, aby włączyć tryb Resource Manager, jak pokazano poniżej.
   
        azure config mode arm
   
    Oto oczekiwane dane wyjściowe po wprowadzeniu powyższego polecenia:
   
        info:    New mode is arm
3. Otwórz plik parametrów, wybierz jego zawartość i zapisz ją w pliku na komputerze. W tym przykładzie zapisaliśmy plik parametrów w pliku *parameters.json*.
4. Uruchom polecenie **azure group deployment create**, aby wdrożyć nowy wewnętrzny moduł równoważenia obciążenia przy użyciu plików parametrów i szablonu pobranych i zmodyfikowanych w powyższych krokach. Lista wyświetlana po danych wyjściowych zawiera opis używanych parametrów.
   
        azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json

## <a name="next-steps"></a>Następne kroki
[Configure a load balancer distribution mode using source IP affinity](load-balancer-distribution-mode.md) (Konfigurowanie trybu dystrybucji modułu równoważenia obciążenia przy użyciu koligacji źródłowych adresów IP)

[Configure idle TCP timeout settings for your load balancer](load-balancer-tcp-idle-timeout.md) (Konfigurowanie ustawień limitu czasu bezczynności protokołu TCP dla modułu równoważenia obciążenia)




<!--HONumber=Nov16_HO2-->


