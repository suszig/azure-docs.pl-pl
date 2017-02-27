---
title: "Przewodnik rozwiązywania problemów z usługą Azure Security Center | Microsoft Docs"
description: "Ten dokument pomaga rozwiązywać problemy w usłudze Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 44462de6-2cc5-4672-b1d3-dbb4749a28cd
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: b9f4a8b185f9fb06f8991b6da35a5d8c94689367
ms.openlocfilehash: dbbec729c14d0d9dc5781e7a88a1db3f66f7df97


---
# <a name="azure-security-center-troubleshooting-guide"></a>Przewodnik rozwiązywania problemów z usługą Azure Security Center
Ten przewodnik jest przeznaczony dla specjalistów IT, analityków zabezpieczeń informacji oraz administratorów chmury, których organizacje używają usługi Azure Security Center i muszą rozwiązywać problemy związane z usługą Azure Security Center.

## <a name="troubleshooting-guide"></a>Przewodnik rozwiązywania problemów
W tym przewodniku wyjaśniano, jak rozwiązywać problemy związane z usługą Security Center. W większości przypadków rozwiązywanie problemów w usłudze Security Center będzie rozpoczynać się od sprawdzenia rekordów [dziennika inspekcji](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) składnika, w przypadku którego wystąpił błąd. Za pomocą dzienników inspekcji można określić:

* Operacje, które zostały wykonane
* Użytkownika, który zainicjował operację
* Czas wystąpienia operacji
* Stan operacji
* Wartości innych właściwości, które mogą ułatwić zbadanie operacji

Dziennik inspekcji zawiera informacje o wszystkich operacjach zapisu (PUT, POST, DELETE) wykonywanych względem zasobów, ale nie zawiera informacji dotyczących operacji odczytu (GET).

## <a name="troubleshooting-monitoring-agent-installation-in-windows"></a>Rozwiązywanie problemów z instalacją agenta monitorowania w systemie Windows
Agent monitorowania usługi Security Center służy do przeprowadzania zbierania danych. Po włączeniu zbierania danych i poprawnym zainstalowaniu agenta na maszynie docelowej następujące procesy powinny być wykonywane:

* ASMAgentLauncher.exe — agent monitorowania platformy Azure 
* ASMMonitoringAgent.exe — rozszerzenie monitorowania zabezpieczeń platformy Azure
* ASMSoftwareScanner.exe — menedżer skanowania platformy Azure

Rozszerzenie Azure Security Monitoring skanuje pod kątem różnych konfiguracji związanych z zabezpieczeniami i zbiera dzienniki zabezpieczeń z maszyny wirtualnej. Menedżer skanowania będzie używany jako skaner poprawek.

Jeśli instalacja zostanie przeprowadzona pomyślnie, w dziennikach inspekcji dla docelowej maszyny wirtualnej powinien zostać wyświetlony wpis podobny do przedstawionego poniżej:

