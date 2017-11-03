---
title: "Odbiorcy rozliczeń i obciążenia zwrotnego w stosie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak pobrać informacji o użyciu zasobów Azure stosu."
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: a9afc7b6-43da-437b-a853-aab73ff14113
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: alfredop
ms.openlocfilehash: ea7510c239ee07a9a27f3e682e61a6b08eb5694d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="usage-and-billing-in-azure-stack"></a>Użycie i rozliczenia Azure stosu

Użycie reprezentuje ilość zasobów używanych przez użytkownika. Stos Azure zbiera informacje o użyciu dla każdego użytkownika i używa go do nich. W tym artykule opisano, jak użytkownicy stosu Azure są rozliczane za użycie zasobów i sposobu dostępu do informacji dotyczących rozliczeń dla analityka, obciążenia zwrotnego itp.

Stos Azure zawiera infrastruktury zbieranie i agregowanie danych użycia dla wszystkich zasobów oraz przekazywanie tych danych do platformy Azure commerce. Możesz dostęp do tych danych i wyeksportować go do systemów rozliczeniowych za pomocą karty rozliczeń lub eksportowania ich do narzędzia do analizy biznesowej, takiego jak Microsoft Power BI. Po wyeksportowaniu, tej informacji dotyczących rozliczeń jest używane do analityka lub przeniesiona do systemu obciążenia zwrotnego.

![Model koncepcyjny rozliczeń karty nawiązywania stosu Azure rozliczeń aplikacji](media/azure-stack-billing-and-chargeback/image1.png)

## <a name="usage-pipeline"></a>Użycie potoku

Każdy dostawca zasobów w stosie Azure emituje danych użycia zgodnie z wykorzystania zasobów. Usługa użycia okresowo (co godzinę lub codziennie) agreguje dane dotyczące użytkowania i zapisuje je w bazie danych użycia. Dane przechowywane dane dotyczące użycia mogą uzyskiwać operatory stosu Azure i użytkownikami lokalnie, używając użycia interfejsów API. 

Jeśli masz [zarejestrowany wystąpienia stosu Azure Azure](azure-stack-register.md), mostek użycia jest skonfigurowany do wysyłania danych użycia do handlu platformy Azure. Po dane są dostępne na platformie Azure, można do niego dostęp za pośrednictwem portalu rozliczeń lub za pomocą usługi Azure użycia interfejsu API. Zapoznaj się [raportowania danych użycia](azure-stack-usage-reporting.md) tematu, aby dowiedzieć się więcej o korzystaniu z jakiego danych jest zgłaszany na platformie Azure. 

Na poniższej ilustracji przedstawiono najważniejsze składniki w potoku użycia:

![Użycie potoku](media/azure-stack-billing-and-chargeback/usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>Jakie informacje dotyczące użycia można znaleźć i w jaki sposób?

Azure dostawcy zasobów stos, na przykład obliczeniowych, magazynu i sieci, generowania danych użycia co godzinę dla każdej subskrypcji. Dane użycia zawiera informacje dotyczące zasobu używanego, takie jak nazwa zasobu, subskrypcją użytą ilość używane itp. Aby dowiedzieć się o zasobach identyfikator liczników, zapoznaj się [użycia interfejsu API często zadawane pytania dotyczące](azure-stack-usage-related-faq.md) artykułu. 

Po gromadzenia danych użycia jest [raportowanych do programu Azure](azure-stack-usage-reporting.md) do generowania rachunek, które można wyświetlić w portalu Azure rozliczeń. 

> [!NOTE]
> Raportowanie danych użycia nie jest wymagana dla usługi Azure stosu Development Kit i stosu Azure zintegrowany system licencjonowanych zgodnie z modelem pojemności. Aby dowiedzieć się więcej na temat licencjonowania w stosie Azure, zobacz [pakowania i cenach](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf) arkusz danych.

Portalu Azure rozliczeń zawiera dane użycia tylko w przypadku mogą być obciążane zasobów. Oprócz zasobów mogą być obciążane stosu Azure przechwytuje dane użycia dla szerszy zestaw zasobów, które są dostępne w środowisku Azure stosu za pośrednictwem interfejsów API REST lub programu PowerShell. Azure stosu operatory pobierać dane użycia dla wszystkich subskrypcji użytkownika podczas, gdy użytkownik może uzyskać ich szczegóły użycia.

## <a name="retrieve-usage-information"></a>Pobieranie informacji o użyciu

Do generowania danych użycia, powinien mieć zasobów, które są uruchomione i aktywnie za pomocą systemu, na przykład, aktywnej maszyny wirtualnej lub konto magazynu zawierające niektórych danych itp. Jeśli nie masz pewności czy dysponuje danymi umieszczonymi w Azure Marketplace stosu, wdrożyć maszynę wirtualną (VM) i sprawdzić, maszyna wirtualna monitorowania bloku, aby upewnić się, że działa. Aby wyświetlić dane użycia, użyj następujących poleceń cmdlet programu PowerShell:

1. [Instalowanie programu PowerShell Azure stosu.](azure-stack-powershell-install.md)
2. [Skonfiguruj użytkownika stosu Azure](user/azure-stack-powershell-configure-user.md) lub [stosu Azure operator](azure-stack-powershell-configure-admin.md) środowiska PowerShell 

3. Aby pobrać dane użycia, należy użyć [Get-UsageAggregates](/powershell/module/azurerm.usageaggregates/get-usageaggregates) polecenia cmdlet programu PowerShell:

   ```powershell
   Get-UsageAggregates -ReportedStartTime "<Start time for usage reporting>" -ReportedEndTime "<end time for usage reporting>" -AggregationGranularity <Hourly or Daily>
   ```

## <a name="next-steps"></a>Następne kroki

[Raport danych użycia usługi Azure stosu na platformie Azure](azure-stack-usage-reporting.md)

[Użycie zasobów dostawcy interfejsu API](azure-stack-provider-resource-api.md)

[Interfejs API użycia zasobów dzierżawcy](azure-stack-tenant-resource-usage-api.md)

[Często zadawane pytania dotyczące wykorzystania](azure-stack-usage-related-faq.md)

