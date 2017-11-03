---
title: "Źródła danych obsługiwane w usłudze Azure Analysis Services | Dokumentacja firmy Microsoft"
description: "Zawiera opis źródła danych obsługiwane w przypadku modeli danych z usług Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 6ec63319-ff9b-4b01-a1cd-274481dc8995
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 5ba7ef5aa2cccad7cda3cb39459a5a5722516524
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Źródła danych obsługiwane w usłudze Azure Analysis Services
Azure serwery usług Analysis Services obsługują połączenia ze źródłami danych w chmurze i lokalnie w Twojej organizacji. Dodatkowe obsługiwanych źródeł danych są dodawane przez cały czas. Zaglądaj tu. 

Obecnie obsługiwane są następujące źródła danych:

| Chmura  |
|---|
| Magazyn obiektów Blob platformy Azure *  |
| Usługa Azure SQL Database  |
| Magazyn danych Azure |


| Lokalnie  |   |   |   |
|---|---|---|---|
| Dostępu do bazy danych  | Folder * | Baza danych Oracle  | Baza danych programu Teradata |
| Active Directory *  | Dokument JSON *  | Baza danych Postgre SQL *  |Tabela XML * |
| Analysis Services  | Wiersze z danych binarnych *  | SAP HANA *  |
| Analytics Platform System  | Baza danych MySQL  | SAP Business Warehouse *  | |
| Dynamics CRM *  | Źródła danych OData *  | SharePoint *  |
| Skoroszyt programu Excel  | Zapytanie ODBC  | SQL Database  |
| Exchange *  | OLE DB  | Baza danych programu Sybase  |

\*Tylko modele w 1400 tabelarycznych. 

> [!IMPORTANT]
> Łączenie ze źródłami danych lokalnych wymagają [bramy danych lokalnych](analysis-services-gateway.md) zainstalowany na komputerze w danym środowisku.

## <a name="data-providers"></a>Dostawcy danych

Modeli danych z usług Azure Analysis Services może wymagać różnych dostawców podczas nawiązywania połączenia z niektórych źródeł danych. W niektórych przypadkach modeli tabelarycznych łączenie ze źródłami danych przy użyciu macierzystych dostawców, takich jak SQL Server Native Client (SQLNCLI11) może zostać zwrócony błąd.

W przypadku modeli danych, które nawiązania połączenia danych w chmurze źródło takich jak bazy danych SQL Azure, jeśli używasz macierzystych dostawców innych niż SQLOLEDB, może zostać wyświetlony komunikat o błędzie: **"Dostawca"SQLNCLI11.1"nie jest zarejestrowana."** Lub, jeśli masz model zapytania bezpośredniego połączenia ze źródłami danych lokalnych, jeśli używasz macierzystych dostawców może zostać wyświetlony komunikat o błędzie: **"błąd podczas tworzenia zestawu wierszy OLE DB. Błędna składnia w pobliżu "Granica" "**.

Następujący dostawcy źródła danych są obsługiwane w pamięci lub modeli danych zapytania bezpośredniego, podczas nawiązywania połączenia ze źródłami danych w chmurze lub lokalne:

### <a name="cloud"></a>Chmura
| **Źródło danych** | **W pamięci** | **Zapytania bezpośredniego** |
|  --- | --- | --- |
| Azure SQL Data Warehouse |.NET framework Data Provider for SQL Server |.NET framework Data Provider for SQL Server |
| Usługa Azure SQL Database |.NET framework Data Provider for SQL Server |.NET framework Data Provider for SQL Server | |

### <a name="on-premises-via-gateway"></a>Lokalne (za pośrednictwem bramy)
|**Źródło danych** | **W pamięci** | **Zapytania bezpośredniego** |
|  --- | --- | --- |
| Oprogramowanie SQL Server |SQL Server Native Client 11.0 |.NET framework Data Provider for SQL Server |
| Oprogramowanie SQL Server |Dostawca Microsoft OLE DB dla programu SQL Server |.NET framework Data Provider for SQL Server | |
| Oprogramowanie SQL Server |.NET framework Data Provider for SQL Server |.NET framework Data Provider for SQL Server | |
| Oracle |Dostawca Microsoft OLE DB dla programu Oracle |Dostawca danych programu Oracle dla platformy .NET | |
| Oracle |Dostawca danych programu Oracle dla platformy .NET |Dostawca danych programu Oracle dla platformy .NET | |
| Teradata |Dostawca OLE DB dla programu Teradata |Dostawca danych programu Teradata dla platformy .NET | |
| Teradata |Dostawca danych programu Teradata dla platformy .NET |Dostawca danych programu Teradata dla platformy .NET | |
| Analytics Platform System |.NET framework Data Provider for SQL Server |.NET framework Data Provider for SQL Server | |

> [!NOTE]
> Upewnij się, że zainstalowano dostawców 64-bitowym, podczas korzystania z bramy lokalnej.
> 
> 

Podczas migrowania lokalnymi modelu tabelarycznego usług SQL Server Analysis Services do usług Azure Analysis Services, może być konieczna zmiana dostawcy.

**Aby określić dostawcę źródła danych**

1. Programu SSDT > **Eksplorator modelu tabelarycznego** > **źródeł danych**, kliknij prawym przyciskiem myszy połączenie ze źródłem danych, a następnie kliknij przycisk **Edytuj źródło danych**.
2. W **Edytuj połączenie**, kliknij przycisk **zaawansowane** aby otworzyć okno właściwości wyprzedzeniem.
3. W **ustawić właściwości zaawansowane** > **dostawców**, następnie wybierz odpowiedniego dostawcę.

## <a name="impersonation"></a>Personifikacja
W niektórych przypadkach może być konieczne określenie konta różnych personifikacji. Można określić konta personifikacji w SSDT lub SSMS.

Dla lokalnych źródeł danych:

* Jeśli uwierzytelnianie SQL personifikacji powinna być konta usługi.
* Jeśli przy użyciu uwierzytelniania systemu Windows, należy ustawić hasło użytkownika systemu Windows. Dla programu SQL Server uwierzytelnianie systemu Windows przy użyciu konta personifikacji określonych jest obsługiwana tylko w przypadku modeli danych w pamięci.

Dla źródeł danych w chmurze:

* Jeśli uwierzytelnianie SQL personifikacji powinna być konta usługi.

## <a name="next-steps"></a>Następne kroki
Jeśli masz lokalnych źródeł danych, należy zainstalować [brama lokalna](analysis-services-gateway.md).   
Aby dowiedzieć się więcej na temat zarządzania serwerem w SSDT lub SSMS, zobacz [Zarządzanie serwerem](analysis-services-manage.md).

