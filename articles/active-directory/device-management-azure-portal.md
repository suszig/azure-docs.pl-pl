---
title: "Zarządzanie urządzeniami przy użyciu platformy Azure portal - preview | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać portalu Azure do zarządzania urządzeniami."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 4b46e1627a229b0649d9ccd2550cd28fda9849f8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="managing-devices-using-the-azure-portal---preview"></a>Zarządzanie urządzeniami przy użyciu platformy Azure portal - preview

>[!NOTE]
>Ta funkcja jest obecnie w wersji zapoznawczej. Przygotuj się do przywrócenia lub Usuń wszystkie zmiany. Funkcja jest dostępna w żadnych subskrypcji usługi Azure Active Directory (Azure AD) w publicznej wersji zapoznawczej. Gdy funkcja stanie się ogólnie dostępna, niektórych aspektów funkcji mogą jednak wymagać subskrypcję usługi Azure Active Directory premium.


Z zarządzania urządzeniami w usłudze Azure Active Directory (Azure AD) można zapewnić, że użytkownicy uzyskują dostęp do zasobów z urządzeń, które spełniają standardy zabezpieczeń i zgodności. 

W tym temacie:

- Przyjęto założenie, że czytelnik zna [wprowadzenie do zarządzania urządzeniami w usłudze Azure Active Directory](device-management-introduction.md)

- Dostarcza informacji na temat zarządzania urządzeniami przy użyciu portalu Azure


Aby zarządzać urządzeniami w portalu Azure, musisz kliknąć przycisk **urządzeń** w **Zarządzaj** sekcji **usługi Azure Active Directory** bloku.

![Zarządzanie urządzeń w usłudze Intune](./media/device-management-azure-portal/11.png)




## <a name="configure-device-settings"></a>Konfiguruj ustawienia urządzeń

Do zarządzania urządzeniami przy użyciu portalu Azure, muszą być zarejestrowane lub dołączone do usługi Azure AD. Jako administrator Aby precyzyjnie zdefiniować proces rejestracji i dołączenie urządzeń przez skonfigurowanie ustawień urządzenia.

![Zarządzanie urządzeń w usłudze Intune](./media/device-management-azure-portal/22.png)


Blok ustawień urządzenia można skonfigurować:

- **Użytkownicy mogą dołączać urządzenia do usługi Azure AD** — tego ustawienia umożliwia Wybierz użytkowników, którzy mogą dołączać urządzenia do usługi Azure AD. Wartość domyślna to **wszystkich**.

- **Urządzeniach przyłączonych do kolejnych administratorów lokalnych na usługi Azure AD** — można wybrać użytkowników, którzy mają prawa administratora lokalnego na urządzeniu. Dodane w tym miejscu użytkownicy są dodawani do *Administratorzy urządzenia* roli w usłudze Azure AD. Administratorzy globalni w usłudze Azure AD i właścicielom urządzeń mają prawa administratora lokalnego domyślnie. Ta opcja jest dostępna za pośrednictwem produktów, takich jak Azure AD Premium lub Enterprise Mobility Suite (EMS) możliwości wersji premium. 

- **Użytkownicy mogą zarejestrować swoje urządzenia w usłudze Azure AD** — należy skonfigurować to ustawienie, aby zezwolić urządzeniom, które mają być zarejestrowane w usłudze Azure AD. W przypadku wybrania **Brak**, urządzenia są niedozwolone do rejestru, jeśli nie są one usługi Azure AD przyłączone lub hybrydowego przyłączonych do usługi Azure AD. Rejestrowanie w usłudze Microsoft Intune lub zarządzania urządzeniami przenośnymi (MDM) dla usługi Office 365 wymaga rejestracji. Jeśli skonfigurowano którąś z tych usług **wszystkie** jest zaznaczone i **Brak** nie jest dostępna.

