---
title: "Szyfrowanie w usłudze Azure Data Lake Store | Microsoft Docs"
description: "Omówienie sposobu działania szyfrowania i wymiany kluczy w usłudze Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: yagupta
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 4/14/2017
ms.author: yagupta
ms.translationtype: Human Translation
ms.sourcegitcommit: db034a8151495fbb431f3f6969c08cb3677daa3e
ms.openlocfilehash: 9bd9996a4a22b2f57510b6f3b6625e22b3183b1c
ms.contentlocale: pl-pl
ms.lasthandoff: 04/29/2017

---

# <a name="encryption-of-data-in-azure-data-lake-store"></a>Szyfrowanie danych w usłudze Azure Data Lake Store

## <a name="overview-of-encryption-in-azure-data-lake-store"></a>Omówienie szyfrowania w usłudze Azure Data Lake Store

Szyfrowanie w usłudze Azure Data Lake Store (ADLS) zapewnia możliwość ochrony danych, wdrażania zasad bezpieczeństwa w przedsiębiorstwie i spełniania prawnych wymagań dotyczących zgodności. Ten artykuł zawiera omówienie projektu oraz technicznych aspektów dotyczących sposobu wdrażania szyfrowania danych przez usługę Data Lake Store.

Usługa Azure Data Lake Store domyślnie obsługuje przezroczyste szyfrowanie danych magazynowanych. Poniżej przedstawiono bardziej szczegółowe wyjaśnienie tych terminów:

* Domyślnie włączone: podczas tworzenia nowego konta usługi Azure Data Lake Store domyślne ustawienie włącza szyfrowanie. Dzięki temu dane przechowywane w usłudze Data Lake Store są zawsze szyfrowane jeszcze przed zapisaniem ich na nośniku trwałym. Takie działanie dotyczy wszystkich danych i nie można tego zmienić po utworzeniu konta.
* Przezroczyste: usługa Azure Data Lake Store automatycznie szyfruje dane przed utrwaleniem i odszyfrowuje przed pobraniem. Szyfrowanie jest konfigurowane i zarządzane na poziomie usługi Data Lake Store przez administratora. Żadne zmiany nie są wprowadzane w interfejsach API dostępu do danych, w związku z tym nie ma konieczności wprowadzania zmian w aplikacjach i usługach, które współdziałają z usługą Data Lake Store w kwestii szyfrowania.

Dane przesyłane (inaczej dane w ruchu) również są zawsze szyfrowane w usłudze Data Lake Store. Oprócz tego, że dane są szyfrowane przed zapisaniem na nośniku trwałym, są również zawsze zabezpieczane podczas przesyłania lub w ruchu przy użyciu protokołu HTTPS. Protokół HTTPS jest jedynym protokołem obsługiwanym przez interfejsy REST usługi Data Lake Store.

![Rysunek 1.](./media/data-lake-store-encryption/fig1.png)


## <a name="setting-up-encryption-with-azure-data-lake-store"></a>Konfigurowanie szyfrowania przy użyciu usługi Azure Data Lake Store

Szyfrowanie w usłudze Azure Data Lake Store konfiguruje się podczas tworzenia konta i zawsze jest domyślnie włączone. Klienci mogą sami zarządzać kluczami lub zezwolić usłudze Azure Data Lake Store (domyślnie) na zarządzanie kluczami w ich imieniu.

Aby dowiedzieć się, jak skonfigurować szyfrowanie za pomocą usługi Azure Data Lake Store, zobacz [Wprowadzenie](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal)

## <a name="under-the-hood--how-encryption-works-in-azure-data-lake-store"></a>Za kulisami — jak działa szyfrowanie w usłudze Azure Data Lake Store

### <a name="master-encryption-keys"></a>Główne klucze szyfrowania

Usługa Azure Data Lake Store udostępnia dwa tryby zarządzania głównymi kluczami szyfrowania. Użycie głównych kluczy szyfrowania opisano szczegółowo w dalszej części artykułu. Na razie załóżmy, że główny klucz szyfrowania jest kluczem najwyższego poziomu. Aby móc odszyfrować dowolne dane przechowywane w usłudze Data Lake Store, wymagany jest dostęp do głównego klucza szyfrowania.

Oto dwa tryby zarządzania głównym kluczem szyfrowania:

1.    Klucze zarządzane przez usługę
2.    Klucze zarządzane przez klienta

