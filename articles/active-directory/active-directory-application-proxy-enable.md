<properties
    pageTitle="Włączanie serwera proxy aplikacji usługi Azure AD | Microsoft Azure"
    description="Włącz serwer proxy aplikacji w klasycznym portalu Azure i zainstaluj łączniki dla zwrotnego serwera proxy."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="StevenPo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/01/2016"
    ms.author="kgremban"/>

# Włączanie serwera proxy aplikacji w portalu Azure

W tym artykule przedstawiono włączanie serwera proxy aplikacji usługi Microsoft Azure AD dla katalogu w chmurze w ramach usługi Azure AD. Obejmuje ono instalowanie łącznika serwera proxy aplikacji w sieci prywatnej, co umożliwia utrzymywanie połączenia z Twojej sieci do usługi serwera proxy. Łącznik zostanie również zarejestrowany w ramach subskrypcji dzierżawy usługi Microsoft Azure AD. Jeśli nie znasz możliwości serwera proxy aplikacji, zapoznaj się z artykułem [How to provide secure remote access to on-premises applications](active-directory-application-proxy-get-started.md) (Zapewnianie bezpiecznego dostępu zdalnego do aplikacji lokalnych).

Po ukończeniu tego przewodnika w celu włączenia serwera proxy aplikacji usługi Azure AD użytkownik będzie gotowy do publikowania lokalnych aplikacji na potrzeby zdalnego dostępu.

> [AZURE.NOTE] Serwer proxy aplikacji jest funkcją dostępną tylko w przypadku uaktualnienia do wersji usługi Azure Active Directory w warstwie Premium lub Podstawowa. Aby uzyskać więcej informacji, zobacz [Wersje usługi Azure Active Directory](active-directory-editions.md).

## Wymagania wstępne serwera proxy aplikacji
Wymagania umożliwiające włączenie i używanie usług serwera proxy aplikacji:

- [Subskrypcja usługi Microsoft Azure AD w warstwie Podstawowa lub Premium](active-directory-editions.md) i katalog usługi Azure AD, dla którego jesteś administratorem globalnym.
- Serwer z systemem Windows Server 2012 R2, Windows 8.1 lub nowszym, na którym można zainstalować łącznik serwera proxy aplikacji. Serwer będzie wysyłał żądania HTTPS do usług serwera proxy aplikacji w chmurze i wymaga połączenia HTTPS do aplikacji, które planujesz opublikować.
- Jeśli na ścieżce znajduje się zapora, upewnij się, że jest otwarta, co umożliwia łącznikowi wysyłanie żądań protokołu HTTPS (TCP) do serwera proxy aplikacji. Łącznik używa tych portów w połączeniu z poddomenami, które są częścią domen wysokiego poziomu: *msappproxy.net* i *servicebus.windows.net*. Upewnij się, że otwarto **wszystkie** z następujących portów dla ruchu **wychodzącego**:

  	| Numer portu | Opis |
  	| --- | --- |
  	| 80 | Zezwolenie na wychodzący ruch protokołu HTTP na potrzeby weryfikacji zabezpieczeń. |
  	| 443 | Zezwolenie na uwierzytelnianie użytkownika w usłudze Azure AD (wymagane tylko dla procesu rejestracji łącznika) |
  	| 10100-10120 | Zezwolenie na odpowiedzi LOB HTTP wysyłane z powrotem do serwera proxy |
  	| 9352, 5671 | Zezwolenie na komunikację od łącznika do usługi platformy Azure na potrzeby żądań przychodzących. |
  	| 9350 | Opcjonalnie, aby podwyższyć wydajność obsługi przychodzących żądań |
  	| 8080 | Zezwolenie na sekwencję uruchamiania łącznika i automatyczne aktualizacje łącznika |
  	| 9090 | Zezwolenie na zarejestrowanie łącznika (wymagane tylko dla procesu rejestracji łącznika) |
  	| 9091 | Zezwolenie na automatyczne odnawianie certyfikatu zaufania łącznika |

Jeśli zapora wymusza ruch odpowiednio dla użytkowników generujących ruch, otwórz te porty dla ruchu przychodzącego z usług systemu Windows pracujących w ramach konta Usługa sieciowa. Upewnij się także, że jest włączony port 8080 dla konta Zarządzanie NT\System.


