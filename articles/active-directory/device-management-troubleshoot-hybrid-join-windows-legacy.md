---
title: Rozwiązywanie problemów z hybrydowe usługi Azure Active Directory niższego poziomu urządzeniach przyłączonych do | Dokumentacja firmy Microsoft
description: Rozwiązywanie problemów z hybrydowe usługi Azure Active Directory połączone urządzenia niższego poziomu.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 0d21a8848222c4b09723e22d2d51ec43b2154553
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Rozwiązywanie problemów z hybrydowe usługi Azure Active Directory przyłączone do urządzeń niskiego poziomu 

Ten temat dotyczy tylko następujące urządzenia: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Dla systemu Windows 10 lub Windows Server 2016, zobacz [hybrydowego Rozwiązywanie problemów z usługą Azure Active Directory przyłączone do urządzeń z systemem Windows 10 i Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md).

W tym temacie założono, że [urządzeniach przyłączonych do skonfigurowanego hybrydowe usługi Azure Active Directory](device-management-hybrid-azuread-joined-devices-setup.md) do obsługi następujących scenariuszy:

- Dostępu warunkowego opartego na urządzeniu

- [Enterprise roaming ustawień](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello dla firm](active-directory-azureadjoin-passport-deployment.md) 





Ten temat zawiera rozwiązania wskazówki dotyczące rozwiązywania potencjalnych problemów.  

**Co należy wiedzieć:** 

- Maksymalna liczba urządzeń dla każdego użytkownika jest perspektywy skoncentrowanej na urządzeniach. Na przykład jeśli *jdoe* i *jharnett* logowania na urządzeniu, oddzielne rejestracji (DeviceID) jest tworzony dla każdego z nich w **użytkownika** kartę informacje.  

- Początkowe rejestracyjny / przyłączenia urządzeń jest skonfigurowany do wykonania próba logowania lub blokowanie / odblokowanie. Mogą wystąpić opóźnienia 5-minutowy wyzwalane przez zadania harmonogramu zadań. 

- Ponowna instalacja systemu operacyjnego lub ręcznego unregister i wykonaj ponowną rejestrację może utworzyć nowej rejestracji w usłudze Azure AD i powoduje wiele wpisów w karcie informacje użytkownika w portalu Azure. 

## <a name="step-1-retrieve-the-registration-status"></a>Krok 1: Pobierz stan rejestracji 

**Aby sprawdzić stan rejestracji:**  

1. Otwórz wiersz polecenia jako administrator 

2. Typ `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

To polecenie wyświetla okno dialogowe, która udostępnia szczegółowe informacje o stanie sprzężenia.

![Dołącz do miejsca pracy dla systemu Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>Krok 2: Ocena hybrydowe usługi Azure AD join stanu 

Jeśli sprzężenie hybrydowe usługi Azure AD nie powiodło się, okno dialogowe zawiera szczegółowe informacje o problemie, który wystąpił.

**Najbardziej typowe problemy są:**

- Nieprawidłowej konfiguracji usług AD FS lub Azure AD

    ![Dołącz do miejsca pracy dla systemu Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- Użytkownik nie jest zarejestrowany jako użytkownik domeny

    ![Dołącz do miejsca pracy dla systemu Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)
    
    Istnieje kilka przyczyn dlaczego taka sytuacja może wystąpić:
    
    1. Jeśli użytkownik zalogował się nie jest użytkownikiem domeny (na przykład użytkownik lokalny). Hybrydowe usługi Azure AD join na niższym poziomie urządzeń jest obsługiwane tylko dla użytkowników domeny.
    
    2. Jeśli z jakiegokolwiek powodu, Autoworkplace.exe nie dyskretnie uwierzytelniania za pomocą usługi Azure AD lub AD FS. Kilka możliwych przyczyn mogą być problemy z połączeniem sieciowym powiązania wyjściowego do usługi Azure AD adresów URL (Sprawdź wymagania wstępne) lub jeśli uwierzytelnianie wieloskładnikowe jest włączone/skonfigurowanego dla użytkownika, ale nie skonfigurowano WIAORMUTLIAUTHN na serwerze federacyjnym (kroki konfiguracji wyboru). Inną możliwością jest stronę odnajdowania (obszaru macierzystego HRD) tego obszaru macierzystego oczekuje do interakcji z użytkownikiem, uniemożliwia Autoworkplace.exe dyskretnie uzyskiwania tokenu.
    
    3. Jeśli organizacja korzysta z usługi Azure AD bezproblemowe logowanie jednokrotne, następujący adres URL nie jest dostępny w ustawienia sieci intranet IE urządzenia:
    
       - https://autologon.microsoftazuread-sso.com

    
       a ustawienie "Zezwalaj na aktualizacje na pasku stanu za pomocą skryptu" musi być włączona dla strefy intranetowej.

- Osiągnięto limit przydziału

    ![Dołącz do miejsca pracy dla systemu Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- Usługa nie odpowiada. 

    ![Dołącz do miejsca pracy dla systemu Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Można również znaleźć informacje o stanie w dzienniku zdarzeń w obszarze **aplikacji i usług Log\Microsoft-dołączania**.
  
**Najczęstszymi przyczynami sprzężenia nie powiodło się hybrydowej usługi Azure AD są:** 

- Komputer nie znajduje się w wewnętrznej sieci firmy lub sieci VPN bez połączenia z lokalnym kontrolerem domeny usługi AD.

- Użytkownik jest zalogowany na komputerze przy użyciu konta komputera lokalnego. 

- Problemy z konfiguracją usługi: 

  - Serwer federacyjny został skonfigurowany do obsługi **WIAORMULTIAUTHN**. 

  - Nie ma żadnego obiektu punktu połączenia usługi, który wskazuje nazwę domeny zweryfikowane w usłudze Azure AD w lesie usługi AD, w którym komputer należy do.

  - Użytkownik osiągnął limit urządzeń. 

## <a name="next-steps"></a>Kolejne kroki

Odpowiedzi na pytania, zobacz [zarządzanie urządzeniami — często zadawane pytania](device-management-faq.md)  
