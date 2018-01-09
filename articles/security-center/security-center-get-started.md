---
title: "Centrum zabezpieczeń Azure — Szybki Start - dołączyć subskrypcją Azure Security Center Standard | Dokumentacja firmy Microsoft"
description: "Ta opcja szybkiego startu pokazano, jak uaktualnić do Centrum zabezpieczeń warstwy cenowej standardowa celu zapewnienia dodatkowych zabezpieczeń."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/07/2018
ms.author: terrylan
ms.openlocfilehash: ac4e3b36b08223f7e3b54850ed53a8185e85f0d2
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-your-azure-subscription-to-security-center-standard"></a>Szybki Start: Dołączyć subskrypcją Azure Security Center Standard
Centrum zabezpieczeń Azure zapewnia ujednolicone zarządzanie i zagrożeń ochronę dla obciążeń chmury hybrydowej. Gdy warstwę bezpłatna zapewnia ograniczone bezpieczeństwo dla zasobów platformy Azure tylko, warstwy standardowa rozszerza możliwości lokalnych i innych chmur. Standard Centrum zabezpieczeń pomaga wyszukiwania i napraw luk w zabezpieczeniach, zastosowanie kontroli dostępu i aplikacji w celu blokowania złośliwych działań, wykrywać zagrożenia, korzystając z analizy i analizy oraz szybko reagować, gdy trwa atak. Możesz spróbować Security Center Standard bezpłatnie przez pierwsze 60 dni.

W tym artykule uaktualnienie do warstwy standardowa, aby zwiększyć bezpieczeństwo i zainstalować program Microsoft Monitoring Agent na maszynach wirtualnych do monitorowania luk w zabezpieczeniach i zagrożenia.

