# 高度なプロンプトの作成

[![高度なプロンプトの作成](../../images/05-lesson-banner.png)](https://youtu.be/32GBH6BTWZQ)

前章で学んだことを復習しよう：

> プロンプトエンジニアリングとは、より有用な指示や文脈を提供することで、モデルをより適切な回答**に導くプロセスです。

また、プロンプトを書くには2つのステップがあります。1つは適切なコンテキストを提供することによってプロンプトを構成すること、もう1つはプロンプトを徐々に改善する方法である「最適化」です。

ここまでで、プロンプトの書き方についての基本的な理解はできました。この章では、さまざまなプロンプトを試してみることから、あるプロンプトが他のプロンプトよりも優れている理由を理解することに進みます。どの LLM にも適用できる基本的なテクニックに従ってプロンプトを作成する方法を学びます。 

## はじめに

この章では以下のトピックを取り上げる：

- プロンプトにさまざまなテクニックを適用することで、 プロンプトエンジニアリングの知識を広げる。
- 出力を変化させるためのプロンプトの設定。

## 学習目標

このレッスンを修了すると、以下のことができるようになります：

- プロンプトの結果を向上させるプロンプトエンジニアリングのテクニックを適用する。
- プロンプトの結果を改善するプロンプトエンジニアリングのテクニックを適用する。

## プロンプトエンジニアリング

プロンプトエンジニアリングとは、望ましい結果をもたらすプロンプトを作成するプロセスです。プロンプトエンジニアリングには、単にテキストプロンプトを書くだけではありません。プロンプトエンジニアリングは工学的な学問分野ではなく、望ましい結果を得るために適用できる一連のテクニックである。

### プロンプトの例

このような基本的なプロンプトを考えてみましょう：

> 地理に関する問題を10問作成する。

このプロンプトでは、実際には一連の異なるプロンプトテクニックを適用しています。

これを分解してみましょう。

- **コンテクスト**では、「地理」に関する問題であることを指定します。
- **アウトプットを制限する**、10問以下にする。

### 単純なプロンプトの限界

望む結果が得られるかもしれませんし、得られないかもしれません。質問は作成されますが、地理は大きなトピックであるため、次のような理由で望んだ結果が得られないかもしれません：

- **トピックが大きい**、国、首都、河川などについて出題されるかどうかわからない。
- **出題形式**、出題形式が決まっていたらどうしますか？

このように、プロンプトを作成する際には考慮すべきことがたくさんあります。

ここまでは、シンプルなプロンプトの例を見てきたが、生成AIは、さまざまな役割や業界の人々を支援するために、もっと多くのことができる。次に、いくつかの基本的なテクニックを探ってみよう。

### プロンプトのテクニック

まず、プロンプトはLLMの性質であることを理解する必要があります。つまり、プロンプトはモデルに組み込まれた機能ではなく、モデルを使う中で発見されるものなのです。

LLMにプロンプトを出すために使える基本的なテクニックがいくつかあります。それらを探ってみましょう。

- **Few shot prompting**, これはプロンプトの最も基本的な形です。プロンプトは1回だけで、いくつかの例を示します。
- **チェーン・オブ・ソート（Chain-of-thought）**、このタイプのプロンプトは、LLMに問題をステップに分解する方法を伝えます。
- **生成された知識**、プロンプトの応答を向上させるために、プロンプトに加えて生成された事実や知識を提供することができます。
- **Least to most**, 思考の連鎖のように、このテクニックは問題を一連のステップに分解し、そのステップを順番に実行するよう求めるものです。
- **Self-refine**, このテクニックは、LLMのアウトプットを批評し、それを改善するよう求めるものです。
- **自己改善を促す**。ここであなたが望んでいるのは、LLMの答えが正しいことを確認し、答えのさまざまな部分を説明するよう求めることです。これは自己改良の一形態である。

### フューショット・プロンプティング

このスタイルのプロンプティングは非常にシンプルで、1つのプロンプトと、場合によってはいくつかの例題で構成されます。LLMについて学び始めたあなたが使っているのは、おそらくこのテクニックでしょう。以下はその例です：

- プロンプト 「代数学とは何ですか？」
- 答え： 「代数学は数学の一分野であり、数学記号とその記号を操作する規則を研究する。」

### 思考の連鎖 Chain-of-thought

Chain-of-thoughtは、LLMに一連のステップを踏ませるという意味で、非常に興味深いテクニックである。LLMが何かをする方法を理解するように、LLMに指示を与えることである。次の例を、チェーン・オブ・ソートの有無で考えてみよう：

    - プロンプト 「アリスは5個のリンゴを持っていて、3個のリンゴを投げ、2個をボブに渡し、ボブは1個を返します。」
    - 答え 5

LLMは5個と答えますが、これは不正解です。正解は（5 -3 -2 + 1 = 1）という計算から、リンゴ1個です。

では、どうすればLLMにこれを正しく教えることができるだろうか？

思考の連鎖を試してみよう。チェーン・オブ・ソートを使うとは

1. LLMに似たような例を与える。
1. 計算と正しい計算方法を示す。
1. 元のプロンプトを提示する。

以下はその方法です：

- プロンプト 「リサは7個のリンゴを持っていて、1個のリンゴを投げ、4個のリンゴをバートに渡し、バートは1個を返します：
  7 -1 = 6
  6 -4 = 2
  2 +1 = 3  
  アリスは5個のリンゴを持っていて、3個のリンゴを投げ、2個をボブに渡し、ボブは1個を返します。"アリスは何個のリンゴを持っていますか？
  答え 1

別の例、計算、そして元のプロンプトと、いかに長いプロンプトを書くかに注意してください。 

思考の連鎖は非常に強力なテクニックです。

### 生成された知識

プロンプトを作成する場合、自社のデータを使用することがよくあります。プロンプトの一部は自社のもので、もう一部は関心のある実際のプロンプトであるべきです。

例として、あなたが保険業を営んでいる場合、プロンプトはこのようになります：

    ```text
    {{company}}: {{company_name}}
    {{products}}: 
    {{products_list}}
    Please suggest an insurance given the following budget and requirements:
    Budget: {{budget}}
    Requirements: {{requirements}}
    ```

上では、テンプレートを使用してプロンプトがどのように構築されるかを見ている。テンプレート内には `{{variable}}` で示される変数がいくつかあり、これらは企業の API から取得した実際の値に置き換えられます。

変数が企業のコンテンツに置き換えられた後のプロンプトの例を以下に示す：

    ```text
    Insurance company: ACME Insurance
    Insurance products (cost per month): 
    - Car, cheap, 500 USD
    - Car, expensive, 1100 USD 
    - Home, cheap, 600 USD
    - Home, expensive, 1200 USD
    - Life, cheap, 100 USD
    
    Please suggest an insurance given the following budget and requirements:
    Budget: $1000
    Requirements: Car, Home
    ```

このプロンプトをLLMで実行すると、次のような応答が返ってくる：

    ```output
    , and Life insurance
    
    Given the budget and requirements, we suggest the following insurance package from ACME Insurance: 
    - Car, cheap, 500 USD 
    - Home, cheap, 600 USD 
    - Life, cheap, 100 USD 
    Total cost: $1,200 USD
    ```

見ての通り、生命保険も提案されているが、これは提案すべきではない。この結果は、プロンプトを最適化する必要があることを示している。試行錯誤の結果、次のようなプロンプトにたどり着いた：

    ```text
    Insurance company: ACME Insurance
    Insurance products (cost per month): 
    - type: Car, cheap, cost: 500 USD
    - type: Car, expensive, cost: 1100 USD 
    - type: Home, cheap, cost: 600 USD
    - type: Home, expensive, cost: 1200 USD
    - type: Life, cheap, cost: 100 USD
    
    Please suggest an insurance given the following budget and requirements:
    Budget: $1000 restrict choice to types: Car, Home
    ```

type_と_cost_を追加し、_restrict_というキーワードを使うことで、LLMがこちらの要求を理解しやすくなっていることに注目してほしい。

これで次のような応答が得られる：

    ```output
    Given the budget and requirements, we suggest the Car, Cheap insurance product which costs 500 USD per month.
    ```

この例のポイントは、「生成された知識」のような基本的なテクニックを使っていても、望ましい結果を得るためには、ほとんどの場合、プロンプトを最適化する必要があることを示したことだ。

### 最小から最大

Least-to-mostプロンプトのアイデアは、大きな問題をサブ問題に分解することである。そうすることで、LLMに大きな問題を「克服」する方法を教えることができます。データサイエンスの良い例として、LLMに次のように問題を分割するように頼むことができる：

> プロンプト プロンプト：データサイエンスを5つのステップで実行するには？

AIアシスタントは次のように答える：

1. データを集める
1. データをきれいにする
1. データの分析
1. データのプロット
1. データを提示する

### 自己改良、結果の批評

生成AIやLLMでは、出力を信用することはできない。検証する必要がある。結局のところ、LLMは次に言いそうなことを提示しているだけで、何が正しいかを示しているわけではない。したがって、LLMに自分自身を批評してもらうのが良いアイデアだ。

その方法とは、以下のステップを踏むことだ：

1. LLMに問題を解決するよう求める最初のプロンプト
1. LLMが答える
1. あなたは答えを批評し、AIに改善を求める
1. LLMが再び答え、今度は批評を考慮し、思いついた解決策を提案する。

このプロセスは何度でも繰り返すことができます。

このテクニックを使った例を示します：

> プロンプト 「プロンプト：「商品と顧客のルートを持つPythonのWeb APIを作成する。

AIの答え：

    ```python
    import flask
    app = Flask(__name__)
    
    @app.route('/products')
    def products():
        return 'Products'
    
    @app.route('/customers')
    def customers():
        return 'Customers'
    ```

> プロンプト：上記のコードの改善点を3つ提案してください。

AIの答え：

    ```text
    Sure, here are three suggestions to improve the code:
    
    Import the correct module: The first line should import Flask instead of flask. The correct import statement should be from flask import Flask.
    
    Add a main function: Add a if __name__ == '__main__': block to the end of the file to run the application when the file is executed.
    
    Use f-strings: Use f-strings to format the return statements in the route functions. For example, return 'Products' can be changed to return f'This is the {request.path} page'.
    ```

    ```python
    from flask import Flask, request
    
    app = Flask(__name__)
    
    @app.route('/products')
    def products():
        return f'This is the {request.path} page'
    
    @app.route('/customers')
    def customers():
        return f'This is the {request.path} page'
    
    if __name__ == '__main__':
        app.run()
    ```

ご覧のように、上記のAIの回答は、最初の回答を批評したおかげで、最初に提案されたコードを改善している。

### マイユーティック・プロンプティング

Maieuticプロンプトはself-refineに似ているが、LLMに自分自身を説明するよう求めるテクニッ クである。ゴールは、LLMの出力の矛盾を減らし、LLMが正しい答えにたどり着くようにすることである。従うべきワークフローは

1. LLMに質問に対する回答を求める
1. 解答の各部分について、LLMにさらに詳しく説明するよう求める。
1. 矛盾があれば、矛盾している部分を捨てる。

すべての部分に目を通し、答えに納得がいくまで2と3を繰り返します。

以下はプロンプトの例である：

> プロンプト パンデミックを緩和する危機管理計画を5つのステップで作成するには？
LLMの答え

    ```text
    1. リスクを特定する
    1. ステークホルダーを特定する
    1. リソースの特定
    1. 行動を特定する
    1. コミュニケーション・チャネルを特定する
    ```

5つのステップが特定されたが、これが正しいかどうか判断できるだろうか？LLMに各ステップを説明してもらおう：

> プロンプト パンデミックの詳細なリスクは何ですか？

    ```text
    そのリスクとは以下のようなものである：
    
    - 人命の損失
    - 事業の損失
    - 評判の失墜
    - 顧客の喪失
    - 従業員の損失
    ```

この時点で、LLMが正しいことを確認したいので、最初のリスクについてより詳しく説明するようLLMに求め、それが上記の答えと一致していることを望む：

> プロンプト: パンデミック（世界的大流行）において、最大のリスクは何か？

    ```text
    最大のリスクは人命の損失であり、それは守るべき最も重要なものだからだ。
    ```

> パンデミックにおける2大リスクとは？

    ```text
    最大のリスクは、人命の損失と事業の損失である。
    ```

この時点で、LLMは一貫して、「生活」と「ビジネス」を上位2つのリスクとして挙げている。これで次のステップに進むことができ、かなり自信を持つことができる。しかし、LLMを盲目的に信用すべきではない。

## 出力を変える

LLMは本質的に非決定的である。つまり、同じプロンプトを実行するたびに、異なる結果が得られる。例えば次のようなプロンプトを試してみてください：

> "Python Web APIのコードを生成する"

    ```text
    # Import necessary modules
    import flask
    from flask import request, jsonify
    
    # Create a Flask app
    app = flask.Flask(__name__)
    
    # Create a route for the API
    @app.route('/api', methods=['GET'])
    def api():
        # Get the data from the request
        data = request.args
    
        # Process the data
        result = process_data(data)
    
        # Return the result as JSON
        return jsonify(result)
    
    # Function to process the data
    def process_data(data):
        # Do something with the data
        result = data + 1
    
        # Return the result
        return result
    
    # Run the app
    if __name__ == '__main__':
        app.run()
    
    ```

同じプロンプトをもう一度実行すると、少し違った反応が返ってくる：


    ```text
    #import necessary packages
    import flask
    from flask import request, jsonify
    
    #create the Flask application
    app = flask.Flask(__name__)
    app.config["DEBUG"] = True
    
    #create a list of books
    books = [
        {'id': 0,
         'title': 'A Fire Upon the Deep',
         'author': 'Vernor Vinge',
         'first_sentence': 'The coldsleep itself was dreamless.',
         'year_published': '1992'},
        {'id': 1,
         'title': 'The Ones Who Walk Away From Omelas',
         'author': 'Ursula K. Le Guin',
         'first_sentence': 'With a clamor of bells that set the swallows soaring, the Festival of Summer came to the city Omelas, bright-towered by the sea.',
         'published': '1973'},
        {'id': 2,
         'title': 'Dhalgren',
         'author': 'Samuel R. Delany',
         'first_sentence': 'to wound the autumnal city.',
         'published': '1975'}
    ]
    
    #create an endpoint for the API
    @app.route('/', methods=['GET'])
    def home():
        return '''<h1>Books API</h1>
    <p>A prototype API for retrieving books.</p>'''
    
    #create an endpoint to return all books
    @app.route('/api/v1/resources/books/all', methods=['GET'])
    def api_all():
        return jsonify(books)
    
    #create an endpoint to return a single book
    @app.route('/api/v1/resources/books', methods=['GET'])
    def api_id():
        #check if an ID was provided as part of the URL
        #if ID is provided, assign it to a variable
        #if no ID is provided, display an error
        if 'id' in request.args:
            id = int(request.args['id'])
        else:
            return "Error: No id field provided. Please specify an id."
    
        #create an empty list for our results
        results = []
    
        #loop through the data and match results that fit the requested ID
        #IDs are unique, but other fields might return many results
        for book in books:
            if book['id'] == id:
                results.append(book)
    
        #use the jsonify function from Flask to convert our list of
        #Python dictionaries to the JSON format
        return jsonify(results)
    
    app.run()
    ```

> では、出力のばらつきは問題なのか？

何をしようとしているかによる。特定の回答が欲しいのであれば問題です。もし、「地理に関する3つの質問を生成する」のような多様な出力で構わないのであれば、問題ありません。

### 出力を変化させるために温度（temperature）を使う

では、出力をより予測可能なもの、つまりより決定論的なものに制限したいと考えました。どうすればいいのだろう？

温度（temperature）は0から1の間の値で、0が最も決定論的で、1が最も変化に富んでいる。デフォルト値は0.7である。温度（temperature）を0.1に設定して、同じプロンプトを2回実行するとどうなるか見てみよう：

> "Python Web APIのコードを生成する"

    ```python
    #import necessary libraries
    import flask
    from flask import request, jsonify
    
    #create an instance of the Flask class
    app = flask.Flask(__name__)
    
    #create an endpoint for the API
    @app.route('/api/v1/endpoint', methods=['GET'])
    def api_endpoint():
        #get the request data
        data = request.get_json()
    
        #process the data
        result = process_data(data)
    
        #return the result
        return jsonify(result)
    
    #function to process the data
    def process_data(data):
        #process the data
        result = {'result': 'success'}
    
        #return the result
        return result
    
    #run the Flask app
    if __name__ == '__main__':
        app.run()
    
    ```

もう一度プロンプトを実行すると、このような結果になる：

    ```python
    #import necessary libraries
    import flask
    from flask import request, jsonify
    
    #create a Flask app
    app = flask.Flask(__name__)
    
    #create a route for the API
    @app.route('/api', methods=['GET'])
    def api_call():
        #get the data from the request
        data = request.get_json()
        
        #process the data
        result = process_data(data)
        
        #return the result
        return jsonify(result)
    
    #define the process_data function
    def process_data(data):
        #do something with the data
        result = data + 1
        
        #return the result
        return result
    
    #run the app
    if __name__ == '__main__':
        app.run()
    
    ```

この2つの出力にはわずかな差しかない。 今度は逆に、温度（temperature）を0.9に設定してみよう：

    ```python
    # Import necessary libraries
    import flask
    from flask import request, jsonify
    
    # Create a Flask app
    app = flask.Flask(__name__)
    
    # Create a route for the API
    @app.route('/api', methods=['GET'])
    def api_call():
        # Get the data from the request
        data = request.args
    
        # Process the data
        result = process_data(data)
    
        # Return the result
        return jsonify(result)
    
    # Function to process the data
    def process_data(data):
        # Do something with the data
        result = data + 1
    
        # Return the result
        return result
    
    # Run the app
    if __name__ == '__main__':
        app.run()
    
    ```

そして2回目の試みは、温度（temperature）として0.9を設定した：

    ```python
    import flask
    from flask import request, jsonify
    
    # create the Flask app
    app = flask.Flask(__name__)
    app.config['DEBUG'] = True
    
    # create some test data
    books = [
        {'id': 0, 'title': 'A Fire Upon The Deep', 'author': 'Vernor Vinge', 'first_sentence': 'The coldsleep itself was dreamless.', 'year_published': '1992'},
        {'id': 1, 'title': 'The Ones Who Walk Away From Omelas', 'author': 'Ursula K. Le Guin', 'first_sentence': 'With a clamor of bells that set the swallows soaring, the Festival of Summer came to the city Omelas, bright-towered by the sea.', 'published': '1973'},
        {'id': 2, 'title': 'Dhalgren', 'author': 'Samuel R. Delany', 'first_sentence': 'to wound the autumnal city.', 'published': '1975'}
    ]
    
    # create an endpoint
    @app.route('/', methods=['GET'])
    def home():
        return '''<h1>Welcome to our book API!</h1>'''
    
    @app.route('/api/v1/resources/books
    
    ```

ご覧のように、結果はこれ以上ないほど多様である。

> top-k、top-p、繰り返しペナルティ、長さペナルティ、多様性ペナルティのように、出力を変化させるために変更できるパラメータはもっとありますが、これらはこのカリキュラムの範囲外であることに注意してください。  

## グッドプラクティス

あなたが望むものを得るために適用できるプラクティスはたくさんある。プロンプトをどんどん使っていくうちに、自分なりのスタイルが見つかるでしょう。

これまで取り上げてきたテクニックに加えて、LLMにプロンプトを出すときに考慮すべきグッドプラクティスがいくつかあります。

以下がその例です：

- **文脈を明示する**。文脈は重要であり、ドメインやトピックなど、指定できることが多ければ多いほどよい。
- 出力を制限する。特定の項目数や長さが必要なら、それを指定する。
- **何を、どのように**の両方を指定する。例えば、"商品と顧客をルーティングしたPython Web APIを作成し、3つのファイルに分割する "といったように、何が欲しいのか、どのように欲しいのかの両方を記載することを忘れないでください。
- テンプレートを使いましょう。多くの場合、プロンプトをあなたの会社のデータでリッチにしたいと思うでしょう。そのためにはテンプレートを使います。テンプレートには、実際のデータに置き換える変数を含めることができます。
- スペルを正しく入力する。LLMは正しい回答を提供するかもしれませんが、スペルを正しく書けば、より良い回答が得られます。 

## 課題

Pythonで、Flaskを使って簡単なAPIを構築するコードを示します：

    ```python
    from flask import Flask, request
    
    app = Flask(__name__)
    
    @app.route('/')
    def hello():
        name = request.args.get('name', 'World')
        return f'Hello, {name}!'
    
    if __name__ == '__main__':
        app.run()
    ```

GitHub CopilotやChatGPTのようなAIアシスタントを使い、「セルフリファイン」のテクニックを適用してコードを改善する。

## 解答

コードに適切なプロンプトを追加して、課題の解決を試みてください。

> ヒント
> プロンプトをフレーズにして改善を求めましょう。例えば、アーキテクチャ、パフォーマンス、セキュリティなどです。

[ソリューション](../../solution.py)

## 知識チェック

なぜ思考連鎖プロンプトを使うのか？1つの正解と2つの不正解を示してください。

1. LLMに問題の解き方を教えるため。
1. B, LLMにコードの誤りを見つけることを教えるため。
1. C, LLMにさまざまな解決策を思いつくように指示する。

A: 1。思考の連鎖とは、LLMに一連の手順や類似の問題とその解き方を提供することで、LLMに問題の解き方を示すことだから。

## 🚀チャレンジ

あなたは課題の中で自己洗練のテクニックを使ったばかりです。あなたが作ったプログラムの中から、どのような改良を加えたいかを考えてみてください。次に、提案された変更を適用するために、セルフ・リファイン技法を使いま しょう。その結果、良くなったと思いますか、悪くなったと思いますか？

## よくやった！学習を続ける

このレッスンを終えたら、[生成AI学習コレクション](https://aka.ms/genai-collection)をチェックして、生成AI知識のレベルアップを続けましょう！

レッスン6では、[テキスト生成アプリを作る](../../../06-text-generation-apps/translations/ja/README.md)によって、プロンプトエンジニアリングの知識を応用します。
