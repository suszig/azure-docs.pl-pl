---
title: "Włączanie serwera proxy aplikacji usługi Azure AD | Microsoft Docs"
description: "Włącz serwer proxy aplikacji w klasycznym portalu Azure i zainstaluj łączniki dla zwrotnego serwera proxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/19/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: d0a5cfe8fe9782cc9b75392d6f21965430ee2347


---
# <a name="enable-application-proxy-in-the-azure-portal"></a>Włączanie serwera proxy aplikacji w witrynie Azure Portal
W tym artykule przedstawiono procedurę włączania serwera proxy aplikacji usługi Microsoft Azure AD dla katalogu w chmurze w ramach usługi Azure AD.

Jeśli nie znasz możliwości serwera proxy aplikacji, zapoznaj się z artykułem [How to provide secure remote access to on-premises applications](active-directory-application-proxy-get-started.md) (Zapewnianie bezpiecznego dostępu zdalnego do aplikacji lokalnych).

## <a name="application-proxy-prerequisites"></a>Wymagania wstępne serwera proxy aplikacji
Wymagania umożliwiające włączenie i używanie usług serwera proxy aplikacji:

* [Subskrypcja usługi Microsoft Azure AD w warstwie Podstawowa lub Premium](active-directory-editions.md) i katalog usługi Azure AD, dla którego jesteś administratorem globalnym.
* Serwer z systemem Windows Server 2012 R2, Windows 8.1 lub nowszym, na którym można zainstalować łącznik serwera proxy aplikacji. Serwer wysyła żądania do usług serwera proxy aplikacji w chmurze i wymaga połączenia HTTP i HTTPS do aplikacji, które publikujesz.
  
  * Na potrzeby logowania jednokrotnego do opublikowanej aplikacji ta maszyna powinny być przyłączona do domeny w tej samej domenie usług AD co publikowane aplikacje.
* Jeśli na ścieżce znajduje się zapora, upewnij się, że jest otwarta, aby umożliwić łącznikowi wysyłanie żądań protokołu HTTPS (TCP) do serwera proxy aplikacji. Łącznik używa tych portów razem z poddomenami, które są częścią domen wysokiego poziomu: msappproxy.net i servicebus.windows.net. Upewnij się, że otwarto następujące porty dla ruchu **wychodzącego**:
  
  | Numer portu | Opis |
  | --- | --- |
  | 80 |Zezwolenie na wychodzący ruch protokołu HTTP na potrzeby weryfikacji zabezpieczeń. |
  | 443 |Zezwolenie na uwierzytelnianie użytkownika w usłudze Azure AD (wymagane tylko dla procesu rejestracji łącznika) |
  | 10100–10120 |Zezwolenie na odpowiedzi LOB HTTP wysyłane z powrotem do serwera proxy |
  | 9352, 5671 |Zezwolenie na komunikację od łącznika do usługi platformy Azure na potrzeby żądań przychodzących. |
  | 9350 |Opcjonalnie, aby podwyższyć wydajność obsługi przychodzących żądań |
  | 8080 |Zezwolenie na sekwencję uruchamiania łącznika i automatyczne aktualizacje łącznika |
  | 9090 |Zezwolenie na zarejestrowanie łącznika (wymagane tylko dla procesu rejestracji łącznika) |
  | 9091 |Zezwolenie na automatyczne odnawianie certyfikatu zaufania łącznika |
  
    Jeśli zapora wymusza ruch odpowiednio dla użytkowników generujących ruch, otwórz te porty dla ruchu przychodzącego z usług systemu Windows pracujących w ramach konta Usługa sieciowa. Upewnij się także, że jest włączony port 8080 dla konta Zarządzanie NT\System.
* Jeśli organizacja używa serwerów proxy do łączenia się z Internetem, zobacz wpis w blogu [Working with existing on-premises proxy servers](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/) (Praca z istniejącymi lokalnymi serwerami proxy), aby uzyskać szczegółowe informacje na temat sposobu ich konfigurowania.