- **Wymagaj uwierzytelniania wieloskładnikowego urządzeń** — możesz wybrać, czy użytkownicy muszą zapewnić drugi składnik uwierzytelniania w celu przyłączenia urządzenia do usługi Azure AD. Wartość domyślna to **nr**. Firma Microsoft zaleca, wymagając uwierzytelniania wieloskładnikowego podczas rejestrowania urządzenia. Przed włączeniem uwierzytelniania wieloskładnikowego dla tej usługi, musisz zapewnić, że skonfigurowano uwierzytelnianie wieloskładnikowe dla użytkowników, które rejestrują swoje urządzenia. Aby uzyskać więcej informacji dotyczących usług różnych uwierzytelnianie wieloskładnikowe platformy Azure, zobacz [wprowadzenie do korzystania z usługi Azure Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md). 

- **Maksymalna liczba urządzeń** — to ustawienie umożliwia wybranie maksymalną liczbę urządzeń, które użytkownik może mieć w usłudze Azure AD. Jeśli użytkownik osiągnie ten limit przydziału, nie są będzie można dodać dodatkowych urządzeń dopóki jedna lub więcej istniejące urządzenia zostaną usunięte. Oferta urządzenia jest liczony dla wszystkich urządzeń, które są usługi Azure AD przyłączone lub usługi Azure AD, w obecnie zarejestrowany. Wartość domyślna to **20**.

- **Użytkownicy mogą synchronizować ustawienia i dane aplikacji na urządzeniach** — domyślnie to ustawienie ma wartość **Brak**. Wybranie określonych użytkowników lub grup lub wszystkich umożliwia użytkownika ustawień i danych aplikacji można zsynchronizować na ich urządzeniach z systemem Windows 10. Dowiedz się więcej informacji na temat sposobu synchronizacji działa w systemie Windows 10.
Ta opcja jest dostępna za pośrednictwem produktów, takich jak Azure AD Premium lub Enterprise Mobility Suite (EMS) możliwości premium.
 
    ![Zarządzanie urządzeń w usłudze Intune](./media/device-management-azure-portal/21.png)




## <a name="locate-devices"></a>Znajdź urządzenia

Jako administrator w portalu Azure, masz dwie opcje można znaleźć urządzenia zarejestrowane i są przyłączone do:

- **Wszystkie urządzenia** w **Zarządzaj** sekcji **urządzeń** bloku  

    ![Wszystkie urządzenia](./media/device-management-azure-portal/41.png)


- **Urządzenia** w **Zarządzaj** sekcji **użytkownika** bloku
 
    ![Wszystkie urządzenia](./media/device-management-azure-portal/43.png)



Z obu opcji, aby przejść do widoku który:


- Umożliwia wyszukiwanie urządzeń przy użyciu nazwy wyświetlanej jako filtr.

- Udostępnia szczegółowy przegląd urządzeń zarejestrowanych i połączone

- Służy do wykonywania typowych zadań zarządzania dla urządzenia
   

![Wszystkie urządzenia](./media/device-management-azure-portal/51.png)


## <a name="device-management-tasks"></a>Zadania dotyczące zarządzania urządzeniami

Jako administrator można zarządzać urządzeniami zarejestrowane lub połączone. Ta sekcja zawiera informacje o typowych zadań zarządzania dla urządzenia.


**Zarządzanie urządzeniem Intune** — Jeśli jesteś administratorem usługi Intune, możesz zarządzać urządzeniami oznaczona jako **Microsoft Intune**. Administrator może zobaczyć dodatkowe urządzenia 

![Zarządzanie urządzeń w usłudze Intune](./media/device-management-azure-portal/31.png)


**Włączanie / wyłączanie urządzeń usługi Azure AD**

Aby włączyć lub wyłączyć urządzenie, musisz być administratorem globalnym w usłudze Azure AD. Wyłączenie urządzenia zapobiega urządzenia podczas uzyskiwania dostępu do zasobów usługi Azure AD.  Aby wyłączyć urządzenie, możesz kliknąć przycisk *...* Kliknij urządzenie, aby uzyskać dodatkowe szczegóły.

 
![Zarządzanie urządzeń w usłudze Intune](./media/device-management-azure-portal/33.png)

