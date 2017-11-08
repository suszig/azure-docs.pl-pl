---
title: "Pulpit nawigacyjny usługi Menedżer StorSimple | Dokumentacja firmy Microsoft"
description: "Zawiera opis pulpit nawigacyjny usługi Menedżer StorSimple i wyjaśniono, jak korzystać z niego do monitorowania kondycji rozwiązania StorSimple."
services: storsimple
documentationcenter: 
author: SharS
manager: carmonm
editor: 
ms.assetid: fb0f131d-d60b-45d7-ace2-56d0502e6627
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: 62273c0093876f136d97eedf5a509f0b306a1379
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-dashboard"></a>Pulpit nawigacyjny usługi Menedżer StorSimple
> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Aby wyświetlić wersję tego artykułu dla nowego portalu Azure, przejdź do [pulpit nawigacyjny usługi Menedżer StorSimple](storsimple-8000-service-dashboard.md). Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Omówienie
Na stronie pulpitu nawigacyjnego usługi Menedżer StorSimple zawiera widok podsumowania wszystkich urządzeń, które są podłączone do usługi Menedżer StorSimple, wyróżnianie tych, które wymagają uwagi administratora systemu. W tym samouczku wprowadza strony pulpitu nawigacyjnego, opisano zawartości pulpitu nawigacyjnego i funkcji oraz opisano zadania, które można wykonywać na tej stronie.

![Pulpit nawigacyjny usług](./media/storsimple-service-dashboard/HCS_ServiceDashboard.png)

Pulpit nawigacyjny usługi Menedżer StorSimple zawiera następujące informacje:

* W **wykresu** obszaru, zobaczysz wykres metryki istotne dla urządzeń. Możesz wyświetlić podstawowy magazynu (lokalnie przypięty i warstwowa) używany przez wszystkie urządzenia, a także w magazynie w chmurze używane przez urządzenia w danym okresie czasu. Użyj opcji w prawym górnym rogu wykresu, aby określić przedział czasu 1 tydzień, 1-miesięcznego, 3-miesięczna lub 1 rok.
* **Przegląd wykorzystania** przedstawia podstawowy pamięci masowej, udostępniane i używane przez wszystkie urządzenia względem magazynu całkowita, która jest dostępna dla wszystkich urządzeń. **Zainicjowano obsługę administracyjną** odnosi się do magazynu, który jest przygotowany i przydzielona do użycia, podczas gdy **używane** odwołuje się do użycia woluminów, tak jak to pokazano inicjatorzy, które są podłączone do urządzenia.
* **Alerty** obszaru zawiera migawkę wszystkich aktywnych alertów dla wszystkich urządzeń pogrupowane według ważności alertu. Poziom ważności kliknięcie spowoduje otwarcie **alerty** strony zakres do wyświetlenia alertów. Na **alerty** strony, możesz kliknąć alert indywidualny w celu wyświetlenia dodatkowych szczegółów dotyczących tego alertu, w tym wszystkie zalecane działania. Można także wyczyścić alert, jeśli ten problem został rozwiązany.
* **Zadania** obszaru zawiera migawkę ostatnich zadań dla wszystkich urządzeń, które są połączone z usługą. Brak łącza, które można przyjrzeć się zadania, które są obecnie w toku, te, których nie powiodła się w ciągu ostatnich 24 godzin lub te, które są zaplanowane do uruchomienia w ciągu 24 godzin.
* **Szybkiego dostępu** obszaru zawiera przydatne informacje takie jak stan usługi, liczba urządzeń podłączonych do usługi, lokalizację usługi i szczegóły subskrypcji, która jest skojarzona z usługą. Istnieje również link do dziennika operacji. Kliknij łącze, aby wyświetlić listę wszystkich ukończone operacje usługi Menedżer StorSimple.

Na stronie pulpitu nawigacyjnego usługi Menedżer StorSimple umożliwia zainicjowanie następujących zadań:

* Wyświetl lub ponownie wygenerować klucz rejestracji usługi.
* Należy zmienić wartość klucza szyfrowania danych usługi.
* Sprawdź dzienniki operacji.

## <a name="view-or-regenerate-the-service-registration-key"></a>Wyświetl lub ponownie wygenerować klucz rejestracji usługi
Klucz rejestracji usługi służy do rejestrowania urządzeń Microsoft Azure StorSimple w usłudze Menedżer StorSimple, tak, aby urządzenia zostanie wyświetlony w klasycznym portalu Azure dodatkowe akcje zarządzania. Klucz jest tworzone na pierwszym urządzeniem i udostępniać pozostałe urządzenia.

Kliknięcie przycisku **klucz rejestracji** (w dolnej części strony) otwiera **klucz rejestracji usługi** okno dialogowe, w którym można skopiować do Schowka bieżący klucz rejestracji usługi lub ponownie wygenerować klucz rejestracji usługi.

Trwa ponowne generowanie klucza nie ma wpływu na wcześniej zarejestrowane urządzenia: ma wpływ tylko na urządzeniach, które są zarejestrowane w usłudze po klucz zostanie ponownie wygenerowany.

Aby uzyskać więcej informacji na temat przeglądania i generowanie klucza rejestracji usługi, przejdź do [pobieranie klucza rejestracji usługi](storsimple-manage-service.md#get-the-service-registration-key).

## <a name="change-the-service-data-encryption-key"></a>Zmiana klucza szyfrowania danych usługi
Klucze szyfrowania danych usługi są używane do szyfrowania danych poufnych klienta, takich jak poświadczeń konta magazynu, które są wysyłane z usługi Menedżer StorSimple na urządzeniu StorSimple. Należy zmienić te klucze okresowo, jeśli Twoja organizacja IT zasady rotacją kluczy w urządzeniach magazynujących. Proces zmiany klucza może być nieznacznie różnić w zależności od tego, czy istnieje jednego lub wielu urządzeniach zarządzanych przez usługę Menedżer StorSimple.

Zmiana klucza szyfrowania danych usługi jest procesem krok 3:

1. Przy użyciu klasycznego portalu Azure, autoryzacji urządzeń można zmienić klucza szyfrowania danych usługi.
2. Przy użyciu programu Windows PowerShell dla urządzenia StorSimple, inicjują zmiany klucza szyfrowania danych usługi.
3. Jeśli masz więcej niż jedno urządzenie StorSimple, należy zaktualizować klucz szyfrowania danych usługi na innych urządzeniach.

W poniższych krokach opisano proces przerzucania klucza szyfrowania danych usługi.

[!INCLUDE [storsimple-change-data-encryption-key](../../includes/storsimple-change-data-encryption-key.md)]

## <a name="view-the-operations-logs"></a>Sprawdź dzienniki operacji
Dzienniki operacji można wyświetlić, klikając łącze dzienniki operacji dostępne w **szybkiego dostępu** okienku pulpitu nawigacyjnego. Spowoduje to przejście do strony usługi zarządzania, których można filtrować i znajduje się w dziennikach, które są określone w usłudze Menedżer StorSimple.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [Rozwiązywanie problemów z urządzeniem StorSimple](storsimple-troubleshoot-operational-device.md).
* Dowiedz się więcej o sposobie [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

