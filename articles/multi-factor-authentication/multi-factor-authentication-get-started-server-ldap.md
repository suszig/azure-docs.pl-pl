---
title: "Uwierzytelnianie LDAP i usługa Serwer Azure Multi-Factor Authentication"
description: "Ta strona jest poświęcona tematyce uwierzytelniania wieloskładnikowego na platformie Azure i zawiera informacje pomocne podczas wdrażania uwierzytelniania LDAP i korzystania z usługi Serwer Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: e1a68568-53d1-4365-9e41-50925ad00869
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/04/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 1c57047f9a7eefcc0c4dbf08e705c3193e89f9a7


---
# <a name="ldap-authentication-and-azure-multifactor-authentication-server"></a>Uwierzytelnianie LDAP i usługa Serwer Azure Multi-Factor Authentication
Domyślnie usługa Serwer Azure Multi-Factor Authentication jest skonfigurowana pod kątem importowania lub synchronizowania użytkowników z usługi Active Directory. Można ją jednak skonfigurować pod kątem powiązań z różnymi katalogami LDAP, takimi jak katalog ADAM, lub z określonym kontrolerem domeny usługi Active Directory. Usługę Serwer Azure Multi-Factor Authentication skonfigurowana pod kątem nawiązywania połączenia z katalogiem za pośrednictwem protokołu LDAP można skonfigurować w taki sposób, aby pełniła rolę serwera proxy LDAP wykorzystywanego do przeprowadzania uwierzytelnień. Umożliwia ona również korzystanie z powiązania LDAP jako elementu docelowego protokołu RADIUS, wstępne uwierzytelnianie użytkowników przy użyciu uwierzytelniania w usługach IIS lub uwierzytelnianie podstawowe w portalu dla użytkowników usługi Azure Multi-Factor Authentication.

W przypadku użycia usługi Azure Multi-Factor Authentication jako serwera proxy LDAP jej serwer zostaje umieszczony pomiędzy klientem LDAP (takim jak urządzenie sieci VPN lub aplikacja) i serwerem katalogu LDAP, aby można było dodać uwierzytelnianie wieloskładnikowe. Aby działanie usługi Azure Multi-Factor Authentication było możliwe, jej serwer musi zostać skonfigurowany pod kątem komunikacji zarówno z serwerami klienta, jak i z katalogiem LDAP. W tej konfiguracji serwer usługi Azure Multi-Factor Authentication akceptuje żądania LDAP od serwerów i aplikacji klientów i przekazuje je do docelowego serwera katalogu LDAP w celu sprawdzania poprawności podstawowych poświadczeń. Jeśli odpowiedź z katalogu LDAP wskazuje, że podstawowe poświadczenia są prawidłowe, usługa Azure Multi-Factor Authentication przeprowadza uwierzytelnianie za pomocą drugiego składnika i wysyła odpowiedź z powrotem do klienta LDAP. Cały proces uwierzytelniania powiedzie się tylko wtedy, jeśli pomyślnie przebiegnie zarówno uwierzytelnianie na serwerze LDAP, jak i uwierzytelnianie wieloskładnikowe.

## <a name="ldap-authentication-configuration"></a>Konfigurowanie uwierzytelniania LDAP
Aby skonfigurować uwierzytelnianie LDAP, zainstaluj usługę Serwer Azure Multi-Factor Authentication na serwerze z systemem Windows. Postępuj zgodnie z następującą procedurą:

1. W usłudze Serwer Azure Multi-Factor Authentication kliknij ikonę uwierzytelniania LDAP w menu po lewej stronie.
2. Zaznacz pole wyboru Włącz uwierzytelnianie LDAP.![Uwierzytelnianie LDAP](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)
3. Zmień port TCP i port SSL na karcie Klienci, jeśli usługa LDAP Azure Multi-Factor Authentication powinna być powiązana z portami niestandardowymi w celu umożliwienia nasłuchiwania żądań LDAP od klientów, które zostaną skonfigurowane.
4. Jeśli planujesz korzystanie z protokołu LDAPS z klienta na serwerze usługi Azure Multi-Factor Authentication, należy zainstalować certyfikat SSL na serwerze, na którym jest uruchomiony serwer usługi. Kliknij przycisk Przeglądaj... znajdujący się obok pola certyfikatu SSL i wybierz zainstalowany certyfikat, który będzie używany do nawiązywania bezpiecznego połączenia.
5. Kliknij przycisk Dodaj...
6. W oknie dialogowym Dodawanie klienta LDAP wprowadź adres IP urządzenia, serwera lub aplikacji, które będą uwierzytelniane na serwerze, oraz (opcjonalnie) nazwę aplikacji. Nazwa aplikacji jest widoczna w raportach usługi Azure Multi-Factor Authentication i może być wyświetlana w uwierzytelniających wiadomościach SMS lub wiadomościach aplikacji mobilnej.
7. Zaznacz pole Wymagaj dopasowania użytkownika usługi Azure Multi-Factor Authentication, jeśli wszyscy użytkownicy zostali lub zostaną zaimportowani na serwer i będą podlegać uwierzytelnianiu wieloskładnikowemu. Jeśli znaczna liczba użytkowników nie została jeszcze zaimportowana na serwer lub będzie wykluczona z uwierzytelniania wieloskładnikowego, należy pozostawić to pole puste. Zapoznaj się z plikiem pomocy, aby uzyskać dodatkowe informacje na temat tej funkcji.
8. Możesz powtórzyć kroki od 5 do 7, aby dodać dodatkowe klienty LDAP.
9. Gdy usługa Azure Multi-Factor Authentication jest skonfigurowana pod kątem odbierania uwierzytelnień LDAP, musi ona pełnić rolę serwera proxy przesyłającego te uwierzytelnienia do katalogu LDAP. W związku z tym na karcie Cel wyświetlana jest tylko jedna, wyszarzona opcja użycia obiektu docelowego LDAP. Aby skonfigurować połączenie z katalogiem LDAP, kliknij ikonę integracji katalogu.
10. Na karcie Ustawienia wybierz przycisk radiowy Użyj określonej konfiguracji LDAP.
11. Kliknij przycisk Edytuj...
12. W oknie dialogowym Edycja konfiguracji LDAP wypełnij pola informacjami wymaganymi do nawiązania połączenia z katalogiem LDAP. W poniższej tabeli znajdują się opisy pól. Uwaga: te informacje znajdują się również w pliku pomocy usługi Serwer Azure Multi-Factor Authentication.![Integracja katalogu](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)
13. Przetestuj połączenie LDAP, klikając przycisk Testuj.
14. Jeśli test połączenia LDAP zakończył się pomyślnie, kliknij przycisk OK.
15. Kliknij kartę Filtry. Serwer jest wstępnie skonfigurowany w sposób umożliwiający załadowanie kontenerów, grup zabezpieczeń i użytkowników z katalogu Active Directory. W przypadku konieczności utworzenia powiązania z innym katalogiem LDAP prawdopodobnie będzie konieczna edycja wyświetlanych filtrów. Kliknij łącze Pomoc, aby uzyskać więcej informacji o filtrach.
16. Kliknij kartę Atrybuty. Serwer jest wstępnie skonfigurowany pod kątem mapowania atrybutów z katalogu Active Directory.
17. W przypadku konieczności utworzenia powiązania z innym katalogiem LDAP lub zmiany wstępnie skonfigurowanego mapowania atrybutów należy kliknąć przycisk Edytuj...
18. W oknie dialogowym Edycja atrybutów zmodyfikuj mapowanie atrybutu LDAP katalogu. Nazwy atrybutów można wpisać lub wybrać poprzez kliknięcie przycisku ... znajdującego się obok każdego pola.
19. Kliknij łącze Pomoc, aby uzyskać więcej informacji o atrybutach.
20. Kliknij przycisk OK.
21. Kliknij ikonę Ustawienia firmy, a następnie wybierz kartę Rozpoznawanie nazwy użytkownika.
    22. W przypadku nawiązywania połączenia z katalogiem Active Directory z serwera przyłączonego do domeny powinno być możliwe pozostawienie zaznaczonego przycisku radiowego Użyj identyfikatorów zabezpieczeń systemu Windows (SID) w celu dopasowania nazw użytkowników. W przeciwnym razie wybierz przycisk radiowy Użyj atrybutu unikatowego identyfikatora LDAP w celu dopasowania nazw użytkowników. W przypadku wybrania tej opcji usługa Serwer Azure Multi-Factor Authentication próbuje rozpoznać unikatowy identyfikator w katalogu LDAP na podstawie każdej z nazw użytkowników. Wyszukiwanie LDAP zostanie przeprowadzone z uwzględnieniem atrybutów nazwy użytkownika określonych w obszarze Integracja katalogu na karcie Atrybuty. Podczas uwierzytelniania użytkownika na podstawie jego nazwy użytkownika zostanie rozpoznany unikatowy identyfikator z katalogu LDAP, który zostanie następnie użyty do dopasowania użytkownika w pliku danych usługi Azure Multi-Factor Authentication. Umożliwia to porównywanie nazw bez względu na wielkość liter, jak również korzystanie z długich i krótkich nazw użytkowników. Konfiguracja usługi Serwer Azure Multi-Factor Authentication jest ukończona. Po przeprowadzeniu tych czynności serwer nasłuchuje żądań dostępu LDAP ze skonfigurowanych klientów w ramach skonfigurowanych portów; jest także ustawiony jako serwer proxy przesyłający wspomniane żądania do katalogu LDAP w celu przeprowadzenia uwierzytelniania.

## <a name="ldap-client-configuration"></a>Konfigurowanie klientów LDAP
Aby skonfigurować klienta LDAP, skorzystaj z poniższych wytycznych:

* Skonfiguruj urządzenie, serwer lub aplikację pod kątem uwierzytelniania z użyciem protokołu LDAP na serwerze usługi Azure Multi-Factor Authentication, tak jakby był to katalog LDAP. Należy użyć tych samych ustawień, które zwykle są używane do bezpośredniego połączenia się z katalogiem LDAP, z wyjątkiem nazwy serwera lub adresu IP, które będą odpowiadać danym serwera usługi Azure Multi-Factor Authentication.
* Należy skonfigurować limit czasu protokołu LDAP na 30–60 sekund, aby umożliwić sprawdzenie poprawności poświadczeń użytkownika w katalogu LDAP, przeprowadzenie uwierzytelniania za pomocą drugiego składnika, odebranie odpowiedzi oraz odpowiedzenie na żądania dostępu LDAP.
* W przypadku użycia protokołu LDAPS urządzenie lub serwer, z którego pochodzą zapytania protokołu LDAP, muszą ufać certyfikatowi SSL zainstalowanemu na serwerze usługi Azure Multi-Factor Authentication.




<!--HONumber=Nov16_HO2-->


