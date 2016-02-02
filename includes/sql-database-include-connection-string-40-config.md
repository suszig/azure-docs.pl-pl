


### 接続文字列をセキュリティで保護するための構成ファイルの例

接続文字列をリテラルとして C# コード内に配置することは得策ではありません。 接続文字列は構成ファイル内に配置することをお勧めします。 再コンパイルなしでいつでも文字列を編集できます。

C# の場合、コンパイル済みプログラムの名前と仮定 **ConsoleApplication1.exe**, にこの .exe があると、 **bin \debug\** ディレクトリ。

この例では、接続文字列の大部分が、**ConsoleApplication1.exe.config** という名前の構成ファイルに保存されます。 この構成ファイルにも配置する必要があります **bin \debug\**します。

次の構成ファイルの XML の中に、**ConnectionString4NoUserIDNoPassword** という名前の接続文字列があります。 C# コードはこの文字列を探します。

次のプレースホルダーを実際の名前に編集する必要があります。

- {your_serverName_here} (サーバー名）
- {your_databaseName_here} (データベース名)


      <?xml version="1.0" encoding="utf-8" ?>
      <configuration>
          <startup> 
              <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
          </startup>
    
          <connectionStrings>
              <clear />
              <add name="ConnectionString4NoUserIDNoPassword"
              providerName="System.Data.ProviderName"
    
              connectionString=
              "Server=tcp:{your_serverName_here}.database.windows.net,1433;
              Database={your_databaseName_here};
              Connection Timeout=30;
              Encrypt=True;
              TrustServerCertificate=False;" />
          </connectionStrings>
      </configuration>




この説明では、次の 2 つのパラメーターを省略することを選択しました。

- User ID={your_userName_here}; (ユーザー名)
- Password={your_password_here}; (パスワード)


これらを含めることもできますが、これらの値はユーザーによるキーボード入力をプログラムで取得したほうがよい場合があります。 一概には言えません。









