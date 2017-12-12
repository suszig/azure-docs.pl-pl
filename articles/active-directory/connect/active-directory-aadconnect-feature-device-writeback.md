---
title: "Azure AD Connect: Włączanie zapisywania zwrotnego urządzeń | Dokumentacja firmy Microsoft"
description: "Ten dokument zawiera szczegóły Włączanie zapisywania zwrotnego urządzeń za pomocą usługi Azure AD Connect"
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: curtand
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: billmath
ms.openlocfilehash: 9c0ff3394dac12bdcac9d618832566ef0d3a6609
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: Włączanie zapisywania zwrotnego urządzeń
> [!NOTE]
> Subskrypcja usługi Azure AD Premium jest wymagana dla zapisu zwrotnego urządzeń.
> 
> 

Poniższa dokumentacja zawiera informacje o sposobie włączania funkcji zapisywania zwrotnego urządzeń w programie Azure AD Connect. Zapisywanie zwrotne urządzeń jest używany w następujących scenariuszach:

* Włączanie dostępu warunkowego, w oparciu o urządzenia do usług AD FS (2012 R2 lub nowszej) chronione aplikacje (relacje zaufania podmiotów zależnych).

Zapewnia to dodatkowe zabezpieczenie i gwarantują, że udzielono dostępu do aplikacji wyłącznie na zaufanych urządzeniach. Aby uzyskać więcej informacji dotyczących dostępu warunkowego, zobacz [zarządzania ryzykiem przy użyciu dostępu warunkowego](../active-directory-conditional-access-azure-portal.md) i [Konfigurowanie lokalnego dostępu warunkowego przy użyciu rejestracji urządzeń usługi Azure Active Directory](../active-directory-conditional-access-automatic-device-registration-setup.md).

> [!IMPORTANT]
> <li>Urządzenia muszą znajdować się w tym samym lesie co użytkowników. Ponieważ urządzenia musi być zapisywane z powrotem do jednego lasu, ta funkcja nie obsługuje obecnie wdrożenia z wieloma lasami użytkownika.</li>
> <li>Obiekt konfiguracji rejestracji tylko jedno urządzenie można dodać do lokalnego lasu usługi Active Directory. Ta funkcja nie jest zgodne z topologią, gdzie jest zsynchronizowany w lokalnej usłudze Active Directory do wielu katalogów usługi Azure AD.</li>> 

## <a name="part-1-install-azure-ad-connect"></a>Część 1: Instalowanie programu Azure AD Connect
1. Zainstaluj program Azure AD Connect przy użyciu niestandardowych albo ustawienia ekspresowe. Firma Microsoft zaleca do wszystkich użytkowników i grup pomyślnie zsynchronizowano przed włączeniem zapisu zwrotnego urządzeń.

## <a name="part-2-prepare-active-directory"></a>Część 2: Przygotowanie usługi Active Directory
Wykonaj następujące kroki, aby przygotować się do przy użyciu zapisywania zwrotnego urządzeń.

1. Z komputera, na którym zainstalowano Azure AD Connect Uruchom program PowerShell w trybie podniesionych uprawnień.
2. Jeśli nie zainstalowano modułu środowiska PowerShell usługi Active Directory, należy zainstalować narzędzia administracji zdalnej serwera, które zawiera moduł AD PowerShell i dsacls.exe co jest wymagane do uruchomienia skryptu.  Uruchom następujące polecenie:
  
   ``` powershell
   Add-WindowsFeature RSAT-AD-Tools
   ```

