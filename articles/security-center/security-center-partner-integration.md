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
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 8cc44da0f61362018d2757da58ca4fb3a9a43764
ms.contentlocale: pl-pl
ms.lasthandoff: 09/14/2017

---
# <a name="partner-and-solutions-integration-in-azure-security-center"></a>Integracja z partnerami i rozwiązaniami w usłudze Azure Security Center

W tym artykule opisano sposób, w jaki usługa Azure Security Center integruje się z partnerami w celu poprawy ogólnego bezpieczeństwa. Usługa Security Center zapewnia zintegrowane środowisko na platformie Azure i korzysta z portalu Azure Marketplace do certyfikacji partnera i rozliczeń.

## <a name="why-deploy-partner-solutions-from-security-center"></a>Dlaczego warto wdrażać rozwiązania partnerskie z usługi Security Center

Cztery główne powody, dla których warto korzystać z integracji z partnerami w usłudze Security Center:

- **Łatwość wdrażania**. Wdrożenie rozwiązania partnerskiego zgodnie z zaleceniem usługi Security Center jest znacznie łatwiejsze. Proces wdrażania można całkowicie zautomatyzować przy użyciu domyślnej konfiguracji i topologii sieci. Alternatywnie klienci mogą wybrać opcję częściowej automatyzacji, dającą większą elastyczność i możliwość dostosowania.
- **Zintegrowane wykrycia**. Zdarzenia zabezpieczeń z rozwiązań partnerskich są automatycznie zbierane, agregowane i wyświetlane w ramach zdarzeń i alertów usługi Security Center. Te zdarzenia są także połączone z funkcjami wykrywania z innych źródeł, aby zapewnić zaawansowane możliwości w zakresie wykrywania zagrożeń.
- **Ujednolicone monitorowanie i zarządzanie**. Klienci mogą używać zintegrowanych zdarzeń kondycji w celu jednoczesnego monitorowania wszystkich rozwiązań partnerskich. Podstawowe funkcje zarządzania zapewniają łatwy dostęp do konfiguracji zaawansowanej przy użyciu rozwiązania partnerskiego.
- **Eksportowanie do rozwiązania SIEM**. Klienci mogą wyeksportować wszystkie alerty usługi Security Center i alerty partnerów w formacie Common Event Format (CEF) do lokalnych systemów Security Information and Event Management (SIEM) za pomocą integracji dzienników Azure (wersja zapoznawcza).


## <a name="partners-that-integrate-with-security-center"></a>Partnerzy, którzy integrują się z usługą Security Center

Obecnie natywna integracja rozwiązań partnerskich dostępnych w witrynie Azure Marketplace z usługą Security Center obejmuje następujące elementy:

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

W celu wyświetlenia informacji dotyczących kondycji rozwiązania i wykonywania podstawowych zadań zarządzania po wdrożeniu na pulpicie nawigacyjnym **Security Center** wybierz opcję **Rozwiązania partnerskie**.

