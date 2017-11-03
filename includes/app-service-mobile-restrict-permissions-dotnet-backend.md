
Domyślnie interfejsy API w zaplecze aplikacji mobilnej można wywołać anonimowo. Następnie należy ograniczyć dostęp tylko do uwierzytelnionych klientów.  

* **Ponownie node.js zakończenia (za pośrednictwem portalu Azure)** :  

    W ustawieniach aplikacji mobilnej kliknij **łatwe tabele** i wybierz tabelę. Kliknij przycisk **zmienić uprawnienia**, wybierz pozycję **uwierzytelniony dostęp tylko** wszystkie uprawnienia, a następnie kliknij przycisk **zapisać**.
* **.NET zaplecza (C#)**:  

    W projekcie serwer, przejdź do **kontrolerów** > **TodoItemController.cs**. Dodaj `[Authorize]` atrybutu **TodoItemController** klasy, w następujący sposób. Ograniczyć dostęp tylko do określonych metod, można również zastosować tego atrybutu tylko do tych metod, zamiast klasy. Ponownie opublikować projekt serwera.

        [Authorize]
        public class TodoItemController : TableController<TodoItem>

* **Zaplecza node.js (za pośrednictwem kodu Node.js)** :  

    Aby wymagać uwierzytelniania dla dostępu do tabel, Dodaj następujący wiersz do skryptu serwera Node.js:

        table.access = 'authenticated';

    Aby uzyskać więcej informacji, zobacz [porady: Wymagaj uwierzytelniania dostępu do tabel](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Aby dowiedzieć się, jak pobrać szybkiego startu projektu kodu z lokacji, zobacz [porady: pobieranie projekt kodu szybkiego startu zaplecza Node.js przy użyciu narzędzia Git](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).
