---
title: "AS2 wiadomości B2B integracji przedsiębiorstwa - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Wymiana komunikatów AS2 B2B enterprise integracji z usługą Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: c9b7e1a9-4791-474c-855f-988bd7bf4b7f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 91b2f16611b88aa4b9395ca301d88042065ad9dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="exchange-as2-messages-for-enterprise-integration-with-logic-apps"></a>Wymiana komunikatów AS2 enterprise integracji z usługą logic apps

Przed przystąpieniem do wymiany wiadomości AS2 dla usługi Azure Logic Apps, należy utworzyć umowy AS2 i przechowywać na koncie integracji tej Umowy. Poniżej przedstawiono procedurę tworzenia umowy AS2.

## <a name="before-you-start"></a>Przed rozpoczęciem

Oto elementy, które są potrzebne:

* [Konta integracji](../logic-apps/logic-apps-enterprise-integration-accounts.md) który został już zdefiniowany i skojarzone z subskrypcją platformy Azure
* Co najmniej dwa [partnerów](logic-apps-enterprise-integration-partners.md) który są już zdefiniowane w ramach konta integracji i skonfigurowane z kwalifikatorem AS2 w obszarze **tożsamości firm**

> [!NOTE]
> Podczas tworzenia umowy zawartości w pliku umowy musi odpowiadać typowi umowy.    

Po [Tworzenie konta usługi integracji](../logic-apps/logic-apps-enterprise-integration-accounts.md) i [dodać partnerów](logic-apps-enterprise-integration-partners.md), można utworzyć umowy AS2, wykonaj następujące czynności.

## <a name="create-an-as2-agreement"></a>Tworzenie umów AS2

