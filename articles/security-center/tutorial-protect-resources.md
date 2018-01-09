---
title: "Samouczek Centrum zabezpieczeń Azure — ochrona zasobów w Centrum zabezpieczeń Azure | Dokumentacja firmy Microsoft"
description: "W tym samouczku przedstawiono sposób konfigurowania tylko w czasie maszyny Wirtualnej dostęp do zasad i zasady kontroli aplikacji."
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
ms.date: 01/08/2018
ms.author: terrylan
ms.openlocfilehash: f0a32f90e68101f805a52427fab2d5bb29b94939
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2018
---
# <a name="tutorial-protect-your-resources-with-azure-security-center"></a>Samouczek: Ochrona zasobów w Centrum zabezpieczeń Azure
Centrum zabezpieczeń ogranicza poziom narażenia na zagrożenia przy użyciu kontroli dostępu i aplikacji do blokowania złośliwych działań. Tylko w czasie maszyny wirtualnej (VM) dostępu zmniejsza poziom narażenia na ataki, umożliwiając odmowa trwały dostęp do maszyn wirtualnych. Zamiast tego Podaj kontrolowanych i inspekcji dostępu do maszyn wirtualnych tylko w razie potrzeby. Formanty adaptacyjną aplikacji pomocy, ograniczenia funkcjonalności maszyn wirtualnych przed złośliwym oprogramowaniem kontrolowanie, które aplikacje mogą działać na maszyny wirtualne. Usługa Security Center analizuje procesy uruchomione na maszynie wirtualnej przy użyciu uczenia maszynowego i za pomocą tej analizy ułatwia zastosowanie listy reguł elementów dozwolonych.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie tylko w czasie wirtualna zasady dostępu
> * Konfigurowanie zasad kontroli aplikacji

Jeśli nie masz subskrypcji platformy Azure, Utwórz [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/) przed rozpoczęciem.

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków opisanych funkcje omówione w tym samouczku, musi być w Centrum zabezpieczeń warstwy cenowej standardowa. Możesz spróbować Security Center Standard bezpłatnie przez pierwsze 60 dni. Procedury szybkiego startu [Onboard subskrypcją Azure Security Center Standard](security-center-get-started.md) przedstawiono sposób uaktualniania Standard.

## <a name="manage-vm-access"></a>Zarządzanie dostępem do maszyny Wirtualnej
Tylko w czasie dostępu do maszyny Wirtualnej można zablokować ruch przychodzący na maszynach wirtualnych platformy Azure, ograniczenia narażenia na ataki, zapewniając łatwy dostęp do nawiązania połączenia maszyn wirtualnych w razie potrzeby.

Tylko w czasie dostępu do maszyny Wirtualnej jest w wersji zapoznawczej.

Porty zarządzania nie muszą być otwarte przez cały czas. Tylko muszą być otwarte, gdy są połączone z maszyną wirtualną, na przykład w celu wykonywania zadań zarządzania i konserwacji. Gdy tylko w czasie jest włączona, Centrum zabezpieczeń używa reguł sieciowej grupy zabezpieczeń (NSG), które ograniczanie dostępu do portów zarządzania, więc nie może być wskazywane przez osoby atakujące.

1. W menu głównym Centrum zabezpieczeń, wybierz **bezpośrednio w dostęp do maszyny Wirtualnej czasu** w obszarze **zaawansowane OBRONY chmury**.

  ![Dostęp just in time do maszyny wirtualnej][1]

  **Na wszelki dostęp do maszyny Wirtualnej czasu** zawiera informacje o stanie maszyn wirtualnych:

  - **Skonfigurowane** -maszyn wirtualnych, które zostały skonfigurowane do obsługi tylko w dostęp maszyny Wirtualnej.
  - **Zalecane** -maszyn wirtualnych, które obsługują tylko w dostęp maszyny Wirtualnej, ale nie został skonfigurowany do.
  - **Brak rekomendacji** -przyczyny, które mogą spowodować maszyny Wirtualnej, aby nie zaleca się to:

    - Brak grupy NSG — tylko w czasie rozwiązanie wymaga grupy NSG w miejscu.
    - Klasycznym VM - Centrum zabezpieczeń na wszelki dostęp do maszyny Wirtualnej czasu aktualnie obsługuje tylko maszyn wirtualnych wdrożonych za pośrednictwem usługi Azure Resource Manager.
    - Inne - maszyny Wirtualnej jest w tej kategorii jeśli tylko w czasie rozwiązania jest wyłączona w zasadach zabezpieczeń subskrypcji lub grupy zasobów bądź czy maszyna wirtualna nie ma publicznego adresu IP i nie ma grupy NSG w miejscu.

