---
title: "Zarządzanie dostępem użytkowników za pomocą przeglądów dostępu w usłudze Azure AD | Microsoft Docs"
description: "Dowiedz się, jak za pomocą przeglądów dostępu w usłudze Azure Active Directory zarządzać dostępem użytkowników — członkostwem w grupie lub przypisaniem do aplikacji."
services: active-directory
documentationcenter: 
author: markwahl-msft
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: 0459eb5cc71939202c8491f6b2714e28bd8e202d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-user-access-with-azure-ad-access-reviews"></a>Zarządzanie dostępem użytkowników za pomocą przeglądów dostępu w usłudze Azure AD

Dzięki usłudze Azure Active Directory można łatwo zapewnić użytkownikom odpowiedni dostęp, prosząc ich — lub osobę podejmującą decyzje — o uczestnictwo w przeglądzie dostępu i ponownie certyfikując („poświadczając”) ich dostęp.  Na podstawie sugestii pochodzących z usługi Azure AD recenzenci mogą wyrazić opinię dotyczącą przedłużenia dostępu poszczególnych użytkowników. Po zakończeniu przeglądu dostępu można wprowadzić zmiany i odebrać dostęp użytkownikom, którzy już go nie potrzebują.

> [!NOTE]
> Jeśli zamiast wszystkich typów użytkowników chcesz przejrzeć tylko dostęp gości, zobacz [Managing guest user access with access reviews (Zarządzanie dostępem gości za pomocą przeglądów dostępu)](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md).  Z kolei jeśli chcesz przejrzeć członkostwo użytkowników w rolach administracyjnych, takich jak administrator globalny, zobacz [How to start an access review in Azure AD PIM (Jak rozpocząć przegląd dostępu w usłudze Azure AD PIM)](active-directory-privileged-identity-management-how-to-start-security-review.md). 
>
>

## <a name="prerequisites"></a>Wymagania wstępne 

Przeglądy dostępu są dostępne w wersji Premium P2 usługi Azure Active Directory, zawartej w pakiecie EMS E5. Aby uzyskać więcej informacji, zobacz [Wersje usługi Azure Active Directory](active-directory-editions.md).  Użytkownicy, który chcą korzystać z tej funkcji w celu tworzenia lub stosowania przeglądu albo przeprowadzania przeglądu dostępu, muszą mieć licencję.


## <a name="creating-and-performing-an-access-review"></a>Tworzenie i przeprowadzanie przeglądu dostępu

W przeglądzie dostępu może uczestniczyć jeden lub większa liczba recenzentów.  

1. Wybierz grupę usługi Azure Active Directory zawierającą co najmniej jednego członka lub aplikację połączoną z usługą Azure Active Directory, do której przypisano co najmniej jednego użytkownika. 
2. Zdecyduj, czy poszczególni użytkownicy mają dokonać przeglądu własnego dostępu, czy wybrana grupa użytkowników na przeprowadzić przegląd dostępu wszystkich użytkowników.
3. Włącz wyświetlanie przeglądów dostępu na panelach dostępu recenzentów.  Korzystając z uprawnień administratora globalnego, przejdź do [strony przeglądów dostępu](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/). 
4. Rozpocznij przegląd dostępu. Aby dowiedzieć się więcej, zobacz [How to create an access review (Jak utworzyć przegląd dostępu)](active-directory-azure-ad-controls-create-access-review.md).
5. Poproś recenzentów o wyrażenie opinii. Domyślnie otrzymają oni wiadomość e-mail z usługi Azure AD z linkiem do panelu dostępu, który umożliwia [przeprowadzenie przeglądu dostępu](active-directory-azure-ad-controls-perform-access-review.md).
6. Jeśli recenzenci nie przekażą opinii, można wysłać im przypomnienia za pośrednictwem usługi Azure AD.  Domyślnie usługa Azure AD automatycznie wysyła przypomnienia recenzentom, którzy jeszcze nie odpowiedzieli, po upływie połowy czasu przeznaczonego na przekazanie opinii.
7. Po otrzymaniu opinii recenzentów zakończ przegląd dostępu i zastosuj zmiany. Aby dowiedzieć się więcej, zobacz [How to complete an access review (Jak zakończyć przegląd dostępu)](active-directory-azure-ad-controls-complete-access-review.md).


## <a name="next-steps"></a>Następne kroki

- [Create an access review for members of a group or access to an application (Tworzenie przeglądu dostępu dla członków grupy lub na potrzeby dostępu do aplikacji)](active-directory-azure-ad-controls-create-access-review.md)




