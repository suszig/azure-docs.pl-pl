---
title: "Co nowego? Informacje o wersji dla usługi Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Dowiedz się, co jest nowego w usłudze Azure Active Directory (Azure AD) w tym informacje o najnowszej wersji, znane problemy, poprawki, przestarzałe funkcje i nadchodzących zmianach."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
featureFlags: clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 408e1680e7aee13663bb6511e65fd5685eb7d518
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/20/2017
---
# <a name="whats-new-in-azure-active-directory"></a>Nowości w usłudze Azure Active Directory




> Stosowanie nowości w usłudze Azure Active Directory przez subskrybowanie naszych ![RSS](./media/whats-new/feed-icon-16x16.png) [źródła danych](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us).



Firma Microsoft umożliwiają zwiększenie usługi Azure Active Directory w sposób ciągły. Aby umożliwić bądź na bieżąco o najnowszych zmianach, w tym temacie przedstawiono informacje o:

-   Najnowsze wersje 
-   Znane problemy 
-   Poprawki błędów 
-   Funkcje uznane za przestarzałe 
-   Plany zmiany 

Należy ponownie tę stronę regularnie, ponieważ trwa jego aktualizacja co miesiąc.


## <a name="december-2017"></a>2017 grudnia
 

### <a name="terms-of-use-in-the-access-panel-for-end-users"></a>Warunki użytkowania w panelu dostępu dla użytkowników końcowych

**Typ:** nowej funkcji  
**Kategoria usług:** warunki użytkowania  
**Możliwości produktu:** ładu/zgodności
 
Użytkownicy końcowi mają teraz możliwość przejdź do panelu dostępu i Wyświetl warunki użytkowania, który już wcześniej zaakceptowano.

Użytkownicy mogą wyświetlać i przeglądać warunki użytkowania, które zaakceptowali. W tym celu można wykonać następujące czynności:

