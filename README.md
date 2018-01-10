JSON SchemaはJSONベースでJSONデータの構造を定義するフォーマットである、"application/schema+json"というメディアタイプを定義する。
JSON SchemaはJSONドキュメントがどのように見え、情報の抽出方法、及びその操作方法を主張する。
"application/schema-instance+json"メディアタイプは"application/json"文書に可能なものに加え、さらなる機能統合を提供する。

## Note to Readers
この草案のissueリストは < https://github.com/json-schema-org/json-schema-spec/issues > で見ることができる。
追加の情報は、 < http://json-schema.org > で得ることができる。
フィードバックを提供するには、このissueトラッカーか、ホームページに記載の連絡方法、または文書編集者へのemailを使ってほしい。

## Status of This Memo
このInternet-DraftはBCP78とBCP79に準拠する。
Internet-DraftはInternet Engineering Task Force(IETF)の作業文書である。
他のグループはInternet-Draftとして作業文書を配布することもできる。
現在のInternet-Draftの一覧は http://datatracker.ietf.org/drafts/current/ にある。

Internet-Draftは更新されたり置き換えられたり他の文書により廃止されてから最大6ヶ月の間有効である。
参考資料としてInternet-Draftを使用したり、「進行中の作業」以外の場合にInternet-Draftを引用したりするのは不適切である。

このInternet-Draftは2018年5月24日で失効する。

## Copyright Notice
Copyright (c) 2017 IETF Trust and the persons identified as the document authors. All rights
reserved.

