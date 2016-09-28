<properties 
    pageTitle="Uwierzytelnianie usługi RADIUS i serwer usługi Azure Multi-Factor Authentication"
    description="Ta strona dotyczy usługi Azure Multi-Factor Authentication i zawiera wskazówki pomocne we wdrażaniu uwierzytelniania usługi RADIUS i serwera usługi Azure Multi-Factor Authentication."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>




# Uwierzytelnianie usługi RADIUS i serwer usługi Azure Multi-Factor Authentication

Sekcja Uwierzytelnianie usługi RADIUS pozwala włączyć i skonfigurować uwierzytelnianie usługi RADIUS dla serwera usługi Azure Multi-Factor Authentication. RADIUS jest standardowym protokołem, który umożliwia akceptowanie i przetwarzanie żądań uwierzytelniania. Serwer usługi Azure Multi-Factor Authentication działa jako serwer usługi RADIUS. Jest umieszczany między klientem usługi RADIUS (np. urządzeniem sieci VPN) i obiektem docelowym uwierzytelniania (np. usługą Active Directory (AD), katalogiem LDAP lub innym serwerem RADIUS) w celu dodania usługi Azure Multi-Factor Authentication. Aby możliwe było działanie usługi Azure Multi-Factor Authentication, serwer usługi Azure Multi-Factor Authentication musi zostać skonfigurowany pod kątem komunikacji zarówno z serwerami klientów, jak i obiektem docelowym uwierzytelniania. Serwer usługi Azure Multi-Factor Authentication odbiera żądania od klienta usługi RADIUS, przeprowadza walidację poświadczeń względem obiektu docelowego uwierzytelniania, dodaje usługę Azure Multi-Factor Authentication i wysyła odpowiedź do klienta usługi RADIUS. Cały proces uwierzytelniania powiedzie się tylko w przypadku pomyślnego przebiegu zarówno uwierzytelniania podstawowego, jak i uwierzytelniania usługi Azure Multi-Factor Authentication.

>[AZURE.NOTE]
>Gdy serwer usługi MFA działa jako serwer usługi RADIUS, obsługuje tylko protokoły usługi RADIUS: PAP (protokół uwierzytelniania haseł) i MSCHAPv2 (protokół uwierzytelniania typu Challenge Handshake firmy Microsoft).  Inne protokoły, np. EAP (protokół uwierzytelniania rozszerzonego), mogą być używane, jeśli serwer usługi MFA działa jako serwer proxy usługi RADIUS połączony z innym serwerem usługi RADIUS, który obsługuje taki protokół, jak Microsoft NPS.
></br>
>W przypadku używania innych protokołów w tej konfiguracji jednokierunkowe tokeny SMS i OATH nie będą działać, ponieważ serwer usługi MFA nie będzie mógł zainicjować pomyślnej odpowiedzi na żądanie usługi RADIUS przy użyciu tego protokołu.


![Uwierzytelnianie usługi Radius](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## Konfiguracja uwierzytelniania usługi RADIUS

Aby skonfigurować uwierzytelnianie usługi RADIUS, zainstaluj serwer usługi Azure Multi-Factor Authentication na serwerze z systemem Windows. Jeśli korzystasz ze środowiska usługi Active Directory, przyłącz serwer do domeny znajdującej się w sieci. Poniższa procedura umożliwia skonfigurowanie serwera usługi Azure Multi-Factor Authentication:

1. Na serwerze usługi Azure Multi-Factor Authentication kliknij ikonę uwierzytelniania usługi RADIUS w menu po lewej stronie.
2. Zaznacz pole wyboru Włącz uwierzytelnianie usługi RADIUS.
3. Na karcie Klienci zmień porty uwierzytelniania i porty ewidencjonowania aktywności, jeśli usługa Azure Multi-Factor Authentication RADIUS ma być powiązana z niestandardowymi portami w celu nasłuchiwania żądań usługi RADIUS pochodzących od skonfigurowanych klientów.
4. Kliknij przycisk Dodaj Edytuj...
5. W oknie dialogowym Dodawanie klienta RADIUS wprowadź adres IP urządzenia/serwera, które będą uwierzytelniane na serwerze usługi Azure Multi-Factor Authentication, nazwę aplikacji (opcjonalnie) i wspólny klucz tajny. Wspólny klucz tajny musi być taki sam dla serwera usługi Azure Multi-Factor Authentication i urządzenia/serwera. Nazwa aplikacji jest widoczna w raportach usługi Azure Multi-Factor Authentication i może być wyświetlana w uwierzytelniających wiadomościach SMS lub wiadomościach aplikacji mobilnej.
6. Zaznacz pole Wymagaj dopasowania użytkownika usługi Multi-Factor Authentication, jeśli wszyscy użytkownicy zostali lub zostaną zaimportowani na serwer i będą podlegać uwierzytelnianiu wieloskładnikowemu. Jeśli znaczna liczba użytkowników nie została jeszcze zaimportowana na serwer i/lub będzie wykluczona z uwierzytelniania wieloskładnikowego, nie zaznaczaj tego pola. Zapoznaj się z plikiem pomocy, aby uzyskać dodatkowe informacje na temat tej funkcji.
7. Zaznacz pole Włącz rezerwowy token OATH, jeśli użytkownicy będą korzystać z uwierzytelniania aplikacji mobilnej za pomocą usługi Azure Multi-Factor Authentication i chcesz używać kodów dostępu OATH jako rezerwowej metody uwierzytelniania w przypadku uwierzytelniania pozapasmowego z użyciem połączeń telefonicznych, wiadomości SMS lub powiadomień wypychanych.
8. Kliknij przycisk OK.
9. Aby kolejnych klientów usługi RADIUS, powtórz kroki od 4 do 8.
10. Kliknij kartę Cel.
11. Jeśli serwer usługi Azure Multi-Factor Authentication jest zainstalowany na serwerze przyłączonym do domeny w środowisku usługi Active Directory, wybierz domenę systemu Windows.
12. Jeśli użytkownicy mają być uwierzytelniani względem katalogu LDAP, wybierz powiązanie LDAP. W przypadku używania powiązania LDAP kliknij ikonę Integracja katalogu i zmień konfigurację protokołu LDAP na karcie Ustawienia, tak aby serwer mógł utworzyć powiązanie z katalogiem. Instrukcje dotyczące konfigurowania protokołu LDAP można znaleźć w przewodniku po konfiguracji serwera proxy LDAP.
13. Jeśli użytkownicy mają być uwierzytelniani względem innego serwera RADIUS, wybierz co najmniej jeden serwer RADIUS.
14. Kliknij przycisk Dodaj, aby skonfigurować serwer jako serwer proxy dla żądań usługi RADIUS. Edytuj...
15. W oknie dialogowym Dodawanie serwera RADIUS wprowadź adres IP serwera RADIUS i wspólny klucz tajny. Wspólny klucz tajny musi być taki sam dla serwera usługi Azure Multi-Factor Authentication i serwera RADIUS. Zmień port uwierzytelniania i port ewidencjonowania aktywności, jeśli serwer RADIUS używa innych portów.
16. Kliknij przycisk OK.
17. Serwer usługi Azure Multi-Factor Authentication musisz dodać jako klienta RADIUS na drugim serwerze RADIUS, aby umożliwić mu przetwarzanie żądań dostępu wysyłanych z serwera usługi Azure Multi-Factor Authentication. Musisz użyć tego samego wspólnego klucza tajnego, który został skonfigurowany na serwerze usługi Azure Multi-Factor Authentication.
18. Możesz powtórzyć ten krok, aby dodać kolejne serwery RADIUS. Aby skonfigurować kolejność ich wywoływania na serwerze, użyj przycisków przenoszenia w górę i w dół. W tym momencie konfiguracja serwera usługi Azure Multi-Factor Authentication jest zakończona. Przy użyciu skonfigurowanych portów serwer nasłuchuje żądań dostępu usługi RADIUS pochodzących od skonfigurowanych klientów.   


## Konfiguracja klientów RADIUS

Aby skonfigurować klienta RADIUS, skorzystaj z następujących wskazówek:

- Skonfiguruj uwierzytelnianie urządzenia/serwera pod adresem IP serwera usługi Azure Multi-Factor Authentication (który będzie działał jako serwer RADIUS) za pośrednictwem usługi RADIUS.
- Użyj tego samego wspólnego klucza tajnego, który został skonfigurowany powyżej.
- Limit czasu usługi RADIUS ustaw na 30–60 sekund, aby zapewnić czas na przeprowadzenie walidacji poświadczeń użytkownika, przeprowadzenie uwierzytelniania wieloskładnikowego, odebranie odpowiedzi i przesłanie odpowiedzi na żądanie dostępu usługi RADIUS.



<!--HONumber=Sep16_HO3-->


