---
title: Przewodnik rozwiązywania problemów z usługą Azure Security Center | Microsoft Docs
description: Ten dokument pomaga rozwiązywać problemy w usłudze Azure Security Center.
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: ''

ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2016
ms.author: yurid

---
# Przewodnik rozwiązywania problemów z usługą Azure Security Center
Ten przewodnik jest przeznaczony dla specjalistów IT, analityków zabezpieczeń informacji oraz administratorów chmury, których organizacje używają usługi Azure Security Center i muszą rozwiązywać problemy związane z usługą Azure Security Center.

## Przewodnik rozwiązywania problemów
W tym przewodniku wyjaśniano, jak rozwiązywać problemy związane z usługą Security Center. W większości przypadków rozwiązywanie problemów w usłudze Security Center będzie rozpoczynać się od sprawdzenia rekordów [dziennika inspekcji](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) składnika, w przypadku którego wystąpił błąd. Za pomocą dzienników inspekcji można określić:

* Operacje, które zostały wykonane
* Użytkownika, który zainicjował operację
* Czas wystąpienia operacji
* Stan operacji
* Wartości innych właściwości, które mogą ułatwić zbadanie operacji

Dziennik inspekcji zawiera informacje o wszystkich operacjach zapisu (PUT, POST, DELETE) wykonywanych względem zasobów, ale nie zawiera informacji dotyczących operacji odczytu (GET).

## Rozwiązywanie problemów z instalacją agenta monitorowania w systemie Windows
Agent monitorowania usługi Security Center służy do przeprowadzania zbierania danych. Po włączeniu zbierania danych i poprawnym zainstalowaniu agenta na maszynie docelowej następujące procesy powinny być wykonywane:

* ASMAgentLauncher.exe — agent monitorowania platformy Azure 
* ASMMonitoringAgent.exe — rozszerzenie monitorowania zabezpieczeń platformy Azure
* ASMSoftwareScanner.exe — menedżer skanowania platformy Azure

Rozszerzenie monitorowania zabezpieczeń Azure skanuje pod kątem różnych konfiguracji związanych z zabezpieczeniami i zbiera dzienniki zabezpieczeń z maszyny wirtualnej. Menedżer skanowania będzie używany jako skaner poprawek.

Jeśli instalacja zostanie przeprowadzona pomyślnie, w dziennikach inspekcji dla docelowej maszyny wirtualnej powinien zostać wyświetlony wpis podobny do przedstawionego poniżej:

![Dzienniki inspekcji](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

Więcej informacji na temat procesu instalacji można również uzyskać, odczytując dzienniki agentów znajdujące się w katalogu *%systemdrive%\windowsazure\logs* (na przykład: C:\WindowsAzure\Logs).

> [!NOTE]
> Jeśli agent Azure Security Center nie działa poprawnie, należy ponownie uruchomić docelową maszynę wirtualną, ponieważ nie istnieje żadne polecenie służące do zatrzymywania i uruchamiania agenta.
> 
> 

## Rozwiązywanie problemów z instalacją agenta monitorowania w systemie Linux
Podczas rozwiązywania problemów z instalacją agenta maszyny wirtualnej w systemie Linux należy upewnić się, że rozszerzenie zostało pobrane do katalogu /var/lib/waagent/. Można uruchomić poniższe polecenie, aby sprawdzić, czy rozszerzenie zostało zainstalowane:

`cat /var/log/waagent.log` 

Inne pliki dziennika, które można przeglądać w celu rozwiązania problemów, to: 

* /var/log/mdsd.err
* /var/log/azure/

W działającym systemie powinno istnieć połączenie z procesem mdsd na porcie 29130 protokołu TCP. Jest to program Syslog komunikujący się z procesem mdsd. To zachowanie można walidować, uruchamiając poniższe polecenie:

`netstat -plantu | grep 29130`

## Kontaktowanie się z pomocą techniczną firmy Microsoft
Niektóre problemy można zidentyfikować za pomocą wskazówek znajdujących się w tym artykule. Inne problemy można znaleźć udokumentowane na publicznym [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter) usługi Security Center. Jeśli jednak konieczna jest dalsza pomoc w rozwiązywaniu problemów, można otworzyć nowe żądanie pomocy technicznej za pomocą usługi Azure Portal, jak pokazano poniżej: 

![Pomoc techniczna firmy Microsoft](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)

## Zobacz też
W tym dokumencie przedstawiono konfigurowanie zasad zabezpieczeń w Centrum zabezpieczeń Azure. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

* [Przewodnik planowania i obsługi usługi Azure Security Center](security-center-planning-and-operations-guide.md)— informacje na temat planowania i zagadnień projektowych podczas wdrażania usługi Azure Security Center.
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure
* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md) — informacje na temat reagowania na alerty zabezpieczeń i zarządzania nimi
* [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure

<!--HONumber=Sep16_HO3-->


