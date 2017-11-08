---
title: "CENC z wieloma DRM i kontrolą dostępu: odwołanie do projektu i wdrożenia na platformie Azure i usługi Azure Media Services | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o tym, jak dotyczące licencjonowania Microsoft® Smooth Streaming klienta Eksportowanie zestawu."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 7814739b-cea9-4b9b-8370-538702e5c615
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: willzhan;kilroyh;yanmf;juliako
ms.openlocfilehash: e4a53d053a4c792f54e215c19a8f0c4064815839
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>Szyfrowanie CENC przy użyciu technologii Multi-DRM i kontroli dostępu: odwołanie do projektowania i implementacji na platformie Azure i w usłudze Azure Media Services
 
## <a name="introduction"></a>Wprowadzenie
Należy dobrze znany, jest złożonym zadaniem projektowanie i tworzenie Podsystem DRM dla OTT lub w trybie online, rozwiązań do przesyłania strumieniowego. I jest typowym rozwiązaniem dla operatorów/online dostawców wideo do zewnętrznej obsługi tej części specjalne DRM usługodawców. Celem tego dokumentu jest do prezentowania odwołanie do projektowania i implementacji w OTT lub online rozwiązania do przesyłania strumieniowego podsystemu DRM end-to-end.

Docelowe czytników tego dokumentu są Inżynierowie pracujący w podsystemie DRM OTT online przesyłania strumieniowego/kilku-screen rozwiązania lub dowolnego czytników zainteresowane w podsystemie DRM. Zakłada się, że czytników zapoznali się z co najmniej jednej z technologii DRM na rynku, takich jak PlayReady i Widevine, FairPlay Adobe dostępu.

