# spring-bootcamp

https://start.spring.io/#!type=gradle-project&language=kotlin&platformVersion=2.5.5&packaging=jar&jvmVersion=11&groupId=cypher&artifactId=spring-bootcamp&name=spring-bootcamp&description=Demo%20project%20for%20Spring%20Boot&packageName=cypher.spring-bootcamp&dependencies=web をもとに開始

# 課題

### 1.  Hello World API

\1.   Hello Worldを返却するAPI。Spring Bootの基礎となる部分です。

### 2.  Book API

\1.   書籍管理API。CRUDの勉強用です。1 が終わったらこちらに進んでください。

# Hello World API

| **No** | **課題**                               | **やること**                                                 |
| ------ | -------------------------------------- | ------------------------------------------------------------ |
| 1      | Hello World                            | "Hello       World" を Stringで返却するAPIを作成してください   Spring       Initializrを利用して作成してください。ブラウザでDownloadしてもIDEの機能を利用してもOKです   Project名称はお好きなもので |
| 2      | Hello World API                        | requestで name を受け取り、例の通りのResponseをJsonで返却してください。        例: {"message":"Hello,       $name"} |
| 3      | 設定していない PathへのRequestへの対応 | 設定していないPathにリクエストが来たときにHTTP Status 404でResponseをJsonで返却してください。        例: {"reason":"no handler       found"} |
| 4      | Exception 対応                         | Applicationで例外が発生した際、HTTP Status 500でJsonを返却してください。        例: {"reason":"something       wrong ;-("} |
| 5      | Validation                             | nameに「3文字以上10文字以内」のValidationを設定する   ValidationExceptionが発生した際、HTTP Status 400でResponseをJsonで返却してください。        例: {"reason":"invalid parameter:       detail: [[hello.name](http://hello.name): 3 から 10 の間のサイズにしてください]"} |
| 6      | Spring Profilesごとに定数を定義する    | APIのResponseを {"message":"Hello       $text, $name"} に修正してください   textの値はapplication.ymlに定義してApplication内で利用してください   spring       profilesがdevのときはtextの値を変えてください              spring        profiles dev以外の場合の出力: {"message":"Hello        my best friend, foo"}    spring        profiles がdevの場合の出力: {"message":"Hello        my friend, foo"} |
| 7      | Debug Log                              | Applicationにdebug logを仕込み、request nameをlog出力してください   自身の開発パッゲージにのみ設定するようにしてください (他のLibrary, 例えばspringframeworkのdebug logは出力しない)   spring       profilesがdevのときにだけdebug       logを出力するようにしてください |
| 8      | Kotlin Code Style                      | プロジェクトのKotlin formatをKotlin Code Styleにしてください (gradle.propertiesの設定) |
| 9      | その他                                 | graceful       shutdown対応をしてください   healthcheck対応をしてください |



# Book API

