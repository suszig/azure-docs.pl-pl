---
title: "Azure Active Directory Identity Protection podręcznika dotyczącego | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak Azure AD Identity Protection umożliwia ograniczenie możliwości osoby atakującej, która wykorzystać, którego bezpieczeństwo zostało naruszone tożsamości lub urządzenie i secure tożsamości lub urządzeń, które wcześniej podejrzenia lub znane naruszenia."
services: active-directory
keywords: "ochronę tożsamości usługi Azure active directory, usługa cloud app discovery, zarządzanie aplikacjami, zabezpieczeń, ryzyka, poziom ryzyka, luki w zabezpieczeniach, zasady zabezpieczeń"
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: e274a9364dcb7bf4d220159e1f906bbd015369aa
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-identity-protection-playbook"></a>Azure podręcznikowym ochronę tożsamości w usłudze Active Directory
Tego podręcznika dotyczącego ułatwia:

* Wypełnij dane w środowisku Identity Protection według symulowanie symulacje zdarzenia o podwyższonym ryzyku i luk w zabezpieczeniach
* Konfigurowanie zasad dostępu warunkowego opartego na zagrożenia i testowanie wpływu tych zasad

## <a name="simulating-risk-events"></a>Symulacja zdarzeń o podwyższonym ryzyku
W tej sekcji przedstawiono kroki symulowanie następujące typy zdarzeń ryzyka:

* Logowania z anonimowych adresów IP (prosty)
* Logowania z nieznanych lokalizacji (umiarkowany)
* Niemożliwa podróż do nietypowych lokalizacji (trudne)

Inne zdarzenia o podwyższonym ryzyku nie symulowane w bezpieczny sposób.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Logowania z anonimowych adresów IP
Ten typ zdarzenia ryzyka identyfikuje użytkowników, którzy zalogowali się pomyślnie z adresu IP, który został zidentyfikowany jako adres IP anonimowy serwer proxy. Te serwery proxy są używane przez osoby, które chcesz ukryć adres IP swoich urządzeń i może służyć do złośliwymi działaniami.

**Aby symulować logowanie z anonimowego adresu IP, wykonaj następujące kroki**:

