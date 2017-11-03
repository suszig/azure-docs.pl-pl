---
title: "Uwierzytelnianie i autoryzację w usługi Azure Mobile Apps | Dokumentacja firmy Microsoft"
description: "Odwołanie koncepcyjne i omówienie uwierzytelniania / autoryzacji funkcji usługi Azure Mobile Apps"
services: app-service\mobile
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: a46dbf70-867d-48f6-8885-7f5207ad102e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 4105392f58eaf37e88c1d9ffb74f3f4133fa5482
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Uwierzytelnianie i autoryzacja w usłudze Azure Mobile Apps
## <a name="what-is-app-service-authentication--authorization"></a>Co to jest aplikacja usługi uwierzytelniania / autoryzacji?
> [!NOTE]
> W tym temacie zostaną poddane migracji do skonsolidowanego [aplikacji usługi uwierzytelniania / autoryzacji](../app-service/app-service-authentication-overview.md) temat, który obejmuje aplikacje interfejsu API, mobilne i sieci Web.
> 
> 

Aplikacja usługi uwierzytelniania / autoryzacji jest to funkcja umożliwia aplikacji do logowania użytkowników z nie zmian w kodzie wymaganych do wewnętrznej bazy danych aplikacji. Zapewnia prosty sposób ochrona aplikacji i pracować z danymi użytkownika.

Korzysta z usługi aplikacji tożsamości federacyjnych, w którym strona 3rd **dostawcy tożsamości** ("IDP") przechowuje konta i uwierzytelnia użytkowników, a aplikacja używa tej tożsamości zamiast własny. App Service obsługuje pięć dostawców tożsamości fabrycznej: *usługi Azure Active Directory*, *Facebook*, *Google*, *Account Microsoft* , a *Twitter*. Ta obsługa można również rozwinąć dla aplikacji, integrując innego dostawcy tożsamości lub rozwiązania tożsamości niestandardowej.

Aplikację można wykorzystać dowolną liczbę tych dostawców tożsamości, więc możesz zapewnić użytkownikom końcowym opcje dotyczące sposobu logowania.

Jeśli chcesz rozpocząć pracę od razu, zobacz jedną z następujących samouczków:

* [Dodawanie uwierzytelniania do aplikacji systemu iOS]
* [Dodawanie uwierzytelniania do aplikacji platformy Xamarin.iOS]
* [Dodawanie uwierzytelniania do aplikacji platformy Xamarin.Android]
* [Dodawanie uwierzytelniania do aplikacji systemu Windows]

## <a name="how-authentication-works"></a>Jak działa uwierzytelniania
W celu uwierzytelnienia przy użyciu jednego z dostawców tożsamości, należy najpierw skonfigurować dostawcy tożsamości, aby dowiedzieć się o aplikacji. Dostawca tożsamości następnie zapewnia identyfikatory i hasła podanych do aplikacji. To kończy relacji zaufania i umożliwia aplikacji usługi do weryfikowania tożsamości podany.

Te kroki są szczegółowo opisane w następujących tematach:

* [Jak skonfigurować aplikację do używania nazwy logowania w usłudze Azure Active Directory]
* [Jak skonfigurować aplikację do używania logowania usługi Facebook]
* [Jak skonfigurować aplikację do używania logowania Google]
* [Jak skonfigurować aplikację do używania Account Microsoft logowania]
* [Jak skonfigurować aplikację do używania logowania usługi Twitter]

Gdy wszystko jest skonfigurowany do wewnętrznej bazy danych, można zmodyfikować klientowi Zaloguj się. Istnieją dwie metody w tym miejscu:

* Za pomocą jednego wiersza kodu, umożliwiają Mobile Apps SDK klienta Zaloguj się użytkownicy.
* Korzystaj z zestawu SDK opublikowane przez dostawcę tożsamości danego do ustalenia tożsamości i uzyskać dostęp do usługi App Service.

> [!TIP]
> Większość aplikacji należy używać dostawcy zestawu SDK można pobrać mogą logować się bardziej wrażenie macierzystego i wykorzystanie odświeżania pomocy technicznej i inne korzyści specyficznego dla dostawcy.
> 
> 

### <a name="how-authentication-without-a-provider-sdk-works"></a>Jak działa uwierzytelniania bez dostawcy zestawu SDK
Jeśli nie chcesz, aby skonfigurować dostawcy zestawu SDK, można zezwolić Mobile Apps wykonać nazwy logowania. Klient Mobile Apps SDK Otwórz widok sieci web dostawcy wybrane i zakończyć logowanie. Od czasu do czasu na blogi i fora widocznych się, że to określane jako "serwera przepływ" lub "przepływ skierowane do serwera" od momentu jego zarządza logowania i zestawu SDK klienta nigdy nie otrzymuje token dostawcy.

