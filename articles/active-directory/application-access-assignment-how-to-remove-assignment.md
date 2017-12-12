---
title: "Jak usunąć dostępu użytkownika do aplikacji | Dokumentacja firmy Microsoft"
description: "Zrozumienie sposobu usuwania dostępu użytkownika do aplikacji"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: bb1dc88164aa7971427984b5956e00b1d343cab7
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-remove-a-users-access-to-an-application"></a>Jak usunąć dostępu użytkownika do aplikacji

Ten artykuł ułatwia zrozumienie sposobu usuwania dostępu użytkownika do aplikacji.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Usunąć przypisanie określonego użytkownika lub grupy do aplikacji

Aby usunąć przypisanie grupy do aplikacji lub użytkownika, wykonaj czynności opisane w [Usuń przypisanie użytkownika lub grupy z aplikacji przedsiębiorstwa w usłudze Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) artykułu.

. ## chcę wyłączyć dostęp do aplikacji dla każdego użytkownika

Aby wyłączyć wszystkie logowania użytkownika do aplikacji, wykonaj czynności opisane w [wyłączyć logowania użytkowników dla aplikacji przedsiębiorstwa w usłudze Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) artykułu.

## <a name="i-want-to-delete-an-application-entirely"></a>Chcę, aby całkowicie usunąć aplikację

Aby **usunąć aplikację**, postępuj zgodnie z instrukcjami poniżej:

1.  Otwórz [ **Azure Portal** ](https://portal.azure.com/) i zaloguj się jako **administratora globalnego** lub **ko-administratora.**

2.  Otwórz **rozszerzenia usług Azure Active Directory** klikając **więcej usług** u dołu menu nawigacji głównego po lewej stronie.

3.  Wpisz w **"Azure Active Directory**" w polu wyszukiwania filtr a wybierz **usługi Azure Active Directory** elementu.

4.  Kliknij przycisk **aplikacje dla przedsiębiorstw** w menu nawigacji po lewej stronie usługi Azure Active Directory.

5.  Kliknij przycisk **wszystkie aplikacje** Aby wyświetlić listę wszystkich aplikacji.

   * Jeśli nie ma aplikacji ma tutaj będą wyświetlane, użyj **filtru** kontroli nad **listę wszystkich aplikacji** i ustaw **Pokaż** opcji w celu **wszystkich aplikacji.**

6.  Wybierz aplikację, którą chcesz usunąć.

7.  Po załadowaniu aplikacji, kliknij przycisk **usunąć** ikony z najwyższym aplikacji **omówienie** bloku.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Chcę wyłączyć wszystkie operacje zgody przyszłych użytkownika do aplikacji

Wyłączanie zgody użytkownika dla całego katalogu uniemożliwić użytkownikom końcowym zgodę dowolnej aplikacji. Administratorzy mogą nadal oznacza zgodę na behalves użytkownika. Dowiedz się więcej o zgodę aplikacji i dlaczego może lub nie chcesz to zrobić, przeczytaj [zgody administratora i użytkownika opis](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

Aby **Wyłącz wszystkie operacje zgody użytkownika w przyszłości w katalogu cały**, postępuj zgodnie z instrukcjami poniżej:

1.  Otwórz [ **Azure Portal** ](https://portal.azure.com/) i zaloguj się jako **administratora globalnego.**

2.  Otwórz **rozszerzenia usług Azure Active Directory** klikając **więcej usług** u dołu menu nawigacji głównego po lewej stronie.

3.  Wpisz w **"Azure Active Directory**" w polu wyszukiwania filtr a wybierz **usługi Azure Active Directory** elementu.

4.  Kliknij przycisk **użytkowników i grup** w menu nawigacji.

5.  Kliknij przycisk **ustawienia użytkownika**.

6.  Wyłącz wszystkie operacje zgody użytkownika w przyszłości przez ustawienie **użytkownicy mogą zezwolić aplikacjom na dostęp do danych** Przełącz, aby **nr** i kliknij przycisk **zapisać** przycisku.


# <a name="next-steps"></a>Następne kroki
[Zarządzanie dostępem do aplikacji](active-directory-managing-access-to-apps.md)
