---
title: "Dostęp warunkowy usługi Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Wyszukuj określonych warunków podczas uwierzytelniania w celu uzyskania dostępu do aplikacji za pomocą kontroli dostępu warunkowego w usłudze Azure Active Directory."
services: active-directory
keywords: "dostęp warunkowy do aplikacji, dostęp warunkowy przy użyciu usługi Azure AD, bezpieczny dostęp do zasobów firmy, zasady dostępu warunkowego"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/07/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 40849e7f0c8a76bdd8a0e03d0780534569d9874a
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/08/2017
---
# <a name="conditional-access-in-azure-active-directory"></a>Dostęp warunkowy w usłudze Azure Active Directory

W świecie pierwszy mobile, najpierw chmury Azure Active Directory umożliwia logowanie jednokrotne do urządzeń, aplikacji i usług z dowolnego miejsca. Rosnąca liczba urządzeń (takich jak BYOD), będzie działać poza siedzibą, a 3 stron aplikacji SaaS, specjalistów IT muszą stawiać czoła dwóch celów przeciwna:

- Zwiększenie możliwości dostępnych dla użytkowników końcowych do produktywność wszędzie tam, gdzie i zawsze, gdy
- Ochrona zasobów firmy w dowolnym momencie

Aby zwiększyć wydajność, Azure Active Directory oferuje użytkownikom szeroki zakres opcji, aby uzyskać dostępu do zasobów firmowych. Zarządzanie aplikacjami dostępu do usługi Azure Active Directory umożliwia upewnij się, że tylko *odpowiednich osób* mogą uzyskiwać dostęp do aplikacji. Co zrobić, jeśli chcesz mieć większą kontrolę nad jak odpowiednich osób uzyskują dostęp do zasobów w niektórych warunkach? Co zrobić, jeśli jeszcze masz warunków, w których chcesz zablokować dostęp do niektórych aplikacji, nawet w przypadku *prawym przyciskiem myszy osób*? Na przykład może być OK dla Ciebie Jeżeli odpowiednich osób uzyskuje dostęp do niektórych aplikacji z sieci zaufanej. jednak nie można ich dostęp do tych aplikacji z sieci, której nie ufasz. Można rozwiązać te pytania, przy użyciu dostępu warunkowego.

Dostęp warunkowy jest możliwość usługi Azure Active Directory, która umożliwia wymuszanie kontroli dostępu do aplikacji w danym środowisku, na podstawie określonych warunków. Funkcje kontroli albo może powiązać dodatkowe wymagania dotyczące dostępu lub można go zablokować. Implementacja dostępu warunkowego jest oparta na zasadach. Podejście oparte na zasadach upraszcza środowiska konfiguracji, ponieważ wynika sposób myślisz na temat wymagań dostępu.  

Zazwyczaj określa wymagań dostępu za pomocą instrukcji, które są oparte na następujący wzór:

![Formant](./media/active-directory-conditional-access-azure-portal/10.png)

Gdy Zastąp dwa wystąpienia "*to*" informacje rzeczywistych ma przykład deklaracji zasad, który prawdopodobnie wygląda bardzo podobne do:

*Podczas wykonawców dostęp do naszej aplikacji w chmurze z sieci, które nie są zaufane następnie zablokować dostęp.*

Powyżej deklaracji zasad prezentuje możliwości dostępu warunkowego. Podczas gdy można włączyć wykonawców do zasadniczo dostęp do aplikacji w chmurze (**kto**), przy użyciu dostępu warunkowego, można również zdefiniować warunki, w których dostęp jest możliwe (**jak**).

W kontekście dostępu warunkowego dla usługi Azure Active Directory,

- "**w takim przypadku**" jest wywoływana **warunku — instrukcja**
- "**To zrobić**" jest wywoływana **formantów**

![Formant](./media/active-directory-conditional-access-azure-portal/11.png)

Kombinacja instrukcji warunku z formantów reprezentuje zasady dostępu warunkowego.

![Formant](./media/active-directory-conditional-access-azure-portal/12.png)


## <a name="controls"></a>Kontrolki

