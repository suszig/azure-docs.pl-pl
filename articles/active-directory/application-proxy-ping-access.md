---
title: "Uwierzytelnianie oparte na nagłówka z PingAccess dla serwera Proxy aplikacji usługi Azure AD | Dokumentacja firmy Microsoft"
description: "Publikowania aplikacji za pomocą PingAccess i serwera Proxy aplikacji do obsługi uwierzytelniania opartego na nagłówka."
services: active-directory
documentationcenter: 
author: kgremban
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7c2e56a5f747aa2a37fc4bed0e3f3877b64f2be2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a>Nagłówek uwierzytelniania dla logowania jednokrotnego z serwera Proxy aplikacji i PingAccess

Azure Active Directory serwera Proxy aplikacji i PingAccess ma współpracę ze sobą aby zapewnić klientom usługi Azure Active Directory dostępu do nawet więcej aplikacji. Rozwija PingAccess [istniejący serwer Proxy aplikacji ofert](active-directory-application-proxy-get-started.md) uwzględnienie dostępu pojedynczego logowania do aplikacji, które nagłówków jest używany do uwierzytelniania.

## <a name="what-is-pingaccess-for-azure-ad"></a>Co to jest PingAccess dla usługi Azure AD?

PingAccess dla usługi Azure Active Directory jest oferty PingAccess umożliwiającą można umożliwić użytkownikom dostępu i logowanie jednokrotne do aplikacji, które używają nagłówki uwierzytelniania. Serwer Proxy aplikacji traktuje te aplikacje, takie jak każdy inny, za pomocą usługi Azure AD w celu uwierzytelniania dostępu i następnie przekazywanie ruchu przez usługę łącznika. PingAccess znajduje się przed aplikacje i tłumaczy token dostępu z usługi Azure AD na nagłówka, tak aby po otrzymaniu uwierzytelnianie w formacie, który można odczytać.

Użytkownicy nie będą Zwróć uwagę, inne po zalogowaniu się do użycia w aplikacjach firmowych. Wciąż pracować z dowolnego miejsca na dowolnym urządzeniu. 

Ponieważ łączniki serwera Proxy aplikacji kierować zdalnego ruch do wszystkich aplikacji niezależnie od ich typ uwierzytelniania, nadal równoważenia obciążenia, automatycznie, jak również.

## <a name="how-do-i-get-access"></a>Jak uzyskać dostęp?

Ponieważ ten scenariusz jest dostępna za pośrednictwem usługi Azure Active Directory i PingAccess, potrzebnych licencji dla obu usług. Jednak subskrypcje usługi Azure Active Directory Premium obejmują podstawowe licencji PingAccess, która obejmuje aplikacje do 20. Jeśli potrzebujesz do publikowania aplikacji opartych na nagłówkach więcej niż 20, możesz kupić dodatkowych licencji z PingAccess. 

Aby uzyskać więcej informacji, zobacz [Wersje usługi Azure Active Directory](active-directory-editions.md).

## <a name="publish-your-application-in-azure"></a>Publikowanie aplikacji na platformie Azure

