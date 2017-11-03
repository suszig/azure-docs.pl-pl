---
title: "Włączanie Azure serwera Proxy aplikacji usługi AD w klasycznym portalu | Dokumentacja firmy Microsoft"
description: "Włącz serwer proxy aplikacji w klasycznym portalu Azure i zainstaluj łączniki dla zwrotnego serwera proxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: ea97fdc8d146ed524a932018b572ceda0982738b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="enable-application-proxy-in-the-classic-portal-and-download-connectors"></a>Włącz serwer Proxy aplikacji w portalu klasycznym i pobierania łączników
W tym artykule przedstawiono procedurę włączania serwera proxy aplikacji usługi Microsoft Azure AD dla katalogu w chmurze w ramach usługi Azure AD.

Jeśli nie znasz możliwości serwera proxy aplikacji, zapoznaj się z artykułem [How to provide secure remote access to on-premises applications](active-directory-application-proxy-get-started.md) (Zapewnianie bezpiecznego dostępu zdalnego do aplikacji lokalnych).

## <a name="application-proxy-prerequisites"></a>Wymagania wstępne serwera proxy aplikacji
Wymagania umożliwiające włączenie i używanie usług serwera proxy aplikacji:

* [Subskrypcja usługi Microsoft Azure AD w warstwie Podstawowa lub Premium](active-directory-editions.md) i katalog usługi Azure AD, dla którego jesteś administratorem globalnym.
* Serwer z systemem Windows Server 2012 R2 lub 2016, w którym można zainstalować łącznik serwera Proxy aplikacji. Serwer wysyła żądania do usług serwera proxy aplikacji w chmurze i wymaga połączenia HTTP i HTTPS do aplikacji, które publikujesz.
  * Na potrzeby logowania jednokrotnego do opublikowanej aplikacji ta maszyna powinny być przyłączona do domeny w tej samej domenie usług AD co publikowane aplikacje. Aby uzyskać informacje, zobacz [rejestracji jednokrotnej z serwerem Proxy aplikacji](active-directory-application-proxy-sso-using-kcd.md)
* Jeśli organizacja używa serwerów proxy, aby nawiązać połączenie z Internetem, przeczytaj [pracy przy użyciu istniejących lokalnych serwerów proxy](application-proxy-working-with-proxy-servers.md) szczegółowe informacje na temat konfigurowania ich.

## <a name="open-your-ports"></a>Otwieranie portów sieci

Aby przygotować swoje środowisko dla serwera Proxy aplikacji usługi Azure AD, należy najpierw włączyć komunikację w centrach danych platformy Azure. Jeśli na ścieżce znajduje się zapora, upewnij się, że jest otwarta, aby umożliwić łącznikowi wysyłanie żądań protokołu HTTPS (TCP) do serwera proxy aplikacji.

