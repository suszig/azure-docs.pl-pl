## <a name="connect-to-outlookcom"></a>Połączenie do usługi Outlook.com
### <a name="prerequisites"></a>Wymagania wstępne
* Konto usługi Outlook.com

Aby korzystać z konta usługi Outlook.com, w aplikacji logiki, musisz zezwolić aplikacji logiki do łączenia się z kontem usługi Outlook.com. Na szczęście można w tym z aplikacji logiki w portalu Azure. 

Poniżej przedstawiono kroki, aby zezwolić aplikacji logiki do łączenia się z kontem usługi Outlook.com:

1. Wszystkie aplikacje logiki muszą ma być uruchamiana przez wyzwalacz, więc po utworzeniu aplikacji logiki, otwiera projektanta i wyświetla listę wyzwalaczy, które można uruchomić aplikację logiki:
   
   ![](./media/connectors-create-api-outlook/office365-outlook-0.png)
2. Wprowadź "outlook" w polu wyszukiwania. Należy zauważyć, że lista jest filtrowana, aby wyświetlić listę wszystkich wyzwalaczy z "Outlook" w nazwie:![](./media/connectors-create-api-outlook/office365-outlook-0-5.png)
3. Wybierz **Office 365 Outlook - na nowy adres e-mail**.   
   Jeśli nie utworzono wszystkie połączenia z programu Outlook przed będzie zostanie wyświetlony monit o podanie poświadczeń Outlook.com. Te poświadczenia będą używane do autoryzacji aplikację logiki, aby nawiązać połączenie i dostęp do danych konta usługi Outlook.com:![](./media/connectors-create-api-outlook/office365-outlook-1.png)
4. Podaj poświadczenia dla programu Outlook i zaloguj się:![](./media/connectors-create-api-outlook/office365-outlook-2.png)  
   To już wszystko. Połączenie z programem Outlook został już utworzony. To połączenie będzie dostępne w innych tworzonej aplikacji logiki.

