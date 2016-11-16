---
title: "Azure AD Connect: integrowanie tożsamości lokalnych z usługą Azure Active Directory. | Microsoft Docs"
description: "Program Azure AD Connect umożliwia integrowanie katalogów lokalnych z usługą Azure Active Directory. Dzięki temu można posługiwać się wspólną tożsamością dla usługi Office 365, platformy Azure i aplikacji SaaS zintegrowanych z usługą Azure AD."
keywords: "wprowadzenie do programu Azure AD Connect, omówienie programu Azure AD Connect, co to jest program Azure AD Connect, instalowanie usługi Active Directory"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/04/2016
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: eedb788b2a174d01a2ef661cf4093ff938649bce


---
# <a name="integrating-your-onpremises-identities-with-azure-active-directory"></a>Integrowanie tożsamości lokalnych z usługą Azure Active Directory
Program Azure AD Connect umożliwia integrowanie katalogów lokalnych z usługą Azure Active Directory. Dzięki temu użytkownicy mogą posługiwać się wspólną tożsamością dla usługi Office 365, platformy Azure i aplikacji SaaS zintegrowanych z usługą Azure AD. W tym temacie przedstawiono kroki związane z planowaniem, wdrażaniem i obsługą. Zawiera on zbiór linków do tematów związanych z tym obszarem.

> [!IMPORTANT]
> [Użycie programu Azure AD Connect to najlepszy sposób na połączenie katalogu lokalnego z usługami Azure AD i Office 365. Jest to doskonały moment na uaktualnienie do programu Azure AD Connect z narzędzia Windows Azure Active Directory Sync (DirSync) lub Azure AD Sync, ponieważ są one przestarzałe, a wsparcie dla nich zakończy się 13 kwietnia 2017 r.](active-directory-aadconnect-dirsync-deprecated.md)
> 
> 

![Co to jest program Azure AD Connect](./media/active-directory-aadconnect/arch.png)

## <a name="why-use-azure-ad-connect"></a>Dlaczego warto korzystać z programu Azure AD Connect
Zintegrowanie katalogów lokalnych z usługą Azure AD zwiększa produktywność użytkowników, zapewniając wspólną tożsamość na potrzeby dostępu do zasobów, zarówno lokalnych, jak i w chmurze. Użytkownicy i organizacje uzyskują następujące korzyści:

* Użytkownicy mogą korzystać z jednej tożsamości w celu uzyskiwania dostępu do aplikacji lokalnych oraz usług w chmurze, takich jak Office 365.
* Jedno narzędzie zapewniające łatwe w użyciu środowisko wdrażania na potrzeby synchronizacji i logowania.
* Najnowsze możliwości we wszystkich scenariuszach. Program Azure AD Connect zastępuje starsze wersje narzędzi do integracji tożsamości, takie jak DirSync i Azure AD Sync. Aby uzyskać więcej informacji, zobacz [Porównanie narzędzi do integracji katalogów tożsamości hybrydowej](active-directory-hybrid-identity-design-considerations-tools-comparison.md).

### <a name="how-azure-ad-connect-works"></a>Jak działa program Azure AD Connect
Program Azure Active Directory Connect obejmuje trzy główne składniki: usługi synchronizacji, opcjonalny składnik usług federacyjnych Azure Directory oraz składnik monitorowania o nazwie [Azure AD Connect Health](active-directory-aadconnect-health.md).

<center>![Stos programu Azure AD Connect](./media/active-directory-aadconnect-how-it-works/AADConnectStack2.png)
</center>

* Synchronizacja — ten składnik odpowiada za tworzenie użytkowników, grup i innych obiektów. Odpowiada także za zapewnienie zgodności informacji o tożsamości lokalnych użytkowników i grup z informacjami w chmurze.
* AD FS — usługi federacyjne to opcjonalny składnik programu Azure AD Connect, za pomocą którego można skonfigurować środowisko hybrydowe przy użyciu lokalnej infrastruktury usług AD FS. Za jego pomocą organizacje mogą obsługiwać złożone wdrożenia, na przykład obejmujące logowanie jednokrotne w domenie, wymuszanie stosowania zasad logowania usługi AD lub korzystanie z karty inteligentnej bądź usługi MFA innych firm.
* Monitorowanie kondycji — składnik Azure AD Connect Health zapewnia zaawansowane monitorowanie z możliwością wyświetlania aktywności w centralnej lokalizacji w portalu Azure. Aby uzyskać więcej informacji, zobacz [Azure Active Directory Connect Health](active-directory-aadconnect-health.md).

