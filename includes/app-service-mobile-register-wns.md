
1. W Eksploratorze rozwiązań programu Visual Studio kliknij prawym przyciskiem myszy projekt aplikacji Sklepu Windows. Następnie wybierz **magazynu** > **Skojarz aplikację ze sklepem**.

    ![Skojarz aplikację ze Sklepu Windows](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. W kreatorze Wybierz **dalej**. Następnie zaloguj się przy użyciu konta Microsoft. W **Zarezerwuj nazwę nowej aplikacji**, wpisz nazwę aplikacji, a następnie wybierz **rezerwy**.
3. Po pomyślnym utworzeniu rejestracji aplikacji, wybierz nową nazwę aplikacji. Wybierz **dalej**, a następnie wybierz **skojarzyć**. Ten proces powoduje dodanie wymaganych informacji o rejestracji Sklepu Windows do manifestu aplikacji.
4. Powtórz kroki 1 i 3 dla projektu aplikacji ze Sklepu Windows Phone przy użyciu tego samego rejestracji, wcześniej utworzone dla aplikacji ze Sklepu Windows.  
5. Przejdź do [Centrum deweloperów systemu Windows](https://dev.windows.com/en-us/overview), a następnie zaloguj się przy użyciu konta Microsoft. W **Moje aplikacje**, wybierz nowej rejestracji aplikacji. Następnie rozwiń węzeł **usług** > **powiadomienia wypychane**.
6. Na **powiadomienia wypychane** w obszarze **Push powiadomienia usługi WNS (Windows) i Microsoft Azure Mobile Apps**, wybierz pozycję **witryna usług Live**.  Zanotuj wartości **identyfikatora SID pakietu** i *bieżącego* wartość w **klucz tajny aplikacji**. 

    ![Ustawienie aplikacji w Centrum deweloperów](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > Klucz tajny aplikacji i identyfikator SID pakietu są ważnymi poświadczeniami zabezpieczeń. Nie udostępniaj nikomu tych wartości ani nie rozpowszechniaj ich z aplikacją.
   >
   >