3. Jeśli nie zainstalowano modułu programu PowerShell usługi Azure Active Directory, następnie Pobierz i zainstaluj go z [Azure Active Directory modułu dla środowiska Windows PowerShell (wersja 64-bitowa)](http://go.microsoft.com/fwlink/p/?linkid=236297). Ten składnik ma zależność na Asystent logowania, która jest instalowana z programem Azure AD Connect.  
4. Przy użyciu poświadczeń administratora przedsiębiorstwa uruchom następujące polecenia, a następnie zamknij programu PowerShell.
   
   ``` powershell
   Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'
   ```

   ``` powershell
   Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}
   ```

Wymagane są poświadczenia administratora przedsiębiorstwa, ponieważ potrzebne będzie wprowadzenie zmian do konfiguracji przestrzeni nazw. Administrator domeny nie ma wystarczających uprawnień.

![Programu PowerShell dla Włączanie zapisywania zwrotnego urządzeń](./media/active-directory-aadconnect-feature-device-writeback/powershell.png) d


Opis:

* Jeśli już istnieje, tworzy i konfiguruje nowe kontenery i obiekty CN = Device Registration Configuration, CN = Services, CN = Configuration, [nazwa wyróżniająca lasu].
* Jeśli już istnieje, tworzy i konfiguruje nowe kontenery i obiekty CN = RegisteredDevices, [nazwa wyróżniająca domeny]. Obiekty urządzeń zostaną utworzone w tym kontenerze.
* Ustawia odpowiednie uprawnienia dla konta łącznika usługi Azure AD, do zarządzania urządzeniami w usłudze Active Directory.
* Musi zostać uruchomiony jedynie w jednym lesie, nawet jeśli Azure AD Connect jest instalowany na wiele lasów.

Parametry:

* DomainName: Domena usługi Active Directory którym obiekty urządzeń zostanie utworzona. Uwaga: Wszystkie urządzenia dla podanego lasu usługi Active Directory zostanie utworzona w jednej domenie.
* AdConnectorAccount: Konto usługi Active Directory, który będzie używany przez program Azure AD Connect do zarządzania obiektami w katalogu. To konto używane przez synchronizacja programu Azure AD Connect na łączenie z usługą AD. Jeśli został zainstalowany przy użyciu ustawień ekspresowych, jest prefiksem MSOL_ konta.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>Część 3: Włącz urządzenia funkcji zapisywania zwrotnego w programie Azure AD Connect
Poniższa procedura umożliwia włączenie funkcji zapisywania zwrotnego urządzeń w programie Azure AD Connect.

1. Uruchom ponownie Kreatora instalacji. Wybierz **Dostosuj opcje synchronizacji** z dodatkowych zadań i kliknij przycisk **dalej**.
   ![Instalacja niestandardowa Dostosuj opcje synchronizacji](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2. Na stronie opcjonalnych funkcji zapisywania zwrotnego urządzeń będą już wyszarzone. Sprawdź należy pamiętać, że jeśli programu Azure AD Connect przygotowywania kroki nie są zapisu zwrotnego urządzeń ukończone będzie wyszarzany limit na stronie funkcje opcjonalne. Pole wyboru dla zapisu zwrotnego urządzeń, a następnie kliknij przycisk **dalej**. Jeśli pole wyboru nadal jest wyłączone, zobacz [Rozwiązywanie problemów z sekcji](#the-writeback-checkbox-is-still-disabled).
   ![Instalacja niestandardowa opcjonalnych funkcji zapisywania zwrotnego urządzeń](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3. Na stronie funkcji zapisywania zwrotnego zobaczysz domeny podana jako domyślne lasu zapisywania zwrotnego urządzeń.
   ![Niestandardowe lasu docelowego zapisywania zwrotnego urządzeń instalacji](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4. Ukończ instalację kreatora bez zmian konfiguracji dodatkowych. W razie potrzeby odwoływać się do [Instalacja niestandardowa programu Azure AD Connect.](active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>Włączanie dostępu warunkowego
Szczegółowe instrukcje dotyczące tego scenariusza są dostępne w ramach [Konfigurowanie lokalnego dostępu warunkowego przy użyciu rejestracji urządzeń usługi Azure Active Directory](../active-directory-conditional-access-automatic-device-registration-setup.md).

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Sprawdź, czy urządzenia są synchronizowane z usługi Active Directory
Teraz można działa poprawnie zapisu zwrotnego urządzeń. Należy pamiętać, że może potrwać do 3 godzin obiekty urządzeń ma być zapisany wstecz AD.  Aby sprawdzić, czy urządzenia są synchronizowanego poprawnie, wykonaj następujące czynności po zakończeniu reguły synchronizacji:

1. Uruchom Centrum administracyjne usługi Active Directory.
2. Rozwiń RegisteredDevices w domenie, do której jest on federacyjnych.
   ![Centrum administracyjnego usługi Active Directory zarejestrowanych urządzeń](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3. Zostaną wyświetlone bieżące zarejestrowanych urządzeń.
   ![Lista urządzeń zarejestrowanych Centrum administracyjnego usługi Active Directory](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>Rozwiązywanie problemów
### <a name="the-writeback-checkbox-is-still-disabled"></a>Pole wyboru funkcji zapisywania zwrotnego nadal jest wyłączona.
Jeśli pole wyboru dla zapisu zwrotnego urządzeń nie jest włączona, nawet jeśli powyższe kroki zostały wykonane, następujące kroki poprowadzi Cię przez co Kreator instalacji sprawdza przed pole jest włączone.

Przede wszystkim pierwszy:

* Upewnij się, że co najmniej jednego lasu systemu Windows Server 2012 R2. Typ obiektu urządzenie musi być obecny.
* Jeśli Kreator instalacji jest już uruchomiona, zmiany nie można wykryć. W takim przypadku można zakończyć działanie Kreatora instalacji i uruchom go ponownie.
* Upewnij się, że konto podane w skrypcie inicjowania jest rzeczywiście odpowiednich użytkowników używane przez łącznik usługi Active Directory. Aby to sprawdzić, wykonaj następujące kroki:
  * Z start menu, otwórz **usługi synchronizacji**.
  * Otwórz **łączniki** kartę.
  * Znajdź łącznik, używając typu usług domenowych w usłudze Active Directory i zaznacz go.
  * W obszarze **akcje**, wybierz pozycję **właściwości**.
  * Przejdź do **nawiązać połączenia z lasu usługi Active Directory**. Sprawdź, czy domena i nazwa użytkownika określona w tym dopasowania ekranu konta przekazywane do skryptu.
    ![Konta łącznika w synchronizacji programu Service Manager](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Sprawdź konfigurację w usłudze Active Directory:

* Sprawdź, czy usługa rejestracji urządzeń znajduje się w lokalizacji poniżej (CN DeviceRegistrationService, CN = = usługi rejestracji urządzeń, CN = Device Registration Configuration, CN = Services, CN = Configuration) w kontekście nazewnictwa konfiguracji.

![Rozwiązywanie problemów, DeviceRegistrationService w przestrzeni nazw konfiguracji](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

* Sprawdź, czy istnieje tylko jeden obiekt konfiguracji przez wyszukiwanie nazw konfiguracji. Jeśli istnieje więcej niż jeden, Usuń duplikat.

![Rozwiązywanie problemów, Wyszukaj zduplikowane obiekty](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

* Upewnij się, atrybut msDS-DeviceLocation jest obecny i ma wartość na obiekt usługi rejestracji urządzeń. Wyszukiwanie tej lokalizacji i upewnij się, że jest obecny razem z objectType msDS-DeviceContainer.

![Rozwiązywanie problemów, msDS-DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Rozwiązywanie problemów, klasa obiektu RegisteredDevices](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

* Sprawdź, czy konto używane przez łącznik usługi Active Directory ma wymagane uprawnienia w kontenerze zarejestrowane urządzenia znalezionych w poprzednim kroku. Jest to oczekiwane uprawnienia do tego kontenera:

![Rozwiązywanie problemów, sprawdź uprawnienia do kontenera](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

* Sprawdź konto usługi Active Directory ma uprawnienia do CN = Device Registration Configuration, CN = Services, CN = obiekt konfiguracji.

![Rozwiązywanie problemów, sprawdź uprawnienia do Konfiguracja rejestracji urządzeń](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Dodatkowe informacje
* [Zarządzanie ryzykiem przy użyciu dostępu warunkowego](../active-directory-conditional-access-azure-portal.md)
* [Konfigurowanie lokalnego dostępu warunkowego przy użyciu rejestracji urządzeń usługi Azure Active Directory](../active-directory-device-registration-on-premises-setup.md)

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na temat [integrowania tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).

