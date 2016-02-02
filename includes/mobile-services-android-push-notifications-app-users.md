次に、登録を試みる前にユーザーが認証されるように、通知を登録するタイミングを変更する必要があります。


1. Android studio Project Explorer で ToDoActivity.java ファイルを開き、検索、 `onCreate` メソッドです。 次のコードを移動、 `onCreate` メソッドの先頭に、 `createTable` メソッドです。

     NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

  `CreateTable` メソッドが呼び出されます `認証` メソッドが完了するとします。 全体 `createTable` メソッドは、次のようになります。

     private void createTable() {
    
         NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);
    
         // Get the Mobile Service Table instance to use
         mToDoTable = mClient.getTable(ToDoItem.class);
    
         mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
    
         // Create an adapter to bind the items with the view
         mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
         ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
         listViewToDo.setAdapter(mAdapter);
    
         // Load the items from the Mobile Service
         refreshItemsFromTable();
     }   







