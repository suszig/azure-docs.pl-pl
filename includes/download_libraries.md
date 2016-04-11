## Azure Client Libraries for Java - 手動ダウンロード

Azure Libraries for Java はの下で配布、 [Apache License, バージョン 2.0][license]します。 をクリックして [ここ][zip-download] ライブラリとすべての依存関係の ZIP ファイルです。  Microsoft Open Technologies, Inc. によって利用可能なこのされます。 License.txt と ThirdPartyNotices.txt ファイル、ZIP 内のライセンスおよびその他の情報を参照してください。

## Azure Libraries for Java - Maven

プロジェクトが既に Maven を使用してビルドする設定になっている場合には、pom.xml ファイルに次の依存関係を追加します。 メモ: Java 用 Azure ライブラリを使用する Eclipse で Maven プロジェクトを作成する方法の詳細については、表示 [Azure Management Libraries for Java の概要][maven-getting-started]します。

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-compute</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-network</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-sql</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-storage</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-websites</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-media</artifactId>
        <version>0.6.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>0.6.0</version>
    </dependency>


内で、 `<version>` 要素から取得できる有効なバージョン番号を持つこの例ではバージョン番号を置き換えて、 [Maven の Azure ライブラリ リポジトリ](http://go.microsoft.com/fwlink/?LinkID=286274)します。

[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
