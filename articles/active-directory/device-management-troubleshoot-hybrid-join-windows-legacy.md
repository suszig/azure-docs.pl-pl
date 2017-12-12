---
title: "Rozwiązywanie problemów z hybrydowe usługi Azure Active Directory niższego poziomu urządzeniach przyłączonych do | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów z hybrydowe usługi Azure Active Directory połączone urządzenia niższego poziomu."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: f1b92c604e20198714e9697bf4d08b3f71f23ae3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
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

2. Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

To polecenie wyświetla okno dialogowe, która udostępnia szczegółowe informacje o stanie sprzężenia.

![Dołącz do miejsca pracy dla systemu Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>Krok 2: Ocena hybrydowe usługi Azure AD join stanu 

Jeśli sprzężenie hybrydowe usługi Azure AD nie powiodło się, okno dialogowe zawiera szczegółowe informacje o problemie, który wystąpił.

**Najbardziej typowe problemy są:**

- Nieprawidłowej konfiguracji usług AD FS lub Azure AD

    ![Dołącz do miejsca pracy dla systemu Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- Użytkownik nie jest zarejestrowany jako użytkownik domeny

    ![Dołącz do miejsca pracy dla systemu Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

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

## <a name="next-steps"></a>Następne kroki

Odpowiedzi na pytania, zobacz [zarządzanie urządzeniami — często zadawane pytania](device-management-faq.md)  
