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
ms.date: 09/16/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 119189415e75134ff0c77a551536559b81116fc6
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
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

Jeśli masz zasady w klasycznym portalu Azure skonfigurowane migracji ich do portalu Azure ponieważ:


- Użytkownik, który znajduje się w zasadach Azure portalu klasycznego i zasad portalu Azure musi spełniać wymagania w przypadku obu zasad 

- Nie w przypadku migrowania istniejących zasad, nie będziesz w stanie wdrożenia zasad, które są udzielanie dostępu


### <a name="migration-from-the-azure-classic-portal"></a>Migracja z klasycznego portalu Azure

W tym scenariuszu: 

- W Twojej [klasycznego portalu Azure](https://manage.windowsazure.com), zostały skonfigurowane:

    - SharePoint Online

    ![Dostęp warunkowy](./media/active-directory-conditional-access-best-practices/14.png)

    - Zasady dostępu warunkowego opartego na urządzeniu

    ![Dostęp warunkowy](./media/active-directory-conditional-access-best-practices/15.png)

- Aby skonfigurować zasady dostępu warunkowego do zarządzania aplikacjami mobilnymi w portalu Azure 
 

#### <a name="configuration"></a>Konfiguracja 

- Przejrzyj zasady dostępu warunkowego opartego na urządzeniu

- Migrowanie ich do portalu Azure 

- Dodawanie zasad dostępu warunkowego do zarządzania aplikacjami mobilnymi


### <a name="migrating-from-intune"></a>Migrowanie z usługi Intune 

W tym scenariuszu:

- W [Intune](https://portal.azure.com/#blade/Microsoft_Intune/SummaryBlade ), możesz mieć zasady dostępu warunkowego do zarządzania aplikacjami mobilnymi dla każdej usługi Exchange Online lub skonfigurować usługi SharePoint Online

    ![Dostęp warunkowy](./media/active-directory-conditional-access-best-practices/15.png)

- Użytkownik chce migrować przy użyciu dostępu warunkowego zarządzania aplikacjami mobilnymi w portalu Azure


#### <a name="configuration"></a>Konfiguracja 
 
- Przejrzyj zasady dostępu warunkowego opartego na urządzeniu

- Migrowanie ich do portalu Azure 

- Przejrzyj zostanie skonfigurowany dla usługi Exchange Online lub SharePoint Online w usłudze Intune zasady dostępu warunkowego do zarządzania aplikacjami mobilnymi

- Dodawanie formantu do **wymagają zatwierdzonych aplikacji** oprócz sterowania opartego na urządzeniach 
 

### <a name="migrating-from-the-azure-classic-portal-and-intune"></a>Migracja z klasycznego portalu Azure i usługi Intune

W tym scenariuszu:

- Masz skonfigurowano następujące elementy:

    - **Klasycznego portalu Azure:** warunkowego opartego na urządzeniach 

    - **Usługa Intune:** zasad dostępu warunkowego do zarządzania aplikacjami mobilnymi 
    
- Użytkownik chce migrować obie zasady przy użyciu zasad dostępu warunkowego do zarządzania aplikacjami mobilnymi w portalu Azure


#### <a name="configuration"></a>Konfiguracja

- Przejrzyj zasady dostępu warunkowego opartego na urządzeniu

- Migrowanie ich do portalu Azure 

- Przejrzyj zostanie skonfigurowany dla usługi Exchange Online lub SharePoint Online w usłudze Intune zasady dostępu warunkowego zarządzania aplikacjami mobilnymi

- Dodawanie formantu do **wymagają zatwierdzonych aplikacji** oprócz oparta na urządzeniach 




## <a name="next-steps"></a>Następne kroki

Jeśli chcesz wiedzieć, jak skonfigurować zasady dostępu warunkowego, zobacz [wprowadzenie dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).
