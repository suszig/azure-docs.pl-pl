---
title: "Klient biblioteki wymagane do nawiązywania połączenia z usług Azure Analysis Services | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano biblioteki klienta wymagane dla aplikacji klienckich i narzędzi można połączyć z usług Azure Analysis Services"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 9c9ceea9cc7fa9b6e4a63660bfa988bf50c0f5ae
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a>Biblioteki klienta do nawiązywania połączenia z usług Azure Analysis Services

Biblioteki klienta są niezbędne dla aplikacji klienckich i narzędzia do połączenia się z serwerami usług Analysis Services. 

Trzy bibliotek klienckich korzystać z usług Analysis Services. ADOMD.NET i Analysis Services Management Objects (AMO) są zarządzanego klienta biblioteki. Dostawca OLE DB usług Analysis Services (MSOLAP DLL) jest biblioteka klienta natywnego. Zazwyczaj wszystkie trzy są instalowane w tym samym czasie. Azure Analysis Services wymaga najnowszej wersji. 

Aplikacji klienta firmy Microsoft, takich jak Power BI Desktop i Excel zainstalować wszystkie trzech klientów biblioteki. Jednak w zależności od wersji lub częstotliwości aktualizacji, bibliotek klienta nie może być najnowsze wersje, które są wymagane przez usług Azure Analysis Services. To samo dotyczy aplikacji niestandardowych lub innych interfejsów, takich jak AsCmd, TOM czy ADOMD.NET. Te aplikacje wymagają ręcznej instalacji w bibliotekach. Z bibliotekami ręcznej instalacji klienta znajdują się w pakiety funkcji programu SQL Server jako dystrybucyjnego pakietów. Jednak te biblioteki klienta są powiązane z wersji programu SQL Server i nie może być r.  

Biblioteki klienta dla połączeń klienckich różnią się od dostawców danych wymagane do nawiązania połączenia z serwerem usług Azure Analysis Services źródła danych. Aby dowiedzieć się więcej na temat połączenia źródła danych, zobacz [połączenia źródła danych](analysis-services-datasource.md).

## <a name="download-the-latest-client-libraries"></a>Pobierz najnowsze biblioteki klienta  
Użyj następujących bibliotek klienta, jeśli w środowisku produkcyjnym i wymagają pełni zwolnione i obsługiwanych wersjach.

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>

## <a name="next-steps"></a>Następne kroki
[Połącz przy użyciu programu Excel](analysis-services-connect-excel.md)    
[Łączenie z usługą Power BI](analysis-services-connect-pbi.md)
