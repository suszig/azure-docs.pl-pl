
1. W Eksploratorze rozwiązań programu Visual Studio, kliknij prawym przyciskiem myszy projekt aplikacji Sklepu Windows, a następnie kliknij przycisk **magazynu** > **Skojarz aplikację ze sklepem**.

    ![Skojarz aplikację ze Sklepu Windows](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. W kreatorze kliknij **dalej**i zaloguj się przy użyciu konta Microsoft. Wpisz nazwę aplikacji w **Zarezerwuj nazwę nowej aplikacji**, a następnie kliknij przycisk **rezerwy**.
3. Po pomyślnym utworzeniu rejestracji aplikacji, wybierz nową nazwę aplikacji, kliknij przycisk **dalej**, a następnie kliknij przycisk **skojarzyć**. Spowoduje to dodanie wymaganych informacji dotyczących rejestracji w Sklepie Windows do manifestu aplikacji.
4. Powtórz kroki 1 i 3 dla projektu aplikacji ze Sklepu Windows Phone przy użyciu tego samego rejestracji, wcześniej utworzone dla aplikacji ze Sklepu Windows.  
5. Przejdź do [Centrum deweloperów systemu Windows](https://dev.windows.com/en-us/overview)i zaloguj się przy użyciu konta Microsoft. Kliknij przycisk nowej rejestracji aplikacji w **Moje aplikacje**, a następnie rozwiń węzeł **usług** > **powiadomienia wypychane**.
6. Na **powiadomienia wypychane** kliknij przycisk **witryna usług Live** w obszarze **Push powiadomienia usługi WNS (Windows) i Microsoft Azure Mobile Apps**. Zanotuj wartości **identyfikatora SID pakietu** i *bieżącego* wartość w **klucz tajny aplikacji**. 

    ![Ustawienie aplikacji w Centrum deweloperów](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > Klucz tajny aplikacji i identyfikator SID pakietu są ważnymi poświadczeniami zabezpieczeń. Nie udostępniaj nikomu tych wartości ani nie rozpowszechniaj ich razem z aplikacją.
   >
   >
