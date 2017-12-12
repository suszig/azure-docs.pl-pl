---
title: "Azure Active Directory Connect: Często zadawane pytania dotyczące - | Dokumentacja firmy Microsoft"
description: "Ta strona zawiera często zadawane pytania dotyczące usługi Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2017
ms.author: billmath
ms.openlocfilehash: 13f12f39fb72bd7d4e32056753a012c38034f807
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a>Często zadawane pytania dotyczące usługi Azure Active Directory Connect

## <a name="general-installation"></a>Ogólne instalacji
**Pytanie: będzie instalacji działać, jeśli administrator globalny usługi Azure AD ma 2FA włączone?**  
Z kompilacji w lutym 2016 r. jest to obsługiwane.

**Pytanie: czy istnieje sposób, aby zainstalować program Azure AD Connect nienadzorowanej?**  
Możliwe jest tylko zainstalować program Azure AD Connect przy użyciu Kreatora instalacji. Instalacja nienadzorowana i dyskretnej nie jest obsługiwana.

**Pytanie: czy masz lasu, gdy nie można skontaktować się z jednej domeny. Jak zainstalować usługi Azure AD Connect?**  
Z kompilacji w lutym 2016 r. jest to obsługiwane.

**Pytanie: agenta programu health usług AD DS działa w instalacji server core?**  
Tak. Po zainstalowaniu agenta, można ukończyć procesu rejestracji, za pomocą następującego polecenia cmdlet programu PowerShell: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**Pytanie: czy programu AADConnect obsługuje synchronizowane z obiema domenami do na usługi Azure AD?**</br>
Tak, takie rozwiązanie jest obsługiwane. Zapoznaj się [wielu domen](active-directory-aadconnect-multiple-domains.md)
 
**Pytanie: czy obsługujemy mających wiele łączników dla tej samej domeny usługi Active Directory w usłudze Azure AD connect?**</br> Nie, to nie jest obsługiwane 

## <a name="network"></a>Sieć
**Pytanie: czy mam zapory, urządzenia sieciowego lub czegoś innego, która ogranicza maksymalny czas połączenia pozostają otwarte w sieci. Jak długo Moje próg limitu czasu po stronie klienta należy przy użyciu usługi Azure AD Connect?**  
Całe oprogramowanie sieciowe, urządzenia fizyczne lub czymkolwiek innym, która ogranicza maksymalny czas połączenia może pozostawać otwarte, powinna używać progu co najmniej 5 minut (300 sekund) dla połączenia między serwerami, na którym jest zainstalowany klient programu Azure AD Connect i Usługa Azure Active Directory. Dotyczy to również wszystkich wydanych narzędzia synchronizacji pakietu Microsoft Identity.

**Pytanie: czy domeny drugiego poziomu (jednej etykiety domen) obsługiwane?**  
Nie, usługi Azure AD Connect nie obsługuje lokalnymi lasami/domenami przy użyciu drugiego poziomu.

**Pyt.: czy lasów domenom AD rozłącznego obsługiwane?**  
Nie, usługi Azure AD Connect nie obsługuje lokalnymi lasami zawierający rozłączne przestrzenie nazw.

**Pytanie: czy "kropkami" o nazwie NetBios obsługiwane?**  
Nie, usługi Azure AD Connect nie obsługuje lokalnymi lasami/domenami, których nazwa NetBios zawiera kropkę "." w nazwie.

**Pytanie: jest czysty środowisko IPv6 obsługiwane?**  
Nie, usługi Azure AD Connect nie obsługuje czystego środowiska protokołu IPv6.

## <a name="federation"></a>Federacja
**Pytanie: co należy zrobić, jeśli otrzymuję wiadomość e-mail, który monit o odnawiania certyfikatu usługi Office 365**  
Użyj wskazówek opisaną w [odnawiania certyfikatów](active-directory-aadconnect-o365-certs.md) temacie dotyczące sposobu odnawiania certyfikatu.

