---
title: "Integracja z partnerami w usłudze Azure Security Center | Microsoft Docs"
description: "W tym dokumencie wyjaśniono sposób integracji usługi Azure Security Center z partnerami w celu ulepszenia ogólnego stanu zabezpieczeń zasobów platformy Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 0c946ce6a96f2e3644b9890dad5d60a35ad4bcb7
ms.openlocfilehash: 10184c52d532eb56e66212fafdea3d059b0c43e3


---
# <a name="partner-integration-in-azure-security-center"></a>Integracja z partnerami w usłudze Azure Security Center
W tym dokumencie wyjaśniono sposób integracji usługi Azure Security Center z partnerami w celu ulepszenia ogólnego stanu zabezpieczeń i zapewnienia zintegrowanego środowiska na platformie Azure przy jednoczesnym wykorzystaniu witryny Azure Marketplace na potrzeby rozliczania i certyfikowania partnerów.

## <a name="why-deploy-partners-solutions-from-security-center"></a>Dlaczego warto wdrożyć rozwiązania partnerskie z usługi Security Center?

Cztery główne powody, dla których warto korzystać z integracji z partnerami w usłudze Security Center:

- **Łatwość wdrażania**: Wdrożenie rozwiązania partnerskiego zgodnie z zaleceniem usługi Security Center jest znacznie łatwiejsze. Proces wdrażania można całkowicie zautomatyzować przy użyciu domyślnej konfiguracji i topologii sieciowej. Klienci mogą także wybrać opcję półzautomatyzowaną, aby zapewnić większą elastyczność i lepsze możliwości dostosowywania konfiguracji.
- **Zintegrowane funkcje wykrywania**: Zdarzenia zabezpieczeń z rozwiązań partnerskich są automatycznie zbierane, agregowane i wyświetlane w ramach zdarzeń i alertów usługi Security Center. Te zdarzenia są także połączone z funkcjami wykrywania z innych źródeł, aby zapewnić zaawansowane możliwości w zakresie wykrywania zagrożeń.
- **Ujednolicone zarządzanie monitorowaniem kondycji**: Zintegrowane zdarzenia dotyczące kondycji umożliwiają klientom błyskawiczne monitorowanie rozwiązań partnerskich. Podstawowe funkcje zarządzania zapewniają łatwy dostęp do konfiguracji zaawansowanej przy użyciu rozwiązania partnerskiego.
- **Eksportowanie do systemu SIEM**: Klienci mogą teraz eksportować wszystkie alerty partnerów i usługi Security Center w formacie CEF do lokalnych systemów SIEM za pomocą usługi Microsoft Azure Log Integration (w wersji zapoznawczej)


## <a name="what-partners-are-integrated-with-security-center"></a>Jacy partnerzy są integrowani z usługą Security Center?
Obecnie usługę Security Center można zintegrować z następującymi partnerami:

- ochrona punktów końcowych (Trend Micro); 
- zapora aplikacji sieci Web (Barracuda, F5, Imperva oraz wkrótce Microsoft WAF i Fortinet); 
- rozwiązania zapory nowej generacji (Check Point, Barracuda oraz wkrótce Fortinet i Cisco); 
- rozwiązania do oceny luk w zabezpieczeniach (Qualys — wersja zapoznawcza). 

W miarę upływu czasu w usłudze Security Center będzie zwiększana liczba partnerów w tych istniejących kategoriach i zostaną dodane nowe kategorie. 

## <a name="how-to-deploy-a-partner-solution"></a>Jak wdrożyć rozwiązanie partnerskie?

Do rozwiązań partnerskich już wdrożonych do usługi Security Center można łatwo uzyskać dostęp za pomocą kafelka rozwiązania partnerskiego na głównym pulpicie nawigacyjnym usługi Security Center:

![Integracja z partnerami](./media/security-center-partner-integration/security-center-partner-integration-fig1.png)

Aby wdrożyć nowe rozwiązanie partnerskie na podstawie zalecenia usługi Security Center, wykonaj następujące kroki:

> [!NOTE]
> W krokach podanych w poniższym przykładzie założono, że masz obciążenie, które chcesz chronić za pomocą zapory aplikacji sieci Web.

1. Na pulpicie nawigacyjnym usługi Security Center kliknij kafelek **Zalecenia**.
2. W bloku **Zalecenia** kliknij pozycję **Dodaj zaporę aplikacji sieci Web**.
3. Kliknij nazwę aplikacji w bloku **Dodawanie zapory aplikacji sieci Web**.
4. W bloku **Dodawanie zapory aplikacji sieci Web** kliknij pozycję **Utwórz nową**.
5. W bloku **Tworzenie nowej zapory aplikacji sieci Web** zostanie wyświetlona lista obecnych partnerów oferujących funkcję zapory aplikacji sieci Web.
6. Wybierz odpowiednie rozwiązanie partnerskie i postępuj zgodnie instrukcjami (mogą się one różnić w zależności od partnera).

Na tym etapie ogólne środowisko wdrażania może się różnić w zależności od partnera. Aby uzyskać więcej informacji na temat zarządzania rozwiązaniami partnerskimi w usłudze Security Center, przeczytaj artykuł dotyczący [monitorowania rozwiązań partnerskich](security-center-partner-solutions.md) w usłudze Azure Security Center.

## <a name="see-also"></a>Zobacz też
W tym dokumencie przedstawiono integrowanie rozwiązania partnerskiego w usłudze Azure Security Center. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

* [Przewodnik planowania i obsługi usługi Azure Security Center](security-center-planning-and-operations-guide.md)
* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Alerty zabezpieczeń według typu w usłudze Azure Security Center](security-center-alerts-type.md)
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure.
* [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.



<!--HONumber=Nov16_HO5-->