## <a name="prerequisites"></a>Wymagania wstępne
Do rozpoczęcia korzystania z usługi Security Center wymagana jest subskrypcja usługi Microsoft Azure. Jeśli nie masz subskrypcji, możesz zarejestrować się, aby uzyskać dostęp do [bezpłatnego konta](https://azure.microsoft.com/pricing/free-trial/).

Aby uaktualnić subskrypcję do warstwy standardowa, musi mieć przypisaną rolę właściciela subskrypcji, współautor subskrypcji lub administratora zabezpieczeń.

## <a name="enable-your-azure-subscription"></a>Włączenie subskrypcji platformy Azure

1. Zaloguj się do [Azure Portal](https://azure.microsoft.com/features/azure-portal/).
2. W menu platformy **Microsoft Azure** wybierz pozycję **Security Center**. **Centrum zabezpieczeń — omówienie** otwiera.

 ![Omówienie Centrum zabezpieczeń][2]

**Centrum zabezpieczeń — omówienie** zapewnia ujednolicone wgląd w stan zabezpieczeń hybrydowego obciążeń chmury, co pozwala na potrzeby odnajdywania i ocenę zabezpieczeń obciążeń i identyfikacji i ograniczenia ryzyka. Centrum zabezpieczeń automatycznie włącza żadnej z Twoich subskrypcji platformy Azure został wcześniej załadowany przez innego użytkownika subskrypcji do warstwy bezpłatna.

Możesz wyświetlić i filtrować listy subskrypcji, klikając **subskrypcje** elementu menu. Centrum zabezpieczeń rozpocznie teraz oceny zabezpieczeń te subskrypcje, aby zidentyfikować luki w zabezpieczeniach. Aby dostosować typy ocen, można zmodyfikować zasady zabezpieczeń. Zasady zabezpieczeń definiują pożądaną konfigurację Twoich obciążeń oraz pomagają zapewnić zgodność z wymaganiami dotyczącymi zabezpieczeń określonymi przez firmę lub przepisy.

W ciągu minut uruchamiania Centrum zabezpieczeń po raz pierwszy może wystąpić błąd:

- **Zalecenia dotyczące** sposobów poprawy zabezpieczeń subskrypcji platformy Azure. Kliknięcie przycisku **zalecenia** kafelka spowoduje uruchomienie listy priorytetów.
- Spis **obliczeniowe**, **sieci**, **magazyn & danych**, i **aplikacji** zasobów, które obecnie są oceniane przez Centrum zabezpieczeń oraz strukturę każdego z nich.

Aby w pełni korzystać z Centrum zabezpieczeń, należy wykonać poniższe kroki, aby uaktualnienie do warstwy standardowa i zainstalować program Microsoft Monitoring Agent.

## <a name="upgrade-to-the-standard-tier"></a>Uaktualnij do planu Standard
Przewodniki Szybki Start Centrum zabezpieczeń i samouczki na potrzeby można uaktualnić do warstwy standardowa. Twoje pierwsze 60 dni są wolne i można było powrócić do warstwy bezpłatna dowolnej chwili.

1. W menu głównym Centrum zabezpieczeń, wybierz **dołączania do zaawansowanych zabezpieczeń**.

2. W obszarze **dołączania z zabezpieczeniami zaawansowanymi**, Centrum zabezpieczeń zawiera listę subskrypcji i kwalifikujących się do dołączenia do obszarów roboczych. Wybierz subskrypcję z listy.

  ![Wybierz subskrypcję][4]

3. **Zasady zabezpieczeń** zawiera informacje na temat grup zasobów zawartych w subskrypcji. **Cennik** otwiera również.
4. W obszarze **cennik**, wybierz pozycję **standardowe** do uaktualnienia z wolnym do standardowego i kliknij przycisk **zapisać**.

  ![Wybierz opcję standardowe][5]

Teraz, gdy wcześniej przeprowadzano uaktualnienie do warstwy standardowa, masz dostęp do dodatkowych funkcji Centrum zabezpieczeń, w tym **formanty adaptacyjną aplikacji**, **na wszelki dostęp do maszyny Wirtualnej czasu**, **zabezpieczeń alerty**, **analizy zagrożeń**, **playbooks automatyzacji**itd. Należy pamiętać, że alerty zabezpieczeń będą wyświetlane tylko, gdy Centrum zabezpieczeń wykryje złośliwe działania.

  ![Alerty zabezpieczeń][7]

## <a name="automate-data-collection"></a>Automatyzowanie zbierania danych
Centrum zabezpieczeń zbiera dane z maszyn wirtualnych platformy Azure i komputerów z systemem innym niż Azure monitorowanie luk w zabezpieczeniach i zagrożeń. Dane są zbierane za pomocą programu Microsoft Monitoring Agent, która odczytuje różnych konfiguracji związanych z zabezpieczeniami i dzienniki zdarzeń z komputera i kopiuje dane do swojego obszaru roboczego do analizy. Domyślnie Centrum zabezpieczeń będzie tworzy nowy obszar roboczy.

Po włączeniu automatycznej obsługi Centrum zabezpieczeń instaluje program Microsoft Monitoring Agent na wszystkich obsługiwanych maszyn wirtualnych platformy Azure i nowe pliki, które są tworzone. Zdecydowanie zalecane jest automatyczne udostępnianie.

Aby włączyć automatyczne Inicjowanie obsługi programu Microsoft Monitoring Agent:

1. W menu głównym Centrum zabezpieczeń, wybierz **zasady zabezpieczeń**.
2. Wybierz subskrypcję.
3. W obszarze **zasady zabezpieczeń**, wybierz pozycję **zbierania danych**.
4. W obszarze **zbierania danych**, wybierz pozycję **na** Aby włączyć automatyczne udostępnianie.
5. Wybierz pozycję **Zapisz**.

  ![Włącz automatyczne Inicjowanie obsługi][6]

Dzięki tej nowej wgląd w maszynach wirtualnych platformy Azure Centrum zabezpieczeń mogą udostępnić dodatkowe zalecenia dotyczące systemu aktualizacji stanu, konfiguracji zabezpieczeń systemu operacyjnego, program endpoint protection, a także wygenerować dodatkowe alerty zabezpieczeń.

  ![Zalecenia][8]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Inne elementy szybkiego startu i samouczki w tej kolekcji zależą od tego przewodnika Szybki Start. Jeśli planujesz na pracę z kolejne Przewodniki Szybki Start i samouczki, nadal działać warstwy standardowa i Zachowaj automatyczne udostępnianie włączone. Jeśli nie planujesz kontynuować, lub chcesz powrócić do warstwy bezpłatna:

1. Wrócić do menu głównego Centrum zabezpieczeń i wybrać **zasady zabezpieczeń**.
2. Wybierz zasady, które chcesz powrócić do wolnego lub subskrypcji. **Zasady zabezpieczeń** otwiera.
3. W obszarze **składniki zasad**, wybierz pozycję **warstwa cenowa**.
4. Wybierz **wolne** zmienić subskrypcję zgodne ze standardem warstwy do warstwy bezpłatna.
5. Wybierz pozycję **Zapisz**.

Jeśli chcesz wyłączyć automatyczne Inicjowanie obsługi administracyjnej:

1. Wrócić do menu głównego Centrum zabezpieczeń i wybrać **zasady zabezpieczeń**.
2. Wybierz subskrypcję, która ma zostać Wyłącz automatyczne udostępnianie.
3. W obszarze **zasady zabezpieczeń — zbieranie danych**, wybierz pozycję **poza** w obszarze **dołączania** do Wyłącz automatyczne udostępnianie.
4. Wybierz pozycję **Zapisz**.

>[!NOTE]
> Wyłączanie automatycznego inicjowania obsługi administracyjnej nie powoduje usunięcia programu Microsoft Monitoring Agent z maszyn wirtualnych platformy Azure, w którym zainicjowano agenta. Wyłączanie automatycznego inicjowania obsługi administracyjnej limitów monitorowania zabezpieczeń dla zasobów.
>

## <a name="next-steps"></a>Kolejne kroki
W tym szybkiego startu uaktualnione do warstwy standardowa i udostępniane między obciążeń chmury hybrydowej programu Microsoft Monitoring Agent zarządzania ujednoliconego zabezpieczeń i ochrony przed zagrożeniami. Aby dowiedzieć się więcej na temat korzystania z Centrum zabezpieczeń, nadal szybkiego startu dla dołączania komputerów z systemem Windows lokalnie i w innych chmur.

> [!div class="nextstepaction"]
> [Szybki Start: Dodać komputery do Centrum zabezpieczeń Azure](quick-onboard-windows-computer.md)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/onboarding.png
[5]: ./media/security-center-get-started/pricing.png
[6]: ./media/security-center-get-started/enable-automatic-provisioning.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