1. Otwórz następujące porty, aby **wychodzących** ruchu:

   | Numer portu | Jak jest używany |
   | --- | --- |
   | 80 | Pobieranie odwołania certyfikatu list podczas sprawdzania poprawności certyfikatu SSL |
   | 443 | Cała komunikacja wychodząca z usługą serwera Proxy aplikacji |

   Jeśli zapora wymusza ruch odpowiednio dla użytkowników generujących ruch, otwórz te porty dla ruchu przychodzącego z usług systemu Windows pracujących w ramach konta Usługa sieciowa.

   > [!IMPORTANT]
   > Tabela odzwierciedla wymagania dotyczące portów dla wersji łącznika 1.5.132.0 i nowszych. Jeśli nadal masz starszą wersję łącznika, należy włączyć następujące porty: 5671, 8080 9090, 9091, 9350, 9352 i 10100 — 10120.
   >
   >Aby uzyskać informacje o aktualizowaniu łączniki do najnowszej wersji, zobacz [łączniki serwera Proxy aplikacji usługi AD zrozumieć Azure](application-proxy-understand-connectors.md#automatic-updates).

2. Jeśli zapora lub serwer proxy zezwala na listę dozwolonych podobnej DNS, możesz połączeń dozwolonych msappproxy.net i servicebus.windows.net. Jeśli nie, musisz zezwolić na dostęp do [zakresy IP centrum danych Azure](https://www.microsoft.com/download/details.aspx?id=41653), które są aktualizowane co tydzień.

3. Użyj [Azure AD aplikacji serwera Proxy łącznika porty testu narzędzia](https://aadap-portcheck.connectorporttest.msappproxy.net/) można zweryfikować, że Twoje łącznika może nawiązać połączenie usługi Serwer Proxy aplikacji. Co najmniej upewnij się, że region środkowe stany USA i najbliższego region jest wszystkich zielony zaznaczeń. Ponadto więcej zaznaczeń zielony oznacza większą elastyczność.

## <a name="enable-application-proxy-in-azure-ad"></a>Włącz serwer Proxy aplikacji w usłudze Azure AD
1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/) jako administrator.
2. Przejdź do obszaru Usługa Active Directory i wybierz katalog, w którym chcesz włączyć serwer proxy aplikacji.

    ![Active Directory — ikona](./media/active-directory-application-proxy-enable/ad_icon.png)
3. Wybierz pozycję **Konfiguruj** na stronie katalogu, a następnie przewiń w dół do pozycji **Serwer proxy aplikacji**.
4. Przełącz pozycję **Włącz usługi serwera proxy aplikacji dla tego katalogu** na wartość **Włączone**.

    ![Włączanie serwer proxy aplikacji](./media/active-directory-application-proxy-enable/app_proxy_enable.png)
5. Wybierz polecenie **Pobierz teraz**. **Azure AD aplikacji pobieranie łącznika serwera Proxy** otwiera. Przeczytaj i zaakceptuj postanowienia licencyjne, a następnie kliknij pozycję **Pobierz**, aby zapisać plik Instalatora Windows (exe) łącznika.

## <a name="install-and-register-the-connector"></a>Instalowanie i rejestrowanie łącznika
1. Uruchom plik **AADApplicationProxyConnectorInstaller.exe** na serwerze przygotowanym zgodnie z wymaganiami wstępnymi.
2. Postępuj zgodnie z instrukcjami w kreatorze, aby przeprowadzić instalację.
3. Podczas instalacji wyświetlany jest monit o zarejestrowanie łącznika serwera proxy aplikacji dzierżawy usługi Azure AD.

   * Podaj poświadczenia administratora globalnego usługi Azure AD. Administrator globalny dzierżawy może mieć inne poświadczenia platformy Microsoft Azure niż Twoje.
   * Upewnij się, że administrator, który rejestruje łącznik, jest w tym samym katalogu, w którym włączono usługę serwera proxy aplikacji. Na przykład jeśli domena dzierżawy to contoso.com, administratorem powinien być admin@contoso.com lub dowolny alias w tej domenie.
   * Jeśli **Konfiguracja zwiększonych zabezpieczeń** ustawiono **na** na serwerze, na ekranie rejestracji może zostać zablokowany. Aby zezwolić na dostęp, postępuj zgodnie z instrukcjami w komunikacie o błędzie. Upewnij się, że pozycja Konfiguracja zwiększonych zabezpieczeń programu Internet Explorer jest wyłączona.
   * Jeśli rejestracja łącznika nie powiedzie się, zobacz [Troubleshoot Application Proxy](active-directory-application-proxy-troubleshoot.md) (Rozwiązywanie problemów z serwerem proxy aplikacji).  
4. Po zakończeniu instalacji na serwerze zostaną dodane dwie nowe usługi:

   * **Łącznik serwera proxy aplikacji usługi Microsoft AAD** umożliwia łączność

     * **Aktualizator łącznika serwera Proxy aplikacji usługi AAD Microsoft** jest automatyczna usługa aktualizacji. Okresowo sprawdza dostępność nowych wersji łącznika i aktualizuje go w razie potrzeby.

     ![Usługi łącznika serwera proxy aplikacji — zrzut ekranu](./media/active-directory-application-proxy-enable/app_proxy_services.png)
5. Kliknij przycisk **Zakończ** w oknie instalacji.

Aby uzyskać informacje dotyczące łączników, zobacz [Understand Azure AD Application Proxy connectors](application-proxy-understand-connectors.md) (Objaśnienie łączników serwera proxy aplikacji usługi Azure AD).

Aby zapewnić wysoką dostępność, należy wdrożyć co najmniej dwa łączniki. Aby wdrożyć więcej łączników, powtórz kroki 2 i 3. Każdy łącznik musi zostać zarejestrowany oddzielnie.

Jeśli chcesz odinstalować łącznik, odinstaluj usługę łącznika i usługę aktualizacji. Uruchom ponownie komputer, aby całkowicie usunąć usługę.

## <a name="next-steps"></a>Następne kroki
Teraz można przystąpić do [publikowania aplikacji za pomocą serwera proxy aplikacji](active-directory-application-proxy-publish.md).

Jeśli masz aplikacje znajdujące się w oddzielnych sieciach lub w różnych lokalizacjach, możesz zorganizować różne łączniki w jednostki logiczne za pomocą grup łączników. Dowiedz się więcej o [pracy z łącznikami serwera proxy aplikacji](active-directory-application-proxy-connectors.md).