**Pytanie: czy masz "Automatycznie Aktualizuj jednostki uzależnionej" dla jednostki uzależnionej usługi Office 365. Czy muszę wykonywać żadnych czynności w przypadku, gdy mój token certyfikatu podpisywania automatycznie najedzie na?**  
Użyj wskazówek opisaną w artykule [odnawiania certyfikatów](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Środowisko
**Q: Zmień nazwę serwera, po zainstalowaniu usługi Azure AD Connect jest obsługiwane?**  
Nie. Zmiana nazwy serwera spowoduje, że aparat synchronizacji nie będą mogli nawiązywać połączeń z bazą danych SQL i usługa nie będzie możliwe jej uruchomienie.

## <a name="identity-data"></a>Danych tożsamości
**Pyt.: atrybut nazwy UPN (userPrincipalName) w usłudze Azure AD nie odpowiada UPN lokalnego — Dlaczego?**  
Zobacz następujące artykuły:

* [Niezgodne nazwy użytkowników w usłudze Office 365, Azure lub Intune lokalną nazwą UPN lub alternatywnym Identyfikatorem logowania](https://support.microsoft.com/en-us/kb/2523192)
* [Zmiany nie są synchronizowane przez narzędzie do synchronizacji usługi Azure Active Directory po zmianie nazwy UPN konta użytkownika, aby użyć innej domeny federacyjnej](https://support.microsoft.com/en-us/kb/2669550)

Można również skonfigurować usługi Azure AD, aby umożliwić aparatu synchronizacji można zaktualizować właściwości userPrincipalName zgodnie z opisem w [funkcji Usługa synchronizacji Azure AD Connect](active-directory-aadconnectsyncservice-features.md).

**P: jest obsługiwane nietrwałego dopasowania grupy AD skontaktuj się z obiekty z istniejących obiektów usługi Azure AD grupy/skontaktuj się z lokalnymi?**  
Nie jest to obecnie nieobsługiwane.

**Pytanie: czy jest ona obsługiwana na ręczne ustawienie atrybutu nazwę ImmutableId w istniejących obiektach Azure AD grupy/skontaktuj się z twardego dopasowanie do obiektów grupy AD skontaktuj się z lokalnymi?**  
Nie jest to obecnie nieobsługiwane.

## <a name="custom-configuration"></a>Konfiguracja niestandardowa
**Pytanie: gdzie są udokumentowane poleceń cmdlet programu PowerShell dla usługi Azure AD Connect**  
Z wyjątkiem poleceń cmdlet opisane w tej lokacji innych poleceń cmdlet programu PowerShell znaleziono w programie Azure AD Connect nie są obsługiwane przez klienta.

**Pytanie: czy można użyć "Serwera serwer eksportu/importu" znaleziony w *Menedżera usługi synchronizacji* na przenoszenie konfiguracji między serwerami?**  
Nie. Ta opcja nie pobierze wszystkie ustawienia konfiguracji i nie powinna być używana. Zamiast tego należy użyć Kreatora tworzenia konfiguracji podstawowej na drugim serwerze i generowania skryptów programu PowerShell, aby przenieść wszystkie reguły niestandardowej między serwerami za pomocą edytora reguły synchronizacji. Zobacz [migracji w kierunku](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**Pytanie: hasła można buforować Azure strony logowania i może to być zablokowana, ponieważ zawiera on element input hasła z automatycznego uzupełniania = "false" atrybutu?**</br>
Obecnie nie obsługujemy modyfikowanie atrybutów HTML hasła pola wejściowego, tym autocomplete tag. Obecnie pracujemy nad elementem, który umożliwia niestandardowych skryptów Javascript, której będzie można dodać atrybutu do pole hasła. Powinna to być nowsze część 2017 r.

**Pytanie: czy w witrynie Azure logowania są wyświetlane nazwy użytkowników dla użytkowników, którzy wcześniej zalogowali się pomyślnie.  Można to zachowanie wyłączyć?**</br>
Obecnie nie obsługujemy modyfikowania atrybutów HTML strony logowania. Obecnie pracujemy nad elementem, który umożliwia niestandardowych skryptów Javascript, której będzie można dodać atrybutu do pole hasła. Powinna to być nowsze część 2017 r.

**Pytanie: czy istnieje sposób, aby zapobiec równoczesnych sesji?**</br>
Nie.

## <a name="troubleshooting"></a>Rozwiązywanie problemów
**Pytanie: jak uzyskać pomoc dotyczącą usługi Azure AD Connect?**

[Wyszukaj w bazie wiedzy Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Wyszukiwanie Microsoft Knowledge Base (KB) dla techniczne rozwiązania typowych problemów naprawa w razie awarii o obsłudze programu Azure AD Connect.

[Fora dotyczące usługi Active Directory platformy Microsoft Azure](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Można wyszukiwać i wyszukać technicznych pytania i odpowiedzi od społeczności lub zadać pytanie własne klikając [tutaj](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Jak uzyskać pomoc techniczną dotyczącą usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)

* Aby uzyskać pomoc techniczną za pośrednictwem portalu Azure, użyj tego łącza.

