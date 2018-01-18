---
title: "Implementowanie synchronizacji haseł z synchronizacji Azure AD Connect | Dokumentacja firmy Microsoft"
description: "Zawiera informacje o sposobie działania synchronizacji haseł i sposobu konfigurowania."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 05f16c3e-9d23-45dc-afca-3d0fa9dbf501
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: billmath
ms.openlocfilehash: 2a28089c85150f1876cd0ade58eb128e2d28f778
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="implement-password-synchronization-with-azure-ad-connect-sync"></a>Implementowanie synchronizacji haseł z synchronizacji Azure AD Connect
Ten artykuł zawiera informacje potrzebne do synchronizacji haseł użytkowników z lokalnego wystąpienia usługi Active Directory do wystąpienia usługi Azure Active Directory (Azure AD) oparte na chmurze.

## <a name="what-is-password-synchronization"></a>Co to jest synchronizacja haseł
Prawdopodobieństwo, że jest blokowane pracę z powodu zapomniane hasło jest powiązany z liczbę różnych haseł należy pamiętać. Więcej haseł należy należy pamiętać, że większe prawdopodobieństwo zapomnieć jeden. Pytania i wywołania dotyczące resetowania hasła i inne problemy związane z hasłem popytu najwięcej zasobów pomocy technicznej.

Synchronizacja haseł to funkcja używana do synchronizacji haseł użytkowników z lokalnego wystąpienia usługi Active Directory na platformie Azure opartej na chmurze wystąpienia usługi AD.
Ta funkcja służy do logowania do usługi Azure AD, takie jak usługi Office 365, Microsoft Intune CRM Online i Azure Active Directory Domain Services (Azure AD DS). Możesz logować się do usługi przy użyciu tego samego hasła, używanego do logowania się na lokalnym wystąpieniem usługi Active Directory.

