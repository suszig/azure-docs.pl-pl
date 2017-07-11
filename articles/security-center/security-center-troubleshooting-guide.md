---
title: "Przewodnik rozwiązywania problemów z usługą Azure Security Center | Microsoft Docs"
description: "Ten dokument pomaga rozwiązywać problemy w usłudze Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 44462de6-2cc5-4672-b1d3-dbb4749a28cd
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: fa70dffc2a4bade44e1dec583bdfcf7b5dae6801
ms.contentlocale: pl-pl
ms.lasthandoff: 06/17/2017


---
<a id="azure-security-center-troubleshooting-guide" class="xliff"></a>

# Przewodnik rozwiązywania problemów z usługą Azure Security Center
Ten przewodnik jest przeznaczony dla specjalistów IT, analityków zabezpieczeń informacji oraz administratorów chmury, których organizacje używają usługi Azure Security Center i muszą rozwiązywać problemy związane z usługą Azure Security Center.

>[!NOTE] 
>Począwszy od początku czerwca 2017 roku, usługa Security Center będzie używać programu Microsoft Monitoring Agent do gromadzenia i przechowywania danych. Aby dowiedzieć się więcej, zobacz [Migracja platformy usługi Azure Security Center](security-center-platform-migration.md). Informacje przedstawione w tym artykule reprezentują funkcję Security Center po przejściu do programu Microsoft Monitoring Agent.
>

<a id="troubleshooting-guide" class="xliff"></a>

## Przewodnik rozwiązywania problemów
W tym przewodniku wyjaśniano, jak rozwiązywać problemy związane z usługą Security Center. W większości przypadków rozwiązywanie problemów w usłudze Security Center rozpoczyna się od sprawdzenia rekordów [dziennika inspekcji](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) składnika, w przypadku którego wystąpił błąd. Za pomocą dzienników inspekcji można określić:

* Operacje, które zostały wykonane
* Użytkownika, który zainicjował operację
* Czas wystąpienia operacji
* Stan operacji
* Wartości innych właściwości, które mogą ułatwić zbadanie operacji

Dziennik inspekcji zawiera informacje o wszystkich operacjach zapisu (PUT, POST, DELETE) wykonywanych względem zasobów, ale nie zawiera informacji dotyczących operacji odczytu (GET).

<a id="microsoft-monitoring-agent" class="xliff"></a>

## Microsoft Monitoring Agent
Do zbierania danych zabezpieczeń z maszyn wirtualnych platformy Azure usługa Security Center korzysta z programu Microsoft Monitoring Agent — jest to ten sam agent, który jest używany przez pakiet Operations Management Suite i usługę Log Analytics. Po włączeniu zbierania danych i poprawnym zainstalowaniu agenta na maszynie docelowej powinny być wykonywane poniższe procesy:

* HealthService.exe

Po otwarciu konsoli zarządzania usługami (services.msc) będzie również widoczna uruchomiona usługa Microsoft Monitoring Agent (jak pokazano poniżej):

![Usługi](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig5.png)

Aby sprawdzić, która wersja agenta jest używana, otwórz **Menedżera zadań**, na karcie **Procesy** znajdź kartę **Usługa Microsoft Monitoring Agent**, kliknij ją prawym przyciskiem myszy, a następnie kliknij pozycję **Właściwości**. Na karcie **Szczegóły** sprawdź wersję pliku, jak pokazano poniżej:

![Plik](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig6.png)
   

<a id="microsoft-monitoring-agent-installation-scenarios" class="xliff"></a>

## Scenariusze instalacji programu Microsoft Monitoring Agent
Istnieją dwa scenariusze instalacji, które mogą wygenerować różne wyniki podczas instalowania programu Microsoft Monitoring Agent na komputerze. Obsługiwane scenariusze są następujące:

* **Agent instalowany automatycznie za pomocą usługi Security Center**: w tym scenariuszu możliwe będzie wyświetlanie alertów w obu lokalizacjach — usłudze Security Center i funkcji przeszukiwania dzienników. Powiadomienia e-mail będą wysyłane na adres e-mail skonfigurowany w ramach zasad zabezpieczeń dla subskrypcji, do której należy zasób.
.
* **Agent zainstalowany ręcznie na maszynie wirtualnej znajdującej się na platformie Azure**: w tym scenariuszu w przypadku używania agentów pobranych i zainstalowanych ręcznie wcześniej niż w lutym 2017 roku wyświetlanie alertów w portalu usługi Security Center będzie możliwe tylko po odfiltrowaniu subskrypcji, do której należy obszar roboczy. W przypadku odfiltrowania subskrypcji, do której należy zasób, nie będzie możliwe wyświetlenie jakichkolwiek alertów. Powiadomienia e-mail będą wysyłane na adres e-mail skonfigurowany w ramach zasad zabezpieczeń dla subskrypcji, do której należy obszar roboczy.

>[!NOTE]
> Aby uniknąć zachowania opisanego w drugim scenariuszu, upewnij się, że pobrano najnowszą wersję agenta.
> 

<a id="troubleshooting-monitoring-agent-network-requirements" class="xliff"></a>

## Rozwiązywanie problemów z wymaganiami dotyczącymi sieci agenta monitorowania
Aby agenci mogli nawiązać połączenie z usługą Security Center i zarejestrować się za jej pomocą, muszą mieć oni dostęp do zasobów sieciowych, w tym numerów portów i adresów URL domeny.

