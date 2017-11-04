---
title: "Testowanie pióra | Dokumentacja firmy Microsoft"
description: "Artykuł zawiera omówienie penetracji testowanie procesu (pentest) oraz jak wykonywać pentest względem aplikacji uruchomionych w infrastrukturze Azure."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: yurid
ms.openlocfilehash: 070e848f753452953b9e5dfe94799e7c0a314530
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="pen-testing"></a>Testowanie pióra
Jedną z najważniejszych funkcji testowania aplikacji i wdrażania przy użyciu programu Microsoft Azure jest, że nie należy opracować infrastrukturę lokalną, aby tworzyć, testować i wdrażać aplikacje. Cała infrastruktura jest poświęcony na obsługę przez usług platformy Microsoft Azure. Nie trzeba martwić tworzenia zapotrzebowania, pobierania, a "rozlania i układania" sprzętu lokalnymi.

Jest to doskonały —, ale nadal konieczne upewnij się, że należy wykonać normalnego zabezpieczeń z powodu starannością. Jest jednym z elementów, należy wykonać penetracji Przetestuj aplikacje można wdrożyć na platformie Azure.

Może już wiedzieć, że Microsoft wykonuje [penetracji testowania naszym środowisku Azure](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). To pomoże nam w ulepszeniu platformy i przeprowadza działania pod względem poprawy zabezpieczeń, wprowadzenie nowych kontrolach zabezpieczeń i poprawy naszych procesów zabezpieczeń.

Nie możemy pióra przetestować aplikację dla Ciebie, ale Rozumiemy będzie mają i konieczne jest wykonanie pióra testowania na własnych aplikacji. Że jest to przydatne, ponieważ po podniesieniu bezpieczeństwa aplikacji, należy zabezpieczyć cały ekosystem platformy Azure.

Gdy pióro należy przetestować aplikacje, może wyglądać jak atak z nami. Firma Microsoft [na stałe monitorowanie](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) dla wzorców ataków i zainicjuje proces odpowiedzi na zdarzenia, jeśli chcemy. Go nie będą pomocne, a nie pomoże nam, jeśli wyzwalana odpowiedzi na zdarzenia z powodu powodu pióra starannością testowania.

Co można zrobić w takiej sytuacji?

Gdy wszystko jest gotowe do pióra Przetestuj aplikacje hostowanymi na platformie Azure, masz możliwość [Daj nam znać](https://portal.msrc.microsoft.com/en-us/engage/pentest). Po wiemy, że użytkownik chce się wykonywania określonych testów, firma Microsoft nie będą przypadkowo zamknięty można (na przykład blokowanie adres IP, który w przypadku testowania z), tak długo, jak testy są zgodne z Azure pióra testowania warunki i warunki opisane w [ Microsoft Cloud Unified penetracji testowania zasad zaangażowania](https://technet.microsoft.com/en-us/mt784683).
Standardowych testów, które można wykonać obejmują:

* Testy na punktów końcowych do ujawniania [Otwórz sieci Web aplikacji zabezpieczeń projektu (OWASP) pierwszych 10 luk w zabezpieczeniach](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [Testowanie argumentu rozmycie](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) z punktami końcowymi
* [Skanowanie portów](https://en.wikipedia.org/wiki/Port_scanner) z punktami końcowymi

Jeden typ testu, który nie może wykonać jest dowolnego rodzaju [przeprowadzenie ataku typu "odmowa usługi" (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) ataku. Dotyczy to również inicjowanie sam atak DoS lub przy użyciu powiązanych testy, które może ustalić, pokazują lub symulować dowolnego typu atak DoS.

Czy chcesz rozpocząć pracę za pomocą pióra testowanie aplikacji hostowanej na platformie Microsoft Azure Jeśli tak, a następnie przejdź na za pośrednictwem do [omówienie testów penetracji](https://technet.microsoft.com/library/mt784683.aspx) (i kliknij przycisk Utwórz testowania żądania w dolnej części strony. Będzie także znaleźć więcej informacji na temat pióra testowania warunki i przydatne łącza w sposób można zgłaszania luk w zabezpieczeniach dotyczących Azure lub innych usług firmy Microsoft.
