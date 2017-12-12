---
title: "Sposób tworzenia aplikacji, które można zarejestrować się w każdy użytkownik usługi Azure AD"
description: "Krok po kroku instrukcje tworzenia aplikacji, które można zalogować użytkownika z dowolnej dzierżawy usługi Azure Active Directory nazywanego także wielodostępnych aplikacji."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.assetid: 35af95cb-ced3-46ad-b01d-5d2f6fd064a3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: f6d8d2c07c2860059c4e9deb75d0bc4a876e057b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>Jak zarejestrować każdy użytkownik usługi Azure Active Directory (AD) przy użyciu wzorca wielodostępnych aplikacji
Jeśli oferujesz oprogramowania jako usługi aplikacji dla wielu organizacji, można skonfigurować aplikację do akceptowania logowania z dowolnej dzierżawy usługi Azure AD.  W usłudze Azure AD ta konfiguracja jest nazywana tworzenie dzierżawy usługi aplikacji.  Użytkownicy w dowolnej dzierżawy usługi Azure AD będą mogli logować się do aplikacji po zgodę swojego konta za pomocą aplikacji.  

Jeśli masz istniejącej aplikacji, która ma swój własny system konta lub inne rodzaje logowania od innych dostawców w chmurze obsługuje dodawanie usługi Azure AD logowania z dowolnej dzierżawy jest proste. Tylko rejestrowanie aplikacji, Dodaj kod logowania za pomocą protokołu OAuth2, OpenID Connect lub SAML i umieść przycisk "Logowania w with Microsoft" w swojej aplikacji. Kliknij poniższy przycisk, aby dowiedzieć się więcej o znakowaniu aplikacji.

[![Zaloguj się przycisk][AAD-Sign-In]][AAD-App-Branding]

W tym artykule przyjęto założenie, że znasz już tworzenia aplikacji pojedynczej dzierżawy dla usługi Azure AD.  Nie masz, head wykonać kopię zapasową [strony głównej przewodnik dewelopera] [ AAD-Dev-Guide] i wypróbować jeden z naszych Szybki Start!

Istnieją cztery prostych czynności w celu konwertowania aplikacji na aplikację usługi Azure AD wielodostępne:

1. Zaktualizuj rejestrację aplikacji się wieloma dzierżawcami
2. Zaktualizuj kod do wysyłania żądań do / Common punktu końcowego 
3. Zaktualizuj swój kod obsługi wielu wartości wystawcy
4. Zrozumienie zgody użytkownika i administratora i zmienić odpowiedni kod