W zasadach dostępu warunkowego kontroli zdefiniować, co jest to, że powinno się zdarzyć, gdy instrukcja warunku zostały spełnione.  
Funkcje kontroli możesz zablokować dostęp lub zezwolić na dostęp z dodatkowymi wymaganiami.
Po skonfigurowaniu zasad, która zezwala na dostęp, musisz wybrać co najmniej jedno wymaganie.  

Istnieją dwa typy formantów: 

- **Formanty GRANT** -formanty Grant określającym, czy użytkownik może wykonać uwierzytelnianie i uzyskać dostęp do zasobu, którego one próbuje zalogować się do. Jeśli masz wiele formantów wybrana, możesz określić, czy wszystkie z nich są wymagane podczas przetwarzania zasad.
Bieżąca implementacja usługi Azure Active Directory można skonfigurować następujące wymagania dotyczące sterowania grant:

    ![Formant](./media/active-directory-conditional-access-azure-portal/73.png)

- **Formanty sesji** -sesji steruje Włącz ograniczanie doświadczenie w aplikacji w chmurze. Formanty sesji są wymuszane przez aplikacje w chmurze i polegać na dodatkowe informacje dostarczane przez usługę Azure AD do aplikacji o sesji.

    ![Formant](./media/active-directory-conditional-access-azure-portal/31.png)


Aby uzyskać więcej informacji, zobacz [formantów w usłudze Azure Active Directory dostępu warunkowego](active-directory-conditional-access-controls.md).


## <a name="condition-statement"></a>Instrukcja warunku

Poprzedniej sekcji wprowadziła Aby zablokować lub ograniczyć dostęp do zasobów w postaci formantów obsługiwane opcje. W zasadach dostępu warunkowego można zdefiniować kryteria, które muszą zostać spełnione dla formantów stosowanych w formie instrukcja warunku.  

Może obejmować następujące przypisania do instrukcji warunku:

![Formant](./media/active-directory-conditional-access-azure-portal/07.png)


### <a name="who"></a>Kto?

Po skonfigurowaniu zasad dostępu warunkowego, musisz wybrać użytkowników lub grup, które dotyczą zasady. W wielu przypadkach ma formantów ma zostać zastosowany do określonych użytkowników. W instrukcji warunku można zdefiniować tego zestawu, wybierając wymaganych użytkowników i grup, które dotyczą zasady. W razie potrzeby można również jawnie wykluczyć zbiór użytkowników z zasad, zwalniając je.  

![Formant](./media/active-directory-conditional-access-azure-portal/08.png)



### <a name="what"></a>Co?

Po skonfigurowaniu zasad dostępu warunkowego, musisz wybrać zasady stosowane do aplikacji w chmurze.
Zwykle Brak niektórych aplikacji w danym środowisku, z punktu widzenia ochrony wymagających uwagi więcej niż inne. Wpływa to na przykład aplikacje, które mają dostęp do poufnych danych.
Po wybraniu aplikacji w chmurze, należy zdefiniować zakres zasad ma zastosowanie do aplikacji w chmurze. W razie potrzeby można również jawnie wykluczyć zestawu aplikacji z zasad.

![Formant](./media/active-directory-conditional-access-azure-portal/09.png)

Aby uzyskać pełną listę aplikacji w chmurze można użyć w zasadach dostępu warunkowego, zobacz [informacje techniczne dotyczące usługi Azure Active Directory dostępu warunkowego](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).

### <a name="how"></a>W jaki sposób?

Tak długo, jak dostępu do aplikacji jest wykonywana w warunkach, które można kontrolować, może być potrzebę nakładające dodatkowe kontrole jak aplikacje w chmurze są dostępne dla użytkowników. Jednak czynności mogą wyglądać inaczej, jeśli dostęp do aplikacji w chmurze jest wykonywana, na przykład z sieci, które nie są zaufane lub urządzeń, które nie są zgodne. W instrukcji warunku można zdefiniować niektóre warunki dostępu, które mają dodatkowe wymagania dotyczące realizację dostępu do aplikacji.

![Warunki](./media/active-directory-conditional-access-azure-portal/01.png)


## <a name="conditions"></a>Warunki

W bieżącej implementacji usługi Azure Active Directory można określić warunki dla następujących obszarów:

- Ryzyko logowania
- Platformy urządzeń
- Lokalizacje
- Aplikacje klienta


![Warunki](./media/active-directory-conditional-access-azure-portal/01.png)

### <a name="sign-in-risk"></a>Ryzyko logowania

Ryzyko logowania jest obiekt, który jest używany przez usługę Azure Active Directory do śledzenia prawdopodobieństwo, że logowanie podejmować nie została wykonana przez wiarygodnego właściciela konta użytkownika. W tym obiekcie prawdopodobieństwo (wysoki, średni lub niski) są przechowywane w postaci atrybutu o nazwie [poziom ryzyka logowania](active-directory-reporting-risk-events.md#risk-level). Ten obiekt jest generowany podczas logowania użytkownika, jeśli logowanie ryzyka zostały wykryte przez usługę Azure Active Directory. Aby uzyskać więcej informacji, zobacz [Ryzykowne logowania](active-directory-identityprotection.md#risky-sign-ins).  
Poziom ryzyka obliczeniowej logowania można użyć jako warunek w zasadach dostępu warunkowego. 

![Warunki](./media/active-directory-conditional-access-azure-portal/22.png)

### <a name="device-platforms"></a>Platformy urządzeń

Platformy urządzeń charakteryzuje się systemu operacyjnego, który działa na twoim urządzeniu: można zdefiniować platformy urządzeń, które znajdują się, jak również platformy urządzeń, które są wykluczone z zasad.  
Do używania platformy urządzeń w zasadach, najpierw zmień przełączników Konfiguruj, aby **tak**, a następnie wybierz wszystkie lub platform urządzeń poszczególnych zasad dotyczy. W przypadku wybrania platformy poszczególnych urządzeń zasady ma wpływ tylko na tych platformach. W takim przypadku logowania do innych platform obsługiwanych przez zasady nie są zagrożone.


![Warunki](./media/active-directory-conditional-access-azure-portal/02.png)

Aby uzyskać pełną listę obsługiwanych platform urządzeń, zobacz [warunku platformy urządzenia](active-directory-conditional-access-technical-reference.md#device-platform-condition).


### <a name="locations"></a>Lokalizacje

Lokalizacje istnieje możliwość zdefiniowania warunków, które są oparte na której zainicjowano próby połączenia z. Wpisy na liście lokalizacje są albo **o nazwie lokalizacje** lub **MFA zaufanych adresów IP**.  

**O nazwie lokalizacje** to funkcja usługi Azure Active Directory, która pozwala na definiowanie etykiety dla podejmowano połączenia lokalizacji z. Aby określić lokalizację, albo można skonfigurować adresu IP zakresów adresów, lub wybierz kraj / region.  

![Warunki](./media/active-directory-conditional-access-azure-portal/42.png)

Ponadto można oznaczyć nazwanego lokalizacji jako zaufanej lokalizacji. Zasady dostępu warunkowego, zaufanej lokalizacji jest inną opcją filtr, który umożliwia wybranie *wszystkich zaufanych lokalizacji* w warunku lokalizacji.
Nazwane lokalizacje są również istotne w kontekście wykrywania [ryzyka zdarzenia](active-directory-reporting-risk-events.md) Aby zmniejszyć liczbę false alarmów dla niemożliwa podróż do nietypowych lokalizacji ryzyka zdarzenia. 

Liczba nazwane lokalizacje, które można skonfigurować jest ograniczona przez rozmiar obiektu pokrewnego w usłudze Azure AD. Można skonfigurować:
 
 - Jedną lokalizację o nazwie o maksymalnie 500 zakresów adresów IP
 - Maksymalnie 60 lokalizacji o nazwie (wersja zapoznawcza) jeden zakres adresów IP przypisanych do każdego z nich 

Aby uzyskać więcej informacji, zobacz [o nazwie lokalizacjach w usłudze Azure Active Directory](active-directory-named-locations.md).


**Uwierzytelnianie wieloskładnikowe zaufanych adresów IP** to funkcja usługi Multi-Factor Authentication, która umożliwia zdefiniowanie zaufanych zakresów adresów IP, reprezentujący lokalny intranet firmy. Po skonfigurowaniu warunek lokalizacji zaufanych adresów IP umożliwia rozróżnianie między połączenia z siecią organizacji i innych lokalizacji. Aby uzyskać więcej informacji, zobacz [zaufanych adresów IP](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  

W zasadach dostępu warunkowego można:

- Obejmują
    - Dowolnego miejsca.
    - Wszystkie zaufane lokalizacje
    - Wybranej lokalizacji
- Wyklucz
    - Wszystkie zaufane lokalizacje
    - Wybranej lokalizacji
     
![Warunki](./media/active-directory-conditional-access-azure-portal/03.png)


### <a name="client-apps"></a>Aplikacje klienta

Aplikacja kliencka mogą być na poziomie ogólnego aplikacji (przeglądarki sieci web, aplikacji mobilnej, klient usług pulpitu) zostały użyte do nawiązania połączenia usługi Azure Active Directory lub musisz wybrać programu Exchange Active Sync.  
Starsze uwierzytelnianie odnosi się do klientów przy użyciu uwierzytelniania podstawowego, takich jak starszych klientów pakietu Office, które nie używają nowoczesnego uwierzytelniania. Dostęp warunkowy nie jest obecnie obsługiwane przy użyciu starszej wersji uwierzytelniania.

![Warunki](./media/active-directory-conditional-access-azure-portal/04.png)


Aby uzyskać pełną listę aplikacji klienckich, których można używać w zasadach dostępu warunkowego, zobacz [informacje techniczne dotyczące usługi Azure Active Directory dostępu warunkowego](active-directory-conditional-access-technical-reference.md#client-apps-condition).




## <a name="common-scenarios"></a>Typowe scenariusze

### <a name="requiring-multi-factor-authentication-for-apps"></a>Wymagania uwierzytelniania wieloskładnikowego dla aplikacji

Wiele środowisk ma aplikacje wymagające wyższego poziomu ochrony od innych.
Jest to, na przykład w przypadku aplikacji, które mają dostęp do poufnych danych.
Jeśli chcesz dodać kolejną warstwę ochrony do tych aplikacji, można skonfigurować zasady dostępu warunkowego, która wymaga usługi Multi-Factor authentication, gdy użytkownicy uzyskują dostęp do tych aplikacji.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Wymagania uwierzytelniania wieloskładnikowego dla dostępu z sieci, które nie są zaufane

Ten scenariusz jest podobny do poprzedniego scenariusza, ponieważ powoduje ona dodanie wymaganie uwierzytelniania wieloskładnikowego.
Główną różnicą jest jednak warunek tego wymagania.  
Podczas na aplikacje z dostępem do danych sensitve fokus poprzedniego scenariusza, w tym scenariuszu koncentruje się na zaufanych lokalizacji.  
Innymi słowy może być wymagane uwierzytelnianie wieloskładnikowe, jeśli aplikacja jest dostępna przez użytkownika z sieci, której nie ufasz.


### <a name="only-trusted-devices-can-access-office-365-services"></a>Tylko zaufane urządzenia mają dostęp do usług Office 365

Jeśli używasz usługi Intune w danym środowisku, mogą natychmiast rozpocząć korzystanie interfejs zasad dostępu warunkowego w konsoli platformy Azure.

Wielu klientów usługi Intune są przy użyciu dostępu warunkowego, aby upewnić się, że tylko zaufane urządzenia mają dostęp do usług Office 365. Oznacza to, że urządzenia przenośne zarejestrowane w usłudze Intune i spełnić wymagania zasad zgodności i że komputery z systemem Windows są przyłączone do domeny lokalnej. Poprawy klucza jest, że nie trzeba ustawić te same zasady dla każdej usługi Office 365.  Podczas tworzenia nowych zasad konfigurowania aplikacji w chmurze uwzględnienie wszystkich aplikacji usługi Office 365, które chcesz chronić za pomocą dostępu warunkowego.

## <a name="next-steps"></a>Następne kroki

- Jeśli chcesz wiedzieć, jak skonfigurować zasady dostępu warunkowego, zobacz [wprowadzenie dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Jeśli wszystko jest gotowe do skonfigurowania zasad dostępu warunkowego dla danego środowiska, zobacz [najlepszych rozwiązań dotyczących dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-best-practices.md). 