Ten artykuł jest przeznaczony dla osób, które publikują aplikacji w tym scenariuszu po raz pierwszy. Instruktaże jak rozpocząć pracę z aplikacji i PingAccess, oprócz kroki publikowania. Jeśli skonfigurowano już obie usługi, ale odświeżacz kroki publikowania, możesz pominąć instalacja łącznika i przenieść na [Dodaj aplikację do usługi Azure AD przy użyciu serwera Proxy aplikacji](#add-your-app-to-Azure-AD-with-Application-Proxy).

>[!NOTE]
>Ponieważ ten scenariusz jest powiązanie między usługą Azure AD i PingAccess, niektóre instrukcji istnieje w witrynie tożsamości Ping.

### <a name="install-an-application-proxy-connector"></a>Instalowanie łącznika serwera Proxy aplikacji

Jeśli już ma włączony serwer Proxy aplikacji, a ma zainstalowany łącznik, możesz pominąć tę sekcję i przenieść na [Dodaj aplikację do usługi Azure AD przy użyciu serwera Proxy aplikacji](#add-your-app-to-azure-ad-with-application-proxy).

Łącznik serwera Proxy aplikacji jest usługi systemu Windows Server, który kieruje ruch z zdalnego pracowników do opublikowanej aplikacji. Aby uzyskać szczegółowe instrukcje dotyczące instalacji, zobacz [Włączanie serwera Proxy aplikacji w portalu Azure](active-directory-application-proxy-enable.md).

1. Zaloguj się do [portalu Azure](https://portal.azure.com) jako administrator globalny.
2. Wybierz **usługi Azure Active Directory** > **serwera proxy aplikacji**.
3. Wybierz **Pobierz łącznik** aby rozpocząć pobieranie łącznika serwera Proxy aplikacji. Postępuj zgodnie z instrukcjami instalacji.

   ![Włącz serwer Proxy aplikacji i Pobierz łącznik](./media/application-proxy-ping-access/install-connector.png)

4. Pobieranie łącznika automatycznie należy włączyć serwer Proxy aplikacji dla katalogu, ale jeśli nie możesz wybrać **Włączanie serwera Proxy aplikacji**.


### <a name="add-your-app-to-azure-ad-with-application-proxy"></a>Dodaj aplikację do usługi Azure AD przy użyciu serwera Proxy aplikacji

Istnieją dwie akcje, które należy wykonać w portalu Azure. Najpierw należy opublikować aplikację przy użyciu serwera Proxy aplikacji. Następnie które należy zebrać pewne informacje o aplikacji, którą można użyć podczas wykonywania czynności PingAccess.

Wykonaj następujące kroki, aby opublikować aplikację. Bardziej szczegółowe wskazówki kroki 1 – 8, zobacz [publikowanie aplikacji przy użyciu serwera Proxy aplikacji usługi Azure AD](application-proxy-publish-azure-portal.md).

1. Jeśli nie w ostatniej sekcji, zaloguj się do [portalu Azure](https://portal.azure.com) jako administrator globalny.
2. Wybierz **usługi Azure Active Directory** > **aplikacje dla przedsiębiorstw**.
3. Wybierz **Dodaj** w górnej części bloku.
4. Wybierz **lokalnej aplikacji**.
5. Wypełnij wymagane pola informacje o nowej aplikacji. Użyj poniższych wskazówek dla ustawienia:
   - **Wewnętrzny adres URL**: zwykle Podaj adres URL, który przyjmuje stronę logowania aplikacji podczas pracy w sieci firmowej. W tym scenariuszu łącznik musi traktować PingAccess proxy jako pierwsza strona aplikacji. Użyj tego formatu: `https://<host name of your PA server>:<port>`. Numer portu to 3000 domyślnie, ale można go skonfigurować w PingAccess.
   - **Metoda wstępnego uwierzytelnienia**: Azure Active Directory
   - **Tłumaczenie adresów URL w nagłówkach**: nie

   >[!NOTE]
   >Jeśli swoją pierwszą aplikację, aby uruchomić i wrócić i zaktualizować to ustawienie, jeśli zmienisz konfigurację PingAccess za pomocą port 3000. Jeśli jest to drugi lub nowszym aplikacji, będzie to musi być zgodny odbiornika skonfigurowaniu PingAccess. Dowiedz się więcej o [odbiorników w PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).

6. Wybierz **Dodaj** w dolnej części bloku. Aplikacja zostanie dodany, a następnie otwiera menu szybki start.
7. Wybierz z menu szybki start **przypisać użytkownika do testowania**, i Dodaj przynajmniej jednego użytkownika do aplikacji. Upewnij się, że to konto testu ma dostęp do aplikacji lokalnych.
8. Wybierz **przypisać** można zapisać przypisanie użytkownika testu.
9. W bloku zarządzania aplikacjami, wybierz **logowanie jednokrotne**.
10. Wybierz **na podstawie nagłówka logowania jednokrotnego** z menu rozwijanego. Wybierz pozycję **Zapisz**.

   >[!TIP]
   >Jeśli jest to pierwsza przy użyciu opartych na nagłówkach logowania jednokrotnego, musisz zainstalować PingAccess. Aby upewnić się, że subskrypcji platformy Azure jest automatycznie kojarzony z instalacją PingAccess, skorzystaj z łącza po tej jednej strony logowania do pobrania PingAccess. Teraz Otwórz witrynę pobierania lub powracanie do tej strony. 

   ![Wybierz na podstawie nagłówka logowania jednokrotnego](./media/application-proxy-ping-access/sso-header.PNG)

11. Zamknij bloku aplikacji przedsiębiorstwa lub przewiń do lewej strony, aby wrócić do menu usługi Azure Active Directory.
12. Wybierz **rejestracji aplikacji**.

   ![Wybierz rejestracji aplikacji](./media/application-proxy-ping-access/app-registrations.png)

13. Wybierz aplikację, dodaną, następnie **adresy URL odpowiedzi**.

   ![Wybierz adresy URL odpowiedzi](./media/application-proxy-ping-access/reply-urls.png)

14. Sprawdź, czy zewnętrznego adresu URL, który zostanie przypisany do aplikacji w kroku 5 znajduje się na liście adresów URL odpowiedzi. Jeśli nie, dodaj go.
15. W bloku ustawień aplikacji, wybierz **wymagane uprawnienia**.

  ![Wybierz wymagane uprawnienia](./media/application-proxy-ping-access/required-permissions.png)

16. Wybierz pozycję **Dodaj**. W interfejsie API, wybierz opcję **Windows Azure Active Directory**, następnie **wybierz**. Uprawnienia, wybierz **odczytu i zapisu wszystkie aplikacje** i **Zaloguj się i odczytuj profil użytkownika**, następnie **wybierz** i **gotowe**.  

  ![Wybierz uprawnienia](./media/application-proxy-ping-access/select-permissions.png)

17. Przyznaj uprawnienia przed zamknięciem ekranu uprawnienia. 
![Udzielanie uprawnień](media/application-proxy-ping-access/grantperms.png)

### <a name="collect-information-for-the-pingaccess-steps"></a>Zbieranie informacji o krokach PingAccess

1. W bloku ustawień aplikacji, wybierz **właściwości**. 

  ![Wybierz polecenie Właściwości](./media/application-proxy-ping-access/properties.png)

2. Zapisz **identyfikator aplikacji** wartość. Służy to do tego Identyfikatora klienta po skonfigurowaniu PingAccess.
3. W bloku ustawień aplikacji, wybierz **klucze**.

  ![Wybierz klucze](./media/application-proxy-ping-access/Keys.png)

4. Utwórz klucz, wprowadzając opis klucza i wybierając z menu rozwijanego datę wygaśnięcia.
5. Wybierz pozycję **Zapisz**. Identyfikator GUID jest wyświetlana w **wartość** pola.

  Zapisz tę wartość teraz, ponieważ nie można go ponownie wyświetlić po zamknięciu tego okna.

  ![Utwórz nowy klucz](./media/application-proxy-ping-access/create-keys.png)

6. Zamknij bloku rejestracji aplikacji lub przewiń do lewej strony, aby wrócić do menu usługi Azure Active Directory.
7. Wybierz **właściwości**.
8. Zapisz **identyfikator katalogu** identyfikatora GUID.

### <a name="optional---update-graphapi-to-send-custom-fields"></a>Opcjonalne - GraphAPI aktualizacji do wysyłania niestandardowych pól

Aby uzyskać listę tokeny zabezpieczające, które wysyła usługi Azure AD do uwierzytelniania, zobacz [odwołania do tokenu usługi Azure AD](./develop/active-directory-token-and-claims.md). Jeśli potrzebujesz niestandardowych oświadczenie, które wysyła innych tokenów, użyj GraphAPI, aby ustawić pole aplikacji *acceptMappedClaims* do **True**. Azure AD Graph Explorer umożliwia tylko ta konfiguracja. 

W tym przykładzie użyto wykres Explorer:

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```

>[!NOTE]
>Aby użyć niestandardowej oświadczenia, musi mieć również zasady niestandardowe definiowane i przypisywane do aplikacji.  Ta zasada powinna zawierać wszystkie wymagane atrybuty niestandardowe.
>
>Definicja zasad i przypisania mogą być przeprowadzane za pomocą programu PowerShell, Azure AD Graph Explorer lub MS Graph.  Jeśli robią to w programie PowerShell, należy najpierw użyć `New-AzureADPolicy `i przypisz je do aplikacji z `Set-AzureADServicePrincipalPolicy`.  Aby uzyskać więcej informacji, zobacz [dokumentacji usługi Azure AD zasad](active-directory-claims-mapping.md#claims-mapping-policy-assignment).

### <a name="optional---use-a-custom-claim"></a>Opcjonalne - Użyj oświadczenia niestandardowego
Aby aplikacja oświadczenia niestandardowego i zawiera dodatkowe pola, upewnij się, czy masz również [utworzony oświadczenia niestandardowe mapowanie zasad i przypisywać do aplikacji](active-directory-claims-mapping.md#claims-mapping-policy-assignment).

## <a name="download-pingaccess-and-configure-your-app"></a>Pobierz PingAccess i konfigurowanie aplikacji

Teraz, zostały ukończone wszystkie kroki konfiguracji usługi Azure Active Directory, możesz przejść do konfigurowania PingAccess. 

Szczegółowe informacje na temat PingAccess częścią tego scenariusza kontynuować w dokumentacji polecenia Ping tożsamości [PingAccess konfigurowanie dla usługi Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).

Tych krokach objaśniono proces pobierania konta PingAccess, jeśli nie masz już konto, instalowany serwer PingAccess i tworzenie połączenia dostawcy usługi Azure AD OIDC o identyfikatorze katalogu, który został skopiowany z portalu Azure. Następnie należy użyć wartości Identyfikatora aplikacji i klucza do utworzenia sesji sieci Web na PingAccess. Po wykonaniu tej można Konfigurowanie mapowania tożsamości i tworzenia hostów wirtualnych, witryn i aplikacji.

### <a name="test-your-app"></a>Testowanie aplikacji

Po zakończeniu wszystkich tych kroków aplikacji powinna być uruchomiona. Aby ją przetestować, otwórz przeglądarkę i przejdź do zewnętrzny adres URL utworzony po opublikowaniu aplikacji na platformie Azure. Zaloguj się przy użyciu konta testowego przypisany do aplikacji.

## <a name="next-steps"></a>Następne kroki

- [Skonfiguruj PingAccess dla usługi Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [W jaki sposób serwera Proxy aplikacji usługi Azure AD zapewnia rejestrację jednokrotną](application-proxy-sso-overview.md)
- [Rozwiązywanie problemów z serwera Proxy aplikacji](active-directory-application-proxy-troubleshoot.md)