![Integracja rozwiązań partnerskich](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

Zawartość wyświetlana po otwarciu obszaru rozwiązań w zakresie bezpieczeństwa może różnić się w zależności od infrastruktury. Jeśli jako przykładu użyjemy poprzedniego rysunku, ta strona będzie mieć trzy sekcje:

- **Rozwiązania połączone**: rozwiązania połączone z usługą Security Center.
- **Rozwiązania odnalezione**: rozwiązania niepołączone z usługą Security Center. Te rozwiązania możesz połączyć — po wykonaniu tej czynności zostaną one wyświetlone w obszarze rozwiązań połączonych.  Jeśli usługa Security Center nie wykrywa żadnych niepołączonych rozwiązań, ta sekcja jest ukrywana.
- **Dodaj źródła danych**: źródła danych platformy Azure i inne niż platformy Azure, które można dodać do usługi Security Center.

### <a name="connected-solutions"></a>Rozwiązania połączone

Sekcja **Rozwiązania połączone** zawiera wszystkie rozwiązania w zakresie bezpieczeństwa, które są aktualnie połączone z usługą Security Center. 

![Rozwiązania połączone](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

Poszczególne wyświetlane informacje mogą różnić się w zależności od rozwiązania. Na każdym kafelku mogą na przykład zostać wyświetlone następujące dane:

- Ikona firmy partnera.  Jeśli usługa Security Center nie ma ikony firmy, są wyświetlane pierwsze znaki nazwy partnera.
- Typ rozwiązania.
- Może zostać wyświetlona nazwa komputera.
- Stan kondycji.  Jeśli wskaźnik kondycji nie zostanie wysłany, usługa Security Center pokaże datę i godzinę ostatniego odebranego zdarzenia, aby wskazać, czy urządzenie wykonuje raportowanie. Jeśli usługa Security Center nie odbierze wskaźnika kondycji z danego rozwiązania, kafelek rozwiązania nie pojawi się w tej sekcji.

> [!NOTE]
> Zobacz: usługa Security Center pokazuje datę i godzinę ostatniego odebranego zdarzenia, aby wskazać, czy urządzenie wykonuje raportowanie. Rozwiązania, które nie wysyłają wskaźnika kondycji, są wyświetlane jako połączone, jeśli w ciągu ostatnich 14 dni został wysłany alert lub zdarzenie.
>  

Niektóre z tych rozwiązań mogły zostać w pełni zintegrowane na platformie Azure, inne mogą działać w środowisku lokalnym. Ponieważ usługa Security Center obsługuje format [Common Event Format (CEF)](https://docs.microsoft.com/azure/operations-management-suite/oms-security-connect-products#what-is-cef), można połączyć ją z rozwiązaniami używającymi formatu CEF, takimi jak Zapora, która obsługuje format CEF. Po dodaniu tego rozwiązania do usługi Security Center Zapora wysyła dzienniki w formacie CEF do usługi Security Center, która obsługuje je w usłudze [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview). Zapora nie jest zasobem platformy Azure i wysyła zdarzenia, ale nie wskaźnik kondycji.  Jedyną informacją na temat kondycji dostępną w usłudze Security Center jest czas ostatniego wysłania zdarzenia z tego urządzenia.  W przypadku wszystkich zasobów innych niż zasoby platformy Azure usługa Security Center wyświetla w obszarze kafelka datę i godzinę odebrania ostatniego zdarzenia, co oznacza, że zasób inny niż platformy Azure nadal przeprowadza raportowanie.

### <a name="discovered-solutions"></a>Rozwiązania odnalezione

Sekcja **Rozwiązania odnalezione** zawiera wszystkie rozwiązania, które zostały dodane za pośrednictwem platformy Azure, a usługa Security Center sugeruje połączenie z nimi.

![Rozwiązania odnalezione](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Usługę Security Center można zintegrować z wbudowanymi rozwiązaniami platformy Azure, takimi jak usługa [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection). Jeśli masz licencję usługi Azure AD Identity Protection, ale nie połączono jej z usługą Security Center, usługa Azure AD Identity Protection zostanie wyświetlona w obszarze **Rozwiązania odnalezione**. Aby zintegrować to rozwiązanie z usługą Security Center, kliknij pozycję **POŁĄCZ** na kafelku **Azure AD Identity Protection**. Zostanie wyświetlona następująca strona:

![Usługa Azure AD Identity Protection](./media/security-center-partner-integration/security-center-partner-integration-fig6.png)

Aby zakończyć proces łączenia usługi Azure AD Identity Protection, musisz wybrać obszar roboczy, w którym są zapisywane dane. Wszystkie dane z usługi Azure AD Identity Protection będą przepływać z regionu obszaru roboczego wybranego w tym kroku.  Konieczne będzie przejście przez selektor obszaru roboczego w celu wybrania obszaru roboczego. Z tego miejsca rozpocznie się przepływ danych.

Aby nawiązać połączenie z usługą Security Center musisz być administratorem globalnym lub administratorem zabezpieczeń.  Przycisk **Połącz** będzie wyłączony, jeśli nie masz uprawnień. W takiej sytuacji pojawi się również komunikat wyjaśniający, dlaczego przycisk został wyłączony.

Alerty usługi Azure AD Identity Protection przechodzą przez potok wykrywania usługi Security Center, co pozwala na pobieranie alertów z usług Security Center i Azure Active Directory Identity Protection. Usługa Security Center scali wszystkie odpowiednie alerty w celu utworzenia [zdarzenia związanego z bezpieczeństwem](https://docs.microsoft.com/azure/security-center/security-center-incident). Opis zdarzenia związanego z bezpieczeństwem będzie zawierać dalsze informacje na temat podejrzanego działania.

### <a name="add-data-sources"></a>Dodawanie źródeł danych

Komputery z platformą Azure i bez niej można dodawać w celu zintegrowania z usługą Security Center.  Dodawanie komputerów bez platformy Azure oznacza, że można dodać komputer lokalny lub urządzenie obsługujące format CEF. 

![Źródła danych](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="see-also"></a>Zobacz też

W tym artykule przedstawiono sposób zintegrowania rozwiązania partnerskiego w usłudze Azure Security Center. Aby dowiedzieć się więcej na temat usługi Security Center, zobacz następujące artykuły:

* [Przewodnik planowania i obsługi usługi Security Center](security-center-planning-and-operations-guide.md)
* [Zarządzanie alertami zabezpieczeń i reagowanie na nie w usłudze Security Center](security-center-managing-and-responding-alerts.md)
* [Alerty zabezpieczeń według typu w usłudze Security Center](security-center-alerts-type.md)
* [Monitorowanie kondycji zabezpieczeń w usłudze Security Center](security-center-monitoring.md). Informacje na temat sposobu monitorowania kondycji zasobów platformy Azure.
* [Monitorowanie rozwiązań partnerskich w usłudze Security Center](security-center-partner-solutions.md). Informacje na temat sposobu monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md). Odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/). Wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.

