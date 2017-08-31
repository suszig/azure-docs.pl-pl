---
title: "Integracja z partnerami w usłudze Azure Security Center | Dokumentacja firmy Microsoft"
description: "Poznaj sposób integracji usługi Azure Security Center z partnerami w celu poprawy ogólnego stanu zabezpieczeń zasobów platformy Azure."
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
ms.date: 08/18/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: 44beafeff5cbe58ac8ca37632879f6ffc2b67e53
ms.contentlocale: pl-pl
ms.lasthandoff: 08/19/2017

---
# <a name="partner-integration-in-azure-security-center"></a>Integracja z partnerami w usłudze Azure Security Center

W tym artykule opisano sposób, w jaki usługa Azure Security Center integruje się z partnerami w celu poprawy ogólnego bezpieczeństwa. Usługa Security Center zapewnia zintegrowane środowisko na platformie Azure i korzysta z portalu Azure Marketplace do certyfikacji partnera i rozliczeń.

> [!NOTE] 
> Począwszy od czerwca 2017 roku usługa Security Center używa programu Microsoft Monitoring Agent do gromadzenia i przechowywania danych. Aby uzyskać więcej informacji, zobacz artykuł [Migracja platformy usługi Azure Security Center](security-center-platform-migration.md). Informacje przedstawione w tym artykule reprezentują funkcję Security Center po przejściu do programu Microsoft Monitoring Agent.
>

## <a name="why-deploy-partner-solutions-from-security-center"></a>Dlaczego warto wdrażać rozwiązania partnerskie z usługi Security Center

Cztery główne powody, dla których warto korzystać z integracji z partnerami w usłudze Security Center:

- **Łatwość wdrażania**. Wdrożenie rozwiązania partnerskiego zgodnie z zaleceniem usługi Security Center jest znacznie łatwiejsze. Proces wdrażania można całkowicie zautomatyzować przy użyciu domyślnej konfiguracji i topologii sieci. Alternatywnie klienci mogą wybrać opcję częściowej automatyzacji, dającą większą elastyczność i możliwość dostosowania.
- **Zintegrowane wykrycia**. Zdarzenia zabezpieczeń z rozwiązań partnerskich są automatycznie zbierane, agregowane i wyświetlane w ramach zdarzeń i alertów usługi Security Center. Te zdarzenia są także połączone z funkcjami wykrywania z innych źródeł, aby zapewnić zaawansowane możliwości w zakresie wykrywania zagrożeń.
- **Ujednolicone monitorowanie i zarządzanie**. Klienci mogą używać zintegrowanych zdarzeń kondycji w celu jednoczesnego monitorowania wszystkich rozwiązań partnerskich. Podstawowe funkcje zarządzania zapewniają łatwy dostęp do konfiguracji zaawansowanej przy użyciu rozwiązania partnerskiego.
- **Eksportowanie do rozwiązania SIEM**. Klienci mogą wyeksportować wszystkie alerty usługi Security Center i alerty partnerów w formacie Common Event Format (CEF) do lokalnych systemów Security Information and Event Management (SIEM) za pomocą integracji dzienników Azure (wersja zapoznawcza).


## <a name="partners-that-integrate-with-security-center"></a>Partnerzy, którzy integrują się z usługą Security Center

Obecnie usługę Security Center można zintegrować z następującymi rozwiązaniami:

- Ochrona punktów końcowych ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec i [usługa firmy Microsoft chroniąca przed złośliwym kodem dla usług Azure Cloud Services i maszyn wirtualnych](https://docs.microsoft.com/azure/security/azure-security-antimalware)) 
- Zapora aplikacji sieci Web ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) i [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/)) 
- Zapora nowej generacji ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) i [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html)) 
- Ocena luk w zabezpieczeniach ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

Z biegiem czasu w usłudze Security Center będzie zwiększana liczba partnerów w tych kategoriach, a także zostaną dodane nowe kategorie. 

## <a name="deploy-a-partner-solution"></a>Wdrażanie rozwiązania partnerów

W zależności od konfiguracji środowiska platformy Azure i zdefiniowanych przez Ciebie zasad zabezpieczeń usługa Security Center może zalecić wdrożenie rozwiązania partnerskiego. W ramach zalecenia usługi Security Center przejdziesz przez proces wyboru i instalacji rozwiązania partnerskiego. Ogólne środowisko wdrażania może się różnić w zależności od typu rozwiązania i wybranego partnera. Aby uzyskać więcej informacji zobacz następujące artykuły:

- [Instalowanie ochrony punktu końcowego](security-center-install-endpoint-protection.md)
- [Dodawanie zapory aplikacji sieci Web](security-center-add-web-application-firewall.md)
- [Dodawanie zapory nowej generacji](security-center-add-next-generation-firewall.md)
- [Funkcja oceny luk w zabezpieczeniach nie jest zainstalowana](security-center-vulnerability-assessment-recommendations.md)

## <a name="manage-partner-solutions"></a>Zarządzanie rozwiązaniami partnerskimi

W celu wyświetlenia informacji dotyczących kondycji rozwiązania i wykonywania podstawowych zadań zarządzania po wdrożeniu w bloku **Centrum zabezpieczeń** wybierz opcję **Rozwiązania partnerskie**. Aby uzyskać więcej informacji na temat zarządzania rozwiązaniami partnerskimi w usłudze Security Center, zobacz artykuł [Monitor partner solutions with Azure Security Center](security-center-partner-solutions.md) (Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center).

![Integracja z partnerami](./media/security-center-partner-integration/security-center-partner-integration-fig1-new2.png)

> [!NOTE]
> Obsługa rozwiązania ochrony punktu końcowego firmy Symantec jest ograniczona do odnajdywania. Nie są dostępne żadne alerty dotyczące kondycji.
>

## <a name="see-also"></a>Zobacz też

W tym artykule przedstawiono sposób zintegrowania rozwiązania partnerskiego w usłudze Azure Security Center. Aby dowiedzieć się więcej na temat usługi Security Center, zobacz następujące artykuły:

* [Przewodnik planowania i obsługi usługi Security Center](security-center-planning-and-operations-guide.md)
* [Zarządzanie alertami zabezpieczeń i reagowanie na nie w usłudze Security Center](security-center-managing-and-responding-alerts.md)
* [Alerty zabezpieczeń według typu w usłudze Security Center](security-center-alerts-type.md)
* [Monitorowanie kondycji zabezpieczeń w usłudze Security Center](security-center-monitoring.md). Informacje na temat sposobu monitorowania kondycji zasobów platformy Azure.
* [Monitorowanie rozwiązań partnerskich w usłudze Security Center](security-center-partner-solutions.md). Informacje na temat sposobu monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md). Odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/). Wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.

