---
title: "Wskazówki dotyczące platformy Azure | Microsoft Docs"
description: "Najlepsze praktyki i wskazówki dotyczące platformy Azure"
services: 
documentationcenter: na
author: bennage
manager: marksou
editor: 
tags: 
ms.assetid: de94c74a-fea7-4815-8484-553e421a7490
ms.service: guidance
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: christb
translationtype: Human Translation
ms.sourcegitcommit: 5f3ced657cf3d6587a63789b3dd3ca41cd2856f0
ms.openlocfilehash: 0061e1ff2ae2d6b8ed7b7c3bb60405e76d4cc91b


---
# <a name="azure-guidance"></a>Wskazówki dotyczące platformy Azure
[!INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Zespół firmy Microsoft ds. wzorców i rozwiązań jest częścią zespołu doradczego klientów korzystających z platformy Azure. Naszym celem jest pomoc deweloperom, architektom i specjalistom IT w pomyślnym wykonywaniu zadań na platformie Microsoft Azure. Opracowujemy wskazówki dotyczące najlepszych praktyk związanych z kompilowaniem rozwiązań w chmurze na platformie Azure.

## <a name="checklists"></a>Listy kontrolne
Listy te pozwalają na szybkie przeglądanie informacji na temat podstawowych aspektów dostępności i skalowalności. 

* [Lista kontrolna dotycząca dostępności][AvailabilityChecklist] 
  
    Podsumowanie zalecanych rozwiązań zapewniających dostępność.
* [Lista kontrolna dotycząca skalowalności][ScalabilityChecklist]
  
    Podsumowanie zalecanych rozwiązań dotyczących projektowania i implementowania skalowalnych usług oraz obsługi zarządzania danymi.

## <a name="best-practices-articles"></a>Artykuły dotyczące najlepszych rozwiązań
Artykuły te zawierają szczegółowe omówienie ważnych koncepcji przeważnie związanych z przetwarzaniem danych w chmurze. 

* [Projekt interfejsu API][APIDesign] 
  
    Omówienie zagadnień projektowych, które należy wziąć pod uwagę podczas projektowania interfejsu API sieci Web.
* [Wdrażanie interfejsu API][APIImplementation] 
  
    Zestaw zalecanych rozwiązań dotyczących wdrażania i publikowania interfejsu API sieci Web.
* [Wskazówki dotyczące bezpieczeństwa interfejsu API](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 
  
    Omówienie zagadnień dotyczących uwierzytelniania i autoryzacji (takich jak typy tokenów, protokoły autoryzacji, przepływy autoryzacji i zapobieganie zagrożeniom).
* [Wskazówki dotyczące skalowania automatycznego][AutoscalingGuidance] 
  
    Podsumowanie zagadnień dotyczących skalowania rozwiązań bez interwencji użytkownika.
* [Wskazówki dotyczące zadań w tle][BackgroundJobsGuidance] 
  
    Opis dostępnych opcji i zalecanych rozwiązań dotyczących implementowania zadań, które powinny być wykonywane w tle, niezależnie od operacji przeprowadzanych na pierwszym planie lub w trybie interaktywnym.
* [Wskazówki dotyczące usługi Content Delivery Network (CDN)][CDNGuidance] 
  
    Ogólne wskazówki i zalecane rozwiązania dotyczące korzystania z usługi CDN w celu minimalizowania obciążenia aplikacji oraz maksymalizowania dostępności i wydajności.
* [Wskazówki dotyczące buforowania][CachingGuidance] 
  
    Podsumowanie informacji dotyczących zwiększania wydajności i skalowalności systemu za pomocą usługi buforowania.
* [Wskazówki dotyczące partycjonowania danych][DataPartitioningGuidance]
  
    Opis strategii, przy użyciu których można partycjonować dane w celu zwiększenia skalowalności, zmniejszenia stopnia rywalizacji i zoptymalizowania wydajności.
* [Wskazówki dotyczące monitorowania i diagnostyki][MonitoringandDiagnosticsGuidance] 
  
    Wskazówki dotyczące śledzenia informacji o sposobie korzystania z systemu i wykorzystaniu zasobów, a także ogólnego monitorowania kondycji i wydajności systemu.
* [Zalecane konwencje nazewnictwa][naming-conventions] 
  
    Zalecane konwencje nazewnictwa dla zasobów platformy Azure.
* [Ogólne wskazówki dotyczące ponawiania próby][RetryGeneralGuidance] 
  
    Omówienie ogólnych koncepcji obsługi przejściowych błędów.
* [Wskazówki dotyczące ponawiania próby uruchomienia określonych usług][RetryServiceSpecificGuidance]
  
    Podsumowanie funkcji ponownych prób dla wielu usług platformy Azure, w tym informacje pomocne w przypadku używania, dostosowywania lub rozszerzania mechanizmu ponawiania prób dla danej usługi.

## <a name="scenario-guides"></a>Przewodniki dotyczące scenariuszy
* [Uruchamianie usługi Elasticsearch na platformie Azure][elasticsearch] 
  
    Elasticsearch to wysoce skalowalna baza danych i aparat wyszukiwania typu open source. Należy używać ich w sytuacjach wymagających szybkiej analizy i odnajdywania informacji przechowywanych w dużych zestawach danych. Wskazówki te dotyczą niektórych kluczowych aspektów do uwzględnienia podczas projektowania klastra usługi Elasticsearch.
* [Zarządzanie tożsamościami dla aplikacji wielodostępnych][identity-multitenant] 
  
    Wielodostępność to architektura, w której aplikacja jest udostępniana wielu izolowanym od siebie dzierżawom. Wskazówki te przedstawiają sposób zarządzania tożsamościami użytkowników w aplikacji wielodostępnej za pomocą usługi [Azure Active Directory][AzureAD] w celu obsługi logowania i uwierzytelniania.
* [Opracowywanie rozwiązań do przetwarzania danych big data](https://msdn.microsoft.com/library/dn749874.aspx)
  
    W tym przewodniku opisano sposób korzystania z usługi HDInsight w przypadku scenariuszy, takich jak eksploracja iteracyjna, jako magazyn danych, dla procesów ETL oraz integracja z istniejącymi systemami analizy biznesowej. Zawiera on również wskazówki ułatwiające zrozumienie pojęć dotyczących danych big data oraz dotyczące planowania, projektowania i implementowania rozwiązań do przetwarzania danych big data.

## <a name="patterns"></a>Wzorce
* [Wzorce projektowe oparte na chmurze: normatywne wskazówki dotyczące architektury dla aplikacji w chmurze](https://msdn.microsoft.com/library/dn568099.aspx)
  
    Wzorce projektowe oparte na chmurze to biblioteka wzorców projektowania i powiązanych tematów zawierających wskazówki. Przedstawia ona korzyści z zastosowania wzorców przez zaprezentowanie sposobu działania poszczególnych elementów w architekturach aplikacji w chmurze.
* [Optymalizacja wydajności dla aplikacji w chmurze](https://github.com/mspnp/performance-optimization)
  
    Wskazówki te umożliwiają eksplorowanie typowych antywzorców, które utrudniają aplikacjom skalowanie w przypadku wystąpienia obciążenia. Zawierają one przykłady ośmiu antywzorców, [podstawowe informacje na temat analizy wydajności](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) i przewodnik dotyczący [oceny wydajności względem kluczowych metryk](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md).

## <a name="reference-architectures"></a>Architektury referencyjne
Nasze architektury referencyjne są uporządkowane według scenariusza.
Każda architektura oferuje zalecane praktyki, normatywne kroki oraz składnik wykonywalny, który stanowi realizację zaleceń.

Aktualna biblioteka architektur referencyjnych jest dostępna pod adresem [http://aka.ms/architecture](http://aka.ms/architecture).

## <a name="resiliency-guidance"></a>Wskazówki dotyczące odporności
W poniższych tematach opisano sposób projektowania aplikacji odpornych na awarie w środowisku rozproszonym chmury.   

* [Omówienie odporności][ResiliencyOvervew]
  
     Sposób kompilowania na platformie Azure aplikacji, które mogą odzyskać sprawność po awarii i kontynuować działanie. Omówienie zawiera opis strukturyzowanego podejścia umożliwiającego osiągnięcie odporności, od projektowania do implementowania, wdrażania i operacji.
* [Lista kontrolna odporności][resiliency-checklist]
  
    Lista kontrolna zawierająca zalecenia, które ułatwiają planowanie czynności w przypadku wystąpienia różnych trybów awarii.
* [Analiza trybu błędu][resiliency-fma] 
  
    Analiza trybu błędu (FMA, failure mode analysis) to proces budowania odporności w systemie przez określanie możliwych punktów wystąpienia błędów. Jako punkt początkowy dla procesu FMA ten artykuł zawiera wykaz potencjalnych trybów błędów i sposoby ograniczania odpowiedniego ryzyka. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md




<!--HONumber=Jan17_HO3-->


