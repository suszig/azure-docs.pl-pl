---
title: Azure ML Workbench informacje o wersji dla przebiegu 3 stycznia 2018
description: "Ten dokument zawiera szczegóły aktualizacje dla wersji przebiegu 3 uczenie Maszynowe Azure"
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 01/22/2018
ms.openlocfilehash: f75fcec3b722563949b6553f17c4f3db3e223675
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/25/2018
---
# <a name="sprint-3---january-2018"></a>Wypalenie w 3 — styczeń 2018 

#### <a name="version-number-01171218263"></a>Numer wersji: 0.1.1712.18263

>Poniżej przedstawiono sposób [Znajdź numer wersji](https://docs.microsoft.com/en-us/azure/machine-learning/preview/known-issues-and-troubleshooting-guide).

Aktualizacja czwarty Azure Machine Learning Workbench — Zapraszamy! Poniżej przedstawiono aktualizacje i usprawnienia w tym przebiegu. Wiele z tych aktualizacji są wykonywane jako bezpośrednio w wyniku opinie użytkowników. 

## <a name="notable-new-features-and-changes"></a>Godne nowe funkcje i zmiany
- Aktualizacje stosu uwierzytelniania wymusza logowania i konta wybór przy uruchamianiu

## <a name="detailed-updates"></a>Szczegółowe aktualizacji
Poniżej znajduje się lista szczegółowe aktualizacji w obszarze każdego składnika uczenie maszynowe Azure w tym przebiegu.

### <a name="workbench"></a>Workbench
- Możliwość instalacji/dezinstalacji aplikacji z apletu Dodaj/Usuń programy
- Aktualizacje stosu uwierzytelniania wymusza wyboru logowania i konto podczas rozruchu
- Udoskonalone środowisko pojedynczy znak na rejestracji jednokrotnej (SSO) w systemie Windows
- Użytkownicy, którzy należą do wielu dzierżawców z innymi poświadczeniami teraz będą mogli zalogować się do środowiska roboczego

#### <a name="ui"></a>UI
- Ogólne poprawki i poprawki

### <a name="notebooks"></a>Komputery przenośne
- Ogólne poprawki i poprawki

### <a name="data-preparation"></a>Przygotowywanie danych 
- Ulepszone sugestie automatycznie podczas wykonywania przekształcenia według przykładu
- Ulepszone algorytm inspektora częstotliwość wzorca
- Umożliwia wysyłanie przykładowych danych i opinie podczas wykonywania przekształcenia przez przykład ![obrazu wysyłania link opinii na pochodzi przekształcenia kolumny](media/release-notes-sprint-3/SendFeedbackFromDeriveColumn.png)
- Ulepszenia środowiska uruchomieniowego Spark
- Scala zastąpił Pyspark
- Stałe brakiem zamknięcia czas inspektora serii danych nie dotyczy 
- Stały czas zawieszenie wykonywania przygotowanie bazy danych dla HDI

### <a name="model-management-cli-updates"></a>Aktualizuje model administracyjny interfejsu wiersza polecenia 
  - Posiadania subskrypcji nie jest już wymagane do obsługi zasobów. Współautor dostępu do grupy zasobów będą wystarczające, aby skonfigurować środowisko wdrażania.
  - Włączone lokalnego środowiska instalacji bezpłatnej subskrypcji 