![Co to jest program Azure AD Connect](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Dzięki zmniejszeniu liczby haseł, użytkownicy muszą zachować do co najmniej jeden. Synchronizacja haseł ułatwia:

* Zwiększenie produktywności użytkowników.
* Obniżyć koszty pomocy technicznej.  

Ponadto jeśli zdecydujesz się używać [federacji z usługi Active Directory Federation Services (AD FS)](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect), można opcjonalnie skonfigurować synchronizacji haseł jako zapasowy na wypadek awarii infrastruktury usług AD FS.

Synchronizacja haseł to rozszerzenie funkcji synchronizacji katalogu implementowane przez synchronizacja programu Azure AD Connect. Aby korzystać z synchronizacji haseł w danym środowisku, musisz:

* Instalowanie usługi Azure AD Connect.  
* Skonfigurować synchronizację katalogów między lokalnym wystąpieniem usługi Active Directory i wystąpienie usługi Azure Active Directory.
* Włączanie synchronizacji haseł.

Aby uzyskać więcej informacji, zobacz [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).

> [!NOTE]
> Aby uzyskać więcej informacji na temat usługi Azure Active Directory Domain Services skonfigurowany dla FIPS i synchronizacja haseł, zobacz "synchronizacji haseł i FIPS" w dalszej części tego artykułu.
>
>

## <a name="how-password-synchronization-works"></a>Opis działania synchronizacji haseł
Domeny usługi Active Directory są przechowywane hasła w postaci reprezentację wartość skrótu hasła rzeczywistego użytkownika. Wartość skrótu jest wynikiem jednokierunkowej funkcji matematycznych ( *algorytmem wyznaczania wartości skrótu*). Nie istnieje metoda można przywrócić wynik jednokierunkowa funkcja wersji tekstowego hasła. Nie można użyć skrótu hasła do logowania się w sieci lokalnej.

Aby synchronizować hasła, synchronizacja programu Azure AD Connect wyodrębnia z skrót hasła z lokalnego wystąpienia usługi Active Directory. Przetwarzanie dodatkowych zabezpieczeń są stosowane do skrótu hasła przed jest zsynchronizowany z usługą uwierzytelniania usługi Azure Active Directory. Hasła są synchronizowane na poszczególnych użytkowników i w kolejności chronologicznej.

Przepływ danych rzeczywistych proces synchronizacji haseł jest podobny do synchronizacji danych użytkownika, takich jak DisplayName lub adresy E-mail. Jednak hasła są synchronizowane częściej niż okno synchronizacji katalogu standard dla innych atrybutów. Proces synchronizacji haseł jest uruchamiany co 2 minuty. Nie można zmodyfikować częstotliwości tego procesu. Po zsynchronizowaniu hasła, zastępuje on istniejące hasło chmury.

Przy pierwszym włączeniu funkcji synchronizacji haseł, wykonuje wstępnej synchronizacji haseł wszystkich użytkowników w zakresie. Nie można jawnie definiować podzbiór hasła użytkownika, które mają być synchronizowane.

Jeśli zmienisz hasło lokalne, zaktualizowanego hasła są synchronizowane, często w ciągu kilku minut.
Funkcja synchronizacji hasła ma automatycznie ponawiać próby synchronizacji nie powiodło się. Jeśli wystąpi błąd podczas próby synchronizować hasła, błąd jest rejestrowany w podglądu zdarzeń.

Synchronizacja hasła nie ma wpływu na użytkownika, który jest aktualnie zalogowany.
Zmiany hasła zsynchronizowane, która występuje, gdy użytkownik jest zalogowany do usługi w chmurze nie występuje od razu w bieżącej sesji usługi chmury. Jednakże gdy usługa w chmurze, należy uwierzytelnić się ponownie, należy podać nowe hasło.

Użytkownik musi wprowadzić poświadczenia firmowe po raz drugi do uwierzytelniania usługi Azure AD, niezależnie od tego, czy ich nastąpiło zalogowanie do sieci firmowej. Wzorzec te można zminimalizować, jednak jeśli użytkownik wybierze Keep mnie podpisany w pole wyboru (KMSI) podczas logowania. Wybranie tej opcji ustawia plik cookie sesji omija uwierzytelniania krótki okres. Zachowanie KMSI można włączona lub wyłączona przez administratora usługi Azure AD.

> [!NOTE]
> Synchronizacja haseł jest obsługiwana tylko dla typu obiektu użytkownika w usłudze Active Directory. Nie jest obsługiwana dla typu obiektu iNetOrgPerson.

### <a name="detailed-description-of-how-password-synchronization-works"></a>Szczegółowy opis działania synchronizacji haseł
Poniżej przedstawiono szczegółowy opis działania synchronizacji haseł od usługi Active Directory i Azure AD.

![Przepływ szczegółowe hasła](./media/active-directory-aadconnectsync-implement-password-synchronization/arch3.png)


1. Co dwie minuty, agent synchronizacji haseł na serwerze AD Connect żądań skrótów haseł przechowywanych (atrybut unicodePwd) z kontrolera domeny za pomocą standardowego [MS DRSR](https://msdn.microsoft.com/library/cc228086.aspx) używane do synchronizowania danych między protokołu replikacji Kontrolery domeny. Konto usługi musi mieć replikować zmiany katalogu i replikować katalogu zmiany wszystkich AD uprawnienia (domyślnie w instalacji) do uzyskania wartości skrótu hasła.
2. Przed wysłaniem, kontroler domeny szyfruje MD4 skrót hasła przy użyciu klucza, który jest [MD5](http://www.rfc-editor.org/rfc/rfc1321.txt) skrótu klucza sesji RPC i soli. Następnie wysyła wynik do agent synchronizacji haseł za pośrednictwem wywołania RPC. Kontroler domeny również przekazuje soli agent synchronizacji przy użyciu protokołu replikacji kontrolera domeny, agent będzie można odszyfrować koperty.
3.  Agent synchronizacji haseł po zaszyfrowanych koperty używa [MD5CryptoServiceProvider](https://msdn.microsoft.com/library/System.Security.Cryptography.MD5CryptoServiceProvider.aspx) i soli do wygenerowania klucza do odszyfrowywania odebranych danych do oryginalnego formatu MD4. W żadnym punkcie agent synchronizacji haseł ma dostęp do hasła w postaci zwykłego tekstu. Użyj agent synchronizacji haseł MD5 obejmuje wyłącznie zgodność z protokołem replikacji z kontrolerem domeny i jest używana tylko na lokalne między kontrolerem domeny i agent synchronizacji haseł.
4.  Agent synchronizacji haseł rozszerza wartość skrótu hasła binarne 16-bajtowych 64 bajtów konwertując pierwszy skrót na ciąg szesnastkowy 32-bajtowych, następnie konwertując tego ciągu z powrotem do binarną, używając kodowania UTF-16.
5.  Agent synchronizacji haseł dodaje soli, składające się soli długość 10-bajtowych, do pliku binarnego 64-bajtowych, aby jeszcze lepiej chronić oryginalnego wyznaczania wartości skrótu.
6.  Agent synchronizacji haseł następnie łączy skrót MD4 plus soli i danych wejściowych do [PBKDF2](https://www.ietf.org/rfc/rfc2898.txt) funkcji. iteracje 1000 [HMAC SHA256](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) kluczem algorytmu wyznaczania wartości skrótu jest używany. 
7.  Agent synchronizacji haseł przyjmuje wynikowy skrót 32-bajtowych, łączy zarówno soli i liczby iteracji SHA256 do niego (na potrzeby używania przez usługę Azure AD), a następnie przesyła ciąg z usługi Azure AD Connect do usługi Azure AD przy użyciu protokołu SSL.</br> 
8.  Gdy użytkownik próbuje zalogować się do usługi Azure AD i wprowadza swoje hasło, hasło jest uruchamiane za pomocą tego samego MD4 + ziarna + PBKDF2 + HMAC SHA256 procesu. Jeśli wynikowy skrót zgodny skrótu przechowywane w usłudze Azure AD, użytkownik wprowadził prawidłowe hasło i zostanie uwierzytelniony. 

>[!Note] 
>Oryginalny skrót MD4 nie są przesyłane do usługi Azure AD. Zamiast tego są przesyłane skrót SHA256 wyznaczania wartości skrótu MD4 oryginalnego. W związku z tym Jeśli skrót przechowywane w usłudze Azure AD są uzyskiwane, nie można użyć w ataku pass--hash lokalnymi.

### <a name="how-password-synchronization-works-with-azure-active-directory-domain-services"></a>Opis działania synchronizacji haseł z usług domenowych Azure Active Directory
Umożliwia także funkcji synchronizacji haseł można zsynchronizować hasła lokalnego do [Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md). W tym scenariuszu wystąpienia usługi Azure Active Directory Domain Services uwierzytelnia użytkowników w chmurze z wszystkich metod dostępnych w lokalnym wystąpieniu usługi Active Directory. Środowisko w tym scenariuszu jest podobny do sposobu używania narzędzia migracji usługi Active Directory (ADMT) w środowisku lokalnym.

### <a name="security-considerations"></a>Zagadnienia związane z zabezpieczeniami
Podczas synchronizacji haseł wersji tekstowego hasła nie jest widoczne dla funkcji synchronizacji haseł do usługi Azure AD, ani dla żadnej z powiązanymi usługami.

Uwierzytelnianie użytkownika ma miejsce przed usługi Azure AD, a nie na wystąpieniu usługi Active Directory w organizacji. Jeśli Twoja organizacja ma problemów dotyczących danych hasła w żadnym tworzą, pozostawiając lokalnej instalacji programu, należy wziąć pod uwagę fakt, że dane hasło SHA256 przechowywane w usłudze Azure AD — skrót oryginalnego wyznaczania wartości skrótu MD4 — jest znacznie bezpieczniejsze niż co to jest przechowywane w usłudze Active Directory. Co więcej ponieważ nie można odszyfrować ten skrót SHA256, uniemożliwiający przywrócone do środowiska usługi Active Directory w firmie i widoczne jako prawidłowego użytkownika hasła w ataku typu pass--hash.





### <a name="password-policy-considerations"></a>Zagadnienia dotyczące zasad haseł
Istnieją dwa typy zasad haseł, których dotyczy Włączanie synchronizacji haseł:

* Zasady złożoności haseł
* Zasady wygasania haseł

#### <a name="password-complexity-policy"></a>Zasady złożoności haseł  
Po włączeniu synchronizacji haseł zasady złożoności haseł w lokalnym wystąpieniu usługi Active Directory zastępują zasady złożoności w chmurze na potrzeby zsynchronizowanych użytkowników. Możliwość używania wszystkich prawidłowy haseł z lokalnym wystąpieniem usługi Active Directory dostępu do usług Azure AD.

> [!NOTE]
> Hasła użytkowników, które są tworzone bezpośrednio w chmurze podlegają nadal zasady haseł zgodnie z definicją w chmurze.

#### <a name="password-expiration-policy"></a>Zasady wygasania haseł  
Jeśli użytkownik znajduje się w zakresie synchronizacji haseł, hasło konta chmury jest ustawiony na *nigdy nie wygasa*.

Możesz logować się do usługi w chmurze przy użyciu hasła zsynchronizowane, który wygasł w środowisku lokalnym. Hasło chmury jest aktualizowany przy następnym zmienić hasło w środowisku lokalnym.

#### <a name="account-expiration"></a>Okres ważności konta
Jeśli organizacja używa atrybutu accountExpires jako część Zarządzanie kontami użytkowników, należy pamiętać, że ten atrybut nie jest zsynchronizowany z usługą Azure AD. W związku z tym wygasłe konto usługi Active Directory w środowisku skonfigurowana z synchronizacją hasła pozostaje aktywna w usłudze Azure AD. Zaleca się, że wygasł konto akcji przepływu pracy powinno spowodować skrypt programu PowerShell, które wyłączają konta usługi Azure AD. Z drugiej strony gdy konto jest włączona, wystąpienie usługi Azure AD powinna być włączona.

### <a name="overwrite-synchronized-passwords"></a>Zastąp synchronizowane hasła
Administrator może ręcznie zresetować hasło przy użyciu programu Windows PowerShell.

W takim przypadku nowe hasło zastępuje synchronizowane hasła, a wszystkie zasady haseł zdefiniowane w chmurze są stosowane do nowego hasła.

Jeśli zmienisz hasło lokalne ponownie nowe hasło jest zsynchronizowany z chmurą i zastępuje on ręcznie zaktualizowanego hasła.

Synchronizacja hasła nie ma wpływu na użytkowników platformy Azure, który jest zalogowany. Zmiany hasła zsynchronizowane, która występuje, gdy użytkownik jest zalogowany do usługi w chmurze nie występuje od razu w bieżącej sesji usługi chmury. KMSI rozszerza czas trwania tej różnicy. Gdy usługa w chmurze, należy uwierzytelnić się ponownie, należy podać nowe hasło.

### <a name="additional-advantages"></a>Dodatkowe korzyści

- Ogólnie rzecz biorąc synchronizacja haseł jest prostsza do zaimplementowania niż usługi federacyjnej. Nie wymaga żadnych dodatkowych serwerów i eliminuje zależność od usługi federacyjnej wysokiej dostępności do uwierzytelniania użytkowników. 
- Synchronizację haseł można również włączyć oprócz federacji. Mogą być używane jako rezerwowe, jeśli usługi federacyjnej ulegnie awarii.












## <a name="enable-password-synchronization"></a>Włączanie synchronizacji haseł
Po zainstalowaniu usługi Azure AD Connect przy użyciu **ustawień ekspresowych** opcji synchronizacji haseł jest automatycznie włączone. Aby uzyskać więcej informacji, zobacz [wprowadzenie do korzystania z usługi Azure AD Connect przy użyciu ustawień ekspresowych](active-directory-aadconnect-get-started-express.md).

Jeśli używasz ustawienia niestandardowe, po zainstalowaniu usługi Azure AD Connect, synchronizacja haseł jest dostępne na stronie logowania użytkownika. Aby uzyskać więcej informacji, zobacz [Instalacja niestandardowa programu Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

![Włączanie synchronizacji haseł](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Synchronizacja haseł i FIPS
Jeśli serwer został zablokowany zgodnie z przetwarzania Standard FIPS (Federal Information), MD5 jest wyłączone.

**Aby włączyć MD5, aby synchronizować hasła, wykonaj następujące czynności:**

1. Przejdź do Sync\Bin %programfiles%\Azure AD.
2. Otwórz miiserver.exe.config.
3. Przejdź do węzła konfiguracji/runtime na końcu pliku.
4. Dodaj następującego węzła:`<enforceFIPSPolicy enabled="false"/>`
5. Zapisz zmiany.

Odwołanie ta Wstawka kodu jest jak go powinna wyglądać:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Aby uzyskać informacje o zabezpieczeniach i FIPS, zobacz [zgodności AAD Sync hasła, szyfrowania i FIPS](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/).

## <a name="troubleshoot-password-synchronization"></a>Rozwiązywanie problemów z synchronizacją haseł
Jeśli masz problemy z synchronizacją haseł, zobacz [Rozwiązywanie problemów z synchronizacją hasła](active-directory-aadconnectsync-troubleshoot-password-synchronization.md).

## <a name="next-steps"></a>Kolejne kroki
* [Synchronizacja programu Azure AD Connect: Dostosowywanie opcji synchronizacji](active-directory-aadconnectsync-whatis.md)
* [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md)
