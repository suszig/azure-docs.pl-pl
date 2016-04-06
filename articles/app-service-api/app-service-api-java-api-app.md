<properties
    pageTitle="Azure App Service での Java API アプリの構築とデプロイ"
    description="Java API アプリ パッケージを作成して Azure App Service にデプロイする方法について説明します。"
    services="app-service\api"
    documentationCenter="java"
    authors="bradygaster"
    manager="mohisri",
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="11/27/2015"
    ms.author="bradygaster"/>

# Azure App Service での Java API アプリの構築とデプロイ

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

このチュートリアルでは、Java アプリケーションを作成し、Azure App Service API Apps を使用する展開方法 [Git](http://git-scm.com)します。 このチュートリアルの手順は、Java を実行できる任意のオペレーティング システムで使用できます。 このチュートリアルのコードは、使用して構築された [Maven](https://maven.apache.org/)します。 [Jax RS](https://jax-rs-spec.java.net/) RESTful サービスを作成するために使用して、に基づいて生成される [Swagger](http://swagger.io) メタデータの仕様を使用して、 [Swagger のエディター](http://editor.swagger.io/)します。

## 前提条件

1. [Java Developer Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (またはそれ以降)
1. 有料または [無料評価版](https://azure.microsoft.com/pricing/free-trial/) へのサブスクリプション [Microsoft Azure](https://azure.microsoft.com)
1. [Maven](https://maven.apache.org/) 、開発用コンピューターにインストールされています。
1. 開発用コンピューターにインストールされた Git 

## Swagger.IO を使用して API をスキャフォールディングする
swagger.io オンライン エディターを使用して、API の構造を表す Swagger JSON または YAML コードに入力できます。 API のセキュリティを設計した後は、さまざまなプラットフォームやフレームワークのコードをエクスポートできます。 次のセクションでは、スキャフォールディングされたコードに変更を加え、モック機能を含めます。 

このデモでは、初めに Swagger JSON 本文を swagger.io エディターに貼り付け、それと JAX-RS を使って、REST API エンドポイントにアクセスするコードを生成します。 次に、スキャフォールディングされたコードを編集してモック データが返されるようにし、データの永続化メカニズムに基づいて構築された REST API をシミュレートします。  

1. 次の Swagger JSON コードをクリップボードにコピーします。

        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }
        
1. 移動し、 [オンライン Swagger のエディター](http://editor.swagger.io/)します。 1 回をクリックして、 **ファイル]、[貼り付け JSON** メニュー項目です。

    ![Paste Json](media/app-service-api-java-api-app/paste-json.png)

1. 前にコピーした Contacts List API Swagger JSON を貼り付けます。 

    ![Pasted Swagger](media/app-service-api-java-api-app/pasted-swagger.png)

1. エディターに表示されるドキュメントのページと API の概要を確認します。 

    ![View Swagger Generated Docs](media/app-service-api-java-api-app/view-swagger-generated-docs.png)

1. 選択、 **生成サーバー JAX RS]-> [** モック実装を追加して後で編集をサーバー側のコードをスキャフォールディングするメニュー オプション。 

    ![Generate Code Menu Item](media/app-service-api-java-api-app/generate-code-menu-item.png)
    
    コードが生成されると、ダウンロード用の zip ファイルが提供されます。 このファイルには、Swagger コード ジェネレーターによってスキャフォールディングされたコードと、関連するすべてのビルド スクリプトが含まれています。 ライブラリ全体を開発用ワークステーション上のディレクトリに解凍します。 

## コードを編集して API 実装を追加する
このセクションでは、生成されたコードのサーバー側の実装をカスタム コードに置き換えます。 新しいコードは、Contact エンティティの ArrayList を呼び出し元のクライアントに返します。 

1. 開いている、 *Contact.java* にあるモデル ファイル、 *src/gen/java/swagger/モデル* ] フォルダーを使用して [Visual Studio Code](https://code.visualstudio.com) または使い慣れたテキスト エディターでします。 

    ![Open Contact Model File](media/app-service-api-java-api-app/open-contact-model-file.png)

1. 次のコンス トラクターを追加、 **連絡先** クラスです。 

        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }

1. 開いている、 *ContactsApiServiceImpl.java* にあるサービス実装ファイル、 *src/main/java/swagger/api/impl* ] フォルダーを使用して [Visual Studio Code](https://code.visualstudio.com) または使い慣れたテキスト エディターでします。

    ![Open Contact Service Code File](media/app-service-api-java-api-app/open-contact-service-code-file.png)

1. この新しいコードでファイル内のコードを上書きし、サービス コードにモック実装を追加します。 

        package io.swagger.api.impl;

        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;

        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
  
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
  
            @Override
            public Response contactsGet()
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
  
            @Override
            public Response contactsGetById(Integer id)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
            
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                    {
                        ret = list.get(i);
                    }
                }
                return Response.ok().entity(ret).build();
            }
        }

1. コマンド プロンプトを開きます。

1. 次の Maven コマンドを実行してコードをビルドし、Jetty アプリケーション サーバーを使用してローカルで実行します。 

        mvn package jetty:run
        
1. Jetty がポート 8080 でコードを開始したことがコマンド ウィンドウに表示されます。 

    ![Open Contact Service Code File](media/app-service-api-java-api-app/run-jetty-war.png)
    
