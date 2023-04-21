# Interwiki

これは、SCPの英語版である[SCP Wiki](https://scpwiki.com)が作成したインターウィキモジュールであり、[国際SCP](http://scp-int.wikidot.com/)および[Wanderers' Library](http://wanderers-library.wikidot.com/)コミュニティのすべての支部に対応しています。

このインターウィキモジュールは、中国のSCP支部である[SCP 基金会](http://scp-wiki-cn.wikidot.com/)からforkedされました。元々は[**@7happy7**](https://github.com/7happy7)と[**@Sekai-s**](https://github.com/Sekai-s)によって作成されました。

インターウィキは、国際的な支部間で記事を翻訳するために使用されます。 SCPまたはWLコミュニティ内の任意のページに配置すると、そのコミュニティ他支部から「fullname」/「UNIX名」/ URLが一致するページがリスト表示されます。

CSSを使用してインターウィキをスタイル付けし、適用可能なスタイル数に制限はありません。

# 使用法

Wikidotサイト上でInterwikiを使用する場合：

## ベースInterwikiをインストールする

Interwikiを含むページがどこか知っている必要があります。このページの内容は、その翻訳にリンクするすべてのページに表示される必要があるため、選択肢は限られます。

- SCPサイトでは、通常サイドバーにInterwikiを配置し、すべてのページで表示されるように構成します。規約上はfullname `nav:side` です。
- WLサイトでは、デフォルトのページテンプレート内にインターウィキを配置し、「_template」というfullnameで定義された内容がすべてのページに表示されます。

このページで、[`html/interwikiFrame.html`](html/interwikiFrame.html) ファイルを含むiframeを作成し、現在のページに関する情報を渡す[ListPages module](https://www.wikidot.com/doc-modules:listpages-module) を使用してください。

```Soong
[[div class="scpnet-interwiki-wrapper interwiki-stylable"]]
[[module ListPages range="." limit="1"]]
[[embed]]
<iframe src="//SITE.com/interwikiFrame.html?lang=LANG&community=COMMUNITY&pagename=%%fullname%%" allowtransparency="true" class="html-block-iframe scpnet-interwiki-frame"></iframe>
[[/embed]]
[[/module]]
[[/div]]
```

URL内にいくつかのプレースホルダーがあります：

- **`SITE`:** `interwikiFrame.html` がアップロードされたサイト（UNIX名）。SCP Wikiはこのファイルを[`interwiki.scpwiki.com`](interwiki.scpwiki.com)にアップロードしているため、自分のサイトを用意する必要がない場合は、それを設定するだけで使用できます。
- **`LANG`:** サイトの言語コード。[`js/branches-info-scp.js`](js/branches-info-scp.js) または [`js/branches-info-wl.js`](js/branches-info-wl.js) で定義されています。
- **`COMMUNITY:`** 現在のページが属するコミュニティ。scpまたはwlです。サイトが両方のコミュニティからページをホストする場合は、ListPages を使用して現在のページがどちらに属するか検出することもできます。

SCP Wiki の `interwikiFrame.html`(および `styleFrame.html`) を使用している場合、サイトを用意する必要はありません。

iframe内のURLが`http://`または`https://`ではなく、 `//`で始まっていることに注意してください。これは相対プロトコルURLであり、ユーザーが使用している接続タイプを保持し、サイトがHTTPまたはHTTPS（または両方）を使用するように構成されているかどうかに関係なくInterwikiが動作することを確認します。

ラッピングコンテナ内の `interwiki-stylable`クラスに注目してください。これはCSSテーマにインターウィキ自体のスタイリングが可能であることを示し、外部から近似したスタイル（たとえばCSSフィルターを使用）を適用する必要がないことを示します。 例えばBlack Highlighterなど複数のサイトに対応したテーマでこの新しいInterwiki以外も含めてInterwikisへアクセスする場合に使われます。

<details>
<summary>
<b>ListPagesを使用したコミュニティー別ページ表示例</b>
</summary>

この例では2つのListPagesモジュールおよびカテゴリフィルターを使用して、現在のページがどのコミュニティに属するかを判断します。この具体的な例では、 `wanderers` または `wanderers-adult` カテゴリのページがWLに属し、それ以外はSCPに属すると仮定しています。

カテゴリフィルターは相互排他的であるため、1つのページに対して複数のInterwikiを生成することはありません。

```Soong
[[div class="scpnet-interwiki-wrapper interwiki-stylable"]]
[[module ListPages range="." limit="1" category="-wanderers -wanderers-adult"]]
[[embed]]
<iframe src="//SITE.com/interwikiFrame.html?lang=LANG&community=scp&pagename=%%fullname%%" class="html-block-iframe scpnet-interwiki-frame"></iframe>
[[/embed]]
[[/module]]

[[module ListPages range="." limit="1" category="wanderers wanderers-adult"]]
[[embed]]
<iframe src="//SITE.com/interwikiFrame.html?lang=LANG&community=wl&pagename=%%fullname%%" class="html-block-iframe scpnet-interwiki-frame"></iframe>
[[/embed]]
[[/module]]
[[/div]]
```

</details>

## Interwiki のスタイリング

デフォルトでは、Interwiki は Wikidot のデフォルトスタイリングしか持ちません。さらにスタイリングするには、 `styleFrame` を追加する必要があります。 `styleFrame` は CSS スタイリングを `interwikiFrame` に適用します。

Wikidot ページに `styleFrame` を追加する方法：

```Soong
[[embed]]
<iframe src="//SITE.com/styleFrame.html?priority=PRIORITY&theme=THEME&css=CSS" style="display: none;"></iframe>
[[/embed]]
```

URL 内のいくつかのプレースホルダーを変更する必要があります:

- **`SITE`:** `interwikiFrame.html` がアップロードされたサイト（UNIX名）。SCP Wikiはこのファイルを[`interwiki.scpwiki.com`](interwiki.scpwiki.com)にアップロードしているため、自分のサイトを用意する必要がない場合は、それを設定するだけで使用できます。
- **`PRIORITY`:** この `styleFrame` が適用するスタイリングの優先度。CSS の内部ソート順序に対応します。サイトの基本テーマは優先度0を持つ必要があります。他のどの CSS テーマもそのテーマが拡張するテーマの優先度番号と同じ数値である必要があります。ブラウザコンソールで2つ以上のスタイルが同じ優先度番号でエンカウントされた場合、Interwiki は警告を発生させます。
- **`THEME`:** Interwiki を適用する CSS スタイルシートへのリンク。
- **`CSS`:** 直接 Interwiki に適用する [URI エンコード](https://meyerweb.com/eric/tools/dencoder/) CSS。

`theme` パラメータと `css` パラメータは、 `styleFrame` が追加する CSS スタイリングを決定します。少なくとも1つは存在している必要があります。両方存在する場合、 `theme` のスタイリングは `css` のスタイリングよりも先に追加されます。

`styleFrame` iframe の最後にある `style="display: none;"` に注目してください。これは単純に iframe を非表示にします &mdash; Interwiki のスタイルを設定しません。

### デフォルトスタイルを追加

サイトの Interwiki にデフォルトスタイルを追加する場合、「interwikiFrame」iframeと「styleFrame」が **同じページ** にあります。

たとえば、次の `interwikiFrame` と `styleFrame` が[`interwiki.scpwiki.com`](interwiki.scpwiki.com)下にある場合：

```Soong
[[module ListPages range="." limit="1"]]
[[embed]]
<iframe src="//SITE.com/interwikiFrame.html?lang=en&community=scp&pagename=%%fullname%%" class="html-block-iframe scpnet-interwiki-frame"></iframe>
[[/embed]]
[[/module]]

[[embed]]
<iframe src="//SITE.com/styleFrame.html?priority=0&theme=component:theme" style="display: none;"></iframe>
[[/embed]]
```

### 追加スタイルを追加

（CSSテーマがInterwikiに独自のスタイリングを追加するためなど）追加のスタイルを追加する場合は、別の `styleFrame` をそのテーマに追加し、より高い優先度番号で設定します。

サイト上でホストされている CSS テーマは Wikidot [`[[include]]`](https://www.wikidot.com/doc-wiki-syntax:include) を使用して使用されることが期待されています。これが正しい `priority` 番号を持つすべての CSS テーマが使用されている限り、Interwiki に適用する CSS スタイルシートを含む CSS テーマを `[[include]]`すると、期待どおりにスタイリングが適用されるはずです。

たとえば、SCP Wiki の `theme:my-theme` にある CSS テーマで、すべての CSS を含む単一のコードブロックを持ち、 `[[include theme:my-theme]]` を介して使用することを意図した Interwiki に独自のスタイリングを追加する場合：

```Soong
[[iframe https://SITE.com/styleFrame.html?priority=1&theme=theme:my-theme style="display: none;"]]
```

この例では、テーマ `theme：my-theme` は Sigma-9（サイトの基本テーマであるため優先度番号0）の拡張機能であり、その優先度は1（Sigma-9 の優先度プラス1）です。

### CSS の書き方

Interwiki の内部構造は SCP サイトで使用される標準的なサイドバーと一貫性があるように設計されています。

Interwiki は単一の `html#interwiki body div.side-block` です。 `.heading` 内にはページ左側メニュー項目名が表示されます。各言語版ごとに異なったメニューアイテムが作成されます。オリジナル記事へのリンクではなく翻訳へのリンクである場合、要素には `.original` クラスが付与されます。各 `.menu-item` 内部には Sigma-9 のバレットポイントを含む `img.image` と、翻訳リンク用の `a[href]` があります。

Interwiki 上で垂直スクロールバーが表示されないようにするため、 `html` または `body` に`margin`や`padding`を追加しないでください。

# 開発

[NPM](https://www.npmjs.com/)を使用してローカルにインストールします：`npm install`

フォーマットには[Prettier](https://prettier.io/)を使用します：`npm run format`

ローカルでテストするには、 `npm run serve` を実行し、サーバーのローカルIPを確認し、通常のセットアップ手順に従ってホストされた場所として使用します。ページを更新すると、再コンパイルする必要なく新しい変更が含まれるようにiframeが再読み込みされます。

ビルドするには、「 `npm run build`」を実行します。出力は「 `dist/`」ディレクトリ内で見つけることができます。すべてのファイルがホスティング用に存在している必要があります。

すべてのJavaScriptはES5で書かれている必要があります。