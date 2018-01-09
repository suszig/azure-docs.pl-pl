---
title: "Centrum zabezpieczeń Azure — Szybki Start - dołączeniu komputerów do Centrum zabezpieczeń systemu Linux | Dokumentacja firmy Microsoft"
description: "Ta opcja szybkiego startu pokazano, jak dołączyć komputery Linux do Centrum zabezpieczeń."
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
ms.openlocfilehash: 365afd2199b1b8b2e70d882f6a729384edbdffbc
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-linux-computers-to-azure-security-center"></a>Szybki Start: Komputery z systemem Linux dołączyć Centrum zabezpieczeń Azure
Po dołączeniu subskrypcji Azure, można włączyć Centrum zabezpieczeń dla systemu Linux zasoby z uruchomionym poza platformą Azure, na przykład lokalnie lub w innych chmur, aprowizując agenta systemu Linux.

Ta opcja szybkiego startu przedstawia sposób instalowania agenta systemu Linux na komputerze z systemem Linux.

## <a name="prerequisites"></a>Wymagania wstępne
Do rozpoczęcia korzystania z usługi Security Center wymagana jest subskrypcja usługi Microsoft Azure. Jeśli nie masz subskrypcji, możesz zarejestrować się, aby uzyskać dostęp do [bezpłatnego konta](https://azure.microsoft.com/pricing/free-trial/).

Przed uruchomieniem tego przewodnika Szybki Start, musi być w Centrum zabezpieczeń warstwy cenowej standardowa. Zobacz [Onboard subskrypcją Azure Security Center Standard](security-center-get-started.md) instrukcje uaktualniania. Możesz spróbować Standard Centrum zabezpieczeń bezpłatnie przez pierwsze 60 dni.

## <a name="add-new-linux-computer"></a>Dodaj nowy komputer z systemem Linux

1. Zaloguj się do [Azure Portal](https://azure.microsoft.com/features/azure-portal/).
2. W menu platformy **Microsoft Azure** wybierz pozycję **Security Center**. **Centrum zabezpieczeń — omówienie** otwiera.

 ![Omówienie Centrum zabezpieczeń][2]

3. W menu głównym Centrum zabezpieczeń, wybierz **dołączania do zaawansowanych zabezpieczeń**.
4. Wybierz **czy chcesz dodać komputery z systemem innym niż Azure**.
   ![Rozpocząć korzystanie z zaawansowanych zabezpieczeń][3]

5. Na **dodać nowe komputery z systemem innym niż Azure**, przedstawiono listę obszarów roboczych analizy dzienników. Lista zawiera, jeśli to konieczne, domyślny obszar roboczy utworzony przez Centrum zabezpieczeń podczas automatycznego inicjowania obsługi administracyjnej został włączony. Wybierz ten obszar roboczy lub innego obszaru roboczego, który ma zostać użyty.

    ![Dodać komputer z systemem innym niż Azure][4]

6.  Na **bezpośredniego agenta** w obszarze **pobierania i DOŁĄCZYĆ AGENTA dla systemu LINUX**, wybierz pozycję **kopiowania** przycisk, aby skopiować *wget* polecenia.

7.  Otwórz program Notatnik i Wklej tego polecenia. Zapisz ten plik do lokalizacji, która może być dostępny z komputera z systemem Linux.

## <a name="install-the-agent"></a>Zainstaluj agenta

1.  Na komputerze z systemem Linux Otwórz plik, który został wcześniej zapisany. Wybierz całą zawartość, kopiowania, otwórz konsolę terminali i wklej poniższe polecenie.
2.  Po zakończeniu instalacji możesz można sprawdzić, czy *omsagent* jest zainstalowany, uruchamiając *pgrep* polecenia. Polecenie zwróci błąd *omsagent* PID (identyfikator procesu) w sposób przedstawiony poniżej:

  ![Zainstaluj agenta][5]

Dzienniki Centrum zabezpieczeń agenta dla systemu Linux można znaleźć w folderze: */var/opt/microsoft/omsagent/<workspace id>/log/*

  ![Dzienniki dla agenta][6]

Po pewnym czasie może potrwać do 30 minut, nowy komputer z systemem Linux będą wyświetlane w Centrum zabezpieczeń.

Teraz można monitorować z maszynami wirtualnymi Azure i komputerów z systemem innym niż Azure w jednym miejscu. W obszarze **obliczeniowe**, masz przegląd wszystkich maszyn wirtualnych i komputerach wraz z zaleceniami. Każda kolumna reprezentuje jeden zestaw zaleceń. Kolor reprezentuje maszyny Wirtualnej lub komputera bieżący stan zabezpieczeń dla tego zalecenia. Centrum zabezpieczeń udostępnia również wszystkie wykrywania dla tych komputerów w alertów zabezpieczeń.

  ![Obliczeniowe bloku][7] istnieją dwa typy ikon reprezentowane na **obliczeniowe** bloku:

  ![icon1](./media/quick-onboard-linux-computer/security-center-monitoring-icon1.png) Komputer bez Azure

  ![icon2](./media/quick-onboard-linux-computer/security-center-monitoring-icon2.png) Maszyna wirtualna platformy Azure

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Jeśli nie jest już potrzebne, można usunąć agenta z komputera z systemem Linux.

Aby usunąć agenta:

1. Pobierz agenta systemu Linux [uniwersalnego skryptu](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) do komputera.
2. Uruchom plik SH pakietu z *--przeczyścić* argumentu na komputerze, który powoduje całkowite usunięcie agenta i jego konfiguracja.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

## <a name="next-steps"></a>Kolejne kroki
W tym szybki start aprowizowanej agenta na komputerze z systemem Linux. Aby dowiedzieć się więcej na temat sposobu korzystania z Centrum zabezpieczeń, nadal samouczek do konfigurowania zasad zabezpieczeń i oceny zabezpieczeń zasobów.

> [!div class="nextstepaction"]
> [Samouczek: Definiowanie i oceny zasad zabezpieczeń](tutorial-security-policy.md)

<!--Image references-->
[1]: ./media/quick-onboard-linux-computer/portal.png
[2]: ./media/quick-onboard-linux-computer/overview.png
[3]: ./media/quick-onboard-linux-computer/onboard-windows-computer.png
[4]: ./media/quick-onboard-linux-computer/add-computer.png
[5]: ./media/quick-onboard-linux-computer/pgrep-command.png
[6]: ./media/quick-onboard-linux-computer/logs-for-agent.png
[7]: ./media/quick-onboard-linux-computer/compute.png
