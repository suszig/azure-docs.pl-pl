---
title: "Integracja z partnerami w usłudze Azure Security Center | Microsoft Docs"
description: "W tym dokumencie wyjaśniono sposób integracji usługi Azure Security Center z partnerami w celu ulepszenia ogólnego stanu zabezpieczeń zasobów platformy Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/28/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: 7f8b63c22a3f5a6916264acd22a80649ac7cd12f
ms.openlocfilehash: 8dddfc8929ab1a0c44522ed2a2596e2c82e3987d
ms.contentlocale: pl-pl
ms.lasthandoff: 05/01/2017


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

- Ochrona punktów końcowych ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html)) 
- Zapora aplikacji sieci Web ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets), [App Gateway WAF](https://azure.microsoft.com/en-us/blog/azure-web-application-firewall-waf-generally-available/)) 
- Zapora nowej generacji ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) i [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html)) 
- Ocena luk w zabezpieczeniach ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/) — wersja zapoznawcza)  

W miarę upływu czasu w usłudze Security Center będzie zwiększana liczba partnerów w tych istniejących kategoriach i zostaną dodane nowe kategorie. 

## <a name="how-to-deploy-a-partner-solution"></a>Jak wdrożyć rozwiązanie partnerskie?

W zależności od konfiguracji środowiska platformy Azure i zdefiniowanych przez Ciebie zasad zabezpieczeń usługa Security Center może zalecić wdrożenie rozwiązania partnerskiego. W ramach zalecenia przejdziesz przez proces wyboru i instalacji rozwiązania partnerskiego. Na tym etapie ogólne środowisko wdrażania może się różnić w zależności od typu rozwiązania i partnera. Więcej informacji można znaleźć, korzystając z poniższych linków:

- [Dodawanie zapory aplikacji sieci Web](security-center-add-web-application-firewall.md)
- [Dodawanie zapory nowej generacji](security-center-add-next-generation-firewall.md)
- [Zainstaluj punkt końcowy](security-center-install-endpoint-protection.md)
- [Funkcja oceny luk w zabezpieczeniach nie jest zainstalowana](security-center-vulnerability-assessment-recommendations.md)

## <a name="how-to-manage-partner-solutions"></a>Jak zarządzać rozwiązaniami partnerskimi?

Po wdrożeniu rozwiązania partnerskiego można wyświetlić informacje o kondycji rozwiązania i wykonywać podstawowe zadania w zakresie zarządzania na kafelku rozwiązania partnerskiego na głównym pulpicie nawigacyjnym usługi Security Center. Aby uzyskać więcej informacji na temat zarządzania rozwiązaniami partnerskimi w usłudze Security Center, przeczytaj artykuł [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md).

![Integracja z partnerami](./media/security-center-partner-integration/security-center-partner-integration-fig1-new.png)


## <a name="see-also"></a>Zobacz też
W tym dokumencie przedstawiono integrowanie rozwiązania partnerskiego w usłudze Azure Security Center. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

* [Przewodnik planowania i obsługi usługi Azure Security Center](security-center-planning-and-operations-guide.md)
* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Alerty zabezpieczeń według typu w usłudze Azure Security Center](security-center-alerts-type.md)
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure.
* [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.

