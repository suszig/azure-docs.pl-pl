<properties
    pageTitle="Tworzenie konta usługi Azure Batch | Microsoft Azure"
    description="Dowiedz się, jak utworzyć konto usługi Azure Batch w portalu Azure w celu równoległego uruchamiania dużych obciążeń w chmurze"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/01/2016"
    ms.author="marsma"/>

# Tworzenie konta usługi Azure Batch i zarządzanie nim w portalu Azure

> [AZURE.SELECTOR]
- [Portal Azure](batch-account-create-portal.md)
- [Batch Management .NET](batch-management-dotnet.md)

[Portal Azure][azure_portal] zawiera narzędzia niezbędne do utworzenia konta usługi Azure Batch i zarządzania nim. Z konta tego można korzystać w celu równoległego przetwarzania dużych obciążeń. W tym artykule omawiamy krok po kroku proces tworzenia konta usługi Azure Batch przy użyciu portalu, a także jego ważne ustawienia i właściwości. Na przykład aplikacje i usługi tworzone przy użyciu usługi Azure Batch wymagają adresu URL konta oraz klucza dostępu, aby komunikować się z interfejsami API usługi Batch, które są dostępne w portalu Azure.

>[AZURE.NOTE] Portal Azure obsługuje obecnie podzbiór funkcji w usłudze Batch, w tym tworzenie konta, zarządzanie jego ustawieniami i właściwościami oraz tworzenie i monitorowanie pul i zadań. Pełny zestaw funkcji usługi Batch jest dostępny dla deweloperów za pośrednictwem jej interfejsów API.

## Tworzenie konta usługi Batch

1. Zaloguj się do [portalu Azure][azure_portal].

2. Kliknij opcję **Nowy** > **Virtual Machines** > **Usługa Batch**.

    ![Usługa Batch w witrynie Marketplace][marketplace_portal]

