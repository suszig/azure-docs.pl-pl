---
title: "Zarządzanie usług Azure Analysis Services przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "Zarządzanie Azure Analysis Services przy użyciu programu PowerShell."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: owend
ms.openlocfilehash: 95593053950f96a83e093c29516e9f66ebad53bf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Zarządzanie usług Azure Analysis Services przy użyciu programu PowerShell

W tym artykule opisano poleceń cmdlet programu PowerShell używane w celu wykonania zadania zarządzania bazy danych i serwera usług Azure Analysis Services. 

Zadania zarządzania serwerem, takie jak tworzenie lub usuwanie serwera, wstrzymywania lub wznawiania operacji serwera lub zmiana poziomu usług (warstwy), użyj polecenia cmdlet usługi Azure Resource Manager (AzureRM). Innych zadań związanych z zarządzaniem baz danych, takich jak dodawanie lub usuwanie członków roli przetwarzania lub podziału na partycje za pomocą poleceń cmdlet zawartych w tym samym module SqlServer jako SQL Server Analysis Services.

## <a name="permissions"></a>Uprawnienia
Większość zadań programu PowerShell wymaga uprawnień administratora na serwerze usług Analysis Services, którym zarządzasz. Zaplanowane zadania programu PowerShell są operacje instalacji nienadzorowanej. Konta, na którym działa planista musi mieć uprawnienia administratora na serwerze usług Analysis Services. 

Dla operacji serwera za pomocą poleceń cmdlet AzureRm, konta lub konta, na którym działa harmonogramu musi również należeć do roli właściciela zasobu w [based kontroli dostępu (RBAC)](../active-directory/role-based-access-control-what-is.md). 

## <a name="server-operations"></a>Operacji serwera 
Polecenia cmdlet systemu Azure Analysis Services są uwzględnione w [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) składnika modułu. Aby zainstalować AzureRM moduły poleceń cmdlet, zobacz [poleceń cmdlet usługi Azure Resource Manager](/powershell/azure/overview) w galerii programu PowerShell.

|Polecenie cmdlet|Opis| 
|------------|-----------------| 
|[AzureAnalysisServicesInstance eksportu](/powershell/module/azurerm.analysisservices/export-azureanalysisservicesinstancelog)|Eksporty dziennik do pliku.| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Pobiera Szczegóły wystąpienia serwera.|  
|[Nowe AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Tworzy wystąpienie serwera.|
|[Usuń AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Usuwa wystąpienie serwera.|  
|[Wstrzymaj AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Wstrzymuje wystąpienia serwera.| 
|[Resume-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Wznawia wystąpienia serwera.|  
|[Zestaw AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Modyfikuje wystąpienia serwera.|   
|[AzureRmAnalysisServicesServer testu](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Testy istnienia wystąpienia serwera.| 

## <a name="database-operations"></a>Operacje bazy danych

Azure operacje bazy danych usług Analysis Services używać tego samego [SqlServer](https://www.powershellgallery.com/packages/SqlServer) modułu jako SQL Server Analysis Services. Jednak nie wszystkie polecenia cmdlet są obsługiwane dla usług Azure Analysis Services. 

Moduł SqlServer udostępnia polecenia cmdlet do bazy danych specyficznych dla zadań zarządzania, jak również polecenia cmdlet ogólnego przeznaczenia Invoke ASCmd akceptującego zapytań tabelarycznych modelu skryptów języka (TMSL) lub skryptu. Następujące polecenia cmdlet w SqlServer module są obsługiwane dla usług Azure Analysis Services.

  
|Polecenie cmdlet|Opis|
|------------|-----------------| 
|[Dodaj RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Dodaj członka do roli bazy danych.| 
|[ASDatabase kopii zapasowej](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet)|Wykonywanie kopii zapasowej bazy danych usług Analysis Services.|  
|[Usuń RoleMember](https://msdn.microsoft.com/library/hh510173.aspx)|Usuwanie członka z roli bazy danych.|   
|[Wywołanie ASCmd](https://msdn.microsoft.com/library/hh479579.aspx)|Wykonanie skryptu TMSL.|
|[Wywołanie ProcessASDatabase](https://msdn.microsoft.com/library/mt651773.aspx)|Przetwarzanie bazy danych.|  
|[Wywołanie ProcessPartition](https://msdn.microsoft.com/library/hh510164.aspx)|Przetwarzanie partycji.| 
|[Wywołanie ProcessTable](https://msdn.microsoft.com/library/mt651774.aspx)|Przetwarza tabelę.|  
|[Scalania partycji](https://msdn.microsoft.com/library/hh479576.aspx)|Scalania partycji.|  
|[Przywracanie ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet)|Przywróć bazę danych usług Analysis Services.| 
  

## <a name="related-information"></a>Informacje pokrewne

* [Pobierz moduł PowerShell programu SQL Server](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Pobierz narzędzia SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [Moduł SqlServer w galerii programu PowerShell](https://www.powershellgallery.com/packages/SqlServer)    
* [Tabelaryczne modelu programowania dla 1200 poziom zgodności lub nowszym](https://msdn.microsoft.com/library/mt712541.aspx)
