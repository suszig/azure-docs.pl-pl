---
title: "O nazwie lokalizacjach w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Konfigurując o nazwie lokalizacji, możesz uniknąć generowanie adresów IP, które są własnością organizacji fałszywych alarmów dla Impossible podróż do nietypowych lokalizacji ryzyka zdarzeń typu."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: da437908509e40386ed23863648bd6956b308186
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="named-locations-in-azure-active-directory"></a>Nazwane lokalizacje w usłudze Azure Active Directory

Dzięki funkcji o nazwie lokalizacji usługi Azure Active Directory można opisać zaufanych zakresów adresów IP w Twojej organizacji. W danym środowisku, można użyć nazwane lokalizacje w kontekście wykrywania [ryzyka zdarzenia](active-directory-reporting-risk-events.md). Pomaga zmniejszyć liczbę fałszywych alarmów zgłoszone dla *Impossible podróż do nietypowych lokalizacji* ryzyka typ zdarzenia. 

## <a name="configuration"></a>Konfiguracja

Aby skonfigurować lokalizację o nazwie:

1. Zaloguj się do [portalu Azure](https://portal.azure.com) jako administrator globalny.

2. W okienku po lewej stronie kliknij **usługi Azure Active Directory**.

    ![Łącze usługi Azure Active Directory w okienku po lewej stronie](./media/active-directory-named-locations/01.png)

3. Na **usługi Azure Active Directory** bloku, w **zabezpieczeń** kliknij **dostępu warunkowego**.

    ![Polecenie dostępu warunkowego](./media/active-directory-named-locations/05.png)


4. Na **dostępu warunkowego** bloku, w **Zarządzaj** kliknij **o nazwie lokalizacje**.

    ![Polecenie lokalizacji o nazwie](./media/active-directory-named-locations/06.png)


5. Na **o nazwie lokalizacje** bloku, kliknij przycisk **nową lokalizację**.

    ![Nowe polecenie lokalizacji](./media/active-directory-named-locations/07.png)


6. Na **nowy** blok, wykonaj następujące czynności:

    ![Nowy blok](./media/active-directory-named-locations/56.png)

    a. W **nazwa** wpisz nazwę dla nazwanego lokalizacji.

    b. W **zakresów IP** wpisz zakres adresów IP. Zakres adresów IP musi być w *Bezklasowego routingu międzydomenowego* formacie (CIDR).  

    c. Kliknij przycisk **Utwórz**.



## <a name="what-you-should-know"></a>Co należy wiedzieć

**Aktualizacje zbiorcze**: tworzenia lub aktualizowania nazwane lokalizacje aktualizacje zbiorcze, możesz przekazać lub pobrać plik CSV z zakresu adresów IP. Przekazanie dodaje zakresu adresów IP w pliku do listy zamiast zastępowanie na liście.

![Przekazywanie i pobieranie łącza](./media/active-directory-named-locations/09.png)


**Ograniczenia**: można określić maksymalnie 60 nazwane lokalizacje, z jeden zakres adresów IP przypisanych do każdego z nich. Jeśli masz tylko jedną lokalizację o nazwie skonfigurowane do 500 zakresów adresów IP można zdefiniować dla niego.


## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat zdarzeń o podwyższonym ryzyku, zobacz [zdarzenia o podwyższonym ryzyku usługi Azure Active Directory](active-directory-reporting-risk-events.md).

