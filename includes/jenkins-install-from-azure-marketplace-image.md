1. W przeglądarce otwórz [portalu Azure Marketplace obrazu Wpięć](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview).

1. Wybierz **Pobierz teraz**.

    ![Wybierz teraz IT GIT do uruchamiania procesu instalacji obrazu Wpięć Marketplace.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. Po przejrzeniu informacji cenową szczegóły i warunki, zaznacz **Kontynuuj**.

    ![Marketplace Wpięć ceny i warunki informacji o obrazie.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. Wybierz **Utwórz** skonfigurować serwer Wpięć w portalu Azure. 

    ![Zainstaluj obraz Wpięć Marketplace.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-create.png)

1. W **podstawy** karcie, określ następujące wartości:

    - **Nazwa** — wprowadź `Jenkins`.
    - **Użytkownik** — wprowadź nazwę użytkownika używane podczas rejestrowania się w maszynie wirtualnej, na którym działa Wpięć.
    - **Typ uwierzytelniania** — wybierz tę opcję **hasło**.
    - **Hasło** — wprowadź hasło do użycia podczas rejestrowania się w maszynie wirtualnej, na którym działa Wpięć.
    - **Potwierdź hasło** — wprowadź ponownie hasło do użycia podczas rejestrowania się w maszynie wirtualnej, na którym działa Wpięć.
    - **Typ zlecenia Wpięć** — wybierz tę opcję **LTS**.
    - **Subskrypcja** — Wybierz subskrypcję platformy Azure, w którym chcesz zainstalować Wpięć.
    - **Grupa zasobów** — wybierz tę opcję **Utwórz nowy**, a następnie wprowadź nazwę grupy zasobów, która służy jako kontener logiczny dla kolekcji zasobów, które składają się na instalację Wpięć.
    - **Lokalizacja** — wybierz tę opcję **wschodnie stany USA**.

    ![Wprowadź informacje grupy uwierzytelniania i zasobów dla Wpięć na karcie podstawowego.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. Wybierz **OK** do przejścia do **ustawienia** kartę. 

1. W **ustawienia** karcie, określ następujące wartości:

    - **Rozmiar** — wybierz opcję odpowiednią rozmiaru Wpięć maszyny wirtualnej.
    - **Typ dysku maszyny Wirtualnej** — określ albo dysk twardy (dysk twardy) lub dysków SSD (SSD), aby wskazać, który typ dysku magazynu jest dozwolony dla Wpięć maszyny wirtualnej.
    - **Publiczny adres IP** — nazwa adresu IP domyślnie nazwa Wpięć określone w poprzedniej strony z sufiksem - IP. Można wybrać opcję, aby zmienić tego ustawienie domyślne.
    - **Etykieta nazwy domeny** — Określ wartość dla w pełni kwalifikowany adres URL do maszyny wirtualnej Wpięć.

    ![Wprowadź ustawienia maszyny wirtualnej Wpięć na karcie Ustawienia.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. Wybierz **OK** do przejścia do **Podsumowanie** kartę.

1. Gdy **Podsumowanie** Wyświetla kartę, wprowadzonej zostanie zweryfikowany. Przeanalizowanie **weryfikacji przekazany** wiadomości, wybierz opcję **OK**. 

    ![Karta Podsumowanie Wyświetla i sprawdza poprawność wybranych opcji.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. Gdy **Utwórz** Wyświetla kartę, zaznacz **Utwórz** można utworzyć maszyny wirtualnej Wpięć. Gdy serwer jest gotowy, powiadomienie będzie wyświetlane w portalu Azure.

    ![Wpięć jest gotowy powiadomień.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-notification.png)