Kod potrzebne do uruchomienia tego przepływu zostało opisane w samouczku uwierzytelniania dla każdej platformy. Na końcu przepływu klienta SDK ma token usługi aplikacji, a token są automatycznie dołączane do wszystkich żądań do wewnętrznej bazy danych.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Jak działa uwierzytelniania za pomocą dostawcy zestawu SDK
Praca z dostawcą zestawu SDK umożliwia środowisko logowania ściślej interakcję z platformy systemu operacyjnego, aplikacja jest uruchomiona na. Zapewnia to również token dostawcy i niektóre informacje użytkownika na komputerze klienckim, co pozwala na znacznie łatwiejsze do wykorzystania wykres interfejsów API i dostosowanie środowiska użytkownika. Od czasu do czasu na blogi i fora będą widoczne to określane jako "przebieg klienta" lub "klient skierowany przepływu" od kodu na kliencie jest obsługa logowania i kod klienta ma dostęp do dostawcy tokenu.

Po uzyskaniu tokenu dostawcy musi wysłane do aplikacji usługi do sprawdzania poprawności. Na końcu przepływu klienta SDK ma token usługi aplikacji, a token są automatycznie dołączane do wszystkich żądań do wewnętrznej bazy danych. Deweloper można również zarejestrować odwołania do tokenu dostawcy, gdy wybiera on.

## <a name="how-authorization-works"></a>Jak działa autoryzacji
Aplikacja usługi uwierzytelniania / autoryzacji udostępnia kilka opcji **działania należy podjąć w przypadku nieuwierzytelnionego żądania**. Przed kodu odbiera danego żądania, może mieć Sprawdź usługi aplikacji, jeśli żądanie jest uwierzytelniane i jeśli nie, go odrzucić i podejmij próbę ich użytkownik zalogować przed podjęciem ponownej próby.

Jedną z opcji ma mieć nieuwierzytelniony Przekieruj żądania do jednego z dostawców tożsamości. W przeglądarce sieci web może to potrwać faktycznie użytkownika do nowej strony. Jednak nie można przekierować z klientów urządzeń przenośnych w ten sposób i nieuwierzytelnione odpowiedzi będą otrzymywać HTTP *401 nieautoryzowane* odpowiedzi. Biorąc pod uwagę to, zawsze należy pierwszego żądania przez klienta do punktu końcowego logowania i dokonaj wywołania do innych interfejsów API. Próba wywołania innego interfejsu API przed zalogowaniem się użytkownika klienta zostanie zwrócony błąd.

Jeśli chcesz mieć bardziej szczegółowego kontrolowania, przez które punkty końcowe wymagają uwierzytelniania, można również wybrać "żadna akcja ze strony (Zezwalaj na żądanie)" dla nieuwierzytelnione żądania. W takim przypadku wszystkie decyzje uwierzytelniania są odroczone w kodzie aplikacji. Umożliwia to także zezwolić na dostęp do określonych użytkowników na podstawie reguł autoryzacji niestandardowej.

## <a name="documentation"></a>Dokumentacja
Następujące samouczki przedstawiają sposób dodawania uwierzytelniania dla klientów mobilnych za pomocą usługi aplikacji:

* [Dodawanie uwierzytelniania do aplikacji systemu iOS]
* [Dodawanie uwierzytelniania do aplikacji platformy Xamarin.iOS]
* [Dodawanie uwierzytelniania do aplikacji platformy Xamarin.Android]
* [Dodawanie uwierzytelniania do aplikacji systemu Windows]

Następujące samouczki pokazują, jak skonfigurować usługę aplikacji, aby korzystać z uwierzytelniania różnych dostawców:

* [Jak skonfigurować aplikację do używania nazwy logowania w usłudze Azure Active Directory]
* [Jak skonfigurować aplikację do używania logowania usługi Facebook]
* [Jak skonfigurować aplikację do używania logowania Google]
* [Jak skonfigurować aplikację do używania Account Microsoft logowania]
* [Jak skonfigurować aplikację do używania logowania usługi Twitter]

Jeśli chcesz użyć systemu tożsamości innych niż podana w tym miejscu można również wykorzystać [obsługę niestandardowych uwierzytelniania na serwerze .NET SDK w wersji preview](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Dodawanie uwierzytelniania do aplikacji systemu iOS]: app-service-mobile-ios-get-started-users.md
[Dodawanie uwierzytelniania do aplikacji platformy Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started-users.md
[Dodawanie uwierzytelniania do aplikacji platformy Xamarin.Android]: app-service-mobile-xamarin-android-get-started-users.md
[Dodawanie uwierzytelniania do aplikacji systemu Windows]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Jak skonfigurować aplikację do używania nazwy logowania w usłudze Azure Active Directory]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Jak skonfigurować aplikację do używania logowania usługi Facebook]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[Jak skonfigurować aplikację do używania logowania Google]: ../app-service/app-service-mobile-how-to-configure-google-authentication.md
[Jak skonfigurować aplikację do używania Account Microsoft logowania]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[Jak skonfigurować aplikację do używania logowania usługi Twitter]: ../app-service/app-service-mobile-how-to-configure-twitter-authentication.md
