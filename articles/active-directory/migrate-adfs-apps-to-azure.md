---
title: Migracja lokalnych aplikacji usług AD FS na platformę Azure | Microsoft Docs
description: Ten dokument ma ułatwić organizacjom zrozumienie sposobu przeprowadzania migracji lokalnych aplikacji do usługi Azure AD, ze szczególnym uwzględnieniem federacyjnych aplikacji SaaS.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/02/2018
ms.author: billmath
ms.openlocfilehash: f1256e2fefe1ad3d37403f37804888ab9dcf6e99
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="migrate-ad-fs-on-premises-apps-to-azure"></a>Migracja lokalnych aplikacji usług AD FS na platformę Azure 

Ten dokument ma ułatwić organizacjom zrozumienie sposobu przeprowadzania migracji lokalnych aplikacji do usługi Azure AD.  W szczególności skoncentrowano się na federacyjnych aplikacjach SaaS.  Ten dokument nie zawiera wskazówek krok po kroku.  Podano w nim ogólne wskazówki, które pomogą pomyślnie przeprowadzić migrację dzięki zrozumieniu sposobu przekładania konfiguracji lokalnych na usługę Azure AD. Opisano w nim również elementy wymagane przez najbardziej typowe scenariusze.

## <a name="introduction"></a>Wprowadzenie

Jeśli masz katalog lokalny, który zawiera konta użytkownika, prawdopodobnie masz również co najmniej jedną lub dwie aplikacje.  A aplikacje te zostały skonfigurowane dla użytkowników tak, aby umożliwić im dostęp przez zalogowanie przy użyciu tych tożsamości.

I jeśli Twoja firma działa tak jak większość organizacji, prawdopodobnie jesteś właśnie w trakcie implementowania tożsamości i aplikacji w chmurze.  Być może pracujesz z usługą Office 365 i programem Azure AD Connect.  Może dla niektórych kluczowych obciążeń skonfigurowano oparte na chmurze aplikacje SaaS, ale nie dla wszystkich.  

Wiele organizacji często ma — oprócz aplikacji opartych na usługach Office 365 i Azure AD — aplikacje SaaS lub niestandardowe aplikacje biznesowe sfederowane bezpośrednio do lokalnej usługi logowania jednokrotnego, takiej jak Active Directory Federation Service (AD FS).  W tym przewodniku migracji opisano przyczyny i sposób przeprowadzania migracji aplikacji lokalnych do usługi Azure AD.

>[!NOTE]
>Ten przewodnik zawiera szczegółowe informacje dotyczące konfiguracji i migracji aplikacji SaaS oraz ogólne informacje o niestandardowych aplikacjach biznesowych.  W przyszłości planujemy opublikowanie bardziej szczegółowych wskazówek dotyczących niestandardowych aplikacji biznesowych.

Rysunek 1. Aplikacje połączone bezpośrednio w środowisku lokalnym ![środowisko lokalne](media/migrate-adfs-apps-to-azure/migrate1.png)

Rysunek 2. Aplikacje sfederowane przy użyciu usługi Azure AD ![platforma Azure](media/migrate-adfs-apps-to-azure/migrate2.png)

## <a name="why-migrate-apps-to-azure-ad"></a>Dlaczego warto przeprowadzić migrację aplikacji do usługi Azure AD?

W przypadku organizacji, które już korzystają z usług AD FS, Ping lub innego dostawcy uwierzytelniania lokalnego, migrowanie aplikacji do usługi Azure AD oferuje następujące korzyści:

**Bezpieczniejszy dostęp**
- Skonfiguruj szczegółowe procesy kontroli dostępu do poszczególnych aplikacji, w tym usługę Multi-Factor Authentication (MFA), za pomocą funkcji [dostępu warunkowego usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).  Zasady można stosować do aplikacji SaaS i niestandardowych w taki sam sposób, w jaki mogą być one stosowane obecnie w usłudze Office 365
- Aby wykrywać zagrożenia i chronić logowanie w oparciu o uczenie maszynowe i algorytmy heurystyczne identyfikujące ryzykowny ruch, skorzystaj z wbudowanych i stale zmieniających się możliwości usługi Azure AD za pomocą usługi [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

**Współpraca B2B w usłudze Azure AD**
- Gdy logowanie do aplikacji SaaS opiera się na usłudze Azure AD, można przyznać partnerom prawa dostępu do zasobów w chmurze dzięki funkcji [współpracy B2B w usłudze Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

**Prostsze środowisko pracy administratora i dodatkowe możliwości usługi Azure AD**
- Usługa Azure AD jako dostawca tożsamości dla aplikacji SaaS obsługuje dodatkowe możliwości, takie jak certyfikaty podpisywania tokenu w poszczególnych aplikacjach, [konfigurowalne daty wygaśnięcia certyfikatów](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs) i [automatyczna aprowizacja](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) kont użytkowników (w kluczowych aplikacjach z Galerii) w oparciu o tożsamości usługi Azure AD

**Zachowanie korzyści oferowanych przez lokalnego dostawcę tożsamości**
- Zyskując dostęp do korzyści związanych z usługą Azure AD, możesz nadal korzystać z lokalnego rozwiązania do uwierzytelniania, dzięki czemu korzyści, takie jak lokalne rozwiązania usługi Multi-Factor Authentication (MFA), rejestrowanie i inspekcja, są nadal dostępne 

**Rozpoczynanie wycofywania lokalnego dostawcy tożsamości**
- W organizacjach, które chcą wycofać produkt do uwierzytelniania lokalnego, migracja aplikacji do usługi Azure AD umożliwia łatwiejsze przejście dzięki eliminowaniu niektórych czynności 

## <a name="mapping-types-of-apps-on-premises-to-types-of-apps-in-azure-ad"></a>Mapowanie typów aplikacji w środowisku lokalnym na typy aplikacji w usłudze Azure AD
Większość aplikacji można dopasować do jednej z kilku kategorii na podstawie używanego typu logowania.  Te kategorie określają, jak aplikacja jest reprezentowana w usłudze Azure AD.

Krótko mówiąc, aplikacje SAML 2.0 można zintegrować z usługą Azure AD za pomocą galerii aplikacji usługi Azure AD lub jako aplikacje spoza galerii.  Aplikacje używające uwierzytelniania OAuth 2.0 lub OpenID Connect można podobnie zintegrować z usługą Azure AD jako „rejestracje aplikacji”.  Dalsze informacje znajdziesz poniżej.

### <a name="federated-saas-apps-vs-custom-lob-apps"></a>Aplikacje federacyjne SaaS a niestandardowe aplikacje biznesowe
Aplikacje federacyjne to aplikacje, które należą do wymienionych kategorii.

- Aplikacje SaaS 
    - Jeśli użytkownicy logują się do aplikacji SaaS, takich jak Salesforce, ServiceNow lub Workday, i przeprowadzana jest integracja z lokalnym dostawcą tożsamości, takim jak usługi AD FS lub Ping, używasz federacyjnego logowania jednokrotnego dla aplikacji SaaS.
    - Aplikacje zazwyczaj używają protokołu SAML 2.0 na potrzeby logowania federacyjnego.
    - Aplikacje, które należą do tej kategorii, można zintegrować z usługą Azure AD jako aplikacje przedsiębiorstwa, z poziomu galerii albo jako aplikacje spoza galerii.
- Niestandardowe aplikacje biznesowe
    - Ta nazwa odnosi się do aplikacji innych niż SaaS, które opracowano wewnętrznie w organizacji lub które są dostępne jako standardowy produkt w pakiecie zainstalowanym w centrum danych.  Obejmuje to aplikacje programu SharePoint i aplikacje skompilowane w rozwiązaniu Windows Identity Foundation (WIF).
    - Aplikacje mogą używać uwierzytelniania SAML 2.0, WS-Federation, OAuth lub OpenID Connect na potrzeby logowania federacyjnego
    - Aplikacje niestandardowe, które używają protokołu OAuth 2.0, OpenID Connect lub WS-Federation, można zintegrować z usługą Azure AD jako rejestracje aplikacji, a aplikacje niestandardowe, które używają protokołu SAML 2.0 i WS-Federation, można zintegrować jako aplikacje spoza galerii w aplikacjach przedsiębiorstwa

### <a name="non-federated-apps"></a>Aplikacje inne niż federacyjne
Ponadto aplikacje inne niż federacyjne można zintegrować z usługą Azure AD przy użyciu serwera proxy aplikacji usługi Azure AD i powiązanych możliwości.  Aby uzyskać więcej informacji na temat możliwości, skorzystaj z poniższych linków:
- Aplikacje korzystające ze zintegrowanego uwierzytelniania systemu Windows (WIA, Windows Integrated Auth) bezpośrednio do usługi Active Directory
    - Te aplikacje można zintegrować z usługą Azure AD za pomocą [serwera proxy aplikacji usługi Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal)
- Aplikacje integrowane z dostawcą logowania jednokrotnego za pośrednictwem agenta i używające nagłówków na potrzeby autoryzacji
    - Aplikacje lokalne, które używają zainstalowanego agenta do logowania jednokrotnego autoryzacji opartej na nagłówkach, można skonfigurować na potrzeby logowania opartego na usłudze Azure AD przy użyciu serwera proxy aplikacji usługi Azure AD i funkcji [dostępu usługi Ping dla usługi Azure AD](https://blogs.technet.microsoft.com/enterprisemobility/2017/06/15/ping-access-for-azure-ad-is-now-generally-available-ga/)

## <a name="translating-on-premises-federated-apps-to-azure-ad"></a>Tłumaczenie lokalnych aplikacji federacyjnych na usługę Azure AD 
Na szczęście usługi AD FS i Azure AD działają podobnie, dlatego pojęcia dotyczące konfigurowania adresów URL i identyfikatorów związanych z relacjami zaufania, logowania i wylogowywania można stosować w obydwu przypadkach.  Istnieją jednak niewielkie różnice, które należy zrozumieć w przypadku przejścia.

W tabeli przedstawiliśmy kilka kluczowych koncepcji współdzielonych przez usługi AD FS i Azure AD oraz aplikacje SaaS, co ułatwia ich przekład. 

### <a name="representing-the-app-in-azure-ad-or-ad-fs"></a>Reprezentowanie aplikacji w usługach Azure AD lub AD FS
Migracja rozpoczyna się od oceny konfiguracji aplikacji w środowisku lokalnym i sposobu mapowania tej konfiguracji do usługi Azure AD.  Poniżej przedstawiono mapowanie elementów konfiguracji jednostek uzależnionych usług AD FS na odpowiednie elementy w usłudze Azure AD.  
- Termin dotyczący usług AD FS: jednostka uzależniona lub zaufanie jednostki uzależnionej
- Termin dotyczący usługi Azure AD: aplikacja przedsiębiorstwa lub rejestracja aplikacji (w zależności od typu aplikacji)

|Element konfiguracji aplikacji|Opis|W ramach konfiguracji usług AD FS|Odpowiednia lokalizacja w konfiguracji usługi Azure AD|Element tokenu języka SAML|
|-----|-----|-----|-----|-----|
|Adres URL logowania do aplikacji|Adres URL strony logowania do danej aplikacji. Jest to miejsce, do którego użytkownik przechodzi w celu zainicjowania logowania do aplikacji w ramach przepływu SAML zainicjowanego przez SP.|Nie dotyczy|W usłudze Azure AD adres URL logowania jest konfigurowany w witrynie Azure Portal w obszarze właściwości logowania jednokrotnego aplikacji jako adres URL logowania.</br></br>(W celu wyświetlenia adresu URL być może trzeba będzie kliknąć pozycję Pokaż zaawansowane ustawienia adresu URL logowania)||
|Adres URL odpowiedzi aplikacji|Adres URL aplikacji z perspektywy dostawcy tożsamości.  Jest to miejsce, do którego wysyła się użytkownika i token, gdy użytkownik zaloguje się do dostawcy tożsamości.</br></br>  Jest ono również czasem nazywane punktem końcowym odbiorcy potwierdzeń SAML.|Znajduje się w relacji zaufania jednostki uzależnionej usług AD FS dla aplikacji.  Kliknij prawym przyciskiem myszy jednostkę uzależnioną i wybierz pozycję „Właściwości” -> karta „Punkty końcowe”.|W usłudze Azure AD adres URL odpowiedzi jest konfigurowany w witrynie Azure Portal w obszarze właściwości logowania jednokrotnego aplikacji jako adres URL odpowiedzi.</br></br>(W celu wyświetlenia adresu URL odpowiedzi być może trzeba będzie kliknąć pozycję Pokaż zaawansowane ustawienia adresu URL)|Mapowany do elementu docelowego w tokenie SAML.</br></br>  Przykładowa wartość: https://contoso.my.salesforce.com|
|Adres URL wylogowania aplikacji|Adres URL, do którego są wysyłane żądania „czyszczenia wylogowania”, gdy użytkownik wylogowuje się z aplikacji — ma to na celu wylogowanie ze wszystkich innych aplikacji, do których dostawca tożsamości zalogował użytkownika.|Znajduje się w funkcji zarządzania usługami w obszarze relacji zaufania jednostki uzależnionej.  Kliknij prawym przyciskiem myszy jednostkę uzależnioną i wybierz pozycję „Właściwości” -> kliknij kartę „Punkty końcowe”|Nie dotyczy — usługa Azure AD nie obsługuje „wylogowania jednokrotnego”, czyli wylogowania ze wszystkich aplikacji.  Wylogowuje ona po prostu użytkownika z samej usługi Azure AD.|Nie dotyczy|
|Identyfikator aplikacji|Identyfikator aplikacji z perspektywy dostawcy tożsamości. Wartość adresu URL logowania jest często używana jako identyfikator (ale nie zawsze)</br></br>  Czasami w aplikacji ta wartość jest nazywana „identyfikatorem jednostki”.|W usługach AD FS jest to identyfikator jednostki uzależnionej: kliknij prawym przyciskiem myszy relację zaufania jednostki uzależnionej, a następnie wybierz pozycję „Właściwości” -> kliknij kartę „Identyfikatory”|W usłudze Azure AD identyfikator jest konfigurowany we właściwościach logowania jednokrotnego aplikacji w witrynie Azure Portal jako identyfikator w obszarze domeny i adresów URL (być może trzeba będzie kliknąć pole wyboru „Pokaż zaawansowane ustawienia adresu URL”)|Odnosi się do elementu odbiorców w tokenie SAML|
|Metadane federacji aplikacji|Lokalizacja metadanych federacji aplikacji.  Używana przez dostawców tożsamości do automatycznego konfigurowania określonych ustawień konfiguracji, takich jak punkty końcowe lub certyfikaty szyfrowania.|Adres URL metadanych federacji aplikacji znajduje się w relacji zaufania jednostki uzależnionej usług AD FS dla aplikacji.  Kliknij prawym przyciskiem myszy relację zaufania, a następnie wybierz pozycję Właściwości i kliknij kartę Monitorowanie.|Nie dotyczy — usługa Azure AD nie obsługuje bezpośredniego korzystania z metadanych federacji aplikacji|Nie dotyczy|
|Identyfikator użytkownika/NameID|Atrybut używany do jednoznacznego wskazywania tożsamości użytkownika z usług Azure AD lub AD FS do aplikacji.</br></br>  Jest to przeważnie główna nazwa lub adres e-mail użytkownika.|W usługach AD FS jest to reguła oświadczenia powiązana z jednostką uzależnioną.  W większości przypadków jest to reguła oświadczenia, która wystawia oświadczenie z typem kończącym się ciągiem „nameidentifier”|W usłudze Azure AD identyfikator użytkownika można znaleźć we właściwościach logowania jednokrotnego aplikacji w witrynie Azure Portal w obszarze atrybutów użytkownika w nagłówku.</br></br>Domyślnie jest używana główna nazwa użytkownika.|Wartość przekazywana przez dostawcę tożsamości do aplikacji jako element „NameID” w tokenie języka SAML.|
|Inne oświadczenia do wysłania do aplikacji|Oprócz identyfikatora użytkownika/elementu NameID dostawca tożsamości przeważnie wysyła do aplikacji inne informacje o oświadczeniu, na przykład imię, nazwisko, adres e-mail i grupy, do których należy użytkownik|W usługach AD FS są to inne reguły oświadczenia powiązane z jednostką uzależnioną.|W usłudze Azure AD ten element można znaleźć we właściwościach logowania jednokrotnego aplikacji w witrynie Azure Portal w obszarze atrybutów użytkownika w nagłówku. Kliknij pozycję Wyświetl i edytuj wszystkie inne atrybuty użytkownika.|| 

### <a name="representing-azure-ad-as-an-identity-provider-idp-in-a-saas-app"></a>Reprezentowanie usługi Azure AD jako dostawcy tożsamości w aplikacji SaaS
W ramach migracji aplikację należy skonfigurować tak, aby wskazywała usługę Azure AD (w odróżnieniu od lokalnego dostawcy tożsamości).  W tej sekcji skupiono się przede wszystkim na aplikacjach SaaS, które używają protokołu SAML, a nie na aplikacjach biznesowych/niestandardowych. Opisane pojęcia będą jednak dotyczyć również aplikacji niestandardowych/biznesowych. 

Na poziomie ogólnym istnieje kilka kluczowych zagadnień dotyczących wskazywania usługi Azure AD w aplikacji SaaS
- Wystawca dostawcy tożsamości: https&#58;//sts.windows.net/{tenant-id}/
- Adres URL logowania dostawcy tożsamości: https&#58;//login.microsoftonline.com/{tenant-id}/saml2
- Adres URL wylogowywania dostawcy tożsamości: https&#58;//login.microsoftonline.com/{tenant-id}/saml2 
- Lokalizacja metadanych federacji: https&#58;//login.windows.net/{tenant-id} <tenant-id>/federationmetadata/2007-06/federationmetadata.xml?appid={<application-id} 

Gdzie element {tenant-id} jest zastępowany identyfikatorem dzierżawy znajdującym się w witrynie Azure Portal w obszarze Azure Active Directory -> Właściwości jako „identyfikator katalogu”, a element {application-id} jest zastępowany identyfikatorem aplikacji znajdującym się we właściwościach aplikacji jako „identyfikator aplikacji”

W tabeli bardziej szczegółowo opisano kluczowe elementy konfiguracji dostawcy tożsamości do konfigurowania ustawień logowania jednokrotnego w aplikacji oraz ich wartości lub lokalizacje w usługach AD FS i Azure AD.  Układem odniesienia tabeli jest aplikacja SaaS, która musi wiedzieć, dokąd wysyłać żądania uwierzytelniania i jak weryfikować odebrane tokeny.

|Element konfiguracji|Opis|AD FS|Azure AD|
|---|---|---|---|
|Dostawca tożsamości </br>Logowanie </br>Adres URL|Adres logowania dostawcy tożsamości z perspektywy aplikacji (do którego użytkownik jest przekierowany na potrzeby logowania).|Adres URL logowania usług AD FS to nazwa usługi federacyjnej AD FS, po której następuje ciąg „/adfs/ls/”, na przykład: https&#58;//fs.contoso.com/adfs/ls/|Odpowiadająca wartość w usłudze Azure AD jest zgodna z wzorcem, w którym element {tenant-id} jest zastępowany identyfikatorem dzierżawy znajdującym się w witrynie Azure Portal w obszarze Azure Active Directory -> Właściwości jako „identyfikator katalogu”.</br></br>W przypadku aplikacji korzystających z protokołu SAML-P: https&#58;//login.microsoftonline.com</br>/{tenant-id}/saml2 </br></br>W przypadku aplikacji korzystających z protokołu WS-Federation: https&#58;//login.microsoftonline.com</br>/{tenant-id}/wsfed|
|Dostawca tożsamości </br>Wylogowanie </br>Adres URL|Adres URL wylogowania z perspektywy aplikacji (miejsce docelowe przekierowania użytkownika w celu „wylogowania” z aplikacji).|W usługach AD FS adres URL wylogowania jest taki sam jak adres URL logowania lub jest to ten sam adres URL z dołączonym ciągiem „wa=wsignout1.0”, na przykład https&#58;//fs.contoso.com/adfs/ls/?wa=wsignout1.0|Odpowiadająca wartość w usłudze Azure AD zależy od tego, czy aplikacja może obsługiwać wylogowywanie w protokole SAML 2.0.</br></br>Jeśli aplikacja obsługuje wylogowywanie w protokole SAML, wartość jest zgodna z wzorcem, w którym wartość elementu {tenant-id} jest zastępowana identyfikatorem dzierżawy znajdującym się w witrynie Azure Portal w obszarze Azure Active Directory -> Właściwości jako „identyfikator katalogu”. https&#58;//login.microsoftonline.com</br>/{tenant-id}/saml2</br></br>Jeśli aplikacja nie obsługuje wylogowywania protokołu SAML: https&#58;//login.microsoftonline.com</br>/common/wsfederation?wa=wsignout1.0|
|Token </br>Podpisywanie </br>Certyfikat|Certyfikat, którego klucza prywatnego dostawca tożsamości używa do podpisywania wystawionych tokenów.  Sprawdza, czy token pochodzi od tego samego dostawcy tożsamości, z którym aplikacja ma skonfigurowaną relację zaufania.|Certyfikat podpisywania tokenu usług AD FS znajduje się w funkcji zarządzania usługami AD FS w obszarze Certyfikaty.|W usłudze Azure AD certyfikat podpisywania tokenu można znaleźć we właściwościach logowania jednokrotnego aplikacji w witrynie Azure Portal w obszarze certyfikatu podpisywania SAML nagłówka, z którego można pobrać certyfikat do przekazania do aplikacji.</br></br>  Jeśli aplikacja ma więcej niż jeden certyfikat, wszystkie certyfikaty można znaleźć w pliku XML metadanych federacji.|
|Identyfikator/ </br>„wystawca”|Identyfikator dostawcy tożsamości z perspektywy aplikacji (czasami nazywany „wystawcą” lub „identyfikatorem wystawcy”)</br></br>W tokenie języka SAML wartość jest wyświetlana jako element „Wystawca”|Identyfikator w usługach AD FS to zazwyczaj identyfikator usługi federacyjnej w funkcji zarządzania usługami AD FS w obszarze Usługa -> Edytuj właściwości usługi federacyjnej.  Na przykład: http&#58;//fs.contoso.com/adfs/services/trust|Odpowiadająca wartość w usłudze Azure AD jest zgodna z wzorcem, w którym wartość elementu {tenant-id} jest zastępowana identyfikatorem dzierżawy znajdującym się w witrynie Azure Portal w obszarze Azure Active Directory -> Właściwości jako „identyfikator katalogu”.  https&#58;//sts.windows.net/{tenant-id}/|
|Dostawca tożsamości </br>Federacja </br>Metadane|Lokalizacja publicznie dostępnych metadanych federacji dostawcy tożsamości.  (Metadane federacji są używane przez niektóre aplikacje jako alternatywa sytuacji, w której administrator indywidualnie konfiguruje adresy URL, identyfikator i certyfikat podpisywania tokenu)|Adres URL metadanych federacji usług AD FS można znaleźć w funkcji zarządzania usługami AD FS w obszarze Usługa -> Punkty końcowe -> Metadane -> wpisz: metadane federacji, na przykład: https&#58;//fs.contoso.com/FederationMetadata/2007-06/</br>FederationMetadata.xml|Odpowiadająca wartość w usłudze Azure AD jest zgodna z wzorcem https&#58;//login.microsoftonline.com</br>/{TenantDomainName}/FederationMetadata/2007-06/</br>FederationMetadata.xml, gdzie wartość {TenantDomainName} jest zastępowana nazwą dzierżawy w formacie „contoso.onmicrosoft.com” </br></br>[Więcej informacji](https://docs.microsoft.com/azure/active-directory/develop/active-directory-federation-metadata) o metadanych federacji w usłudze Azure AD.

## <a name="migrating-saas-apps"></a>Migrowanie aplikacji SaaS
Migrowanie aplikacji SaaS z usług AD FS lub innego dostawcy tożsamości do usługi Azure Active Directory jest obecnie procesem wykonywanym ręcznie. Aby uzyskać wskazówki specyficzne dla aplikacji, [zobacz listę samouczków dotyczących integrowania aplikacji SaaS znajdujących się w Galerii](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

W samouczkach integracji założono, że integracja jest przeprowadzana w trybie Green Field.  Istnieje kilka podstawowych pojęć dotyczących migracji, które należy poznać, jeśli planujesz, oceniasz i konfigurujesz aplikacje lub przeprowadzasz ich migrację jednorazową.  
- Podczas gdy migracja niektórych aplikacji jest prosta, aplikacje o bardziej złożonych wymaganiach, takie jak oświadczenia niestandardowe, mogą wymagać dodatkowej konfiguracji w usłudze Azure AD i/lub programie Azure AD Connect
- W najbardziej typowych scenariuszach aplikacja wymaga tylko oświadczenia NameId i innych typowych oświadczeń identyfikatora użytkownika. Aby określić, czy dodatkowe oświadczenia są wymagane, należy sprawdzić, które oświadczenia są wystawiane z usług AD FS przez lub zewnętrznego dostawcę tożsamości
- Jeśli ustalono, że dodatkowe oświadczenia są wymagane, należy upewnić się, że są one dostępne w usłudze Azure AD.  Musisz sprawdzić konfigurację synchronizacji programu Azure AD Connect, aby upewnić się, że wymagany atrybut, na przykład samAccountName, jest synchronizowany z usługą Azure AD
- Gdy atrybuty są dostępne w usłudze Azure AD, dodaj reguły wystawiania oświadczeń w usłudze Azure AD, aby uwzględnić te atrybuty jako oświadczenia w wystawionych tokenach.  Ta czynność jest wykonywana we właściwościach logowania jednokrotnego aplikacji w usłudze Azure AD.

### <a name="assessing-what-can-be-migrated"></a>Ocenianie aplikacji, które można migrować
Aplikacje SAML 2.0 można zintegrować z usługą Azure AD za pomocą galerii aplikacji usługi Azure AD lub jako aplikacje spoza galerii.  

Istnieją pewne konfiguracje, które wymagają wykonania dodatkowych czynności w celu skonfigurowania w usłudze Azure AD, a niektóre nie są obecnie obsługiwane.  Aby ustalić, co można przenieść, zapoznaj się z bieżącą konfiguracją każdej aplikacji, a w szczególności z następującymi elementami:
- Skonfigurowane reguły oświadczeń (reguły przekształcania wystawiania)
- Atrybut i format identyfikatora SAML NameID
- Wystawione wersje tokenu języka SAML
- Inne konfiguracje, takie jak reguły autoryzacji wystawiania lub zasady kontroli dostępu i reguły uwierzytelniania wieloskładnikowego (uwierzytelniania dodatkowego)

#### <a name="what-can-be-migrated-today"></a>Co można obecnie migrować
Aplikacje, które można obecnie łatwo migrować, to aplikacje SAML 2.0 używające standardowego zestawu elementów konfiguracji i oświadczeń.  Te aplikacje składają się z następujących elementów:
- Nazwa główna użytkownika
- Adres e-mail
- Imię
- Nazwisko
- Atrybut alternatywny, taki jak NameID w języku SAML, w tym atrybut poczty usługi Azure AD, prefiks poczty, identyfikator pracownika, atrybuty rozszerzenia 1–15 lub lokalny element SamAccountName (zobacz [Editing the NameIdentifier claim [Edytowanie oświadczenia NameIdentifier])](./develop/active-directory-saml-claims-customization.md)
- Oświadczenia niestandardowe (zobacz dokument [tutaj](active-directory-claims-mapping.md) i [tutaj](./develop/active-directory-saml-claims-customization.md), aby uzyskać informacje na temat mapowania obsługiwanych oświadczeń)

Oprócz oświadczeń niestandardowych i elementów nameID, konfiguracje, które wymagają wykonania dodatkowych kroków konfiguracji w usłudze Azure AD w ramach migracji, to:
- Niestandardowe reguły autoryzacji lub uwierzytelniania MFA w usługach AD FS (skonfigurowane za pomocą funkcji [dostępu warunkowego usługi Azure AD](active-directory-conditional-access-azure-portal.md))
- Aplikacje z wieloma punktami końcowymi języka SAML można skonfigurować w usłudze Azure AD przy użyciu programu PowerShell (ta funkcja jest niedostępna w portalu)
- Aplikacje WS-Federation, takie jak aplikacje programu SharePoint, które wymagają tokenów w wersji SAML 1.1, należy skonfigurować ręcznie przy użyciu programu PowerShell

#### <a name="appsconfigurations-not-supported-in-azure-ad-today"></a>Aplikacje/konfiguracje nieobsługiwane obecnie w usłudze Azure AD
Obecnie nie można migrować aplikacji, które wymagają wymienionych poniżej możliwości.  Jeśli masz aplikacje, które wymagają tych funkcji, wprowadź swoją opinię w sekcji komentarzy.
- Możliwości protokołu
    - Obsługa wylogowania jednokrotnego protokołu SAML ze wszystkich aplikacji, do których się zalogowano
    - Obsługa wzorca WS-Trust ActAs
    - Rozpoznawanie artefaktów języka SAML 
    - Weryfikacja podpisu podpisanych żądań języka SAML (pamiętaj, że podpisane żądania są akceptowane, ale podpis nie jest weryfikowany)
 - Możliwości tokenów 
     - Szyfrowanie tokenu języka SAML 
     - Tokeny SAML w wersji 1.1 w odpowiedziach protokołu SAML 
- Możliwości oświadczeń w tokenie
    - Wystawianie nazw grup lokalnych jako oświadczeń
    - Oświadczenia z magazynów innych niż usługa Azure AD
    - Złożone reguły przekształcania wystawiania oświadczeń (zobacz ten [dokument](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) i ten [dokument](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization) w celu uzyskania informacji o mapowaniach obsługiwanych oświadczeń)
    - Wystawianie rozszerzeń katalogów jako oświadczeń
    - Niestandardowa specyfikacja formatu NameID
    - Wystawianie atrybutów o wielu wartościach

>[!NOTE]
>Usługa Azure AD stale się zmienia, co pozwala na dodawanie kolejnych możliwości w tym obszarze. Ten dokument jest aktualizowany na bieżąco. 

### <a name="configuring-azure-ad"></a>Konfigurowanie usługi Azure AD    
#### <a name="configure-single-sign-on-sso-settings-for-the-saas-app"></a>Konfigurowanie ustawień logowania jednokrotnego dla aplikacji SaaS

W usłudze Azure AD konfigurowanie logowania języka SAML zgodnie z wymaganiami aplikacji odbywa się we właściwościach logowania jednokrotnego aplikacji w obszarze atrybutów użytkownika w następujący sposób:

![](media/migrate-adfs-apps-to-azure/migrate3.png)
- Kliknij pozycję „Wyświetl i edytuj wszystkie inne atrybuty użytkownika”, aby zobaczyć atrybuty do wysłania jako oświadczenia w tokenie zabezpieczającym

![](media/migrate-adfs-apps-to-azure/migrate4.png)
- Kliknij wiersz wybranego atrybutu, aby go edytować, lub kliknij pozycję „Dodaj atrybut”, aby dodać nowy atrybut. 
![](media/migrate-adfs-apps-to-azure/migrate5.png)

#### <a name="assign-users-to-the-app"></a>Przypisywanie użytkowników do aplikacji
Aby użytkownicy w usłudze Azure AD mogli logować się do danej aplikacji SaaS, muszą mieć prawa dostępu przydzielone z poziomu usługi Azure AD.

Aby przypisać użytkowników w portalu usługi Azure AD, przejdź do ekranu aplikacji SaaS w portalu, a następnie kliknij pozycję „Użytkownicy i grupy” na pasku bocznym. Aby dodać użytkownika lub grupę, kliknij pozycję „Dodaj użytkownika” w górnej części ekranu. 

![](media/migrate-adfs-apps-to-azure/migrate6.png) 

![](media/migrate-adfs-apps-to-azure/migrate7.png) Aby zweryfikować dostęp, użytkownik powinien podczas logowania widzieć daną aplikację SaaS w swoim [panelu dostępu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) pod adresem http://myapps.microsoft.com. Na przykład użytkownikowi pomyślnie przypisano prawa dostępu do aplikacji Salesforce i usługi ServiceNow.

![](media/migrate-adfs-apps-to-azure/migrate8.png)

### <a name="configuring-the-saas-app"></a>Konfigurowanie aplikacji SaaS
Proces migracji jednorazowej z federacji lokalnej do usługi Azure AD zależy od tego, czy aplikacja SaaS, z którą pracujesz, obsługuje wielu dostawców tożsamości.  Poniżej przedstawiono niektóre typowe pytania dotyczące obsługi wielu dostawców tożsamości:

   **Pyt.: Co to znaczy, że aplikacja obsługuje wielu dostawców tożsamości?**
    
   Odp.: Aplikacje SaaS, które obsługują wielu dostawców tożsamości, umożliwiają wprowadzanie wszystkich informacji o nowym dostawcy tożsamości (w naszym przypadku o usłudze Azure AD) przed zatwierdzeniem zmian środowiska logowania.  Po zakończeniu konfigurowania można przełączać konfigurację uwierzytelniania aplikacji tak, aby wskazywała usługę Azure AD.

   Pyt.: Dlaczego ważne jest, czy aplikacja obsługuje wielu dostawców tożsamości?

   Odp.: Jeśli opcja wielu dostawców tożsamości nie jest obsługiwana, administrator będzie musiał zarezerwować krótki przedział czasu jako okres niedostępności usługi lub niedostępności z powodu konserwacji. W tym okresie usługa Azure AD zostanie skonfigurowana jako nowy dostawca tożsamości aplikacji. Podczas tej niedostępności należy powiadomić użytkowników, że nie będą mogli logować się do swoich kont.

   Jeśli aplikacja obsługuje wielu dostawców tożsamości, konfigurację dodatkowego dostawcy tożsamości można przeprowadzić z wyprzedzeniem, aby administrator mógł po prostu przełączyć dostawcę tożsamości podczas przełączania platformy Azure.

   Ponadto jeśli aplikacja obsługuje wielu dostawców tożsamości i można wybrać wielu dostawców tożsamości, którzy będą jednocześnie obsługiwać uwierzytelnianie podczas logowania, użytkownik może wskazać dostawcę tożsamości do uwierzytelnienia na swojej stronie logowania.

#### <a name="example-multiple-idp-support"></a>Przykład: obsługa wielu dostawców tożsamości
Na przykład w usłudze Salesforce konfiguracja dostawcy tożsamości znajduje się w obszarze Ustawienia -> Ustawienia firmowe -> Moja domena -> Konfiguracja uwierzytelniania.

![](media/migrate-adfs-apps-to-azure/migrate9.png)

Ze względu na konfigurację utworzoną wcześniej w obszarze Tożsamość -> Ustawienia logowania jednokrotnego powinna być możliwa zmiana dostawcy tożsamości dla konfiguracji uwierzytelniania z usług AD FS na usługę Azure AD. 

![](media/migrate-adfs-apps-to-azure/migrate10.png)

### <a name="optional-configure-user-provisioning-in-azure-ad"></a>Opcjonalnie: konfigurowanie aprowizowania użytkowników w usłudze Azure AD
Jeśli opcjonalnie chcesz, aby usługa Azure AD bezpośrednio obsługiwała aprowizowanie użytkowników dla danej aplikacji SaaS, zobacz ten dokument dotyczący zarządzania aprowizowaniem kont użytkowników dla aplikacji przedsiębiorstwa w usłudze Azure AD.

## <a name="next-steps"></a>Następne kroki

- [Managing Applications with Azure Active Directory (Zarządzanie aplikacjami za pomocą usługi Azure Active Directory)](active-directory-enable-sso-scenario.md)
- [Zarządzanie dostępem do aplikacji](active-directory-managing-access-to-apps.md)
- [Azure AD Connect Federation (Program Azure AD Connect i federacja)](active-directory-aadconnectfed-whatis.md)