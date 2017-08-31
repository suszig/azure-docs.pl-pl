---
title: "Uruchamianie dowolnej aplikacji systemu Windows na dowolnym urządzeniu za pomocą usługi Azure RemoteApp | Microsoft Docs"
description: "Dowiedz się, jak udostępniać użytkownikom dowolną aplikację systemu Windows przy użyciu usługi Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: 961d40ca-9673-4977-aa54-d6b22fc61ce1
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 348e154f8398b5e60ad5a698cd319fa381de2fa5
ms.contentlocale: pl-pl
ms.lasthandoff: 08/21/2017

---
# <a name="run-any-windows-app-on-any-device-with-azure-remoteapp"></a>Uruchamianie dowolnej aplikacji systemu Windows na dowolnym urządzeniu za pomocą usługi Azure RemoteApp
> [!IMPORTANT]
> Usługa Azure RemoteApp nie będzie obsługiwana od 31 sierpnia 2017 r. Szczegółowe informacje zawiera [powiadomienie](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Możesz uruchomić aplikację systemu Windows w dowolnym miejscu na dowolnym urządzeniu w tej chwili, korzystając z usługi Azure RemoteApp. Niezależnie od tego, czy chodzi o niestandardową aplikację napisaną 10 lat temu, czy też aplikację pakietu Office, użytkownicy nie są już ograniczeni do określonego systemu operacyjnego (na przykład Windows XP).

Dzięki usłudze Azure RemoteApp użytkownicy mogą również używać własnych urządzeń z systemem Android lub urządzeń Apple, korzystając z tego samego sposobu obsługi, jaki zapewnia system Windows (lub Windows Phone). Jest to możliwe dzięki temu, że aplikacje systemu Windows są hostowane w kolekcji maszyn wirtualnych z systemem Windows na platformie Azure — użytkownicy mogą uzyskiwać do nich dostęp z dowolnego miejsca, jeśli mają połączenie z Internetem. 

W dalszej części przedstawiony jest przykład wyjaśniający, jak to robić.

W tym artykule udostępnimy bazę danych Access wszystkim naszym użytkownikom. Można jednak użyć do tego celu dowolnej aplikacji. Jeśli aplikację można zainstalować na komputerze z systemem Windows Server 2012 R2, to można ją udostępnić, wykonując poniższe kroki. Możesz przejrzeć [wymagania dotyczące aplikacji](remoteapp-appreqs.md), aby się upewnić, że aplikacja będzie działać.

Należy pamiętać, że program Access jest bazą danych, która powinna być użyteczna, dlatego wykonamy kilka dodatkowych czynności, aby umożliwić użytkownikom dostęp do udziału danych programu Access. Jeśli używana aplikacja nie jest bazą danych lub nie ma potrzeby, aby użytkownicy mieli dostęp do udziału plików, można pominąć te kroki samouczka.

> [!NOTE]
> <a name="note"></a>Do ukończenia tego samouczka jest potrzebne konto platformy Azure.
> 
> * Możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) — otrzymasz kredyt, który można wykorzystać do wypróbowania płatnych usług Azure. Nawet po wyczerpaniu kredytu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure, takich jak Websites. Karta kredytowa nie zostanie obciążona, chyba że jawnie zmienisz ustawienia i poprosisz o jej obciążenie.
> * Możesz [aktywować korzyści subskrybenta MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) — w ramach subskrypcji MSDN co miesiąc otrzymasz kredyt, który można przeznaczyć na płatne usługi platformy Azure.
> 
> 

## <a name="create-a-collection-in-remoteapp"></a>Tworzenie kolekcji w usłudze RemoteApp
Rozpocznij od utworzenia kolekcji. Kolekcja służy jako kontener dla aplikacji i użytkowników. Każdy kolekcja jest oparta na obrazie — można utworzyć własny obraz lub użyć obrazu dostarczonego w ramach subskrypcji. W tym samouczku jest używany obraz wersji próbnej pakietu Office 2013 zawierający aplikację, która ma być udostępniona.

