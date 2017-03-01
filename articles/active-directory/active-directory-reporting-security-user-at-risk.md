---
title: "Raport o zabezpieczeniach dotyczący narażonych użytkowników w portalu Azure Active Directory — wersja zapoznawcza | Microsoft Docs"
description: "Dowiedz się więcej o raporcie o zabezpieczeniach dotyczącym narażonych użytkowników w portalu Azure Active Directory — wersja zapoznawcza."
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/21/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 349109e0c12a1394f96529a94ab884eeb451d242
ms.openlocfilehash: 48c504a9ed5bc4ef9f0bff889df031962c5bf6e8
ms.lasthandoff: 02/22/2017


---
# <a name="users-at-risk-security-report-in-the-azure-active-directory-portal---preview"></a>Raport o zabezpieczeniach dotyczący narażonych użytkowników w portalu Azure Active Directory — wersja zapoznawcza

Dzięki raportom o zabezpieczeniach w ramach [podglądu](active-directory-preview-explainer.md) usługi Azure Active Directory możesz uzyskać wgląd w prawdopodobieństwo naruszenia bezpieczeństwa kont użytkowników w środowisku. 

Usługa Azure Active Directory wykrywa podejrzane akcje powiązane z kontami użytkowników. Dla każdej wykrytej akcji jest tworzony wpis nazywany *zdarzeniem o podwyższonym ryzyku*. Aby uzyskać więcej informacji, zobacz [Zdarzenia o podwyższonym ryzyku w usłudze Azure Active Directory](active-directory-identity-protection-risk-events.md). 

Za pomocą wykrytych zdarzeń o podwyższonym ryzyku obliczane są:

- **Ryzykowne logowania** — ryzykowne logowanie jest wskaźnikiem próby logowania, które mogło zostać wykonane przez osobę, która nie jest prawowitym właścicielem konta użytkownika. Aby uzyskać więcej informacji, zobacz [Ryzykowne logowania](active-directory-identityprotection.md#risky-sign-ins). 

- **Użytkownicy oflagowani w związku z ryzykiem** — ryzykowny użytkownik jest wskaźnikiem konta użytkownika, którego bezpieczeństwo mogło zostać naruszone. Aby uzyskać więcej informacji, zobacz [Użytkownicy oflagowani w związku z ryzykiem](active-directory-identityprotection.md#users-flagged-for-risk).  

W witrynie Azure Portal raporty dotyczące zabezpieczeń można znaleźć w bloku **Azure Active Directory** w sekcji **Zabezpieczenia**.  

![Ryzykowne logowania](./media/active-directory-reporting-security-user-at-risk/10.png)

## <a name="azure-active-directory-free-and-basic-edition"></a>Azure Active Directory — wersja Bezpłatna i Podstawowa

Raport dotyczący narażonych użytkowników w usłudze Azure Active Directory w wersji Bezpłatna i Podstawowa zapewnia listę kont użytkowników, których bezpieczeństwo mogło zostać naruszone. 


![Ryzykowne logowania](./media/active-directory-reporting-security-user-at-risk/03.png)

Wybranie użytkownika powoduje otwarcie bloku z danymi tego użytkownika.
W przypadku narażonego użytkownika można przejrzeć jego historię logowania i w razie potrzeby zresetować hasło.

![Ryzykowne logowania](./media/active-directory-reporting-security-user-at-risk/46.png)

## <a name="azure-active-directory-premium-editions"></a>Azure Active Directory — wersje Premium

Raport dotyczący narażonych użytkowników w usłudze Azure Active Directory w wersjach Premium zawiera następujące elementy:

- [Lista kont użytkowników](active-directory-identityprotection.md#users-flagged-for-risk), których bezpieczeństwo mogło zostać naruszone 

- Zagregowane informacje o wykrytych [typach zdarzeń o podwyższonym ryzyku](active-directory-identity-protection-risk-events.md)

- Opcja pobrania raportu

- Opcja skonfigurowania [zasad podejmowania działań naprawczych dotyczących ryzyka związanego z użytkownikiem](active-directory-identityprotection.md#user-risk-security-policy)  


![Ryzykowne logowania](./media/active-directory-reporting-security-user-at-risk/71.png)

Po wybraniu użytkownika jest dla niego wyświetlany szczegółowy widok raportu, który umożliwia wykonanie następujących czynności:

- Otwieranie widoku wszystkich logowań.

- Resetowanie hasła użytkownika.

- Odrzucanie wszystkich zdarzeń.

- Badanie zgłoszonych zdarzeń o podwyższonym ryzyku dla użytkownika. 


![Ryzykowne logowania](./media/active-directory-reporting-security-user-at-risk/324.png)


Aby zbadać zdarzenie o podwyższonym ryzyku, wybierz je z listy.  
Spowoduje to otwarcie bloku **Szczegóły** dla tego zdarzenia o podwyższonym ryzyku. W bloku **Szczegóły** jest opcja [ręcznego zamknięcia zdarzenia o podwyższonym ryzyku](active-directory-identityprotection.md#closing-risk-events-manually) lub ponownego aktywowania ręcznie zamkniętego zdarzenia o podwyższonym ryzyku. 


![Ryzykowne logowania](./media/active-directory-reporting-security-user-at-risk/325.png)



## <a name="next-steps"></a>Następne kroki

- Aby uzyskać więcej informacji na temat ochrony tożsamości w usłudze Azure Active Directory, zobacz [Ochrona tożsamości w usłudze Azure Active Directory](active-directory-identityprotection.md).


