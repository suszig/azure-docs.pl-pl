## <a name="set-up-the-development-environment"></a>Konfigurowanie środowiska deweloperskiego

W tej sekcji przedstawiono konfigurowanie środowiska deweloperskiego, w tym tworzenie aplikacji ASP.NET MVC, dodawanie połączenia usług połączonych dodawania kontrolera i określanie dyrektywy wymaganej przestrzeni nazw.

### <a name="create-an-aspnet-mvc-app-project"></a>Utwórz projekt aplikacji platformy ASP.NET MVC

1. Otwórz program Visual Studio.

1. Wybierz **Plik -> Nowy -> Projekt** z poziomu menu głównego

1. Na **nowy projekt** okna dialogowego i określ opcje jako wyróżnione na poniższej ilustracji:

    ![Tworzenie projektu platformy ASP.NET](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. Kliknij przycisk **OK**.

1. Na **nowy projekt ASP.NET** okna dialogowego i określ opcje jako wyróżnione na poniższej ilustracji:

    ![Określ MVC](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

1. Kliknij przycisk **OK**.

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Połącz się z kontem magazynu platformy Azure za pomocą połączenia usług

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz z menu kontekstowego **podłączonej usługi -> Dodaj**.

1. Na **dodać podłączonej usługi** okno dialogowe, wybierz opcję **usługi Azure Storage**, a następnie wybierz **Konfiguruj**.

    ![Podłączonej usługi okna dialogowego](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. Na **usługi Azure Storage** okno dialogowe, wybierz konto żądaną magazynu Azure, z którym chcesz pracować, a następnie wybierz **Dodaj**.