1. W portalu Azure przewiń w dół listę w drzewie nawigacji po lewej stronie, do momentu ukazania się pozycji RemoteApp. Otwórz tę stronę.
2. Kliknij przycisk **Create a RemoteApp collection** (Utwórz kolekcję RemoteApp).
3. Kliknij pozycję **Szybkie tworzenie** i wprowadź nazwę kolekcji.
4. Wybierz region, którego chcesz użyć do utworzenia kolekcji. Aby uzyskać najlepsze wyniki, wybierz region najbliższy geograficznie lokalizacji, w której użytkownicy będą uzyskiwać dostęp do aplikacji. Na przykład w tym samouczku użytkownicy będą znajdować się w Redmond w stanie Waszyngton. Najbliższy region platformy Azure to **Zachodnie stany USA**.
5. Wybierz plan rozliczeniowy, którego chcesz użyć. Podstawowy plan rozliczeniowy przewiduje umieszczenie 16 użytkowników na dużej maszynie wirtualnej platformy Azure, natomiast standardowy plan rozliczeniowy obejmuje umieszczenie 10 użytkowników na dużej maszynie wirtualnej platformy Azure. Generalnie plan podstawowy świetnie sprawdza się w przypadku przepływu prac obejmującego wprowadzanie danych. W przypadku aplikacji biurowych, takich jak pakiet Office, odpowiedni będzie plan standardowy.
6. Następnie wybierz obraz pakietu Office 2013 Professional. Ten obraz zawiera aplikacje pakietu Office 2013. Dla przypomnienia — ten obraz nadaje się tylko do wersji próbnych kolekcji i weryfikacji koncepcji. Nie można używać tego obrazu w kolekcji przeznaczonej dla systemów produkcyjnych.
7. Teraz kliknij pozycję **Create a RemoteApp collection** (Utwórz kolekcję RemoteApp).

