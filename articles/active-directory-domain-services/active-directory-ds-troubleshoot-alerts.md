---
title: "Azure Active Directory Domain Services: Rozwiązywanie problemów z alertami | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów z alerty dla usług domenowych Azure AD"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: ergreenl
ms.openlocfilehash: b2e0edf3588f3b1db5f4b6641019be1ded9cb50e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Usługi domenowe Azure AD — rozwiązywanie alertów
Ten artykuł zawiera przewodniki dotyczące rozwiązywania problemów w ramach wszystkich alertów, które mogą występować w domenie zarządzanej.


Wybierz kroki rozwiązywania problemów, które odpowiadają lub alertu identyfikator lub wiadomości, które wystąpią.

| **Identyfikator alertu** | **Komunikat alertu** | **Rozdzielczość** |
| --- | --- | :--- |
| AADDS001 | *Bezpiecznego protokołu LDAP, za pośrednictwem Internetu jest włączona dla domeny zarządzanej. Dostęp do portu 636 nie jest zablokowany w dół za pomocą grupy zabezpieczeń sieci. Ta operacja może narazić kont użytkowników w domenie zarządzanej do ataków siłowych hasła.* | [Nieprawidłowa konfiguracja bezpiecznego protokołu LDAP](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *Katalog usługi Azure AD skojarzony z domeny zarządzanej mogła zostać usunięta. Domeny zarządzanej nie jest już w obsługiwanej konfiguracji. Microsoft nie monitorowanie, zarządzanie, zastosować poprawki i synchronizacji domeny zarządzanej.* | [Brakujący katalog](#aadds100-missing-directory) |
| AADDS101 | *Nie można włączyć usługi domenowe Azure AD w katalogu usługi Azure AD B2C.* | [Usługa Azure AD B2C jest uruchomiona w tym katalogu](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Wymagane dla usług domenowych Azure AD do prawidłowej nazwy głównej usługi został usunięty z katalogu usługi Azure AD. Ta konfiguracja ma wpływ na zdolność firmy Microsoft do monitorowania, zarządzania i poprawki i zsynchronizować domeny zarządzanej.* | [Brak nazwy głównej usługi](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *Zakres adresów IP dla sieci wirtualnej, w której włączono usługi domenowe Azure AD jest w zakresie publicznego adresu IP. Usługi domenowe Azure AD musi być włączony w sieci wirtualnej z zakresu prywatnych adresów IP. Ta konfiguracja ma wpływ na zdolność firmy Microsoft do monitorowania, zarządzania i poprawki i zsynchronizować domeny zarządzanej.* | [Adres jest w zakresie publicznego adresu IP](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Firma Microsoft dokłada nawiązać kontrolery domeny dla tej domeny zarządzanej. Może się tak zdarzyć, jeśli grupa zabezpieczeń sieci (NSG) skonfigurowany na Twojej sieci wirtualnej blokuje dostęp do domeny zarządzanej. Inny możliwych przyczyn jest Brak trasy zdefiniowane przez użytkownika tego blokuje ruch przychodzący z Internetu.* | [Błąd sieci](active-directory-ds-troubleshoot-nsg.md) |

## <a name="aadds100-missing-directory"></a>AADDS100: Brak katalogu
**Komunikat alertu:**

*Katalog usługi Azure AD skojarzony z domeny zarządzanej mogła zostać usunięta. Domeny zarządzanej nie jest już w obsługiwanej konfiguracji. Microsoft nie monitorowanie, zarządzanie, zastosować poprawki i synchronizacji domeny zarządzanej.*

**Środki zaradcze:**

Przyczyną tego błędu jest zwykle nieprawidłowo przenoszenie subskrypcji platformy Azure do nowej usługi Azure AD directory i usuwanie starego katalogu usługi Azure AD, który jest nadal skojarzone z usług domenowych Azure AD.

Ten błąd jest nieodwracalny. Aby rozwiązać, należy [usunąć istniejące domeny zarządzanej](active-directory-ds-disable-aadds.md) i utwórz ją ponownie w katalogu nowe. W przypadku usuwania problemów, skontaktuj się z zespołem produktu usługi Azure Active Directory Domain Services [obsługi](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: W tym katalogu jest uruchomiona usługa Azure AD B2C
**Komunikat alertu:**

*Nie można włączyć usługi domenowe Azure AD w katalogu usługi Azure AD B2C.*

**Środki zaradcze:**

>[!NOTE]
>Aby kontynuować korzystanie z usług domenowych Azure AD, należy ponownie utworzyć wystąpienie usług domenowych Azure AD w katalogu — z usługi Azure AD B2C.

Aby przywrócić usługę, wykonaj następujące kroki:

1. [Usuwanie domeny zarządzanej](active-directory-ds-disable-aadds.md) z istniejącego katalogu usługi Azure AD.
2. Utwórz nowy katalog, który nie jest katalogiem usługi Azure AD B2C.
3. Postępuj zgodnie z [wprowadzenie](active-directory-ds-getting-started.md) przewodnika, aby odtworzyć domeny zarządzanej.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: Adres znajduje się w zakresie IP

**Komunikat alertu:**

*Zakres adresów IP dla sieci wirtualnej, w której włączono usługi domenowe Azure AD jest w zakresie publicznego adresu IP. Usługi domenowe Azure AD musi być włączony w sieci wirtualnej z zakresu prywatnych adresów IP. Ta konfiguracja ma wpływ na zdolność firmy Microsoft do monitorowania, zarządzania i poprawki i zsynchronizować domeny zarządzanej.*

**Środki zaradcze:**

> [!NOTE]
> Aby rozwiązać ten problem, należy usunąć istniejące domeny zarządzanej i utwórz go ponownie w sieci wirtualnej z zakresu prywatnych adresów IP. Ten proces jest destrukcyjne.

Przed rozpoczęciem przeczytaj **prywatnej przestrzeń adresową v4** sekcji [w tym artykule](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

1. [Usuwanie domeny zarządzanej](active-directory-ds-disable-aadds.md) z katalogu.
2. Usuń zakres adresów IP podsieci
  1. Przejdź do [strony sieci wirtualnych w portalu Azure](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
  2. Wybierz sieć wirtualną, której planujesz używać usług domenowych Azure AD.
  3. Polecenie **przestrzeni adresów** w obszarze Ustawienia
  4. Zaktualizuj zakres adresów klikając istniejący zakres adresów, a jego edycji lub dodawanie dodatkowy zakres adresów. Upewnij się, że nowy zakres adresów jest w zakresie prywatnego adresu IP. Zapisz zmiany.
  5. Polecenie **podsieci** w obszarze nawigacji po lewej stronie.
  6. Kliknij podsieć, którą chcesz edytować w tabeli.
  7. Zaktualizuj zakres adresów, a następnie zapisz zmiany.
3. Postępuj zgodnie z [przewodnik wprowadzenie uruchomiony przy użyciu usług domenowych Azure AD](active-directory-ds-getting-started.md) Aby odtworzyć domeny zarządzanej. Upewnij się, że możesz wybrać sieć wirtualną z zakresu prywatnych adresów IP.
4. Aby przyłączenie do domeny maszyn wirtualnych do nowej domeny, wykonaj [w tym przewodniku](active-directory-ds-admin-guide-join-windows-vm-portal.md).
8. Sprawdź kondycję Twojej domeny w dwóch godzin, aby upewnić się, że kroki zostały wykonane prawidłowo.


## <a name="contact-us"></a>Skontaktuj się z nami
Skontaktuj się z zespołem produktu usługi Azure Active Directory Domain Services, aby [udostępnić opinię lub pomocy technicznej](active-directory-ds-contact-us.md).