This document is subject to BCP 78 and the IETF Trust's Legal Provisions Relating to IETF
Documents (http://trustee.ietf.org/license-info) in effect on the date of publication of this document.
Please review these documents carefully, as they describe your rights and restrictions with respect
to this document. Code Components extracted from this document must include Simplified BSD
License text as described in Section 4.e of the Trust Legal Provisions and are provided without
warranty as described in the Simplified BSD License.

# Introduction
JSON SchemaはJSONデータの構造を定義するためのJSONメディアタイプである。
JSON Schemaはvalidationやdocumentation、hyperlink navigation、相互制御のJSONデータを定義することを意図されている。

この仕様は参照によって他のJSON Schemaを指し示したり、JSON Schemaの参照を解除したりすること、使用される語彙を特定したり
ということを含む、JSON Schemaの中心的な用語やメカニズムを定義する。

他の仕様はvalidationやlinking、annotation、navigation、interactionのための語彙を定義する。

# Conventions and Terminology
この文書において、しなければならない(MUST)、してはならない(MUST NOT)、要求される(REQUIRED)、することになる(SHALL)、
することはない(SHALL NOT)、する必要がある(SHOULD)、しないほうがよい(SHOULD NOT)、推奨される(RECOMMENDED)、してもよい(MAY)、
選択できる(OPTIONAL)のキーワードが使用された場合は、[RFC2119] における定義に沿って解釈されるものとする。

この文書において、JSON、JSONテキスト(JSON text)、JSON値(JSON value)、メンバ(member)、要素(element)、オブジェクト(object)、配列(array)、数値(number)、
文字列(string)、真偽値(boolean)、真値(true)、偽値(false)、nullの語が使用された場合は、[RFC7159]における定義に沿って解釈されるものとする。

# Overview
この文書はJSONデータを記述するためのJSON Schemaを識別するため、新しいメディアタイプである"application/schema+json"を提案する。
また、追加の統合機能を提供するため、オプションのメディアタイプとして"application/schema-instance+json"を提案する。
JSON Schema自体はJSON文書である。
この仕様及び関連する仕様では著者がJSONデータをいくつかの方法で記述するためのキーワードを定義する。

## Validation
JSON SchemaはJSON文書の構造(つまり、必要なプロパティや長さの制限)を記述する。
アプリケーションはこの情報を使用してインスタンスを検証(制約が満たされているかを確認する)したり、制約を満たす様にユーザ入力を集めるよう、
インターフェースに通知したりことができる。

Validationの挙動とキーワードは別のドキュメント[json-schema-validation]に規定されている。

## Hypermedia and Linking
JSON Hyper-SchemaはJSON文書のhypertext構造を記述する。
これはインスタンスから他のリソースへのリンク関係、マルチメディアデータとしてのインスタンスの解釈、
APIを使用するために要求されるデータの送信方法などを含む。

Hyper-schemaの挙動とキーワードは別の文書[json-hyper-schema]に規定されている。

# Definitions
## JSON Document
JSON文書はapplication/jsonメディアタイプで記述された情報リソース(octetの列)である。
JSON Schemaでは、JSON文書(JSON document)、JSONテキスト(JSON text)、JSON値(JSON value)という用語はそれが定義するデータモデルのため互換性がある。

JSON SchemaはJSON文書でのみ定義される。しかし、JSON Schemaのデータモデルに基づいて
解析または処理できる文書またはメモリ構造はCBOR[RFC7049]の様なメディアタイプを含むJSON Schemaとして解釈できる。

## Instance
スキーマが適用されたJSON文書を"instance"と呼ぶ。

### Instance Data Model
JSON Schemaは文書をデータモデルとして解釈する。
このデータモデルに従って解釈されるJSON値は"instance"と呼ばれる。

instanceは6つのprimitive型の内一つを持ち、その値は型による。

* null :: JSONのnull表現
* boolean :: JSONの真値または偽値で表現されるtrueまたはfalse値
* object :: JSONのオブジェクトで表現される順不同な文字列からインスタンスへのマッピング集合
* array :: JSONの配列で表現される順序付きリスト
* number :: JSON数値で表現される任意制度の10進数の値
* string :: JSONの文字列で表現されるUnicodeのコードポイント列

データモデルとして等しい、異なる語彙表現空白や書式の違いはJSON Schemaの範囲外である。
このような字句表現の違いを扱うJSON Schema vocabularies[ボキャブラリ]は元のJSONのUnicode文字列に頼るより、データモデル内の書式設定された文字列を
正確に解釈するためのキーワードを定義するべきである(SHOULD)。

オブジェクトは同じキーに対して二つのプロパティを持つことができないので、
二つのプロパティを一つのオブジェクトの同じキーに定義しようとするJSON文書の挙動は未定義である。

JSON Schema vocabulariesは独自の拡張を持った型システムを定義できることに注意せよ。
これはここで定義されたコアデータモデルの型と混同してはならない。
一例として、"integer"は語彙としてはキーワードの値として定義するのに妥当な型ではあるが、データモデルは整数と他の数とを区別しない。

### Instance Media types
JSON Schemaは"application/json"文書だけではなく、"+json"という構造の構文接尾辞をもったメディアタイプで完全に動作するようデザインされている。
スキーマの操作に役立つ機能の中には、各メディア型、つまりメディア型パラメータとURIフラグメント識別子と意味論により定義されるものがある。
これらの機能はコンテンツネゴシエーションやインスタンスないの特定の場所のURIを計算するのに役に立つ。

この仕様では、インスタンス著者がこれらの目的のため、パラメータとフラグメント識別子を最大限活用できるように
"application/schema-instance+json"メディア型を定義している。

### Instance Equality
同じ型で、データモデルにおいて同じ値を持つ場合に限り、二つのインスタンスは等しいと言う。
特に次の様な場合である。

* 両方ともnullである
* 両方とも真値である
* 両方とも偽値である
* 両方とも文字列で、すべて同じコードポイントの並びである
* 両方とも数値で、数学的に同値である
* 両方とも配列で、すべて同じ要素のならびである
* 両方ともオブジェクトで、それぞれのプロパティがちょうどもう一方のキーと等しいキーを持つただ一つのプロパティを持ち、他のプロパティは等しい値を持つ

この定義で暗黙的に、配列は同じ長さであり、オブジェクトは同じ数のメンバとプロパティを持ち、オブジェクトのプロパティの順序は順不同であり、
同じキーで複数のプロパティを定義する方法はなく、単なる書式の違い(インデントやカンマ、末尾のゼロ)は重要ではない。

## JSON Schema Documents
JSON Schema文書、あるいは単にスキーマはインスタンスを記述するために用いられるJSON文書である。
スキーマはそれ自体がインスタンスとして解釈されるが、"application/schema-instance+json"よりは
"application/schema+json"がメディア型として用いられるべきである(SHOULD)。
"application/schema+json"メディア型は"application/schema-instance+json"によって提供されるメディア型パラメータやフラグメント識別子構文、意味論の
スーパーセットとして定義されている。

### JSON Schema Values and Keywords
JSON Schemaはオブジェクトまたは真偽値でなければならない(MUST)。

インスタンスに適用されるオブジェクトプロパティはキーワードまたはスキーマキーワードと呼ばれる。
広義に、キーワードは次のカテゴリのいずれか、または両方である。

* assertions :: インスタンスに適用された結果、真偽値の結果を生成する
* annotations :: アプリケーションで使用するためにインスタンスに情報を付加する

キーワードはどちらか、または両方のカテゴリに分類されます。この文書および関連文書の外で定義された拡張キーワードはその他の動作も自由に定義できる。

真偽値のスキーマ値である"true"と"false"はインスタンスの値に関係なく常に自分自身を返す単純な表現である。
一例として、真偽値スキーマは以下の挙動と同値である。

* true :: 空スキーマ{}のように常にバリデーションを通過する
* false :: { "not":{} }スキーマのように常にバリデーションに失敗する

JSON Schemaはスキーマキーワードではないプロパティを含んでもよい(MAY)。
未知のキーワードは無視されるべきである(SHOULD)。

空のスキーマとは、プロパティを持たないJSON Schemaまたは未知のプロパティのみを含むJSON Schemaのことである。

### JSON Schema Vocabularies
JSON Schema ボキャブラリは特定の用途のために定義されたキーワードの集合である。
ボキャブラリはassertion、annotation、あるいは任意のボキャブラリで定義されたキーワードカテゴリの意味を指定する。
この文書の二つの関連標準文書はそれぞれボキャブラリを定義する。
1つはインスタンスvalidation、もう一つはハイパーメディアアノテーションである。
ボキャブラリはJSON Schemaメディア型内での拡張性の主なメカニズムである。

ボキャブラリは存在を定義するかもしれない。ボキャブラリの著者は広く使われたり、
他のボキャブラリと併せて使用される可能性がある場合はキーワード名の衝突に気をつけるべきである(SHOULD)。
JSON Schemaはいかなる形式的な名前空間システムを提供しないが、複数の名前空間アプローチを可能にする命名を制約しない。


vacabularyはべつのものからっいーワードの挙動に関するキーワードを定義することで、
あるいは拡張された別のボキャブラリからのキーワードを使用することによって、相互に構築されるかもしれない。
そのようなボキャブラリの再利用のすべてがそれが構築されたボキャブラリと互換性のある新しいボキャブラリを生み出すわけではない。
ボキャブラリの著者はどのレベルの互換性が期待されるのかを明確に文書化すべきである(SHOULD)。

スキーマを記述するスキーマをメタスキーマと呼ぶ。
メタスキーマはJSON Schemaを検証すること、使用するボキャブラリを指定することのために使用される。[CREF1]

### Root Schema and Subschemas
ルートスキーマは問題のJSON文書すべてを構成するスキーマである。

いくつかのキーワードがスキーマそれ自体を取り、JSON Schemaはネスト可能である。

```
{
  "title": "root",
  "items": {
    "title": "array item"
  }
}
```

この例では、"array item"というタイトルのつけられたスキーマがサブスキーマであり、"root"というタイトルのつけられたスキーマがルートスキーマである。

ルートスキーマと同様、サブスキーマはオブジェクトまたは真偽値である。

# Fragment Identifiers
RFC6839の3.1節に従い、+jsonメディア型のフラグメント識別子の構文と意味論は"application/json"型と同様にすべきである(SHOULD)。
(この文書の公開時点では"application/json"のためのフラグメント識別子の定義は存在しない)

加えて、"application/schema+json"メディア型は二つのフラグメント識別子構造をサポートする。
プレーン・ネームとJSON Pointerである。
"application/schema-instance+json"メディア型は一つのフラグメント識別子構造をサポートする。
JSON Pointerである。

JSON PointerのURIフラグメント識別子としての使用方法はRFC6901に記述されている。
二つのフラグメント識別子構文をサポートする"application/schema+json"のため、JSON Pointer構文にマッチするフラグメント識別子、
すなわち空文字列を含むものはJSON Pointerフラグメント識別子として解釈されなければならない(MUST)。

W3Cによるフラグメント識別子に関するベストプラクティス[W2C.WD-fragid-best-practices-20121025]では、
"application/schema+json"のプレーン・ネーム・フラグメント識別子はローカルに指定されたスキーマを参照するために予約されている。
JSON Pointerの構文とマッチしないすべてのフラグメント識別子はプレーン・ネーム・フラグメント識別子として解釈されなければならない(MUST)。

プレーン・ネーム・フラグメント識別子を"application/schema+json"文書内で定義したり参照したりすることは"$id" keywordの節で定義されている。

# General Considerations
## Range of JSON Values
インスタンスはJSON[RFC7159]で定義されたJSON値である。
JSON Schemaは型に制約を課していない。
JSON Schemaは例えば、nullを含んでいるような、いかなるJSON値も記述することができる。

## Programming Language Independence
JSON Schemaはプログラミング言語に非依存で、データモデルに記述されたすべての値をサポートする。
ただし、一部の言語や一部のJSONパーサでは、JSONで記述できる値のすべての範囲をメモリで表現できない場合がある。

## Mathematical Integers
いくつかのプログラミング言語やパーサでは、浮動小数点数の内部表現と整数の内部表現が異なる場合がある。

一貫性のため、JSONの整数値を小数部で符号化すべきではない(SHOULD NOT)。

## Extending JSON Schema
実装はJSON Schemaに追加のキーワードを定義しても良い(MAY)。
明示的な同意がある場合を除いて、、スキーマの作者はこれらの追加キーワードがピア実装によりサポートされていいると期待しない(SHALL NOT)。
実装はサポートしていないキーワードは無視するべきである(SHOULD)。

JSON Schemaへの拡張の作者は"allOf"を使用した既存のメタスキーマを拡張子、独自のメタスキーマを書くことを推奨される。
この拡張されたメタスキーマは、ツールが正しい挙動に従うよう、"$schema"キーワードを使用して参照されるべきである(SHOULD)。

メタスキーマの再帰性は、JSON Hyper-Schemaのメタスキーマに見られるように、拡張されたメタスキーマに再帰キーワードを再定義することを要求することに注意せよ。

# The "$schema" keyword
"$schema"キーワードはJSON Schemaのバージョン識別子と、特定のバー所ののスキーマについて記述されたJSON Schemaそれ自体の場所を示すことの両方に用いられる。

このキーワードの値はURI[RFC3986](スキームを含む)でなければならず(MUST)、このURIは正規化されていなければならない(MUST)。
現在のスキーマはこのURIで特定されるメタスキーマに対して有効でなければならない(MUST)。

もしこのURI識別子が取得可能なリソースであるならば、そのリソースのメディアタイプは"application/schema+json"であるべきである(SHOULD)。

"$schema"キーワードはルートスキーマで使用されるべきである(SHOULD)。サブスキーマでは使用されてはならない(MUST NOT)。

[CREF2]

このプロパティの値は別のドキュメント、あるいは他の人によって定義されている。
JSON Schemaの実装は現在及び以前に発行されたJSON Schema vocabulariyの草案を妥当であるとしてサポートする実装とすべきである(SHOULD)。

# Schema References With "$ref"
"$ref"キーワードはスキーマを参照するために使用され、自己参照によって再帰構造を検証する能力を提供する。

"$ref"キーワードを使用したオブジェクトスキーマは"$ref"参照として解釈されなければならない(MUST)。
"$ref"プロパティの値はURI参照でなければならない(MUST)。
現在のURIベースで解決され、使用するスキーマのURIを識別する。
"$ref"オブジェクトのその他すべてのプロパティは無視されなければならない(MUST)。

URIはネットワーク上の位置を指し示すものではなく、ただの識別子である。
スキーマはそれがネットワークアドレスとして指定できるアドレスだったとしても、スキーマはアドレスからダウンロード可能である必要はなく、
実装はネットワークアドレスとして指定できるURIを見つけたときにもネットワーク操作をすべきと考えるべきではない(SHOULD)。

スキーマはスキーマに対して無限ループを実行してはならない(MUST NOT)。
例えば、二つのスキーマ"#alice"と"#bob"の両方がもう一方を参照する"allOf"プロパティを持っていた場合、
素朴なバリデータはインスタンスを検証しようとして無限再帰ループに陥るだろう。
スキーマはこのような無限再帰ネストを使うべきではない(SHOULD NOT)。
その挙動は未定義である。

# Base URI and Dereferencing
## Initial Base URI
RFC3986の5.1節で、文書の既定のベースURIをどのように決定するかが定義されている。

スキーマの初期ベースURIはそれが見つかったURIか、わからなければ適切な代替URIである。

## The "$id" Keyword
"$id"キーワードはスキーマのURIを定義し、スキーマ内の他のURI参照はそれに対して解決する。
サブスキーマの"$id"はその親スキーマのベースURIに対して解決される。
親が"$id"によって明示的なベースURIを指定していなかった場合、RFC2986の5章に従い決定されるようにベースURIは文書全体である。

もし存在するなら、このキーワードの値は文字列でなければならず(MUST)、有効なURI参照[RFC3986]を表していなければならない(MUST)。
この値は正規化されているべき(SHOULD)で、空フラグメント<#>や空文字列<>であるべきではない(SHOULD NOT)。

JSON Schema文書のルートスキーマはURI(スキームを含む)"$id"キーワードを含んでいるべきである(SHOULD)。
このURIはフラグメントを持たないか、空の文字列であるべきである(SHOULD)。[CREF3]

JSON Schema文書中でサブスキーマに名前をつけるため、サブスキーマは"$id"を使用することで自身に文書ローカルな識別子を与えることができる。
これは"$id"を(JSON Pointerフラグメントではなく)プレーン・ネーム・フラグメントのみからなるURI参照に設定することで行われる。
フラグメント識別子は文字([A-Za-z])で始まり、残りは文字、数字([0-9])、ハイフン("-")、
アンダースコア("_")、コロン(":")、ピリオド(".")からならなければならない(MUST)。

プレーン・ネーム・フラグメントを提供することは、JSON Poiner参照を更新せずにスキーマ中でサブスキーマを移動させることを可能にする。

上記の条件にも合致せず、有効なJSON Pointerでもないフラグメントだけで構成された"$id"URI参照を定義する効果は定義されていない。

```
{
  "$id": "http://example.com/root.json",
  "definitions": {
    "A": { "$id": "#foo" },
    "B": {
      "$id": "other.json",
      "definitions": {
        "X": { "$id": "#bar" },
        "Y": { "$id": "t/inner.json" }
      }
    },
    "C": {
      "$id": "urn:uuid:ee564b8a-7a87-4125-8c96-e9f123d6766f"
    }
  }
}
```

例として、次のURIエンコードされたJSON Pointer[RFC6901] (ルートスキーマに相対)は続くベースURIを持ち、5章に従い、それぞれのURIで識別できる。

* \# (document root) :: http://example.com/rooot.json#
* \#/definitions/A :: http://example.com/root.json#foo
* \#/definitions/B :: http://example.com/other.json
* \#/definitions/B/definitions/X :: http://example.com/other.json#bar
* \#/definitions/B/definitions/Y :: http://example.com/t/inner.json
* \#/definitions/C :: urn:uuid:ee564b8a-7a87-4125-8c96-e9f123d6766f

## Internal References
スキーマは、JSON Pointerや"$id"で直接指定されたURIなど、与えられた任意のURIによって識別できる。

ツールは、"$id"によって提供される、スキーマのURIを記録しておくべきである(SHOULD)。
これを「内部参照」と呼ぶ。

例えば、このスキーマについて考える。

```
{
  "$id": "http://example.net/root.json",
  "items": {
    "type": "array",
    "itema": { "$ref": "#item" }
  },
  "definitions": {
    "single": {
      "$id": "#item",
      "type": "integer"
    }
  }
}
```

実装が <#/definitions/single> を読み込んだとき、それは"$id"URI参照を現在のベースURIを < http://example.net/root.json#item > の形で解決する。

実装が <#/items>スキーマを読み込んだとき、 <#item>参照に遭遇し、これを < http://example.net/root.json#item > へと解決され、
ベースURIに対してフラグメントの解決なしで同じドキュメント内の他の場所で定義されたものとして解釈される。

## External References
莫大なエコシステム内でスキーマ同士を区別するために、スキーマはURIによって識別される。
上記で指定されているように、コレは必ずしもダウンロードされたものを意味するものではないが、代わりに、JSON Schemaの実装は識別するべきURIを含めた、
使用されるスキーマをすでに理解しているべきである(SHOULD)。

実装は任意のURIを任意のスキーマに関連づけることができるべき(SHOULD)であり、
かつ/またはスキーマ内のバリデータの信頼性に依存して、スキーマの"$id"で与えられたURIを自動的に関連づけることができるべきである(SHOULD)。

スキーマは複数のURIを持っても良い(MAY)が、URIは複数のスキーマを区別する方法がない。
複数のスキーマを同じURIで識別しようとした場合、バリデータはエラー状態を発生させるべきである(SHOULD)。

# Comments With "$comment"
このキーワードはスキーマの作者から読者またはスキーマのメンテナへのコメントを書くために予約されている。
このキーワードの値は文字列でなければならない(MUST)。
実装は個の文字列をエンドユーザに表示してはならない(MUST NOT)。
スキーマを編集するツールはこのキーワードを表示したり編集したりする機能をサポートするべきである(SHOULD)。
このキーワードの値は開発者がスキーマを利用するためのデバッグまたエラー出力に使用しても良い(MAY)。
スキーマボキャブラリはボキャブラリのキーワードを含むいかなるオブジェクト中でも"$comment"を許容するべきである(SHOULD)。
実装はそのボキャブラリが特別に禁止しない限り、"$comment"が許容されると仮定しても良い(MAY)。
ボキャブラリはこの仕様で記述されているものを超えて"$comment"の効果を指定してはならない(MUST NOT)。
他のメディア型やプログラミング言語と"application/schema+json"との間で翻訳するツールは、そのメディア型やプログラミング言語のネイティブコメントと"$comment"
から変換してもよい(MAY)。
ネイティブコメントと"$comment"プロパティの両方が総菜する場合のそのような翻訳の挙動は実装依存である。
実装は未知の拡張キーワードと同様、"$comment"を取り扱うべきである(SHOULD)。
それらは、処理の途中いかなる時点で"$comment"を取り除いてもよい(MAY)。
特に、これはデプロイされたスキーマのサイズが懸念される時にスキーマを短くするために許容される。
実装は"$comment"プロパティの存在、不在、あるいは内容によっていかなる他の動作を行ってはならない(MUST NOT)。

# Usage for Hypermedia
JSONは自動化されたAPIやロボットのため、HTTPサーバによって広く利用されている。
この章ではメディア型とWebリンク[RFC8288]をサポートするプロトコルを使用したときに、JSON文書をよりRESTfulな方法で強化する方法について記述する。

## Linking to a Schema
スキーマによって記述されたインスタンスはLink Data Protocol 1.0の8.1節[W3C.REC-ldp-20150226]で定義されているように、
リンク関係"describedby"を用いてダウンロード可能なJSON Schemaへのリンクを提供することが推奨される(RECOMMENDED)。

HTTPでは、そのようなリンクはLinkヘッダ[RFC8288]を使用することで任意のレスポンスに添付することができる。
例としては:

```
Link: <http://example.com/my-hyper-schema#>; rel="describedby"
```

## Identifying a Schema via Media Type Parameter
メディア型は、スキーマに基づき、Content-Typeネゴシエーションを実行する能力をHTTPサーバに与える、"schema"メディア型パラメータを許容しても良い(MAY)。
メディア型パラメータは空白で区切られたURIのリストでなければならない(MUST)(つまり、相対参照は無効である)。

application/schema-instance+jsonメディア型を使用するとき、"schema"パラメータは与えられなければならない(MUST)。

スキーマURIは不透明で、自動的に逆参照されるべきではない(SHOULD)。
実装が提供されたschemaの意味論を理解できない場合、代わりに、スキーマを処理する方法に関する情報を提供しているかもしれない
"describedby"リンクをたどることができる、
"schema"はネットワーク上の場所を指し示す必要がないため、"describedby"関係はダウンロード可能なスキーマへのリンクのため使用される。
しかし、簡単のため、可能であるなら、スキーマの作者はこれらのURIを同じリソースの場所にするべきである。

HTTPでは、メディア型パラメータはContent-Typeヘッダ内で送られるだろう。
```
Content-Type: application/json;
    schema="http://example.com/my-hyper-schema#"
```

複数のスキーマは空白で区切られる。

```
Content-Type: application/json;
    schema="http://example.com/alice http://example.com/bob"
```

[CREF4] HTTPは"schema"をリンク中で送信することもできるが、
メディア型の意味論とContent-Typeネゴシエーションがメディア型のパラメータを完全に置き換えた場合に影響を受ける可能性がある。

```
Link: </alice>;rel="schema", </bob>;rel="schema"
```

## Usage Over HTTP
ネットワーク上のハイパーメディアシステムのために使用する時、HTTP[RFC7231]は分散スキーマのため頻繁に選択されるプロトコルである。
不作法なクライアントは、キャッシュ可能な時間が長く設定されているときでも、ネットワーク越しに必要以上にプルを行い、サーバ管理者に問題を起こす場合がある。

HTTPサーバはJSON Schemaに対して、長期間のキャッシュヘッダを設定すべきである(SHOULD)。
HTTPクライアントはキャッシュヘッダをよく見て、キャッシュが利用可能な期間の間は再リクエストを行わないようにすべきである(SHOULD)。
分散システムでは、共有キャッシュかキャッシュプロキシ、あるいはその両方を使用すべきである(SHOULD)。

```
User-Agent: product-name/5.4.1 so-cool-json-schema/1.0.2 curl/7.43.0
```

クライアントはユーザエージェントヘッダをJSON Schemaの実装かソフトウェア製品に設定、あるいは準備するべきである(SHOULD)。
シンボルは重要度の高い順番にならべられているため、JSON Schemaライブラリの名称あるいはバージョンは
より一般的なHTTPライブラリ(存在する場合)の名称に先行する場合がある。

クライアントは、サーバの運用者が間違った挙動をするスクリプトの持ち主に連絡を取れるよう、リクエストに"From"ヘッダをつけられるようにするべきである(SHOULD)。

# Security Considerations
スキーマとインスタンスはJSONの値である。
それ自体はRFC7159で定義されたセキュリティの考えがすべて適用される。

インスタンスとスキーマはどちらも頻繁に、信用のないサードパーティによってかかれ、公開インターネットサーバにデプロイされる。
バリデータはスキーマを解析する際、過大なリソースを消費しないよう注意すべきである。
バリデータは無限ループに陥ってはならない(MUST NOT)。

サーバは、既存のスキーマや、よく似た"$id"を持つスキーマをアップロードすることで既存のスキーマの機能を変更できないよう注する必要がある。

個別のJSON Schemaボキャブラリはそれぞれのセキュリティ考慮事項を持つことになる。
詳細は各仕様を参照せよ。

スキーマの作者は、悪意のある実装が仕様に反して"$comment"をエンドユーザに表示することがあるため、その内容には注意すべきである。

悪意のあるスキーマの作者は実行可能なコードやその他危険な素材を"$comment"に入れることができるだろう。
実装は"$comment"の内容を解析したり、の内容によって何らかの挙動を行ってはならない(MUST NOT)。

# IANA Considerations
(省略)