## <a name="install-azure-ad-connect"></a>Instalowanie programu Azure AD Connect
Program Azure AD Connect można pobrać z [Centrum pobierania Microsoft](http://go.microsoft.com/fwlink/?LinkId=615771).

| Rozwiązanie | Scenariusz |
| --- | --- |
| Przed rozpoczęciem — [Sprzęt i wymagania wstępne](active-directory-aadconnect-prerequisites.md) |<li>Czynności, które należy wykonać przed rozpoczęciem instalacji programu Azure AD Connect.</li> |
| [Ustawienia ekspresowe](connect/active-directory-aadconnect-get-started-express.md) |<li>Jeśli masz usługę AD z jednym lasem, jest to zalecana opcja.</li> <li>Użytkownicy logują się przy użyciu tego samego hasła dzięki synchronizacji haseł.</li> |
| [Ustawienia dostosowane](connect/active-directory-aadconnect-get-started-custom.md) |<li>Opcja używana, gdy masz większą liczbę lasów. Obsługuje wiele [topologii](active-directory-aadconnect-topologies.md) lokalnych.</li> <li>Możesz dostosować opcje logowania, takie jak usługi federacyjne AD FS lub inny dostawca tożsamości.</li> <li>Możesz dostosować funkcje synchronizacji, na przykład filtrowanie i zapisywanie zwrotne.</li> |
| [Uaktualnianie z narzędzia DirSync](connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) |<li>Używane, jeśli masz już działający serwer DirSync.</li> |
| [Uaktualnienie z narzędzia Azure AD Sync lub Azure AD Connect](active-directory-aadconnect-upgrade-previous-version.md) |<li>Istnieje kilka różnych metod do wyboru w zależności od preferencji.</li> |

[Po instalacji](active-directory-aadconnect-whats-next.md) należy sprawdzić, czy wszystko działa zgodnie z oczekiwaniami, i przydzielić licencje do użytkowników.

### <a name="next-steps-to-install-azure-ad-connect"></a>Następne kroki instalowania programu Azure AD Connect
| Temat |
| --- | --- |
| Pobieranie programu Azure AD Connect |
| Instalowanie przy użyciu ustawień ekspresowych |
| Instalowanie przy użyciu ustawień dostosowanych |
| Uaktualnianie przy użyciu narzędzia DirSync |
| Po instalacji |

### <a name="learn-more-about-install-azure-ad-connect"></a>Więcej informacji o instalowaniu programu Azure AD Connect
Warto również przygotować się na kwestie związane z [działaniem](active-directory-aadconnectsync-operations.md). Może być wskazane zastosowanie serwera zapasowego umożliwiającego łatwe przełączenie w przypadku [awarii](active-directory-aadconnectsync-operations.md#disaster-recovery). Jeśli planujesz częste wprowadzanie zmian konfiguracji, należy zaplanować serwer [trybu przejściowego](active-directory-aadconnectsync-operations.md#staging-mode).

| Temat |
| --- | --- |
| Obsługiwane topologie |
| Zagadnienia dotyczące projektowania |
| Konta używane do instalacji |
| Planowanie operacyjne |
| Opcje logowania użytkowników |

## <a name="configure-sync-features"></a>Konfigurowanie funkcji synchronizacji
Program Azure AD Connect zawiera szereg funkcji, które są domyślnie włączone lub które można włączyć opcjonalnie. Niektóre funkcje mogą wymagać wykonania dodatkowych czynności konfiguracyjnych w określonych scenariuszach i topologiach.

[Filtrowanie](active-directory-aadconnectsync-configure-filtering.md) umożliwia ograniczenie zakresu obiektów synchronizowanych z usługą Azure AD. Domyślnie synchronizacja obejmuje wszystkich użytkowników, kontakty, grupy i komputery z systemem Windows 10. Możesz zmienić ustawienia filtrowania na podstawie domen, jednostek organizacyjnych lub atrybutów.

[Synchronizacja haseł](active-directory-aadconnectsync-implement-password-synchronization.md) umożliwia synchronizowanie skrótów haseł w usłudze Active Directory z usługą Azure AD. Użytkownik końcowy może korzystać z tego samego hasła lokalnie i w chmurze, zarządzając nim w jednej lokalizacji. Ponieważ źródłem jest lokalna usługa Active Directory, można używać również własnych zasad haseł.

[Zapisywanie zwrotne haseł](active-directory-passwords-getting-started.md) umożliwia użytkownikom zmienianie i resetowanie haseł w chmurze i stosowanie lokalnych zasad haseł.

[Zapisywanie zwrotne urządzeń](active-directory-aadconnect-feature-device-writeback.md) umożliwia zapisywanie w lokalnej usłudze Active Directory urządzeń zarejestrowanych w usłudze Azure AD w celu użycia ich na potrzeby dostępu warunkowego.

Funkcja [zapobiegania przypadkowemu usuwaniu](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) jest domyślnie włączona i zabezpiecza katalog w chmurze przed usunięciem dużej liczby elementów jednocześnie. Domyślnie dozwolone jest usunięcie 500 elementów w jednym przebiegu. Możesz zmienić to ustawienie w zależności od wielkości organizacji.

[Automatyczne uaktualnianie](active-directory-aadconnect-feature-automatic-upgrade.md) jest domyślnie włączone w przypadku instalacji z ustawieniami ekspresowymi i zapewnia korzystanie z najnowszej wersji programu Azure AD Connect.

### <a name="next-steps-to-configure-sync-features"></a>Następne kroki konfigurowania funkcji synchronizacji
| Temat |
| --- | --- |
| Konfigurowanie filtrowania |
| Synchronizacja haseł |
| Zapisywanie zwrotne haseł |
| Zapisywanie zwrotne urządzeń |
| Zapobieganie przypadkowemu usuwaniu |
| Automatycznie uaktualnianie |

## <a name="customize-azure-ad-connect-sync"></a>Dostosowywanie synchronizacji w programie Azure AD Connect
Składnik synchronizacji programu Azure AD Connect ma konfigurację domyślną, która powinna działać w przypadku większości klientów i topologii. Zawsze jednak mogą występować sytuacje, w których konfiguracja domyślna nie sprawdza się i wymaga dostosowania. Obsługiwane jest wprowadzanie zmian zgodnie z opisem w tej sekcji i w połączonych tematach.

Jeśli nie masz doświadczenia z topologią synchronizacji, warto zapoznać się z podstawowymi informacjami i używanymi pojęciami przedstawionymi w temacie [zagadnienia techniczne](active-directory-aadconnectsync-technical-concepts.md). Program Azure AD Connect powstał na podstawie narzędzi MIIS2003, ILM2007 i FIM2010. Mimo że niektóre elementy są identyczne, wprowadzono także wiele zmian.

[Konfiguracja domyślna](active-directory-aadconnectsync-understanding-default-configuration.md) dopuszcza istnienie więcej niż jednego lasu w konfiguracji. W takich topologiach obiekt użytkownika może być reprezentowany jako kontakt w innym lesie. Użytkownik może także mieć połączoną skrzynkę pocztową w innym lesie zasobów. Działanie konfiguracji domyślnej przedstawiono w temacie [użytkownicy i kontakty](active-directory-aadconnectsync-understanding-users-and-contacts.md).

Model konfiguracji synchronizacji jest nazywany [aprowizacją deklaratywną](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md). Zaawansowane przepływy atrybutów używają [funkcji](active-directory-aadconnectsync-functions-reference.md) do wyrażania przekształceń atrybutów. Całą konfigurację można wyświetlić i sprawdzić za pomocą narzędzi dostarczanych wraz z programem Azure AD Connect. Jeśli konieczne jest wprowadzenie zmian w konfiguracji, pamiętaj o postępowaniu zgodnie z [najlepszymi rozwiązaniami](active-directory-aadconnectsync-best-practices-changing-default-configuration.md), co ułatwi przyjmowanie nowych wersji.

### <a name="next-steps-to-customize-azure-ad-connect-sync"></a>Następne kroki dostosowywania synchronizacji w programie Azure AD Connect
| Temat |
| --- | --- |
| Wszystkie artykuły dotyczące synchronizacji programu Azure AD Connect |
| Zagadnienia techniczne |
| Opis konfiguracji domyślnej |
| Opis użytkowników i kontaktów |
| Aprowizacja deklaratywna |
| Zmienianie konfiguracji domyślnej |

## <a name="configure-federation-features"></a>Konfigurowanie funkcji federacyjnych
Usługi AD FS można skonfigurować pod kątem obsługi [wielu domen](active-directory-aadconnect-multiple-domains.md). Możesz na przykład mieć wiele domen najwyższego poziomu, których musisz użyć na potrzeby federacji.

Jeśli serwer usług AD FS nie został skonfigurowany tak, aby automatycznie aktualizował certyfikaty z usługi Azure AD, lub jeśli korzystasz z rozwiązania innego niż AD FS, otrzymasz powiadomienie o konieczności [zaktualizowania certyfikatów](active-directory-aadconnect-o365-certs.md).

### <a name="next-steps-to-configure-federation-features"></a>Następne kroki konfigurowania funkcji federacyjnych
| Temat |
| --- | --- |
| Wszystkie artykuły dotyczące usług AD FS |
| Konfigurowanie usług AD FS z poddomenami |
| Zarządzanie farmą usług AD FS |
| Ręczne aktualizowanie certyfikatów federacji |

## <a name="more-information-and-references"></a>Więcej informacji i materiały referencyjne
| Temat |
| --- | --- |
| Historia wersji |
| Porównanie narzędzi DirSync, Azure ADSync i Azure AD Connect |
| Lista zgodności usługi Azure AD z usługami innymi niż AD FS |
| Synchronizowane atrybuty |
| Monitorowanie za pomocą narzędzia Azure AD Connect Health |
| Często zadawane pytania |

**Dodatkowe zasoby**

Prezentacja z konferencji Ignite 2015 dotycząca rozszerzania katalogów lokalnych do chmury.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3862/player]
> 
> 




<!--HONumber=Nov16_HO2-->


