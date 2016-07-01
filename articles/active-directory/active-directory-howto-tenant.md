<properties
    pageTitle="Jak uzyskać dzierżawę usługi Azure AD | Microsoft Azure"
    description="Jak uzyskać dzierżawę usługi Azure Active Directory w celu rejestracji i tworzenia aplikacji."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# Jak uzyskać dzierżawę usługi Azure Active Directory

W usłudze Azure Active Directory (Azure AD) [dzierżawa](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) reprezentuje organizację.  Jest to dedykowane wystąpienie usługi Azure AD, którą organizacja otrzymuje i której zostaje właścicielem po zarejestrowaniu się w usłudze w chmurze firmy Microsoft, takiej jak platforma Azure, usługa Microsoft Intune czy usługa Office 365.  Każda dzierżawa usługi Azure AD jest unikatowa i oddzielona od innych dzierżaw usługi Azure AD.  

Dzierżawa gromadzi użytkowników w firmie i informacje o nich — hasła, dane profilu użytkownika, uprawnienia i tak dalej.  Zawiera także grupy, aplikacje i inne informacje dotyczące organizacji i jej zabezpieczeń.

Aby umożliwić użytkownikom usługi Azure AD zalogowanie się do Twojej aplikacji, musisz zarejestrować aplikację w dzierżawie.  Publikowanie aplikacji w dzierżawie usługi Azure AD jest **całkowicie bezpłatne**.  W rzeczywistości większość deweloperów utworzy kilka dzierżaw i aplikacji do celów testowych, związanych z programowaniem, przeprowadzaniem badań oraz w ramach etapu przejściowego.  Organizacje, które zarejestrują się i będą korzystać z aplikacji, mogą opcjonalnie zakupić licencje, jeśli chcą skorzystać z zaawansowanych funkcji katalogów.

Jak uzyskać dzierżawę usługi Azure AD?  Procedura może przebiegać nieco inaczej, jeśli:

- [Masz istniejącą subskrypcję usługi Office 365](#use-an-existing-office-365-subscription)
- [Masz istniejącą subskrypcję platformy Azure skojarzoną z kontem Microsoft](#use-an-msa-azure-subscription)
- [Masz istniejącą subskrypcję platformy Azure skojarzoną z kontem organizacyjnym](#use-an-organizational-azure-subscription)
- [Nie masz żadnej z powyższych subskrypcji i chcesz rozpocząć od zera](#start-from-scratch)

## Korzystanie z istniejącej subskrypcji usługi Office 365
Jeśli masz istniejącą subskrypcję usługi Office 365, ale nie masz subskrypcji platformy Azure (i nie możesz zalogować się do [Portalu zarządzania platformy Azure](https://manage.windowsazure.com)), postępuj według [tych instrukcji](https://technet.microsoft.com/library/dn832618.aspx), aby uzyskać dostęp do dzierżawy usługi Azure AD.

## Korzystanie z subskrypcji platformy Azure dla konta Microsoft
Jeśli Twoje indywidualne konto Microsoft zostało wcześniej zarejestrowane w celu uzyskania subskrypcji platformy Azure, masz już dzierżawę.  W [Portalu zarządzania platformy Azure](https://manage.windowsazure.com) powinna być widoczna dzierżawa o nazwie „Domyślna dzierżawa” wyświetlana w obszarze „Wszystkie elementy” i „Active Directory”.  Możesz korzystać z tej dzierżawy zgodnie z własnymi potrzebami, ale warto utworzyć konto administratora organizacyjnego.

W tym celu wykonaj poniższe kroki.  Możesz też utworzyć nową dzierżawę i utworzyć administratora w tej dzierżawie, postępując według podobnej procedury.

1.  Zaloguj się do [Portalu zarządzania platformy Azure](https://manage.windowsazure.com) za pomocą indywidualnego konta.
2.  W portalu przejdź do sekcji „Active Directory” (na lewym pasku nawigacyjnym).
3.  Z listy dostępnych katalogów wybierz pozycję „Katalog domyślny”.
4.  Kliknij link Użytkownicy na górze strony.  Pojawi się lista z pojedynczym użytkownikiem i wartością „Konto Microsoft” w kolumnie Źródło.
5.  Kliknij polecenie „Dodaj użytkownika” w dolnej części strony.
6.  W formularzu Dodawanie użytkownika podaj następujące dane:
    - Typ użytkownika: nowy użytkownik w organizacji.
    - Nazwa użytkownika: (wybierz nazwę użytkownika dla tego administratora).
    - Imię/Nazwisko/Nazwa wyświetlana: (wybierz odpowiednie wartości).
    - Rola: administrator globalny.
    - Alternatywny adres e-mail: (wprowadź odpowiednie wartości).
    - Opcjonalnie: włącz usługę Multi-Factor Authentication.
    - Na koniec kliknij zielony przycisk „UTWÓRZ”, aby zakończyć tworzenie użytkownika (i wyświetlić hasło tymczasowe).
7.  Po ukończeniu pracy z formularzem Dodawanie użytkownika i otrzymaniu tymczasowego hasła dla nowego użytkownika administracyjnego pamiętaj o zapisaniu hasła. Jest to konieczne, ponieważ w celu zmiany hasła trzeba będzie zalogować się za pomocą danych nowego użytkownika. Używając alternatywnego adresu e-mail, możesz również wysłać hasło bezpośrednio do użytkownika.
8.  Aby zmienić hasło tymczasowe, zaloguj się na stronie https://login.microsoftonline.com, używając nowego konta użytkownika, i zmień hasło po wyświetleniu odpowiedniego monitu.


## Korzystanie z organizacyjnej subskrypcji platformy Azure
Jeśli konto organizacyjne zostało wcześniej zarejestrowane w celu uzyskania subskrypcji platformy Azure, masz już dzierżawę.  W [Portalu zarządzania platformy Azure](https://manage.windowsazure.com) dzierżawa powinna być widoczna w obszarze „Wszystkie elementy” i „Active Directory.”  Z tej dzierżawy możesz korzystać zależnie od potrzeb.  Możesz również utworzyć nową dzierżawę za pomocą przycisku „Nowy” w lewym dolnym rogu portalu.


## Rozpoczynanie od zera
Jeśli wszystkie powyższe informacje są dla Ciebie mało zrozumiałe, nie przejmuj się.  Po prostu odwiedź stronę [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization), aby zarejestrować się na platformie Azure jako nowa organizacja.  Po wykonaniu procesu będziesz dysponować własną dzierżawą usługi Azure AD wraz z nazwą domeny wybraną podczas rejestracji.  W [Portalu zarządzania platformy Azure](https://manage.windowsazure.com) możesz odnaleźć dzierżawę, przechodząc do obszaru „Usługa Active Directory” w panelu nawigacyjnym po lewej stronie.

Podczas procesu rejestracji na platformie Azure wymagane jest podanie danych karty kredytowej.  Możesz to zrobić bez obaw — nie będą naliczane opłaty ani za publikowanie aplikacji w usłudze Azure AD, ani za tworzenie nowych dzierżaw.



<!--HONumber=Jun16_HO2-->


