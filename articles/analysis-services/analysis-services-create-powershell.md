---
title: "Tworzenie serwera usług Azure Analysis Services przy użyciu programu PowerShell | Microsoft Docs"
description: "Dowiedz się, jak utworzyć serwer usług Azure Analysis Services przy użyciu programu PowerShell"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/01/2017
ms.author: owend
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: 43700de27ca643bd699159b86fe80c2e45e4acc7
ms.contentlocale: pl-pl
ms.lasthandoff: 08/04/2017

---

# <a name="create-an-azure-analysis-services-server-by-using-powershell"></a>Tworzenie serwera usług Azure Analysis Services przy użyciu programu PowerShell

Ten przewodnik Szybki start opisano, jak utworzyć serwer usług Azure Analysis Services przy użyciu programu PowerShell z poziomu wiersza polecenia w [grupie zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) w ramach subskrypcji platformy Azure.

Dla tego zadania jest wymagany moduł Azure PowerShell w wersji 4.0 lub nowszej. Aby dowiedzieć się, jaka wersja jest używana, uruchom polecenie ` Get-Module -ListAvailable AzureRM`. Aby przeprowadzić instalację lub uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). 

> [!NOTE]
> Utworzenie serwera może skutkować powstaniem nowej usługi płatnej. Aby dowiedzieć się więcej, zobacz [cennik usług Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="prerequisites"></a>Wymagania wstępne
Aby ukończyć ten przewodnik Szybki Start, musisz spełnić następujące warunki:

* **Subskrypcja platformy Azure**: odwiedź stronę [bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/offers/ms-azr-0044p/), aby utworzyć konto.
* **Azure Active Directory**: subskrypcja musi być skojarzona z dzierżawą usługi Azure Active Directory i musisz mieć konto w tym katalogu. Aby dowiedzieć się więcej, zobacz [Authentication and user permissions (Uwierzytelnianie i uprawnienia użytkownika)](analysis-services-manage-users.md).

## <a name="import-azurermanalysisservices-module"></a>Importowanie modułu AzureRm.AnalysisServices
Aby utworzyć serwer w ramach subskrypcji, należy użyć modułu składnika [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices). Załaduj moduł AzureRm.AnalysisServices w sesji programu PowerShell.

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Zaloguj się do subskrypcji platformy Azure przy użyciu polecenia [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount). Postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```powershell
Add-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
 
[Grupa zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi w formie grupy. Podczas tworzenia serwera musisz podać grupę zasobów w ramach subskrypcji. Jeśli nie masz jeszcze grupy zasobów, możesz ją utworzyć teraz przy użyciu polecenia [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `myResourceGroup` w regionie Zachodnie stany USA.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
```

## <a name="create-a-server"></a>Tworzenie serwera

Utwórz nowy serwer przy użyciu polecenia [New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver). W poniższym przykładzie utworzono serwer o nazwie myServer w grupie myResourceGroup w regionie Zachodnie stany USA w warstwie D1 i określono użytkownika philipc@adventureworks.com jako administratora serwera.

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myServer" -Location West US -Sku D1 -Administrator "philipc@adventure-works.com"
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Możesz usunąć serwer z subskrypcji przy użyciu polecenia [Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver). Jeśli będziesz korzystać z innych przewodników Szybki start i samouczków w tej kolekcji, nie usuwaj serwera. Poniższy przykład obejmuje usuwanie serwera utworzonego w poprzednim kroku.


```powershell
Remove-AzureRmAnalysisServicesServer -Name "myServer" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Następne kroki
[Zarządzanie usługami Azure Analysis Services przy użyciu programu PowerShell](analysis-services-powershell.md)   
[Wdrażanie modelu w programie SSDT](analysis-services-deploy.md)   
[Create a model in Azure portal (preview) (Tworzenie modelu w wersji zapoznawczej witryny Azure Portal)](analysis-services-create-model-portal.md)