![Dzienniki inspekcji](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

Więcej informacji na temat procesu instalacji można również uzyskać, odczytując dzienniki agentów znajdujące się w katalogu *%systemdrive%\windowsazure\logs* (na przykład: C:\WindowsAzure\Logs).

> [!NOTE]
> Jeśli agent Azure Security Center nie działa poprawnie, należy ponownie uruchomić docelową maszynę wirtualną, ponieważ nie istnieje żadne polecenie służące do zatrzymywania i uruchamiania agenta.


Jeśli nadal będziesz mieć problemy z kolekcją danych, możesz odinstalować agenta, wykonując następujące kroki poniżej:

1. W witrynie **Azure Portal** wybierz maszynę wirtualną, na której występują problemy z kolekcją danych, i kliknij pozycję **Rozszerzenia**.
2. Kliknij prawym przyciskiem myszy pozycję **Microsoft.Azure.Security.Monitoring** i wybierz pozycję **Odinstaluj**.

![Usuwanie agenta](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig4.png)

Rozszerzenie Azure Security Monitoring powinno automatycznie zainstalować siebie ponownie w ciągu kilku minut.

## <a name="troubleshooting-monitoring-agent-installation-in-linux"></a>Rozwiązywanie problemów z instalacją agenta monitorowania w systemie Linux
Podczas rozwiązywania problemów z instalacją agenta maszyny wirtualnej w systemie Linux należy upewnić się, że rozszerzenie zostało pobrane do katalogu /var/lib/waagent/. Można uruchomić poniższe polecenie, aby sprawdzić, czy rozszerzenie zostało zainstalowane:

`cat /var/log/waagent.log` 

Inne pliki dziennika, które można przeglądać w celu rozwiązania problemów, to: 

* /var/log/mdsd.err
* /var/log/azure/

W działającym systemie powinno istnieć połączenie z procesem mdsd na porcie 29130 protokołu TCP. Jest to program Syslog komunikujący się z procesem mdsd. To zachowanie można walidować, uruchamiając poniższe polecenie:

`netstat -plantu | grep 29130`

## <a name="troubleshooting-endpoint-protection-not-working-properly"></a>Rozwiązywanie problemów z niedziałającą prawidłowo ochroną punktów końcowych

Agent gościa to proces nadrzędny w stosunku do wszystkich czynności wykonywanych przez rozszerzenie [Usługa firmy Microsoft chroniąca przed złośliwym kodem](../security/azure-security-antimalware.md). Jeśli proces agenta gościa zakończy się niepowodzeniem, usługa firmy Microsoft chroniąca przed złośliwym kodem uruchomiona jako proces podrzędny agenta gościa może również zakończyć się niepowodzeniem.  W przypadku takich scenariuszy zalecamy zweryfikowanie następujących opcji:

- Jeśli docelowa maszyna wirtualna to obraz niestandardowy, a twórca maszyny wirtualnej nigdy nie zainstalował agenta gościa.
- Jeśli cel to maszyna wirtualna z systemem Linux, a nie Windows, instalowanie rozszerzenia usługi firmy Microsoft chroniącej przed złośliwym kodem w wersji dla systemu Windows na maszynie wirtualnej z systemem Linux zakończy się niepowodzeniem. Agent gościa z systemem Linux ma określone wymagania dotyczące wersji systemu operacyjnego i wymaganych pakietów. Jeśli nie zostaną one spełnione, agent maszyny wirtualnej również nie będzie działać. 
- Jeśli maszyna wirtualna została utworzona za pomocą starszej wersji agenta gościa. W takiej sytuacji należy pamiętać, że niektórzy starsi agenci nie mogą przeprowadzić automatycznie samodzielnej aktualizacji i może to prowadzić do wystąpienia tego problemu. Zawsze używaj najnowszej wersji agenta gościa podczas tworzenia własnych obrazów.
- Niektóre programy administracyjne innych firm mogą wyłączać agenta gościa lub blokować dostęp do niektórych lokalizacji plików. Jeśli na maszynie wirtualnej zainstalowano programy innych firm, upewnij się, że agent znajduje się na liście wykluczeń.
- Niektóre ustawienia zapory lub grupy zabezpieczeń sieci (NSG) mogą blokować ruch sieciowy do i z agenta gościa.
- Niektóre listy kontroli dostępu (ACL) mogą uniemożliwiać dostęp do dysku.
- Brak miejsca na dysku może uniemożliwić prawidłowe działanie agenta gościa. 

Domyślnie interfejs użytkownika usługi firmy Microsoft chroniącej przed złośliwym kodem jest wyłączony. Więcej informacji na temat jego włączania w razie potrzeby można znaleźć w temacie [Enabling Microsoft Antimalware User Interface on Azure Resource Manager VMs Post Deployment](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/09/enabling-microsoft-antimalware-user-interface-post-deployment/) (Włączanie interfejsu użytkownika usługi firmy Microsoft chroniącej przed złośliwym kodem po wdrożeniu maszyn wirtualnych usługi Azure Resource Manager).

## <a name="troubleshooting-problems-loading-the-dashboard"></a>Rozwiązywanie problemów z ładowaniem pulpitu nawigacyjnego

Jeśli masz problemy z ładowaniem pulpitu nawigacyjnego usługi Security Center, upewnij się, że użytkownik, który rejestruje subskrypcję usługi Security Center (tj. pierwszy użytkownik, który otworzył usługę Security Center za pomocą subskrypcji), oraz użytkownik, który chce włączyć kolekcję danych, mają uprawnienia *Właściciel* lub *Współautor* w subskrypcji. Od tej pory również użytkownicy z uprawnieniem *Czytelnik* w subskrypcji będą widzieć pulpit nawigacyjny/alerty/rekomendacje/zasady.

## <a name="contacting-microsoft-support"></a>Kontaktowanie się z pomocą techniczną firmy Microsoft
Niektóre problemy można zidentyfikować za pomocą wskazówek znajdujących się w tym artykule. Inne problemy można znaleźć udokumentowane na publicznym [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter) usługi Security Center. Jeśli jednak potrzebujesz dalszych wskazówek dotyczących rozwiązywania problemów, możesz otworzyć nowe żądanie pomocy technicznej, używając witryny **Azure Portal** w sposób pokazany poniżej: 

![Pomoc techniczna firmy Microsoft](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)

## <a name="see-also"></a>Zobacz też
W tym dokumencie przedstawiono konfigurowanie zasad zabezpieczeń w Centrum zabezpieczeń Azure. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

* [Przewodnik planowania i obsługi usługi Azure Security Center](security-center-planning-and-operations-guide.md)— informacje na temat planowania i zagadnień projektowych podczas wdrażania usługi Azure Security Center.
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure
* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md) — informacje na temat reagowania na alerty zabezpieczeń i zarządzania nimi
* [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure




<!--HONumber=Feb17_HO3-->


