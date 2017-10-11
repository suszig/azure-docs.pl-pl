---
title: "Tworzenie przepływów pracy za pomocą szablonów - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Wprowadzenie — szybkie tworzenie przepływów pracy przy użyciu szablonów aplikacji logiki platformy Azure, aby połączyć aplikacje i integrowanie danych."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 3656acfb-eefd-4e75-b5d2-73da56c424c9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 89272869f7dfaa34cbd2ad32d67dca0955e6158b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-workflow-using-a-pre-built-template-or-pattern-to-get-started-quickly"></a>Konfigurowanie przepływu pracy za pomocą wbudowanych szablonów lub wzorca szybkie rozpoczęcie pracy

## <a name="what-are-logic-app-templates"></a>Co to są szablony aplikacji logiki
Szablon aplikacji logiki jest aplikacji logiki wbudowanych, które umożliwiają szybkie rozpoczęcie tworzenia własnych przepływu pracy. 

Te szablony są dobrze do odnajdywania różnych wzorców, które mogą zostać skompilowane przy użyciu aplikacji logiki. Możesz użyć tych szablonów jako- lub zmodyfikować je do danego scenariusza.

## <a name="overview-of-available-templates"></a>Omówienie dostępnych szablonów
Istnieje wiele dostępnych szablonów, w obecnie opublikowanych przez platformę aplikacji logiki. Poniżej przedstawiono niektóre przykładowe kategorie, a także typu łączników używane w nich.

### <a name="enterprise-cloud-templates"></a>Szablony Enterprise chmury
Szablony, które integracji usługi Dynamics CRM, Salesforce pole, obiektów Blob platformy Azure i inne łączniki do potrzeb chmury przedsiębiorstwa. Przykłady co można zrobić za pomocą te szablony obejmują organizowanie potencjalnych klientów i tworzenie kopii zapasowej danych plików firmowych.

### <a name="enterprise-integration-pack-templates"></a>Szablony pakietów integracji przedsiębiorstwa
Konfiguracje VETER (Sprawdzanie poprawności, extract, transform, uzupełnić, trasy) potoki odbieranie X12 EDI dokumentów za pośrednictwem AS2 i przekształcenia go do pliku XML, jak również komunikat X12 i AS2 obsługi.

### <a name="protocol-pattern-templates"></a>Protokół wzorzec szablonów
Te szablony składają się z aplikacji logiki, które zawierają protokołu wzorców, takich jak żądanie odpowiedź za pośrednictwem protokołu HTTP, a także integracji między FTP i SFTP. Użyj tych istniejących lub jako podstawy do tworzenia bardziej złożonych wzorców protokołu.  

### <a name="personal-productivity-templates"></a>Szablony produktywność
Wzorce, aby zwiększyć produktywność obejmują szablony, które ustawić codzienne przypomnienia, Włącz elementów roboczych ważne do listy zadań do wykonania i zautomatyzować długich zadań krok zatwierdzenia jednego użytkownika.

### <a name="consumer-cloud-templates"></a>Szablony konsumentów chmury
Proste szablonów, które integrują się z usługami mediów społecznościowych, takich jak usługi Twitter, zapas czasu i poczty e-mail, ostatecznie stanie wzmocnienia mediów społecznościowych marketingu inicjatyw. Obejmują one również szablony taką jak kopiowanie mętna, co może pomóc zwiększyć wydajność, aby zaoszczędzić czas tradycyjnie powtarzających się zadań. 

## <a name="how-to-create-a-logic-app-using-a-template"></a>Jak utworzyć aplikację logiki, przy użyciu szablonu
Aby rozpocząć korzystanie z szablon aplikacji logiki, przejdź do projektanta aplikacji logiki. Jeśli wprowadzasz projektanta, otwierając istniejących aplikacji logiki, aplikację logiki automatycznie ładuje w widoku projektanta. Jednak jeśli tworzysz nową aplikację logiki, zobacz na poniższym zrzucie ekranu.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

Na tym ekranie można albo rozpoczynać się od aplikacji logiki puste lub wcześniej utworzonego szablonu. W przypadku wybrania jednego z szablonów można uzyskać dodatkowe informacje. W tym przykładzie używamy *podczas tworzenia nowego pliku w Dropbox, skopiuj go do usługi OneDrive* szablonu.  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Jeśli chcesz użyć szablonu, wystarczy zaznaczyć *użyć tego szablonu* przycisku. Użytkownik zostanie zapytany logować się do konta, na podstawie łączników, które korzysta z szablonu. Lub, jeśli zostało wcześniej ustanowione połączenie z tych łączników, możesz wybrać kontynuować, jak pokazano poniżej.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

Po ustanowieniu połączenia i wybraniu *kontynuować*, otwiera aplikację logiki w widoku projektanta.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

W powyższym przykładzie jak w przypadku wielu szablonów, niektóre pola obowiązkowej właściwości może zostać wypełnione w łączniki; Jednak niektóre mogą nadal wymagana była wartość aby można było prawidłowo wdrożyć aplikację logiki. Jeśli spróbujesz wdrażania bez konieczności wprowadzania niektóre brakujące pola zostanie wyświetlone powiadomienie z komunikatem o błędzie.

Jeśli chcesz powrócić do podglądu szablonu, wybierz *szablony* przycisku na górnym pasku nawigacyjnym. Przełączanie z powrotem do podglądu szablonu, utracisz wszystkie niezapisane postępu. Przed nastąpi przełączenie do podglądu szablonu, zobaczysz komunikat ostrzegawczy informujący o tym.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a>Jak wdrożyć aplikację logiki, utworzonych na podstawie szablonu
Po ładowany szablon i wprowadzone żadne zmiany żądaną opcję Zapisz przycisk w lewym górnym rogu. Spowoduje to zapisanie i publikuje aplikacji logiki.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

Jeśli chcesz więcej informacji na temat dodać kolejne kroki do istniejącego szablonu aplikacji logiki, lub dokonaj edycji ogólnie rzecz biorąc, Dowiedz się więcej o [tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md).