Przyjrzyjmy się każdego kroku szczegółowo. Można także przejść bezpośrednio do [tej listy próbek wielodostępne][AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Zaktualizuj rejestrację się wieloma dzierżawcami
Domyślnie rejestracji aplikacji/interfejsu API sieci web w usłudze Azure AD są pojedynczej dzierżawy.  Umożliwia rejestrację wielodostępne znajdując przełącznika "Wielu dzierżawcza" na stronie właściwości Twojej rejestracji aplikacji w [portalu Azure] [ AZURE-portal] i ustawiając go na "Tak".

Należy również zauważyć, zanim aplikacji może się wieloma dzierżawcami usługi Azure AD wymaga aplikacji mogą być globalnie unikatowy identyfikator URI aplikacji. Identyfikator URI aplikacji jest jednym ze sposobów, który aplikacja zostanie zidentyfikowana w wiadomości protokołu.  Dla aplikacji pojedynczej dzierżawy jest wystarczająca dla identyfikator URI aplikacji być unikatowe w obrębie tej dzierżawy.  Aplikacji wielodostępnych musi być globalnie unikatowe dzięki usłudze Azure AD można znaleźć aplikacji we wszystkich dzierżawców.  Globalne unikatowości jest wymuszana przez wymaganie identyfikator URI aplikacji ma nazwę hosta pasującą zweryfikowanej domeny dzierżawy usługi Azure AD.  Na przykład, jeśli nazwę dzierżawy został contoso.onmicrosoft.com, a następnie prawidłowy identyfikator URI aplikacji będzie `https://contoso.onmicrosoft.com/myapp`.  Gdyby dzierżawy zweryfikowanej domeny `contoso.com`, również będą prawidłowy identyfikator URI aplikacji, a następnie `https://contoso.com/myapp`.  Jeśli identyfikator URI aplikacji nie będzie zgodna z tego wzorca ustawienia aplikacji, ponieważ wielodostępne zakończy się niepowodzeniem.

Aplikacja Native client są wielodostępne domyślnie.  Nie musisz podejmować żadnych działań, aby natywny klienta aplikacji rejestracji wielu dzierżawców.

## <a name="update-your-code-to-send-requests-to-common"></a>Zaktualizuj kod do wysyłania żądań do/Common
W aplikacji pojedynczej dzierżawy żądań logowania są wysyłane do dzierżawcy logowania punktu końcowego. Na przykład dla contoso.onmicrosoft.com będzie punktu końcowego:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Żądania wysyłane do punktu końcowego dzierżawcy zalogować się użytkownicy (lub gości) w tej dzierżawie do aplikacji w tej dzierżawie.  Z aplikacją wielodostępnych aplikacji nie może ustalić góry dzierżawy, jakie użytkownik ma, więc nie można wysłać żądania do punktu końcowego dzierżawcy.  Zamiast tego żądania są wysyłane do punktu końcowego, który multiplexes między dzierżaw wszystkie usługi Azure AD:

    https://login.microsoftonline.com/common

Jeśli usługi Azure AD odbiera żądanie na / Common punktu końcowego, jego loguje użytkownika i w konsekwencji odnajduje dzierżawy, którym użytkownik jest z.  / Wspólnego punktu końcowego współpracuje z wszystkie protokoły obsługiwane przez usługę Azure AD: OpenID Connect, OAuth 2.0 SAML 2.0 i WS-Federation.

Odpowiedź logowania do aplikacji, następnie zawiera token reprezentujący użytkownika.  Wartości wystawcy tokenu informuje aplikacji dzierżawy, jakie użytkownik jest z.  Gdy zwraca odpowiedź znajdujący punktu końcowego, odpowiada wartości wystawcy tokenu użytkownika dzierżawcy.  Należy pamiętać, / Common jest punkt końcowy nie jest dzierżawcy i nie jest wystawcę, jest tylko multiplekser.  Używając/Common logikę w aplikacji do sprawdzania poprawności tokenów musi zostać zaktualizowany do to uwzględniać. 

Jak wspomniano wcześniej, aplikacje wielodostępne powinny również zapewnia spójne środowisko logowania użytkowników w następującej aplikacji usługi Azure AD znakowania wytyczne. Kliknij poniższy przycisk, aby dowiedzieć się więcej o znakowaniu aplikacji.

[![Zaloguj się przycisk][AAD-Sign-In]][AAD-App-Branding]

Spójrzmy na użycie / Common punktu końcowego i implementacji kodu bardziej szczegółowo.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Zaktualizuj swój kod obsługi wielu wartości wystawcy
Aplikacje sieci Web i interfejsów API sieci web odbierają i sprawdzania poprawności tokenów z usługi Azure AD.  

> [!NOTE]
> Aplikacje klienckie natywnego żądania i odbierać tokeny od usługi Azure AD, w tym celu ich wysłania do interfejsów API, w którym są weryfikowane.  Natywnych aplikacji nie sprawdzania poprawności tokenów i należy je traktować jako przezroczystości.
> 
> 

Zobaczmy, w jaki sposób aplikacja weryfikuje tokeny odbiera z usługi Azure AD.  Stosowanie pojedynczej dzierżawy zwykle przyjmuje wartość punktu końcowego, takie jak:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

i korzystać do utworzenia adresu URL metadanych (w tym przypadku OpenID Connect), takich jak:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

Aby pobrać dwóch najistotniejsze informacje służące do sprawdzania poprawności tokenów: dzierżawcy podpisywania kluczy i wartości wystawcy.  Każda dzierżawa usługi Azure AD ma wartość unikatowy wystawcy formularza:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

wartości identyfikatora GUID w przypadku zmiany nazwy bezpieczna wątkowo wersja identyfikatora dzierżawy dzierżawcy.  Po kliknięciu poprzedni element metadanych dla `contoso.onmicrosoft.com`, ta wartość wystawcy dokumentu.

Stosowanie pojedynczej dzierżawy weryfikuje token, sprawdza zgodność podpisu aplikacji tokenu przed klucze podpisywania dokumentu metadanych. Ten test pozwala na upewnij się, że wartości wystawcy tokenu jest zgodna ze strukturą, które odnaleziono w dokumencie metadanych.

Ponieważ / Common punktu końcowego nie jest zgodny z dzierżawą usługi, który nie jest wystawcy należy zbadać wartości wystawcy metadanych dla / wspólnej składa się z szablonem adresu URL zamiast rzeczywista wartość:

    https://sts.windows.net/{tenantid}/

W związku z tym aplikacji wielodostępnych nie można sprawdzić poprawności tokenów porównując tylko wartości wystawcy w metadanych z `issuer` wartość w tokenie.  Aplikacji wielodostępnych logikę do określania wartości wystawcy, które są prawidłowe i mają, nie musi, część wartości wystawcy Identyfikatora dzierżawcy w oparciu.  

Na przykład jeśli aplikacja wielodostępne zezwala tylko logowania z określonym dzierżawców, którzy utworzyli konto usługi, następnie powinien sprawdzić wartości wystawcy lub `tid` wartości w tokenie, aby się upewnić, że tej dzierżawy jest na liście abonentów oświadczenia.  Aplikacji wielodostępnych tylko dotyczy osób, nie decyzje żadnych dostępu oparte na dzierżaw następnie zignorowanie wartości wystawcy całkowicie.

W przykładach wielodostępne w [powiązane zawartości](#related-content) sekcji na końcu tego artykułu, sprawdzania poprawności wystawcy jest wyłączone, aby włączyć dowolnej dzierżawy usługi Azure AD do logowania.

Teraz Przyjrzyjmy się czynności użytkownika dla użytkowników, którzy są logowanie do aplikacji z wieloma dzierżawcami.

## <a name="understanding-user-and-admin-consent"></a>Opis użytkowników i zgody administratora
Aby użytkownik mógł zalogować się do aplikacji w usłudze Azure AD aplikacja musi być reprezentowana w dzierżawie użytkownika.  Dzięki temu wykonywanie czynności, takich jak zastosować unikatowych zasad podczas ich dzierżawy logowania do aplikacji w organizacji.  W przypadku aplikacji pojedynczej dzierżawy tej rejestracji jest proste; jest to jeden, która jest wywoływana podczas rejestrowania aplikacji w [portalu Azure][AZURE-portal].

Dla wielodostępnych aplikacji początkowej rejestracji aplikacji znajduje się w dzierżawie usługi Azure AD używane przez dewelopera.  Po zalogowaniu użytkownika z innej dzierżawy do aplikacji po raz pierwszy, usługi Azure AD monituje użytkownika o zgodę uprawnień wymaganych przez aplikację.  Jeśli ich zgody, a następnie wywołuje reprezentację aplikacji *nazwy głównej usługi* jest tworzony w dzierżawie użytkownika i można kontynuować logowania. Delegowanie również jest tworzony w katalogu, który rejestruje zgody użytkownika do aplikacji. Zobacz [obiekty aplikacji i nazwy głównej usługi] [ AAD-App-SP-Objects] szczegółowe informacje na temat aplikacji aplikacji i ServicePrincipal obiektów i ich relacje między sobą.

![Wyrażenia zgody na jednowarstwową aplikacji][Consent-Single-Tier] 

To środowisko zgody zależy od uprawnień wymaganych przez aplikację.  Usługi Azure AD obsługuje dwa rodzaje uprawnienia tylko do aplikacji i delegowani:

* Delegowane uprawnienia przyznaje aplikacji, które może wykonywać możliwość działania jako zalogowanego użytkownika dla podzbioru czynności użytkownika.  Na przykład można udzielić aplikacji delegowane uprawnienia do odczytu kalendarza zalogowanego użytkownika.
* Uprawnienia tylko do aplikacji otrzymuje bezpośrednio do odpowiedniej tożsamości aplikacji.  Na przykład można udzielić aplikacji uprawnienia tylko do aplikacji, można odczytać listy użytkowników w dzierżawie, niezależnie od tego, który jest zalogowany do aplikacji.

Niektóre uprawnienia można zgodę na przez zwykłego użytkownika, a inne wymagają zgody administratora dzierżawy. 

### <a name="admin-consent"></a>Zgody administratora
Uprawnienia tylko do aplikacji zawsze Wymagaj zgody administratora dzierżawy.  Jeśli aplikacja żąda uprawnienia tylko do aplikacji, a użytkownik próbuje zalogować się do aplikacji, jest wyświetlany komunikat o błędzie informujący, że użytkownik nie jest w stanie zgody.

Niektóre delegowane uprawnienia również wymagać zgody administratora dzierżawy.  Na przykład możliwość zapisywania zwrotnego z usługą Azure AD jako zalogowany użytkownik wymaga zgody administratora dzierżawy.  W przypadku zwykłego użytkownika próbuje zalogować się do aplikacji, która żąda uprawnień delegowanych, które wymaga zgody administratora, takich jak uprawnienia tylko do aplikacji, aplikacji odbierze wystąpił błąd.  Określa, czy uprawnienia wymaga zgody administratora, jest określana przez deweloperów, która wydała zasobu i można znaleźć w dokumentacji dla zasobu.  Łącza do tematów opisujących uprawnienia dostępne dla interfejsu API usługi Azure AD Graph i interfejsu API Graph usługi Microsoft są w [powiązane zawartości](#related-content) sekcji tego artykułu.

Jeśli aplikacja używa uprawnienia, które wymagają zgody administratora, należy mieć gestu, takich jak przycisk lub łącze którym administrator może zainicjować akcji.  Żądanie aplikacji wysyła ta akcja jest zwykle żądania autoryzacji OAuth2/OpenID Connect, lecz który obejmuje również `prompt=admin_consent` parametr ciągu zapytania.  Po zgodził administratora i nazwy głównej usługi jest tworzony w dzierżawie klienta, nie ma potrzeby kolejnych żądań logowania `prompt=admin_consent` parametru. Ponieważ administrator zdecydował się, że żądane uprawnienia są dopuszczalne, nie innych użytkowników w dzierżawie monit o zgodę od tej pory.

`prompt=admin_consent` Parametru można również przez aplikacje, które zażądać uprawnień, które nie wymagają zgody administratora. Można to zrobić po aplikator wymaga środowisko, w którym dzierżawy admin "zarejestrowaniu" czasie, a nie innych użytkowników monit o wyrażenie zgody od tego momentu na.

Jeśli aplikacja wymaga zgody administratora, a administrator loguje ale `prompt=admin_consent` parametru nie są wysyłane, administrator pomyślnie zgadza się na aplikacji **tylko dla swojego konta użytkownika**.  Normalnych użytkowników są nadal nie można zalogować się i wyrażenia zgody na aplikację.  Ta funkcja jest przydatna, jeśli chcesz nadać możliwość Eksploruj aplikacji przed zezwoleniem na innym użytkownikom dostęp administratora dzierżawy.

Administrator dzierżawy może wyłączyć przez regularne użytkownikom wyrazić zgodę na aplikacje.  Jeśli ta funkcja jest wyłączona, zgody administratora jest zawsze wymagane dla aplikacji można skonfigurować w dzierżawie.  Jeśli chcesz przetestować aplikację za zgodą użytkownika regularne wyłączone, można znaleźć przełącznik konfiguracji w dzierżawie usługi Azure AD sekcji konfiguracji [portalu Azure][AZURE-portal].

> [!NOTE]
> Niektóre aplikacje mają środowisko, w którym regularne użytkownicy będą mogli początkowo zgody, a później aplikacja może obejmować uprawnienia administratora i żądania, które wymagają zgody administratora.  Nie istnieje sposób w tym celu z rejestracją pojedynczej aplikacji w usłudze Azure AD dzisiaj.  Nadchodzących punktu końcowego modelu wdrażania Menedżera zasobów Azure AD umożliwia aplikacji, aby zażądać uprawnień w czasie wykonywania, zamiast w czasie rejestracji, który umożliwia ten scenariusz.  Aby uzyskać więcej informacji, zobacz [modelu wdrażania usługi Azure AD aplikacji modelu Resource Manager — Przewodnik dewelopera][AAD-V2-Dev-Guide].
> 
> 

### <a name="consent-and-multi-tier-applications"></a>Aplikacje wielowarstwowe i zgody
Aplikacja może mieć wielu warstw, każdy reprezentowany przez jego własnej rejestracji w usłudze Azure AD.  Na przykład natywnych aplikacji, która wywołuje interfejs API sieci web lub aplikacji sieci web która wywołuje interfejs API sieci web.  W obu przypadkach klient (aplikacji sieci web lub aplikacji natywnej) żąda uprawnień do wywoływania zasobów (interfejs API sieci web).  Klient pomyślnie można zgodę na klienta dzierżawy wszystkie zasoby, do których żąda uprawnień musi już istnieć w dzierżawy przez klienta.  Jeśli ten warunek nie jest spełniony, usługi Azure AD zwraca błąd czy zasobu należy najpierw dodać.

**Konfiguracja wielu warstw w pojedynczej dzierżawy**

Może to być problem, jeśli aplikacja logicznej składa się z dwóch lub więcej rejestracji aplikacji, na przykład oddzielnych klienta i zasobów.  Jak można uzyskać zasobu do dzierżawy klienta pierwszy?  Usługi Azure AD obejmuje przypadek przez włączenie klienta i zasobów być zgodę w jednym kroku. Użytkownik widzi łączną sumę uprawnień żądany przez klienta i zasobów na stronie zgody.  Aby włączyć to zachowanie, Rejestracja aplikacji zasobu musi zawierać identyfikator aplikacji klienta jako `knownClientApplications` w manifeście aplikacji.  Na przykład:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Ta właściwość może być aktualizowana przez zasób [manifest aplikacji][AAD-App-Manifest]. To jest przedstawiona w klientami wielowarstwowych wywoływanie przykładowy interfejs API sieci web w [powiązane zawartości](#related-content) sekcji na końcu tego artykułu. Na poniższym diagramie przedstawiono omówienie zgody dla aplikacji wielowarstwowych zarejestrowane w pojedynczej dzierżawy:

![Wyrażenia zgody na aplikację wielowarstwową znane klienta][Consent-Multi-Tier-Known-Client] 

**Konfiguracja wielu warstw w wielu dzierżawców**

Podobne przypadku się stanie w przypadku różnych warstw aplikacji są rejestrowane w różnym dzierżawcom.  Rozważmy na przykład w przypadku tworzenia aplikacji klientami, która wywołuje interfejs API z Online Exchange Office 365.  Aby opracować natywnego aplikacji, a później do natywnej aplikacji do uruchamiania w dzierżawie klienta, główną usługi Exchange Online musi być obecny.  W takim przypadku deweloperów i klient musi zakupić usługi Exchange Online dla podmiotu zabezpieczeń mogą być tworzone w swoich dzierżaw usługi.  

W przypadku interfejsu API utworzony przez organizację innych niż Microsoft developer interfejsu API musi umożliwiają klientom zgody aplikacji do ich klientom dzierżaw. Zalecany projekt jest 3 developer strony do tworzenia interfejsu API w taki sposób, aby również może działać jako klient sieci web do implementowania rejestracji:

1. Postępuj zgodnie z wcześniejszych sekcjach, aby upewnić się, że interfejs API zaimplementowano wymagania rejestracji/kod wielodostępnych aplikacji
2. Oprócz udostępnianie role/zakresy interfejsie API, upewnij się, zawiera rejestracji "Zaloguj się i odczytuj profil użytkownika" uprawnienia usługi Azure AD (domyślnie dostępne)
3. Implementuje stronę logowania — w/tworzenia konta w kliencie sieci web po [zgody administratora](#admin-consent) wskazówki wymienione powyżej 
4. Po wyrażeniu zgody przez użytkownika do aplikacji, linki delegowania podmiot zabezpieczeń i zgody usługi są tworzone w swojej dzierżawy, a aplikacji natywnej mogą uzyskiwać tokeny dla interfejsu API

Na poniższym diagramie przedstawiono omówienie zgody dla aplikacji wielowarstwowych zarejestrowane w różnych dzierżawców:

![Wyrażenia zgody na wieloosobowa aplikację wielowarstwową][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Cofnięcie zgody
Użytkownicy i Administratorzy mogą wycofać zgodę do aplikacji w dowolnym momencie:

* Użytkownicy odwołać dostęp do poszczególnych aplikacji, usuwając je z ich [aplikacji panelu dostępu] [ AAD-Access-Panel] listy.
* Administratorzy odwołać dostęp do aplikacji, usuwając je z usługą Azure AD, korzystając z sekcji zarządzania usługi Azure AD [portalu Azure][AZURE-portal].

Jeśli administrator zgadza się na aplikacji dla wszystkich użytkowników w dzierżawie, użytkownicy nie mogą indywidualnie odwołać dostęp.  Tylko administrator może odwołać dostęp i tylko dla całej aplikacji.

### <a name="consent-and-protocol-support"></a>Obsługa protokołu i zgodę
Zgoda jest obsługiwana w usłudze Azure AD za pomocą uwierzytelniania OAuth, OpenID Connect, WS-Federation oraz SAML protokołów.  Nie obsługują protokoły SAML i WS-Federation `prompt=admin_consent` parametru, więc zgody administratora tylko jest to możliwe za pośrednictwem protokołu OAuth i OpenID Connect.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Aplikacje wielodostępne i buforowanie tokeny dostępu
Aplikacje wielodostępne można również uzyskać tokenów dostępu do wywoływania interfejsów API, które są chronione przez usługę Azure AD.  Typowym błędem podczas przy użyciu biblioteki uwierzytelniania usługi Active Directory (ADAL) z aplikacją wielodostępne jest początkowo uzyskać token dla użytkownika za pomocą/Common, otrzymują odpowiedź, a następnie żądania tokenu kolejnych dla tego samego użytkownika również przy użyciu/Common.  Ponieważ odpowiedzi z usługi Azure AD nie pochodzi od dzierżawcy, / wspólne, ADAL buforuje token jako od dzierżawcy. Wpis pamięci podręcznej chybień kolejne wywołanie/Common do uzyskania tokenu dostępu dla użytkownika, a użytkownik jest monitowany o Zaloguj się ponownie.  Aby uniknąć, Brak pamięci podręcznej, upewnij się, że wezwań do już zalogowanego użytkownika zostały wprowadzone do punktu końcowego dzierżawcy.

## <a name="next-steps"></a>Następne kroki
W tym artykule przedstawiono sposób tworzenia aplikacji, która może zalogować użytkownika z dowolnej dzierżawy usługi Azure Active Directory. Po włączeniu rejestracji jednokrotnej między aplikacji i usługi Azure Active Directory, należy zaktualizować aplikacji dostępu do interfejsów API udostępnianych przez zasoby firmy Microsoft, takich jak usługi Office 365. Dlatego możesz zaoferować spersonalizowane środowisko w aplikacji, na przykład przedstawiający informacje kontekstowe użytkownikom, jak ich obraz profilu lub ich dalej terminu kalendarza. Aby dowiedzieć się więcej o wywołaniach interfejsu API usługi Azure Active Directory i usługi Office 365, takie jak Exchange, SharePoint, OneDrive, OneNote, planowania, Excel i więcej, odwiedź stronę: [interfejsu API programu Microsoft Graph][MSFT-Graph-overview].


## <a name="related-content"></a>Zawartość pokrewna
* [Przykłady aplikacji z wieloma dzierżawcami][AAD-Samples-MT]
* [Znakowania wytyczne dotyczące aplikacji][AAD-App-Branding]
* [Przewodnik dewelopera usługi Azure AD][AAD-Dev-Guide]
* [Obiekty aplikacji i nazwy głównej usługi][AAD-App-SP-Objects]
* [Integrowanie aplikacji z usługą Azure Active Directory][AAD-Integrating-Apps]
* [Omówienie struktury zgody][AAD-Consent-Overview]
* [Zakresy uprawnień Microsoft Graph API][MSFT-Graph-permision-scopes]
* [Zakresy uprawnień usługi Azure AD Graph API][AAD-Graph-Perm-Scopes]

W poniższej sekcji komentarzy umożliwia wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-portal]: https://portal.azure.com
[MSFT-Graph-overview]: https://graph.microsoft.io/en-us/docs/overview/overview
[MSFT-Graph-permision-scopes]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ../active-directory-appmodel-v2-overview.md
[AZURE-portal]: https://portal.azure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














