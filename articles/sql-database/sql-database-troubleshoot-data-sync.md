---
title: "Rozwiązywanie problemów z synchronizacji danych Azure SQL | Dokumentacja firmy Microsoft"
description: "Sposoby rozwiązywania typowych problemów z synchronizacją danych SQL Azure"
services: sql-database
ms.date: 11/2/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 29fb935ada4aa7f2571b128f82d4c037bbb88eb1
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="troubleshoot-issues-with-azure-sql-data-sync-preview"></a>Rozwiązywanie problemów z synchronizacją danych SQL Azure (wersja zapoznawcza)

W tym artykule opisano sposób rozwiązywania bieżących problemach, które są znane do zespołu synchronizacji danych SQL (wersja zapoznawcza). W przypadku rozwiązania problemu, jest podana w tym miejscu.

Omówienie synchronizacji danych SQL, zobacz [synchronizacji danych między wieloma bazami danych chmury i lokalnych z synchronizacji danych SQL Azure (wersja zapoznawcza)](sql-database-sync-data.md).
                                                           
## <a name="my-client-agent-doesnt-work"></a>Moje agenta klienta nie działa

### <a name="description-and-symptoms"></a>Opis i objawy

Wystąpiły następujące komunikaty o błędach podczas próby użycia agenta klienta.

"Synchronizacja nie powiodła się z powodu wyjątku, który wystąpił błąd podczas próby zdeserializowania parametru www.microsoft.com/.../05:GetBatchInfoResult. Aby uzyskać więcej informacji, zobacz InnerException.

"Komunikat wyjątku wewnętrznego: typ"Microsoft.Synchronization.ChangeBatch"jest nieprawidłowym typem kolekcji, ponieważ nie ma domyślnego konstruktora."


### <a name="cause"></a>Przyczyna

Ten błąd jest problem z synchronizacją danych SQL (wersja zapoznawcza).

Najbardziej prawdopodobną przyczyną tego problemu jest:

-   Systemem Windows 8 Developer Preview, lub

-   Masz .NET 4.5 zainstalowane.

### <a name="solution-or-workaround"></a>Rozwiązania lub obejścia

Upewnij się, zainstaluj agenta klienta na komputerze, który nie jest uruchomiony system Windows 8 Developer Preview, a nie zainstalowano programu .NET Framework 4.5.

## <a name="my-client-agent-doesnt-work-after-i-cancel-the-uninstall"></a>Moje agenta klienta nie działa po anulować dezinstalacji

### <a name="description-and-symptoms"></a>Opis i objawy

Agenta klienta nie działa, nawet jeśli anulowano jego dezinstalacji.

### <a name="cause"></a>Przyczyna

Ten problem występuje, ponieważ agent klienta synchronizacji danych SQL (wersja zapoznawcza) nie przechowuje poświadczeń.

### <a name="solution-or-workaround"></a>Rozwiązania lub obejścia

Istnieją dwa rozwiązań:

-   Najpierw użyj services.msc, aby ponownie wprowadzić swoje poświadczenia dla agenta klienta.

