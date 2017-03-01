---
title: "Usługa sieci Web aplikacji mobilnej serwera usługi Azure MFA | Microsoft Docs"
description: "Aplikacja Microsoft Authenticator oferuje dodatkową opcję uwierzytelniania poza pasmem.  Aplikacja umożliwia serwerowi MFA obsługę powiadomień wypychanych dla użytkowników."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 6c8d6fcc-70f4-4da4-9610-c76d66635b8b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/15/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 999361daa2faebe3e88cab0b6085a938d6f40e9d
ms.openlocfilehash: d3a122d7d26635e13281b1cba450937519ed4be6


---
# <a name="getting-started-with-the-mfa-server-mobile-app-web-service"></a>Wprowadzenie do usługi sieci Web aplikacji mobilnej serwera usługi MFA
Aplikacja Microsoft Authenticator oferuje dodatkową opcję weryfikacji poza pasmem. Zamiast wykonywać automatyczne połączenia telefoniczne lub wysyłać wiadomości SMS do użytkownika podczas logowania, usługa Azure Multi-Factor Authentication wypycha powiadomienia do aplikacji Microsoft Authenticator na smartfonie lub tablecie użytkownika. Wystarczy, że użytkownik naciśnie pozycję **Weryfikuj** (lub wprowadzi numer PIN i naciśnie pozycję „Uwierzytelnij”) w aplikacji, aby się zalogować. 

Korzystanie z aplikacji mobilnej w celu weryfikacji dwuetapowej jest preferowane, jeśli zasięg telefonu jest niestabilny. Jeśli używasz aplikacji jako generatora tokenów OATH, nie wymaga ona żadnego połączenia z siecią ani Internetem. 

Zainstalowanie portalu użytkowników na serwerze innym niż serwer usługi Azure Multi-Factor Authentication wymaga wykonania następujących kroków:

1. Instalowanie zestawu SDK usługi sieci Web
2. Instalacja usługi sieci Web aplikacji mobilnej
3. Skonfigurowanie ustawień aplikacji mobilnej na serwerze usługi Azure Multi-Factor Authentication
4. Aktywowanie aplikacji Microsoft Authenticator dla użytkowników końcowych

## <a name="requirements"></a>Wymagania

Aby korzystać z aplikacji Microsoft Authenticator, wymagane jest spełnienie następujących warunków w celu umożliwienia skutecznej komunikacji aplikacji z usługą sieci Web aplikacji mobilnej:

* Korzystanie z serwera usługi Azure Multi-Factor Authentication w wersji&6;.0 lub nowszej.
* Zainstalowanie usługi sieci Web aplikacji mobilnej na dostępnym z Internetu serwerze sieci Web z uruchomionymi usługami Microsoft® [Internet Information Services (IIS) w wersji 7.x lub nowszej](http://www.iis.net/).
* Zainstalowanie i zarejestrowanie programu ASP.NET w wersji&4;.0.30319 oraz wybranie dla niego opcji Dozwolone.
* Wymagane usługi ról obejmują usługę ASP.NET oraz usługę zgodności z metabazą usług IIS 6.
* Udostępnienie usługi sieci Web aplikacji mobilnej za pośrednictwem publicznego adresu URL.
* Zabezpieczenie usługi sieci Web aplikacji mobilnej za pomocą certyfikatu SSL.
* Zainstalowanie zestawu SDK usługi sieci Web usługi Azure Multi-Factor Authentication w usługach IIS w wersji 7.x lub nowszej na tym samym serwerze, na którym zainstalowano serwer usługi Azure Multi-Factor Authentication.
* Zabezpieczenie zestawu SDK usługi sieci Web usługi Azure Multi-Factor Authentication za pomocą certyfikatu SSL.
* Zapewnienie możliwości nawiązania połączenia między usługą sieci Web aplikacji mobilnej i zestawem SDK usługi sieci Web usługi Azure Multi-Factor Authentication za pośrednictwem protokołu SSL.
* Zapewnienie możliwości wykonywania uwierzytelniania usługi sieci Web aplikacji mobilnej w zestawie SDK sieci Web usługi Azure MFA przy użyciu poświadczeń konta usługi, które należy do grupy zabezpieczeń „PhoneFactor Admins”. To konto usługi i ta grupa istnieją w usłudze Active Directory, jeśli serwer usługi Azure Multi-Factor Authentication znajduje się na serwerze przyłączonym do domeny. To konto usługi i ta grupa istnieją lokalnie na serwerze usługi Azure Multi-Factor Authentication, jeśli nie jest on przyłączony do domeny.


## <a name="install-the-web-service-sdk"></a>Instalowanie zestawu SDK usługi sieci Web
Jeśli zestaw SDK usługi sieci Web usługi Azure Multi-Factor Authentication nie został jeszcze zainstalowany na serwerze usługi Azure Multi-Factor Authentication (MFA), przejdź na ten serwer i otwórz okno serwera usługi Azure MFA. 

1. Kliknij ikonę zestawu SDK usługi sieci Web.
2. Kliknij przycisk **Zainstaluj zestaw SDK usługi sieci Web** i wykonaj wyświetlone instrukcje. 

Zestaw SDK usługi sieci Web musi zostać zabezpieczony za pomocą certyfikatu SSL. W tym celu wystarczy certyfikat z podpisem własnym. Zaimportuj certyfikat do magazynu „Zaufane główne urzędy certyfikacji” konta komputera lokalnego na serwerze sieci Web portalu użytkowników, dzięki czemu magazyn uzna ten certyfikat za zaufany podczas inicjowania połączenia SSL.

![Konfiguracja](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)

## <a name="install-the-mobile-app-web-service"></a>Instalacja usługi sieci Web aplikacji mobilnej
Przed zainstalowaniem usługi sieci Web aplikacji mobilnej pamiętaj o następujących szczegółach:

* Jeśli portal użytkowników usługi Azure MFA jest już zainstalowany na serwerze dostępnym z Internetu, nazwa użytkownika, hasło i adres URL powiązane z zestawem SDK usługi sieci Web mogą zostać skopiowane z pliku web.config portalu użytkowników.
* Warto otworzyć przeglądarkę sieci Web na dostępnym z Internetu serwerze sieci Web i przejść do adresu URL zestawu SDK usługi sieci Web wprowadzonego w pliku web.config. Jeśli przeglądarka pomyślnie uzyska dostęp do usługi sieci Web, zostanie wyświetlony monit o poświadczenia. Wprowadź nazwę użytkownika i hasło wprowadzone w pliku web.config w dokładnie takiej samej postaci, w jakiej występują w pliku. Upewnij się, że nie są wyświetlane żadne ostrzeżenia ani błędy dotyczące certyfikatów.
* Jeśli zwrotny serwer proxy lub zapora znajdują się przed serwerem sieci Web usługi sieci Web aplikacji mobilnej i odpowiadają za odciążanie protokołu SSL, można edytować plik web.config usługi sieci Web aplikacji mobilnej tak, aby usługa sieci Web aplikacji mobilnej mogła korzystać z protokołu http zamiast https. Protokół SSL jest nadal wymagany w celu nawiązania komunikacji między aplikacją mobilną i zaporą/zwrotnym serwerem proxy. Dodaj następujący klucz do sekcji \<appSettings\>: 

        <add key="SSL_REQUIRED" value="false"/>

### <a name="install-the-service"></a>Instalowanie usługi

1. Otwórz Eksploratora Windows na serwerze usługi Azure Multi-Factor Authentication i przejdź do folderu, w którym zainstalowano serwer usługi Azure MFA (zazwyczaj C:\Program Files\Azure Multi-Factor Authentication). Wybierz wersję 32- lub 64-bitową pliku instalacyjnego Azure Multi-Factor AuthenticationPhoneAppWebServiceSetup. Skopiuj plik instalacyjny na serwer dostępny z Internetu.

2. Na serwerze sieci Web dostępnym z Internetu uruchom plik instalacyjny z uprawnieniami administratora. Otwórz wiersz polecenia jako administrator i przejdź do lokalizacji, do której został skopiowany plik instalacyjny.

3. Uruchom plik instalacyjny Multi-Factor AuthenticationMobileAppWebServiceSetup, w miarę potrzeby zmieniając miejsce instalacji, a następnie zmień nazwę katalogu wirtualnego na krótką, taką jak „PA”.

  Zaleca się użycie krótkiej nazwy katalogu wirtualnego, ponieważ użytkownik musi podczas aktywacji wprowadzić adres URL usługi sieci Web aplikacji mobilnej w urządzeniach przenośnych.

4. Po zakończeniu instalacji pliku Azure Multi-Factor AuthenticationMobileAppWebServiceSetup przejdź do lokalizacji C:\inetpub\wwwroot\PA (lub odpowiedniego katalogu określonego na podstawie nazwy katalogu wirtualnego) i dokonaj edycji pliku web.config. 

5. Znajdź klucze WEB_SERVICE_SDK_AUTHENTICATION_USERNAME i WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD. Jako wartości ustaw nazwę użytkownika i hasło konta usługi należącego do grupy zabezpieczeń PhoneFactor Admins. Może to być to samo konto, które jest używane jako tożsamość portalu użytkowników usługi Multi-Factor Authentication, jeśli zostało ono wcześniej zainstalowane. Wprowadź na końcu wiersza nazwę użytkownika i hasło w cudzysłowie (value=””/>). Użyj kwalifikowanej nazwy użytkownika, np. domena\nazwa_użytkownika lub maszyna\nazwa_użytkownika.  

6. Znajdź ustawienie pfMobile App Web Service_pfwssdk_PfWsSdk. Zmień wartość z *http://localhost:4898/PfWsSdk.asmx* na adres URL zestawu SDK usługi sieci Web uruchomionego na serwerze usługi Azure Multi-Factor Authentication (np. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). 

  Ponieważ na potrzeby tego połączenia jest używany protokół SSL, do zestawu SDK usługi sieci Web musisz się odwoływać za pomocą nazwy serwera, a nie adresu IP. Certyfikat SSL będzie wystawiony dla nazwy serwera, a używany adres URL musi być zgodny z nazwą w certyfikacie. Nazwa serwera nie może być rozpoznawana jako powiązana z adresem IP z serwera dostępnego z Internetu. Jeśli tak się dzieje, dodaj odpowiedni wpis w pliku hosts na danym serwerze, aby zamapować nazwę serwera usługi Azure Multi-Factor Authentication na jej adres IP. Po dokonaniu zmian zapisz plik web.config.

7. Jeśli witryna sieci Web, na której została zainstalowana usługa sieci Web aplikacji mobilnej nie została jeszcze powiązana z certyfikatem z podpisem publicznym, zainstaluj certyfikat na serwerze, otwórz Menedżera usług IIS i powiąż certyfikat z witryną sieci Web.

8. Otwórz przeglądarkę sieci Web z dowolnego komputera i przejdź do adresu URL, pod którym zainstalowano usługę sieci Web aplikacji mobilnej (np. https://www.publicznawitrynasieciweb.com/PA). Upewnij się, że nie są wyświetlane żadne ostrzeżenia ani błędy dotyczące certyfikatów.

### <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Skonfigurowanie ustawień aplikacji mobilnej na serwerze usługi Azure Multi-Factor Authentication
Po zainstalowaniu usługi sieci Web aplikacji mobilnej należy skonfigurować serwer usługi Azure Multi-Factor Authentication do pracy z portalem.

1. Na serwerze usługi Azure MFA kliknij ikonę portalu użytkowników. Jeśli użytkownicy mogą kontrolować metody ich uwierzytelniania, zaznacz opcję **Aplikacja mobilna** na karcie Ustawienia w obszarze **Zezwalaj użytkownikom na wybór metody**. W przeciwnym razie użytkownicy końcowi będą musieli skontaktować się z działem pomocy technicznej w celu ukończenia procesu aktywacji aplikacji mobilnej.
2. Zaznacz pole **Zezwalaj użytkownikom na uaktywnienie aplikacji mobilnej**.
3. Zaznacz pole **Zezwalaj na rejestrację użytkownika**.
4. Kliknij ikonę aplikacji mobilnej.
5. Wprowadź używany z katalogiem wirtualnym adres URL utworzony podczas instalowania pliku Azure Multi-Factor AuthenticationMobileAppWebServiceSetup. W przeznaczonym do tego celu polu można wprowadzić nazwę konta. Nazwa firmy będzie wyświetlana w aplikacji mobilnej. Jeśli pole pozostanie puste, będzie wyświetlana nazwa dostawcy usługi Multi-Factor Authentication utworzonego w klasycznej witrynie Azure Portal.

<center>![Konfiguracja](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>



<!--HONumber=Feb17_HO3-->