Wyłączenie urządzenia zmienia stan na **włączone** kolumny **nr**.

![Wyłącz urządzenia](./media/device-management-azure-portal/32.png)


**Usuwanie urządzenia z systemem Azure AD** — Aby usunąć urządzenie, musisz być administratorem globalnym w usłudze Azure AD.  
Usuwanie urządzenia:
 
- Zapobiega urządzenia podczas uzyskiwania dostępu do zasobów usługi Azure AD 

- Usuwa wszystkie szczegóły, które są dołączone do urządzenia, na przykład, klucze funkcji BitLocker dla urządzeń z systemem Windows  

- Reprezentuje nieodwracalny działania i nie jest zalecane, chyba że jest to wymagane

Jeśli urządzenie jest zarządzane przez inny urząd zarządzania (np. Microsoft Intune), upewnij się, że urządzenie zostało wyczyszczone / wycofane przed usunięciem urządzenia w usłudze Azure AD.

Możesz wybrać "..." Aby usunąć urządzenie lub kliknij przycisk na urządzeniu, aby uzyskać dodatkowe szczegóły
 
![Usuwanie urządzenia](./media/device-management-azure-portal/34.png)


**Wyświetl lub skopiuj identyfikator urządzenia** — identyfikator urządzenia umożliwia Sprawdź szczegóły identyfikator urządzenia na urządzeniu lub podczas rozwiązywania problemów przy użyciu programu PowerShell. Aby uzyskać dostęp do opcji kopię, kliknij urządzenie.

![Wyświetl identyfikator urządzenia](./media/device-management-azure-portal/35.png)
  

**Wyświetl lub skopiować klucze funkcji BitLocker** — Jeśli jesteś administratorem, można wyświetlać i kopiowanie kluczy funkcji BitLocker, aby ułatwić użytkownikom na odzyskiwanie ich zaszyfrowanego dysku. Klucze te są dostępne tylko dla urządzeń z systemem Windows, które są szyfrowane i ich kluczy przechowywanych w usłudze Azure AD. Możesz skopiować te klucze podczas uzyskiwania dostępu do szczegółów urządzenia.
 
![Wyświetl klucze funkcji BitLocker](./media/device-management-azure-portal/36.png)



## <a name="audit-logs"></a>Dzienniki inspekcji


Działania urządzenia są dostępne Dzienniki aktywności. Obejmuje to wyzwalane przez usługę rejestracji urządzeń lub przez użytkownika działań:

- Tworzenie urządzenia i dodawanie użytkowników/właścicieli na urządzeniu

- Zmiany w ustawieniach urządzenia

- Urządzenie operacji, takich jak usuwanie lub aktualizowanie urządzenia
 
Punkt wejścia do inspekcji danych jest **dzienniki inspekcji** w **działania** sekcji **urządzeń* bloku.

![Dzienniki inspekcji](./media/device-management-azure-portal/61.png)


Dziennik inspekcji zawiera domyślny widok listy, który pokazuje:

- datę i godzinę wystąpienia,

- obiekty docelowe

- Inicjator / aktora (który) działania

- działanie (co)

![Dzienniki inspekcji](./media/device-management-azure-portal/63.png)

Możesz dostosować widok listy, klikając pozycję **Kolumny** na pasku narzędzi.
 
![Dzienniki inspekcji](./media/device-management-azure-portal/64.png)


Aby zawęzić zgłaszane dane do odpowiedniego poziomu, możesz odfiltrować dane inspekcji przy użyciu następujących pól:

- Catergory
- Typ zasobu działania
- Działanie
- Zakres dat
- docelowy
- Zainicjowane przez (aktora)

Oprócz filtrów możesz wyszukać wybrane wpisy.

![Dzienniki inspekcji](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Następne kroki

* [Wprowadzenie do zarządzania urządzeniami w usłudze Azure Active Directory](device-management-introduction.md)



