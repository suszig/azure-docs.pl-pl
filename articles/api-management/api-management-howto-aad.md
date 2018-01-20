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
ms.date: 01/16/2018
ms.author: apimpm
ms.openlocfilehash: 01ea39983c4c1ad41692fce31ef6fd9c81461c8e
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Sposób autoryzowania konta dewelopera przy użyciu usługi Azure Active Directory w usłudze Azure API Management

Ten przewodnik zawiera instrukcje umożliwić dostęp do portalu dla deweloperów dla użytkowników z usługi Azure Active Directory. W tym przewodniku przedstawiono również sposób zarządzać grupami użytkowników usługi Azure Active Directory przez dodanie zewnętrznej grupy, które zawierają użytkowników usługi Azure Active Directory.

> [!WARNING]
> Integracja z usługą Azure Active Directory jest dostępny w [Developer, Standard i Premium](https://azure.microsoft.com/pricing/details/api-management/) tylko warstwy.

## <a name="prerequisites"></a>Wymagania wstępne

- Ukończenie następujących Szybki Start: [utworzenia wystąpienia usługi Azure API Management](get-started-create-service-instance.md).
- Importowanie i opublikować wystąpienia interfejsu API zarządzania. Aby uzyskać więcej informacji, zobacz [Import i opublikuj](import-and-publish.md).

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Sposób autoryzowania konta dewelopera przy użyciu usługi Azure Active Directory

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com). 
2. Wybierz pozycję ![Strzałka](./media/api-management-howto-aad/arrow.png).
3. W polu wyszukiwania wpisz "interfejsu api".
4. Kliknij przycisk **usługi Zarządzanie interfejsami API**.
5. Wybierz APIM wystąpienia usługi.
6. W obszarze **zabezpieczeń**, wybierz pozycję **tożsamości**.

    ![Tożsamości zewnętrznych](./media/api-management-howto-aad/api-management-with-aad001.png)
7. Kliknij przycisk **+ Dodaj** od góry.

    **Dostawcy tożsamości Dodaj** po prawej stronie zostanie wyświetlone okno.
8. W obszarze **typ dostawcy**, wybierz pozycję **usługi Azure Active Directory**.

    Formanty, które umożliwiają wprowadź inne potrzebne informacje są wyświetlane w oknie. Dostępne kontrolki: **identyfikator klienta**, **klucz tajny klienta** (możesz uzyskać w dalszej części tego samouczka).
