---
title: 'Azure Active Directory B2C: rejestrowanie aplikacji | Microsoft Docs'
description: "Jak zarejestrować aplikację w usłudze Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 3/13/2017
ms.author: parakhj
translationtype: Human Translation
ms.sourcegitcommit: 9553c9ed02fa198d210fcb64f4657f84ef3df801
ms.openlocfilehash: e04fbd97dd4d5ecaf12edf47d80572b32d29ed00
ms.lasthandoff: 03/23/2017



---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: rejestrowanie aplikacji

> [!IMPORTANT]
> Aplikacje utworzone w bloku Azure AD B2C w witrynie Azure Portal muszą być zarządzane z tej samej lokalizacji. Jeśli edytujesz aplikacje B2C przy użyciu programu PowerShell lub innego portalu, stają się one nieobsługiwane i prawdopodobnie nie będą działać w usłudze Azure AD B2C.
> 
> 

## <a name="prerequisite"></a>Wymagania wstępne
Aby utworzyć aplikację, która akceptuje tworzenie kont i logowanie użytkowników, musisz najpierw zarejestrować aplikację w dzierżawie usługi Azure Active Directory B2C. Aby utworzyć własną dzierżawę, wykonaj kroki opisane w temacie [Tworzenie dzierżawy usługi Azure AD B2C](active-directory-b2c-get-started.md). Wykonanie wszystkich czynności przedstawionych w tym artykule spowoduje przypięcie bloku funkcji B2C do tablicy startowej.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Przechodzenie do bloku funkcji B2C
Blok funkcji B2C przypięty do tablicy startowej będzie widoczny od razu po zalogowaniu do witryny [Azure Portal](https://portal.azure.com/) w roli administratora globalnego dzierżawy usługi B2C.

Do bloku można również przejść, klikając pozycję **Więcej usług**, a następnie wyszukując usługę **Azure AD B2C** w lewym okienku nawigacji w witrynie [Azure Portal](https://portal.azure.com/).

> [!IMPORTANT]
> Tylko administrator globalny dzierżawy B2C może uzyskiwać dostęp do bloku funkcji B2C. Administrator globalny innej dzierżawy ani użytkownik dowolnej dzierżawy nie mogą uzyskać dostępu do tego bloku.  Na swoją dzierżawę B2C możesz przełączyć się, używając przełącznika dzierżawy w prawym górnym rogu witryny Azure Portal.
> 
> 

## <a name="register-a-web-application"></a>Rejestrowanie aplikacji sieci Web
1. W bloku funkcji B2C w witrynie Azure Portal kliknij pozycję **Aplikacje**.
2. Kliknij pozycję **+Dodaj** w górnej części bloku.
3. Wprowadź wartość **Nazwa**, która będzie opisywać aplikację na potrzeby klientów. Możesz na przykład wprowadzić nazwę „Aplikacja Contoso B2C”.
4. Przestaw przełącznik **Uwzględnij aplikację sieci Web/interfejs API sieci Web** na wartość **Tak**. **Adresy URL odpowiedzi** to punkty końcowe, w których usługa Azure AD B2C będzie zwracać wszystkie tokeny żądań aplikacji. Na przykład wprowadź wartość `https://localhost:44316/`.
5. Kliknij pozycję **Utwórz**, aby zarejestrować aplikację.
6. Kliknij nowo utworzoną aplikację i skopiuj globalnie unikatowy **identyfikator klienta aplikacji**, który będzie używany w dalszej części kodu. 
7. Jeśli aplikacja sieci Web będzie również wywoływała interfejs API sieci Web zabezpieczony za pomocą usługi Azure AD B2C, może być konieczne utworzenie **klucza tajnego aplikacji** przez przejście do bloku **Klucze** i kliknięcie przycisku **Generuj klucz**.

> [!NOTE]
> **Klucz tajny aplikacji** jest ważnym poświadczeniem zabezpieczeń i powinien być odpowiednio zabezpieczony.
> 
   

## <a name="register-a-web-api"></a>Rejestrowanie interfejsu API sieci Web
1. W bloku funkcji B2C w witrynie Azure Portal kliknij pozycję **Aplikacje**.
2. Kliknij pozycję **+Dodaj** w górnej części bloku.
3. Wprowadź wartość **Nazwa**, która będzie opisywać aplikację na potrzeby klientów. Możesz na przykład wprowadzić nazwę „Interfejs API Contoso B2C”.
4. Przestaw przełącznik **Uwzględnij aplikację sieci Web/interfejs API sieci Web** na wartość **Tak**. **Adresy URL odpowiedzi** to punkty końcowe, w których usługa Azure AD B2C będzie zwracać wszystkie tokeny żądań aplikacji. Na przykład wprowadź wartość `https://localhost:44316/`.
5. Wprowadź **identyfikator URI aplikacji**. Jest to identyfikator używany na potrzeby interfejsu API sieci Web. Na przykład wprowadź wartość „uwagi”. Spowoduje to wygenerowanie poniżej pełnego identyfikatora URI. 
6. Kliknij pozycję **Utwórz**, aby zarejestrować aplikację.
7. Kliknij nowo utworzoną aplikację i skopiuj globalnie unikatowy **identyfikator klienta aplikacji**, który będzie używany w dalszej części kodu.
8. Kliknij pozycję **Opublikowane zakresy**. Jest to miejsce, w którym można zdefiniować uprawnienia (zakresy), które mogą być udzielone innym aplikacjom.
9. W razie potrzeby dodaj więcej zakresów. Domyślnie zostanie zdefiniowany zakres „user_impersonation”. Daje to innym aplikacjom możliwość dostępu do tego interfejsu API w imieniu zalogowania użytkownika. W razie potrzeby może to zostać usunięte. 
10. Kliknij pozycję **Zapisz**.

## <a name="register-a-mobilenative-application"></a>Rejestrowanie aplikacji mobilnej/natywnej
1. W bloku funkcji B2C w witrynie Azure Portal kliknij pozycję **Aplikacje**.
2. Kliknij pozycję **+Dodaj** w górnej części bloku.
3. Wprowadź wartość **Nazwa**, która będzie opisywać aplikację na potrzeby klientów. Możesz na przykład wprowadzić nazwę „Aplikacja Contoso B2C”.
4. Przestaw przełącznik **Uwzględnij klienta natywnego** na wartość **Tak**.
5. Wprowadź **Identyfikator URI przekierowania** ze schematem niestandardowym. Na przykład com.onmicrosoft.contoso.appname://przekierowanie/ścieżka. Upewnij się, że wybrano [dobry identyfikator URI przekierowania](#choosing-a-redirect-uri).
6. Kliknij pozycję **Zapisz**, aby zarejestrować aplikację.
7. Kliknij nowo utworzoną aplikację i skopiuj globalnie unikatowy **identyfikator klienta aplikacji**, który będzie używany w dalszej części kodu.
8. Jeśli aplikacja natywna będzie również wywoływała interfejs API sieci Web zabezpieczony za pomocą usługi Azure AD B2C, może być konieczne utworzenie **klucza tajnego aplikacji** przez przejście do bloku **Klucze** i kliknięcie przycisku **Generuj klucz**.

> [!NOTE]
> **Klucz tajny aplikacji** jest ważnym poświadczeniem zabezpieczeń i powinien być odpowiednio zabezpieczony.
> 

### <a name="choosing-a-redirect-uri"></a>Wybieranie identyfikatora URI przekierowania
Istnieją dwie ważne kwestie, które należy wziąć pod uwagę podczas wybierania identyfikatora URI przekierowania dla aplikacji mobilych/natywnych: 
* **Unikatowość**: schemat identyfikatora URI przekierowania powinien być unikatowy dla każdej aplikacji. W naszym przykładzie (com.onmicrosoft.contoso.appname://przekierowanie/ścieżka) schematem jest fragment com.onmicrosoft.contoso.appname. Zalecamy stosowanie się do tego wzorca. Jeśli dwie aplikacje mają ten sam schemat, użytkownik zobaczy okno dialogowe „Wybór aplikacji”. Jeśli użytkownik dokona nieprawidłowego wyboru, logowanie nie powiedzie się. 
* **Kompletność**: identyfikator URI przekierowania musi mieć schemat i ścieżkę. Ścieżka musi zawierać co najmniej jeden ukośnik po nazwie domeny (na przykład identyfikator //contoso/ jest prawidłowy, ale identyfikator //contoso jest błędny). 

## <a name="build-a-quick-start-application"></a>Tworzenie aplikacji Szybki start
Po zarejestrowaniu aplikacji w usłudze Azure AD B2C możesz wykonać czynności opisane w jednym z naszych samouczków szybkiego startu, aby rozpocząć pracę. Poniżej przedstawiono kilka zaleceń:

[!INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]