3. Wyświetlony zostanie blok **Nowe konto usługi Batch**. Każdy element bloku opisano poniżej w punktach od *a* do *e*.

    ![Tworzenie konta usługi Batch][account_portal]

    a. **Nazwa konta**: unikatowa nazwa konta usługi Batch. Nazwa musi być niepowtarzalna w regionie usługi Azure, w którym konto zostanie utworzone (patrz punkt *Lokalizacja* poniżej). Może zawierać tylko małe litery i cyfry oraz mieć od 3 do 24 znaków.

    b. **Subskrypcja**: subskrypcja, w której ma zostać utworzone konto usługi Batch. Jeśli masz tylko jedną subskrypcję, jest ona wybrana domyślnie.

    c. **Grupa zasobów**: istniejąca grupa zasobów dla nowego konta usługi Batch. Opcjonalnie można utworzyć nową grupę zasobów.

    d. **Lokalizacja**: region usługi Azure, w którym ma zostać utworzone konto usługi Batch. Tylko regiony obsługiwane przez subskrypcję i grupę zasobów będą wyświetlane jako opcje.

    e. **Konto magazynu** (opcjonalne): konto magazynu **Ogólnego przeznaczenia** skojarzone (link) z nowym kontem usługi Batch. Funkcja [pakiety aplikacji](batch-application-packages.md) usługi Batch będzie wykorzystywać połączone konto magazynu do przechowywania i pobierania pakietów aplikacji. Więcej informacji na temat tej funkcji można znaleźć w temacie [Wdrażanie aplikacji za pomocą pakietów aplikacji usługi Azure Batch](batch-application-packages.md).

     > [AZURE.IMPORTANT] Ponowne generowanie kluczy w połączonym koncie magazynu wymaga szczególnej uwagi. Więcej informacji można znaleźć w temacie [Zagadnienia dotyczące kont usługi Batch](#considerations-for-batch-accounts).

4. Kliknij przycisk **Utwórz**, aby utworzyć konto.

  W portalu pojawi się wskazanie, że konto jest **Wdrażane**, a po ukończeniu wdrażania zostanie wyświetlone powiadomienie **Wdrożenia zakończone pomyślnie** w obszarze *Powiadomienia*.

## Wyświetlanie właściwości konta usługi Batch

W bloku konta usługi Batch są wyświetlane różne właściwości konta, a także dodatkowe ustawienia — takie jak klucze dostępu, przydziały, użytkownicy i skojarzenia konta magazynu.

* **Adres URL konta usługi Batch**: ten adres URL umożliwia dostęp do konta usługi Batch przy użyciu interfejsów API (takich jak [Batch REST][api_rest] czy biblioteka klienta [Batch .NET][api_net]) i działa zgodnie z następującym formatem:

    `https://<account_name>.<region>.batch.azure.com`

* **Klucze dostępu**: w celu wyświetlenia kluczy dostępu konta usługi Batch i zarządzania nimi kliknij ikonę klucza, aby otworzyć blok **Zarządzaj kluczami**, lub kliknij opcję **Wszystkie ustawienia** > **Klucze**. Klucz dostępu jest wymagany podczas komunikacji z interfejsami API usługi Batch, takimi jak [Batch REST][api_rest] lub biblioteka klienta [Batch .NET][api_net].

    ![Klucze konta usługi Batch][account_keys]

* **Wszystkie ustawienia**: w celu zarządzania wszystkimi ustawieniami konta usługi Batch lub wyświetlenia jego właściwości, kliknij przycisk **Wszystkie ustawienia**, aby otworzyć blok **Ustawienia**. Ten blok zapewnia dostęp do wszystkich ustawień i właściwości konta, w tym do wyświetlania przydziałów konta, wybierania konta usługi Azure Storage, które ma zostać powiązane z kontem usługi Batch oraz do zarządzania użytkownikami.

    ![Bloki ustawień i właściwości konta usługi Batch][5]

## Uwagi dotyczące kont usługi Batch

* Konta usługi Batch można również tworzyć i zarządzać nimi za pomocą [poleceń cmdlet programu PowerShell usługi Batch](batch-powershell-cmdlets-get-started.md) i biblioteki [Batch Management .NET](batch-management-dotnet.md).

* Za samo konto usługi Batch nie są naliczane opłaty. Opłaty są naliczane za wszystkie zasoby obliczeniowe platformy Azure używane przez rozwiązania usługi Batch oraz za zasoby używane przez inne usługi podczas działania Twoich obciążeń. Na przykład opłaty są naliczane za węzły obliczeniowe w Twoich pulach, a gdy używasz funkcji [pakietów aplikacji](batch-application-packages.md), opłaty są naliczane za zasoby usługi Azure Storage wykorzystywane do przechowywania wersji pakietu aplikacji. Więcej informacji można znaleźć w temacie [Ceny usługi Batch][batch_pricing].

* Wiele obciążeń usługi Batch można uruchomić na jednym koncie usługi Batch lub rozdzielić obciążenia pomiędzy konta tej usługi w różnych regionach usługi Azure.

* W przypadku używania kilku dużych obciążeń usługi Batch należy pamiętać o [Przydziałach i limitach usługi Batch](batch-quota-limit.md), które odnoszą się do subskrypcji platformy Azure i każdego konta usługi Batch. Bieżące przydziały na koncie usługi Batch są wyświetlane w portalu we właściwościach konta.

* W przypadku skojarzenia (link) konta magazynu z kontem usługi Batch należy szczególnie uważać podczas ponownego generowania kluczy dostępu do konta magazynu. Należy ponownie wygenerować tylko jeden klucz konta magazynu, kliknąć opcję **Klucze synchronizacji** w bloku konta połączonego magazynu, poczekać 5 minut, aby umożliwić propagację kluczy do węzłów obliczeniowych w pulach użytkownika, a następnie w razie potrzeby ponownie wygenerować i zsynchronizować inny klucz. Jeśli w tym samym czasie zostaną ponownie wygenerowane oba klucze, węzły obliczeniowe nie będą mogły zsynchronizować żadnego z nich i w ten sposób utracą dostęp do konta magazynu.

  ![Ponowne generowanie kluczy konta magazynu][4]

> [AZURE.IMPORTANT] Usługa Batch obsługuje obecnie *tylko* typ konta magazynu **Ogólnego przeznaczenia**, zgodnie z opisem w kroku 5 [Tworzenie konta magazynu](../storage/storage-create-storage-account.md#create-a-storage-account) w temacie [Informacje o kontach magazynu Azure](../storage/storage-create-storage-account.md). Gdy łączysz konta usługi Azure Storage ze swoim kontem usługi Batch, połącz *tylko* konto magazynu **Ogólnego przeznaczenia**.

## Następne kroki

* Aby dowiedzieć się więcej o zasadach działania i funkcjach usługi Batch, zobacz temat [Omówienie funkcji usługi Azure Batch](batch-api-basics.md). W artykule omówiono podstawowe zasoby usługi Batch, takie jak pule, węzły obliczeniowe i zadania, oraz opisano funkcje, które umożliwiają wykonywanie obciążeń zasobów obliczeniowych na dużą skalę.

* Poznaj podstawy tworzenia aplikacji wykorzystujących usługę Batch za pomocą biblioteki klienta [Batch .NET](batch-dotnet-get-started.md). [Artykuł wprowadzający](batch-dotnet-get-started.md) zawiera omówienie działającej aplikacji, która korzysta z usługi Batch do wykonywania obciążenia na wielu węzłach obliczeniowych, i opisuje zastosowanie usługi Azure Storage do tymczasowego przechowywania i pobierania pliku obciążenia.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Ponowne generowanie kluczy konta magazynu"
[5]: ./media/batch-account-create-portal/batch_acct_05.png "Bloki ustawień i właściwości konta usługi Batch"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG



<!--HONumber=jun16_HO2-->


