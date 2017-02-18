---
title: "Raporty o zabezpieczeniach w usłudze Azure Active Directory w wersji Bezpłatna i Podstawowa — podgląd | Microsoft Docs"
description: "Lista raportów dostępnych w ramach podglądu usługi Azure Active Directory"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/19/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: fa5a6dab1e76d62956cbfdd9b8b0f64c014696db
ms.openlocfilehash: c1a3953c79cfac9d6f55b38971ee2b79ff4244eb


---
# <a name="security-reporting-in-the-azure-active-directory-free-and-basic-edition---preview"></a>Raporty o zabezpieczeniach w usłudze Azure Active Directory w wersji Bezpłatna i Podstawowa — podgląd

Dzięki raportom o zabezpieczeniach w ramach [podglądu](active-directory-preview-explainer.md) usługi Azure Active Directory możesz uzyskać wgląd w prawdopodobieństwo naruszenia bezpieczeństwa kont użytkowników w środowisku. 

Usługa Azure Active Directory wykrywa podejrzane akcje powiązane z kontami użytkowników. Dla każdej wykrytej akcji jest tworzony wpis nazywany *zdarzeniem o podwyższonym ryzyku*. Aby uzyskać więcej informacji, zobacz [Zdarzenia o podwyższonym ryzyku w usłudze Azure Active Directory](active-directory-identity-protection-risk-events.md). 

Za pomocą wykrytych zdarzeń o podwyższonym ryzyku obliczane są:

- **Ryzykowne logowania** — ryzykowne logowanie jest wskaźnikiem próby logowania, które mogło zostać wykonane przez osobę, która nie jest prawowitym właścicielem konta użytkownika. Aby uzyskać więcej informacji, zobacz [Ryzykowne logowania](active-directory-identityprotection.md#risky-sign-ins). 

- **Użytkownicy oflagowani w związku z ryzykiem** — ryzykowny użytkownik jest wskaźnikiem konta użytkownika, którego bezpieczeństwo mogło zostać naruszone. Aby uzyskać więcej informacji, zobacz [Użytkownicy oflagowani w związku z ryzykiem](active-directory-identityprotection.md#users-flagged-for-risk).  


## <a name="risky-sign-ins-report"></a>Raport dotyczący ryzykownych logowań

Usługa Azure Active Directory w wersji Bezpłatna i Podstawowa zapewnia listę wykrytych ryzykownych logowań dla użytkowników. Raport zdarzeń o podwyższonym ryzyku zawiera następujące informacje:

- **Użytkownik** — nazwa użytkownika użyta podczas logowania
- **IP** — adres IP urządzenia, którego użyto do nawiązania połączenia z usługą Azure Active Directory
- **Lokalizacja** — lokalizacja, z której nawiązano połączenie z usługą Azure Active Directory
- **Godzina logowania** — godzina, o której przeprowadzono logowanie
- **Stan** — stan logowania

Dane raportu można pobrać.

![Raportowanie](./media/active-directory-reporting-security-azure-portal-free-basic/01.png)

Na podstawie badania ryzykownego logowania możesz przekazać usłudze Azure Active Directory swoją reakcję w postaci następujących akcji:

- Rozwiąż
- Oznacz jako wynik fałszywie dodatni
- Zignoruj
- Uaktywnij ponownie

![Raportowanie](./media/active-directory-reporting-security-azure-portal-free-basic/21.png)

Aby uzyskać więcej informacji, zobacz [Ręczne zamykanie zdarzeń o podwyższonym ryzyku](active-directory-identityprotection.md#closing-risk-events-manually).


## <a name="users-at-risk-report"></a>Raport dotyczący narażonych użytkowników

Usługa Azure Active Directory w wersji Bezpłatna zapewnia listę kont użytkowników, których bezpieczeństwo mogło zostać naruszone. 


![Raportowanie](./media/active-directory-reporting-security-azure-portal-free-basic/03.png)

Kliknięcie użytkownika na liście spowoduje otwarcie bloku z danymi tego użytkownika.
W przypadku narażonego użytkownika przejrzyj jego historię logowania i w razie potrzeby zresetuj hasło.

![Raportowanie](./media/active-directory-reporting-security-azure-portal-free-basic/46.png)



## <a name="next-steps"></a>Następne kroki

- Aby uzyskać więcej szczegółowych informacji na temat raportów usługi Azure Active Directory, zobacz [Przewodnik po raportach usługi Azure Active Directory](active-directory-reporting-guide.md).
- Aby uzyskać więcej informacji na temat ochrony tożsamości w usłudze Azure Active Directory, zobacz [Ochrona tożsamości w usłudze Azure Active Directory](active-directory-identityprotection.md).




<!--HONumber=Jan17_HO3-->