1. Nawigowanie i zaloguj się do [MyApps portal](https://myapps.microsoft.com).

2. W prawym górnym rogu kliknij swoją nazwę i wybierz pozycję **Profil** z listy rozwijanej. 

3. Na stronie profilu kliknij pozycję **Przejrzyj warunki użytkowania**. 

4. Następnie możesz przejrzeć zaakceptowane warunki użytkowania. 

Aby uzyskać więcej informacji, zobacz [funkcji Azure Active Directory warunków użytkowania (wersja zapoznawcza)](https://docs.microsoft.com/azure/active-directory/active-directory-tou)
 
---
 

### <a name="new-azure-ad-sign-in-experience"></a>Nowego środowiska logowania usługi Azure AD

**Typ:** nowej funkcji  
**Kategoria usług:** usługi Azure AD  
**Możliwości produktu:** uwierzytelnianie użytkownika
 
W ramach podróży zbieżności platformy Azure AD i systemów tożsamości konta Microsoft, firma Microsoft ma przeprojektowany interfejsu użytkownika na obu systemów, tak aby miały spójny wygląd i zachowanie. Ponadto firma Microsoft ma podzielony na strony strony logowania usługi Azure AD, aby zbieramy nazwę użytkownika, następuje poświadczeń na drugim ekranie.

Aby uzyskać więcej informacji, zobacz [nowego środowiska usługi Azure AD rejestrowanie znajduje się teraz w publicznej wersji zapoznawczej](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/)
 
---
 

### <a name="fewer-login-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-login"></a>Mniejszą liczbę monitów logowania: środowisko do nowego "wylogowuj mnie" dla nazwy logowania w usłudze Azure AD

**Typ:** nowej funkcji  
**Kategoria usług:** usługi Azure AD  
**Możliwości produktu:** uwierzytelnianie użytkownika
 
Firma Microsoft zastąpiła **wylogowuj mnie** uwierzytelnia wyboru na stronie logowania usługi Azure AD z nowego wierszu, który jest wyświetlany po użytkownik pomyślnie. 

Jeśli użytkownik odpowie **tak** do wiersza polecenia usługi daje, ich token odświeżania trwałych. To jest takie samo zachowanie jako Jeśli użytkownik włączy **wylogowuj mnie** checkbox w starym środowisko. W przypadku dzierżaw federacyjnych tego monitu zostaną wyświetlone po użytkownik zostanie pomyślnie uwierzytelniony w usłudze federacyjnej.

Aby uzyskać więcej informacji, zobacz [mniejszą liczbę monitów logowania: nowe środowisko "Wylogowuj mnie" dla usługi Azure AD jest w wersji zapoznawczej](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/) 

---
 

### <a name="add-configuration-to-require-the-tou-to-be-expanded-prior-to-accepting"></a>Dodaj konfigurację, aby wymagać do wyodrębnienia przed rozpoczęciem akceptowania warunków.

**Typ:** nowej funkcji  
**Kategoria usług:** warunki użytkowania  
**Możliwości produktu:** ładu
 
Teraz dodano możliwość rozwiń warunki użytkowania, przed rozpoczęciem akceptowania warunków dla administratorów wymagać użytkownikom końcowym.

Wybierz opcję on lub off dla wymagać od użytkowników rozwinąć warunki użytkowania. W przypadku włączenia tego ustawienia od użytkowników końcowych wymagane będzie wyświetlenie warunków użytkowania przed ich akceptacją.

Aby uzyskać więcej informacji, zobacz [funkcji Azure Active Directory warunków użytkowania (wersja zapoznawcza)](active-directory-tou.md)
 
---
 

### <a name="scoped-activation-for-eligible-role-assignments"></a>Zakresie aktywacji dla przypisania ról kwalifikujących się

**Typ:** nowej funkcji  
**Kategoria usług:** Privileged Identity Management  
**Możliwości produktu:** Privileged Identity Management
 
Zakresie aktywacji pozwala aktywować przypisania ról kwalifikujących się zasobów platformy Azure z mniej niezależność od oryginalnej wartości domyślnych przydziału. Na przykład przypisano właściciela subskrypcji w dzierżawie. Z zakresie aktywacji, może aktywować właściciela zasobów do pięciu zawartych w subskrypcji (wziąć pod uwagę grup zasobów, maszyny wirtualne itp...). Określanie zakresu proces aktywacji może zmniejszyć możliwość wykonywania niepożądanych zmian do kluczowych zasobów platformy Azure.

Aby uzyskać więcej informacji, zobacz [co to jest usługa Azure AD Privileged Identity Management?](active-directory-privileged-identity-management-configure.md).
 
---
 

### <a name="new-federated-apps-in-azure-ad-app-gallery"></a>Nowe aplikacje federacyjne w galerii aplikacji Azure AD

**Typ:** nowej funkcji  
**Kategoria usług:** aplikacje przedsiębiorstwa  
**Możliwości produktu:** 3 firm integracji
 
W grudniu 2017 dodano obsługę następujących nowych aplikacji w galerii aplikacji z Federacją:

|Name (Nazwa)|Typ integracji|Opis|
|:-- |----------------|:----------|
|EFI cyfrowe sklepu|SAML 2.0|[Aplikacja drukowania sieci Web 2](https://go.microsoft.com/fwlink/?linkid=861685)|
|Vodeclic|SAML 2.0|[Zarządzanie dostępem użytkowników i Włącz logowanie jednokrotne z Vodeclic przy użyciu usługi Azure AD](https://go.microsoft.com/fwlink/?linkid=863522).  Wymaga istniejącego konta Vodeclic.|
|Accredible|SAML 2.0|[Tworzenie i dostarczania certyfikatów, symbole i blockchain poświadczeń i zarządzanie](https://go.microsoft.com/fwlink/?linkid=863523)|
|FactSet|SAML 2.0|[Logowanie jednokrotne do aplikacji FDSWeb FactSet firmy](https://go.microsoft.com/fwlink/?linkid=863525)|
|Integracja usług MobileIron Azure AD|SAML 2.0|[MobileIron](https://go.microsoft.com/fwlink/?linkid=858027) misji jest umożliwienie nowoczesnych przedsiębiorstwom zabezpieczenie i zarządzanie nimi informacje przesyłane do mobile i w chmurze przy zachowaniu użytkownika końcowego i poufności informacji w relacji zaufania.|
|DZIAŁA OBRAZU|SAML 2.0|Zarządzanie dostępem użytkowników, obsługi administracyjnej kont użytkowników i Włącz logowanie jednokrotne z przy użyciu usługi Azure AD [WORKS obrazu](https://go.microsoft.com/fwlink/?linkid=863517). Wymaga istniejącej subskrypcji działa obrazu.|
|Logowania jednokrotnego SAML dla Bitbucket przez rozpoznawania GmbH|SAML 2.0|[Usługa rejestracji Jednokrotnej Bitbucket](https://go.microsoft.com/fwlink/?linkid=863519) delegatów uwierzytelniania usługi Azure AD, użytkownicy już zalogowany do usługi Azure AD mogą uzyskiwać dostęp do Bitbucket bezpośrednio. Użytkownicy mogą być tworzone i aktualizowane na bieżąco z danymi z atrybutów SAML.|
|Logowania jednokrotnego SAML dla Bambus przez rozpoznawania GmbH|SAML 2.0|[Usługa rejestracji Jednokrotnej Bambus](https://go.microsoft.com/fwlink/?linkid=863520) delegatów uwierzytelniania usługi Azure AD, użytkownicy już zalogowany do usługi Azure AD mogą uzyskiwać dostęp do Bambus bezpośrednio.|
|Communifire|SAML 2.0|[Communifire](https://go.microsoft.com/fwlink/?linkid=861676) jest nowoczesny, oferujący wszystkie potrzebne funkcje społecznościowe intranet oprogramowania obsługującego pracowników i firmy.|
|MOBI|SAML 2.0|[Scentralizowanie, pojmować i kontrolę z ekosystemem wszystkich danych z urządzenia](https://go.microsoft.com/fwlink/?linkid=863521).|
|Reflektive|SAML 2.0|[Reflektive](https://go.microsoft.com/fwlink/?linkid=863518) dotyczy nowoczesnych platformy zarządzania wydajnością, opinie w czasie rzeczywistym i ustawienia celu. Firma Microsoft pozwalają pracowników do rozwoju, dlatego może być ważniejsze.|
|CybSafe|OpenID Connect & OAuth|CybSafe to platforma świadomości certyfikowane GCHQ ataków. Sposób oczywisty zmniejszenie człowieka aspekt ryzyka ochrony danych i bezpieczeństwa przez używa zaawansowanej technologii i analizy danych.|
|WebHR|OpenID Connect & OAuth|Dla każdej osoby ulubionych w jednym HR społecznościowych oprogramowania. Zaufany przez ponad 20 000 firmy w krajach 197|
 |Integracja usług Zenegy Azure AD|OpenID Connect & OAuth|Z tą aplikacją poświadczeń usługi Azure Active Directory w firmie służy do zalogowania się do Zenegy.|
|Menedżer obsługi Adobe|SAML 2.0|Menedżer obsługi Adobe bez agenta (AEM), jest kompleksowe zarządzanie zawartością platformy rozwiązania do tworzenia witryn sieci Web, aplikacje mobilne i formularze — co ułatwia zarządzanie zawartości marketing i zasoby.|

 
---
 

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Przepływów pracy dla ról katalogu usługi Azure AD

**Typ:** funkcji zmienione  
**Kategoria usług:** Privileged Identity Management  
**Możliwości produktu:** Privileged Identity Management
 
Przepływ pracy zatwierdzania dla ról katalogu usługi Azure AD jest ogólnie dostępna.

Z procedury zatwierdzania ról uprzywilejowanych, Administratorzy mogą wymagać od członków roli kwalifikujących się żądania aktywacji ról, aby móc korzystać z ról uprzywilejowanych.
Wielu użytkowników i grup może być delegowane zatwierdzenia obowiązki członków roli kwalifikujących się otrzymywanie powiadomień o zatwierdzenie jest zakończenie i ich rolą jest aktywny

---
 

### <a name="pass-through-authentication---skype-for-business-support"></a>Uwierzytelniania przekazywanego - Skype dla firm pomocy technicznej

**Typ:** funkcji zmienione  
**Kategoria usług:** uwierzytelnienia (logowania do)  
**Możliwości produktu:** uwierzytelnianie użytkownika


Teraz uwierzytelniania przekazywanego obsługuje logowania użytkowników do usługi Skype dla firm aplikacji klienckich, które obsługują nowoczesnego uwierzytelniania, w tym Online i hybrydowych topologii. 

Aby uzyskać więcej informacji, zobacz [Skype dla firm topologie obsługiwane z nowoczesnego uwierzytelniania](https://technet.microsoft.com/library/mt803262.aspx).
 
---
 

### <a name="updates-to-azure-active-directory-privileged-identity-management-pim-for-azure-rbac-preview"></a>Aktualizacje do usługi Azure Active Directory uprzywilejowanego tożsamości zarządzania (PIM) dla Azure RBAC (wersja zapoznawcza)

**Typ:** funkcji zmienione  
**Kategoria usług:** PIM  
**Możliwości produktu:** Privileged Identity Management
 
Z naszych publicznego Podgląd Odśwież z Azure Active Directory uprzywilejowanych tożsamości zarządzania (PIM) dla Azure RBAC, można teraz:

Użyj właśnie, wystarczająco administracyjnej wymagają zatwierdzenia Uaktywnij role zasobów planowanie przyszłych aktywacji roli, która wymaga zatwierdzenia dla zarówno usługi AAD i Azure RBAC ról

 
Aby uzyskać więcej informacji, zobacz [PIM zasobów Azure (wersja zapoznawcza)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)

 
---
 
## <a name="november-2017"></a>2017 listopada
 
### <a name="retiring-acs"></a>Wycofanie usług ACS



**Typ:** Planowanie zmian  
**Kategoria usług:** ACS  
**Możliwości produktu:** usługi kontroli dostępu 


Microsoft Azure Active Directory kontroli dostępu (znanej także jako usługa kontroli dostępu lub ACS) zostaną wycofane w 2018 opóźnione.  Dodatkowe informacje, w tym szczegółowy harmonogram i wskazówki dotyczące migracji wysokiego poziomu, zapewnia się w następnych kilku tygodni. W międzyczasie, pozostaw komentarze na tej stronie pytania dotyczące usług ACS i członkiem naszego zespołu może pomóc w odpowiedzi.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Ogranicz dostęp przeglądarki do programu Intune managed browser 


**Typ:** Planowanie zmian  
**Kategoria usług:** dostępu warunkowego  
**Możliwości produktu:** tożsamości bezpieczeństwa i ochrony




Z tego zachowania można ograniczyć dostęp za pomocą przeglądarki do usługi Office 365 i inne aplikacje chmury Azure AD, połączony jako zatwierdzonych aplikacji przy użyciu usługi Intune Managed Browser. 

Ta zmiana umożliwia skonfigurowanie następujący warunek dla dostępu warunkowego opartego na aplikacji:

**Aplikacje klienta:** przeglądarki

**Jaki jest wpływ tej zmiany?**

Obecnie dostęp będzie zablokowany, korzystając z tego warunku. Gdy Podgląd tego zachowania nie jest dostępny, dostępu wymaga użycia aplikacji programu managed browser. 

Szukaj dla tej funkcji i bardziej w nadchodzących blogi i wersji uwagi. 

Aby uzyskać więcej informacji, zobacz [dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal.md).

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nowe aplikacje zatwierdzone klienta dla dostępu warunkowego na podstawie aplikacji usługi Azure AD

 
**Typ:** Planowanie zmian  
**Kategoria usług:** dostępu warunkowego  
**Możliwości produktu:** tożsamości bezpieczeństwa i ochrony




Następujące aplikacje są planowane do dodania do listy [zatwierdzonych aplikacji klienta](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement):

- [Kaizala firmy Microsoft](https://www.microsoft.com/garage/profiles/kaizala/)

- [StaffHub firmy Microsoft](https://staffhub.office.com/what-it-is)


Aby uzyskać więcej informacji, zobacz:

- [Wymagania aplikacji zatwierdzonych klienta](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)

- [Azure Active Directory na podstawie aplikacji dostępu warunkowego](active-directory-conditional-access-mam.md)


---

### <a name="terms-of-use-support-for-multiple-languages"></a>Warunki Użyj obsługi wielu języków



**Typ:** nowej funkcji    
**Kategoria usług:** warunki użytkowania  
**Możliwości produktu:** ładu/zgodności





Administratorzy mogą teraz tworzyć nowe warunki użytkowania (TOU), który zawiera wiele dokumentów PDF. Można oznaczać tych dokumentów PDF z odpowiedniego języka. Użytkownicy mieszczące się w zakresie są pokazywane PDF z zgodny język, w oparciu o ich preferencje. Jeśli nie są niezgodne, jest wyświetlany w języku domyślnym.


---
 

### <a name="realtime-password-writeback-client-status"></a>Stan klienta zapisywania zwrotnego haseł czasu rzeczywistego



**Typ:** nowej funkcji  
**Kategoria usług:** SSPR  
**Możliwości produktu:** uwierzytelnianie użytkownika


 

Można teraz przeglądać stan klienta zapisywania zwrotnego haseł lokalnych. Ta opcja jest dostępna w **integracji lokalnego** sekcji  **[resetowania hasła](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)**  strony. 

Jeśli występują problemy z połączeniem klienta lokalnego zapisywania zwrotnego, zobaczysz komunikat o błędzie, który zawiera program:

- Informacje o Dlaczego nie można nawiązać połączenia klienckie zapisywania zwrotnego lokalnej 
- Łącze do dokumentacji, która pomaga w rozwiązaniu problemu. 


Aby uzyskać więcej informacji, zobacz [integracji lokalnego](active-directory-passwords-how-it-works.md#on-premises-integration).

 
---


### <a name="azure-ad-app-based-conditional-access"></a>Azure AD na podstawie aplikacji dostępu warunkowego 



 
**Typ:** nowej funkcji  
**Kategoria usług:** usługi Azure AD  
**Możliwości produktu:** tożsamości bezpieczeństwa i ochrony





Teraz możesz ograniczyć dostęp do innych aplikacji chmury Azure połączone AD i Office 365 [zatwierdzonych aplikacji klienta](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) obsługujących zasad ochrony aplikacji usługi Intune przy użyciu [dostępu warunkowego na podstawie aplikacji usługi Azure AD](active-directory-conditional-access-mam.md). Zasady ochrony aplikacji usługi Intune służą do konfigurowania i ochrony danych firmy na tych aplikacji klienckich.

Łącząc [aplikacji na podstawie](active-directory-conditional-access-mam.md) z [oparta na urządzeniach](active-directory-conditional-access-policy-connected-applications.md) zasady dostępu warunkowego, masz możliwość ochrony danych osobistych i firmowych urządzeń.

Poniższe warunki i formanty są teraz dostępne do użycia z dostępu warunkowego opartego na aplikacji:

**Warunek obsługiwanych platform**

- iOS
- Android

**Stan aplikacji klienta**

- Aplikacje mobilne i klienci stacjonarni

**Kontrola dostępu**

- Wymagają klienta zatwierdzonych aplikacji


Aby uzyskać więcej informacji, zobacz [dostępu warunkowego na podstawie aplikacji usługi Azure Active Directory](active-directory-conditional-access-mam.md).

 
---

### <a name="managing-azure-ad-devices-in-the-azure-portal"></a>Zarządzanie urządzeniami w usłudze Azure AD w portalu Azure



**Typ:** nowej funkcji  
**Kategoria usług:** rejestracji urządzeń i zarządzanie  
**Możliwości produktu:** tożsamości bezpieczeństwa i ochrony

 



Teraz można znaleźć wszystkie urządzenia podłączone do usługi Azure AD i działania dotyczące urządzeń w jednym miejscu. Brak nowych czynności administracyjne do zarządzania tożsamościami urządzenia i ustawienia w portalu Azure. W tej wersji można wykonywać następujące czynności:

- Wyświetl wszystkie urządzenia, które są dostępne dla dostępu warunkowego w usłudze Azure AD

- Urządzeniach przyłączonych do właściwości widoku, łącznie z hybrydowej usługi Azure AD

- Znajdowanie kluczy funkcji BitLocker dla urządzeń przyłączonych do usługi AD platformy Azure, zarządzać urządzenie z usługi Intune i inne.

- Zarządzanie ustawieniami związanymi z urządzenia usługi Azure AD


Aby uzyskać więcej informacji, zobacz [zarządzanie urządzeniami przy użyciu portalu Azure](device-management-azure-portal.md).



 
---

### <a name="support-for-macos-as-device-platform-for-azure-ad-conditional-access"></a>Obsługa macOS jako platformę urządzenia dla dostępu warunkowego dla usługi Azure AD 



**Typ:** nowej funkcji    
**Kategoria usług:** dostępu warunkowego  
**Możliwości produktu:** tożsamości bezpieczeństwa i ochrony 
 

Można teraz uwzględnić (lub wykluczyć) macOS warunkiem platformy urządzeń w usłudze Azure AD zasady dostępu warunkowego. Z dodatkiem macOS na platformach obsługiwanych urządzeń można:

- **Rejestrowanie urządzeń i zarządzanie nimi macOS przy użyciu usługi Intune** — podobnie jak w innych platform, takich jak systemy iOS i Android, aplikacji portalu firmy jest dostępna dla macOS ujednoliconego rejestracji. Nowa aplikacja portal firmy dla macOS umożliwia rejestrowanie urządzeń w usłudze Intune i zarejestrowanie go za pomocą usługi Azure AD.
 
- **Upewnij się, urządzenia macOS obowiązują zasady zgodności organizacji zdefiniowane w usłudze Intune** — w usłudze Intune w portalu Azure, możesz teraz skonfigurować zasady zgodności dla urządzeń macOS. 
  
- **Ograniczanie dostępu do aplikacji w usłudze Azure AD, aby tylko urządzenia zgodne macOS** — Tworzenie zasad dostępu warunkowego ma macOS jako opcji platformy urządzenia oddzielne. Ta opcja umożliwia tworzenie macOS zasad określonych dostępu warunkowego dla zestawu docelowego aplikacji na platformie Azure.

Aby uzyskać więcej informacji, zobacz:

- [Tworzenie zasad zgodności urządzeń dla urządzeń macOS przy użyciu usługi Intune](https://aka.ms/macoscompliancepolicy)
- [Dostęp warunkowy w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal.md)


 
---

### <a name="nps-extension-for-azure-mfa"></a>Rozszerzenia serwera zasad Sieciowych na potrzeby usługi Azure MFA 


**Typ:** nowej funkcji    
**Kategoria usług:** MFA  
**Możliwości produktu:** uwierzytelnianie użytkownika




Rozszerzenia serwera zasad sieciowych (NPS) dla usługi Azure MFA dodaje możliwości usługi MFA oparte na chmurze do uwierzytelniania przy użyciu istniejących serwerów infrastruktury. Z rozszerzeniem serwera zasad Sieciowych można dodać połączenie telefoniczne, wiadomość tekstowa lub weryfikacji aplikacji telefonicznej z istniejących przepływ uwierzytelniania bez konieczności instalowania, konfigurowania i konserwacji nowe serwery. 

To rozszerzenie został utworzony dla organizacji, które chcesz chronić połączenia sieci VPN bez wdrażania serwera usługi Azure MFA. Rozszerzenia serwera NPS działa jako karty sieciowej między usługi RADIUS i oparte na chmurze usługi Azure MFA, aby zapewnić drugi składnik uwierzytelniania federacyjnego lub zsynchronizowanych użytkowników.


Aby uzyskać więcej informacji, zobacz [integracji istniejącej infrastrukturze serwera NPS z usługi Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-nps-extension.md)

 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Przywróć lub trwale usunąć usunięci użytkownicy


**Typ:** nowej funkcji    
**Kategoria usług:** Zarządzanie użytkownikami  
**Możliwości produktu:** katalogu 



W Centrum administracyjnym usługi Azure AD można teraz:

- Przywróć usuniętego użytkownika 
- Trwale usuń użytkownika 


**Aby wypróbować jej możliwości:**

1. W Centrum administracyjnym usługi Azure AD, wybierz [ **wszyscy użytkownicy** ](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) w **Zarządzaj** sekcji. 

2. Z **Pokaż** listy, wybierz **niedawno usunięta użytkowników**. 

4. Wybierz co najmniej jeden użytkownik niedawno usunięte, następnie przywrócić je i trwale usunąć je.

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nowe aplikacje zatwierdzone klienta dla dostępu warunkowego na podstawie aplikacji usługi Azure AD

 
**Typ:** funkcji zmienione  
**Kategoria usług:** dostępu warunkowego  
**Możliwości produktu:** tożsamości bezpieczeństwa i ochrony


Następujące aplikacje zostały dodane do listy [zatwierdzonych aplikacji klienta](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement):

- Planowanie firmy Microsoft

- Microsoft Azure Information Protection 


Aby uzyskać więcej informacji, zobacz:

- [Wymagania aplikacji zatwierdzonych klienta](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)

- [Azure Active Directory na podstawie aplikacji dostępu warunkowego](active-directory-conditional-access-mam.md)


---

### <a name="ability-to-or-between-controls-in-a-conditional-access-policy"></a>Zdolność do "lub" między formantami w zasadach dostępu warunkowego 


**Typ:** funkcji zmienione    
**Kategoria usług:** dostępu warunkowego  
**Możliwości produktu:** tożsamości bezpieczeństwa i ochrony

 
Możliwość "Lub" (wymagają wybranych formantów) została zwolniona kontroli dostępu warunkowego. Ta funkcja umożliwia tworzenie zasad z **lub** między kontroli dostępu. Na przykład korzystając tej funkcji można utworzyć zasadę, która wymaga od użytkownika zalogować się przy użyciu usługi Multi-Factor authentication **lub** na zgodnego urządzenia.

Aby uzyskać więcej informacji, zobacz [kontroli dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-controls.md).

 
---

### <a name="aggregation-of-realtime-risk-events"></a>Agregacja zdarzenia ryzyka w czasie rzeczywistym


**Typ:** funkcji zmienione    
**Kategoria usług:** Identity Protection  
**Możliwości produktu:** tożsamości bezpieczeństwa i ochrony


Aby udoskonalać jej działanie administracji, w usłudze Azure AD Identity Protection, wszystkie zdarzenia ryzyka czasu rzeczywistego, pochodzących z tego samego adresu IP w danym dniu teraz są agregowane dla każdego typu zdarzenia ryzyka. Ta zmiana ogranicza ilość zdarzenia o podwyższonym ryzyku pokazano bez zmiany zabezpieczeń użytkownika.

Podstawowy wykrywania czasu rzeczywistego działa w każdym razem, gdy użytkownik loguje. Jeśli ustawienia zasad zabezpieczeń w logowania ryzyka na MFA lub blokowanie dostępu, nadal jest wyzwalane podczas każdego ryzykowne logowania.

 
---
 




## <a name="october-2017"></a>2017 października


### <a name="deprecating-azure-ad-reports"></a>Wycofano raportów usługi Azure AD


**Typ:** Planowanie zmian  
**Kategoria usług:** raportowania  
**Możliwości produktu:** zarządzania cyklem życia tożsamości  



Azure portal udostępnia:

- Nową konsolę administracyjną usługi Azure Active Directory 
- Nowe interfejsy API dla działania i zabezpieczeń raportów
 
Z powodu nowych funkcji, raport interfejsów API w obszarze **/reports** punktu końcowego zostaną wycofane na 10 grudnia 2017 r. 

---

### <a name="automatic-sign-in-field-detection"></a>Wykrywanie automatyczne logowanie pola


**Typ:** stałej   
**Kategoria usług:** Moje aplikacje  
**Możliwości produktu:** logowania jednokrotnego  



Azure Active Directory obsługuje wykrywanie pola automatycznego logowania dla aplikacji, które renderują pola HTML nazwy użytkownika i hasła.  Te kroki są udokumentowane w artykule [automatycznie Przechwytywanie pola logowania dla aplikacji](application-config-sso-problem-configure-password-sso-non-gallery.md#how-to-manually-capture-sign-in-fields-for-an-application). Tej funkcji można znaleźć, dodając *Non-galerii* aplikację na **aplikacje dla przedsiębiorstw** strony [portalu Azure](http://aad.portal.azure.com). Ponadto można skonfigurować **rejestracji jednokrotnej** tryb tej nowej aplikacji do **opartego na hasłach rejestracji jednokrotnej**, wprowadzając adres URL sieci web, a następnie zapisanie strony.
 
Ze względu na problem dotyczący usługi ta funkcja została tymczasowo wyłączona w danym okresie czasu. Problem został rozwiązany i wykrywanie automatyczne logowanie pole jest dostępne ponownie.

---

### <a name="new-mfa-features"></a>Nowe funkcje usługi MFA


**Typ:** nowej funkcji  
**Kategoria usług:** MFA  
**Możliwości produktu:** tożsamości bezpieczeństwa i ochrony  



Uwierzytelnianie wieloskładnikowe (MFA) jest integralną część ochrony Twojej organizacji. Aby bardziej adaptacyjną poświadczeń i doświadczenia płynniejszą, dodano następujące funkcje: 

- Integracji wyników wieloskładnikowego żądania bezpośrednio do usługi Azure AD logowania raportu, tym programowy dostęp do usługi MFA wyników

- Lepsza integracja konfiguracji usługi MFA w konfiguracji usługi Azure AD doświadczenie w portalu Azure

Z publicznej wersji zapoznawczej MFA zarządzania i raportowania są integralną częścią środowisko konfiguracji podstawowej usługi Azure AD. Agregowanie obie funkcje umożliwia zarządzanie funkcje portalu zarządzania usługi MFA w ramach obsługi usługi Azure AD.

Aby uzyskać więcej informacji, zobacz [informacje dotyczące uwierzytelniania wieloskładnikowego raportowanie w portalu Azure](active-directory-reporting-activity-sign-ins-mfa.md) 


---

### <a name="introducing-terms-of-use"></a>Wprowadzenie warunki użytkowania



**Typ:** nowej funkcji  
**Kategoria usług:** warunki użytkowania  
**Możliwości produktu:** ładu  



Warunki użytkowania w usłudze Azure AD zapewniają prosty sposób prezentować informacje dla użytkowników końcowych. Daje to pewność, że użytkownicy będą widzieć zastrzeżenia istotne dla prawne lub wymaganiami dotyczącymi zgodności.

Warunki użytkowania usługi Azure AD można użyć w następujących scenariuszach:

- Ogólne warunki użytkowania obowiązujące wszystkich użytkowników w organizacji. 

- Określone warunki użytkowania na podstawie atrybutów użytkownika (np. lekarzy pielęgniarek vs lub krajowych vs międzynarodowych pracowników, programach grup dynamicznych). 

- Określone warunki użytkowania do uzyskiwania dostępu do aplikacji wpływ wysokiej biznesowych, takie jak Salesforce.

Aby uzyskać więcej informacji, zobacz [Azure Active Directory warunki użytkowania](active-directory-tou.md).


---

### <a name="enhancements-to-privileged-identity-management"></a>Ulepszenia privileged identity Management


**Typ:** nowej funkcji  
**Kategoria usług:** PIM  
**Możliwości produktu:** Privileged Identity Management  


Z usługi Azure Active Directory uprzywilejowanych tożsamości zarządzania (PIM), mogą teraz zarządzać, kontrolę i monitorowanie dostępu do zasobów Azure (wersja zapoznawcza) w Twojej organizacji:

- Subskrypcje
- Grupy zasobów
- Maszyny wirtualne. 

Wszystkie zasoby w portalu Azure, korzystających z funkcji Azure roli na podstawie kontroli dostępu (RBAC) mogą korzystać z wszystkich zabezpieczeń i możliwości zarządzania cyklem życia Azure AD PIM musi oferować.

Aby uzyskać więcej informacji, zobacz [PIM zasobów Azure](privileged-identity-management/azure-pim-resource-rbac.md).


---

### <a name="introducing-access-reviews"></a>Wprowadzenie dostępu przeglądy


**Typ:** nowej funkcji  
**Kategoria usług:** dostęp do przeglądu  
**Możliwości produktu:** ładu  



Przeglądy dostęp (wersja zapoznawcza) umożliwiają organizacjom efektywne zarządzanie członkostwami grup, a dostęp do aplikacji przedsiębiorstwa: 

- Można recertify dostępu użytkownika gościa przy użyciu dostępu przeglądy ich dostęp do aplikacji i członkostwa grup. Szczegółowych danych dostarczonych przez przeglądy dostępu Włącz recenzentów wydajnie zdecydować, czy gości powinny mieć stały dostęp.

- Przeprowadzając przeglądy dostępu, można zatwierdzać dostęp do aplikacji i członkostwo w grupach pracowników.

Kontrole z przeglądem dostępu można łączyć w programy odpowiednie dla danej organizacji, aby śledzić przeglądy dla aplikacji dotyczących zgodności lub aplikacji narażonych na ryzyko.

Aby uzyskać więcej informacji, zobacz [sprawdza dostęp do usługi Azure AD](active-directory-azure-ad-controls-access-reviews-overview.md).


---

### <a name="hiding-third-party-applications-from-my-apps-and-the-office-365-launcher"></a>Ukrywanie aplikacji innych firm z Moje aplikacje i uruchamiania usługi Office 365



**Typ:** nowej funkcji  
**Kategoria usług:** Moje aplikacje  
**Możliwości produktu:** logowania jednokrotnego  



Teraz można lepiej zarządzać aplikacjami na Twoje portale użytkownika za pomocą nowego **Ukryj aplikacji** właściwości. Ukrywanie aplikacji pomaga w przypadkach, gdy kafelków aplikacji są wyświetlane dla usługi wewnętrznej bazy danych lub zduplikowanych fragmentów i końcowa przeładowania Przyciski Uruchom aplikacji użytkownika. Przełącznik znajduje się w sekcji właściwości aplikacji innych producentów i etykietą **widoczny dla użytkownika?** Można również ukryć aplikacji programowo przy użyciu programu PowerShell. 

Aby uzyskać więcej informacji, zobacz [Ukryj aplikacji innych producentów, od środowiska użytkownika w usłudze Azure Active Directory](active-directory-coreapps-hide-third-party-app.md). 


**Co to jest dostępny?**

 W ramach przejścia do nowej konsoli administracyjnej 2 nowych interfejsów API do pobierania dzienników działania usługi Azure AD są dostępne. Nowy zestaw interfejsów API zapewnia bardziej zaawansowane funkcje filtrowania i sortowania funkcje oprócz bardziej zaawansowane funkcje inspekcji i rejestrowania działania. Obecnie jest możliwy wcześniej dostępne raporty dotyczące zabezpieczeń danych za pośrednictwem zdarzenia ryzyka Identity Protection interfejsu API programu Microsoft Graph.


## <a name="september-2017"></a>2017 września

### <a name="hotfix-for-microsoft-identity-manager"></a>Poprawka programu Microsoft Identity Manager


**Typ:** funkcji zmienione  
**Kategoria usług:** programu Microsoft Identity Manager  
**Możliwości produktu:** zarządzania cyklem życia tożsamości  



Pakiet zbiorczy poprawek (kompilacja 4.4.1642.0) jest dostępny od 25 września 2017 Microsoft Identity Manager (MIM) 2016 2016 z dodatkiem Service Pack 1 (SP1). Ten pakiet zbiorczy:

- Rozwiązuje problemy i dodaje ulepszenia
- Jest aktualizacją zbiorczą, który zastępuje wszystkie aktualizacje MIM 2016 z dodatkiem SP1 do kompilacji 4.4.1459.0 dla programu Microsoft Identity Manager 2016. 
- Wymagane jest posiadanie **4.4.1302.0 kompilacji programu Microsoft Identity Manager 2016.** 

Aby uzyskać więcej informacji, zobacz [pakiet zbiorczy poprawek (kompilacja 4.4.1642.0) jest dostępna dla programu Microsoft Identity Manager 2016 z dodatkiem SP1](https://support.microsoft.com/help/4021562). 

---