- W przypadku serwerów proxy konieczne jest zapewnienie, że ich odpowiednie zasoby są skonfigurowane w ustawieniach agenta. Przeczytaj ten artykuł, aby uzyskać więcej informacji dotyczących [sposobu zmiany ustawień serwera proxy](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-windows-agents#configure-proxy-settings).
- Zapory ograniczające dostęp do Internetu należy skonfigurować w taki sposób, aby zezwalały na dostęp do pakietu OMS. W ustawieniach agenta nie jest wymagana żadna akcja.

W poniższej tabeli przedstawiono zasoby wymagane do komunikacji.

| Zasób agenta | Porty | Obejście inspekcji HTTPS |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Tak |
| *.oms.opinsights.azure.com | 443 | Tak |
| *.blob.core.windows.net | 443 | Tak |
| *.azure-automation.net | 443 | Tak |

Jeśli wystąpią problemy związane z dołączaniem dotyczące agenta, przeczytaj artykuł [How to troubleshoot Operations Management Suite onboarding issues](https://support.microsoft.com/en-us/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues) (Jak rozwiązywać problemy dotyczące przechodzenia do pakietu Operations Management Suite)


<a id="troubleshooting-endpoint-protection-not-working-properly" class="xliff"></a>

## Rozwiązywanie problemów z niedziałającą prawidłowo ochroną punktów końcowych

Agent gościa to proces nadrzędny w stosunku do wszystkich czynności wykonywanych przez rozszerzenie [Usługa firmy Microsoft chroniąca przed złośliwym kodem](../security/azure-security-antimalware.md). Jeśli proces agenta gościa zakończy się niepowodzeniem, usługa firmy Microsoft chroniąca przed złośliwym kodem uruchomiona jako proces podrzędny agenta gościa może również zakończyć się niepowodzeniem.  W przypadku takich scenariuszy zalecamy zweryfikowanie następujących opcji:

- Jeśli docelowa maszyna wirtualna to obraz niestandardowy, a twórca maszyny wirtualnej nigdy nie zainstalował agenta gościa.
- Jeśli cel to maszyna wirtualna z systemem Linux, a nie Windows, instalowanie rozszerzenia usługi firmy Microsoft chroniącej przed złośliwym kodem w wersji dla systemu Windows na maszynie wirtualnej z systemem Linux zakończy się niepowodzeniem. Agent gościa z systemem Linux ma określone wymagania dotyczące wersji systemu operacyjnego i wymaganych pakietów. Jeśli nie zostaną one spełnione, agent maszyny wirtualnej również nie będzie działać. 
- Jeśli maszyna wirtualna została utworzona za pomocą starszej wersji agenta gościa. W takiej sytuacji należy pamiętać, że niektórzy starsi agenci nie mogą przeprowadzić automatycznie samodzielnej aktualizacji i może to prowadzić do wystąpienia tego problemu. Zawsze używaj najnowszej wersji agenta gościa podczas tworzenia własnych obrazów.
- Niektóre programy administracyjne innych firm mogą wyłączać agenta gościa lub blokować dostęp do niektórych lokalizacji plików. Jeśli na maszynie wirtualnej zainstalowano programy innych firm, upewnij się, że agent znajduje się na liście wykluczeń.
- Niektóre ustawienia zapory lub grupy zabezpieczeń sieci (NSG) mogą blokować ruch sieciowy do i z agenta gościa.
- Niektóre listy kontroli dostępu (ACL) mogą uniemożliwiać dostęp do dysku.
- Brak miejsca na dysku może uniemożliwić prawidłowe działanie agenta gościa. 

Domyślnie interfejs użytkownika usługi firmy Microsoft chroniącej przed złośliwym kodem jest wyłączony. Więcej informacji na temat jego włączania w razie potrzeby można znaleźć w temacie [Enabling Microsoft Antimalware User Interface on Azure Resource Manager VMs Post Deployment](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/09/enabling-microsoft-antimalware-user-interface-post-deployment/) (Włączanie interfejsu użytkownika usługi firmy Microsoft chroniącej przed złośliwym kodem po wdrożeniu maszyn wirtualnych usługi Azure Resource Manager).

<a id="troubleshooting-problems-loading-the-dashboard" class="xliff"></a>

## Rozwiązywanie problemów z ładowaniem pulpitu nawigacyjnego

Jeśli masz problemy z ładowaniem pulpitu nawigacyjnego usługi Security Center, upewnij się, że użytkownik, który rejestruje subskrypcję usługi Security Center (tj. pierwszy użytkownik, który otworzył usługę Security Center za pomocą subskrypcji), oraz użytkownik, który chce włączyć kolekcję danych, mają uprawnienia *Właściciel* lub *Współautor* w subskrypcji. Od tej pory również użytkownicy z uprawnieniem *Czytelnik* w subskrypcji będą widzieć pulpit nawigacyjny/alerty/rekomendacje/zasady.

<a id="contacting-microsoft-support" class="xliff"></a>

## Kontaktowanie się z pomocą techniczną firmy Microsoft
Niektóre problemy można zidentyfikować za pomocą wskazówek znajdujących się w tym artykule. Inne problemy można znaleźć udokumentowane na publicznym [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter) usługi Security Center. Jeśli jednak potrzebna jest dalsza pomoc w rozwiązywaniu problemów, możesz otworzyć nowe żądanie obsługi, używając witryny **Azure Portal** w sposób pokazany poniżej: 

![Pomoc techniczna firmy Microsoft](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)

<a id="see-also" class="xliff"></a>

## Zobacz też
W tym dokumencie przedstawiono konfigurowanie zasad zabezpieczeń w Centrum zabezpieczeń Azure. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

* [Przewodnik planowania i obsługi usługi Azure Security Center](security-center-planning-and-operations-guide.md)— informacje na temat planowania i zagadnień projektowych podczas wdrażania usługi Azure Security Center.
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure
* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md) — informacje na temat reagowania na alerty zabezpieczeń i zarządzania nimi
* [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure


