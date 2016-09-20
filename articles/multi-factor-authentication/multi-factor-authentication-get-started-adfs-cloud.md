<properties 
    pageTitle="Zabezpieczanie zasobów w chmurze przy użyciu usługi Azure Multi-Factor Authentication i usług AD FS" 
    description="Ta strona dotyczy usługi Azure Multi-Factor Authentication i zawiera informacje umożliwiające rozpoczęcie korzystania z usługi Azure MFA i usług AD FS w chmurze." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/04/2016" 
    ms.author="billmath"/>

# Zabezpieczanie zasobów w chmurze przy użyciu usługi Azure Multi-Factor Authentication i usług AD FS

Organizacje, które są sfederowane z użyciem usługi Azure Active Directory i korzystają z zasobów używanych przez usługę Azure AD, mogą je zabezpieczyć za pomocą usługi Azure Multi-Factor Authentication lub usług Active Directory Federation Services Aby zabezpieczyć zasoby usługi Azure Active Directory za pomocą usługi Azure Multi-Factor Authentication lub usług Active Directory Federation Services, postępuj zgodnie z poniższymi procedurami.

## Aby zabezpieczyć zasoby usługi Azure AD za pomocą usług AD FS, wykonaj następujące czynności: 



1. Włącz konta użytkowników, wykonując instrukcje podane w opisie [włączania uwierzytelniania wieloskładnikowego](active-directory/multi-factor-authentication-get-started-cloud.md#turn-on-multi-factor-authentication-for-users)
2. Skonfiguruj regułę oświadczeń za pomocą poniższej procedury:

![Chmura](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)

-   Uruchom konsolę zarządzania usługami AD FS.
-   Przejdź do obszaru Relacje zaufania jednostek zależnych i kliknij prawym przyciskiem myszy pozycję Relacja zaufania jednostek zależnych. Wybierz pozycję Edytuj reguły oświadczeń...
-   Kliknij pozycję Dodaj regułę...
-   Wybierz z listy rozwijanej pozycję Wysyłanie oświadczeń przy użyciu reguły niestandardowej i kliknij przycisk Dalej.
-   Wprowadź nazwę reguły oświadczeń.
-   W obszarze Reguła niestandardowa dodaj następujący kod:


        => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");

    Odpowiadające oświadczenie:

        <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
        <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
        </saml:Attribute>
- Kliknij przycisk OK. Kliknij przycisk Zakończ. Zamknij konsolę zarządzania usługami AD FS.

Użytkownicy będą mogli logować się za pośrednictwem metody lokalnej (na przykład przy użyciu karty inteligentnej).

## Zaufane adresy IP dla użytkowników federacyjnych
Zaufane adresy IP umożliwiają administratorom pomijanie uwierzytelniania wieloskładnikowego w przypadku określonego adresu IP lub użytkowników federacyjnych, którzy wysyłają żądania z firmowej sieci intranet. Poniższe sekcje zawierają instrukcje dotyczące konfigurowania zaufanych adresów IP usługi Azure Multi-Factor Authentication dla użytkowników federacyjnych i pomijania uwierzytelniania wieloskładnikowego w przypadku żądań pochodzących od użytkowników federacyjnych z sieci intranet.  Osiąga się to przez skonfigurowanie usług AD FS pod kątem używania szablonu przekazywania lub szablonu filtrowania oświadczeń przychodzących za pomocą typu oświadczeń wewnętrznej sieci firmowej.  W tym przykładzie użyto usługi Office 365 w celu pokazania obsługi relacji zaufania jednostek zależnych.

### Konfigurowanie reguł oświadczeń usług AD FS

W pierwszej kolejności należy skonfigurować oświadczenia usług AD FS. Utworzymy dwie reguły oświadczeń — jedną dla typu oświadczenia wewnętrznej sieci firmowej, a drugą na potrzeby umożliwienia stałego zalogowania użytkowników.

1. Otwórz przystawkę zarządzania usługami AD FS.
2. Po lewej stronie wybierz pozycję Relacje zaufania jednostek zależnych.
3. W środkowej części kliknij prawym przyciskiem myszy platformę tożsamości usługi Microsoft Office 365 i wybierz pozycję **Edytuj reguły oświadczeń...**
![Chmura](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. Na karcie Reguły przekształcania wystawiania kliknij pozycję **Dodaj regułę.**
![Chmura](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. W Kreatorze dodawania reguły przekształcania oświadczeń wybierz z listy rozwijanej pozycję Przekazywanie lub filtrowanie oświadczenia przychodzącego, a następnie kliknij przycisk Dalej.
![Chmura](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. W polu Nazwa reguły oświadczenia wpisz nazwę reguły, np. WewnSiećFirm.
7. Dla pola Typ oświadczenia przychodzącego wybierz z listy rozwijanej pozycję Wewnątrz sieci firmowej.
![Chmura](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Kliknij przycisk Zakończ.
9. Na karcie Reguły przekształcania wystawiania kliknij pozycję **Dodaj regułę**.
10. W Kreatorze dodawania reguły przekształcania oświadczeń wybierz z listy rozwijanej pozycję Wysyłanie oświadczeń przy użyciu reguły niestandardowej, a następnie kliknij przycisk Dalej.
11. W polu Nazwa reguły oświadczenia wprowadź Nie wylogowuj użytkowników.
12. W polu Reguła niestandardowa wprowadź kod:
        
        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
![Chmura](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Kliknij przycisk **Zakończ**.
14. Kliknij przycisk **Zastosuj**.
15. Kliknij przycisk **OK**.
16. Zamknij przystawkę zarządzania usługami AD FS.



### Konfigurowanie zaufanych adresów IP usługi Azure Multi-Factor Authentication dla użytkowników federacyjnych
Po skonfigurowaniu oświadczeń można przystąpić do konfigurowania zaufanych adresów IP.

1. Zaloguj się do Portalu zarządzania Azure.
2. W obszarze po lewej stronie kliknij pozycję Active Directory.
3. W obszarze Katalog kliknij katalog, dla którego chcesz skonfigurować zaufane adresy IP.
4. W wybranym katalogu kliknij pozycję Konfiguruj.
5. W sekcji uwierzytelniania wieloskładnikowego kliknij pozycję Zarządzaj ustawieniami usługi.
6. Na stronie Ustawienia usługi w obszarze Zaufane adresy IP wybierz pozycję **Aby obsługiwać żądana użytkowników sfederowanych pochodzące z intranetu.**
![Chmura](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. Kliknij pozycję Zapisz.
8. Po zastosowaniu aktualizacji kliknij pozycję Zamknij.


Gotowe. Od tej pory federacyjni użytkownicy usługi Office 365 muszą używać usługi MFA, tylko jeśli ich oświadczenia pochodzą spoza firmowego intranetu.









<!--HONumber=sep16_HO1-->


