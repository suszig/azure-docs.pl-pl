
###Tworzenie projektu usługi Google Cloud Messaging przy użyciu klucza interfejsu API

>[AZURE.NOTE] Aby wykonać tę procedurę, musisz mieć konto Google ze zweryfikowanym adresem e-mail. Aby utworzyć nowe konto Google, przejdź do strony <a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a>.

1. Przejdź do konsoli [Google Cloud Console](https://console.developers.google.com/project) i zaloguj się przy użyciu poświadczeń konta Google.

2. Przejdź do obszaru **All Projects** (Wszystkie projekty), a następnie kliknij polecenie **Create Project** (Utwórz projekt).

3. Wprowadź wartość **Project name** (Nazwa projektu) i kliknij pozycję **Create** (Utwórz)

4. Po utworzeniu projektu upewnij się, że masz zanotowaną wartość **Project number** (Numer projektu), która jest długą wartością liczbową. Numer ten można też znaleźć w sekcji **IAM & Admin** (Menedżer IAM i administrator) w **ustawieniach** projektu. Informacja ta będzie potrzebna później. 
 
    ![](./media/mobile-engagement-enable-google-cloud-messaging/project-number.png)

5. Teraz utworzymy klucz dla platformy Google Cloud do obsługi komunikatów w chmurze, który będzie używany przez naszą platformę do wysyłania powiadomień do urządzeń z systemem Android. Przejdź do sekcji **API Manager** (Interfejs API Manager), a następnie kliknij polecenie **Google Cloud Messaging** w obszarze **Mobile APIs** (Mobilne interfejsy API). 

    ![](./media/mobile-engagement-enable-google-cloud-messaging/gcm.png)

6. Na kolejnej stronie kliknij przycisk **Enable** (Włącz). Pulpit nawigacyjny wyświetli monit o utworzenie poświadczeń. Kliknij przycisk **Go to Credentials** (Przejdź do poświadczeń). 

    ![](./media/mobile-engagement-enable-google-cloud-messaging/enable-GCM.png)

6. Wybierz pozycję **Google Cloud Messaging** w pierwszym polu listy rozwijanej oraz pozycję **Web server** (Serwer sieci Web) w drugim polu listy rozwijanej, po czym kliknij pozycję **What credentials do I need?** (Jakie poświadczenia są wymagane?)

    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key.png)

7. Na stronie **Add credentials to your project** (Dodawanie poświadczeń do projektu) kliknij pozycję **Create API key** (Utwórz klucz interfejsu API).

    ![](./media/mobile-engagement-enable-google-cloud-messaging/create-server-key5.png)

8. Zanotuj wartość w polu **API KEY** (KLUCZ INTERFEJSU API). Ta wartość klucza interfejsu API zostanie później użyta do konfiguracji w sekcji dotyczącej natywnych powiadomień wypychanych. Kliknij przycisk **Done** (Gotowe).



<!--HONumber=sep16_HO1-->