1. Pobierz [przeglądarki Tor](https://www.torproject.org/projects/torbrowser.html.en).
2. Przy użyciu przeglądarki Tor, przejdź do [https://myapps.microsoft.com](https://myapps.microsoft.com).   
3. Wprowadź poświadczenia konta, która ma być wyświetlana w **logowania z anonimowych adresów IP** raportu.

Logowanie będzie wyświetlana na pulpicie nawigacyjnym ochronę tożsamości w ciągu 5 minut. 

### <a name="sign-ins-from-unfamiliar-locations"></a>Logowania z nieznanych lokalizacji
Ryzyko nieznanych lokalizacji jest mechanizm oceny w czasie rzeczywistym logowania, który uwzględnia poza logowania w lokalizacji (IP, szerokości geograficznej / długości i ASN) do określenia lokalizacji nowego / doświadczenia w pracy. System przechowuje poprzednie adresów IP, szerokości geograficznej / geograficzne i numerów ASN użytkownika i ich lokalizacje znane. Lokalizacja logowania jest uznawany za doświadczenia w pracy, jeśli lokalizacja logowania nie pasuje do żadnego z istniejącej lokalizacji znane.

Ochrona tożsamości usługi Azure Active Directory:  

* ma okres learning początkowej 14 dni, w których nie Flaga wszelkie nowe lokalizacje jako nieznanych lokalizacji.
* ignoruje logowania z urządzeń znanych i lokalizacje, które są geograficznie istniejącej lokalizacji znane.

Aby symulować nieznanych lokalizacji, należy zalogować się w lokalizacji i urządzenia, które konta nie zalogował się z przed. 

**Aby symulować logowania z nieznanych lokalizacji, wykonaj następujące kroki**:

1. Wybierz konto, które ma co najmniej 14 dni historii logowania. 
2. Wykonaj jedną:
   
   a. Podczas korzystania z sieci VPN, przejdź do [https://myapps.microsoft.com](https://myapps.microsoft.com) , a następnie wprowadź poświadczenia konta, aby symulować zdarzenia ryzyka.
   
   b. Poproś skojarzenia w innej lokalizacji, aby zalogować się przy użyciu poświadczeń konta (niezalecane).

Logowanie będzie wyświetlana na pulpicie nawigacyjnym ochronę tożsamości w ciągu 5 minut.

### <a name="impossible-travel-to-atypical-location"></a>Niemożliwa podróż do nietypowych lokalizacji
Symuluje warunku niemożliwa podróż jest trudne, ponieważ algorytm chwastów limit false alarmów, takich jak niemożliwa podróż ze znanych urządzeń lub logowania z sieci VPN, które są używane przez innych użytkowników w katalogu uczenia maszynowego. Ponadto algorytm wymaga historię logowania 3-14 dni dla użytkownika, przed rozpoczęciem generowania zdarzeń o podwyższonym ryzyku.

**Aby symulować niemożliwa podróż do nietypowych lokalizacji, wykonaj następujące kroki**:

1. Za pomocą przeglądarki standardowe, przejdź do [https://myapps.microsoft.com](https://myapps.microsoft.com).  
2. Wprowadź poświadczenia konta, które mają zostać wygenerowane zdarzenie niemożliwa podróż ryzyka dla.
3. Zmień agenta użytkownika. Można zmienić agenta użytkownika w programie Internet Explorer z Developer Tools, lub zmień agenta użytkownika przeglądarki Firefox lub Chrome przy użyciu dodatku przełącznik agenta użytkownika.
4. Zmienianie adresu IP. Adres IP można zmienić za pomocą sieci VPN, dodatek Tor, lub kręci się nowego komputera na platformie Azure w centrum danych.
5. Zaloguj się do [https://myapps.microsoft.com](https://myapps.microsoft.com) przy użyciu tych samych poświadczeń jako przed i za kilka minut po poprzednim logowanie.

Przy logowaniu będzie wyświetlana na pulpicie nawigacyjnym ochronę tożsamości w ciągu 2-4 godzin.<br>
Ze względu na maszynie złożonych zaangażowany modeli uczenia istnieje ryzyko, który nie ma pobrać pobierany się.<br> Można replikować te kroki dla wielu kont usługi Azure AD.

## <a name="simulating-vulnerabilities"></a>Symuluje luk w zabezpieczeniach
Luki w zabezpieczeniach występują słabych w środowisku usługi Azure AD, które mogą być używane przez aktora nieprawidłowy. Obecnie 3 typy luk w zabezpieczeniach są udostępniane w Azure AD Identity Protection, zwiększają inne funkcje usługi Azure AD. Te luki w zabezpieczeniach będą wyświetlane na pulpicie nawigacyjnym Identity Protection automatycznie po skonfigurowaniu tych funkcji.

* Usługi Azure AD [uwierzytelnianie wieloskładnikowe?](../multi-factor-authentication/multi-factor-authentication.md)
* Usługi Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).
* Usługi Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 

## <a name="user-compromise-risk"></a>Ryzyko naruszenia zabezpieczeń użytkownika
**Aby przetestować użytkownika ryzyko naruszenia zabezpieczeń, wykonaj następujące kroki**:

1. Zaloguj się do [https://portal.azure.com](https://portal.azure.com) przy użyciu poświadczeń administratora globalnego dla dzierżawy.
2. Przejdź do **ochronę tożsamości**. 
3. W głównym **Azure AD Identity Protection** bloku, kliknij przycisk **ustawienia**. 
4. Na **ustawienia portalu** bloku, w obszarze **reguły zabezpieczeń**, kliknij przycisk **ryzyko naruszenia zabezpieczeń użytkownika**. 
5. Na **Zaloguj ryzyka** bloku, Włącz **Włącz regułę** wyłączone, a następnie kliknij przycisk **zapisać** ustawienia.
6. Symulowanie doświadczenia w pracy dla danego konta użytkownika, lokalizacji lub anonimowe zdarzenie ryzyka adresu IP. Będzie to podnieść poziom ryzyka użytkownika dla tego użytkownika do **średni**.
7. Poczekaj kilka minut, a następnie sprawdź tego poziomu użytkownika dla użytkownika jest **średni**.
8. Przejdź do **ustawienia portalu** bloku.
9. Na **ryzyko naruszenia zabezpieczeń użytkownika** bloku, w obszarze **Włącz regułę**, wybierz pozycję **na** . 
10. Wybierz jedną z następujących opcji:
    
    a. Blok wybierz **średni** w obszarze **bloku Zaloguj**.
    
    b. Aby wymusić zmianę bezpieczne hasło, wybierz **średni** w obszarze **wymusić uwierzytelnianie wieloskładnikowe**.
11. Kliknij pozycję **Zapisz**.
12. Teraz możesz przetestować dostępu warunkowego opartego na ryzyko, logując się przy użyciu użytkownik z poziomem ryzyka z podwyższonym poziomem uprawnień. Ryzyko użytkownika w przypadku średniej, w zależności od konfiguracji zasad, logowanie jest albo zablokowania lub zostało wymuszone, aby zmienić hasło. 
    <br><br>
    ![Podręcznikowym](./media/active-directory-identityprotection-playbook/201.png "Podręcznikowym")
    <br>

## <a name="sign-in-risk"></a>Ryzyko logowania
**Aby przetestować znak ryzyko, należy wykonać następujące czynności:**

1. Zaloguj się do [https://portal.azure.com ](https://portal.azure.com) przy użyciu poświadczeń administratora globalnego dla dzierżawy.
2. Przejdź do **ochronę tożsamości**.
3. W głównym **Azure AD Identity Protection** bloku, kliknij przycisk **ustawienia**. 
4. Na **ustawienia portalu** bloku, w obszarze **reguły zabezpieczeń**, kliknij przycisk **Zaloguj ryzyka**.
5. Na ** Zaloguj ryzyka ** bloku, wybierz opcję **na** w obszarze **Włącz regułę**. 
6. Wybierz jedną z następujących opcji:
   
   a. Aby zablokować, wybierz **średni** w obszarze **bloku Zaloguj**
   
   b. Aby wymusić zmianę bezpieczne hasło, wybierz **średni** w obszarze **wymusić uwierzytelnianie wieloskładnikowe**.
7. Aby zablokować, należy wybrać średnia liczba godzin w bloku logowania.
8. Aby wymusić uwierzytelnianie wieloskładnikowe, wybierz **średni** w obszarze **wymusić uwierzytelnianie wieloskładnikowe**.
9. Kliknij przycisk **Zapisz**.
10. Teraz możesz przetestować dostępu warunkowego opartego na ryzyko symulując nieznanych lokalizacji lub anonimowe IP ryzyka zdarzeń, ponieważ są one zarówno **średni** ryzyka zdarzenia.


![Podręcznikowym](./media/active-directory-identityprotection-playbook/200.png "Podręcznikowym")


## <a name="see-also"></a>Zobacz też
* [Ochronę tożsamości usługi Azure Active Directory](active-directory-identityprotection.md)

