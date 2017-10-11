## <a name="download-install-and-register-the-azure-backup-agent"></a>Pobieranie, instalowanie i rejestrowanie agenta usługi Kopia zapasowa Azure
Po utworzeniu magazynu usługi Kopia zapasowa Azure, należy zainstalować agenta na wszystkich maszynach systemu Windows (Windows Server, klienta systemu Windows, serwer System Center Data Protection Manager lub serwer kopii zapasowej Azure machine), które umożliwia wykonywanie kopii zapasowych danych i aplikacji na platformie Azure.

1. Zaloguj się do [portalu zarządzania](https://manage.windowsazure.com/)
2. Kliknij przycisk **usług odzyskiwania**, następnie wybierz magazyn kopii zapasowych, który ma zostać zarejestrowany na serwerze. Zostanie wyświetlona strona Szybki Start dla tego magazynu kopii zapasowych.
   
    ![Szybki start](./media/backup-install-agent/quickstart.png)
3. Na stronie Szybki Start kliknij **klienta dla systemu Windows Server lub System Center Data Protection Manager lub Windows** opcję w obszarze **Pobierz agenta**. Kliknij przycisk **zapisać** Aby skopiować go na komputerze lokalnym.
   
    ![Zapisz agenta](./media/backup-install-agent/agent.png)
4. Po zainstalowaniu agenta kliknij dwukrotnie MARSAgentInstaller.exe do uruchomienia instalacji agenta usługi Kopia zapasowa Azure. Wybierz folder instalacji i folderu pliki tymczasowe wymagane dla agenta. Określona lokalizacja pamięci podręcznej musi mieć wolnego miejsca, czyli co najmniej 5% danych kopii zapasowej.
5. Jeśli używasz serwera proxy, aby nawiązać połączenie z Internetem, w **konfiguracji serwera Proxy** ekranu, wprowadź szczegóły serwera proxy. Jeśli korzystasz z uwierzytelnionego serwera proxy, wprowadź szczegóły nazwy i hasła użytkownika na tym ekranie.
6. Agent usługi Kopia zapasowa Azure instaluje program .NET Framework 4.5 i programu Windows PowerShell (Jeśli nie jest dostępna) do ukończenia instalacji.
7. Po zainstalowaniu agenta kliknij **przejść do rejestracji** przycisk, aby kontynuować przepływ pracy.
   
   ![Zarejestruj subskrypcję](./media/backup-install-agent/register.png)
8. Na ekranie poświadczenia magazynu Wyszukaj i wybierz plik poświadczeń magazynu, który został wcześniej pobrany.
   
    ![Poświadczenia magazynu](./media/backup-install-agent/vc.png)
   
    Plik poświadczeń magazynu jest prawidłowa tylko dla 48 godzin (po jej pobraniu, z portalu). W przypadku napotkania jakiegokolwiek błędu w tym ekranu (np. "plik poświadczeń magazynu określony limit czasu"), zaloguj się do portalu Azure i ponownie Pobierz plik poświadczeń magazynu.
   
    Upewnij się, że plik poświadczeń magazynu jest dostępny w lokalizacji, które są dostępne dla aplikacji Instalatora. Jeśli wystąpią dostępu pokrewne błędy, skopiuj plik poświadczeń magazynu do tymczasowej lokalizacji na tym komputerze i spróbuj ponownie wykonać operację.
   
    Jeśli wystąpi błąd poświadczeń magazynu nieprawidłowe (np. "udostępniono nieprawidłowe poświadczenia magazynu"), plik jest uszkodzony lub jest nie ma najnowszych poświadczeń skojarzonych z usługą odzyskiwania. Spróbuj ponownie wykonać operację po pobraniu nowego pliku poświadczeń magazynu z portalu. Ten błąd występuje zazwyczaj, gdy użytkownik kliknie **poświadczenia magazynu pobierania** opcji w portalu Azure, szybkie naciśnięcie. W takim przypadku tylko drugi plik poświadczeń magazynu jest nieprawidłowa.
9. W **ustawienie szyfrowania** ekranu, można wygenerować hasło lub podać hasło (co najmniej 16 znaków). Pamiętaj, aby zapisać hasła w bezpiecznym miejscu.
   
    ![Szyfrowanie](./media/backup-install-agent/encryption.png)
   
   > [!WARNING]
   > W przypadku zgubienia lub zapomnienia hasła; Microsoft nie może pomóc w odzyskaniu danych kopii zapasowej. Hasło szyfrowania jest właścicielem użytkownika końcowego i Microsoft nie ma wgląd w miejscu hasło używane przez użytkownika końcowego. Zapisz plik w bezpiecznym miejscu, ponieważ jest on wymagany podczas operacji odzyskiwania.
   > 
   > 
10. Po kliknięciu **Zakończ** przycisk, komputer został pomyślnie zarejestrowany w magazynie i są teraz rozpocząć tworzenie kopii zapasowych Microsoft Azure.
11. Podczas korzystania z autonomicznej kopia zapasowa Microsoft Azure można zmodyfikować ustawienia określone podczas rejestracji przepływu pracy, klikając **Zmień właściwości** opcji w przystawce mmc usługi Kopia zapasowa Azure w.
    
    ![Zmień właściwości](./media/backup-install-agent/change.png)
    
    Alternatywnie, korzystając z programu Data Protection Manager, można zmodyfikować ustawienia określone podczas rejestracji przepływu pracy, klikając **Konfiguruj** opcję, wybierając **Online** w obszarze **Zarządzania** kartę.
    
    ![Konfigurowanie usługi Azure Backup](./media/backup-install-agent/configure.png)