Przez DRM będziemy również obejmować (Common Encryption) CENC z wieloma DRM. Główne trendu w online przesyłania strumieniowego oraz z branży OTT ma używać CENC z kilku-native DRM na różnych platformach klienckich, czyli shift z poprzednich trend przy użyciu pojedynczego DRM i zestawu SDK klienta dla różnych platform klienta. Jeśli używasz CENC z kilku native DRM, zarówno PlayReady i Widevine są szyfrowane na [Common Encryption (ISO/IEC 23001-7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) specyfikacji.

Zalety CENC z wieloma DRM są następujące:

1. Zmniejsza koszt, ponieważ przetwarzanie pojedynczego szyfrowania jest używany przeznaczonych dla różnych platform z jego DRMs natywnego; szyfrowania
2. Zmniejsza koszt zarządzanie zasobami zaszyfrowane, ponieważ jest wymagana tylko jednej kopii zaszyfrowanych zasoby;
3. Eliminuje licencjonowania koszt, ponieważ native DRM client to zwykle wolnego miejsca na jego natywnego platforma klientów DRM.

Microsoft została active promotor kreska i CENC wraz z niektórych odtwarzaczy branży głównych. Microsoft Azure Media Services obsługiwał obsługę kreska i CENC. Ostatnie anonsów, zobacz Mingfei w blogach: [usług dostarczania licencji Announcing Google Widevine w usłudze Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/), i [usługi Azure Media Services dodaje tworzenia pakietów Google Widevine dostarczania strumienia wieloma DRM](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Informacje w tym artykule
Celem tego artykułu obejmuje następujące funkcje:

1. Zawiera odwołanie do projektu podsystemu DRM przy użyciu CENC z wieloma DRM;
2. Udostępnia implementację odwołania na platformie Microsoft Azure/usługi Azure Media Services;
3. W tym artykule omówiono niektóre tematy projekt i implementację.

W artykule "wieloma DRM" obejmuje następujące zagadnienia:

1. Microsoft PlayReady
2. Google Widevine
3. FairPlay firmy Apple 

W poniższej tabeli przedstawiono aplikacji natywnych platformy lub natywne i przeglądarek obsługiwanych przez każdy DRM.

| **Platforma klienta** | **Obsługa natywnego DRM** | **Przeglądarka/aplikacji** | **Formatów przesyłania strumieniowego** |
| --- | --- | --- | --- |
| **Inteligentne telewizorach, operator odbiornikami, odbiornikami OTT** |PlayReady przede wszystkim i/lub Widevine i/lub innych |Linux, Opera, WebKit, innych |Różne formaty. |
| **Urządzenia z systemem Windows 10 (komputera z systemem Windows, tablety z systemem Windows, Windows Phone, Xbox)** |PlayReady |MS krawędzi/IE11/EME<br/><br/><br/>PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS |DASH (dla protokołu HLS, PlayReady nie jest obsługiwane)<br/><br/>DASH, Smooth Streaming (HLS dla, PlayReady nie jest obsługiwane) |
| **Urządzenia z systemem android (telefon i Tablet, TV)** |Widevine |Chrome/EME |DASH, HLS |
| **iOS (iPhone, iPad), OS X, klientów i Apple TV** |FairPlay |Safari 8 +/ EME |HLS |


Biorąc pod uwagę bieżący stan wdrożenia dla każdego DRM usługa zwykle można zaimplementować DRMs 2 lub 3, aby upewnić się, że Rozwiąż wszystkie typy punktów końcowych w najlepszy sposób.

Brak zależności od złożoności logiki usługi i po stronie klienta poziom środowisko użytkownika w różnych klientów.

Aby dokonać wyboru, należy pamiętać, następujące fakty:

* PlayReady natywnie jest zaimplementowana w każde urządzenie z systemem Windows, na niektórych urządzeniach z systemem Android i dostępne za pośrednictwem zestawów SDK oprogramowania w praktycznie dowolnej platformie
* Natywnie zaimplementowano Widevine, każde urządzenie z systemem Android, Chrome i niektóre inne urządzenia
* FairPlay jest dostępna tylko w systemach iOS i Mac OS klientów lub za pomocą programu iTunes.

Dlatego typowe wieloma DRM mogą być następujące:

* Opcja 1: PlayReady i Widevine
* Opcja 2: PlayReady i Widevine FairPlay

## <a name="a-reference-design"></a>Odwołanie do projektu
W tej sekcji możemy przedstawia projekt odwołania, który jest niezależny od technologie używane do implementowania go.

Podsystem DRM może zawierać następujące składniki:

1. Zarządzanie kluczami
2. OPAKOWYWANIE DRM
3. Dostarczanie licencji DRM
4. Sprawdź uprawnienia
5. Uwierzytelniania/autoryzacji
6. Player
7. Pochodzenie/CDN

Na poniższym diagramie przedstawiono wysokiego poziomu interakcji między składnikami w podsystemie DRM.

![Podsystem DRM z CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)

Istnieją trzy podstawowe "warstwy" w projekcie:

1. Warstwa zaplecza (w kolorze czarnym), które nie są widoczne na zewnątrz;
2. Warstwa "DMZ" (niebieski) zawierający wszystkie punkty końcowe skierowane publicznych;
3. Publiczny Internet warstwę (niebieski światła) zawierającą CDN i odtwarzacze z ruchem w publicznej sieci Internet.

Powinien być narzędziem do zarządzania zawartością kontrolowania ochrony DRM, niezależnie od szyfrowania statyczne lub dynamiczne. Dane wejściowe do szyfrowania DRM powinna zawierać:

1. Zawartość wideo MBR;
2. Klucz zawartości;
3. Adresy URL pozyskiwania licencji.

Podczas odtwarzania wysokiego poziomu przebieg jest:

1. Użytkownik jest uwierzytelniony;
2. Token autoryzacji jest tworzona dla użytkownika;
3. Zawartość chroniona DRM (manifest) zostanie pobrana do odtwarzacza;
4. Odtwarzacz przesyła żądanie nabycie licencji do serwerów licencji oraz klucza identyfikator i autoryzacji tokenu.

Przed przejściem do następnego tematu kilka słów dotyczących projektu zarządzania kluczami.

| **ContentKey — zasobów** | **Scenariusz** |
| --- | --- |
| 1 – 1 |Najprostszym przypadku. Zapewnia on Najdrobniejsze formantu. Ale zazwyczaj powoduje to najwyższy koszt dostarczania licencji. W minimalnej jedną licencję żądania jest wymagana dla każdego chronionego zasobu. |
| 1-do wielu |Wiele zasobów, można użyć tego samego klucza zawartości. Na przykład dla wszystkich zasobów w grupę logiczną, takie jak genre lub podzbiór genre (lub gen film), można użyć jednego klucza zawartości. |
| Wiele do-1 |Wiele kluczy zawartości są wymagane dla każdego zasobu. <br/><br/>Na przykład jeśli trzeba zastosować dynamicznej ochrony CENC z wieloma DRM dla protokołu MPEG DASH i dynamicznego szyfrowania AES-128 dla protokołu HLS, należy dwa osobne klucze zawartości, każdy z własną ContentKeyType. (W przypadku klucz zawartości używana na potrzeby dynamicznej ochrony CENC ContentKeyType.CommonEncryption powinien być używany, gdy klucz zawartości dynamicznej szyfrowania AES-128, ContentKeyType.EnvelopeEncryption powinien być używany.)<br/><br/>Inny przykład CENC ochrony zawartości kreska, teoretycznie jeden klucz zawartości mogą być używane do ochrony strumienia wideo i innej zawartości klawisz, aby chronić strumieniem audio. |
| Wiele-do - wielu |Kombinacja powyższych dwóch scenariuszy: jeden zestaw kluczy zawartości są używane dla każdego z wielu zasobów w tej samej zasobów "grupy". |

Innym ważnym czynnikiem do przeanalizowania jest użycie stałe i nietrwałe licencji.

Te zagadnienia są ważne

Mają one bezpośredni wpływ na koszt dostarczania licencji, korzystając z chmury publicznej w celu dostarczania licencji. Zastanówmy następujących przypadków projektowania w celu zilustrowania:

1. Miesięczna subskrypcja: Użyj trwałego licencji i 1-do wielu zawartości mapowanie klucz do zasobów. Na przykład wszystkie filmy dzieci używamy jednego klucza zawartości do szyfrowania. W takim przypadku:

    Łączna liczba licencji # wymagane dla wszystkich dzieci filmy urządzenia = 1
2. Miesięczna subskrypcja: Użyj nietrwałe licencji i 1-do-1 mapowanie między klucz zawartości i zasobów. W takim przypadku:

    Łączna liczba licencji # wymagane dla wszystkich dzieci filmy urządzenia = [filmy # obserwowane] x [sesji #]

Jak łatwo można zauważyć, dwóch różnych projektów powoduje, dlatego bardzo różnych licencji wzorce dostarczania licencji kosztów w przypadku usługi dostarczania licencji przez chmury publicznej, takich jak usługi Azure Media Services.

## <a name="mapping-design-to-technology-for-implementation"></a>Mapowanie projektu do technologii dla implementacji
Następnie możemy mapowania naszych ogólnego projektu technologii na platformie Microsoft Azure/usługi Azure Media Services, określając technologii dla każdego bloku konstrukcyjnego.

W poniższej tabeli przedstawiono mapowania:

| **Bloków konstrukcyjnych** | **Technologia** |
| --- | --- |
| **Player** |[Azure Media Player](https://azure.microsoft.com/services/media-services/media-player/) |
| **Dostawcy tożsamości (IDP)** |Usługa Azure Active Directory |
| **Bezpieczne usługi tokenów (STS)** |Usługa Azure Active Directory |
| **Przepływ pracy ochrony DRM** |Dynamiczne ochrony usługi Azure Media Services |
| **Dostarczania licencji DRM** |1. Usługi Azure Media Services licencji dostarczania (PlayReady i Widevine, FairPlay), lub <br/>2. Serwer licencji Axinom <br/>3. Serwer licencji PlayReady niestandardowych |
| **Punkt początkowy** |Punkt końcowy przesyłania strumieniowego usługi Azure Media Services |
| **Zarządzanie kluczami** |Nie jest wymagane dla implementacji |
| **Zarządzanie zawartością** |Aplikacji konsolowej C# |

Innymi słowy zarówno dostawcy tożsamości (IDP) i Secure Token Service (STS) będzie usługi Azure AD. Dla odtwarzacza, użyjemy [interfejsu API usługi Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/). Zarówno usługi Azure Media Services, jak i usługi Azure Media Player obsługuje kreska i CENC z wieloma DRM.

Poniższy diagram przedstawia ogólną strukturę i przepływ z powyższych mapowania technologii.

![CENC na AMS](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Aby skonfigurować szyfrowania dynamicznego CENC, narzędzie do zarządzania zawartością użyje następujące dane wejściowe:

1. Otwórz zawartość;
2. Klucz zawartości z klucza generowania i zarządzania nimi;
3. Adresy URL pozyskiwania licencji;
4. Lista informacji z usługi Azure AD.

Będą dane wyjściowe narzędzia zarządzania zawartością:

1. ContentKeyAuthorizationPolicy zawierający specyfikację w sposób dostarczania licencji weryfikuje JWT token i specyfikacji licencji DRM.
2. AssetDeliveryPolicy zawierający specyfikacje na format, ochrony DRM i adresy URL pozyskiwania licencji przesyłania strumieniowego.

W czasie wykonywania, przepływ jest poniżej:

1. Podczas uwierzytelniania użytkownika zostanie wygenerowany JWT token;
2. Jednym z oświadczeń zawartych w JWT token jest zawierający identyfikator obiektu grupy "EntitledUserGroup" roszczenie "grupy". To oświadczenie będzie służyć do przekazywania "Sprawdź uprawnienia".
3. Pliki do pobrania odtwarzacz klienta manifest CENC zawartości chronionej i "widzi" następujące czynności:

   1. Identyfikator, klucza
   2. zawartość jest chroniona, CENC
   3. Adresy URL pozyskiwania licencji.
4. Odtwarzacz wysyła żądanie nabycie licencji oparte na przeglądarce DRM obsługiwane. Identyfikator klucza w żądaniu nabycie licencji i również zostaną przesłane tokenu JWT. Usługi dostarczania licencji zweryfikuje JWT token i oświadczeń zawartych przed wystawieniem potrzebnych licencji.

## <a name="implementation"></a>Wdrażanie
### <a name="implementation-procedures"></a>Procedury implementacji
Implementacja obejmuje następujące kroki:

1. Przygotowanie ich testu: kodowanie/package wideo testowe do wielokrotnej szybkości transmisji bitów pofragmentowany plik MP4 w usłudze Azure Media Services. Ten zasób nie jest chroniony DRM. DRM ochrony zostaną wykonane przez dynamiczne ochrony później.
2. Tworzenie klucza identyfikator i zawartości kluczy (opcjonalnie z klucza inicjatora). W tym przypadku systemem zarządzania kluczami nie jest potrzebna, ponieważ firma Microsoft ma do czynienia z tylko jednego zestawu kluczy identyfikator i klucz zawartości do kilku zasobów testu;
3. Umożliwia skonfigurowanie usług dostarczania licencji multi DRM trwałego testu API usług AMS. Jeśli używasz serwerów licencji niestandardowych przez firmy lub dostawców firmy zamiast usług licencyjnych w usłudze Azure Media Services, możesz pominąć ten krok i określać adresy URL pozyskiwania licencji w kroku dotyczące konfigurowania dostarczania licencji. Interfejs API usług AMS jest potrzebne do określania szczegółowych niektóre konfiguracje, takie jak ograniczenia zasad autoryzacji, szablony odpowiedzi licencji dla różnych usług licencji DRM, itp. W tej chwili portalu Azure nie jeszcze zapewnia potrzebne interfejsu użytkownika dla tej konfiguracji. Można znaleźć informacji o poziomie interfejsu API i przykładowy kod w dokumencie Julia Kornich: [za pomocą PlayReady i Widevine, dynamicznego szyfrowania Common Encryption](media-services-protect-with-drm.md).
4. Skonfiguruj zasady dostarczania zasobu dla zasobu testu za pomocą interfejsu API usług AMS. Można znaleźć informacji o poziomie interfejsu API i przykładowy kod w dokumencie Julia Kornich: [za pomocą PlayReady i Widevine, dynamicznego szyfrowania Common Encryption](media-services-protect-with-drm.md).
5. Tworzenie i konfigurowanie dzierżawy usługi Azure Active Directory w środowisku Azure.
6. Utworzyć kilka grup i kont użytkowników w dzierżawie usługi Azure Active Directory: należy utworzyć co najmniej "EntitledUser" grupy i dodać użytkownika do tej grupy. Użytkownicy w tej grupie, przechodzą wyboru uprawnień w nabycie licencji i użytkowników w tej grupie nie zakończy się niepowodzeniem do przekazania uwierzytelnianie i nie będzie można uzyskać licencji. Bycia członkiem tej grupy "EntitledUser" to oświadczenie wymagane "grupy" w token JWT wystawione przez usługę Azure AD. To wymaganie oświadczeń powinny być określone w kroku usługi dostarczania licencji DRM wielu Konfigurowanie.
7. Tworzenie aplikacji platformy ASP.NET MVC, która będzie obsługiwać odtwarzacza wideo. Ta aplikacja ASP.NET będzie chroniony przy użyciu uwierzytelniania użytkownika względem dzierżawy usługi Azure Active Directory. Oświadczenia właściwe zostaną uwzględnione w tokenów dostępu uzyskane po uwierzytelnieniu użytkownika. Interfejs API OpenID Connect jest zalecane dla tego kroku. Musisz zainstalować następujących pakietów NuGet:
   * Pakiet instalacyjny Microsoft.Azure.ActiveDirectory.GraphClient
   * Pakiet instalacyjny Microsoft.Owin.Security.OpenIdConnect
   * Pakiet instalacyjny Microsoft.Owin.Security.Cookies
   * Instalacja pakietu Microsoft.Owin.Host.SystemWeb
   * Pakiet instalacyjny Microsoft.IdentityModel.Clients.ActiveDirectory
8. Tworzenie przy użyciu odtwarzacza [interfejsu API usługi Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/). [Interfejs API ProtectionInfo Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/) służy do określenia technologii DRM na innej platformie DRM.
9. Macierzy testów:

| **DRM** | **Przeglądarka** | **Wynik dla prawo użytkownika** | **Wynik nie jest uprawniony użytkownik** |
| --- | --- | --- | --- |
| **PlayReady** |Krawędź MS lub IE11 w systemie Windows 10 |Powiodło się |Niepowodzenie |
| **Widevine** |Chrome w systemie Windows 10 |Powiodło się |Niepowodzenie |
| **FairPlay** |TBD | | |

George Trifonov Azure Media Services zespołu został zapisany blogu udostępnia szczegółowe kroki w procesie konfigurowania usługi Azure Active Directory dla aplikacji ASP.NET MVC player: [integracji Azure Media Services OWIN MVC na podstawie aplikacji w usłudze Azure Active Directory i ograniczenie klucza dostarczania zawartości na podstawie oświadczeń JWT](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

George również został zapisany na blogu [JWT token uwierzytelniania w usłudze Azure Media Services i szyfrowania dynamicznego](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).  

Aby uzyskać informacje dotyczące usługi Azure Active Directory:

* Możesz znaleźć informacje dla deweloperów w [Azure Active Directory przewodnik dewelopera](../active-directory/active-directory-developers-guide.md).
* Można znaleźć informacje o Administratorze w [administrowania Your Azure AD Directory](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>Niektóre pytań w implementacji
Brak niektórych "pytań" w implementacji. Miejmy nadzieję, że na następującej liście "pytań" ułatwiają rozwiązywanie problemów w przypadku napotkania problemów.

1. **Wystawca** URL powinien kończyć się **"/"**.  

    **Grupy odbiorców** powinny być identyfikator klienta aplikacji odtwarzacza i należy również dodać **"/"** na końcu adresu URL wystawcy.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />

    W [dekodera tokenów JWT](http://jwt.calebb.net/), powinny pojawić się **lub** i **iss** jak poniżej w JWT token:

    ![gotcha 1.](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)
2. Dodaj uprawnienia do aplikacji w usłudze AAD (na karcie Konfiguracja aplikacji). Jest to wymagane dla każdej aplikacji (w wersji lokalnej i wdrożone).

    ![gotcha 2](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)
3. Użyj prawo wystawcy w konfigurowaniu dynamiczne CENC ochrony:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>

    Następujące nie będzie działać:

        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />

    Identyfikator GUID jest identyfikatorem dzierżawy usługi AAD Identyfikator GUID można znaleźć w menu podręcznym punktów końcowych w portalu Azure.
4. Członkostwo w grupie GRANT oświadczeń uprawnień. Upewnij się, że w pliku manifestu aplikacji usługi AAD, następujący

    "groupMembershipClaims": "Wszystkie", (wartość domyślna to null)
5. TokenType odpowiednie ustawienie podczas tworzenia wymagania ograniczeń.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Wartość domyślna TokenType, ponieważ dodanie obsługi JWT (AAD) oprócz SWT (ACS), są TokenType.JWT. Jeśli używasz SWT/ACS, należy ustawić na TokenType.SWT.

## <a name="additional-topics-for-implementation"></a>Dodatkowe tematy dotyczące implementacji
Następnie firma Microsoft będzie dysk uss niektóre dodatkowe tematy w naszym projektowania i implementacji.

### <a name="http-or-https"></a>HTTP lub HTTPS?
Aplikacja odtwarzacza ASP.NET MVC, który budujemy musi spełniać następujące wymagania:

1. Uwierzytelnianie użytkowników za pomocą usługi Azure AD, które muszą być w obszarze protokołu HTTPS.
2. Wymiany tokenu JWT między klientem a usługą Azure AD, które muszą być w obszarze protokołu HTTPS.
3. Nabycie licencji DRM przez klienta, który musi być w obszarze HTTPS, jeśli dostarczania licencji są dostarczane przez usługi Azure Media Services. Oczywiście pakiet produktów PlayReady nie wprowadzić HTTPS w celu dostarczania licencji. Jeśli serwer licencji PlayReady znajduje się poza usługi Azure Media Services, można użyć protokołu HTTP lub HTTPS.

W związku z tym aplikacja odtwarzacza ASP.NET zostanie użyty protokół HTTPS, najlepszym rozwiązaniem. Oznacza to, że będzie Azure Media Player na stronie w obszarze HTTPS. Niemniej jednak w przypadku przesyłania strumieniowego możemy użyć protokołu HTTP, dlatego należy wziąć pod uwagę mieszanych problem zawartości.

1. Przeglądarka nie zezwala na zawartość mieszana. Ale zezwalaj na wtyczki dodatku Silverlight i OSMF do sprawnego i KRESKI. Zawartość mieszana jest problem dotyczący zabezpieczeń — jest to spowodowane zagrożenie możliwość wprowadzenia złośliwego JS, co może spowodować zagrożenie dane klienta.  Przeglądarki blokować to domyślnie i wykonanej do tej pory jest jedynym sposobem obejścia go po stronie serwera (źródło), aby umożliwić wszystkie domeny (niezależnie od tego, https lub http). To prawdopodobnie nie jest dobrze albo.
2. Firma Microsoft należy unikać zawartości mieszanej: zarówno za pomocą protokołu HTTP albo oba rozwiązania używają protokołu HTTPS. Podczas odtwarzania zawartości mieszanej techniczna silverlightSS wymaga wyczyszczenia mieszanych ostrzeżenie zawartości. techniczna flashSS obsługuje zawartość mieszaną bez mieszanych ostrzeżenia zawartości.
3. Jeśli punkt końcowy przesyłania strumieniowego został utworzony przed 2014 sierpnia, nie obsługuje protokołu HTTPS. W takim przypadku Utwórz i używać nowego punktu końcowego przesyłania strumieniowego dla protokołu HTTPS.

W implementacji odwołania, dla zawartości DRM chronionych aplikacji i przesyłania strumieniowego będą HTTTPS. Otwórz zawartość odtwarzacz nie być konieczne uwierzytelniania lub licencji, dlatego należy liberty do używania protokołu HTTP lub HTTPS.

### <a name="azure-active-directory-signing-key-rollover"></a>Usługa Azure Active Directory Przerzucanie klucza podpisywania
Jest to należy wziąć pod uwagę implementacji. Jeśli użytkownik nie należy traktować to w implementacji, ukończonych systemu ostatecznie przestaną działać całkowicie w ciągu maksymalnie 6 tygodni.

Usługi Azure AD standardami branżowymi do ustanawiania relacji zaufania między sobą i aplikacji przy użyciu usługi Azure AD. W szczególności usługi Azure AD używa klucza podpisywania, który składa się z pary kluczy publicznych i prywatnych. Gdy usługi Azure AD tworzy token zabezpieczający, który zawiera informacje o użytkowniku, token ten jest podpisany przez usługę Azure AD przy użyciu jego klucz prywatny, przed wysłaniem do aplikacji. Aby sprawdzić, czy token jest prawidłowy i faktycznie zdalnych z usługi Azure AD, aplikacja musi zweryfikować podpisu tokenu przy użyciu klucza publicznego udostępnianych przez usługi Azure AD, zawarte w dokument metadanych usług federacyjnych dzierżawcy. Ten klucz publiczny — i klucza podpisywania, z którego pochodzi — jest taka sama jak używane dla wszystkich dzierżawców w usłudze Azure AD.

Szczegółowe informacje o Przerzucanie klucza usługi Azure AD można znaleźć w dokumencie: [ważne informacje dotyczące podpisywania przerzucania klucza w usłudze Azure AD](../active-directory/active-directory-signing-key-rollover.md).

Między [pary kluczy publiczno prywatnych](https://login.microsoftonline.com/common/discovery/keys/),

* Klucz prywatny jest używany przez usługę Azure Active Directory do wygenerowania tokenu JWT;
* Klucz publiczny jest używany przez aplikację, takie jak usługi dostarczania licencji DRM w AMS można zweryfikować tokenu JWT;

W celu zabezpieczenia usługi Azure Active Directory obraca tego certyfikatu okresowo (co tydzień). W przypadku naruszenia zabezpieczeń Przerzucanie klucza może wystąpić dowolnej chwili. W związku z tym usługi dostarczania licencji w AMS wymaga zaktualizowania klucza publicznego używany jako pary kluczy obraca usługi Azure AD, w przeciwnym razie token uwierzytelniania w AMS zakończy się niepowodzeniem i pojawi się żadna licencja.

Jest to osiągane przez ustawienie TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument podczas konfigurowania usług dostarczania licencji DRM.

Przepływ tokenu JWT jest poniżej:

1. Usługi Azure AD wystawia JWT token przy użyciu bieżącego klucza prywatnego dla użytkowników uwierzytelnionych;
2. Gdy odtwarzacza widzi CENC z wieloma DRM chroniona zawartość, najpierw zlokalizuje tokenu JWT wystawione przez usługę Azure AD.
3. Odtwarzacz wysyła żądanie nabycie licencji przy użyciu tokenu JWT do usługi dostarczania licencji w AMS;
4. Usługi dostarczania licencji w AMS użyje bieżącego/prawidłowy klucz publiczny z usługi Azure AD można zweryfikować tokenu JWT przed wystawieniem licencji.

Usług dostarczania licencji DRM będzie zawsze wyszukiwanie bieżącej/prawidłowy klucz publiczny z usługi Azure AD. Klucz publiczny przedstawiony przez usługę Azure AD będzie klucz służący do weryfikowania tokenu JWT wystawione przez usługę Azure AD.

Co zrobić, jeśli Przerzucanie klucza się stanie po AAD generuje JWT token, ale przed tokenu JWT token jest wysyłany przez graczy do usług dostarczania licencji DRM w AMS na potrzeby weryfikacji?

Ponieważ klucz mogła zostać wycofana w dowolnym momencie, istnieje więcej niż jeden prawidłowy klucz publiczny dostępnych w dokumencie metadanych federacji. Azure dostarczania licencji Media Services można użyć dowolnego z określonych w dokumencie kluczy od jednego klucza mogła zostać wycofana wkrótce, innego może być zastąpienia itd.

### <a name="where-is-the-access-token"></a>Gdzie znajduje się Token dostępu?
Jeśli przyjrzymy się jak aplikacji sieci web wywołuje aplikację interfejsu API w obszarze [tożsamość aplikacji z OAuth 2.0 klienta poświadczenia Grant](../active-directory/develop/active-directory-authentication-scenarios.md#web-application-to-web-api), przepływ uwierzytelniania jest poniżej:

1. Użytkownik jest zalogowany do usługi Azure AD w aplikacji sieci web (zobacz [przeglądarki sieci Web do aplikacji sieci Web](../active-directory/develop/active-directory-authentication-scenarios.md#web-browser-to-web-application).
2. Punkt końcowy autoryzacji usługi Azure AD przekierowuje agenta użytkownika do aplikacji klienckiej z kodem autoryzacji. Agent użytkownika zwraca kod autoryzacji identyfikator URI przekierowania aplikacji klienckiej.
3. Aplikacja sieci web musi uzyskać token dostępu, który można uwierzytelniać się na interfejsie API sieci web i pobrać żądanego zasobu. Dobrym żądania tokenu punktu końcowego usługi Azure AD, podając poświadczenia, identyfikator klienta i aplikacji sieci web interfejsu API identyfikator URI. Stanowi kod autoryzacji w celu potwierdzenia, że użytkownik zgodził.
4. Usługi Azure AD uwierzytelnia aplikacji i zwraca token dostępu JWT, który służy do wywołania interfejsu API sieci web.
5. Przy użyciu protokołu HTTPS aplikacji sieci web używa zwróconych tokenu dostępu JWT Aby dodać ciąg tokenu JWT z oznaczeniem "Bearer" w nagłówku autoryzacji żądania do interfejsu API sieci web. Interfejs API sieci web sprawdza poprawność tokenu JWT i Jeśli weryfikacja zakończy się pomyślnie, zwraca wartość żądanego zasobu.

W tym przepływie "tożsamość aplikacji" interfejsu API sieci web ufa, że aplikacja sieci web uwierzytelnił użytkownika. Z tego powodu ten wzorzec jest wywoływana zaufany podsystem. [Diagramu na tej stronie](https://docs.microsoft.com/azure/active-directory/active-directory-protocols-oauth-code) w tym artykule opisano, jak kod autoryzacji przyznać przepływ działania.

W przypadku nabycia licencji z ograniczenia tokenu firma Microsoft po tego samego wzorca zaufany podsystem. I usługi dostarczania licencji w usłudze Azure Media Services zasobu interfejsu API, "zasobu wewnętrznej bazy danych" aplikacja sieci web wymaga dostępu do sieci web. Gdzie jest to token dostępu?

Firma Microsoft są w rzeczywistości uzyskiwania tokenu dostępu z usługi Azure AD. Po uwierzytelnieniu użytkownik pomyślnie zostanie zwrócony kod autoryzacji. Kod autoryzacji jest następnie używany, oraz klucz identyfikator i aplikacji klienta do programu exchange dla tokenu dostępu. I token dostępu ma dostęp do aplikacji "wskaźnik" wskazuje lub reprezentujący usługi dostarczania licencji usługi Azure Media Services.

Musimy rejestracji i konfiguracji aplikacji "wskaźnik" w usłudze Azure AD, wykonując następujące czynności:

1. W dzierżawie usługi Azure AD

   * Dodawanie aplikacji (zasobów) z adresem URL logowania:

   https://[resource_name].azurewebsites.NET/ i

   * adres URL Identyfikatora aplikacji:

   https://[aad_tenant_name].onmicrosoft.com/[resource_name];
2. Dodaj nowy klucz dla aplikacji zasobów;
3. Aktualizacja pliku manifestu aplikacji tak, aby właściwość groupMembershipClaims ma następującą wartość: "groupMembershipClaims": "Wszystkie",  
4. W aplikacji usługi Azure AD, wskazujący player aplikacji sieci web w sekcji "uprawnień dotyczących innych aplikacji", Dodaj aplikacji zasobu, który został dodany w kroku 1. Sprawdź znacznik "Dostępu [resource_name]" w obszarze "delegowane uprawnienia". Dzięki temu uprawnienia aplikacji sieci web do utworzenia tokenów dostępu do uzyskiwania dostępu do zasobów aplikacji. Ten krok należy wykonać dla wersji lokalnych i wdrożonej aplikacji sieci web Jeśli tworzysz aplikację sieci web programu Visual Studio i platformy Azure.

W związku z tym tokenu JWT wystawione przez usługę Azure AD jest token dostępu do uzyskiwania dostępu do tego zasobu "wskaźnik".

### <a name="what-about-live-streaming"></a>Jak Live przesyłania strumieniowego?
W przypadku powyższych naszych dyskusji ma zostały koncentrujących się na zasoby na żądanie. Jak transmisja strumieniowa na żywo?

Dobre wieści jest, można użyć dokładnie tego samego projekt i implementację w ochrony transmisja strumieniowa na żywo w usłudze Azure Media Services, traktując zasób skojarzony z programem jako zasób"VOD".

W szczególności jest dobrze znany, że w na żywo, przesyłania strumieniowego w usłudze Azure Media Services, należy utworzyć kanał, a następnie program w kanale. Aby utworzyć program, należy utworzyć zasób będzie zawierać na żywo archiwum dla programu. Aby zapewnić CENC z wieloma DRM ochrony zawartości na żywo, wszystko, czego należy wykonać, jest zastosowanie tej samej konfiguracji/przetwarzania w zasobie tak, jakby było "zasobów VOD" przed uruchomieniem programu.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Co serwerów licencji poza usługi Azure Media Services?
Często klienci mogą zainwestowały w licencji farmy serwerów jest w ich własnych danych Centrum lub hostowanej przez dostawców usług DRM. Na szczęście ochrony zawartości Azure Media Services umożliwia działają w trybie hybrydowego: zawartości hostowanej i dynamicznie chroniony w usłudze Azure Media Services, podczas gdy licencji DRM są dostarczane przez serwery poza usługi Azure Media Services. W takim przypadku istnieją następujące zagadnienia dotyczące zmian:

1. Zabezpieczanie usługi tokenu musi wystawiać tokeny, które są dozwolone i można sprawdzić w farmie serwerów licencji. Na przykład serwerów licencji Widevine, które są udostępniane przez Axinom wymaga określonych token JWT, zawierający "uprawnienie komunikat". W związku z tym musisz mieć tokenu Zabezpieczającego wydania takich tokenu JWT. Autorów zakończył takie implementację i szczegółowe informacje można znaleźć w następującym dokumencie w [Centrum dokumentacji platformy Azure](https://azure.microsoft.com/documentation/): [przy użyciu Axinom na potrzeby dostarczania licencji Widevine do usługi Azure Media Services](media-services-axinom-integration.md).
2. Nie trzeba skonfigurować usługi dostarczania licencji (ContentKeyAuthorizationPolicy) w usłudze Azure Media Services. Co należy zrobić ma na celu dostarczenie adresy URL pozyskiwania licencji (w przypadku PlayReady i Widevine FairPlay) podczas konfigurowania AssetDeliveryPolicy z konfigurowaniem CENC z wieloma DRM.

### <a name="what-if-i-want-to-use-a-custom-sts"></a>Co zrobić, jeśli ma być użyty niestandardowy STS?
Może istnieć kilka przyczyn, dla których klient może wybrać niestandardowy STS (Secure Token Service) udostępnia tokenów JWT. Niektóre z nich są:

1. Dostawca tożsamości (IDP) używanych przez klienta nie obsługuje usługi STS. W takim przypadku niestandardowych STS może być opcję.
2. Klient może być konieczne bardziej elastyczne lub zwiększenie poziomu kontroli w integracji usługi STS z systemem rozliczeniowym subskrybenta klienta. Na przykład MVPD operator może oferować wielu OTT subskrybenta pakiety, takie jak premium i podstawowa sportowych, itp. Operator może być odpowiadają oświadczenia do tokenu z pakietem subskrybenta, dzięki czemu jest udostępniana tylko zawartości w pakiecie prawo. W takim przypadku niestandardowych STS zawiera wymagane elastyczność i kontrolę.

Dwie zmiany należy podjąć, korzystając z STS niestandardowych do:

1. Podczas konfigurowania usługi dostarczania licencji dla zasobu, należy określić klucz zabezpieczeń, używany do weryfikacji przez niestandardowe usługi STS (więcej szczegóły poniżej) zamiast bieżącego klucza z usługi Azure Active Directory.
2. Po wygenerowaniu tokenu JTW klucza zabezpieczeń określono zamiast X509 bieżący klucz prywatny certyfikatu w usłudze Azure Active Directory.

Istnieją dwa typy kluczy zabezpieczeń:

1. Klucz symetryczny: ten sam klucz jest używany zarówno generowania i weryfikowania tokenu JWT;
2. Klucza asymetrycznego: pary kluczy publiczno prywatnych w X509 używany jest certyfikat z kluczem prywatnym do szyfrowania/generowania tokenu JWT i klucz publiczny do weryfikowania tokenu.

#### <a name="tech-note"></a>Uwaga techniczna
Jeśli używasz .NET Framework / C# jako platformy programowanie X509 certyfikat używany dla klucza asymetrycznego zabezpieczeń musi mieć klucz o długości co najmniej 2048. Jest to wymagane klasy System.IdentityModel.Tokens.X509AsymmetricSecurityKey w programie .NET Framework. W przeciwnym razie zostanie zgłoszony następujący wyjątek:

IDX10630: "System.IdentityModel.Tokens.X509AsymmetricSecurityKey" do podpisywania nie może być mniejsza niż "2048" bitów.

## <a name="the-completed-system-and-test"></a>Ukończono systemu i testu
Firma Microsoft przeprowadzi kilka scenariuszy w systemie end-to-end ukończone, aby czytelnicy mogą mieć basic "obraz" zachowania przed pobraniem konto logowania.

Aplikacja sieci web odtwarzacza i jego logowania można znaleźć [tutaj](https://openidconnectweb.azurewebsites.net/).

Jeśli potrzebne jest scenariusz "z systemem innym niż zintegrowany": zasoby wideo hostowanych w usłudze Azure Media Services są albo niechronionej lub chronione DRM, ale bez tokenu uwierzytelniania (wydawania licencji kto ją), można przetestować bez logowania (przełączając HTTP w przypadku sieci strumieniowe przesyłanie wideo za pośrednictwem protokołu HTTP).

Jeśli potrzebne jest na trasie zintegrowane scenariusz: zasoby wideo podlega dynamicznej ochrony DRM w usłudze Azure Media Services z tokenu uwierzytelniania i tokenu JWT generowane przez usługę Azure AD, musisz zalogować się.

### <a name="user-login"></a>Dane logowania użytkownika
W celu przetestowania end-to-end zintegrowanego systemu DRM, musisz mieć "konto", utworzone lub dodane.

Jakiego konta?

Mimo że Azure pierwotnie zezwalała na dostęp tylko przez użytkowników kont Microsoft, teraz zezwala na dostęp przez użytkowników z tymi dwoma systemami. Jest to możliwe, ponieważ wszystkie właściwości platformy Azure ufają usłudze Azure AD na potrzeby uwierzytelniania, dzięki czemu usługa Azure AD uwierzytelnia użytkowników w organizacji, i istnieje relacja federacji, w ramach której usługa Azure AD ufa systemowi obsługi tożsamości konsumentów firmy Microsoft na potrzeby uwierzytelniania konsumentów. W ten sposób usługa Azure AD może uwierzytelniać konta „gościa” firmy Microsoft i „natywne” konta usługi Azure AD.

Ponieważ usługa Azure AD ufa domeny konta Microsoft (MSA), można dodać wszystkich kont z dowolnego z następujących domen do niestandardowych usługi Azure AD dzierżawy i użyj konta do logowania:

| **Nazwa domeny** | **Domeny** |
| --- | --- |
| **Domena dzierżawy niestandardowych usługi Azure AD** |somename.onmicrosoft.com |
| **Domeny firmowej** |Microsoft.com |
| **Domena konta Microsoft (MSA)** |Outlook.com, live.com, hotmail.com |

Możesz skontaktować się żadnego autorów konto utworzone lub dodane.

Poniżej przedstawiono zrzuty ekranu strony logowania innego używany przez inną domenę konta.

**Konto domeny dzierżawy niestandardowych usługi Azure AD**: W tym przypadku zostanie wyświetlona strona logowania dostosowane niestandardowej domeny dzierżawy usługi Azure AD.

![Konto domeny dzierżawcy niestandardowe usługi Azure AD](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Konto domeny firmy Microsoft z kartami inteligentnymi**: W tym przypadku pojawi się strona logowania dostosowane przez firmę Microsoft firmowej IT z uwierzytelniania dwuskładnikowego.

![Konto domeny dzierżawcy niestandardowe usługi Azure AD](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Konto Microsoft (MSA)**: W tym przypadku zostanie wyświetlona strona logowania Account firmy Microsoft dla konsumentów.

![Konto domeny dzierżawcy niestandardowe usługi Azure AD](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Przy użyciu nośnika zaszyfrowanych rozszerzeń dla PlayReady
W nowoczesnych przeglądarki z rozszerzeń szyfrowane nośnika (EME) PlayReady pomocy technicznej, takich jak programu Internet Explorer 11 na Windows 8.1 lub nowszej i przeglądarki Microsoft Edge w systemie Windows 10 PlayReady będzie podstawowej DRM dla EME.

![Przy użyciu EME PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

Obszar ciemny player to z faktu, że ochrona PlayReady zapobiega z wprowadzeniem Przechwytywanie ekranu chronionego wideo.

Następujący ekran pokazuje wtyczki player i MSE/EME pomocy technicznej.

![Przy użyciu EME PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

EME Microsoft Edge i 11 programu Internet Explorer w systemie Windows 10 umożliwia wywoływanie z [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) na urządzeniach z systemem Windows 10, które obsługują. PlayReady SL3000 odblokowuje przepływ zawartości rozszerzone premium (4K, HDR, itp.) i nowe modele dostarczania zawartości (wczesne okno rozszerzone zawartości).

Skupić się na urządzeniach z systemem Windows: PlayReady jest tylko DRM w sprz dostępnej na urządzeniach z systemem Windows (PlayReady SL3000). Usługi przesyłania strumieniowego za pomocą PlayReady za pośrednictwem EME lub aplikacji platformy uniwersalnej systemu Windows i oferują wyższej jakości wideo za pomocą PlayReady SL3000 niż innym DRM. Zwykle zawartości 2K będzie przechodził przez Chrome lub Firefox oraz 4K zawartości za pomocą Microsoft Edge/IE11 lub aplikacji platformy uniwersalnej systemu Windows na tym samym urządzeniu (w zależności od ustawienia usługi i implementację).

#### <a name="using-eme-for-widevine"></a>Przy użyciu EME Widevine
W przypadku nowoczesne przeglądarki z obsługą EME/Widevine, takie jak Chrome 41 + w systemie Windows 10, Windows 8.1, Mac OS x Yosemite i Chrome na Android 4.4.4 Google Widevine jest DRM za EME.

![Przy użyciu EME Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Należy zauważyć, że Widevine nie zapobiega z wprowadzeniem przechwytywania ekranu chronione wideo.

![Przy użyciu EME Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Użytkownicy nie prawo
Jeśli użytkownik nie jest członkiem grupy "Użytkownicy pod tytułem", użytkownik nie będzie można przekazać "Sprawdź uprawnienia" i usługi licencji multi DRM odmówi wystawić żądanego licencji, jak pokazano poniżej. Szczegółowy opis jest "uzyskania licencji nie powiodło się", który jest zgodnie z założeniami.

![Cofanie przysługujących użytkowników](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Usługą niestandardowych Secure Token
W scenariuszu uruchomionych niestandardowych Secure Token Service (STS) tokenu JWT zostaną wystawione przez usługę STS niestandardowych przy użyciu klucza w konfiguracji symetrycznej lub asymetrycznej.

W przypadku używania klucza symetrycznego (za pomocą przeglądarki Chrome):

![Uruchomiona STS niestandardowych](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

W przypadku używania klucza asymetrycznego za pośrednictwem X509 certyfikatu (przy użyciu nowoczesnych przeglądarka firmy Microsoft).

![Uruchomiona STS niestandardowych](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

W obu powyższych przypadkach uwierzytelnianie użytkowników pozostaje — za pomocą usługi Azure AD. Jedyna różnica polega na tym, że tokenów JWT są wystawiane przez usługę STS niestandardowych zamiast usługi Azure AD. Oczywiście, podczas konfigurowania ochrony CENC dynamicznych, ograniczenie usługi dostarczania licencji określa typ tokenu JWT klucza w konfiguracji symetrycznej lub asymetrycznej.

## <a name="summary"></a>Podsumowanie
W tym dokumencie omówiono CENC z kilku native DRM i kontroli dostępu za pośrednictwem tokenu uwierzytelniania: projekt i jego wdrożenie za pomocą usługi Azure, Azure Media Services i usługi Azure Media Player.

* Odwołania projektu są prezentowane zawierającą wszystkie niezbędne składniki w podsystemie DRM/CENC;
* Odwołanie do implementacji na platformie Azure, Azure Media Services i Azure Media Player.
* Omówiono także niektóre tematy, które są bezpośrednio związane z projekt i implementację.

## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
