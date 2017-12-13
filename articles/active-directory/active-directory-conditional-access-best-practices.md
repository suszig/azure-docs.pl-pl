---
title: "Najlepsze rozwiązania dla dostępu warunkowego w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Co to jest się, że należy unikać zrobić podczas konfigurowania zasad dostępu warunkowego i Dowiedz się więcej o rzeczy, o których należy wiedzieć."
services: active-directory
keywords: "dostęp warunkowy do aplikacji, dostęp warunkowy przy użyciu usługi Azure AD, bezpieczny dostęp do zasobów firmy, zasady dostępu warunkowego"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/12/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 8c6707505a6331b53e06b1de60575dd3637ea477
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Najlepsze rozwiązania dotyczące dostępu warunkowego w usłudze Azure Active Directory

Ten temat zawiera informacje o rzeczy, o których należy wiedzieć, i jakie są się, że należy unikać zrobić podczas konfigurowania zasad dostępu warunkowego. Przed przeczytaniem tego tematu, należy zapoznać się z pojęciami i terminologii określone w [dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal.md)

## <a name="what-you-should-know"></a>Co należy wiedzieć

### <a name="whats-required-to-make-a-policy-work"></a>Co to są wymagane do zmiany celu zasad pracy?

Podczas tworzenia nowych zasad nie ma żadnych użytkowników, grup, aplikacji ani wybrane kontroli dostępu.

![Aplikacje w chmurze](./media/active-directory-conditional-access-best-practices/02.png)


Aby działać zgodnie z zasadami, należy skonfigurować następujące ustawienia:


|Co           | Jak                                  | Dlaczego|
|:--            | :--                                  | :-- |
|**Aplikacje w chmurze** |Musisz wybrać co najmniej jedną aplikację.  | Celem zasad dostępu warunkowego jest umożliwiają dostrojenie jak autoryzowani użytkownicy mają dostęp do Twojej aplikacji.|
| **Użytkownicy i grupy** | Musisz wybrać co najmniej jednego użytkownika lub grupę, który jest upoważniony do dostępu do aplikacji w chmurze, który wybrano. | Zasady dostępu warunkowego, który nie ma użytkowników i grupy przypisane, nigdy nie zostanie wywołany. |
| **Kontrola dostępu** | Musisz wybrać kontroli dostępu co najmniej jeden. | Procesor zasady musi wiedzieć, co należy zrobić, jeśli warunki są spełnione.|


Oprócz podstawowych wymagań w wielu przypadkach należy także skonfigurować warunek. Gdy zasady również będzie działać bez skonfigurowanego warunku, warunki są pobudzenie współczynnik dostrajanie dostępu do aplikacji.


![Aplikacje w chmurze](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a>Jak są analizowane przypisania

Wszystkie przypisania są logicznie **and**. Jeśli masz więcej niż jednego przypisania skonfigurowany, aby wyzwolić zasady, wszystkie przypisania muszą być spełnione.  

Jeśli potrzebujesz Konfigurowanie warunku lokalizacji, która ma zastosowanie do wszystkich połączeń z poza siecią organizacji, można wykonać to przez:

- W tym **wszystkich lokalizacji**
- Z wyjątkiem **wszystkich zaufanych adresów IP**

### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Co się stanie, jeśli masz zasady w klasycznym portalu Azure i portalu Azure skonfigurowane?  
Obie zasady są wymuszane przez usługę Azure Active Directory, a użytkownik uzyskuje dostęp tylko wtedy, gdy są spełnione wszystkie wymagania.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>Co się stanie, jeśli masz zasady w portalu usługi Intune Silverlight i portalu Azure?
Obie zasady są wymuszane przez usługę Azure Active Directory, a użytkownik uzyskuje dostęp tylko wtedy, gdy są spełnione wszystkie wymagania.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Co się dzieje w przypadku wielu zasad dla tego samego użytkownika skonfigurowane?  
Dla każdego logowania Azure Active Directory ocenia wszystkie zasady i zapewnia, że przed udzielony dostęp do użytkownika spełniono wszystkie wymagania.


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Dostęp warunkowy działa z programem Exchange ActiveSync?

Tak, można użyć programu Exchange ActiveSync w zasadach dostępu warunkowego.


## <a name="what-you-should-avoid-doing"></a>Co należy zrobić

Dostęp warunkowy framework zapewnia elastyczność dużą konfiguracji. Jednak elastyczność oznacza także, należy dokładnie przejrzeć wszystkie zasady konfiguracji przed zwalniania go, aby uniknąć niepożądane wyniki. W tym kontekście, należy zwrócić szczególną uwagę na przydziały wpływające na zestawy, takie jak **wszyscy użytkownicy / grupy / aplikacji w chmurze**.

W środowisku należy unikać następujące konfiguracje:


**Dla wszystkich użytkowników wszystkich aplikacji w chmurze:**

- **Blokowanie dostępu** — ta konfiguracja zablokuje całej organizacji, który ostatecznie nie jest dobrym rozwiązaniem.

- **Wymagają zgodnego urządzenia** — dla użytkowników, która nie zarejestrowali swoich urządzeń, ale ta zasada blokuje dostęp tym do uzyskiwania dostępu do portalu usługi Intune. Jeśli jesteś administratorem bez zarejestrowanego urządzenia te zasady blokuje powrót do portalu Azure do zmiany zasad.

- **Wymagane było przyłączenie do domeny** — ten blok zasady dostępu ma również możliwość zablokowania dostępu dla wszystkich użytkowników w organizacji, jeśli nie masz jeszcze urządzenia przyłączone do domeny.


**Dla wszystkich użytkowników, wszystkie aplikacje w chmurze, wszystkie platformy urządzeń:**

- **Blokowanie dostępu** — ta konfiguracja zablokuje całej organizacji, który ostatecznie nie jest dobrym rozwiązaniem.



## <a name="policy-migration"></a>Migracja zasad

Należy rozważyć Migrowanie zasad, które nie zostały utworzone w portalu Azure ponieważ:

- Można teraz adresów scenariusze, które nie może obsłużyć przed.

- Pozwala zmniejszyć liczbę zasad, które trzeba zarządzać konsolidując je.   

- Można zarządzać z zasad dostępu warunkowego w jednej centralnej lokalizacji.

- Klasycznym portalu Azure zostaną wycofane.   


Aby uzyskać więcej informacji, zobacz [migracji klasycznego zasad w portalu Azure](active-directory-conditional-access-migration.md).


## <a name="next-steps"></a>Następne kroki

Jeśli chcesz wiedzieć, jak skonfigurować zasady dostępu warunkowego, zobacz [wprowadzenie dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).
