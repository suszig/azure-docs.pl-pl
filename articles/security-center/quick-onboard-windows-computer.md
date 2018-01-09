---
title: "Centrum zabezpieczeń Azure — Szybki Start - dołączeniu komputerów z systemem Windows do Centrum zabezpieczeń | Dokumentacja firmy Microsoft"
description: "Ta opcja szybkiego startu przedstawia udostępnianie programu Microsoft Monitoring Agent na komputerze z systemem Windows."
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
ms.openlocfilehash: 50cbbca9181d67bc41632a4650c76b9636a72356
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-windows-computers-to-azure-security-center"></a>Szybki Start: Dodać komputery do Centrum zabezpieczeń Azure
Po dołączeniu subskrypcji Azure, można włączyć Centrum zabezpieczeń Zasoby z uruchomionym poza platformą Azure, na przykład lokalnie lub w innych chmur przez Inicjowanie obsługi programu Microsoft Monitoring Agent.

Ta opcja szybkiego startu przedstawia sposób instalowania programu Microsoft Monitoring Agent na komputerze z systemem Windows.

## <a name="prerequisites"></a>Wymagania wstępne
Do rozpoczęcia korzystania z usługi Security Center wymagana jest subskrypcja usługi Microsoft Azure. Jeśli nie masz subskrypcji, możesz zarejestrować się, aby uzyskać dostęp do [bezpłatnego konta](https://azure.microsoft.com/pricing/free-trial/).

Przed uruchomieniem tego przewodnika Szybki Start, musi być w Centrum zabezpieczeń warstwy cenowej standardowa. Zobacz [Onboard subskrypcją Azure Security Center Standard](security-center-get-started.md) instrukcje uaktualniania. Możesz spróbować Standard Centrum zabezpieczeń bezpłatnie przez pierwsze 60 dni.

## <a name="add-new-windows-computer"></a>Dodaj nowy komputer z systemem Windows

1. Zaloguj się do [Azure Portal](https://azure.microsoft.com/features/azure-portal/).
2. W menu platformy **Microsoft Azure** wybierz pozycję **Security Center**. **Centrum zabezpieczeń — omówienie** otwiera.

 ![Omówienie Centrum zabezpieczeń][2]

3. W menu głównym Centrum zabezpieczeń, wybierz **dołączania do zaawansowanych zabezpieczeń**.
4. Wybierz **czy chcesz dodać komputery z systemem innym niż Azure**.

   ![Rozpocząć korzystanie z zaawansowanych zabezpieczeń][3]

5. Na **dodać nowe komputery z systemem innym niż Azure**, przedstawiono listę obszarów roboczych analizy dzienników. Lista zawiera, jeśli to konieczne, domyślny obszar roboczy utworzony przez Centrum zabezpieczeń podczas automatycznego inicjowania obsługi administracyjnej został włączony. Wybierz ten obszar roboczy lub innego obszaru roboczego, który ma zostać użyty.

    ![Dodać komputer z systemem innym niż Azure][4]

  **Bezpośredniego agenta** bloku, w łącze do pobierania agenta systemu Windows i klucze dla Identyfikatora obszaru roboczego do użycia podczas konfigurowania agenta.

6.  Wybierz **Pobierz agenta systemu Windows** łącze mające zastosowanie do typu procesora komputera można pobrać pliku ustawień.

7.  Po prawej stronie **identyfikator obszaru roboczego**, wybierz ikonę kopiowania i wklejania identyfikator do Notatnika.

8.  Po prawej stronie **klucz podstawowy**, wybierz ikonę kopiowania i wklejania klucza do Notatnika.

## <a name="install-the-agent"></a>Zainstaluj agenta
Teraz należy zainstalować pobrany plik na komputerze docelowym.

1. Skopiuj plik na komputerze docelowym i uruchom Instalatora.
2. Na **powitalnej** wybierz pozycję **dalej**.
3. Na **postanowień licencyjnych** strony, przeczytaj licencji, a następnie wybierz **zgadzam się**.
4. Na **Folder docelowy** strony, zmienić lub zachować domyślny folder instalacji, a następnie wybierz **dalej**.
5. Na **opcje instalacji agenta** wybierz Połącz agenta z do Analiza dzienników Azure (OMS), a następnie wybierz pozycję **dalej**.
6. Na **Azure Log Analytics** strony, Wklej **identyfikator obszaru roboczego** i **klucz obszaru roboczego (klucz podstawowy)** skopiowany do Notatnika w poprzedniej procedurze.
7. Jeśli komputer należy zgłosić obszaru roboczego analizy dzienników w chmurze Azure dla instytucji rządowych, wybierz **Azure instytucji rządowych Stanów Zjednoczonych** formularza **chmury Azure** listy rozwijanej.  Jeśli komputer musi komunikować się za pośrednictwem serwera proxy z usługą analizy dzienników, wybierz **zaawansowane** i podaj adres URL i numer portu serwera proxy.
8. Wybierz **dalej** po zakończeniu, podając wymagane ustawienia konfiguracyjne.

  ![Zainstaluj agenta][5]

9. Na **gotowy do instalacji** Przejrzyj wybrane opcje, a następnie wybierz **zainstalować**.
10. Na **konfiguracji została ukończona pomyślnie** wybierz pozycję **Zakończ**

Po ukończeniu instalacji program **Microsoft Monitoring Agent** będzie wyświetlany w **Panelu sterowania**. Można sprawdzić konfigurację istnieje i sprawdź, czy agent jest połączony.

Aby uzyskać więcej informacji na temat instalowania i konfigurowania agenta, zobacz [połączyć komputery](../log-analytics/log-analytics-agent-windows.md#install-the-agent-using-setup).

Teraz można monitorować z maszynami wirtualnymi Azure i komputerów z systemem innym niż Azure w jednym miejscu. W obszarze **obliczeniowe**, masz przegląd wszystkich maszyn wirtualnych i komputerach wraz z zaleceniami. Każda kolumna reprezentuje jeden zestaw zaleceń. Kolor reprezentuje maszyny Wirtualnej lub komputera bieżący stan zabezpieczeń dla tego zalecenia. Centrum zabezpieczeń udostępnia również wszystkie wykrywania dla tych komputerów w alertów zabezpieczeń.

  ![Obliczenia bazy danych bloku][6]

Istnieją dwa typy ikon reprezentowane na **obliczeniowe** bloku:

![icon1](./media/quick-onboard-windows-computer/security-center-monitoring-icon1.png) Komputer bez Azure

![icon2](./media/quick-onboard-windows-computer/security-center-monitoring-icon2.png) Maszyna wirtualna platformy Azure

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Gdy nie są już potrzebne, można usunąć agenta z komputera z systemem Windows.

Aby usunąć agenta:

1. Otwórz **Panel sterowania**.
2. Otwórz aplet **Programy i funkcje**.
3. W obszarze **Programy i funkcje** wybierz pozycję **Microsoft Monitoring Agent** i kliknij przycisk **Odinstaluj**.

## <a name="next-steps"></a>Kolejne kroki
W tym szybkiego startu obsługi administracyjnej programu Microsoft Monitoring Agent na komputerze z systemem Windows. Aby dowiedzieć się więcej na temat sposobu korzystania z Centrum zabezpieczeń, nadal samouczek do konfigurowania zasad zabezpieczeń i oceny zabezpieczeń zasobów.

> [!div class="nextstepaction"]
> [Samouczek: Definiowanie i oceny zasad zabezpieczeń](tutorial-security-policy.md)

<!--Image references-->
[2]: ./media/quick-onboard-windows-computer/overview.png
[3]: ./media/quick-onboard-windows-computer/onboard-windows-computer.png
[4]: ./media/quick-onboard-windows-computer/add-computer.png
[5]: ./media/quick-onboard-windows-computer/log-analytics-mma-setup-laworkspace.png
[6]: ./media/quick-onboard-windows-computer/compute.png
