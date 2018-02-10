---
title: "O nazwie lokalizacjach w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o nazwie co lokalizacje są i sposobach ich konfigurowania."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: b6f80cde24edcbec68309ba033d4da16ee97b731
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="named-locations-in-azure-active-directory"></a>Nazwane lokalizacje w usłudze Azure Active Directory

Nazwane lokalizacje można opisać zaufanych zakresów adresów IP w Twojej organizacji. Nazwane lokalizacje w kontekście korzysta z usługi Azure Active Directory:

- Wykrywanie [ryzyka zdarzenia](active-directory-reporting-risk-events.md) Aby zmniejszyć liczbę zgłoszonych fałszywych alarmów.  

- [Na podstawie lokalizacji dostępu warunkowego](active-directory-conditional-access-locations.md).


W tym artykule opisano, jak konfigurować o nazwie lokalizacji w danym środowisku.


## <a name="entry-points"></a>Punkty wejścia

Można dostęp do strony konfiguracji nazwanych lokalizacji w **zabezpieczeń** sekcji strony usługi Azure Active Directory, klikając pozycję:

![Punkty wejścia](./media/active-directory-named-locations/34.png)

- **Dostęp warunkowy:**

    - W **Zarządzaj** kliknij **o nazwie lokalizacje**.
    
        ![Polecenie lokalizacji o nazwie](./media/active-directory-named-locations/06.png)

- **Ryzykowne logowania:**

    - Na pasku narzędzi u góry kliknij **Dodaj znane zakresów adresów IP**.

       ![Polecenie lokalizacji o nazwie](./media/active-directory-named-locations/35.png)



## <a name="configuration-example"></a>Przykład konfiguracji

**Aby skonfigurować lokalizację o nazwie:**

1. Zaloguj się do [portalu Azure](https://portal.azure.com) jako administrator globalny.

2. W okienku po lewej stronie kliknij **usługi Azure Active Directory**.

    ![Łącze usługi Azure Active Directory w okienku po lewej stronie](./media/active-directory-named-locations/01.png)

3. Na **usługi Azure Active Directory** strony w **zabezpieczeń** kliknij **dostępu warunkowego**.

    ![Polecenie dostępu warunkowego](./media/active-directory-named-locations/05.png)


4. Na **dostępu warunkowego** strony w **Zarządzaj** kliknij **o nazwie lokalizacje**.

    ![Polecenie lokalizacji o nazwie](./media/active-directory-named-locations/06.png)


5. Na **o nazwie lokalizacje** kliknij przycisk **nową lokalizację**.

    ![Nowe polecenie lokalizacji](./media/active-directory-named-locations/07.png)


6. Na **nowy** strony, wykonaj następujące czynności:

    ![Nowy blok](./media/active-directory-named-locations/56.png)

    a. W **nazwa** wpisz nazwę dla nazwanego lokalizacji.

    b. W **zakresów IP** wpisz zakres adresów IP. Zakres adresów IP musi być w *Bezklasowego routingu międzydomenowego* formacie (CIDR).  

    c. Kliknij przycisk **Utwórz**.



## <a name="what-you-should-know"></a>Co należy wiedzieć

**Aktualizacje zbiorcze**: tworzenia lub aktualizowania nazwane lokalizacje aktualizacje zbiorcze, możesz przekazać lub pobrać plik CSV z zakresu adresów IP. Przekazanie dodaje zakresu adresów IP w pliku do listy zamiast zastępowanie na liście.

![Przekazywanie i pobieranie łącza](./media/active-directory-named-locations/09.png)


**Ograniczenia**: można określić maksymalnie 60 nazwane lokalizacje, z jeden zakres adresów IP przypisanych do każdego z nich. Jeśli masz tylko jedną lokalizację o nazwie skonfigurowane do 500 zakresów adresów IP można zdefiniować dla niego.


## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej o:

- **Ryzyka zdarzenia**, zobacz [zdarzenia o podwyższonym ryzyku usługi Azure Active Directory](active-directory-reporting-risk-events.md).

- **Dostęp warunkowy**, zobacz [dostępu warunkowego w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal.md).

- **Raporty ryzykowne logowania**, zobacz [raportu ryzykowne logowania w portalu usługi Azure Active Directory](active-directory-reporting-security-risky-sign-ins.md).  