-   Po drugie Odinstaluj agenta klienta i zainstalowanie nowej. Pobierz i zainstaluj najnowszą wersję agenta klienta z [Centrum pobierania](http://go.microsoft.com/fwlink/?linkid=221479).

## <a name="my-database-isnt-listed-in-the-agent-dropdown"></a>Bazy danych nie jest na liście rozwijanej agenta

### <a name="description-and-symptoms"></a>Opis i objawy

Podczas próby dodania istniejącej bazy danych programu SQL Server do grupy synchronizacji bazy danych nie jest wyświetlane na liście rozwijanej.

### <a name="cause"></a>Przyczyna

Istnieje kilka możliwych przyczyn tego problemu:

-   Grupy klientów i agent synchronizacji znajdują się w różnych danych centrów.

-   Agent klienta listy baz danych jest nieaktualna.

### <a name="solution"></a>Rozwiązanie

Rozwiązania zależy od przyczyny.

#### <a name="the-client-agent-and-sync-group-are-in-different-data-centers"></a>Grupy klientów i agent synchronizacji znajdują się w różnych danych centrów

W tym samym centrum danych musi mieć zarówno agenta klienta, jak i grupy synchronizacji. Można skonfigurować tę konfigurację, wykonując jedną z następujących czynności:

-   Utwórz nowy agent, w tym samym centrum danych jako grupy synchronizacji. Następnie przeprowadź rejestrację bazy danych z tego agenta. Zobacz [jak: Instalowanie agenta klienta synchronizacji danych SQL (wersja zapoznawcza)](#install-a-sql-data-sync-client-agent) Aby uzyskać więcej informacji.

-   Usuń bieżącej grupy synchronizacji. Następnie utwórz ją ponownie w tym samym centrum danych na agenta.

#### <a name="the-client-agents-list-of-databases-is-not-current"></a>Agent klienta listy baz danych nie jest aktualny

Zatrzymaj i ponownie uruchom usługę agenta klienta.
Agent lokalny pobiera listę skojarzonych bazach danych tylko na pierwszym przesłanie klucza agenta, a nie na przesyłanie kluczowych kolejnych agentów. W związku z tym podczas przenoszenia agenta zarejestrowane bazy danych nie są wyświetlane w oryginalnym wystąpieniu agenta.

## <a name="client-agent-doesnt-start-error-1069"></a>Agent klienta nie uruchamia się (błąd 1069)

### <a name="description-and-symptoms"></a>Opis i objawy

Wykryj, czy agent nie jest uruchomiony na komputerze, na którym program SQL Server. Podczas próby uruchomienia agenta ręcznie, możesz pobrać okna dialogowego błędu z komunikatem o błędzie "Błąd 1069: nie można uruchomić usługi z powodu niepowodzenia logowania."

![Okno dialogowe błędu 1069 synchronizacji danych](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

### <a name="cause"></a>Przyczyna

Prawdopodobną przyczyną tego błędu jest zmiana hasła na serwerze lokalnym, ponieważ utworzono agenta i nadać mu hasło logowania.

### <a name="solution-or-workaround"></a>Rozwiązania lub obejścia

Zaktualizuj hasło agenta do bieżące hasło serwera.

1. Znajdź agenta klienta synchronizacji danych SQL (wersja zapoznawcza) w wersji zapoznawczej usługi.

    a. Kliknij przycisk **Start**.

    b. W polu wyszukiwania wpisz "services.msc".

    c. W wynikach wyszukiwania kliknij pozycję "Usługi".

    d. W **usług** okna, przewiń do wpisu dla **Agent synchronizacji danych SQL (wersja zapoznawcza) w wersji zapoznawczej**.

2. Kliknij prawym przyciskiem myszy wpis, a następnie wybierz **zatrzymać**.

3. Kliknij prawym przyciskiem myszy wpis, a następnie kliknij przycisk **właściwości**.

4. W **synchronizacji danych SQL (wersja zapoznawcza) Agent Podgląd właściwości** okna, kliknij przycisk **Zaloguj** kartę.

5. Wprowadź hasło w polu tekstowym hasło.

6. Potwierdź hasło w polu tekstowym Potwierdź hasło.

7. Kliknij przycisk **Apply** (Zastosuj), a następnie kliknij przycisk **OK**.

8. W **usług** okna, kliknij prawym przyciskiem myszy **Agent synchronizacji danych SQL (wersja zapoznawcza) w wersji zapoznawczej** usługi, a następnie kliknij przycisk **Start**.

9. Zamknij **usług** okna.

## <a name="i-get-a-disk-out-of-space-message"></a>Wyświetlany jest komunikat "dysk brak miejsca"

### <a name="cause"></a>Przyczyna

Komunikat "dysk mało miejsca na" może wystąpić, gdy pliki, które powinny być usuwane pozostać poza. Ten stan może wystąpić z powodu oprogramowanie antywirusowe, lub ponieważ pliki są Otwórz próba operacji usuwania.

### <a name="solution"></a>Rozwiązanie

Rozwiązanie to ręcznie usunąć pliki synchronizacji, w obszarze `%temp%` (`del \*sync\* /s`), a następnie usuń podkatalogi również.

> [!IMPORTANT]
> Poczekaj, aż synchronizacja zakończyła się przed usunięciem żadnych plików.

## <a name="i-cannot-delete-my-sync-group"></a>Nie można usunąć mojej grupy synchronizacji

### <a name="description-and-symptoms"></a>Opis i objawy

Możesz zakończyć się niepowodzeniem w przypadku próby usunięcia grupy synchronizacji.

### <a name="causes"></a>Powoduje, że

Jedną z następujących czynności może spowodować awarię, aby usunąć grupę synchronizacji.

-   Agent klienta jest w trybie offline.

-   Agent klienta jest niezainstalowana lub ich brak. 

-   Baza danych jest w trybie offline. 

-   Grupy synchronizacji jest inicjowania obsługi administracyjnej lub synchronizacji. 

### <a name="solutions"></a>Rozwiązania

W celu rozwiązania nie powiodło się usunięcie grupy synchronizacji, sprawdź następujące elementy:

-   Pamiętaj, że agent klienta jest w trybie online, a następnie spróbuj ponownie.

-   Jeśli agent klienta jest niezainstalowana lub w inny sposób, Brak:

    a. Jeśli plik istnieje, usuń plik XML agenta z folderu instalacji synchronizacji danych SQL (wersja zapoznawcza).

    b. Zainstaluj agenta na tym samym innego na komputerze lokalnym, przesłać klucz agenta z portalu, który został wygenerowany dla agenta, który jest wyświetlany w trybie offline.

-   **Usługa synchronizacji danych SQL (wersja zapoznawcza) zostanie zatrzymana.**

    a. W **Start** menu, wyszukiwanie usług.

    b. W wynikach wyszukiwania kliknij opcję usługi.

    c. Znajdź **synchronizacji danych SQL (wersja zapoznawcza) w wersji zapoznawczej** usługi.

    d. Jeśli stan usługi jest **zatrzymane**, kliknij prawym przyciskiem myszy nazwę usługi i wybierz **Start** z menu rozwijanego.

-   Sprawdź baz danych bazy danych SQL i programu SQL Server należy upewnić się, że są one wszystkich online.

-   Poczekaj na zakończenie procesu inicjowania obsługi administracyjnej lub synchronizacji. Następnie ponów usunięcie grupy synchronizacji.

## <a name="sync-fails-in-the-portal-ui-for-on-premises-databases-associated-with-the-client-agent"></a>Niepowodzenie synchronizacji w portalu interfejsu użytkownika dla lokalnych baz danych skojarzonych z agenta klienta

### <a name="description-and-symptoms"></a>Opis i objawy

Synchronizacji kończy się niepowodzeniem w portalu synchronizacji danych SQL (wersja zapoznawcza) interfejsu użytkownika dla lokalnych baz danych skojarzonych z agentem. Na komputerze lokalnym, w którym jest uruchomiony agent zobacz temat System.IO.IOException błędy w dzienniku zdarzeń, informujący, że dysk jest za mało miejsca.

### <a name="solution-or-workaround"></a>Rozwiązania lub obejścia

Utwórz więcej miejsca na dysku, na którym znajduje się katalogu % TEMP %.

## <a name="i-cant-unregister-an-on-premises-sql-server-database"></a>I nie można wyrejestrować z lokalną bazą danych programu SQL Server

### <a name="cause"></a>Przyczyna

Prawdopodobnie chcesz wyrejestrować bazy danych, która została już usunięta.

### <a name="solution-or-workaround"></a>Rozwiązania lub obejścia

Aby wyrejestrować lokalną bazą danych programu SQL Server, wybierz bazę danych, a następnie kliknij przycisk **Wymuś usunięcie**.

Jeśli ta operacja nie powiedzie się usunąć bazę danych z grupy synchronizacji, należy wykonać następujące czynności:

1. Zatrzymaj i ponownie uruchom usługę hosta agenta klienta.

    a. Kliknij Start menu.

    b. Wprowadź *services.msc* w polu wyszukiwania.

    c. W programach sekcji w okienku wyników kliknij dwukrotnie **usług**.

    d. Znajdź i kliknij prawym przyciskiem myszy usługę **synchronizacji danych SQL (wersja zapoznawcza)**.

    e. Jeśli usługa jest uruchomiona, zatrzymaj ją.

    f. Kliknij prawym przyciskiem myszy i wybierz **Start**.

    g. Sprawdź, czy baza danych jest już zarejestrowana. Jeśli nie jest już zarejestrowana, wszystko będzie gotowe. W przeciwnym razie przejdź do następnego kroku.

2. Otwórz aplikację agenta klienta (SqlAzureDataSyncAgent).

3. Kliknij przycisk **edytowanie poświadczeń** i podaj poświadczenia dla bazy danych, dlatego jest dostępny.

4. Przejdź do wyrejestrowania.

## <a name="i-cannot-submit-the-agent-key"></a>Nie można przesłać klucz agenta

### <a name="description-and-symptoms"></a>Opis i objawy

Po utworzeniu lub ponownie utworzyć klucz dla agenta, spróbuj przesłać ten klucz przy użyciu aplikacji SqlAzureDataSyncAgent, ale przesyłania nie powiedzie się.

![Okno dialogowe błędu synchronizacji — nie można przesłać klucz agenta](media/sql-database-troubleshoot-data-sync/sync-error-cant-submit-agent-key.png)

Przed kontynuowaniem upewnij się, że awarii jednej z następujących warunków nie jest przyczyną problemu.

-   Usługa synchronizacji danych SQL (wersja zapoznawcza) systemu Windows jest uruchomiona.

-   Konto usługi dla usługi systemu Windows w wersji zapoznawczej synchronizacji danych SQL (wersja zapoznawcza) ma dostęp do sieci.

-   Agent klienta jest w stanie nawiązać kontaktu z usługą lokalizatora. Sprawdź, czy następujący klucz rejestru ma wartość "https://locator.sync.azure.com/LocatorServiceApi.svc"

    -   Na x86 komputera:`HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\SQL Azure Data Sync\\LOCATORSVCURI`

    -   Na x64 komputera:`HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\SQL Azure Data Sync\\LOCATORSVCURI`

### <a name="cause"></a>Przyczyna

Klucz agenta unikatowo identyfikuje każdy agent lokalny. Klucz musi spełniać dwa warunki dla tej funkcji:

-   Klucz agenta klienta na serwerze synchronizacji danych SQL (wersja zapoznawcza) i komputer lokalny musi być identyczne.

-   Klucz agenta klienta mogą być użyte tylko raz.

### <a name="solution-or-workaround"></a>Rozwiązania lub obejścia

Jeśli Twoje agent nie działa, jest on, ponieważ jeden lub oba te warunki nie są spełnione. Aby uzyskać agenta do pracy ponownie:

1. Wygenerowanie nowego klucza.

2. Zastosowanie nowego klucza do agenta.

Aby zastosować nowego klucza do agenta, wykonaj następujące czynności:

1. Przejdź do katalogu instalacji agenta za pomocą Eksploratora plików. Domyślny katalog instalacyjny to `c:\\program files (x86)\\microsoft sql data sync`.

2. Kliknij dwukrotnie `bin` podkatalogu.

3. Uruchamianie `SqlAzureDataSyncAgent` aplikacji.

4. Kliknij przycisk **przesłać klucz agenta**.

5. Wklej klucz ze Schowka w odpowiednim miejscu.

6. Kliknij przycisk **OK**.

7. Zamknij program.

## <a name="i-do-not-have-sufficient-privileges-to-start-system-services"></a>Ma wystarczające uprawnienia do uruchamiania usług systemowych

### <a name="cause"></a>Przyczyna

Ten błąd występuje w dwóch sytuacjach:

-   Nazwa użytkownika i/lub hasło są niepoprawne.

-   Określone konto użytkownika nie ma wystarczających uprawnień do logowania się jako usługa.

### <a name="solution-or-workaround"></a>Rozwiązania lub obejścia

Przyznanie dziennika na — jako — usługa poświadczeń dla konta użytkownika.

1. Przejdź do **Start | Panel sterowania | Narzędzia administracyjne |  Zasady zabezpieczeń lokalnych | Lokalne zasady | Zarządzanie prawami użytkownika**.

2. Znajdź i kliknij **Zaloguj jako usługa** wpisu.

3. Dodaj konto użytkownika w **Zaloguj jako usługa właściwości** okna dialogowego.

4. Kliknij przycisk **zastosować** następnie **OK**.

5. Zamknij okna.

## <a name="local-sync-agent-app-is-unable-to-connect-to-the-local-sync-service"></a>Nie można nawiązać połączenia z usługą synchronizacji lokalnej jest lokalny Agent synchronizacji aplikacji

### <a name="solution-or-workaround"></a>Rozwiązania lub obejścia

Spróbuj wykonać następujące kroki:

1. Zakończyć działanie aplikacji.

2. Otwórz Panel usług składowych.

    a. W polu wyszukiwania na pasku zadań wpisz "services.msc."

    b. W wynikach wyszukiwania, kliknij dwukrotnie ikonę "Usługi".

3. Zatrzymaj i ponownie uruchom usługę "(wersja zapoznawcza) synchronizacji danych SQL w wersji zapoznawczej".

4. Uruchom ponownie aplikację.

## <a name="install-uninstall-or-repair-fails"></a>Zainstalować, odinstalować lub napraw kończy się niepowodzeniem

### <a name="cause"></a>Przyczyna

Istnieje wiele możliwych przyczyn niepowodzenia. Aby ustalić przyczynę określonego dla tego błędu, należy znaleźć w dziennikach.

### <a name="solution-or-workaround"></a>Rozwiązania lub obejścia

Aby znaleźć określone przyczynę błędu, który wystąpił, należy wygenerować i znaleźć w dziennikach Instalatora Windows. Można włączyć rejestrowanie w wierszu polecenia. Na przykład przypuśćmy, że pobrany plik AgentServiceSetup.msi jest LocalAgentHost.msi. Generowanie i sprawdź, czy pliki dziennika przy użyciu następujących wierszy polecenia:

-   Do instalacji:`msiexec.exe /i SQLDataSyncAgent-Preview-ENU.msi /l\*v LocalAgentSetup.InstallLog`

-   Aby uzyskać odinstalowuje:`msiexec.exe /x SQLDataSyncAgent-se-ENU.msi /l\*v LocalAgentSetup.InstallLog`

Można również włączyć rejestrowanie dla wszystkich instalacji wykonywane przez Instalatora Windows. W artykule bazy wiedzy Microsoft Knowledge Base [włączania rejestrowania Instalatora Windows](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) zapewnia rozwiązanie jednego kliknięcia, aby włączyć rejestrowanie dla Instalatora Windows. Umożliwia także lokalizacji tych dzienników.

## <a name="a-database-has-an-out-of-date-status"></a>Bazy danych ma stan "Przestarzałe"

### <a name="cause"></a>Przyczyna

Synchronizacja danych SQL (wersja zapoznawcza) usuwa baz danych, które są w trybie offline dla 45 dni lub więcej (zliczane od momentu przejścia w tryb offline bazy danych) z usługi. Jeśli bazy danych jest w trybie offline, przez co najmniej 45 dni, a następnie wraca do trybu online, jego stan jest ustawiony na "Przestarzałe".

### <a name="solution-or-workaround"></a>Rozwiązania lub obejścia

Stan "Przestarzałe" można uniknąć, upewniając się, że żadna z baz danych przejdź wyłączony 45 dni lub więcej.

Jeśli stan bazy danych "Przestarzałe", należy wykonać następujące czynności:

1. Usuń "Przestarzałe" bazy danych z grupy synchronizacji.

2. Dodawanie bazy danych ponownie do grupy synchronizacji.

> [!WARNING]
> Możesz utracić wszystkie zmiany wprowadzone do tej bazy danych podczas w trybie offline.

## <a name="a-sync-group-has-an-out-of-date-status"></a>Grupy synchronizacji ma stan "Przestarzałe"

### <a name="cause"></a>Przyczyna

W przypadku awarii jednego lub kilku zmian do zastosowania w okresie przechowywania całej 45 dni, grupy synchronizacji może stać się nieaktualne.

### <a name="solution-or-workaround"></a>Rozwiązania lub obejścia

Aby uniknąć stan "Przestarzałe", należy przejrzeć wyniki zadań synchronizacji w przeglądarce historii, w sposób regularny i zbadać i rozwiązać wszelkie zmiany, których nie udało się zastosować.

Jeśli jest w stanie grupy synchronizacji "Przestarzałe", należy usunąć grupę synchronizacji i utwórz ją ponownie.

## <a name="i-see-erroneous-data-in-my-tables"></a>Błędne dane są widoczne w tabeli

### <a name="description-and-symptoms"></a>Opis i objawy

Jeśli tabel o tej samej nazwie, ale z inną schematów w bazie danych są zaangażowane w synchronizacji, zobaczysz błędnych danych w tych tabelach po zakończeniu synchronizacji.

### <a name="cause-and-fix"></a>Poprawka i przyczyny

Synchronizacja danych SQL (wersja zapoznawcza) procesie inicjowania obsługi używa tego samego tabel śledzenia dla tabel o tej samej nazwie, ale w różnych schematach. W związku z tym zmiany z obu tabel są uwzględniane w tej samej tabeli śledzenia i ten problem powoduje błędne dane zmian podczas synchronizacji.

### <a name="resolution-or-workaround"></a>Rozwiązania lub obejścia

Upewnij się, że nazwy tabel, których dotyczy synchronizacji są różne, nawet jeśli należą do różnych schematach.

## <a name="i-see-inconsistent-primary-key-data-after-a-successful-synchronization"></a>Niespójne dane klucza podstawowego są widoczne po pomyślnej synchronizacji

### <a name="description-and-symptoms"></a>Opis i objawy

Po synchronizacji, który został zgłoszony jako pomyślnie, a dziennik zawiera żadnych wierszy nie powiodło się lub zostało pominięte, można zaobserwować, że danych kluczy podstawowych jest niezgodny między bazami danych w grupie synchronizacji.

### <a name="cause"></a>Przyczyna

To zachowanie jest celowe. Zmiany w dowolnej kolumny klucza podstawowego spowodować niespójność danych w wierszach, w którym klucz podstawowy został zmieniony.

### <a name="resolution-or-workaround"></a>Rozwiązania lub obejścia

Aby uniknąć tego problemu, upewnij się, że żadne dane w kolumnie klucza podstawowego zostanie zmieniona.

Aby rozwiązać ten problem, po zdarzeniu, musisz porzucić wierszu ze wszystkich punktów końcowych w grupie synchronizacji i ponownie włóż wiersza.

## <a name="i-see-a-significant-degradation-in-performance"></a>Widać znacznego pogorszenia wydajności

### <a name="description-and-symptoms"></a>Opis i objawy

Znacznie, prawdopodobnie obniża wydajność do punktu, w których jeszcze nie można uruchomić synchronizacji danych interfejsu użytkownika.

### <a name="cause"></a>Przyczyna

Najbardziej prawdopodobną przyczyną jest pętli synchronizacji. Pętla synchronizacji występuje podczas synchronizacji przez synchronizacji grupy A wyzwalacze a synchronizacji przez synchronizacji, grupa B, który z kolei wyzwala synchronizacji przez synchronizacji grupy A. Faktycznego może być bardziej złożonych, obejmujące więcej niż dwie grupy synchronizacji w pętli, ale jest ważnym czynnikiem jest cykliczne wyzwolenie synchronizacje spowodowane grupy synchronizacji nakładających się wzajemnie.

### <a name="resolution-or-workaround"></a>Rozwiązania lub obejścia

Najlepiej będzie zapobiegania. Upewnij się, że nie masz odwołania cykliczne w Twoich grupach synchronizacji. Nie można zsynchronizować dowolnego wiersza, który jest synchronizowany przez jedną grupę synchronizacji przez inną grupę synchronizacji.

## <a name="client-agent-cannot-be-deleted-from-the-portal-if-its-associated-on-premises-database-is-unreachable"></a>Nie można usunąć agenta klienta z portalu, jeśli jego skojarzony lokalnymi bazy danych jest nieosiągalny

### <a name="description-and-symptoms"></a>Opis i objawy

W przypadku lokalnego punktu końcowego (to znaczy bazy danych), który został zarejestrowany za pomocą agenta klienta synchronizacji danych SQL (wersja zapoznawcza) staje się niedostępny, nie można usunąć agenta klienta.

### <a name="cause"></a>Przyczyna

Nie można usunąć lokalnego agenta, ponieważ jest nieosiągalny bazy danych jest nadal zarejestrowany z agentem. Podczas próby usunięcia agenta, proces usuwania próbuje nawiązać połączenia bazy danych, która kończy się niepowodzeniem.

### <a name="resolution-or-workaround"></a>Rozwiązania lub obejścia

Użyj "Wymuś usunięcie" można usunąć bazy danych jest nieosiągalny.

> [!NOTE]
> Jeśli po tabel metadanych synchronizacji "force delete" pozostają deprovisioningutil.exe Użyj je wyczyścić.

## <a name="a-sync-group-cannot-be-deleted-within-three-minutes-of-uninstallingstopping-the-agent"></a>Nie można usunąć grupy synchronizacji w ciągu trzech minut odinstalowanie/zatrzymywania agenta

### <a name="description-and-symptoms"></a>Opis i objawy

Nie jest możliwe usunąć grupę synchronizacji w ciągu trzech minut odinstalowanie/zatrzymywania skojarzone agenta klienta synchronizacji danych SQL (wersja zapoznawcza).

### <a name="resolution-or-workaround"></a>Rozwiązania lub obejścia

1. Usuwanie grupy synchronizacji podczas synchronizacji skojarzone agentów są w trybie online (zalecane).

2. Jeśli agent jest w trybie offline, ale zainstalowane, przełącz go w trybie online na komputerze lokalnym. Poczekaj, aż stan agenta są wyświetlane jako w trybie online w portalu synchronizacji danych SQL (wersja zapoznawcza) i Usuń grupę synchronizacji.

3. Jeśli agent jest w trybie offline, ponieważ została ona odinstalowana, wykonaj następujące czynności. Następnie ponów usunięcie grupy synchronizacji.

    a.  Jeśli plik istnieje, usuń plik XML agenta z folderu instalacji synchronizacji danych SQL (wersja zapoznawcza).

    b.  Zainstaluj agenta na tym samym lub innym na komputerze lokalnym, Prześlij klucz agenta z portalu, który został wygenerowany dla agenta, który jest wyświetlany w trybie offline.

## <a name="my-sync-group-is-stuck-in-the-processing-state"></a>Moja grupa synchronizacji jest zablokowana w stanie przetwarzania

### <a name="description-and-symptoms"></a>Opis i objawy

Grupy synchronizacji w synchronizacji danych SQL (wersja zapoznawcza) był w stanie przetwarzania przez dłuższy czas, nie odpowiada na polecenia zatrzymania i w dziennikach znajdują się nie nowe wpisy.

### <a name="causes"></a>Powoduje, że

Jeden z następujących warunków może spowodować grupy synchronizacji są zablokowane w stanie przetwarzania.

-   **Agent klienta jest w trybie offline.** Pamiętaj, że agent klienta jest w trybie online, a następnie spróbuj ponownie.

-   **Agent klienta jest niezainstalowana lub ich brak.** Jeśli agent klienta jest niezainstalowana lub w inny sposób, Brak:

    1. Jeśli plik istnieje, usuń plik XML agenta z folderu instalacji synchronizacji danych SQL (wersja zapoznawcza).

    2. Zainstaluj agenta na tym samym innego na komputerze lokalnym. Następnie przesłać klucz agenta z portalu, który został wygenerowany dla agenta, który jest wyświetlany w trybie offline.

-   **Usługa synchronizacji danych SQL (wersja zapoznawcza) zostanie zatrzymana.**

    1. W **Start** menu, wyszukiwanie usług.

    2. W wynikach wyszukiwania kliknij opcję usługi.

    3. Znajdź **synchronizacji danych SQL (wersja zapoznawcza)** usługi.

    4. Jeśli stan usługi jest **zatrzymane**, kliknij prawym przyciskiem myszy nazwę usługi i wybierz **Start** z menu rozwijanego.

### <a name="solution-or-workaround"></a>Rozwiązania lub obejścia

Jeśli nie można rozwiązać ten problem, można zresetować stanu grupy synchronizacji przy pomocy technicznej firmy Microsoft. Aby uzyskać stan resetowania, Utwórz wpis na forum na [forum usługi Azure SQL Database](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted)i zawierać identyfikator subskrypcji i identyfikator grupy synchronizacji dla grupy, która powinna zostać zresetowana. Z pracownikiem pomocy technicznej firmy Microsoft będzie odpowiadać na ogłoszenie i informacją o tym, kiedy stan został zresetowany.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na temat synchronizacji danych SQL zobacz:

-   [Synchronizowanie danych w wielu w chmurze i lokalnych baz danych z synchronizacji danych SQL Azure](sql-database-sync-data.md)
-   [Wprowadzenie do synchronizacji danych Azure SQL](sql-database-get-started-sql-data-sync.md)
-   [Najlepsze rozwiązania dotyczące synchronizacji danych SQL Azure](sql-database-best-practices-data-sync.md)
-   [Monitor synchronizacji danych Azure SQL z OMS analizy dzienników](sql-database-sync-monitor-oms.md)

-   Wykonaj przykłady z programu PowerShell, które przedstawiają sposób konfigurowania synchronizacji danych SQL:
    -   [Synchronizacja między wiele baz danych Azure SQL przy użyciu programu PowerShell](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Synchronizacja między bazą danych SQL Azure i lokalnej bazy danych programu SQL Server przy użyciu programu PowerShell](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Pobrać dokumentację interfejsu API REST synchronizacji danych SQL](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Aby uzyskać więcej informacji na temat bazy danych SQL zobacz:

-   [Omówienie bazy danych SQL](sql-database-technical-overview.md)
-   [Zarządzanie cyklem życia bazy danych](https://msdn.microsoft.com/library/jj907294.aspx)
