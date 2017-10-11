---
title: "Znaleziono żadnej grupy łącznika pracy dla aplikacji serwera Proxy aplikacji | Dokumentacja firmy Microsoft"
description: "Rozwiąż problemy, które mogą wystąpić po nie pracy łącznika w grupie łącznika dla aplikacji z serwer Proxy aplikacji usługi Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4945958deedc8a1d9989ff901192c03a5363b4dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Nie znaleziono aplikacji serwera Proxy aplikacji grupy łącznika pracy

Ten artykuł pomocy można rozwiązywać typowe problemy, które muszą ponieść gdy nie jest łącznika dla aplikacji serwera Proxy aplikacji wykryte zintegrowane z usługą Azure Active Directory.

## <a name="overview-of-steps"></a>Omówienie kroków
Jeśli nie ma żadnych pracy łącznika w grupie łącznika dla aplikacji, istnieje kilka sposobów, aby rozwiązać ten problem:

-   Jeśli nie łączniki do grupy, można:

    -   Pobierz nowy łącznik na serwerze lokalnego prawo i przypisz je do tej grupy

    -   Przenieś łącznika usługi active do grupy

-   Jeśli nie łączniki active w grupie, można:

    -   Zidentyfikowanie przyczyny, dla której Twojego łącznika jest nieaktywny i rozwiązanie

    -   Przenieś łącznika usługi active do grupy

Aby wiedzieć, który z nich jest problem, otwórz menu "Serwer Proxy aplikacji" w aplikacji i przyjrzyj się komunikat ostrzegawczy grupy łącznika. Określ go, że grupy wymaga co najmniej jeden łącznik (użytkownik nie masz żadnej grupy) lub że nie ma żadnych aktywnych łączników (Jeśli masz prawdopodobnie nieaktywne łączniki).

   ![Wybór grupy łącznika w portalu Azure](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Aby uzyskać szczegółowe informacje na każdej z tych opcji zobacz odpowiedniej sekcji poniżej. Każdy z tych założono uruchamiasz na stronie zarządzania łącznika. Jeśli jest wyświetlany komunikat o błędzie powyżej, można przejść do tej strony, klikając w komunikacie ostrzegawczym. W przeciwnym razie to można znaleźć, przechodząc do **usługi Azure Active Directory**, klikając pozycję na **aplikacje dla przedsiębiorstw**, następnie **serwera Proxy aplikacji.**

   ![Łącznik grupy zarządzania w portalu Azure](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Pobierz nowy łącznik

Aby pobrać nowy łącznik, przycisk "Pobierz łącznik" w górnej części strony.

Uwaga łącznika musi być zainstalowany na komputerze z bezpośredniego procesów of wiersza do wewnętrznej bazy danych aplikacji i zwykle znajduje się na tym samym serwerze co aplikacja. Po pobraniu, łącznik powinna pojawić się w tym menu. Kliknij łącznik i używając listy rozwijanej "łącznik grupy" Upewnij się, że należy on do grupy prawo. Zapisz zmiany.

   ![Pobierz łącznik z portalu Azure](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Przenieś łącznika usługi Active

Jeśli masz łącznika usługi active powinna należeć do grupy i ma procesów z wiersza do zaplecza aplikacji docelowej, można przenieść łącznika w przypisanej grupie. Aby to zrobić, kliknij przycisk łącznika. W polu "Łącznik grupy" Użyj listy rozwijanej wybierz poprawną grupę, a następnie kliknij przycisk Zapisz.

## <a name="resolve-an-inactive-connector"></a>Rozwiąż nieaktywne łącznika

Jeśli nie są aktywne tylko łączników w grupie, prawdopodobnie na komputerze, na którym nie ma wszystkie niezbędne porty odblokowany.

znaleźć w dokumencie Rozwiązywanie problemów dotyczących portów szczegółowe badanie tego problemu.

## <a name="next-steps"></a>Następne kroki
[Zrozumienie łączniki serwera Proxy aplikacji usługi Azure AD](application-proxy-understand-connectors.md)