| **No**                                                | **課題**                     | **やること**                                                 |
| ----------------------------------------------------- | ---------------------------- | ------------------------------------------------------------ |
| 1                                                     | Databaseの準備               | docker-composeでpostgresqlを用意してください        docker-composeを利用します (dockerが難しい場合は模範解答を提供します)   起動時にtable createと初期データをセットしてください    3.    table 定義 (schema名はなんでも🙆‍♂️)  ![img](file:////Users/abi01384/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)ここをクリックすると展開されます...   `CREATE TABLE main.book``(``  isbn    varchar(13) NOT NULL,``  title   varchar(100) NOT NULL,``  author   varchar(100) NOT NULL,``  publisher varchar(100) NOT NULL,``  price   integer   NOT NULL,``  created_at timestamp  NOT NULL,``  updated_at timestamp  NOT NULL,``  PRIMARY KEY (isbn)``)` |
| 2                                                     | List 実装                    | Databaseに登録されているデータをJsonで返却するlist処理を実装してください   Database       Accessには[JdbcTemplate](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/jdbc/core/JdbcTemplate.html)を使ってください   Databaseへの接続パスワードは環境変数で設定できるようにしてください   データが存在しない場合は空List Jsonを返却してください。HTTP Statusは200を返却してください   Database接続等の例外が発生した場合は HTTP Status 500でJsonを返却してください    6.    実装イメージ (一部省略) この通りに作らなくてもOKです     ![img](file:////Users/abi01384/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)ここをクリックすると展開されます...   `└── src``  └── main``    └── kotlin``      └── com``        └── example``          └── app``            ├── ExampleApplication.kt``            ├── repository``            │  └── BookRepository.kt (JdbcTemplate実装を記述する)``            └── web``              ├── controller``              │  └── BookController.kt``              └── entity``                 └── Book.kt` |
| 3                                                     | Clean Architecture           | gradleをマルチプロジェクト構成にしてアプリケーションをクリーンアーキテクチャにしてください (難しい場合はヘルプします)   DatabaseとのコミュニケーションとController Responseはdomain objectを使わずにそれぞれdtoを利用してください   構成        web        (Controller / SpringBootApplication起動クラス)    domain        (domain object)    infra        (databaseとのコミュニケーション)    application        (usecase / infraのinterface)       4.    実装イメージ (一部省略 / class名などは適宜変更してください)   ![img](file:////Users/abi01384/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)ここをクリックすると展開されます...   `root``├── application``│  ├── build.gradle.kts``│  └── src``│    └── main``│      └── kotlin``│        └── com``│          └── example``│            └── app``│              └── application``│                ├── repository``│                │  └── BookRepository.kt (BookRepositoryImplのInterface)``│                └── usecase``│                  └── FetchBook.kt (usecase. Controllerで利用する)``├── build.gradle.kts``├── domain``│  ├── build.gradle.kts``│  └── src.main.kotlin.com.example.app.domain.entity.Book.kt``│``├── infra``│  ├── build.gradle.kts``│  └── src``│    └── main``│      └── kotlin``│        └── com``│          └── example``│            └── app``│              └── infra``│                ├── entity``│                │  └── Book.kt (Database用のDTO)``│                └── repository``│                  └── BookRepositoryImpl.kt``├── settings.gradle.kts``└── web``  ├── build.gradle.kts``  └── src``    └── main``      ├── kotlin``      │  └── com``      │    └── example``      │      └── app``      │        ├── ExampleApplication.kt``      │        └── web``      │          ├── controller``      │          │  ├── BookController.kt``      │          └── response``      │            ├── BookResponse.kt (Response用のDTO)``      │            └── ErrorResponse.kt``      └── resources``        └── application.yml` |
| 4                                                     | MyBatis 導入                 | JdbcTemplateの実装をMyBatisに変更してください                |
| 5                                                     | Get 実装                     | isbnを引数で受け取ってBookResponseを返却する処理を実装してください   isbnは13桁の数字だけvalidとしてください   dataが見つからない場合は HTTP status 404にして次のようなJSONを返却してください。        例: {"reason":"No Book found.       isbn=9784111111111"} |
| 6                                                     | Value Object                 | isbnのvalue objectを作成してください。換言すると、domain       layerにIsbn Classを作ってください、という意味です   5.       で作成したGet処理のリクエストisbn に、チェックディジット計算のチェックを導入してください。        チェックディジットが不正の場合は HTTP status 400にしてJsonを返却してください   isbnのチェックディジット計算仕様は適当に検索して拾ってください。[例えばここ](https://isbn-information.com/check-digit-for-the-13-digit-isbn.html)とか   チェックディジット計算のfunctionはJUnitで単体テストを書いてください |
| 7                                                     | Database例外ハンドリング     | List,       Get処理でDatabaseAccessExceptionが発生した場合の処理を実装してください   HTTP       status 500にしてJSONを返却してください。        例: {"reason":"BookRepository#findでエラーが発生しました"} |
| 8                                                     | Insert 実装                  | 新規の書籍を登録する処理を自由に実装してください             |
| 9                                                     | Update 実装                  | 書籍情報を更新する処理を自由に実装してください               |
| 10                                                    | Request Logger 実装          | Requestで受け取った情報 (Request Header, Method, URI, Body) をdebug logに出力してください |
| 11                                                    | custom validator             | publisherが       「ほげほげ書房」 のとき、priice < 1000 の商品は登録、更新できないようにしてください              このvalidateはWeb Layerに実装してください |
| 12                                                    | gzip response                | APIのResponseをgzip 圧縮してください                         |
| 13                                                    | API Call                     | Get時にBookのRatingとGenreを返すAPIをCallしてレスポンスに追加してください        APIは自作してもMockを用意してもかまいません    APIはISBNを受け取り、Rating, Genreを返します。例: { "rating": "4.5", "genre":        "少年コミック" }    API        Callに失敗した場合、rating, genreをつけずに従来のGet処理のResponseを返すようにしてください |
| **おまけのチャレンジ課題 (これらはレビューしません)** |                              |                                                              |
| 1                                                     | aws s3にcsv fileをuploadする | listの結果をcsvにしてs3にuploadしてください   s3はlocalstackを利用してください |
| 2                                                     | Response Logger実装          | Responseをdebug logに出力してください                        |
| 3                                                     | 認証認可                     | Insert,       Update処理に対して指定されたユーザ以外は処理を行えないようなセキュリティ機構を実装してください        実装、処理方法、利用するリソースはすべて自由です        🙆‍♂️ |
| 4                                                     | 並列実行                     | Get時のAPI CallとDB処理を並列実行してください   この時点ではDB処理はBlocking IOになります。APIはNon-BlockingでもBlockingでも 🙆‍♂️ |
| 5                                                     | WebFlux                      | 課題で作ったAPIをWebFluxで実装し直します        [BlockHound](https://github.com/reactor/BlockHound) を入れて、エラーにならなければ        🙆‍♂️    Controllerはアノテーション形式でもfunction形式でも🙆‍♂️ |



