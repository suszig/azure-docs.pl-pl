1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com), a następnie kliknij opcję **+NOWE** u dołu ekranu.
2. Kliknij kolejno pozycje **App Services**, **Mobile Engagement** i **Utwórz**.
   
       ![](./media/mobile-engagement-create-app-in-portal/create-mobile-engagement-app.png)
3. W wyświetlonym oknie podręcznym wprowadź następujące informacje:
   
       ![](./media/mobile-engagement-create-app-in-portal/create-azme-popup.png)
   
   * **Application Name** (Nazwa aplikacji): nazwa aplikacji. 
   * **Platforma** (Platform): docelowa platforma aplikacji. Należy utworzyć jedną aplikację usługi Mobile Engagement dla każdej platformy, która ma być platformą docelową aplikacji mobilnej. 
   * **Application Resource Name** (Nazwa zasobów aplikacji): nazwa umożliwiająca dostęp do tej aplikacji za pośrednictwem interfejsów API i adresów URL. 
   * **Location** (Lokalizacja): region/centrum danych, w którym będzie obsługiwana ta aplikacja i kolekcja aplikacji.
   * **Collection** (Kolekcja): wybierz wcześniej utworzoną kolekcję lub wybierz opcję „New Collection” (Nowa kolekcja).
   * **Collection Name** (Nazwa kolekcji): odnosi się do grupy aplikacji. Dzięki temu wszystkie aplikacje będą należeć do grupy, co umożliwi zagregowane obliczenia metryk. Jeśli ma to zastosowanie, w tym miejscu należy użyć nazwy firmy lub działu.
4. Wybierz nowo utworzoną aplikację na karcie **Applications** (Aplikacje).
5. Kliknij opcję **CONNECTION INFO** (INFORMACJE O POŁĄCZENIU), aby wyświetlić ustawienia połączenia, które zostaną użyte do integracji zestawu SDK w aplikacji mobilnej.
6. Skopiuj zawartość pola **CONNECTION STRING** (PARAMETRY POŁĄCZENIA) — te dane będą potrzebne do zidentyfikowania tej aplikacji w kodzie aplikacji i nawiązania połączenia z usługą Mobile Engagement z aplikacji.
   
       ![](./media/mobile-engagement-create-app-in-portal/app-connection-info-page.png)



<!--HONumber=Jan17_HO3-->


