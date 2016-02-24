<properties
    pageTitle="DocumentDB による Python Flask Web アプリケーション開発 | Microsoft Azure"
    description="DocumentDB を使用してデータを格納し、Azure にホストされた Python Flask Web アプリケーションからそのデータにアクセスする方法をデータベース チュートリアルで確認します。 アプリケーション開発ソリューションを探します。" 
    keywords="Application development, database tutorial, python flask, python web application, python web development, documentdb, azure, Microsoft azure"
    services="documentdb"
    documentationCenter="python"
    authors="ryancrawcour"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="09/21/2015"
    ms.author="ryancraw"/>

# DocumentDB による Python Flask Web アプリケーション開発

> [AZURE.SELECTOR]
- [.NET](documentdb-dotnet-application.md)
- [Node.js](documentdb-nodejs-application.md)
- [Java](documentdb-java-application.md)
- [Python](documentdb-python-application.md)

どの顧客効果的に活用して Azure DocumentDB を強調表示するには
格納とクエリの JSON ドキュメントでは、このドキュメントでは、エンド ツー エンドの Python web アプリケーション
Azure DocumentDB を使用して投票の web アプリケーションの構築方法を示すチュートリアルです。

このチュートリアルは、によって提供される DocumentDB サービスを使用する方法を示します
ホストされている Python web アプリケーションから azure データ ストアとアクセスする
Azure Python を使用したいくつかの経験があることを想定しています、
Azure の web サイトです。

このデータベース チュートリアルの内容:

1. DocumentDB アカウントを作成してプロビジョニングする
2. Python MVC アプリケーションを作成する
3. Web アプリケーションから Azure DocumentDB に接続して使用する
4. Web アプリケーションを Azure Websites にデプロイする

このチュートリアルで、単純な投票を作成、
アンケートに回答する単純な投票アプリケーションを作成できます。

![このデータベース チュートリアルで作成された、ToDo リスト Web アプリケーションのスクリーン ショット](./media/documentdb-python-application/image1.png)


## データベース チュートリアルの前提条件

この記事の手順を実行する前に、次のソフトウェアがインストール
あるものをインストールします。

