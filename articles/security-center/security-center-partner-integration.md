---
title: "Integracja z partnerami i rozwiązaniami w usłudze Azure Security Center | Microsoft Docs"
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
ms.date: 09/12/2017
ms.author: yurid
ms.openlocfilehash: a6998c997840f1a9f349b85a4274908b611cd315
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="partner-and-solutions-integration-in-azure-security-center"></a>Integracja z partnerami i rozwiązaniami w usłudze Azure Security Center

W tym artykule opisano sposób, w jaki usługa Azure Security Center integruje się z partnerami w celu poprawy ogólnego bezpieczeństwa. Usługa Security Center zapewnia zintegrowane środowisko na platformie Azure i korzysta z portalu Azure Marketplace do certyfikacji partnera i rozliczeń.

## <a name="deploy-partner-solutions-from-security-center"></a>Wdrażanie rozwiązań partnerskich z usługi Security Center

Istnieją cztery główne powody, dla których warto korzystać z integracji z partnerami w usłudze Security Center:

- **Łatwość wdrażania**. Wdrożenie rozwiązania partnerskiego zgodnie z zaleceniem usługi Security Center jest znacznie łatwiejsze. Proces wdrażania można całkowicie zautomatyzować przy użyciu domyślnej konfiguracji i topologii sieci. Alternatywnie klienci mogą wybrać opcję częściowej automatyzacji, dającą większą elastyczność i możliwość dostosowania.
- **Zintegrowane wykrycia**. Zdarzenia zabezpieczeń z rozwiązań partnerskich są automatycznie zbierane, agregowane i wyświetlane w ramach zdarzeń i alertów usługi Security Center. Te zdarzenia są także połączone z funkcjami wykrywania z innych źródeł, aby zapewnić zaawansowane możliwości w zakresie wykrywania zagrożeń.
- **Ujednolicone monitorowanie i zarządzanie**. Klienci mogą używać zintegrowanych zdarzeń kondycji w celu jednoczesnego monitorowania wszystkich rozwiązań partnerskich. Podstawowe funkcje zarządzania zapewniają łatwy dostęp do konfiguracji zaawansowanej przy użyciu rozwiązania partnerskiego.
- **Eksportowanie do rozwiązania SIEM**. Klienci mogą wyeksportować wszystkie alerty usługi Security Center i alerty partnerów w formacie Common Event Format (CEF) do lokalnych systemów Security Information and Event Management (SIEM) za pomocą integracji dzienników Azure (wersja zapoznawcza).


## <a name="partners-that-integrate-with-security-center"></a>Partnerzy, którzy integrują się z usługą Security Center

Obecnie natywna integracja rozwiązań partnerskich dostępnych w witrynie Azure Marketplace z usługą Security Center obejmuje następujące elementy:

- **Ochronę punktów końcowych**. [Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec i [usługa firmy Microsoft chroniąca przed złośliwym kodem dla usług Azure Cloud Services i maszyn wirtualnych](https://docs.microsoft.com/azure/security/azure-security-antimalware).
- **Zaporę aplikacji internetowych**. [Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) i [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/). 
- **Zaporę nowej generacji**. [Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) i [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html). 
- **Ocenę luk w zabezpieczeniach**. [Qualys](https://www.qualys.com/public-clouds/microsoft-azure/). 

Z czasem w usłudze Security Center będzie zwiększana liczba partnerów w tych kategoriach, a także zostaną dodane nowe kategorie. 

## <a name="deploy-a-partner-solution"></a>Wdrażanie rozwiązania partnerów

W zależności od konfiguracji środowiska platformy Azure i zdefiniowanych przez Ciebie zasad zabezpieczeń usługa Security Center może zalecić wdrożenie rozwiązania partnerskiego. W ramach zalecenia usługi Security Center przejdziesz przez proces wyboru i instalacji rozwiązania partnerskiego. Ogólne środowisko wdrażania może się różnić w zależności od typu rozwiązania i wybranego partnera. Aby uzyskać więcej informacji zobacz następujące artykuły:

- [Instalowanie ochrony punktu końcowego](security-center-install-endpoint-protection.md)
- [Dodawanie zapory aplikacji sieci Web](security-center-add-web-application-firewall.md)
- [Dodawanie zapory nowej generacji](security-center-add-next-generation-firewall.md)
- [Funkcja oceny luk w zabezpieczeniach nie jest zainstalowana](security-center-vulnerability-assessment-recommendations.md)

## <a name="manage-partner-solutions"></a>Zarządzanie rozwiązaniami partnerskimi

W celu wyświetlenia informacji dotyczących kondycji rozwiązania i wykonywania podstawowych zadań zarządzania po wdrożeniu na pulpicie nawigacyjnym usługi **Security Center** wybierz pozycję **Rozwiązania partnerskie**.

![Integracja rozwiązań partnerskich](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

Zawartość wyświetlana po otwarciu obszaru rozwiązań w zakresie bezpieczeństwa może różnić się w zależności od Twojej infrastruktury. Jeśli jako przykładu użyjemy poprzedniego rysunku, ta strona będzie mieć trzy sekcje:

- **Rozwiązania połączone**. Wyświetla rozwiązania połączone z usługą Security Center.
- **Rozwiązania odnalezione**. Wyświetla rozwiązania, które nie są połączone z usługą Security Center. Te rozwiązania możesz połączyć — po wykonaniu tej czynności zostaną one wyświetlone w obszarze **Rozwiązania połączone**. Jeśli usługa Security Center nie wykrywa żadnych niepołączonych rozwiązań, ta sekcja jest ukrywana.
- **Dodaj źródła danych**. Wyświetla źródła danych należące i nienależące do platformy Azure, które można dodać do usługi Security Center.

### <a name="connected-solutions"></a>Rozwiązania połączone

Sekcja **Rozwiązania połączone** zawiera wszystkie rozwiązania w zakresie bezpieczeństwa, które są aktualnie połączone z usługą Security Center. 

![Rozwiązania połączone](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

Wyświetlane informacje mogą różnić się w zależności od rozwiązania. Na każdym kafelku mogą na przykład zostać wyświetlone następujące dane:

- **Ikona firmy partnera**. Jeśli usługa Security Center nie ma ikony firmy, są wyświetlane pierwsze znaki nazwy partnera.
- **Typ rozwiązania**. Jest wyświetlany typ rozwiązania.
- **Nazwa komputera**. Jest wyświetlana nazwa komputera.
- **Stan kondycji**. Jeśli wskaźnik kondycji nie zostanie wysłany, usługa Security Center pokaże datę i godzinę ostatniego odebranego zdarzenia, aby wskazać, czy urządzenie wykonuje raportowanie. Jeśli usługa Security Center nie odbierze wskaźnika kondycji z danego rozwiązania, kafelek rozwiązania nie pojawi się w tej sekcji.

> [!NOTE]
> Usługa Security Center pokazuje datę i godzinę ostatniego odebranego zdarzenia, aby wskazać, czy urządzenie wykonuje raportowanie. Rozwiązania, które nie wysyłają wskaźników kondycji, są wyświetlane jako połączone, jeśli w ciągu ostatnich 14 dni został wysłany alert lub zdarzenia.
>  

Niektóre z tych rozwiązań mogły zostać w pełni zintegrowane na platformie Azure, inne mogą działać w środowisku lokalnym. Ponieważ usługa Security Center obsługuje format [CEF](https://docs.microsoft.com/azure/operations-management-suite/oms-security-connect-products#what-is-cef), można połączyć ją z rozwiązaniami używającymi formatu CEF, takimi jak zapora, która obsługuje format CEF. Po dodaniu tego rozwiązania do usługi Security Center zapora wysyła dzienniki w formacie CEF do usługi Security Center, która obsługuje je w usłudze [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview). Zapora jest zasobem nienależącym do platformy Azure, który wysyła zdarzenia, ale nie wskaźniki kondycji. Jedyną informacją na temat kondycji dostępną w usłudze Security Center jest czas ostatniego wysłania zdarzenia z tego urządzenia. W przypadku wszystkich zasobów nienależących do platformy Azure usługa Security Center wyświetla w obszarze kondycji kafelka datę i godzinę odebrania ostatniego zdarzenia. Te informacje wskazują, że zasób nienależący do platformy Azure nadal wykonuje raportowanie.

### <a name="discovered-solutions"></a>Rozwiązania odnalezione

Sekcja **Rozwiązania odnalezione** przedstawia wszystkie rozwiązania, które zostały dodane za pośrednictwem platformy Azure. Pokazuje ona także wszystkie rozwiązania, z którymi, według sugestii usługi Security Center, należy się połączyć.

![Rozwiązania odnalezione](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Usługę Security Center można zintegrować z wbudowanymi rozwiązaniami platformy Azure, takimi jak usługa [Azure Active Directory (Azure AD) Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection). Jeśli masz licencję usługi Azure AD Identity Protection, ale nie połączono jej z usługą Security Center, usługa Azure AD Identity Protection jest wyświetlana w obszarze **Rozwiązania odnalezione**. Aby zintegrować to rozwiązanie z usługą Security Center, wybierz pozycję **POŁĄCZ** na kafelku **Azure AD Identity Protection**. Zostanie wyświetlona następująca strona:

![Usługa Azure AD Identity Protection](./media/security-center-partner-integration/security-center-partner-integration-fig6.png)

Aby zakończyć proces łączenia usługi Azure AD Identity Protection, wybierz obszar roboczy, w którym są zapisywane dane. Wszystkie dane z usługi Azure AD Identity Protection wypłyną z regionu obszaru roboczego wybranego w tym kroku. Użyj selektora obszaru roboczego w celu wybrania obszaru roboczego, a wówczas dane zaczną tam napływać.

Aby nawiązać połączenie z usługą Security Center, musisz być administratorem globalnym lub administratorem zabezpieczeń. Jeśli nie masz uprawnień, przycisk **Połącz** jest wyłączony. Pojawi się komunikat wyjaśniający, dlaczego przycisk jest wyłączony.

Alerty usługi Azure AD Identity Protection przechodzą przez potok wykrywania usługi Security Center. W ten sposób uzyskujesz alerty zarówno z usługi Security Center, jak i usługi Azure AD Identity Protection. Usługa Security Center scala wszystkie odpowiednie alerty w celu utworzenia [zdarzenia związanego z bezpieczeństwem](https://docs.microsoft.com/azure/security-center/security-center-incident). Opis zdarzenia związanego z bezpieczeństwem zawiera więcej informacji na temat podejrzanego działania.

### <a name="add-data-sources"></a>Dodawanie źródeł danych

Komputery z platformą Azure i bez niej można dodawać w celu zintegrowania z usługą Security Center. Dodawanie komputerów nienależących do platformy Azure oznacza, że możesz dodać komputery lokalne lub urządzenie obsługujące format CEF. 

![Źródła danych](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="see-also"></a>Zobacz też

W tym artykule przedstawiono sposób zintegrowania rozwiązania partnerskiego w usłudze Security Center. Aby dowiedzieć się więcej na temat usługi Security Center, zobacz następujące artykuły:

* [Przewodnik planowania i obsługi usługi Security Center](security-center-planning-and-operations-guide.md)
* [Zarządzanie alertami zabezpieczeń i reagowanie na nie w usłudze Security Center](security-center-managing-and-responding-alerts.md)
* [Alerty zabezpieczeń według typu w usłudze Security Center](security-center-alerts-type.md)
* [Monitorowanie kondycji zabezpieczeń w usłudze Security Center](security-center-monitoring.md). Informacje na temat sposobu monitorowania kondycji zasobów platformy Azure.
* [Monitorowanie rozwiązań partnerskich w usłudze Security Center](security-center-partner-solutions.md). Informacje na temat sposobu monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md). Odpowiedzi na często zadawane pytania dotyczące korzystania z usługi Security Center.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/). Wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.
