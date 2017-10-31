---
title: "Integracja rozwiązań zabezpieczeń w usłudze Azure Security Center | Microsoft Docs"
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
ms.date: 10/23/2017
ms.author: yurid
ms.openlocfilehash: 847a872661bea31b774814188c7707260a16e620
ms.sourcegitcommit: 4d90200f49cc60d63015bada2f3fc4445b34d4cb
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Integracja rozwiązań zabezpieczeń w usłudze Azure Security Center
Ten dokument ułatwia zarządzanie rozwiązaniami zabezpieczeń już połączonymi z usługą Azure Security Center i dodawanie nowych.

## <a name="integrated-azure-security-solutions"></a>Zintegrowane rozwiązania zabezpieczeń platformy Azure
Usługa Security Center ułatwia włączanie zintegrowanych rozwiązań zabezpieczeń na platformie Azure. Korzyści to:

- **Uproszczone wdrażanie**: usługa Security Center oferuje udoskonaloną aprowizację zintegrowanych rozwiązań partnerskich. W przypadku rozwiązań takich jak ocena ochrony przed złośliwym oprogramowaniem i luk w zabezpieczeniach usługa Security Center może aprowizować wymaganego agenta na Twoich maszynach wirtualnych, zaś dla urządzeń zapory usługa Security Center określa większość wymaganej konfiguracji sieci.
- **Zintegrowane funkcje wykrywania**: zdarzenia zabezpieczeń z rozwiązań partnerskich są automatycznie zbierane, agregowane i wyświetlane w ramach zdarzeń i alertów usługi Security Center. Te zdarzenia są także połączone z funkcjami wykrywania z innych źródeł, aby zapewnić zaawansowane możliwości w zakresie wykrywania zagrożeń.
- **Ujednolicone zarządzanie monitorowaniem kondycji**: klienci mogą używać zintegrowanych zdarzeń kondycji do błyskawicznego monitorowania wszystkich rozwiązań partnerskich. Podstawowe funkcje zarządzania zapewniają łatwy dostęp do konfiguracji zaawansowanej przy użyciu rozwiązania partnerskiego.

Obecnie zintegrowane rozwiązania zabezpieczeń obejmują:

- Ochronę punktów końcowych ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec, [usługę firmy Microsoft chroniącą przed złośliwym kodem dla usług Azure Cloud Services i maszyn wirtualnych](https://docs.microsoft.com/azure/security/azure-security-antimalware), Windows Defender i System Center Endpoint Protection (SCEP))
- Zapora aplikacji sieci Web ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) i [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/))
- Zapora nowej generacji ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) i [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html))
- Ocena luk w zabezpieczeniach ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

## <a name="how-security-solutions-are-integrated"></a>Jak są integrowane rozwiązania zabezpieczeń
Rozwiązania zabezpieczeń platformy Azure, które zostały wdrożone z usługi Security Center, są automatycznie połączone. Możesz także połączyć inne źródła danych zabezpieczeń, a w tym:

- Usługa Azure AD Identity Protection
- Komputery działające lokalnie lub w innych chmurach
- Rozwiązanie zabezpieczeń, które obsługuje format Common Event Format (CEF)
- Microsoft Advanced Threat Analytics

![Integracja rozwiązań partnerskich](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Zarządzanie zintegrowanymi rozwiązaniami zabezpieczeń platformy Azure i innymi źródłami danych

Po wdrożeniu możesz wyświetlić informacje o kondycji zintegrowanych rozwiązań zabezpieczeń platformy Azure i wykonać podstawowe zadania zarządzania. Możesz też połączyć inne typy źródeł danych zabezpieczeń, takie jak alerty usługi Azure Active Directory Identity Protection, i dzienniki zapory w formacie CEF. Na pulpicie nawigacyjnym usługi Security Center wybierz rozwiązania zabezpieczeń.

### <a name="connected-solutions"></a>Rozwiązania połączone

Sekcja **Połączone rozwiązania** zawiera rozwiązania zabezpieczeń, które są aktualnie połączone z usługą Security Center, i informacje o stanie kondycji poszczególnych rozwiązań.  

![Rozwiązania połączone](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

### <a name="discovered-solutions"></a>Rozwiązania odnalezione

Sekcja **Rozwiązania odnalezione** przedstawia wszystkie rozwiązania, które zostały dodane za pośrednictwem platformy Azure. Pokazuje ona także wszystkie rozwiązania, z którymi, według sugestii usługi Security Center, należy się połączyć.

![Rozwiązania odnalezione](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Usługa Security Center automatycznie odnajduje inne rozwiązania zabezpieczeń, które działają na platformie Azure. Obejmuje to rozwiązania platformy Azure, takie jak [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection), jak również rozwiązania partnerskie, które są uruchomione na platformie Azure. Aby zintegrować te rozwiązania z usługą Security Center, wybierz pozycję **POŁĄCZ**.

### <a name="add-data-sources"></a>Dodawanie źródeł danych

Sekcja **Dodawanie źródeł danych** obejmuje inne dostępne źródła danych, które mogą zostać połączone. Aby uzyskać instrukcje dotyczące dodawania danych z dowolnego z tych źródeł, kliknij przycisk **DODAJ**.

![Źródła danych](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="next-steps"></a>Następne kroki

W tym artykule przedstawiono sposób zintegrowania rozwiązania partnerskiego w usłudze Security Center. Aby dowiedzieć się więcej na temat usługi Security Center, zobacz następujące artykuły:

* [Przewodnik planowania i obsługi usługi Security Center](security-center-planning-and-operations-guide.md)
* [Connecting Microsoft Advanced Threat Analytics to Azure Security Center (Łączenie usługi Microsoft Advanced Threat Analytics z usługą Azure Security Center)](security-center-ata-integration.md)
* [Connecting Azure Active Directory Identity Protection to Azure Security Center (Łączenie usługi Azure Active Directory Identity Protection z usługą Azure Security Center)](security-center-aadip-integration.md)
* [Monitorowanie kondycji zabezpieczeń w usłudze Security Center](security-center-monitoring.md). Informacje na temat sposobu monitorowania kondycji zasobów platformy Azure.
* [Monitorowanie rozwiązań partnerskich w usłudze Security Center](security-center-partner-solutions.md). Informacje na temat sposobu monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md). Odpowiedzi na często zadawane pytania dotyczące korzystania z usługi Security Center.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/). Wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.
