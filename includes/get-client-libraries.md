### <a name="install-via-composer"></a>Zainstaluj za pośrednictwem Composer
1. Utwórz plik o nazwie **composer.json** w folderze głównym projektu i Dodaj do niej następujący kod:
   
    ```json
    {
      "require": {
        "microsoft/azure-storage": "*"
      }
    }
    ```
2. Pobierz  **[composer.phar] [ composer-phar]**  w katalogu głównym projektu.
3. Otwórz wiersz polecenia i uruchom następujące polecenie w katalogu głównym projektu
   
    ```
    php composer.phar install
    ```

Można również przejdź do [biblioteki klienta usługi Azure Storage PHP] [ php-sdk-github] w serwisie GitHub klonowanie kodu źródłowego.

[php-sdk-github]: https://github.com/Azure/azure-storage-php
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