- [Visual Studio 2013](http://www.visualstudio.com/) または以上、または Visual Studio express を起動、無償版です。
- Python Tools for Visual Studio から [ここ][]します。
- Azure SDK for Visual Studio 2013, version 2.4 以降から利用できます。
[ここで][1]します。
- Python 2.7 から [ここ][2]します。
- Python 2.7 から、Microsoft Visual C コンパイラ [ここ][3]します。

## 手順 1: DocumentDB データベース アカウントを作成する

最初に、DocumentDB アカウントを作成します。 既にアカウントがある場合にスキップできます [手順 2: 新しい Python Flask web アプリケーションの作成](#Step-2:-Create-a-new-Python-Flask-Web-Application)します。

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

[AZURE.INCLUDE [documentdb-keys](../../includes/documentdb-keys.md)]

<br/>
最初から新しい Python Flask web アプリケーションを作成する方法はここで説明します。

## 手順 2: 新しい Python Flask Web アプリケーションを作成する

1. Visual Studio を開き、[ **ファイル** -\ > **新しいプロジェクト** -\ > **Python** -\ >、 **Flask Web
プロジェクト**, 、名前の新しいプロジェクトを作成および **チュートリアル**します。

    Python Flask は、Python で Web アプリケーションを短時間で作成するための Web アプリケーション開発フレームワークです。 [Flask のチュートリアルにアクセスするここをクリックして][]します。

    ![Visual Studio の [新しいプロジェクト] ウィンドウのスクリーン ショット。左側で [Python] が強調表示され、中央で [Python Flask Web プロジェクト] が選択され、[名前] ボックスに tutorial という名前が入力されている](./media/documentdb-python-application/image9.png)

2. 要求を送信するかどうか
外部パッケージをインストールします。 クリックして **に仮想環境にインストール**します。 現時点では PyDocumentDB は Python 3.x をサポートしていないため、必ず Python 2.7 をベースの環境として使用してください。  これで、プロジェクトに必要な Python 仮想環境が設定されます。

    ![[database tutorial - Python Tools for Visual Studio] ウィンドウのスクリーン ショット](./media/documentdb-python-application/image10.png)


## 手順 3: Python Flask Web アプリケーションを変更する

### プロジェクトへの Python Flask パッケージの追加

プロジェクトを設定した後は、特定の Flask パッケージに追加する必要があります。
DocumentDB 用の Python パッケージ pydocumentdb など、プロジェクトの必要があります。

1. という名前のファイルを開く **requirements.txt** 内容を次に置き換えます。

        flask==0.9
        flask-mail==0.7.6
        sqlalchemy==0.7.9
        flask-sqlalchemy==0.16
        sqlalchemy-migrate==0.7.2
        flask-whooshalchemy==0.55a
        flask-wtf==0.8.4
        pytz==2013b
        flask-babel==0.8
        flup
        pydocumentdb>=1.0.0

2. 右クリック **env** ] をクリック **requirements.txt からインストール**します。

    ![[env (Python 2.7)] を示すスクリーン ショット。リストで [requirements.txt からインストール] が選択されている](./media/documentdb-python-application/image11.png)

> [AZURE.NOTE] まれに、出力ウィンドウにエラーが表示されます。 もし
これが発生するかどうか、エラーがクリーンアップに関連を確認します。 場合によって、
クリーンアップに失敗したが、インストールを成功にすることができます (上にスクロール
出力ウィンドウに確認してください)。
<a name="verify-the-virtual-environment"></a> その場合は続行しても問題ありません。


### 仮想環境の確認

すべてが正しくインストールされているかどうかを確認してみましょう。

- キーを押して、web サイトを開始 **f5 キーを押して** Flask 開発サーバーが起動
web ブラウザーを開始します。 次のページが表示されます。

    ![空の Python Flask Web 開発プロジェクトがブラウザーで表示されます](./media/documentdb-python-application/image12.png)

### データベース、コレクション、およびドキュメント定義の作成

投票アプリケーションを作成します。

- という名前のフォルダーを右クリックして、Python ファイルを追加 **チュートリアル** ソリューション エクスプ ローラー。  ファイルに名前を **forms.py**します。  

```python
from flask.ext.wtf import Form
from wtforms import RadioField

class VoteForm(Form):
    deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```

### views.py への必要な import ステートメントの追加

- 先頭に次の import ステートメントを追加 **views.py**します。 これらのステートメントにより、DocumentDB の PythonSDK および Flask パッケージがインポートされます。

```python
from forms import VoteForm
import config
import pydocumentdb.document_client as document_client
```


### データベース、コレクション、およびドキュメントの作成

- 次のコードを追加 **views.py**します。 このコードでは、フォームで使用される
データベースの作成を行っています。 ファイル
**views.py**します。 単にこのコードを末尾に追加してください。

```python
@app.route('/create')
def create():
    """Renders the contact page."""
        client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})
    
        # Attempt to delete the database.  This allows this to be used to recreate as well as create
        try:
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))
        client.DeleteDatabase(db['_self'])
        except:
        pass
    
        # Create database
        db = client.CreateDatabase({ 'id': config.DOCUMENTDB_DATABASE })
        
        # Create collection
        collection = client.CreateCollection(db['_self'],{ 'id': config.DOCUMENTDB_COLLECTION }, { 'offerType': 'S1' })
        
        # Create document
        document = client.CreateDocument(collection['_self'],
        { 'id': config.DOCUMENTDB_DOCUMENT,
          'Web Site': 0,
          'Cloud Service': 0,
          'Virtual Machine': 0,
          'name': config.DOCUMENTDB_DOCUMENT 
        })
    
        return render_template(
            'create.html',
            title='Create Page',
            year=datetime.now().year,
            message='You just created a new database, collection, and document.  Your old votes have been deleted')
```

> [AZURE.TIP]  **CreateCollection** メソッドは、省略可能な **RequestOptions** 3 番目のパラメーターとして。 これを使用すると、コレクションのプランの種類を指定することができます。 offerType の値を指定しないと、コレクションは既定のプランの種類を使用して作成されます。 DocumentDB のプランの種類の詳細については、次を参照してください。 [DocumentDB のパフォーマンス レベル](documentdb-performance-levels.md)します。
>
### データベース、コレクション、およびドキュメントの読み取りとフォームの送信

- 次のコードを追加 **views.py**します。 これにより、セットアップの処理します。
データベース、コレクション、およびドキュメントの読み取りフォームです。 削除しないでください。
既存のコードのいずれかの **views.py**します。 単にこのコードを末尾に追加してください。

```python
@app.route('/vote', methods=['GET', 'POST'])
def vote():
    form = VoteForm()
        replaced_document ={}
        if form.validate_on_submit(): # is user submitted vote  
        client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})
    
        # Read databases and take the first since the id should not be duplicated.
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))
    
        # Read collections and take the first since the id should not be duplicated.
        coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config.DOCUMENTDB_COLLECTION))
    
        # Read documents and take the first since the id should not be duplicated.
        doc = next((doc for doc in client.ReadDocuments(coll['_self']) if doc['id'] == config.DOCUMENTDB_DOCUMENT))
    
        # Take the data from the deploy_preference and increment your database
        doc[form.deploy_preference.data] = doc[form.deploy_preference.data] + 1
        replaced_document = client.ReplaceDocument(doc['_self'], doc)
    
        # Create a model to pass to results.html
        class VoteObject:
            choices = dict()
                total_votes = 0
        
    vote_object = VoteObject()
        vote_object.choices = {
            "Web Site" : doc['Web Site'],
                "Cloud Service" : doc['Cloud Service'],
                "Virtual Machine" : doc['Virtual Machine']
    }
        
        vote_object.total_votes = sum(vote_object.choices.values())
    
        return render_template(
            'results.html',
                year=datetime.now().year,
                vote_object = vote_object)
        
    else :
        return render_template(
            'vote.html',
                title = 'Vote',
                year=datetime.now().year,
                form = form)
```


### HTML ファイルの作成

templates フォルダーの下に、create.html、results.html、vote.html の各 html ファイルを追加します。

1. 次のコードを追加 **create.html**します。 表示を行います
新しいデータベース、コレクション、およびドキュメントを作成したことを示すメッセージ。

```html
{% extends "layout.html" %}
{% block content %}
<h2>{{ title }}.</h2>
<h3>{{ message }}</h3>
<p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
{% endblock %}
```

2. 次のコードを追加 **results.html**します。 表示を行います
投票の結果。

```html
{% extends "layout.html" %}
{% block content %}
<h2>Results of the vote</h2>
    <br />
    
{% for choice in vote_object.choices %}
<div class="row">
    <div class="col-sm-5">{{choice}}</div>
        <div class="col-sm-5">
            <div class="progress">
                <div class="progress-bar" role="progressbar" aria-valuenow="{{vote_object.choices[choice]}}" aria-valuemin="0" aria-valuemax="{{vote_object.total_votes}}" style="width: {{(vote_object.choices[choice]/vote_object.total_votes)*100}}%;">
                            {{vote_object.choices[choice]}}
            </div>
        </div>
        </div>
</div>
{% endfor %}

<br />
<a class="btn btn-primary" href="{{ url_for('vote') }}">Vote again?</a>
{% endblock %}
```

3. 次のコードを追加 **vote.html**します。 表示するコードは、
ポーリングを行い、投票を受け付けます。 コントロールは、投票が登録、します。
views.py に渡さ投票キャストを認識し、
ドキュメントを適宜追加します。

```html
{% extends "layout.html" %}
{% block content %}
<h2>What is your favorite way to host an application on Azure?</h2>
<form action="" method="post" name="vote">
    {{form.hidden_tag()}}
        {{form.deploy_preference}}
        <button class="btn btn-primary" type="submit">Vote</button>
</form>
{% endblock %}
```

4. 内容を置き換える **index.html** 伴っています。 この
アプリケーションのランディング ページとして機能します。

```html
{% extends "layout.html" %}
{% block content %}
<h2>Python + DocumentDB Voting Application.</h2>
<h3>This is a sample DocumentDB voting application using PyDocumentDB</h3>
<p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear the Voting Database &raquo;</a></p>
<p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
{% endblock %}
```

### 構成ファイルの追加と、\_\_init\_\_.py の変更

1. プロジェクト [tutorial を右クリックし、ファイルを追加 **config.py**します。
この構成ファイルは、Flask のフォームで必要になります。 これを使用して、提供することができます、
秘密キーも同様です。 このチュートリアルではこのキーは不要です。

2. 次のコードを config.py に追加します。 値を変更 **documentdb \_host** と **\_key**します。

```python
CSRF_ENABLED = True
SECRET_KEY = 'you-will-never-guess'

DOCUMENTDB_HOST = 'https://YOUR_DOCUMENTDB_NAME.documents.azure.com:443/'
DOCUMENTDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='

DOCUMENTDB_DATABASE = 'voting database'
DOCUMENTDB_COLLECTION = 'voting collection'
DOCUMENTDB_DOCUMENT = 'voting document'
```

3. 同様の内容を置き換える **\_\_init\_\_.py** 次です。

```python
from flask import Flask
app = Flask(__name__)
app.config.from_object('config')
import tutorial.views
```

4. これは、上記に示す手順で後どのようにソリューション
エクスプ ローラーが表示されます。

    ![Visual Studio ソリューション エクスプローラーのウィンドウのスクリーンショット](./media/documentdb-python-application/image15.png)


## 手順 4: ローカルで Web アプリケーションを実行する

1. F5 キーを押すか、クリックして、 **実行** Visual Studio でが表示されます、
次の画面にします。

    ![Web ブラウザーに表示される [Python + DocumentDB Voting Application] のスクリーンショット](./media/documentdb-python-application/image16.png)

2. クリックして **投票データベースの作成/クリア** 、データベースを生成します。

    ![Web アプリケーションの [Create Page] ページのスクリーン ショット - 開発詳細](./media/documentdb-python-application/image17.png)

3. クリックして **投票** し、オプションを選択します。

    ![投稿用の質問を表示する Web アプリケーションのスクリーン ショット](./media/documentdb-python-application/image18.png)

4. 回答を投票するごとに、該当するカウンターの値が増加します。

    ![投票結果のページが表示されているスクリーン ショット](./media/documentdb-python-application/image19.png)


## 手順 5: Web アプリケーションを Azure Websites にデプロイする

に対して正常に動作する完全なアプリケーションが作成できた
DocumentDB、Azure の web サイトに展開するつもりです。

1. ソリューション エクスプ ローラーでプロジェクトを右クリックして (がないかどうかを必ず実行中
ローカルで) を選択して **発行**します。  クリックして **Microsoft Azure Websites**します。

    ![tutorial が選択されたソリューション エクスプローラーのスクリーンショット。[発行] オプションが強調表示されている](./media/documentdb-python-application/image20.png)

2. 資格情報を入力して Azure の web サイトを構成し、 **発行**します。

    ![[Web の発行] ウィンドウのスクリーン ショット](./media/documentdb-python-application/image21.png)

3. Web の発行、数秒で Visual Studio が完了されます。
作成したアプリケーションが Azure で実行されているようすが
確認できます。

## 次のステップ

ご利用ありがとうございます。 最初 Python web アプリケーションを使用して、完了しました
Azure DocumentDB と Azure Websites に発行します。

マイクロソフトでは、このトピックをお客様からのフィードバックに基づいて頻繁に更新、改善しています。  チュートリアルを終了したら、このページの上部と下部にある投票ボタンを使用し、希望される改善内容についてのフィードバックをお寄せください。 マイクロソフトから直接ご連絡を差し上げて問題がなければ、コメント欄に電子メール アドレスをご記入ください。

Web アプリケーションに追加の機能を追加するに入手できる Api を確認、 [DocumentDB Python SDK](https://pypi.python.org/pypi/pydocumentdb)します。

詳細については、次を参照してください。、 [Python デベロッパー センター](/develop/python/)します。


  [Click here to access Flask tutorials]: http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world
  [Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
  [here]: http://aka.ms/ptvs
  [1]: http://go.microsoft.com/fwlink/?linkid=254281&clcid=0x409
  [2]: https://www.python.org/downloads/windows/
  [3]: http://aka.ms/vcpython27
  [Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
  [Azure portal]: http://portal.azure.com

