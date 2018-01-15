---
title: "Autoryzowanie konta dewelopera przy użyciu usługi Azure Active Directory — Zarządzanie interfejsami API Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak i autoryzacji użytkowników w usłudze API Management przy użyciu usługi Azure Active Directory."
services: api-management
documentationcenter: API Management
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: 45c8632f4e03c86cf4e32c6d1151977792f32add
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/13/2018
---
> [!WARNING]
> Integracja z usługą Azure Active Directory jest dostępny w [Developer, Standard i Premium](https://azure.microsoft.com/en-us/pricing/details/api-management/) tylko warstwy.

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Sposób autoryzowania konta dewelopera przy użyciu usługi Azure Active Directory w usłudze Azure API Management
## <a name="overview"></a>Przegląd
Ten przewodnik zawiera instrukcje umożliwić dostęp do portalu dla deweloperów dla użytkowników z usługi Azure Active Directory. W tym przewodniku przedstawiono również sposób zarządzać grupami użytkowników usługi Azure Active Directory przez dodanie zewnętrznej grupy, które zawierają użytkowników usługi Azure Active Directory.

> Aby wykonać kroki opisane w tym przewodniku najpierw musi mieć usługi Azure Active Directory, umożliwiający tworzenie aplikacji.
> 

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Sposób autoryzowania konta dewelopera przy użyciu usługi Azure Active Directory
Aby rozpocząć, kliknij przycisk **portal wydawcy** w portalu Azure usługi Zarządzanie interfejsami API. Spowoduje to przejście do portalu wydawcy usługi API Management.

![Publisher portal][api-management-management-console]

> Jeśli jeszcze nie masz utworzonego wystąpienia usługi API Management, zobacz [Tworzenie wystąpienia usługi API Management][Create an API Management service instance] w samouczku [Wprowadzenie do usługi Azure API Management][Get started with Azure API Management].
> 
> 

Kliknij przycisk **zabezpieczeń** z **zarządzanie interfejsami API** menu po lewej stronie i kliknij przycisk **tożsamości zewnętrznych**.

![Tożsamości zewnętrznych][api-management-security-external-identities]

Kliknij przycisk **usługi Azure Active Directory**. Zanotuj **adresem URL przekierowania** i przełączyć się do usługi Azure Active Directory w klasycznym portalu Azure.

![Tożsamości zewnętrznych][api-management-security-aad-new]

Kliknij przycisk **Dodaj** przycisk, aby utworzyć nową aplikację usługi Azure Active Directory, a następnie wybierz pozycję **Dodaj aplikację moją organizację**.

![Dodaj nową aplikację usługi Azure Active Directory][api-management-new-aad-application-menu]

Wprowadź nazwę aplikacji, wybierz pozycję **sieci Web, aplikacji i/lub interfejs API sieci Web**i kliknij przycisk Dalej.

![Nową aplikację usługi Azure Active Directory][api-management-new-aad-application-1]

Aby uzyskać **adres URL logowania**, wprowadź adres URL rejestracji z portalu dla deweloperów. W tym przykładzie **adres URL logowania** jest `https://aad03.portal.current.int-azure-api.net/signin`. 

Aby uzyskać **adres URL Identyfikatora aplikacji**, wprowadź w domenie domyślnej lub niestandardowej domeny dla usługi Azure Active Directory i Dołącz unikatowy ciąg do niego. W tym przykładzie domyślnej domeny **https://contoso5api.onmicrosoft.com** jest używana z sufiksem **/api** określony.

![Nowe właściwości aplikacji usługi Azure Active Directory][api-management-new-aad-application-2]

Kliknij przycisk wyboru, aby zapisać i utworzyć aplikację, a następnie przejdź do **Konfiguruj** kartę, aby skonfigurować nową aplikację.

![Nowej aplikacji usługi Azure Active Directory][api-management-new-aad-app-created]

Jeśli wiele Azure Active Directory mają być używany dla tej aplikacji, kliknij przycisk **tak** dla **aplikacji jest wielodostępne**. Wartość domyślna to **nr**.

![Aplikacja jest wieloma dzierżawcami][api-management-aad-app-multi-tenant]

Kopiuj **adresem URL przekierowania** z **usługi Azure Active Directory** sekcji **tożsamości zewnętrznych** w portalu wydawcy i wklej ją do **odpowiedzi Adres URL** pola tekstowego. 

![Adres URL odpowiedzi][api-management-aad-reply-url]

Przewiń w dół na karcie Konfiguracja, wybierz opcję **uprawnienia aplikacji** listy rozwijanej i sprawdź **odczytuj dane katalogu**.

![Uprawnienia aplikacji][api-management-aad-app-permissions]

Wybierz **delegowanie uprawnień** listy rozwijanej i sprawdź **włączenia logowania jednokrotnego i odczytu profilów użytkowników**.

![Uprawnienia delegowane][api-management-aad-delegated-permissions]

> Aby uzyskać więcej informacji o aplikacji i uprawnień delegowanych, zobacz [podczas uzyskiwania dostępu do interfejsu API programu Graph][Accessing the Graph API].
> 
> 

Kopiuj **identyfikator klienta** do Schowka.

![Identyfikator klienta][api-management-aad-app-client-id]

Przełączyć się do portalu wydawcy i Wklej w **identyfikator klienta** skopiowanych z konfiguracji aplikacji usługi Azure Active Directory.

![Identyfikator klienta][api-management-client-id]

Wrócić do konfiguracji usługi Azure Active Directory, a następnie kliknij przycisk **wybierz czas trwania** listy rozwijanej w **klucze** sekcji i określ interwał. W tym przykładzie **1 rok** jest używany.

![Klucz][api-management-aad-key-before-save]

Kliknij przycisk **zapisać** do zapisania konfiguracji, a następnie wyświetlić klucz. Skopiuj klucz do Schowka.

> Zanotuj tego klucza. Po zamknięciu okna konfiguracji usługi Azure Active Directory, klucz nie może ponownie wyświetlone.
> 
> 

![Klucz][api-management-aad-key-after-save]

Przełączyć się do portalu wydawcy i Wklej klucz do **klucz tajny klienta** pola tekstowego.

![Klucz tajny klienta][api-management-client-secret]

**Dozwolone dzierżaw** Określa, które katalogi mają dostęp do interfejsów API wystąpienia usługi Zarządzanie interfejsami API. Określ domeny wystąpień usługi Azure Active Directory, do których chcesz udzielić dostępu. Wiele domen można oddzielić newlines, spacjami lub przecinkami.

![Dozwolone dzierżawy][api-management-client-allowed-tenants]


Po wymaganą konfiguracją jest określony, kliknij przycisk **zapisać**.

![Zapisz][api-management-client-allowed-tenants-save]

Po zmiany zostaną zapisane, użytkownicy w określonej usługi Azure Active Directory mogą zalogować się do portalu dla deweloperów wykonując kroki opisane w [zalogować się do portalu dla deweloperów przy użyciu konta usługi Azure Active Directory] [ Log in to the Developer portal using an Azure Active Directory account].

Można określić wiele domen w **dozwolone dzierżaw** sekcji. Przed każdy użytkownik może zalogować się z innej domeny niż domena oryginalnego rejestracji aplikacji, administrator globalny innej domeny musi udzielić uprawnień do dostępu do danych katalogu aplikacji. Aby udzielić uprawnień, przejść do administratora globalnego `https://<URL of your developer portal>/aadadminconsent` (na przykład https://contoso.portal.azure-api.net/aadadminconsent), wpisz nazwę domeny dzierżawy usługi Active Directory, które chcą, aby przyznać dostęp, a następnie kliknij przycisk Prześlij. W poniższym przykładzie administrator globalny `miaoaad.onmicrosoft.com` chce się nadać uprawnienia do tego portalu dewelopera w konkretnym. 

![Uprawnienia][api-management-aad-consent]

Na następnym ekranie administratora globalnego pojawi się monit o potwierdzenie nadanie uprawnień. 

![Uprawnienia][api-management-permissions-form]

> Jeśli administrator globalny nie próbuje zalogować przed uprawnienia są udzielane przez administratora globalnego, próba logowania nie powiedzie się i zostanie wyświetlony komunikat o błędzie.
> 
> 

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Jak dodać zewnętrznych Azure grupy usługi Active Directory
Po włączeniu dostępu dla użytkowników w usłudze Azure Active Directory, można dodać grupy usługi Azure Active Directory interfejsu API do zarządzania w celu skojarzenia deweloperów w grupie z żądaną produktów w prostszy sposób zarządzać.

> Aby skonfigurować zewnętrznego grupy usługi Azure Active Directory, usługi Azure Active Directory należy najpierw skonfigurować na karcie tożsamości wykonując poniższe kroki w poprzedniej sekcji. 
> 
> 

Zewnętrzne grup usługi Azure Active Directory są dodawane z **widoczność** na karcie produktu, dla którego chcesz udzielić dostępu do grupy. Kliknij przycisk **produktów**, a następnie kliknij nazwę żądanego produktu.

![Konfigurowanie produktu][api-management-configure-product]

Przełącz się do **widoczność** , a następnie kliknij pozycję **Dodawanie grup z usługi Azure Active Directory**.

![Dodaj grupy][api-management-add-groups]

Wybierz **Azure Active Directory dzierżawy** z listy rozwijanej liście, a następnie wpisz nazwę w odpowiedniej grupy **grup** do dodania pola tekstowego.

![Wybieranie grupy][api-management-select-group]

Ta nazwa grupy można znaleźć w **grup** lista dla usługi Azure Active Directory, jak pokazano w poniższym przykładzie.

![Lista grup usługi Azure Active Directory][api-management-aad-groups-list]

Kliknij przycisk **Dodaj** do sprawdzania poprawności nazwy grupy i Dodaj grupę. W tym przykładzie **deweloperzy 5 Contoso** jest dodana grupa zewnętrzna. 

![Grupy dodane][api-management-aad-group-added]

Kliknij przycisk **zapisać** można zapisać nowego wybranej grupy.

Po grupy usługi Azure Active Directory zostało skonfigurowane z jednym z produktów, jest ona dostępna do sprawdzenia na **widoczność** kartę dla innych produktów w wystąpieniu usługi Zarządzanie interfejsami API.

Aby przejrzeć i skonfiguruj właściwości dla zewnętrznych grup po ich dodaniu, kliknij nazwę grupy z **grup** kartę.

![Zarządzanie grupami][api-management-groups]

W tym miejscu można edytować **nazwa** i **opis** grupy.

![Edytuj grupę][api-management-edit-group]

Użytkownikom skonfigurowane usługi Azure Active Directory można zalogować się do portalu dla deweloperów i widoku oraz subskrybować żadnej z grup, do których mają widoczność, postępując zgodnie z instrukcjami w następnej sekcji.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Jak zalogować się do portalu dla deweloperów przy użyciu konta usługi Azure Active Directory
Aby zalogować się do portalu dla deweloperów przy użyciu konta usługi Azure Active Directory skonfigurowanych w poprzednich sekcjach, Otwórz nowe okno przeglądarki, za pomocą **adres URL logowania** konfiguracji aplikacji usługi Active Directory i kliknij przycisk **Usługi azure Active Directory**.

![Portalu dla deweloperów][api-management-dev-portal-signin]

Wprowadź poświadczenia użytkowników w usłudze Azure Active Directory, a następnie kliknij przycisk **Zaloguj**.

![Logowanie][api-management-aad-signin]

Może pojawić się prośba o formularz rejestracji Jeśli żadnych dodatkowych informacji jest wymagane. Wypełnij formularz rejestracji, a następnie kliknij przycisk **Zarejestruj**.

![Rejestracja][api-management-complete-registration]

Użytkownika jest obecnie zalogowany do portalu dla deweloperów wystąpienia usługi Zarządzanie interfejsami API.

![Rejestracja zakończona][api-management-registration-complete]

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-aad-consent]: ./media/api-management-howto-aad/api-management-aad-consent.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