W obu trybach główny klucz szyfrowania jest zabezpieczony dzięki przechowywaniu go w usłudze Azure Key Vault. Azure Key Vault to w pełni zarządzana, wysoce bezpieczna usługa platformy Azure, która może służyć do ochrony kluczy kryptograficznych. Więcej informacji na temat usługi Azure Key Vault jest dostępnych [tutaj](https://azure.microsoft.com/services/key-vault)

Oto krótkie porównanie możliwości oferowanych przez dwa tryby zarządzania głównymi kluczami szyfrowania.

|  | Klucze zarządzane przez usługę | Klucze zarządzane przez klienta |
| --- | --- | --- |
|W jaki sposób przechowywane są dane?|Są zawsze szyfrowane przed zapisaniem|Są zawsze szyfrowane przed zapisaniem|
|Gdzie jest przechowywany główny klucz szyfrowania?|W usłudze Azure Key Vault|W usłudze Azure Key Vault|
|Czy niektóre główne klucze szyfrowania są przechowywane poza usługą Azure Key Vault?|Nie|Nie|
|Czy można pobrać główny klucz szyfrowania z usługi Azure Key Vault?|Nie. Po umieszczeniu klucza w magazynie kluczy można go używać tylko do szyfrowania i odszyfrowywania.|Nie. Po umieszczeniu klucza w magazynie kluczy można go używać tylko do szyfrowania i odszyfrowywania.|
|Kto jest właścicielem usługi Azure Key Vault i głównego klucza szyfrowania?|Usługa Azure Data Lake Store.|Właścicielem usługi Azure Key Vault, która należy do subskrypcji platformy Azure, jest klient. Główny klucz szyfrowania w magazynie kluczy może być zarządzany z poziomu oprogramowania lub sprzętu (moduł HSM).|
|Czy klient może odwołać dostęp usługi Azure Data Lake Store do głównego klucza szyfrowania?|Nie|Tak. Klient może zarządzać listami kontroli dostępu w usłudze Azure Key Vault i usuwać pozycje kontroli dostępu do tożsamości usługi w ramach usługi Azure Data Lake Store.|
|Czy klient może trwale usunąć główny klucz szyfrowania?|Nie|Tak. Jeśli klient usunie główny klucz szyfrowania z usługi Azure Key Vault, dane na koncie usługi Azure Data Lake Store nie będą mogły być odszyfrowane przez nikogo, łącznie z usługą Azure Data Lake Store. <br><br> Jeśli przed usunięciem głównego klucza szyfrowania z usługi Azure Key Vault klient utworzył jego jawną kopię zapasową, to można go przywrócić i odzyskać dane. Jednak jeśli przed usunięciem głównego klucza szyfrowania z usługi Azure Key Vault nie utworzono jego kopii zapasowej, dane na koncie usługi Azure Data Lake Store nigdy już nie będą mogły być odszyfrowane.|


Poza różnicą dotyczącą sposobu zarządzania głównym kluczem szyfrowania i magazynem kluczy, w którym ten klucz się znajduje, pozostałe elementy projektu są takie same dla obu trybów.

Wybierając tryb zarządzania głównym kluczem szyfrowania, należy pamiętać o kilku kwestiach.

1.    Podczas aprowizacji konta usługi Azure Data Lake Store można wybrać między kluczami zarządzanymi przez klienta, a kluczami zarządzanymi przez usługę Azure Data Lake Store
2.    Po aprowizacji konta usługi Azure Data Lake Store nie można zmienić trybu

### <a name="encryption-and-decryption-of-data"></a>Szyfrowanie i odszyfrowywanie danych

W projekcie szyfrowania danych w usłudze Azure Data Lake Store używane są trzy typy kluczy. W poniższej tabeli przedstawiono, jakie pełnią role:

| Klucz                   | Skrót | Skojarzony z | Lokalizacja magazynu                             | Typ       | Uwagi                                                                                                   |
|-----------------------|--------------|-----------------|----------------------------------------------|------------|---------------------------------------------------------------------------------------------------------|
| Główny klucz szyfrowania | GKS          | Konto usługi Azure Data Lake Store | W usłudze Azure Key Vault                              | Asymetryczny | Może być zarządzany przez usługę Azure Data Lake Store lub przez klienta                                                              |
| Klucz szyfrowania danych   | KSD          | Konto usługi Azure Data Lake Store | Magazyn trwały — zarządzany przez usługę Azure Data Lake Store | Symetryczny  | Klucza szyfrowania danych — szyfrowany przez główny klucz szyfrowania — jest przechowywany na nośniku trwałym usługi |
| Klucz szyfrowania bloków  | KSB          | Blok danych | Brak                                         | Symetryczny  | Klucz szyfrowania bloków jest tworzony na podstawie klucza szyfrowania danych i bloków danych                                                      |

Poniższy diagram przedstawia te koncepcje:

![Rysunek 2](./media/data-lake-store-encryption/fig2.png)

#### <a name="pseudo-algorithm-when-a-file-is-to-be-decrypted"></a>Pseudoalgorytm stosowany w przypadku odszyfrowywania pliku:
1.    Sprawdź, czy klucz szyfrowania danych dla konta usługi Azure Data Lake Store jest zapisany w pamięci podręcznej i gotowy do użycia.
    * Jeśli nie, odczytaj zaszyfrowany klucz szyfrowania danych z magazynu trwałego i prześlij go do usługi Azure Key Vault w celu odszyfrowania. Zapisz w pamięci podręcznej odszyfrowany klucz szyfrowania danych. Teraz jest gotowy do użycia.
2.    Dotyczy każdego bloku danych w pliku
    * Odczytaj zaszyfrowany blok danych z magazynu trwałego
    * Wygeneruj klucz szyfrowania bloków na podstawie klucza szyfrowania danych i zaszyfrowanego bloku danych
    * Użyj klucza szyfrowania bloków w celu odszyfrowania danych
#### <a name="pseudo-algorithm-when-a-block-of-data-is-to-be-encrypted"></a>Pseudoalgorytm stosowany w przypadku szyfrowania bloku danych:
1.    Sprawdź, czy klucz szyfrowania danych dla konta usługi Azure Data Lake Store jest zapisany w pamięci podręcznej i gotowy do użycia.
    * Jeśli nie, odczytaj zaszyfrowany klucz szyfrowania danych z magazynu trwałego i prześlij go do usługi Azure Key Vault w celu odszyfrowania. Zapisz w pamięci podręcznej odszyfrowany klucz szyfrowania danych. Teraz jest gotowy do użycia.
2.    Wygeneruj unikatowy klucz szyfrowania bloków dla bloku danych na podstawie klucza szyfrowania danych.
3.    Przy użyciu algorytmu AES-256 zaszyfruj blok danych kluczem szyfrowania bloków.
4.    Zapisz zaszyfrowany blok danych w magazynie trwałym

> [!NOTE] 
> Ze względu na wydajność odszyfrowany klucz szyfrowania danych jest zapisywany w pamięci podręcznej przez krótki czas i natychmiast usuwany po jego upływie. W przypadku nośnika trwałego klucz szyfrowania danych jest zawsze przechowywany w postaci zaszyfrowanej przez główny klucz szyfrowania.

## <a name="key-rotation"></a>Wymiana kluczy

W przypadku trybu zarządzania kluczami przez klienta usługa Azure Data Lake Store umożliwia wymianę głównego klucza szyfrowania. Aby dowiedzieć się, jak skonfigurować konto usługi Azure Data Lake Store z kluczami zarządzanymi przez klienta, zobacz stronę [Wprowadzenie](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

### <a name="pre-requisites"></a>Wymagania wstępne

Podczas konfigurowania konta usługi Azure Data Lake klienci zdecydowali się skorzystać z własnych kluczy. Po utworzeniu konta ta opcja nie może zostać zmieniona. Jeśli używasz domyślnych opcji szyfrowania, Twoje dane będą zawsze szyfrowane przy użyciu kluczy zarządzanych przez usługę Azure Data Lake — w przypadku tej opcji klient nie ma możliwości wymiany kluczy. W poniższych krokach przyjęto, że używasz kluczy zarządzanych przez klienta (masz wybrane własne klucze z magazynu kluczy).

### <a name="how-to-rotate-the-key-mek-in-azure-data-lake-store"></a>Jak wymienić główny klucz szyfrowania w usłudze Azure Data Lake Store

1. Zaloguj się w nowej witrynie [Azure Portal](https://portal.azure.com/)
2. Przejdź do magazynu kluczy, który przechowuje klucze skojarzone z Twoim kontem usługi Azure Data Lake Store, a następnie wybierz pozycję Klucze.

    ![Klucze](./media/data-lake-store-encryption/keyvault.png)

3.    Wybierz klucz skojarzony z kontem usługi Azure Data Lake Store i utwórz nową wersję tego klucza.
  
   W tym momencie usługa Azure Data Lake obsługuje wyłącznie wymianę kluczy na nową wersję klucza, wymiana na inny klucz nie jest obsługiwana

   ![Nowa wersja](./media/data-lake-store-encryption/keynewversion.png)

4.    Przejdź do konta usługi Azure Data Lake Store i wybierz pozycję Szyfrowanie

    ![Nowa wersja](./media/data-lake-store-encryption/select-encryption.png)

5.    Pojawi się uwaga informująca o dostępności nowej wersji klucza oraz przycisk umożliwiający wymianę obecnego klucza na nowy. Kliknij polecenie Wymień klucz, aby zaktualizować klucz do nowej wersji.

    ![Gotowe](./media/data-lake-store-encryption/rotatekey.png)

6. Ta operacja powinna zająć mniej niż 2 minuty i nie powinna powodować przestoju. Po zakończeniu operacji używana jest nowa wersja klucza.

