---
title: "Uwierzytelnianie usługi Azure AD na dostęp do interfejsu API usługi multimediów Azure za pomocą REST | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dostęp do interfejsu API usługi Azure Media Services przy użyciu uwierzytelniania usługi Azure Active Directory przy użyciu REST."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: willzhan;juliako
ms.openlocfilehash: 1c62857699fb29b3583363e1c6f2dc7874635f40
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-ad-authentication-to-access-the-azure-media-services-api-with-rest"></a>Azure Media Services API z POZOSTAŁĄ dostęp za pomocą uwierzytelniania usługi Azure AD

Zespół usługi Azure Media Services wydała obsługę uwierzytelniania usługi Azure Active Directory (Azure AD) dla dostępu do usługi Azure Media Services. On również ogłosiła planów można zastąpić uwierzytelniania usługi kontroli dostępu w usłudze Azure Media Services dostępu. Ponieważ każda subskrypcja platformy Azure i co konto usługi Media Services jest dołączony do dzierżawy usługi Azure AD, obsługę uwierzytelniania usługi Azure AD oferuje wiele korzyści w zakresie zabezpieczeń. Aby uzyskać szczegółowe informacje dotyczące tej zmiany i migracji (Jeśli używasz SDK .NET usługi Media Services dla aplikacji) zobacz następujące wpisy na blogu i artykuły:

- [Usługa Azure Media Services ogłasza pomocy technicznej dla usługi Azure AD i amortyzacja uwierzytelniania kontroli dostępu](https://azure.microsoft.com/blog/azure%20media%20service%20aad%20auth%20and%20acs%20deprecation)
- [Interfejs API usług Media Azure dostępu przy użyciu uwierzytelniania usługi Azure AD](media-services-use-aad-auth-to-access-ams-api.md)
- [Użyj uwierzytelniania usługi Azure AD dostęp do interfejsu API usługi Azure Media Services przy użyciu programu Microsoft .NET](media-services-dotnet-get-started-with-aad.md)
- [Wprowadzenie do uwierzytelniania usługi Azure AD przy użyciu portalu Azure](media-services-portal-get-started-with-aad.md)

Niektórzy klienci konieczne również opracowanie ich rozwiązań Media Services, w obszarze następujące ograniczenia:

*   Korzystają z języka programowania, który nie jest program Microsoft .NET lub C# lub środowisko uruchomieniowe nie jest częścią systemu Windows.
*   Bibliotek platformy Azure AD, takich jak biblioteki uwierzytelniania usługi Active Directory są niedostępne dla języka programowania lub nie może służyć do ich środowiska uruchomieniowego.

Niektórzy klienci mają opracowanych aplikacji przy użyciu interfejsu API REST dla uwierzytelniania kontroli dostępu i dostępu do usługi Azure Media Services. Dla tych klientów należy tylko interfejsu API REST na potrzeby uwierzytelniania usługi Azure AD i dostęp do usługi Azure Media Services. Należy nie należy polegać na żadnym z bibliotek usługi Azure AD lub SDK .NET usługi Media Services. W tym artykule firma Microsoft opisano rozwiązania i podaj przykładowy kod dla tego scenariusza. Ponieważ kod jest wszystkie wywołania interfejsu API REST, z nie zależność od usługi Azure AD lub biblioteki usługi Azure Media Services kodu można łatwo tłumaczyć innych językach programowania.

> [!IMPORTANT]
> Obecnie usługa Media Services obsługuje model uwierzytelniania usługi kontroli dostępu platformy Azure. Jednak kontroli dostępu uwierzytelniania zostaną wycofane 1 czerwca 2018. Zaleca się, jak najszybszą migrację do model uwierzytelniania usługi Azure AD.


## <a name="design"></a>Projektowanie

W tym artykule używamy następujący projekt uwierzytelniania i autoryzacji:

*  Protokół HCAP: OAuth 2.0. OAuth 2.0 to standard zabezpieczeń sieci web, który obejmuje zarówno uwierzytelniania i autoryzacji. Jest on obsługiwany przez Google, Microsoft, Facebook i PayPal. Został on ratyfikacji października 2012. Firma Microsoft obsługuje mocno OAuth 2.0 i OpenID Connect. Obsługiwane są zarówno z tymi standardami w wielu usługach i bibliotek klienckich, włącznie z usługi Azure Active Directory, Otwórz interfejs sieci Web dla platformy .NET (OWIN) Katana i bibliotek usługi Azure AD.
*  Przyznaj typu: typ przyznania poświadczeń klienta. Poświadczenia klienta jest jednym grant cztery typy w OAuth 2.0. Często jest używane dla dostępu do usługi Azure AD Microsoft Graph API.
*  Tryb uwierzytelniania: nazwy głównej usługi. Tryb uwierzytelniania jest użytkownika lub uwierzytelnianie interakcyjne.

Łącznie cztery aplikacje lub usługi są zaangażowane w usłudze Azure AD uwierzytelniania i autoryzacji przepływu dotyczące korzystania z usługi Media Services. Aplikacje i usługi oraz przepływu, zostały opisane w poniższej tabeli:

|Typ aplikacji |Aplikacja |Ruch|
|---|---|---|
|Klient | Klient aplikacji lub rozwiązania | Tej aplikacji (w rzeczywistości jego agent proxy) jest zarejestrowana w dzierżawie usługi Azure AD, w którym znajdują się subskrypcji platformy Azure i konto usługi media. Nazwy głównej usługi zarejestrowanych aplikacji otrzymuje się następnie z rolą właściciela lub współautora w dostępu do formantu (IAM) konta usługi media. Nazwy głównej usługi jest reprezentowana przez klienta, a identyfikator klienta klucz tajny aplikacji. |
|Dostawcy tożsamości (IDP) | Azure AD jako dostawca tożsamości | Nazwy głównej usługi zarejestrowanych aplikacji (identyfikator klienta i klucz tajny klienta) jest uwierzytelniony przez usługę Azure AD jako dostawca tożsamości. To uwierzytelnianie jest wykonywane wewnętrznie i niejawnie. Jak przepływ poświadczeń klienta klient jest uwierzytelniany zamiast użytkownika. |
|Secure Token Service (STS) / serwera OAuth | Azure AD jako Usługa STS | Po uwierzytelnieniu przez IDP (lub serwera OAuth względem OAuth 2.0) tokenu dostępu lub tokenu Web JSON (JWT) jest wystawiony przez usługę Azure AD jako serwera STS/OAuth dla dostępu do zasobu warstwy środkowej: w tym przypadku punkt końcowy interfejsu API REST usługi multimediów. |
|Zasób | Interfejs API REST usługi Media Services | Każdego wywołania interfejsu API REST usług Media jest upoważniony przez tokenu dostępu, który został wystawiony przez usługę Azure AD jako STS lub serwera uwierzytelniania OAuth. |

Jeżeli uruchomi się przykładowy kod i przechwycić token JWT lub tokenu dostępu, tokenu JWT ma następujące atrybuty:

    aud: "https://rest.media.azure.net",

    iss: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    iat: 1497146280,

    nbf: 1497146280,
    exp: 1497150180,

    aio: "Y2ZgYDjuy7SptPzO/muf+uRu1B+ZDQA=",

    appid: "02ed1e8e-af8b-477e-af3d-7e7219a99ac6",

    appidacr: "1",

    idp: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    oid: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    sub: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    tid: "72f988bf-86f1-41af-91ab-2d7cd011db47",

Poniżej przedstawiono mapowania między atrybutów w token JWT i cztery aplikacje lub usługi w powyższej tabeli:

|Typ aplikacji |Aplikacja |Atrybut JWT |
|---|---|---|
|Klient |Klient aplikacji lub rozwiązania |AppID: "02ed1e8e-af8b-477e-af3d-7e7219a99ac6". Identyfikator klienta aplikacji zostanie zarejestrowana do usługi Azure AD w następnej sekcji. |
|Dostawcy tożsamości (IDP) | Azure AD jako dostawca tożsamości |IDP: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/".  Identyfikator GUID to identyfikator Microsoft dzierżawy (microsoft.onmicrosoft.com). Każdy dzierżawca ma własną, unikatowy identyfikator. |
|Secure Token Service (STS) / serwera OAuth |Azure AD jako Usługa STS | iss: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/". Identyfikator GUID to identyfikator Microsoft dzierżawy (microsoft.onmicrosoft.com). |
|Zasób | Interfejs API REST usługi Media Services |lub: "https://rest.media.azure.net". Odbiorca lub odbiorcy tokenu dostępu. |

## <a name="steps-for-setup"></a>Kroki instalacji

Do rejestrowania i konfigurowanie aplikacji usługi Azure AD do uwierzytelniania usługi Azure AD i uzyskać token dostępu dla wywołaniem punktu końcowego interfejsu API REST Azure Media Services, wykonaj następujące czynności:

1.  W [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885), zarejestrować aplikację usługi Azure AD (na przykład wzmediaservice) do dzierżawy usługi Azure AD (na przykład microsoft.onmicrosoft.com). Nie ma znaczenia, czy zarejestrowany jako aplikacji sieci web lub aplikacji natywnej. Ponadto można wybrać adres URL logowania, a adres URL odpowiedzi (na przykład http://wzmediaservice.com dla obu).
2. W [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885), przejdź do **Konfiguruj** kartę aplikacji. Uwaga **identyfikator klienta**. Następnie w obszarze **klucze**, generowanie **klucz klienta** (klucz tajny klienta). 

    > [!NOTE] 
    > Zanotuj klucz tajny klienta. Nie można wyświetlić ponownie.
    
3.  W [portalu Azure](http://ms.portal.azure.com), przejdź do konta usługi Media Services. Wybierz **kontroli dostępu** okienka (IAM). Dodaj nowy element członkowski ma właściciela lub roli współautora. Dla podmiotu zabezpieczeń Wyszukaj nazwę aplikacji, które zarejestrowaną w kroku 1 (w tym przykładzie wzmediaservice).

## <a name="info-to-collect"></a>Informacje do zbierania

Aby przygotować REST kodowania, Zbierz następujących punktów danych w celu uwzględnienia w kodzie:

*   Azure AD jako punkt końcowy usługi STS: https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/token. Z tego punktu końcowego tokenu JWT dostępu jest wymagany. Oprócz służąc jako dostawca tożsamości, usługi Azure AD służy również jako tokenu Zabezpieczającego. Usługa Azure AD wystawia token JWT uzyskać dostęp do zasobów (token dostępu). JWT token ma różne oświadczenia.
*   Azure Media interfejsu API REST usług jako zasób lub odbiorców: https://rest.media.azure.net.
*   Identyfikator klienta: Zobacz krok 2 w [kroki instalacji](#steps-for-setup).
*   Klucz tajny klienta: zobacz krok 2 w [kroki instalacji](#steps-for-setup).
*   Usługi multimediów konto punkt końcowy interfejsu API REST w następującym formacie:

    https://[media_service_account_name].restv2. [data_center].media.azure.net/API 

    To jest punkt końcowy, z których wszystkie API REST usługi Media w aplikacji wywołań. Na przykład https://willzhanmswjapan.restv2.japanwest.media.azure.net/API.

Następnie możesz umieścić te pięć parametrów w pliku web.config lub app.config do użycia w kodzie.

## <a name="sample-code"></a>Przykładowy kod

Można znaleźć w przykładowym kodzie w [Azure AD Authentication Azure Media Services dostępu: zarówno za pośrednictwem interfejsu API REST](https://github.com/willzhan/WAMSRESTSoln).

Przykładowy kod ma dwie części:

*   Projekt biblioteki DLL, który ma taki kod interfejsu API REST usługi Azure AD uwierzytelniania i autoryzacji. Ma metodę wywołania interfejsu API REST z punktem końcowym interfejsu API REST usług nośnika przy użyciu tokenu dostępu.
*   Klient testu konsoli, która inicjuje uwierzytelniania usługi Azure AD i wywołuje innego interfejsu API REST usługi Media.

Przykładowy projekt ma trzy funkcje:

*   Azure AD uwierzytelnienia za pomocą poświadczeń klienta przyznać za pośrednictwem tylko interfejsu API REST.
*   Azure Media Services dostęp przy użyciu tylko interfejsu API REST.
*   Azure dostępu do magazynu przy użyciu tylko interfejsu API REST (tak jak utworzyć konto usługi Media Services przy użyciu interfejsu API REST).


## <a name="where-is-the-refresh-token"></a>Gdzie znajduje się token odświeżania?

Niektóre czytniki może poprosić o: gdzie jest token odświeżania? Dlaczego nie używać tokenu odświeżania w tym miejscu?

Token odświeżania ma na celu nie Odśwież tokenu dostępu. Zamiast tego zaprojektowano go do obejścia interwencji użytkownika końcowego uwierzytelniania lub użytkownika i wciąż uzyskać token prawidłowy dostępu po wygaśnięciu wcześniej tokenu. Lepsze nazwę token odświeżania może być następująca: "obejścia ponownie-konta w tokenu".

Jeśli używasz protokołu OAuth 2.0 autoryzacji przyznać przepływu (nazwa użytkownika i hasło, działając w imieniu użytkownika), token odświeżania pomaga Uzyskaj token dostępu odnowionego bez interwencji użytkownika żądania. Jednak OAuth 2.0 poświadczenia klienta umożliwiają przepływ, które opisano w tym artykule, klient działa w swoim imieniu. Nie wymagają interwencji użytkownika na wszystkich, a serwer autoryzacji nie musi i nie będzie zapewniają token odświeżania. Jeśli debugowania **GetUrlEncodedJWT** metody, można zauważyć, że odpowiedź z punktu końcowego tokena ma tokenu dostępu, ale nie token odświeżania.

## <a name="next-steps"></a>Następne kroki

Rozpoczynanie pracy z [przekazywanie plików do konta](media-services-dotnet-upload-files.md).
