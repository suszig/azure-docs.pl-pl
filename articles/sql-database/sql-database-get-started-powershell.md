---
title: 'Azure PowerShell: tworzenie bazy danych SQL | Microsoft Docs'
description: "Dowiedz się, jak utworzyć serwer logiczny, regułę zapory na poziomie serwera i bazy danych usługi SQL Database w witrynie Azure Portal."
keywords: "samouczek usługi sql database, tworzenie bazy danych sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: quick start create
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: hero-article
ms.date: 04/03/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 26d460a699e31f6c19e3b282fa589ed07ce4a068
ms.openlocfilehash: 90dc7e4a07f2a3c514c25b4031128f4df4e32aab
ms.lasthandoff: 04/04/2017

---

# <a name="create-a-single-azure-sql-database-using-powershell"></a>Tworzenie pojedynczej bazy danych Azure SQL Database za pomocą programu PowerShell

Program PowerShell umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. W tym przewodniku znajdują się szczegółowe informacje dotyczące użycia programu PowerShell na potrzeby wdrażania bazy danych Azure SQL Database w [grupie zasobów Azure](../azure-resource-manager/resource-group-overview.md) na [serwerze logicznym Azure SQL Database](sql-database-features.md).

Aby ukończyć kroki tego samouczka, upewnij się, że zainstalowano najnowszy program [Azure PowerShell](/powershell/azureps-cmdlets-docs). 

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [Add-AzureRmAccount](https://docs.microsoft.com/powershell/resourcemanager/azurerm.profile/v2.5.0/add-azurermaccount) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```powershell
Add-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz [grupę zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) na pomocą polecenia [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi w formie grupy. Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `myResourceGroup` w lokalizacji `westeurope`.

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "westeurope"
```
## <a name="create-a-logical-server"></a>Tworzenie serwera logicznego

Utwórz [serwer logiczny Azure SQL Database](sql-database-features.md) za pomocą polecenia [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.5.0/new-azurermsqlserver). Serwer logiczny zawiera grupę baz danych zarządzanych jako grupa. Poniższy przykład obejmuje tworzenie serwera o losowo wybranej nazwie w grupie zasobów za pomocą identyfikatora logowania administratora o nazwie `ServerAdmin` i z hasłem `ChangeYourAdminPassword1`. Zastąp te wstępnie zdefiniowane wartości zgodnie z potrzebami.

```powershell
$servername = "server-$(Get-Random)"
New-AzureRmSqlServer -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -Location "westeurope" `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList "ServerAdmin", $(ConvertTo-SecureString -String "ChangeYourAdminPassword1" -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>Konfigurowanie reguły zapory serwera

Utwórz [regułę zapory na poziomie serwera Azure SQL Database](sql-database-firewall-configure.md) za pomocą polecenia [New-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.5.0/new-azurermsqlserverfirewallrule). Reguła zapory na poziomie serwera umożliwia zewnętrznej aplikacji, takiej jak SQL Server Management Studio lub narzędzie SQLCMD, nawiązanie połączenia z bazą danych SQL za pośrednictwem zapory usługi SQL Database. Poniższy przykład obejmuje tworzenie reguły zapory dla wstępnie zdefiniowanego zakresu adresów, który w tym przypadku jest całym możliwym zakresem adresów IP. Zastąp te wstępnie zdefiniowane wartości wartościami dla zewnętrznego adresu IP lub zakresu adresów IP. 

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress "0.0.0.0" -EndIpAddress "255.255.255.255"
```

## <a name="create-a-blank-database"></a>Tworzenie pustej bazy danych

Utwórz na serwerze pustą bazę danych SQL [z poziomem wydajności S0](sql-database-service-tiers.md) za pomocą polecenia [New-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.5.0/new-azurermsqldatabase). Poniższy przykład obejmuje tworzenie bazy danych o nazwie `mySampleDatabase`. Zastąp tę wstępnie zdefiniowaną wartość zgodnie z potrzebami.

```powershell
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName "MySampleDatabase" `
    -RequestedServiceObjectiveName "S0"
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Inne przewodniki Szybki start w tej kolekcji bazują na tym przewodniku. Jeśli planujesz kontynuować pracę z kolejnymi przewodnikami Szybki start lub samouczkami, nie usuwaj zasobów utworzonych w tym przewodniku Szybki start. Jeśli nie planujesz kontynuować pracy, użyj poniższego polecenia, aby usunąć wszystkie zasoby utworzone w ramach tego przewodnika Szybki start.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>Następne kroki

- Aby nawiązywać połączenia i wykonywać zapytania za pomocą programu SQL Server Management Studio, zobacz [Connect and query with SSMS](sql-database-connect-query-ssms.md) (Nawiązywanie połączeń i wykonywanie zapytań za pomocą programu SSMS).
- Aby nawiązywać połączenia przy użyciu programu Visual Studio, zobacz [Connect and query with Visual Studio](sql-database-connect-query.md) (Nawiązywanie połączeń i wykonywanie zapytań przy użyciu programu Visual Studio).
* Opis techniczny usługi SQL Database można znaleźć w temacie [About the SQL Database service](sql-database-technical-overview.md) (Informacje o usłudze SQL Database).

