---
title: "Omówienie tożsamości dla stosu Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat systemów tożsamości, używanych w usłudze Azure stosu."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/22/2018
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 609ea3300806f3cfed4a3285a096f59be491c684
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/24/2018
---
# <a name="overview-of-identity-for-azure-stack"></a>Omówienie tożsamości Azure stosu

Stos Azure wymaga usługi Azure AD lub usługi federacyjne w usłudze Active Directory (AD FS) kopii przez Active Directory (AD) jako dostawcy tożsamości. Następujące pojęcia i szczegóły autoryzacji może pomóc dokonać wyboru między dostawców tożsamości. Wybór dostawcy jest jednorazowe decyzji, wprowadzone podczas najpierw wdrożyć stosu Azure.  

Wybór między Azure AD i usług AD FS może być ograniczone przez tryb, w którym wdrożeniem Azure stosu: 
- Podczas wdrażania w trybie połączenia, można użyć usługi Azure AD lub AD FS. 
- Podczas wdrażania w trybie rozłączonym bez połączenia z Internetem jest obsługiwana tylko usługi AD FS.

Aby uzyskać więcej informacji o tych opcjach wyświetlić następujące artykuły, w zależności od środowiska Azure stosu:
- Azure stosu Deployment Kit, zobacz [zagadnienia dotyczące tożsamości](azure-stack-datacenter-integration.md#identity-considerations).
- Azure stosu zintegrowanych systemów, zobacz [decyzje dotyczące stosu Azure planowania wdrożenia zintegrowane systemy](azure-stack-deployment-decisions.md).

 
## <a name="common-concepts-for-identity"></a>Wspólne pojęcia dotyczące tożsamości
W poniższych sekcjach omówiono typowe pojęcia dla dostawców tożsamości i ich użycia w stosie Azure.
![Terminologia dostawców tożsamości](media/azure-stack-identity-overview/terminology.png)


### <a name="directories-tenants-and-organizations"></a>Katalogi dzierżawcy i organizacji
Katalog jest kontenerem, który przechowuje informacje o *użytkowników*, *aplikacji*, *grup*, i *usługi podmiotów*.
 
Jest dostępna dzierżawa katalogu *organizacji*, takie jak Microsoft lub firmy użytkownika. 
- Usługi Azure AD obsługuje wielu dzierżawców i może obsługiwać wiele organizacji, w każdej z nich w ich własnego katalogu. Jeśli używasz usługi Azure AD i mieć wielu dzierżawców, można przyznać aplikacji i użytkowników z jednego dzierżawcy dostęp do innych dzierżawców ten sam katalog.
- Usługi AD FS obsługuje pojedynczej dzierżawy i w związku z tym pojedynczej organizacji. 

### <a name="users-and-groups"></a>Użytkownicy i grupy
Konta użytkowników (tożsamości) są standardowe konta, które przeprowadzają uwierzytelnianie przy użyciu nazwy użytkownika i hasła użytkowników indywidualnych. Grupy mogą zawierać użytkowników lub inne grupy. 

Jak tworzyć i zarządzać użytkownikami i grupami zależy od rozwiązania tożsamości, których używasz. 

W stosie Azure, kont użytkowników: 
- Są tworzone w  *username@domain*  format. Mimo że usługi AD FS mapuje kont użytkowników do usługi Active Directory, usługi AD FS nie obsługuje korzystanie z  _&lt;domeny >\<alias >_ format. 
- Można skonfigurować do używania uwierzytelniania wieloskładnikowego. 
- Są ograniczone do katalogu, gdzie są najpierw zarejestrować, jest ich katalogu organizacji.
- Mogą zostać zaimportowane z katalogów lokalnych. Aby uzyskać więcej informacji, zobacz [integrację katalogów lokalnych z usługą Azure Active Directory](/azure/active-directory/connect/active-directory-aadconnect) w dokumentacji platformy Azure.  

Po zalogowaniu się do portalu dzierżawcy Twojej organizacji, możesz użyć https://portal.local.azurestack.external. 

### <a name="guest-users"></a>Użytkownicy-goście
Goście są konta użytkowników od pozostałych dzierżawców katalogu, którym przyznano dostęp do zasobów w katalogu. Aby obsługiwać użytkownika gościa, należy użyć usługi Azure AD i włączyć obsługę wielu dzierżawców. Jeśli są obsługiwane, możesz poprosić użytkownika gościa dostęp do zasobów w dzierżawie katalogu, który umożliwia współpracę z zewnątrz organizacji. 

Aby zaprosić gości, Operatorzy chmur i użytkownicy mogą użyć [współpracy B2B usługi Azure AD](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Zaproszeni użytkownicy otrzyma dostęp do dokumentów, zasobów i aplikacji z katalogu podczas zachować kontrolę nad własnych zasobów i danych. 

Jako gość możesz zalogować się do innej dzierżawy katalogu organizacji. Aby to zrobić, można dołączyć tę nazwę katalogu organizacji do portalu adresu URL.  Na przykład jeśli należą do domeny contoso.com, ale mają być rejestrowane w katalogu firmy Fabrikam, użyj https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.

### <a name="applications"></a>Aplikacje
Rejestrowanie aplikacji do usługi Azure AD lub AD FS i oferowanie ich dla użytkowników w organizacji. 

Aplikacje obejmują:
- **Aplikacja sieci Web** — przykładami portalu Azure i usługi Azure Resource Manager.  Obsługuje wywołania interfejsu API sieci Web. 
- **Aplikacja Native client** — przykładów programu Azure PowerShell, Visual Studio i Azure interfejsu wiersza polecenia (CLI).

Aplikacje mogą obsługiwać dwa rodzaje dzierżawy: 
- **Pojedynczej dzierżawy** aplikacje obsługują użytkowników i usług, tylko z tym samym katalogu, w którym aplikacja jest zarejestrowany. 

  > [!NOTE]    
  > Ponieważ usługi AD FS obsługuje tylko jeden katalog, aplikacje, które możesz utworzyć w topologii usług AD FS są zgodnie z projektem aplikacji pojedynczej dzierżawy.
- **Wielodostępne** używana obsługa aplikacji przez użytkowników i usług z katalogu rejestracji aplikacji oraz dodatkowe dzierżawy katalogów.  Z aplikacje wielodostępne innej dzierżawy katalogu (innej dzierżawy usługi Azure AD) mogą logowania użytkownika do aplikacji.     

  Aby uzyskać więcej informacji na temat obsługi wielu dzierżawców, zobacz [włączenia obsługi wielu dzierżawców](azure-stack-enable-multitenancy.md).  

  Aby uzyskać więcej informacji o tworzeniu aplikacji wielodostępnych, zobacz [aplikacje wielodostępne](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview).

Podczas rejestrowania aplikacji są tworzone dwa obiekty:
- **Obiekt aplikacji** — obiekt application jest globalne reprezentację aplikacji we wszystkich dzierżawców. Ta relacja jest jeden do jednego z aplikacją oprogramowania i istnieje tylko w katalogu, w którym aplikacja jest pierwszym rejestrów.

 
   
- **Obiekt główny usługi** — nazwy głównej usługi jest poświadczeniami, które jest tworzone dla aplikacji w katalogu najpierw rejestracji aplikacji. Nazwy głównej usługi również jest tworzony w katalogu każdej dodatkowe dzierżawy, w którym aplikacja jest używana. Ta relacja może być jeden do wielu z aplikacją oprogramowania.   

Aby dowiedzieć się więcej na temat aplikacji i obiekty główne usługi, zobacz [aplikacji i usług obiektów principal w usłudze Azure Active Directory (Azure AD)](/azure/active-directory/develop/active-directory-application-objects). 

### <a name="service-principals"></a>Nazwy główne usług 
Nazwy głównej usługi jest zestawem *poświadczeń* dla aplikacji lub usługi, która będzie udzielać dostępu do zasobów w stosie Azure. Użyj nazwy głównej usługi oddziela uprawnienia aplikacji uprawnień użytkownika, który korzysta z aplikacji.

Nazwy głównej usługi jest tworzony w każdej dzierżawy, gdy aplikacja jest używana. Nazwy głównej usługi ustanawia tożsamości dla logowania i dostępu do zasobów (takich jak użytkowników), które są zabezpieczone przez tę dzierżawę.   

- Stosowanie pojedynczej dzierżawy ma tylko jedną nazwę główną usługi, w katalogu, w którym najpierw został utworzony.  Tej nazwy głównej usługi zostanie utworzona i zgadza się do użycia podczas rejestracji aplikacji. 
- Wielodostępnych aplikacji sieci Web lub interfejsu API ma nazwę główną usługi utworzone w każdej dzierżawy, gdy użytkownik z tej dzierżawy wyraża zgodę na korzystanie z aplikacji.  

Poświadczenia dla nazwy główne usług może być kluczem (wygenerowane za pośrednictwem portalu Azure) lub certyfikatu.  Użyj certyfikatu jest przeznaczone do automatyzacji, ponieważ przyjęto, że certyfikaty są bezpieczniejsze niż użycie kluczy. 


> [!NOTE]    
> Gdy używasz usług AD FS z stosu Azure tylko administrator może utworzyć jednostki usługi. Z usługami AD FS nazw głównych usług wymagają certyfikatów i są tworzone za pomocą uprzywilejowanych punktu końcowego (program ten). Aby uzyskać więcej informacji zobacz, [dostęp aplikacji do stosu Azure](azure-stack-create-service-principals.md).

Aby poznać nazwy główne usług Azure stosu, zobacz temat [tworzenia nazwy główne usług](azure-stack-create-service-principals.md).


### <a name="services"></a>Usługi
Usługi w stosie Azure współpracujące z dostawcy tożsamości jest zarejestrowana jako aplikacji przy użyciu dostawcy tożsamości. Jak aplikacje rejestracji umożliwia uwierzytelnianie i tożsamość usługi. 

Użyj wszystkich usług Azure [OpenID Connect](/azure/active-directory/develop/active-directory-protocols-openid-connect-code) protokołów i [tokenów sieci Web JSON](/azure/active-directory/develop/active-directory-token-and-claims) (JWT), aby ustalić jego tożsamość. Ze względu na spójne używanie protokołów przez usługę Azure AD i usług AD FS, można użyć Azure [biblioteki uwierzytelniania usługi Active Directory](/azure/active-directory/develop/active-directory-authentication-libraries) (ADAL) do uwierzytelniania lokalnego lub na platformie Azure (w przypadku połączonych). ADAL także pozwala na użycie narzędzi, takich jak Azure PowerShell i interfejsu wiersza polecenia dla chmury między i zarządzania zasobami lokalnymi. 


### <a name="identities-and-your-identity-system"></a>Tożsamości i systemu tożsamości 
Tożsamości dla stosu Azure zawierają konta użytkowników, grup i nazwy główne usług. Po zainstalowaniu stosu Azure kilka wbudowanych aplikacji i usług automatycznego rejestrowania w usłudze dostawcy tożsamości w dzierżawie katalogu. Niektóre usługi, które rejestrują służą do administrowania. Inne usługi są dostępne dla użytkowników. Rejestracje domyślne Przypisz podstawowej tożsamości usługi, które mogą współdziałać ze sobą i z tożsamościami później zostaną dodane.
Jeśli konfigurujesz usługę Azure AD z wielu dzierżawców niektóre aplikacje są propagowane do nowych katalogów.  

## <a name="authentication-and-authorization"></a>Uwierzytelnianie i autoryzacja
 

### <a name="authentication-by-applications-and-users"></a>Uwierzytelnianie w aplikacji i użytkowników
  
![Tożsamości między warstwami Azure stosu](media/azure-stack-identity-overview/identity-layers.png)

Dla aplikacji i użytkowników architektura stosu Azure opisano cztery warstwy. Interakcje między każdym z tych warstw można użyć różnych typów uwierzytelniania.


|Warstwy    |Uwierzytelnianie między warstwami  |
|---------|---------|
|Narzędzia i klientów, takich jak portal administratora     | Dostępu lub modyfikacji zasobu w stosie Azure, narzędzia i klienci używają [tokenów sieci Web JSON](/azure/active-directory/develop/active-directory-token-and-claims) (JWT), aby nawiązać połączenie z usługi Azure Resource Manager.  <br><br> Usługa Azure Resource Manager sprawdza poprawność tokenu JWT i dokonuje wglądu *oświadczeń* w wystawionego tokenu, aby oszacować poziom autoryzacji tego użytkownika lub nazwy głównej usługi ma w stosie Azure.        |
|Usługa Azure Resource Manager i jego core services     |Usługa Azure Resource Manager komunikuje się z dostawcami zasobów na przesyłanie komunikacji z użytkownikami. <br><br> Transfery użyj *bezpośredniego imperatywne* wywołania lub *deklaratywne* wywołuje za pośrednictwem [szablonów usługi Azure Resource Manager](/azure/azure-stack/user/azure-stack-arm-templates.md).|
|Dostawcy zasobów     |Wywołania przekazywane do dostawcy zasobów są zabezpieczone przy użyciu uwierzytelniania opartego na certyfikatach. <br><br>Usługa Azure Resource Manager i dostawca zasobów pozostają w komunikacji przy użyciu interfejsu API. Dla każdego wywołania otrzymane z Menedżera zasobów Azure dostawcy zasobów weryfikuje połączenia z tym certyfikatem.|
|Infrastruktura i logika biznesowa     |Dostawcy zasobów komunikują się z logiki biznesowej i infrastruktury przy użyciu wybranych przez nich trybu uwierzytelniania. Domyślnych zasobów dostawców dostarczanych ze stosu Azure uwierzytelniania systemu Windows do zabezpieczania komunikacji.|

![Informacje potrzebne do uwierzytelniania](media/azure-stack-identity-overview/authentication.png)


### <a name="authenticate-to-azure-resource-manager"></a>Uwierzytelniania do usługi Azure Resource Manager
Do uwierzytelniania za pomocą dostawcy tożsamości i odbierać token JWT, musi mieć następujące informacje: 
1.  **Adres URL dla systemu tożsamości (urzędu)** — adres URL, w którym można osiągnąć dostawcy tożsamości. Na przykład  *&lt;https://login.windows.net>*. 
2.  **Identyfikator URI aplikacji usługi Azure Resource Manager** — Unikatowy identyfikator menedżera zasobów Azure zarejestrowane przy pomocy dostawcy usługi tożsamości i unikatowa dla każdej instalacji stosu Azure.
3.  **Poświadczenia** — tym poświadczeń jest używany do uwierzytelniania z dostawcy tożsamości.  
4.  **Adres URL dla usługi Azure Resource Manager** — adres URL jest lokalizacja usługi Azure Resource Manager. Na przykład  *&lt;https://management.azure.com>* lub  *&lt;https://management.local.azurestack.external>*.

Gdy podmiot zabezpieczeń (klienta, aplikacji lub użytkownika) wysyła żądanie uwierzytelnienia do uzyskania dostępu do zasobu, to żądanie musi zawierać:
- Poświadczenia podmiotu.
- Identyfikator URI aplikacji zasobu chcą uzyskać dostęp.  

Poświadczenia są weryfikowane przez dostawcę tożsamości. Dostawca tożsamości jest również sprawdzane, czy identyfikator URI aplikacji jest dla zarejestrowanej aplikacji i że podmiot zabezpieczeń ma poprawne uprawnienia do pobrania tokenu dla tego zasobu. Jeśli żądanie jest prawidłowe, zostanie przyznany żetonu Web JSON. 

Następnie token musi przejść w nagłówku żądania do usługi Azure Resource Manager. Usługi Azure Resource Manager wykonuje następujące operacje sprawdzania poprawności bez określonej kolejności:
- Sprawdź poprawność *wystawcy* oświadczeń (iss) potwierdzić, czy token jest przez dostawcę tożsamości poprawne. 
- Sprawdź poprawność *odbiorców* oświadczeń (lub), aby potwierdzić, czy token został wystawiony do usługi Azure Resource Manager. 
- Zweryfikuj, że tokenu JWT jest podpisany przy użyciu certyfikatu skonfigurowanym za pośrednictwem protokołu OpenID, która jest znana usłudze Azure Resource Manager. 
- Przegląd *wystawiony w* (iat) i *wygaśnięcia* (exp) oświadczeń potwierdzić, czy token jest aktywny i mogą być akceptowane. 

Po ukończeniu wszystkich operacji sprawdzania poprawności używa usługi Azure Resource Manager *zgłoszą* (oid) i *grup* oświadczeń do listy zasobów, które mogą uzyskiwać dostęp do podmiotu zabezpieczeń. 

![Diagram protokół wymiany tokenu](media/azure-stack-identity-overview/token-exchange.png)


### <a name="use-role-based-access-control"></a>Za pomocą kontroli dostępu opartej na rolach  
Oparta na rolach kontroli dostępu (RBAC) w stosie Azure jest zgodna z implementacją w systemie Microsoft Azure.  Dostęp do zasobów można zarządzać, przypisując odpowiednie role RBAC do użytkowników, grup i aplikacji. Aby uzyskać informacje o sposobie używania RBAC stosu Azure następujące artykuły:
- [Rozpoczynanie pracy z opartej na rolach kontroli dostępu w portalu Azure](/azure/active-directory/role-based-access-control-what-is).
- [Zarządzanie dostępem do zasobów subskrypcji platformy Azure za pomocą opartej na rolach kontroli dostępu](/azure/active-directory/role-based-access-control-configure).
- [Tworzenie niestandardowych ról dla kontroli dostępu](/azure/active-directory/role-based-access-control-custom-roles).
- [Zarządzanie kontrolą dostępu opartą na rolach](azure-stack-manage-permissions.md) Azure stosu.


### <a name="authenticate-with-azure-powershell"></a>Uwierzytelnianie przy użyciu programu Azure PowerShell  
Informacje dotyczące korzystania z programu Azure PowerShell do uwierzytelniania za pomocą stosu Azure można znaleźć w folderze [konfigurowania środowiska PowerShell użytkownika stosu Azure](azure-stack-powershell-configure-user.md).

### <a name="authenticate-with-azure-cli"></a>Uwierzytelniania za pomocą interfejsu wiersza polecenia platformy Azure
Informacje dotyczące korzystania z programu Azure PowerShell do uwierzytelniania za pomocą stosu Azure można znaleźć w folderze [Instalowanie i Konfigurowanie interfejsu wiersza polecenia do użycia z programem Azure stosu](/azure/azure-stack/user/azure-stack-connect-cli.md).

## <a name="next-steps"></a>Kolejne kroki
- [Architektura tożsamości](azure-stack-identity-architecture.md)   
- [Integracja Datacenter - tożsamości](azure-stack-integrate-identity.md)




