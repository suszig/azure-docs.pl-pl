---
title: "Interfejs API Graph usługi Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Omówienie i szybki start — przewodnik dla interfejsu API programu Graph, dzięki czemu programowy dostęp do usługi Azure AD za pomocą punktów końcowych interfejsu API REST."
services: active-directory
documentationcenter: 
author: viv-liu
manager: mbaldwin
editor: mbaldwin
ms.assetid: 5471ad74-20b3-44df-a2b5-43cde2c0a045
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: viviali
ms.custom: aaddev
ms.openlocfilehash: 79bb96171c00fa62f24113800c3824856febe728
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-graph-api"></a>Interfejs API programu Graph usługi Azure Active Directory
> [!IMPORTANT]
> Zdecydowanie zalecamy uzyskiwanie dostępu do zasobów usługi Azure Active Directory za pomocą programu [Microsoft Graph](https://graph.microsoft.io/) zamiast interfejsu API funkcji Azure AD Graph. Obecnie koncentrujemy nasze działania deweloperskie na programie Microsoft Graph i nie planujemy żadnych dodatkowych rozszerzeń dla interfejsu API funkcji Azure AD Graph. Istnieje bardzo ograniczona liczba scenariuszy, w których można używać interfejsu API funkcji Azure AD Graph. Więcej informacji można znaleźć we wpisie w blogu [Microsoft Graph or the Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) w Centrum deweloperów pakietu Office.
> 
> 

Azure Active Directory interfejsu API programu Graph zapewnia dostęp programistyczny do usługi Azure AD za pomocą punktów końcowych interfejsu API REST. Aplikacje mogą używać interfejsu API programu Graph do wykonania tworzenia, odczytu, aktualizacji i usuwania operacji (CRUD) w katalogu danych i obiektów. Na przykład interfejsu API programu Graph obsługuje następujące typowe operacje dla obiekt użytkownika:

* Tworzenie nowego użytkownika w katalogu
* Pobierz właściwości szczegółowe użytkownika, takie jak ich grup
* Zaktualizuj właściwości użytkownika, takie jak ich lokalizacji i numer telefonu lub zmień ich hasła
* Sprawdź członkostwo w grupie użytkownika dla dostępu opartej na rolach
* Wyłącz konto użytkownika lub usuń go całkowicie

Oprócz obiektów użytkowników można wykonywać operacje podobne na inne obiekty, takie jak grupy i aplikacje. Aby wywołać interfejsu API programu Graph dla katalogu, aplikacja musi być zarejestrowana w usłudze Azure AD i umożliwiać dostęp do katalogu. Zazwyczaj jest to osiągane przez użytkownika lub administratora przepływu zgody.

Aby rozpocząć korzystanie z usługi Azure Active Directory interfejsu API programu Graph, zobacz [Przewodnik Szybki Start interfejsu API Graph](active-directory-graph-api-quickstart.md), lub wyświetlić [interakcyjne dokumentacji interfejsu API programu Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Funkcje
Interfejsu API programu Graph zapewnia następujące funkcje:

* **Punkty końcowe interfejsu API REST**: interfejs API programu Graph jest usługą RESTful, składającej się z punktów końcowych, które są dostępne przy użyciu standardowych żądań HTTP. Interfejsu API programu Graph obsługuje XML lub Javascript Object Notation (JSON) typy zawartości dla żądań i odpowiedzi. Aby uzyskać więcej informacji, zobacz [dokumentacja interfejsu API REST usługi Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Uwierzytelnianie za pomocą usługi Azure AD**: każde żądanie do interfejsu API programu Graph musi zostać uwierzytelniony przez dołączenie JSON Web Token (JWT) w nagłówku autoryzacji żądania. Token ten jest uzyskaną przez przesyłania do punktu końcowego tokenu usługi Azure AD i podanie prawidłowych poświadczeń. Można użyć przepływ poświadczeń klienta OAuth 2.0 lub kod autoryzacji Udziel przepływu można uzyskać tokenu umożliwiającego wywołanie wykresu. Aby uzyskać więcej informacji [OAuth 2.0 w usłudze Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Autoryzacji opartej na rolach (RBAC)**: grupy zabezpieczeń są używane do wykonywania RBAC w interfejsu API programu Graph. Na przykład, jeśli chcesz określić, czy użytkownik ma dostęp do określonych zasobów, aplikacja może wywołać [sprawdzić członkostwa w grupie (przechodnie)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) operacja, która zwraca wartość PRAWDA lub FAŁSZ.
* **Zapytanie różnicowej**: Jeśli chcesz sprawdzić zmiany w katalogu między dwoma okresów bez konieczności częste zapytań do interfejsu API programu Graph, można utworzyć żądanie różnicowej zapytania. Ten typ żądania zwróci tylko zmiany wprowadzone od poprzedniego żądania różnicowej zapytania i bieżącego żądania. Aby uzyskać więcej informacji, zobacz [Azure AD Graph API różnicowej zapytania](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Rozszerzenia katalogów**: Jeśli tworzysz aplikację, która należy do odczytu lub zapisu właściwości unikatowy dla obiektów katalogu może rejestrować i używania wartości rozszerzenia przy użyciu interfejsu API programu Graph. Na przykład jeśli aplikacja wymaga właściwości Identyfikatora użytkownika Skype dla każdego użytkownika, możesz zarejestrować nową właściwość w katalogu i będzie on dostępny dla każdego obiektu użytkownika. Aby uzyskać więcej informacji, zobacz [Azure AD Graph interfejsu API katalogu schematu rozszerzenia](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **Zabezpieczone przez zakresy uprawnień**: interfejs API programu Graph usługi AAD przedstawia zakresy uprawnień, umożliwiające bezpieczny zgodę dostęp do danych usługi AAD i obsługi różnych typów aplikacji klienta, w tym:
  
  * z interfejsem użytkownika, które są przyznawane delegować dostępu do danych za pomocą autoryzacji od zalogowanego użytkownika (delegowane)
  * używające aplikacji zdefiniować kontroli dostępu opartej na rolach, takich jak klienci usługi/demon (role aplikacji)
    
    Zarówno delegowane i zakresy uprawnień roli aplikacji reprezentują uprawnienia udostępnianych przez interfejsu API programu Graph i może być wysłane przez aplikacje klienckie za pośrednictwem uprawnień rejestracji aplikacji [funkcji w portalu Azure](https://portal.azure.com). Klientów można sprawdzić zakresy uprawnień przyznanych im sprawdzając oświadczeń zakresu ("punkt połączenia usługi") została odebrana w tokenie dostępu dla delegowane uprawnienia i oświadczeń role ("ról") dla aplikacji uprawnienia roli. Dowiedz się więcej o [zakresy uprawnień interfejsu API Graph usługi Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).

## <a name="scenarios"></a>Scenariusze
Interfejsu API programu Graph umożliwia wiele scenariuszy, w aplikacji. Najczęściej używane są następujące scenariusze:

* **Aplikacji biznesowej (pojedynczej dzierżawy)**: W tym scenariuszu developer enterprise działa w przypadku organizacji, która ma subskrypcji usługi Office 365. Deweloper tworzenia aplikacji sieci web, która współdziała z usługą Azure AD umożliwia wykonywanie zadań takich przypisywania licencji do użytkownika. To zadanie wymaga dostępu do interfejsu API programu Graph, więc rejestrów developer pojedynczy dzierżawy aplikacji w usłudze Azure AD i konfiguruje uprawnienia odczytu i zapisu dla interfejsu API programu Graph. Następnie aplikacja jest skonfigurowana na potrzeby uzyskania tokenu umożliwiającego wywołanie interfejsu API programu Graph własne poświadczenia lub te, które obecnie logowania użytkownika.
* **Aplikacja usługi (wielodostępnej) oprogramowania jako**: W tym scenariuszu niezależnego dostawcy oprogramowania (ISV) jest tworzenie aplikacji hostowanej wielodostępnej sieci web, która zawiera funkcje zarządzania użytkownika do innych organizacji korzystających z usługi Azure AD. Tych funkcji wymaga dostępu do obiektów katalogu, a więc aplikacja musi wywołać interfejsu API programu Graph. Deweloper rejestruje aplikacji w usłudze Azure AD, skonfiguruje je do wymagają odczytu i zapisu uprawnienia dla interfejsu API programu Graph i następnie włącza dostępu zewnętrznego, dzięki czemu inne organizacje mogą wyrazić zgodę korzystanie z aplikacji w ich katalogu. Podczas uwierzytelniania użytkownika w innej organizacji do aplikacji po raz pierwszy, są wyświetlane okno dialogowe zgody użytkownika z uprawnieniami, który żąda aplikacji.  Udzielając zgody określi aplikacji te wymagane uprawnienia do interfejsu API programu Graph dla katalogu użytkownika. Aby uzyskać więcej informacji w ramach zgody, zobacz [omówienie Framework zgody](active-directory-integrating-applications.md).

## <a name="see-also"></a>Zobacz też
[Przewodnik Szybki Start Azure interfejs API Graph usługi AD](active-directory-graph-api-quickstart.md)

[AD Graph REST dokumentacji](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Przewodnik dewelopera usługi Azure Active Directory](active-directory-developers-guide.md)

