---
title: "Czy potrzebujesz pomocy przy użyciu portalu Moje aplikacje w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Pobierz instrukcje dotyczące wykonywania typowych zadań, podczas pracy z panelu dostępu."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: c67cd675-b567-41e1-8bc2-e06fe0b38d3b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/21/2017
ms.author: markvi
ms.reviewer: japere
ms.openlocfilehash: 9e2f8aa6ad7534ff822907285aa9fe290a4db586
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2017
---
# <a name="do-you-need-help-with-the-my-apps-portal"></a>Potrzebujesz pomocy przy użyciu portalu Moje aplikacje?

Ta strona została wyświetlona prawdopodobnie, ponieważ Niestety uruchomionym problem przy użyciu portalu Moje aplikacje. Gdy istnieją przypadki, które wymagają skontaktuj się z pomocą techniczną lub administratorem, aby uzyskać problem można rozwiązać, poniżej przedstawiono niektóre tematy dotyczące rozwiązywania problemów, które mogą okazać się pomocne, najpierw.

## <a name="i-am-having-trouble-signing-into-the-my-apps-portal"></a>Mam problemy z logowaniem do portalu Moje aplikacje

Ogólne problemy, aby sprawdzić:

- Sprawdź, czy logujesz się do poprawny adres URL: [https://myapps.microsoft.com](https://myapps.microsoft.com)

- Spróbuj dodać do zaufanych witryn w przeglądarce adres URL.

- Upewnij się, nie wygasł lub zapomnienia hasła. Sprawdź [tutaj](active-directory-passwords-update-your-own-password.md) Aby uzyskać więcej informacji na temat aktualizowania hasła.

- Sprawdź, czy swoje informacje kontaktowe uwierzytelniania jest aktualny i nie blokują dostęp. Sprawdź [tutaj](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/end-user/multi-factor-authentication-end-user) Aby uzyskać więcej informacji na temat konfigurowania swoje informacje uwierzytelniania.

- Wyczyść pliki cookie w przeglądarce, a następnie spróbuj się ponownie zalogować.

Jeśli nadal wystąpią problemy podczas próby zarejestrowania się, skontaktuj się z administratorem, aby uzyskać dalszą pomoc.


## <a name="how-do-i-update-my-password"></a>Jak zaktualizować hasło?

Jeśli nie pamiętasz hasła, nigdy nie otrzymanych z działu IT, zostały zablokowane konta lub chcesz je zmienić, zobacz [pomocy, pamiętam hasła usługi Azure AD](active-directory-passwords-update-your-own-password.md) więcej szczegółów.

## <a name="how-do-i-register-for-password-reset"></a>Jak zarejestrować do resetowania hasła

Jako użytkownik końcowy może zresetować hasło lub odblokować konto bez konieczności mowy do osoby za pomocą Samoobsługowego resetowania hasła (SSPR). Przed użyciem tej funkcji musisz zarejestrować metody uwierzytelniania lub potwierdzić wstępnie zdefiniowane metody uwierzytelniania podane przez administratora. Aby uzyskać więcej informacji, zobacz [rejestrowanie się w celu samodzielnego resetowania hasła](active-directory-passwords-reset-register.md).


## <a name="i-am-having-trouble-installing-the-my-apps-portal-browser-extension"></a>Problemy dotyczące instalowania rozszerzenia przeglądarki portalu Moje aplikacje

Sprawdź, czy spełniasz wymagania dotyczące przeglądarki:

- Portal wymaga przeglądarki obsługującej JavaScript i włączył CSS. Jeśli używasz hasła aplikacji opartych na jednym logowania jednokrotnego towarzyszący rozszerzenia musi być zainstalowany także. To rozszerzenie jest pobierany automatycznie podczas uruchamiania aplikacji, która jest skonfigurowana dla hasła aplikacji opartych na jednym logowania jednokrotnego.

- Wymagania dotyczące przeglądarki rozszerzenia są:
    - Internet Explorer 8, 9, 10, 11, w systemie Windows 7 lub nowszy
    - Krawędź w systemie Windows 10 Anniversary Edition lub nowszy
    - Chrome w systemie Windows 7 lub nowszy oraz System MacOS x lub nowszych
    - Firefox 26.0 lub nowszej w systemie Windows XP SP2 lub nowszym i w systemie Mac OS X 10.6 lub nowszej

Możesz również pobrać rozszerzenia dla programu Chrome i krawędzi, z poniższe linki bezpośrednie:

- [Rozszerzenia programu Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

- [Rozszerzenie krawędzi](https://www.microsoft.com/store/apps/9pc9sckkzk84)

Po instalacji spróbuj wykonać następujące kroki, jeśli wystąpią problemy:

- Sprawdź w ustawieniach rozszerzenia przeglądarki włączenie rozszerzenia.

- Uruchom ponownie przeglądarkę i zaloguj się do portalu Moje aplikacje.

- Wyczyszczenie plików cookie w przeglądarce i zaloguj się do portalu Moje aplikacje.

## <a name="how-do-i-add-a-new-app"></a>Jak dodać nową aplikację?

1.  Na **aplikacje** kliknij przycisk **Dodaj aplikację**.

2.  Wyszukiwanie aplikacji, którą chcesz dodać, a następnie kliknij przycisk **Dodaj**.

**Uwagi:**

- Tylko masz dostęp do tej opcji, jeśli administrator zostały włączone dla Twojego konta.

- Jeśli aplikacja wymaga uprawnień, może być konieczne czekać na zatwierdzenie administratora.


## <a name="how-do-i-manage-my-group-memberships"></a>Jak zarządzać członkostwa w grupach?

1. Kliknij Kafelek grupy aplikacji. 
2. Aby utworzyć grupę, w obszarze grupy własnego, kliknij polecenie Utwórz grupę, a następnie postępuj zgodnie z instrukcjami.
3. Aby dołączyć do grupy, w obszarze grupy w, kliknij dołączanie do grupy, a następnie postępuj zgodnie z instrukcjami.

**Uwagi:**

- Tylko masz dostęp do tej opcji, jeśli administrator zostały włączone dla Twojego konta.

- Grupy, które są członkami umożliwiają wyświetlanie szczegółów i pozostaw grupę.

- Grupy, który jest właścicielem służy do wyświetlania szczegółów, Dodaj lub usuń elementy członkowskie i pozostawić grupę.


## <a name="next-steps"></a>Następne kroki

Powiązane informacje dotyczące rozwiązywania problemów, zobacz [problemów za pomocą aplikacji dostęp do panelu witryny sieci Web lub aplikacji mobilnej](active-directory-application-access-panel-content-map.md)