![Tworzenie kolekcji w chmurze w usłudze RemoteApp](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

Spowoduje to uruchomienie tworzenia kolekcji, co może potrwać do godziny.

Teraz możesz dodać użytkowników.

## <a name="share-the-app-with-users"></a>Udostępnianie aplikacji użytkownikom
Po pomyślnym utworzeniu kolekcji można opublikować bazę danych Access dla użytkowników i dodać tych użytkowników, którzy powinni mieć do niej dostęp.

Jeśli w czasie tworzenia kolekcji nastąpiło przejście do węzła innego niż Azure RemoteApp, rozpocznij od powrotu do tego węzła ze strony głównej systemu Azure.

1. Kliknij utworzoną wcześniej kolekcję, aby uzyskać dostęp do dodatkowych opcji i skonfigurować kolekcję.
   ![Nowa kolekcja usługi RemoteApp w chmurze](./media/remoteapp-anyapp/ra-anyappcollection.png)
2. Na karcie **Publishing** (Publikowanie) kliknij przycisk **Publish** (Publikuj) u dołu ekranu, a następnie kliknij przycisk **Publish Start menu programs** (Publikuj programy menu Start).
   ![Publikowanie programu usługi RemoteApp](./media/remoteapp-anyapp/ra-anyapppublish.png)
3. Wybierz z listy aplikacje, które mają zostać opublikowane. W tym przypadku wybierz program Access. Kliknij przycisk **Complete** (Zakończ). Poczekaj na zakończenie publikowania aplikacji.
   ![Publikowanie programu Access w usłudze RemoteApp](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)
4. Po zakończeniu publikowania aplikacji przejdź do karty **User Access** (Dostęp użytkowników), aby dodać wszystkich użytkowników, którzy muszą mieć dostęp do aplikacji. Wprowadź odpowiednie nazwy użytkowników (adresy e-mail), a następnie kliknij przycisk **Save** (Zapisz).

![Dodawanie użytkowników do usługi RemoteApp](./media/remoteapp-anyapp/ra-anyappaddusers.png)

1. Nadszedł czas na poinformowanie użytkowników o nowych aplikacjach i sposobie uzyskania do nich dostępu. W tym celu należy wysłać użytkownikom wiadomość e-mail ze wskazaniem adresu URL pobierania klienta pulpitu zdalnego.
   ![Adres URL pobierania klienta usługi RemoteApp](./media/remoteapp-anyapp/ra-anyappurl.png)

## <a name="configure-access-to-access"></a>Konfigurowanie dostępu do programu Access
Niektóre aplikacje wymagają dodatkowej konfiguracji po wdrożeniu za pośrednictwem usługi RemoteApp. W przypadku programu Access zamierzamy w szczególności utworzyć udział plików na platformie Azure, do którego dostęp może uzyskać każdy użytkownik. [Jeśli nie chcesz tego robić, możesz utworzyć [kolekcję hybrydową](remoteapp-create-hybrid-deployment.md) (zamiast kolekcji w chmurze), która umożliwia użytkownikom dostęp do plików i informacji w sieci lokalnej]. Następnie musisz poprosić użytkowników o wykonanie mapowania dysku lokalnego na swoich komputerach do systemu plików Azure.

Pierwsza część musi zostać wykonana przez Ciebie jako administratora. Następnie niektóre kroki zostaną wykonane przez użytkowników.

1. Rozpocznij od opublikowania interfejsu wiersza polecenia (cmd.exe). Na karcie **Publishing** (Publikowanie) wybierz pozycję **cmd**, a następnie kliknij opcje **Publish > Publish program using path** (Publikuj > Publikuj program przy użyciu ścieżki).
2. Wprowadź nazwę i ścieżkę aplikacji. W tym przypadku należy użyć nazwy „File Explorer” i ścieżki „% SYSTEMDRIVE%\windows\explorer.exe”.
   ![Opublikuj plik cmd.exe.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. Teraz należy utworzyć [konto usługi Azure Storage](../storage/common/storage-create-storage-account.md). Nasze konto nazywa się „accessstorage” (magazyn programu Access) — należy wybrać nazwę znaczącą dla użytkownika. (Parafrazując bohatera filmu „Nieśmiertelny”, może istnieć tylko jedno konto „accessstorage”). ![Nasze konto usługi Azure Storage](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Teraz wróć do pulpitu nawigacyjnego, aby określić ścieżkę do magazynu (lokalizacji punktu końcowego). Będzie za chwilę potrzebna, dlatego skopiuj ją w poręczne miejsce.
   ![Ścieżka konta magazynu](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. Po utworzeniu konta magazynu należy uzyskać podstawowy klucz dostępu. Kliknij pozycję **Manage access keys** (Zarządzaj kluczami dostępu), a następnie skopiuj podstawowy klucz dostępu.
6. Teraz ustaw kontekst konta magazynu i utwórz nowy udział plików dla programu Access. Uruchom następujące polecenia cmdlet w oknie programu Windows PowerShell z podwyższonym poziomem uprawnień:
   
        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx
   
    W przypadku naszego udziału uruchamiamy następujące polecenia cmdlet:
   
        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx

Nadeszła kolej na użytkownika. Najpierw użytkownicy powinni zainstalować [klienta usługi RemoteApp](remoteapp-clients.md). Następnie użytkownicy muszą wykonać mapowanie dysku ze swojego konta na utworzony udział plików Azure i dodać swoje pliki programu Access. Należy to wykonać w następujący sposób:

1. W kliencie usługi RemoteApp przejdź do opublikowanych aplikacji. Uruchom program cmd.exe.
2. Uruchom następujące polecenie, aby mapować dysk swojego komputera na udział plików:
   
        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>
   
    Ustawienie parametru **/persistent** na wartość „tak” spowoduje, że mapowanie dysku będzie zachowywane między sesjami.
3. Następnie uruchom Eksploratora plików w usłudze RemoteApp. Skopiuj do udziału plików wszystkie pliki programu Access, które mają być używane w udostępnionej aplikacji.
   ![Umieszczanie plików programu Access w udziale na platformie Azure](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
4. Na koniec otwórz program Access, a następnie otwórz udostępnioną bazę danych. Powinny być widoczne dane w programie Access działającym w chmurze.
   ![Rzeczywista baza danych programu Access uruchomiona w chmurze](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Teraz można używać programu Access na dowolnych urządzeniach — wystarczy zainstalować na nich klienta usługi RemoteApp.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki
Po opanowaniu tworzenia kolekcji spróbuj utworzyć [kolekcję korzystającą z usługi Office 365](remoteapp-tutorial-o365anywhere.md). Możesz też utworzyć [kolekcję hybrydową](remoteapp-create-hybrid-deployment.md) z dostępem do sieci lokalnej.

<!--Image references-->


