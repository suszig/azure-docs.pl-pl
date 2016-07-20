
###Tworzenie projektu usługi Google Cloud Messaging przy użyciu klucza interfejsu API

>[AZURE.NOTE] Aby wykonać tę procedurę, musisz mieć konto Google ze zweryfikowanym adresem e-mail. Aby utworzyć nowe konto Google, przejdź do strony <a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a>.

1. Przejdź do konsoli [Google Cloud Console](https://console.developers.google.com/project) i zaloguj się przy użyciu poświadczeń konta Google.

2. Kliknij pozycję **Go to project** (Przejdź do projektu), a następnie kliknij opcję **Create Project** (Utwórz projekt).
<!--
    ![](./media/mobile-engagement-enable-google-cloud-messaging/new-project.png)

    ![](./media/mobile-engagement-enable-google-cloud-messaging/new-project-2.png)   
-->
3. Wprowadź nazwę projektu.

4. Zanotuj numer projektu, który jest widoczny poniżej pola tekstowego **Project name** (Nazwa projektu). Będzie on potrzebny później podczas wykonywania kroków samouczka w celu uzupełnienia pliku manifestu systemu Android.
    ![](./media/mobile-engagement-enable-google-cloud-messaging/project-number.png)   
5. Kliknij przycisk **Utwórz**.

6. Upewnij się, że w lewej kolumnie została wybrana opcja **Overview** (Przegląd), a następnie w obszarze dotyczącym mobilnych interfejsów API kliknij pozycję **Google Cloud Messaging**. Następnie na kolejnej stronie kliknij opcję **Enable** (Włącz).

    ![](./media/mobile-engagement-enable-google-cloud-messaging/enable-GCM.png)
<!--
    ![](./media/mobile-engagement-enable-google-cloud-messaging/enable-gcm-2.png)
-->
7. Na następnej stronie kliknij opcję **Go to Credentials** (Przejdź do poświadczeń). Na kolejnej stronie wybierz pozycję **Google Cloud Messaging** w pierwszym polu listy rozwijanej oraz pozycję **Web server** (Serwer sieci Web) w drugim polu listy rozwijanej, po czym kliknij pozycję **What credentials do I need?** (Wymagane poświadczenia).

    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key.png)

8. Na stronie **Add credentials to your project** (Dodawanie poświadczeń do projektu) kliknij pozycję **Create API key** (Utwórz klucz interfejsu API).

    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key5.png)
<!--
    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key6.png)
-->
9. Zanotuj wartość w polu **API KEY** (KLUCZ INTERFEJSU API). Ta wartość klucza interfejsu API zostanie później użyta do konfiguracji w sekcji dotyczącej natywnych powiadomień wypychanych. Kliknij przycisk **Done** (Gotowe).



<!--HONumber=Jun16_HO2-->


