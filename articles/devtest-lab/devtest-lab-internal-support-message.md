---
title: "Dodaj instrukcję wewnętrznej pomocy technicznej do laboratorium w usłudze Azure DevTest Labs | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można opublikować instrukcji wewnętrznej pomocy technicznej do laboratorium w usłudze Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: c9900333-6c5e-4d7d-b0f4-889015e9550c
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2017
ms.author: v-craic
ms.openlocfilehash: 33c2f1e9d1ffa83003b7f0dbba46a7a4588dd57a
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/02/2018
---
# <a name="add-an-internal-support-statement-to-a-lab-in-azure-devtest-labs"></a>Dodaj instrukcję wewnętrznej pomocy technicznej do laboratorium w usłudze Azure DevTest Labs

Azure DevTest Labs umożliwia dostosowanie środowiska laboratoryjnego za pomocą instrukcji wewnętrznej pomocy technicznej, która dostarcza użytkownikom informacje techniczne dotyczące laboratorium. Na przykład można określić informacje kontaktowe, co użytkownik zna możliwości dotarcia wewnętrznej pomocy technicznej, jeśli potrzebna jest pomoc dotyczącą rozwiązywania problemów oraz uzyskiwania dostępu do zasobów w środowisku laboratoryjnym. Można też podać łącza do wewnętrznej witryny sieci Web lub często zadawane pytania, których dostęp zespołu przed kontaktu z pomocą techniczną.

Instrukcja wewnętrznej pomocy technicznej jest przeznaczona do umożliwiają ogłaszanie informacji laboratoryjnych, która zazwyczaj nie zmienia zbyt często. Aby powiadomić użytkowników o informacji laboratorium, które jest bardziej tymczasowy charakter — takie jak najnowsze aktualizacje zasad laboratorium — zobacz [anons w laboratorium](devtest-lab-announcements.md).

Łatwo można wyłączyć lub edytować oświadczenie pomocy technicznej, po jej nie ma już zastosowania.

## <a name="steps-to-add-a-support-statement-to-an-existing-lab"></a>Kroki, aby dodać do istniejącego laboratorium oświadczenie pomocy technicznej

1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. W razie potrzeby wybierz **wszystkie usługi**, a następnie wybierz **DevTest Labs** z listy. (Laboratorium może być jest wyświetlana na pulpicie nawigacyjnym w obszarze **wszystkie zasoby**).
1. Z listy labs wybierz laboratorium, w której chcesz dodać oświadczenie pomocy technicznej.  
1. W laboratorium **omówienie** wybierz opcję **konfiguracji i zasadach**.  

    ![Przycisk zasad i konfiguracji](./media/devtest-lab-internal-support-message/devtestlab-config-and-policies.png)

1. Po lewej stronie w obszarze **ustawienia**, wybierz pozycję **wewnętrznej pomocy technicznej**.

    ![Przycisk wewnętrznej pomocy technicznej](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)

1. Aby utworzyć komunikat wewnętrznej pomocy technicznej dla użytkowników, w tym laboratorium, ustaw włączone **tak**.

1. W **komunikat pomocy technicznej** wprowadź instrukcji wewnętrznej pomocy technicznej, które mają być przedstawiane użytkownikom laboratorium. Komunikat pomocy technicznej akceptuje Markdown. Tekst komunikatu jest wprowadzana, można wyświetlić **Podgląd** obszarze u dołu ekranu, aby zobaczyć, jak komunikat zostanie wyświetlony dla użytkowników.

    ![Ekran wewnętrznej pomocy technicznej można utworzyć komunikatu.](./media/devtest-lab-internal-support-message/devtestlab-add-support-statement.png)


1. Wybierz **zapisać** po instrukcji pomocy technicznej jest gotowy do publikowania.

Jeśli nie chcesz już nie pokazuj tego komunikatu pomocy technicznej dla użytkowników w laboratorium, wróć do **wewnętrznej pomocy technicznej** strony i ustaw **włączone** do **nr**.

## <a name="steps-for-users-to-view-the-support-message"></a>Kroki do wyświetlania komunikatu pomocy technicznej

1. Z [portalu Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), wybierz laboratorium.

1. W laboratorium **omówienie** wybierz opcję **wewnętrznej pomocy technicznej**.  

    ![Przycisk wewnętrznej pomocy technicznej](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)


1. Jeśli publikowania wiadomości pomocy technicznej, użytkownik może przeglądać go z okienka wewnętrznej pomocy technicznej.

    ![Okienko obsługi wewnętrznej zaksięgowany komunikat pomocy technicznej](./media/devtest-lab-internal-support-message/devtestlab-view-suport-statement.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Kolejne kroki
* Oświadczenia dotyczące pomocy technicznej wewnętrzne są zwykle używane do zapewniają informacje pomocy technicznej, które nie zmieniają się który często. Możesz też dowiedzieć się, jak [post anonsu do laboratorium](devtest-lab-announcements.md) aby informować użytkowników o tymczasowej zmiany lub aktualizacje do laboratorium.
* [Ustaw zasady i harmonogramy](devtest-lab-set-lab-policy.md) wyjaśniono, jak można stosować inne ograniczenia i konwencje między subskrypcją za pomocą niestandardowych zasad.