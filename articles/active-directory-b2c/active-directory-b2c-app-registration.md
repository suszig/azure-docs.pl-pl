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
ms.date: 6/13/2017
ms.author: parakhj
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 3499ff57e650c70679dfa018eec5dbe1a6173a33
ms.contentlocale: pl-pl
ms.lasthandoff: 07/04/2017



---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: rejestrowanie aplikacji

> [!IMPORTANT]
> Aplikacje utworzone w bloku Azure AD B2C w witrynie Azure Portal muszą być zarządzane z tej samej lokalizacji. Jeśli edytujesz aplikacje B2C przy użyciu programu PowerShell lub innego portalu, stają się one nieobsługiwane i nie będą działać w usłudze Azure AD B2C. Przeczytaj więcej [poniżej](#faulted-apps).
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
1. Kliknij pozycję **+Dodaj** w górnej części bloku.
1. Wprowadź wartość **Nazwa**, która będzie opisywać aplikację na potrzeby klientów. Możesz na przykład wprowadzić nazwę „Aplikacja Contoso B2C”.
1. Przestaw przełącznik **Uwzględnij aplikację sieci Web/interfejs API sieci Web** na wartość **Tak**.
1. Wprowadź [poprawną](#limitations) wartość **adresów URL odpowiedzi** będących punktami końcowymi, w których usługa Azure AD B2C będzie zwracać wszystkie tokeny żądań aplikacji. Na przykład wprowadź wartość `https://localhost:44316/`.
1. Kliknij pozycję **Utwórz**, aby zarejestrować aplikację.
1. Kliknij nowo utworzoną aplikację i skopiuj globalnie unikatowy **identyfikator klienta aplikacji**, który będzie używany w dalszej części kodu.
1. Jeśli Twoja aplikacja internetowa będzie także wywoływać internetowy interfejs API chroniony przez usługę Azure AD B2C, musisz wykonać następujące czynności:
    1. Utwórz **wpis tajny aplikacji**, przechodząc do bloku **Klucze** i klikając przycisk **Wygeneruj klucz**.
    1. Kliknij pozycję **Dostęp do interfejsu API**, kliknij pozycję **Dodaj** i wybierz internetowy interfejs API oraz zakresy (uprawnienia).

> [!NOTE]
> **Klucz tajny aplikacji** jest ważnym poświadczeniem zabezpieczeń i powinien być odpowiednio zabezpieczony.
>

## <a name="register-a-web-api"></a>Rejestrowanie interfejsu API sieci Web

1. W bloku funkcji B2C w witrynie Azure Portal kliknij pozycję **Aplikacje**.
1. Kliknij pozycję **+Dodaj** w górnej części bloku.
1. Wprowadź wartość **Nazwa**, która będzie opisywać aplikację na potrzeby klientów. Możesz na przykład wprowadzić nazwę „Interfejs API Contoso B2C”.
1. Przestaw przełącznik **Uwzględnij aplikację sieci Web/interfejs API sieci Web** na wartość **Tak**.
1. Wprowadź [poprawną](#choosing-a-web-app/api-reply-url) wartość **adresów URL odpowiedzi** będących punktami końcowymi, w których usługa Azure AD B2C będzie zwracać wszystkie tokeny żądań aplikacji. Na przykład wprowadź wartość `https://localhost:44316/`.
1. Wprowadź **identyfikator URI aplikacji**. Jest to identyfikator używany na potrzeby interfejsu API sieci Web. Na przykład wprowadź wartość „uwagi”. Spowoduje to wygenerowanie poniżej pełnego identyfikatora URI.
1. Kliknij pozycję **Utwórz**, aby zarejestrować aplikację.
1. Kliknij nowo utworzoną aplikację i skopiuj globalnie unikatowy **identyfikator klienta aplikacji**, który będzie używany w dalszej części kodu.
1. Kliknij pozycję **Opublikowane zakresy**. Jest to miejsce, w którym można zdefiniować uprawnienia (zakresy), które mogą być udzielone innym aplikacjom.
1. W razie potrzeby dodaj więcej zakresów. Domyślnie zostanie zdefiniowany zakres „user_impersonation”. Daje to innym aplikacjom możliwość dostępu do tego interfejsu API w imieniu zalogowania użytkownika. W razie potrzeby może to zostać usunięte.
1. Kliknij pozycję **Zapisz**.

## <a name="register-a-mobilenative-application"></a>Rejestrowanie aplikacji mobilnej/natywnej

1. W bloku funkcji B2C w witrynie Azure Portal kliknij pozycję **Aplikacje**.
1. Kliknij pozycję **+Dodaj** w górnej części bloku.
1. Wprowadź wartość **Nazwa**, która będzie opisywać aplikację na potrzeby klientów. Możesz na przykład wprowadzić nazwę „Aplikacja Contoso B2C”.
1. Przestaw przełącznik **Uwzględnij klienta natywnego** na wartość **Tak**.
1. Wprowadź **Identyfikator URI przekierowania** ze schematem niestandardowym. Na przykład com.onmicrosoft.contoso.appname://przekierowanie/ścieżka. Pamiętaj, aby wybrać [właściwy identyfikator URI przekierowywania](#choosing-a-native-application-redirect-uri). Nie powinien on zawierać znaków specjalnych, takich jak podkreślenia.
1. Kliknij pozycję **Zapisz**, aby zarejestrować aplikację.
1. Kliknij nowo utworzoną aplikację i skopiuj globalnie unikatowy **identyfikator klienta aplikacji**, który będzie używany w dalszej części kodu.
1. Jeśli Twoja aplikacja natywna będzie także wywoływać internetowy interfejs API chroniony przez usługę Azure AD B2C, musisz wykonać następujące czynności:
    1. Utwórz **wpis tajny aplikacji**, przechodząc do bloku **Klucze** i klikając przycisk **Wygeneruj klucz**.
    1. Kliknij pozycję **Dostęp do interfejsu API**, kliknij pozycję **Dodaj** i wybierz internetowy interfejs API oraz zakresy (uprawnienia).

> [!NOTE]
> **Klucz tajny aplikacji** jest ważnym poświadczeniem zabezpieczeń i powinien być odpowiednio zabezpieczony.
>

## <a name="limitations"></a>Ograniczenia

### <a name="choosing-a-web-appapi-reply-url"></a>Wybieranie adresu URL odpowiedzi aplikacji internetowej/interfejsu API

Obecnie aplikacje, które są zarejestrowane w usłudze Azure AD B2C, mają wartości adresów URL odpowiedzi ograniczone do określonego zestawu. Adres URL odpowiedzi dla aplikacji i usług internetowych musi zaczynać się od schematu `https` i wartości wszystkich adresów URL odpowiedzi muszą współużytkować jedną domenę DNS. Na przykład nie można zarejestrować aplikacji internetowej z jednym z następujących adresów URL odpowiedzi:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

System rejestracji porównuje całą nazwę DNS istniejącego adresu URL odpowiedzi z nazwą DNS dodawanego adresu URL odpowiedzi. Żądanie dodania nazwy DNS zakończy się niepowodzeniem, jeśli będzie spełniony jeden z następujących warunków:

* Cała nazwa DNS nowego adresu URL odpowiedzi nie będzie zgodna z nazwą DNS istniejącego adresu URL odpowiedzi.
* Cała nazwa DNS nowego adresu URL odpowiedzi nie jest poddomeną istniejącego adresu URL odpowiedzi.

Na przykład jeśli aplikacja ma następujący adres URL odpowiedzi:

`https://login.contoso.com`

Można dodać do niego adres w następujący sposób:

`https://login.contoso.com/new`

W takim przypadku nazwa DNS jest idealnie zgodna. Można też zrobić tak:

`https://new.login.contoso.com`

W takim przypadku przywoływana jest poddomena DNS domeny login.contoso.com. Jeśli chcesz mieć aplikację z adresami URL odpowiedzi login-east.contoso.com i login-west.contoso.com, musisz dodać następujące adresy URL w podanej kolejności:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Dwa ostatnie adresy można dodać, ponieważ są poddomenami pierwszego adresu URL odpowiedzi, contoso.com.

### <a name="choosing-a-native-application-redirect-uri"></a>Wybieranie identyfikatora URI przekierowania aplikacji natywnej

Istnieją dwie ważne kwestie, które należy wziąć pod uwagę podczas wybierania identyfikatora URI przekierowania dla aplikacji mobilych/natywnych:

* **Unikatowość**: schemat identyfikatora URI przekierowania powinien być unikatowy dla każdej aplikacji. W naszym przykładzie (com.onmicrosoft.contoso.appname://przekierowanie/ścieżka) schematem jest fragment com.onmicrosoft.contoso.appname. Zalecamy stosowanie się do tego wzorca. Jeśli dwie aplikacje mają ten sam schemat, użytkownik zobaczy okno dialogowe „Wybór aplikacji”. Jeśli użytkownik dokona nieprawidłowego wyboru, logowanie nie powiedzie się.
* **Kompletność**: identyfikator URI przekierowania musi mieć schemat i ścieżkę. Ścieżka musi zawierać co najmniej jeden ukośnik po nazwie domeny (na przykład identyfikator //contoso/ jest prawidłowy, ale identyfikator //contoso jest błędny).

Upewnij się, że w identyfikatorze URI przekierowywania nie ma żadnych znaków specjalnych, takich jak podkreślenia.

### <a name="faulted-apps"></a>Uszkodzone aplikacje

NIE NALEŻY edytować aplikacji B2C:

* W innych portalach zarządzania aplikacjami, takich jak [klasyczna witryna Azure Portal](https://manage.windowsazure.com/) i [portal rejestracji aplikacji](https://apps.dev.microsoft.com/).
* Korzystanie z interfejsu API programu Graph lub programu PowerShell

Jeśli poddasz edycji aplikację B2C w sposób opisany powyżej i spróbujesz ponownie edytować ją w bloku funkcji usługi Azure AD B2C w witrynie Azure Portal, stanie się ona uszkodzoną aplikacją i nie będzie można z niej korzystać w usłudze Azure AD B2C. Będzie trzeba usunąć taką aplikację i utworzyć ją ponownie.

Aby usunąć aplikację, przejdź do [portalu rejestracji aplikacji](https://apps.dev.microsoft.com/) i usuń ją tam. Aby aplikacja była widoczna, musisz być jej właścicielem (a nie tylko administratorem dzierżawy).

## <a name="next-steps"></a>Następne kroki

Po zarejestrowaniu aplikacji w usłudze Azure AD B2C możesz wykonać czynności opisane w jednym z [naszych samouczków szybkiego startu](active-directory-b2c-overview.md#get-started), aby rozpocząć pracę.