9. Zanotuj **adresem URL przekierowania**.  
10. W przeglądarce otwórz innej karty. 
11. Otwórz witrynę [Azure Portal](https://portal.azure.com).
12. Wybierz pozycję ![Strzałka](./media/api-management-howto-aad/arrow.png).
13. Typ "active" **usługi Azure Active Directory** pojawi się.
14. Wybierz **usługi Azure Active Directory**.
15. W obszarze **ZARZĄDZAJ**, wybierz pozycję **rejestracji aplikacji**.

    ![Rejestracja aplikacji](./media/api-management-howto-aad/api-management-with-aad002.png)
16. Kliknij pozycję **Rejestrowanie nowej aplikacji**.

    **Utwórz** po prawej stronie zostanie wyświetlone okno. To, gdzie możesz wprowadzić informacje o relavent usługi AAD aplikacji.
17. Eneter nazwę aplikacji.
18. Wybierz typ aplikacji **aplikacji/interfejs API sieci Web**.
19. Adres URL logowania wprowadź adres URL rejestracji z portalu dla deweloperów. W tym przykładzie jest adres URL logowania: https://apimwithaad.portal.azure-api.net/signin.
20. Kliknij przycisk **Utwórz** do tworzenia aplikacji.
21. Aby znaleźć aplikacji, wybierz **rejestracji aplikacji** i Wyszukaj według nazwy.

    ![Rejestracja aplikacji](./media/api-management-howto-aad/find-your-app.png)
22. Po zarejestrowaniu aplikacji, przejdź do **adres URL odpowiedzi** i upewnij się, "URL przekierowania" jest ustawiona na wartość uzyskano z kroku 9. 
23. Jeśli chcesz skonfigurować aplikację (na przykład zmienić **adres URL Identyfikatora aplikacji**) wybierz **właściwości**.

    ![Rejestracja aplikacji](./media/api-management-howto-aad/api-management-with-aad004.png)

    Jeśli wiele Azure Active Directory mają być używany dla tej aplikacji, kliknij przycisk **tak** dla **aplikacji jest wielodostępne**. Wartość domyślna to **nr**.
24. Ustawianie uprawnienia aplikacji przez wybranie **wymagane uprawnienia**.
25. Wybierz aplikacji i wyboru **odczytuj dane katalogu** i **Zaloguj się i odczytuj profil użytkownika**.

    ![Rejestracja aplikacji](./media/api-management-howto-aad/api-management-with-aad005.png)

    Aby uzyskać więcej informacji o aplikacji i uprawnień delegowanych, zobacz [podczas uzyskiwania dostępu do interfejsu API programu Graph][Accessing the Graph API].
26. W oknie po lewej stronie, skopiuj **identyfikator aplikacji** wartość.

    ![Rejestracja aplikacji](./media/api-management-howto-aad/application-id.png)
27. Przełącz się do aplikacji interfejsu API zarządzania. **Dostawcy tożsamości Dodaj** powinny być wyświetlane okna. <br/>Wklej **identyfikator aplikacji** wartość w **identyfikator klienta** pole.
28. Wrócić do konfiguracji usługi Azure Active Directory, a następnie kliknij polecenie **klucze**.
29. Utwórz nowy klucz przez speicifying nazwę i czas trwania. 
30. Kliknij pozycję **Zapisz**. Pobiera wygenerowany klucz.

    Skopiuj klucz do Schowka.

    ![Rejestracja aplikacji](./media/api-management-howto-aad/api-management-with-aad006.png)

    > [!NOTE]
    > Zanotuj tego klucza. Po zamknięciu okna konfiguracji usługi Azure Active Directory, klucz nie może ponownie wyświetlone.
    > 
    > 
31. Przełącz się do aplikacji interfejsu API zarządzania. <br/>W **dostawcy tożsamości Dodaj** okna, Wklej klucz do **klucz tajny klienta** pola tekstowego.
32. **Dostawcy tożsamości Dodaj** zawiera okno **dozwolone dzierżaw** pole tekstowe, w przypadku Określ, które katalogi mają dostęp do interfejsów API wystąpienia usługi Zarządzanie interfejsami API. <br/>Określ domeny wystąpień usługi Azure Active Directory, do których chcesz udzielić dostępu. Wiele domen można oddzielić newlines, spacjami lub przecinkami.

    Można określić wiele domen w **dozwolone dzierżaw** sekcji. Przed każdy użytkownik może zalogować się z innej domeny niż domena oryginalnego rejestracji aplikacji, administrator globalny innej domeny musi udzielić uprawnień do dostępu do danych katalogu aplikacji. Aby udzielić uprawnień, przejść do administratora globalnego `https://<URL of your developer portal>/aadadminconsent` (na przykład https://contoso.portal.azure-api.net/aadadminconsent), wpisz nazwę domeny dzierżawy usługi Active Directory, które chcą, aby przyznać dostęp, a następnie kliknij przycisk Prześlij. W poniższym przykładzie administrator globalny `miaoaad.onmicrosoft.com` chce się nadać uprawnienia do tego portalu dewelopera w konkretnym. 

33. Po wymaganą konfiguracją jest określony, kliknij przycisk **Dodaj**.

    ![Rejestracja aplikacji](./media/api-management-howto-aad/api-management-with-aad007.png)

Po zmiany zostaną zapisane, użytkownicy w określonej usługi Azure Active Directory mogą zalogować się do portalu dla deweloperów wykonując kroki opisane w [zalogować się do portalu dla deweloperów przy użyciu konta usługi Azure Active Directory](#log_in_to_dev_portal).

![Uprawnienia](./media/api-management-howto-aad/api-management-aad-consent.png)

Na następnym ekranie administratora globalnego pojawi się monit o potwierdzenie nadanie uprawnień. 

![Uprawnienia](./media/api-management-howto-aad/api-management-permissions-form.png)

Jeśli administrator globalny nie próbuje zalogować przed uprawnienia są udzielane przez administratora globalnego, próba logowania nie powiedzie się i zostanie wyświetlony komunikat o błędzie.

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Jak dodać zewnętrznych Azure grupy usługi Active Directory

Po włączeniu dostępu dla użytkowników w usłudze Azure Active Directory, można dodać grupy usługi Azure Active Directory interfejsu API do zarządzania w celu skojarzenia deweloperów w grupie z żądaną produktów w prostszy sposób zarządzać.

Aby skonfigurować zewnętrznego grupy usługi Azure Active Directory, usługi Azure Active Directory należy najpierw skonfigurować na karcie tożsamości wykonując poniższe kroki w poprzedniej sekcji. 

Zewnętrzne grup usługi Azure Active Directory są dodawane z **grup** kartę wystąpienia interfejsu API zarządzania.

![Grupy](./media/api-management-howto-aad/api-management-with-aad008.png)

1. Wybierz kartę **Grupy**.
2. Kliknij przycisk **grupy dodać AAD** przycisku.
3. Wybierz grupę, którą chcesz dodać.
4. Naciśnij klawisz **wybierz** przycisku.

Po utworzeniu grupy usługi Azure Active Directory, można zobaczyć i skonfigurować właściwości dla zewnętrznych grup po zostały dodane, kliknij nazwę grupy z **grup** kartę.

W tym miejscu można edytować **nazwa** i **opis** grupy.
 
Użytkownikom skonfigurowane usługi Azure Active Directory można zalogować się do portalu dla deweloperów i widoku oraz subskrybować żadnej z grup, do których mają widoczność, postępując zgodnie z instrukcjami w następnej sekcji.

## <a name="a-idlogintodevportalhow-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a><a id="log_in_to_dev_portal"/>Jak zalogować się do portalu dla deweloperów przy użyciu konta usługi Azure Active Directory

Aby zalogować się do portalu dla deweloperów przy użyciu konta usługi Azure Active Directory skonfigurowanych w poprzednich sekcjach, Otwórz nowe okno przeglądarki, za pomocą **adres URL logowania** konfiguracji aplikacji usługi Active Directory i kliknij przycisk **Usługi azure Active Directory**.

![Portalu dla deweloperów][api-management-dev-portal-signin]

Wprowadź poświadczenia użytkowników w usłudze Azure Active Directory, a następnie kliknij przycisk **Zaloguj**.

![Logowanie][api-management-aad-signin]

Może pojawić się prośba o formularz rejestracji Jeśli żadnych dodatkowych informacji jest wymagane. Wypełnij formularz rejestracji, a następnie kliknij przycisk **Zarejestruj**.

![Rejestracja][api-management-complete-registration]

Użytkownika jest obecnie zalogowany do portalu dla deweloperów wystąpienia usługi Zarządzanie interfejsami API.

![Rejestracja zakończona][api-management-registration-complete]

[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

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