1. 使用 [Postman](https://www.getpostman.com/) 、「すべての連絡先を取得する」に要求をアドレス帳 http://localhost:8080/api/にある API メソッドにします。

    ![Call the Contacts API](media/app-service-api-java-api-app/calling-contacts-api.png)
    
1. 使用 [Postman](https://www.getpostman.com/) http://localhost:8080/api/連絡先/2 にある「特定の連絡先を取得する」API メソッドに要求を行うためです。

    ![Call the Contacts API](media/app-service-api-java-api-app/calling-specific-contact-api.png)
    
1. 最後に、コンソールで次の Maven コマンドを実行することにより、Java WAR (Web ARchive) ファイルをビルドします。 

        mvn package war:war
        
    配置する WAR ファイルが作成されると、 **ターゲット** フォルダーです。 移動し、 **ターゲット** フォルダーや名前変更、WAR ファイル **ROOT.war** (大文字と小文字がこの形式に一致することを確認してください)。
    
        rename swagger-jaxrs-server-1.0.0.war ROOT.war
        
    最後に、作成するには、次のコマンドを実行、 **展開** フォルダーを使用して WAR ファイルを Azure に展開します。 
    
        mkdir deploy
        mkdir deploy\webapps
        copy target\ROOT.war deploy\webapps
        cd deploy
    
## 出力を Azure App Service に発行する
このセクションでは、Azure ポータルを使用して新しい API アプリを作成する方法、Java アプリケーションをホストするためにこの API アプリを準備する方法、新しく作成した WAR ファイルを Azure App Service にデプロイして新しい API アプリを実行する方法について説明します。 

1. 新しい API アプリを作成、 [Azure ポータル](http://portal.azure.com), 、] をクリックして、 **新規]、[Web + モバイル]、[API アプリ** メニュー項目です。
    
    ![Create a new API App](media/app-service-api-java-api-app/create-api-app.png)

1. クリックして、 **アプリケーション設定** API アプリの設定] ブレードでオプションです。 次に、[Java バージョン] メニューから最新の Java バージョンを選択し、[Web コンテナー] メニューから最新の Tomcat を選択します。

    ![Set up Java in the API App blade](media/app-service-api-java-api-app/set-up-java.png)

1. クリックして、 **デプロイ資格情報** 設定] メニュー項目、およびファイルを API アプリを発行するために使用するユーザー名とパスワードを提供します。 

    ![デプロイメント資格情報の設定](media/app-service-api-java-api-app/deployment-credentials.png)

1. クリックして、 **継続的なデプロイ** 設定] メニューの項目。 1 回をクリックして、 **ソースの選択** ボタンをクリックして、 **ローカル Git リポジトリ** オプション。 これにより、Azure で実行される、API アプリに関連付けられた Git リポジトリが作成されます。 コードをコミットするたびに、 *マスター* ブランチ、Git リポジトリのコードを実際の実行中の API アプリ インスタンスに公開されます。 

    ![新しいローカル Git リポジトリの設定](media/app-service-api-java-api-app/select-git-repo.png)

1. 新しい Git リポジトリの URL をクリップボードにコピーします。 後で重要になるため、これを保存します。 

    ![アプリ用の新しい Git リポジトリの設定](media/app-service-api-java-api-app/copy-git-repo-url.png)

1. Git は WAR ファイルをオンラインのリポジトリにプッシュします。 これを行うに移動、 **展開** フォルダーする前に作成できるように、簡単にコミットするコードまで、App Service で実行されているリポジトリです。 コンソール ウィンドウから webapps フォルダーのあるフォルダーに移動したら、次の Git コマンドを発行してプロセスを起動し、デプロイメントを実行します。 

        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [YOUR GIT URL]     
        git push azure master
        
    発行した後、 **プッシュ** 要求をたずねられます展開資格情報を先ほど作成したパスワードを入力します。 パスワードを入力すると、更新されてデプロイされたポータルが表示されます。 
        
1. 再度 Postman を使用して Azure App Service で動作している新しくデプロイされた API アプリにヒットすると、一貫した動作であること、期待どおりの連絡先データが返されていること、Swagger.io でスキャフォールディングされた Java コードへの単純なコード変更が使用されていることを確認できます。 

    ![Using your Java Contacts REST API live in Azure](media/app-service-api-java-api-app/postman-calling-azure-contacts.png)
    
## 次のステップ
この記事では、初めに Swagger JSON ファイルを使用し、Swagger.io エディターでスキャフォールディングされた Java コードを作成しました。 そこから、単純な変更と Git デプロイ プロセスにより、Java で記述された機能的な API アプリを作成しました。 API アプリの取得中に、次のチュートリアル シリーズの番組をどのように開始する [CORS を使用して、JavaScript クライアントから API アプリを使用する](app-service-api-cors-consume-javascript.md)です。

でこのサンプルを構築する詳細については、 [Storage SDK for Java](../storage/storage-java-how-to-use-blob-storage.md) JSON を保持する blob です。 また、使用することが、 [DB Java SDK のドキュメント](../documentdb/documentdb-java-application.md) Azure Document DB に連絡先データを保存します。 

Azure で Java を使用する方法の詳細については、次を参照してください。、 [Java デベロッパー センター](/develop/java/)します。