1.  Zaloguj się w witrynie [Azure Portal](http://portal.azure.com "Azure Portal").  

2.  Wybierz z menu po lewej stronie **więcej usług**. W polu wyszukiwania wprowadź **integracji** jako filtr. Na liście wyników wybierz **konta integracji**.

    > [!TIP]
    > Jeśli nie widzisz **więcej usług**, trzeba będzie najpierw rozwinąć menu. W górnej części menu zwinięte, wybierz **Pokaż menu**.

    ![Więcej usług, filtr "integrację", wybierz "Konta integracji"](./media/logic-apps-enterprise-integration-agreements/overview-1.png)

3. W **konta integracji** bloku, który zostanie otwarty, wybierz konto integracji, w której chcesz utworzyć umowy.
Jeśli nie widzisz kont integracji [utworzyć pierwszy](../logic-apps/logic-apps-enterprise-integration-accounts.md "wszystkiego o konta integracji").  

    ![Wybierz konto integracji gdzie utworzyć umowy](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Wybierz **umowy** kafelka. Jeśli nie masz kafelka umowy, najpierw Dodaj kafelka.

    ![Wybierz Kafelek "Umów"](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

5. W bloku umów wybierz **Dodaj**.

    ![Wybierz opcję "Dodaj"](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)

6. W obszarze **Dodaj**, wprowadź **nazwa** dla umowy. Aby uzyskać **typ umowy**, wybierz pozycję **AS2**. Wybierz **partnera hosta**, **tożsamości hosta**, **partnera gościa**, i **tożsamości gościa** dla umowy.

    ![Podaj szczegóły umowy](./media/logic-apps-enterprise-integration-agreements/agreement-3.png)  

    | Właściwość | Opis |
    | --- | --- |
    | Nazwa |Nazwa umowy |
    | Typ umowy | Powinien być AS2 |
    | Partner hosta |Umowa musi mieć partnera zarówno hosta, jak i gościa. Partner hosta reprezentuje organizację, która konfiguruje umowy. |
    | Tożsamość hosta |Identyfikator partnera hosta |
    | Partner gościa |Umowa musi mieć partnera zarówno hosta, jak i gościa. Partner gościa reprezentuje organizację, która jest działalność z partnerem hosta. |
    | Tożsamość gościa |Identyfikator partnera gościa |
    | Odbierają ustawienia |Te właściwości stosowane do wszystkich komunikatów odebranych przez umowy. |
    | Wyślij ustawienia |Te właściwości stosowane do wszystkich wiadomości wysłanych przez umowy. |

## <a name="configure-how-your-agreement-handles-received-messages"></a>Skonfiguruj sposób dojść do Porozumienia odebrane wiadomości

Teraz, gdy ustawiono właściwości umowy, można skonfigurować sposób identyfikuje niniejszej Umowy oraz obsługi przychodzących komunikatów odebranych od swojego partnera, za pośrednictwem niniejszej Umowy.

1.  W obszarze **Dodaj**, wybierz pozycję **ustawienia odbierania**.
Skonfigurować te właściwości, na podstawie Twojej umowy z partnerem, który wymienia wiadomości z Tobą. Opis właściwości Zobacz tabelę w tej sekcji.

    ![Skonfiguruj "Otrzymywać ustawienia"](./media/logic-apps-enterprise-integration-agreements/agreement-4.png)

2. Opcjonalnie można zastąpić właściwości wiadomości przychodzących, wybierając **zastąpienie właściwości komunikatu**.

3. Aby wymagać komunikaty przychodzące były podpisane, wybierz **wiadomości powinny być podpisane**. Z **certyfikatu** listy, wybierz istniejący [certyfikatu publicznego partnera gościa](../logic-apps/logic-apps-enterprise-integration-certificates.md) sprawdzania poprawności podpisu dla wiadomości. Lub Utwórz certyfikat, jeśli nie masz.

4.  Aby wymagać komunikaty przychodzące były szyfrowane, wybierz **można zaszyfrować wiadomości**. Z **certyfikatu** listy, wybierz istniejący [certyfikatu prywatnego partnera hosta](../logic-apps/logic-apps-enterprise-integration-certificates.md) do odszyfrowywania wiadomości przychodzących. Lub Utwórz certyfikat, jeśli nie masz.

5. Aby wymagać wiadomości ma być skompresowany, wybierz **wiadomości powinna być kompresowane**.

6. Aby wysłać powiadomienie dyspozycji komunikatu synchronicznego (MDN) po odebraniu wiadomości, zaznacz **wysyłania MDN**.

7. Aby wysłać podpisem MDNs po odebraniu wiadomości, zaznacz **wysyłania podpisany MDN**.

8. Aby wysłać asynchroniczne MDNs po odebraniu wiadomości, zaznacz **Wysyłanie asynchroniczne MDN**.

9. Po zakończeniu upewnij się zapisać ustawienia, wybierając **OK**.

Umowie jest teraz gotowy do obsługi wiadomości przychodzących, które odpowiadają wybrane ustawienia.

| Właściwość | Opis |
| --- | --- |
| Zastąpienie właściwości wiadomości |Wskazuje, można zastąpić właściwości w odebranej wiadomości. |
| Komunikat powinien być podpisany |Wymaga komunikaty były podpisane cyfrowo. Konfigurowanie certyfikatu publicznego partnera gościa do sprawdzenia podpisu.  |
| Komunikat powinien być zaszyfrowany |Wymaga szyfrowanie wiadomości. Wiadomości zaszyfrowanych przez inne niż są odrzucane. Skonfiguruj certyfikat prywatny partnera hosta do odszyfrowywania wiadomości.  |
| Komunikat powinien być skompresowany |Wymaga wiadomości do skompresowania. Skompresowane inne niż wiadomości są odrzucane. |
| Tekst MDN |Domyślne dyspozycji powiadomienia (MDN) do wysłania do nadawcy wiadomości. |
| Wyślij MDN |Wymaga MDNs do wysłania. |
| Wyślij MDN podpisem |Wymaga MDNs były podpisane. |
| Algorytm Micznych |Wybierz algorytm podpisywania wiadomości. |
| Wysyłanie asynchroniczne MDN | Wymaga komunikatów do wysłania asynchronicznie. |
| ADRES URL | Podaj adres URL, który ma zostać wysłana MDNs. |

## <a name="configure-how-your-agreement-sends-messages"></a>Skonfiguruj sposób umowie wysyłania wiadomości

Można skonfigurować sposób identyfikuje niniejszej Umowy oraz obsługi komunikatów wychodzących, które wysyłają do partnerów za pośrednictwem niniejszej Umowy.

1.  W obszarze **Dodaj**, wybierz pozycję **ustawienia wysyłania**.
Skonfigurować te właściwości, na podstawie Twojej umowy z partnerem, który wymienia wiadomości z Tobą. Opis właściwości Zobacz tabelę w tej sekcji.

    ![Ustaw właściwości "Wyślij ustawienia"](./media/logic-apps-enterprise-integration-agreements/agreement-51.png)

2. Aby podpisanych wiadomości wysyłane do partnerów, wybierz **włączyć podpisywanie komunikatów**. Do podpisywania wiadomości, w **algorytm Micznych** listy, wybierz *certyfikatu prywatnego hosta partnera algorytmu Micznych*. I w **certyfikatu** listy, wybierz istniejący [certyfikatu prywatnego partnera hosta](../logic-apps/logic-apps-enterprise-integration-certificates.md).

3. Do wysyłania zaszyfrowanych wiadomości do partnera, wybierz **włączyć szyfrowanie wiadomości**. Do szyfrowania wiadomości, w **algorytm szyfrowania** listy, wybierz *gościa partnera certyfikatu publicznego algorytmu*.
I w **certyfikatu** listy, wybierz istniejący [certyfikatu publicznego partnera gościa](../logic-apps/logic-apps-enterprise-integration-certificates.md).

4. Aby skompresować wiadomości, wybierz **Włącz kompresję komunikat**.

5. Aby ujawniać nagłówka content-type protokołu HTTP w jednym wierszu, wybierz **nagłówków HTTP ujawniać**.

6. Aby otrzymać synchroniczne MDNs wysłanych wiadomości, zaznacz **MDN żądania**.

7. Aby otrzymać podpisane MDNs wysłanych wiadomości, zaznacz **MDN podpisane żądanie**.

8. Aby otrzymać asynchroniczne MDNs wysłanych wiadomości, zaznacz **żądania asynchroniczne MDN**. Jeśli wybierzesz tę opcję, wprowadź adres URL umożliwiający wysyłanie MDNs.

9. Aby wymagać bez odrzucania odbioru, wybierz **włączyć NRR**.  

10. Aby określić algorytm formatu do użycia w Micznych lub logowanie nagłówków wychodzących wiadomości AS2 lub MDN, zaznacz **format algorytmu SHA2**.  

11. Po zakończeniu upewnij się zapisać ustawienia, wybierając **OK**.

Umowie jest teraz gotowy do obsługi komunikatów wychodzących, które odpowiadają wybrane ustawienia.

| Właściwość | Opis |
| --- | --- |
| Włącz podpisywania wiadomości |Wymaga wszystkich wiadomości wysłanych z umowy były podpisane. |
| Algorytm Micznych |Algorytm używany do podpisywania wiadomości. Konfiguruje certyfikatu prywatnego partnera hosta Mikrofon algorytm podpisywania wiadomości. |
| Certyfikat |Wybierz certyfikat do użycia podczas podpisywania wiadomości. Umożliwia skonfigurowanie certyfikatu prywatnego partnera hosta do podpisywania wiadomości. |
| Włącz szyfrowanie wiadomości |Wymaga szyfrowania wszystkich wiadomości wysłanych z niniejszej Umowy. Określa algorytm gościa partnera publicznego certyfikatu do szyfrowania wiadomości. |
| Algorytm szyfrowania |Algorytm szyfrowania do użycia na potrzeby szyfrowania wiadomości. Konfiguruje gościa partnera publiczny certyfikatu do szyfrowania wiadomości. |
| Certyfikat |Certyfikat używany do szyfrowania wiadomości. Konfiguruje gościa partnera prywatny certyfikatu do szyfrowania wiadomości. |
| Włącz kompresję wiadomości |Wymaga kompresji wszystkich wiadomości wysłanych z niniejszej Umowy. |
| Unfold — nagłówki HTTP |Umieszcza nagłówka content-type protokołu HTTP na jeden wiersz. |
| Żądanie MDN |Wymaga MDN dla wszystkich wiadomości wysłanych z niniejszej Umowy. |
| Żądanie podpisany MDN |Wymaga wszystkich MDNs, które są wysyłane do podpisania tej Umowy. |
| Żądania asynchroniczne MDN |Wymaga asynchroniczne MDNs do wysłania do tej Umowy. |
| ADRES URL |Podaj adres URL, który ma zostać wysłana MDNs. |
| Włącz NRR |Wymaga bez odrzucania odbioru (NRR), atrybut komunikacji, który udostępnia dowód odebrania danych jako problemu. |
| Format algorytmu SHA2 |Wybierz format algorytmu do użycia w Micznych lub logowanie nagłówków wychodzących wiadomości AS2 lub MDN |

## <a name="find-your-created-agreement"></a>Znajdź utworzone umowy

1.  Po zakończeniu ustawienie na wszystkie właściwości umowy, **Dodaj** bloku, wybierz **OK** w stanie zakończyć tworzenia umowy, a następnie wróć do bloku konta usługi z integracji.

    Nowo dodany umowy teraz pojawia się w sieci **umowy** listy.

2.  Można również wyświetlić umów w przeglądzie konta integracji. W bloku konta integracji, wybierz **omówienie**, a następnie wybierz pozycję **umowy** kafelka. 

    ![Wybierz Kafelek "Umów", aby wyświetlić wszystkie umowy](./media/logic-apps-enterprise-integration-agreements/agreement-6.png)

## <a name="view-the-swagger"></a>Wyświetlanie struktury swagger
Zobacz [swagger szczegóły](/connectors/as2/). 

## <a name="next-steps"></a>Następne kroki
* [Dowiedz się więcej o pakiet integracyjny dla przedsiębiorstw](logic-apps-enterprise-integration-overview.md "Dowiedz się więcej na temat pakiet integracyjny dla przedsiębiorstw")  
