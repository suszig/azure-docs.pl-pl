---
title: "Zarządzanie kontem magazynu StorSimple | Dokumentacja firmy Microsoft"
description: "W tym artykule wyjaśniono, jak można strony konfiguracji Menedżer StorSimple umożliwia dodawanie, edytowanie i usuwanie lub Obróć kluczy zabezpieczeń dla konta magazynu."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 93207c40-e0eb-489e-8724-59fb94907081
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: 4bf8b397d81e12bc48fe3d0ce16d5fff705cedbc
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-your-storage-account"></a>Zarządzanie kontem magazynu za pomocą usługi Menedżer StorSimple
> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Aby wyświetlić wersję tego artykułu dla nowego portalu Azure, przejdź do [Zarządzanie kontem magazynu za pomocą usługi Menedżer StorSimple](storsimple-8000-manage-storage-accounts.md). Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Omówienie
**Konfiguruj** strona przedstawia wszystkie parametry usługi global service, które mogą zostać utworzone w usłudze Menedżer StorSimple. Te parametry można zastosować do wszystkich urządzeń podłączonych do usługi i obejmują:

* Konta magazynu 
* Szablony przepustowości 
* Rekordy kontroli dostępu 

W tym samouczku opisano, jak używasz **Konfiguruj** strony można dodać, edytować lub usuwać konta magazynu lub Obróć kluczy zabezpieczeń dla konta magazynu.

 ![Konfigurowanie strony](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Konta magazynu zawiera poświadczenia, których urządzenie używa do uzyskania dostępu do konta magazynu z dostawcy usług w chmurze. W przypadku kont magazynu Microsoft Azure są to poświadczenia, takie jak nazwa konta i podstawowy klucz dostępu. 

Na **Konfiguruj** stronę, wszystkie konta magazynu utworzone na potrzeby rozliczeń subskrypcji są wyświetlane w formacie tabelarycznym zawierający następujące informacje:

* **Nazwa** — unikatowa nazwa przypisana do konta, jeśli został on utworzony.
* **Włączony protokół SSL** — czy SSL jest włączona i urządzenia do chmury komunikacja odbywa się za pośrednictwem bezpiecznego kanału.
* **Używane przez** — liczba woluminów przy użyciu konta magazynu.

Typowe zadania związane z kont magazynu, które mogą być wykonywane na **Konfiguruj** są:

* Dodawanie konta magazynu 
* Edytuj konto magazynu 
* Usuwanie konta magazynu 
* Rotacją kluczy kont magazynu 

## <a name="types-of-storage-accounts"></a>Typy kont magazynu
Istnieją trzy typy kont magazynu, które mogą być używane z urządzenia StorSimple.

* **Konta magazynu automatycznie generowanej** — zgodnie z sugestią, nazwa, ten typ konta magazynu jest generowana automatycznie po utworzeniu usługi. Aby dowiedzieć się więcej na temat tworzenia tego konta magazynu, zobacz [krok 1: Tworzenie nowej usługi](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) w [wdrażanie lokalnego urządzenia StorSimple](storsimple-deployment-walkthrough.md). 
* **Konta magazynu w ramach subskrypcji usługi** — są to konta magazynu platformy Azure, które są skojarzone z tej samej subskrypcji co usługa. Aby dowiedzieć się więcej, Magazyn, jak te informacje są tworzone konta, zobacz [o kontach magazynu Azure](../storage/common/storage-create-storage-account.md). 
* **Konta magazynu poza subskrypcji usługi** — są to konta magazynu platformy Azure, które nie są skojarzone z Twoją usługą i prawdopodobnie istniały przed usługa została utworzona.

## <a name="add-a-storage-account"></a>Dodawanie konta magazynu
Można dodać konto magazynu, podając unikatową przyjazną nazwę i poświadczenia dostępu, które są połączone z kontem magazynu (określonej chmury dostawcy usług). Masz również opcję włączenia trybu bezpiecznego sockets layer (SSL), aby utworzyć bezpiecznego kanału komunikacji sieciowej między urządzeniem i chmury.

Można utworzyć wiele kont dla określonej chmury dostawcy usług. Należy jednak pamiętać, że po utworzeniu konta magazynu nie można zmienić dostawcy usług w chmurze.

Przy zapisywaniu konta magazynu, usługa próbuje nawiązać połączenia z dostawcą usług w chmurze. W tej chwili będą uwierzytelniane poświadczenia i materiały dostępu, który został dostarczony. Konto magazynu jest tworzony tylko wtedy, gdy uwierzytelnienie zakończy się pomyślnie. Jeśli uwierzytelnianie nie powiedzie się, następnie pojawi się odpowiedni komunikat o błędzie.

Menedżer zasobów kont magazynu utworzonych w portalu Azure również są obsługiwane z StorSimple. Menedżer zasobów konta magazynu nie pojawi się na liście rozwijanej do wyboru podczas próby utworzenia kontenera woluminów, tylko kont magazynu, które są tworzone w klasycznym portalu Azure będzie wyświetlany. Menedżer zasobów konta magazynu będzie musi zostać dodany przy użyciu procedury, aby dodać konto magazynu opisane poniżej.

> [!NOTE]
> Procedura dodawania konta magazynu różni się w zależności od używanej wersji oprogramowania StorSimple. Pamiętaj wykonać poprawną procedurę dla danej wersji StorSimple.
> 
> 

[!INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[!INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Edytuj konto magazynu
Można edytować konto magazynu, który jest używany przez kontener woluminów. Po zmodyfikowaniu konta magazynu, który jest obecnie używany tylko pole umożliwia modyfikację jest klucz dostępu dla konta magazynu. Możesz podać nowy klucz dostępu do magazynu i Zapisz zaktualizowane ustawienia.

#### <a name="to-edit-a-storage-account"></a>Aby edytować konto magazynu
1. Strona docelowa usługi, wybierz usługę, kliknij dwukrotnie nazwę usługi, a następnie kliknij **Konfiguruj**.
2. Kliknij przycisk **Dodawanie/edytowanie konta magazynu**.
3. W **Dodawanie/edytowanie konta magazynu** okno dialogowe:
   
   1. Na liście rozwijanej **kont magazynu**, wybrać istniejące konto, które chcesz zmodyfikować. Może to także dotyczyć kontach magazynu, które zostały automatycznie generowane, gdy usługa została utworzona.
   2. Jeśli to konieczne, można zmodyfikować **Włącz tryb SSL** zaznaczenia.
   3. Istnieje możliwość obracania kluczy dostępu konta magazynu. Zobacz [klucza obrotu kont magazynu](#key-rotation-of-storage-accounts) Aby uzyskać więcej informacji na temat sposobu wykonywania rotacją kluczy.
   4. Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) Aby zapisać ustawienia. Ustawienia zostaną zaktualizowane na **Konfiguruj** strony. Kliknij przycisk **zapisać** można zapisać zaktualizowanych ustawień.
      
      ![Edytuj konto magazynu](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)

## <a name="delete-a-storage-account"></a>Usuwanie konta magazynu
> [!IMPORTANT]
> Konto magazynu można usunąć tylko wtedy, gdy nie jest on używany przez kontener woluminów. Jeśli konto magazynu jest używana przez kontener woluminów, należy najpierw usunąć kontener woluminów, a następnie usuń skojarzonego konta magazynu.
> 
> 

#### <a name="to-delete-a-storage-account"></a>Aby usunąć konto magazynu
1. Strona docelowa usługi Menedżer StorSimple, wybierz usługę, kliknij dwukrotnie nazwę usługi, a następnie kliknij **Konfiguruj**.
2. Na liście tabelarycznym kont magazynu umieść kursor nad konta, które chcesz usunąć.
3. Ikona usuwania (**x**) zostanie wyświetlony w prawej kolumnie extreme dla tego konta magazynu. Kliknij przycisk **x** ikonę, aby usunąć poświadczenia.
4. Po wyświetleniu monitu o potwierdzenie, kliknij przycisk **tak** aby kontynuować usuwanie. Tabelarycznej listę zostaną zaktualizowane, aby odzwierciedlić zmiany.

## <a name="key-rotation-of-storage-accounts"></a>Rotacją kluczy kont magazynu
Ze względów bezpieczeństwa klucza obrotu często jest to wymagane w centrach danych. 

> [!NOTE]
> Poniższe informacje rotacją kluczy i tej procedury dotyczą tylko kont magazynu Microsoft Azure. Jeśli korzystasz z innego dostawcy usług w chmurze, można zarządzać klucze konta magazynu za pośrednictwem pulpitu nawigacyjnego tego dostawcy.
> 
> 

Każda subskrypcja Microsoft Azure może mieć co najmniej jeden skojarzone konta magazynu. Dostęp do tych kont jest kontrolowana przez klucze dostępu i subskrypcji dla każdego konta magazynu. 

Podczas tworzenia konta magazynu Microsoft Azure generuje dwa klucze dostępu do magazynu 512-bitowe, które są używane do uwierzytelniania podczas uzyskiwania dostępu do konta magazynu. Mając dwa klucze dostępu do magazynu umożliwia ponowne generowanie kluczy nie przerw w usłudze magazynu i dostęp do tej usługi. Klucz, który jest obecnie używany jest *głównej* i klucz kopii zapasowej jest określany jako *dodatkowej* klucza. Gdy urządzenia Microsoft Azure StorSimple uzyskuje dostęp do dostawcy usługi w chmurze magazynu należy podać jedną z tych dwóch kluczy.

## <a name="what-is-key-rotation"></a>Co to jest rotacją kluczy?
Zazwyczaj aplikacje korzystają tylko jeden z kluczy dostępu do danych. Po upływie pewnego czasu program może przełączyć się do przy użyciu drugi klucz aplikacji. Po przełączeniu aplikacji klucza pomocniczego, można wycofać pierwszy klucz, a następnie wygeneruj nowy klucz. W ten sposób za pomocą dwóch kluczy umożliwia aplikacji dostęp do danych bez jakiegokolwiek przestoju.

Klucze konta magazynu zawsze są przechowywane w usłudze w postaci zaszyfrowanej. Jednak te można zresetować za pomocą usługi Menedżer StorSimple. Usługa może uzyskać klucz podstawowy i klucz pomocniczy dla wszystkich kont magazynu w tej samej subskrypcji, w tym kont utworzonych w usługi magazynu, a także magazynu domyślne tworzone konta generowane, gdy usługa Menedżer StorSimple po raz pierwszy. Usługa Menedżer StorSimple będzie zawsze pobiera klucze z klasycznego portalu Azure i przechowywać je w sposób zaszyfrowany.

## <a name="rotation-workflow"></a>Obracanie przepływu pracy
Administratora programu Microsoft Azure można ponownie wygenerować lub zmienić klucz podstawowy lub pomocniczy, uzyskując bezpośrednio dostęp do konta magazynu (za pomocą usługi Microsoft Azure Storage). Usługę Menedżer StorSimple nie widzi automatycznie tej zmiany.

Informują usługę Menedżer StorSimple zmiany, można będzie muszą uzyskać dostęp do usługi Menedżer StorSimple, uzyskać dostęp do konta magazynu, a następnie zsynchronizować klucz podstawowy lub pomocniczy (w zależności od tego, co zostało zmienione). Następnie usługa pobiera najnowszego klucza, szyfruje kluczy i wysyła zaszyfrowany klucz do urządzenia.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service-azure-only"></a>Aby zsynchronizować klucze kont magazynu w tej samej subskrypcji co usługa (tylko wersja Azure)
1. Na **usług** kliknij przycisk **Konfiguruj** kartę.
2. Kliknij przycisk **Dodawanie/edytowanie konta magazynu**.
3. W oknie dialogowym wykonaj następujące czynności:
   
   1. Wybierz konto magazynu przy użyciu klucza, który chcesz synchronizować. Klucze konta magazynu są szyfrowane, gdy są one wyświetlane.
   2. W usłudze Menedżer StorSimple należy zaktualizować klucz, który wcześniej został zmieniony w usłudze Microsoft Azure Storage. Jeśli podstawowy klucz dostępu została zmieniona (wygenerowano ponownie), kliknij przycisk **zsynchronizować klucz podstawowy**. Jeśli klucz pomocniczy został zmieniony, kliknij przycisk **synchronizowanie klucza pomocniczego**.
      
      ![Synchronizowanie kluczy](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Aby zsynchronizować klucze kont magazynu poza subskrypcji usługi
1. Na **usług** kliknij przycisk **Konfiguruj** kartę.
2. Kliknij przycisk **Dodawanie/edytowanie konta magazynu**.
3. W oknie dialogowym wykonaj następujące czynności:
   
   1. Wybierz konto magazynu z kluczem dostępu, który chcesz zaktualizować.
   2. Należy zaktualizować klucz dostępu do magazynu w usłudze Menedżer StorSimple. W takim przypadku można wyświetlić klucz dostępu do magazynu. Wprowadź nowy klucz w **klucz dostępu do konta magazynu**y pole. 
   3. Zapisz zmiany. Należy teraz zaktualizować klucz dostępu konta magazynu.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [zabezpieczenia usługi StorSimple](storsimple-security.md).
* Dowiedz się więcej o [przy użyciu usługi Menedżer StorSimple do administrowania urządzeniem StorSimple](storsimple-manager-service-administration.md).