## Krok 1. Włączanie serwera proxy aplikacji w usłudze Azure AD
1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/) jako administrator.
2. Przejdź do obszaru Usługa Active Directory i wybierz katalog, w którym chcesz włączyć serwer proxy aplikacji.

    ![Active Directory — ikona](./media/active-directory-application-proxy-enable/ad_icon.png)

3. Wybierz pozycję **Konfiguruj** na stronie katalogu, a następnie przewiń w dół do pozycji **Serwer proxy aplikacji**.
4. Przełącz pozycję **Włącz usługi serwera proxy aplikacji dla tego katalogu** na wartość **Włączone**.

    ![Włączanie serwer proxy aplikacji](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. Wybierz polecenie **Pobierz teraz**. Nastąpi przekierowanie na stronę **Pobieranie łącznika serwera proxy aplikacji usługi Azure AD**. Przeczytaj i zaakceptuj postanowienia licencyjne, a następnie kliknij pozycję **Pobierz**, aby zapisać plik Instalatora Windows (exe) łącznika serwera proxy aplikacji.

## Krok 2. Instalowanie i rejestrowanie łącznika
1. Uruchom plik *AADApplicationProxyConnectorInstaller.exe* na serwerze przygotowanym zgodnie z powyższymi wymaganiami wstępnymi.
2. Postępuj zgodnie z instrukcjami w kreatorze, aby przeprowadzić instalację.
3. Podczas instalacji zostanie wyświetlony monit o zarejestrowanie łącznika serwera względem serwera proxy aplikacji dzierżawy usługi Azure AD.

  - Podaj poświadczenia administratora globalnego usługi Azure AD. Administrator globalny dzierżawy może mieć inne poświadczenia platformy Microsoft Azure niż Twoje.
  - Upewnij się, że administrator rejestrujący łącznik jest w tym samym katalogu, w którym włączono usługę serwera proxy aplikacji. Na przykład jeśli domena dzierżawy to contoso.com, administrator powinien mieć konto admin@contoso.com lub dowolny inny alias w tej domenie.
  - Jeśli **Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer** jest **włączona** na serwerze, na którym jest instalowany łącznik usługi Azure AD, ekran rejestracji może zostać zablokowany. Jeśli to się zdarzy, wykonaj instrukcje podane w komunikacie o błędzie, aby zezwolić na dostęp. Upewnij się, że pozycja Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer jest wyłączona.
  - Jeśli rejestracja łącznika nie powiedzie się, zobacz [Troubleshoot Application Proxy](active-directory-application-proxy-troubleshoot.md) (Rozwiązywanie problemów z serwerem proxy aplikacji).  

4. Po zakończeniu instalacji na serwerze zostaną dodane dwie nowe usługi jak pokazano poniżej.

    - **Łącznik serwera proxy aplikacji usługi Microsoft AAD** umożliwia łączność
    - **Aktualizator łącznika serwera proxy aplikacji usługi Microsoft AAD** to automatyczna usługa aktualizacji, która okresowo sprawdza dostępność nowych wersji łącznika i aktualizuje go w miarę potrzeby.

    ![Usługi łącznika serwera proxy aplikacji — zrzut ekranu](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. Kliknij przycisk **Zakończ** w oknie instalacji, aby zakończyć instalację.

Teraz można przystąpić do [publikowania aplikacji za pomocą serwera proxy aplikacji](active-directory-application-proxy-publish.md).

Aby zapewnić wysoką dostępność, należy wdrożyć co najmniej jeden dodatkowy łącznik. Aby wdrożyć więcej łączników, powtórz kroki 2 i 3 powyżej. Każdy łącznik musi zostać zarejestrowany oddzielnie.

Jeśli chcesz odinstalować łącznik, odinstaluj usługę łącznika i usługę aktualizacji, a następnie uruchom ponownie komputer w celu całkowitego usunięcia usługi.


## Następne kroki

- [Publikowanie aplikacji za pomocą serwera proxy aplikacji](active-directory-application-proxy-publish.md)
- [Publikowanie aplikacji przy użyciu własnej nazwy domeny](active-directory-application-proxy-custom-domains.md)
- [Włączanie logowania jednokrotnego](active-directory-application-proxy-sso-using-kcd.md)
- [Rozwiązywanie problemów z serwerem proxy aplikacji](active-directory-application-proxy-troubleshoot.md)

Aby zapoznać się z najnowszymi informacjami i aktualizacjami, zobacz [blog dotyczący serwera proxy aplikacji](http://blogs.technet.com/b/applicationproxyblog/)



<!--HONumber=Jun16_HO2-->