## <a name="step-1-enable-application-proxy-in-azure-ad"></a>Krok 1. Włączanie serwera proxy aplikacji w usłudze Azure AD
1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/) jako administrator.
2. Przejdź do obszaru Usługa Active Directory i wybierz katalog, w którym chcesz włączyć serwer proxy aplikacji.
   
    ![Active Directory — ikona](./media/active-directory-application-proxy-enable/ad_icon.png)
3. Wybierz pozycję **Konfiguruj** na stronie katalogu, a następnie przewiń w dół do pozycji **Serwer proxy aplikacji**.
4. Przełącz pozycję **Włącz usługi serwera proxy aplikacji dla tego katalogu** na wartość **Włączone**.
   
    ![Włączanie serwer proxy aplikacji](./media/active-directory-application-proxy-enable/app_proxy_enable.png)
5. Wybierz polecenie **Pobierz teraz**. Nastąpi przekierowanie na stronę **Pobieranie łącznika serwera proxy aplikacji usługi Azure AD**. Przeczytaj i zaakceptuj postanowienia licencyjne, a następnie kliknij pozycję **Pobierz**, aby zapisać plik Instalatora Windows (exe) łącznika.

## <a name="step-2-install-and-register-the-connector"></a>Krok 2. Instalowanie i rejestrowanie łącznika
1. Uruchom plik **AADApplicationProxyConnectorInstaller.exe** na serwerze przygotowanym zgodnie z wymaganiami wstępnymi.
2. Postępuj zgodnie z instrukcjami w kreatorze, aby przeprowadzić instalację.
3. Podczas instalacji zostanie wyświetlony monit o zarejestrowanie łącznika serwera względem serwera proxy aplikacji dzierżawy usługi Azure AD.
   
   * Podaj poświadczenia administratora globalnego usługi Azure AD. Administrator globalny dzierżawy może mieć inne poświadczenia platformy Microsoft Azure niż Twoje.
   * Upewnij się, że administrator, który rejestruje łącznik, jest w tym samym katalogu, w którym włączono usługę serwera proxy aplikacji. Na przykład jeśli domena dzierżawy to contoso.com, administratorem powinien być admin@contoso.com lub dowolny alias w tej domenie.
   * Jeśli **Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer** jest **włączona** na serwerze, na którym jest instalowany łącznik, ekran rejestracji może zostać zablokowany. Wykonaj instrukcje podane w komunikacie o błędzie, aby zezwolić na dostęp. Upewnij się, że pozycja Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer jest wyłączona.
   * Jeśli rejestracja łącznika nie powiedzie się, zobacz [Troubleshoot Application Proxy](active-directory-application-proxy-troubleshoot.md) (Rozwiązywanie problemów z serwerem proxy aplikacji).  
4. Po zakończeniu instalacji na serwerze zostaną dodane dwie nowe usługi:
   
   * **Łącznik serwera proxy aplikacji usługi Microsoft AAD** umożliwia łączność
     
     * **Aktualizator łącznika serwera proxy aplikacji usługi Microsoft AAD** to automatyczna usługa aktualizacji, która okresowo sprawdza dostępność nowych wersji łącznika i aktualizuje go w miarę potrzeby.
     
     ![Usługi łącznika serwera proxy aplikacji — zrzut ekranu](./media/active-directory-application-proxy-enable/app_proxy_services.png)
5. Kliknij przycisk **Zakończ** w oknie instalacji.

Aby zapewnić wysoką dostępność, należy wdrożyć co najmniej dwa łączniki. Aby wdrożyć więcej łączników, powtórz kroki 2 i 3 powyżej. Każdy łącznik musi zostać zarejestrowany oddzielnie.

Jeśli chcesz odinstalować łącznik, odinstaluj usługę łącznika i usługę aktualizacji. Uruchom ponownie komputer, aby całkowicie usunąć usługę.

## <a name="next-steps"></a>Następne kroki
Teraz można przystąpić do [publikowania aplikacji za pomocą serwera proxy aplikacji](active-directory-application-proxy-publish.md).

Jeśli masz aplikacje znajdujące się w oddzielnych sieciach lub w różnych lokalizacjach, możesz zorganizować różne łączniki w jednostki logiczne za pomocą grup łączników. Dowiedz się więcej o [pracy z łącznikami serwera proxy aplikacji](active-directory-application-proxy-connectors.md).




<!--HONumber=Nov16_HO2-->


