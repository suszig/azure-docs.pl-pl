Portal Azure Marketplace udostępnia szeroką gamę popularnych aplikacji sieci Web opracowanych przez firmę Microsoft, inne firmy oraz zespoły związane z inicjatywami dotyczącymi oprogramowania typu open source. Aplikacje sieci Web utworzone z poziomu portalu Azure Marketplace nie wymagają instalowania żadnego oprogramowania oprócz przeglądarki, przy użyciu której nawiązuje się połączenie z witryną [Azure Portal w wersji zapoznawczej](http://go.microsoft.com/fwlink/?LinkId=529715). 

Niniejszy samouczek zawiera informacje na temat wykonywania następujących czynności:

* Jak utworzyć nową aplikację sieci Web za pośrednictwem portalu Azure Marketplace.
* Jak wdrożyć aplikację sieci Web za pośrednictwem witryny Azure Portal w wersji zapoznawczej.

Utworzysz blog WordPress korzystający z domyślnego szablonu. Poniższa ilustracja przedstawia gotową aplikację:

![Blog WordPress][13]

> [!NOTE]
> Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Wypróbuj usługę App Service](https://azure.microsoft.com/try/app-service/), w którym wyjaśniono, jak od razu utworzyć początkową aplikację sieci Web o krótkim okresie istnienia w usłudze App Service. Bez kart kredytowych i bez zobowiązań.
> 
> 

## <a name="create-a-web-app-in-the-portal"></a>Tworzenie aplikacji sieci Web w portalu
1. Zaloguj się do witryny Azure Portal w wersji zapoznawczej.
2. Otwórz portal Azure Marketplace, klikając ikonę **Marketplace**:
   
    ![Ikona Marketplace][marketplace]
   
    Lub klikając ikonę **Nowe** w prawym górnym rogu pulpitu nawigacyjnego i wybierając pozycję **Marketplace** w dolnej części listy.
   
    ![Create New (Utwórz nowe)][5]
3. Wybierz pozycję **Sieć Web + aplikacje mobilne**. Wyszukaj wyrażenie **WordPress** i kliknij ikonę **WordPress**.
   
    ![Pozycja WordPress na liście][7]
4. Po przeczytaniu opisu aplikacji WordPress wybierz pozycję **Utwórz**.
5. Kliknij pozycję **Aplikacja sieci Web** i podaj wartości wymagane do skonfigurowania aplikacji.
   
    ![Konfigurowanie aplikacji][8]
6. Kliknij pozycję **Baza danych** i podaj wartości wymagane do skonfigurowania bazy danych MySQL. 
   
    ![Konfigurowanie bazy danych][database]
7. Podaj nazwę nowej grupy zasobów.
   
    ![Ustawianie grupy zasobów][groupname]
8. W razie potrzeby kliknij pozycję **SUBSKRYPCJA** i określ subskrypcję, która ma być używana. 
9. Gdy zakończysz definiować aplikację sieci Web, kliknij pozycję **Utwórz** i poczekaj na utworzenie nowej aplikacji sieci Web.
   
   Po utworzeniu aplikacji zostanie wyświetlona grupa zasobów zawierająca aplikację sieci Web i bazę danych.
   
   ![Wyświetlanie grupy][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Uruchamianie aplikacji sieci Web WordPress i zarządzanie nią
1. Kliknij nową aplikację sieci Web, aby zobaczyć jej szczegóły.
   
    ![Uruchamianie pulpitu nawigacyjnego][10]
2. Na stronie **Podstawy** kliknij pozycję **Przeglądaj** lub link pod pozycją **Url**, aby otworzyć stronę powitalną aplikacji sieci Web.
   
    ![Adres URL witryny][browse]
3. Jeśli nie zainstalowano platformy WordPress, wprowadź odpowiednie informacje o konfiguracji wymagane przez platformę WordPress i kliknij pozycję **Zainstaluj platformę WordPress**, aby zakończyć konfigurowanie i otworzyć stronę logowania aplikacji sieci Web.
4. Kliknij pozycję **Zaloguj się** i wprowadź swoje poświadczenia.  
5. Będziesz mieć nową aplikację sieci Web WordPress podobną do aplikacji poniżej.    
   
    ![Witryna WordPress][13]

[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png


<!--HONumber=Jan17_HO3-->