2. Wybierz zalecane maszynę Wirtualną, a następnie kliknij przycisk **JIT włączyć na maszynie Wirtualnej 1** można skonfigurować tylko w czasie zasady dla tej maszyny Wirtualnej:

  Możesz zapisać domyślne porty, które zaleca Centrum zabezpieczeń, lub można dodać i skonfigurować nowy port, na którym chcesz włączyć tylko w rozwiązaniu czasu. W tym samouczku Dodajmy portu wybierając **Dodaj**.

  ![Dodaj konfigurację portu][2]

3. W obszarze **konfiguracji portów Dodaj**, należy określić:

  - Numer portu
  - Typ protokołu
  - Dozwolone źródła adresy IP — zakresy adresów IP mogą uzyskać dostęp na żądanie zatwierdzone
  - Czas żądania maksymalną — maksymalny przedział czasu można otworzyć określonego portu

4. Wybierz **OK** do zapisania.

## <a name="harden-vms-against-malware"></a>VMs wzmacniania ochrony przed złośliwym oprogramowaniem
Formanty aplikacji adaptacyjną pomagają zdefiniować zestaw aplikacji, które mogą być uruchamiane na temat grup zasobów skonfigurowany, która między inne korzyści pomaga zabezpieczyć maszyn wirtualnych przed złośliwym oprogramowaniem. Usługa Security Center analizuje procesy uruchomione na maszynie wirtualnej przy użyciu uczenia maszynowego i za pomocą tej analizy ułatwia zastosowanie listy reguł elementów dozwolonych.

Formanty adaptacyjną aplikacji jest w wersji zapoznawczej. Ta funkcja jest dostępna tylko w przypadku komputerów z systemem Windows.

1. Wróć do menu głównego Centrum zabezpieczeń. W obszarze **zaawansowane OBRONY chmury**, wybierz pozycję **formanty adaptacyjną aplikacji**.

   ![Funkcje adaptacyjnego sterowania aplikacjami][3]

  **Grup zasobów** sekcja zawiera trzy karty:

  - **Skonfigurowane**: Lista zasobów grup zawierających maszyny wirtualne, które zostały skonfigurowane przy użyciu kontroli aplikacji.
  - **Zalecane**: Lista grup zasobów, która aplikacja zaleca się formantu.
  - **Brak rekomendacji**: Lista zasobów grup zawierających maszyny wirtualne bez żadnych zaleceń kontroli aplikacji. Na przykład maszyny wirtualne, na których aplikacje stale się zmieniają i nie osiągają stanu stabilnego.

2. Wybierz **zalecane** kartę Lista grup zasobów z zaleceniami kontroli aplikacji.

  ![Zalecenia dotyczące kontroli aplikacji][4]

3. Wybierz grupę zasobów, aby otworzyć **Utwórz zasady kontroli aplikacji** opcji. W obszarze **Wybierz maszyny wirtualne** zapoznaj się z listą zalecanych maszyn wirtualnych i wyczyść zaznaczenie pól wyboru obok tych, do których nie ma być stosowane sterowanie aplikacjami. W obszarze **Wybierz procesy dla reguł umieszczania na białej liście** zapoznaj się z listą zalecanych aplikacji i wyczyść zaznaczenie pól wyboru obok tych, do których nie ma być stosowana ta funkcja. Lista zawiera:

  - **Nazwa**: Pełna ścieżka aplikacji
  - **PROCESY**: jak wiele aplikacji znajdują się w ścieżce co
  - **TYPOWE**: "Tak" wskazuje, że te procesy zostały wykonane na większości maszyn wirtualnych w tej grupie zasobów
  - **KOŃCZONA**: ikona ostrzeżenia wskazuje, czy aplikacje mogą być użyte przez osobę atakującą do obejścia listę dozwolonych aplikacji podobnej. Zaleca się dokonanie przeglądu tych aplikacji przed ich zatwierdzeniem.

4. Po zakończeniu Opcje wybierz **Utwórz**.

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
W tym samouczku przedstawiono sposób ograniczyć poziom narażenia na zagrożenia przez:

> [!div class="checklist"]
> * Konfigurowanie tylko w dostęp maszyny Wirtualnej zasad w celu zapewnienia kontrolowany i inspekcji dostępu do maszyn wirtualnych tylko w razie potrzeby
> * Konfigurowanie zasad kontroli aplikacji adaptacyjną kontrolowanie, które aplikacje mogą działać na maszyny wirtualne

Przejdź do następnego samouczka, aby dowiedzieć się więcej na temat reagowania na przypadki naruszenia zabezpieczeń.

> [!div class="nextstepaction"]
> [Samouczek: Reagowania na zagrożenia bezpieczeństwa](tutorial-security-incident.md)

<!--Image references-->
[1]: ./media/tutorial-protect-resources/just-in-time-vm-access.png
[2]: ./media/tutorial-protect-resources/add-port.png
[3]: ./media/tutorial-protect-resources/adaptive-application-control-options.png
[4]: ./media/tutorial-protect-resources/recommended-resource-groups.png
