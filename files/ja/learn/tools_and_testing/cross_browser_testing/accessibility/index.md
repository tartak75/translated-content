---
title: よくあるアクセシビリティの問題を扱う
slug: Learn/Tools_and_testing/Cross_browser_testing/Accessibility
---

{{LearnSidebar}}{{PreviousMenuNext("Learn/Tools_and_testing/Cross_browser_testing/JavaScript","Learn/Tools_and_testing/Cross_browser_testing/Feature_detection", "Learn/Tools_and_testing/Cross_browser_testing")}}

次に、私たちはアクセシビリティに注意を向け、一般的な問題、簡単なテストの方法、そしてアクセシビリティの問題を見つけるための監査/自動化ツールの使い方を説明します。

| 前提条件: | コア [HTML](/ja/docs/Learn/HTML)、[CSS](/ja/docs/Learn/CSS)、および [JavaScript](/ja/docs/Learn/JavaScript) 言語に精通していること。 高水準の[クロスブラウザーテストの原則](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Introduction)の理解。 |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 目的:     | 一般的なアクセシビリティの問題を診断し、それらを修正するための適切なツールとテクニックを使用できるようにすること。                                                                                                                                       |

## アクセシビリティとは？

ウェブ技術のコンテキストでアクセシビリティと言うとき、ほとんどの人は即座にウェブサイトやアプリが障碍のある人にも使えるようにすることを考えます。 例えば、

- 視覚障碍者は、スクリーンリーダーや拡大/ズームを使用してテキストにアクセスします。
- 運動機能障碍を持つ人々は、キーボード（または他のマウス以外の機能）を使用してウェブサイトの機能をアクティブ化します。
- 聴覚障碍を持つ人々は、音声や動画のコンテンツのキャプション/字幕またはその他の代替テキストに依存しています。

しかし、アクセシビリティが単に障碍に関するものであると言うのは間違っています。 実際、アクセシビリティの目的は、高性能デスクトップコンピュータを使用しているユーザーだけでなく、できるだけ多くのコンテキストで、できるだけ多くのユーザーがウェブサイトやアプリを使用できるようにすることです。 極端な例には次のものが含まれます。

- モバイルデバイスのユーザー。
- テレビ、腕時計などの代替ブラウジングデバイスのユーザー。
- 最新のブラウザーを搭載していない可能性がある古いデバイスのユーザー。
- 低速プロセッサを搭載している可能性がある、低スペックデバイスのユーザー。

ある意味では、このモジュール全体がアクセシビリティについてのものです — クロスブラウザーテストは、あなたのサイトができるだけ多くの人々によって使用できることを確認します。[アクセシビリティとは？](/ja/docs/Learn/Accessibility/What_is_accessibility)では、この記事よりも完全かつ徹底的にアクセシビリティを定義します。

とは言っても、この記事では、クロスブラウザーと障碍のある人々を取り巻く問題のテスト、そして彼らのウェブの使い方について説明します。 モジュール内の他の場所で、[レスポンシブデザイン](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/HTML_and_CSS#Responsive_design_problems)と[パフォーマンス](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/JavaScript#Performance_issues)のような他の分野についてはすでに説明しました。

> **Note:** ウェブ開発における多くのことと同様に、アクセシビリティは 100% 成功したかどうかではありません。 特にサイトが複雑になるにつれて、100% のアクセシビリティを全てのコンテンツに対して達成することはほとんど不可能です。 その代わりに、防御的なコーディングを介して、できるだけ多くの人があなたのコンテンツのできるだけ多くにアクセスできるようにし、ベストプラクティスに従うようにする努力をします。

## よくあるアクセシビリティの問題

このセクションでは、従うべきベストプラクティスと共に、特定の技術と結びつけて、ウェブのアクセシビリティに関して生じる主な問題のいくつかと、サイトが正しい方向に進んでいるかどうかを確認するための簡単なテストについて、詳細を説明します。

> **Note:** アクセシビリティは道徳的に正しいことであり、ビジネスには適していますし（多くの障碍のあるユーザー、モバイルデバイスのユーザーなどが重要な市場セグメントを提示しています）、ウェブ資産を障碍のある人々がアクセスできないようにすることは、世界の多くの地域で法律にも違反しています。 詳しくは[アクセシビリティのガイドラインと法律](/ja/docs/Learn/Accessibility/What_is_accessibility#Accessibility_guidelines_and_the_law)を読んでください。

### HTML

意味論的 HTML（要素が正しい目的のために使用されているもの）は箱から出してすぐにアクセスできます — そのようなコンテンツは晴眼者でも読めますし（CSS を使用して、テキストを小さくしすぎたり、隠したりするような愚かなことはしないでください）、スクリーンリーダー（文字通りウェブページをユーザーに読み上げるアプリ）などの支援技術でも使用可能になり、他の利点も付与されます。

#### 意味論的構造

意味論的 HTML で最も重要なすばやい勝利は、コンテンツに見出しと段落の構造を使用することです。 これは、スクリーンリーダーのユーザーが、必要なコンテンツをすばやく見つけるために文書の見出しを道標として使用する傾向があるためです。 あなたのコンテンツが見出しを持っていない場合、彼らが得るのは、何かを見つけるための道標のないテキストの巨大な壁だけです。 悪い HTML と良い HTML の例としては、

```html example-bad
<font size="7">私の見出し</font>
<br><br>
これが私の文書の最初のセクションです。
<br><br>
ここにも段落を追加します。
<br><br>
<font size="5">私の副見出し</font>
<br><br>
これが私の文書の最初のサブセクションです。 私は人々がこのコンテンツを見つけることができるようにしたいです！
<br><br>
<font size="5">私の2番目の副見出し</font>
<br><br>
これは私のコンテンツの2番目のサブセクションです。 私は最後のものよりも面白いと思います。
```

```html example-good
<h1>私の見出し</h1>

<p>これが私の文書の最初のセクションです。</p>

<p>ここにも段落を追加します。</p>

<h2>私の副見出し</h2>

<p>これが私の文書の最初のサブセクションです。 私は人々がこのコンテンツを見つけることができるようにしたいです！</p>

<h2>私の2番目の副見出し</h2>

<p>これは私のコンテンツの2番目のサブセクションです。 私は最後のものよりも面白いと思います。</p>
```

さらに、あなたのコンテンツはそのソース順で論理的に意味があるべきです — あなたは後で CSS を使い、いつでもそれを望んだ所に置くことができますが、最初から正しいソース順を手にするべきです。

テストとして、あなたはサイトの CSS をオフにして、CSS がなければそれがどれほど理解できるか見ることができます。 コードから CSS を取り除くだけでこれを手動で行うことができますが、最も簡単な方法はブラウザー機能を使用することです。 例えば、

- Firefox: メニューバーから \[表示] > \[スタイルシート] > \[スタイルシートを使用しない] を選択します。
- Safari: メインメニューから \[開発] > \[スタイルを無効にする] を選択します（\[開発] メニューを有効にするには、\[Safari] > \[環境設定] > \[詳細] > \[メニューバーに開発メニューを表示] を選択）。
- Chrome: Web Developer Toolbar 拡張機能をインストールしてから、ブラウザーを再起動します。 表示される歯車のアイコンをクリックしてから、\[CSS] > \[全てのスタイルを無効にする] を選択します。
- Edge: メインメニューから \[表示] > \[スタイル] > \[スタイルなし] を選択します。

#### ネイティブなキーボード・アクセシビリティを使う

特定の HTML 機能はキーボードのみを使用して選択できます — これはデフォルトの動作で、ウェブの初期の頃から使用可能です。 この機能を持つ要素は、ユーザーがウェブページと対話することを可能にする一般的なもの、すなわちリンク、{{htmlelement("button")}}、そして {{htmlelement("input")}} のようなフォーム要素です。

[native-keyboard-accessibility.html](http://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html) の例を使ってこれを試すことができます（[ソースコード](https://github.com/mdn/learning-area/blob/master/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html)を見る） — これを新しいタブで開いて、そして <kbd>Tab</kbd> キーを押してみてください。 数回押すと、タブフォーカスがさまざまなフォーカス可能な要素を通過し始めます。 どの要素がフォーカスされているかわかるように、フォーカスされた要素には全てのブラウザーでハイライトされたデフォルトのスタイルが与えられます（ブラウザーによって若干異なります）。

![](button-focused-unfocused.png)

次に <kbd>Enter</kbd> / <kbd>Return</kbd> を押してフォーカスのあるリンクをたどるか、ボタンを押すか（ボタンにメッセージを知らせるための JavaScript が含まれています）、テキスト入力にテキストを入力するためにタイプし始めることができます（他のフォーム要素には異なるコントロールがあります。 例えば、{{htmlelement("select")}} 要素では、<kbd>上下の矢印</kbd>キーを使用してそのオプションを表示したり切り替えたりできます）。

ブラウザーが異なれば、使用可能なキーボードコントロールオプションも異なることに注意してください。 最近のほとんどのブラウザーは上記のタブパターンに従いますが（フォーカス可能な要素を逆方向に移動するために <kbd>Shift</kbd> + <kbd>Tab</kbd> を押すこともできます）、次のようにブラウザーによっては独自の特徴があります。

- Mac 用の Firefox はデフォルトではタブ移動を行いません。 オンにするには、 \[環境設定] > \[詳細] > \[一般] の順に選択してから、\[常にページ内を移動するにはカーソルキーを使用する] のチェックを外します。 次に、Mac のシステム環境設定アプリを開き、\[キーボード] > \[ショートカット] の順に選択して、\[全てのコントロール] ラジオボタンを選択します。
- Safari では、デフォルトではリンクをタブ操作することはできません。 これを有効にするには、Safari の \[環境設定] を開き、\[詳細] に移動し、\[Tab キーを押してウェブページの各項目をハイライトする] チェックボックスをオンにする必要があります。

> **Warning:** あなたが書く新しいページのどれでも、この種のテストを実行するべきです — 機能がキーボードによってアクセスできることを確認してください。

この例では、正しい仕事に正しい意味論的要素を使用することの重要性を強調しています。 任意の要素を、CSS でリンクやボタンのように見せたり、JavaScript でリンクやボタンのように振る舞うようにスタイルすることは可能ですが、実際にはリンクやボタンにはならず、あなたはこれらの要素が無料で与えるアクセシビリティの多くを失うでしょう。 あなたがそれを避けることができるならばしないでください。

もう 1 つのヒント — 次の例に示すように、{{cssxref(":focus")}} 疑似クラスを使用して、フォーカス可能要素のフォーカス時の外観をコントロールできます。 フォーカスとホバーのスタイルを倍増するのは良い考えです。 それにより、マウスやキーボードを使用しているかどうかに関わらず、ユーザーがコントロールをアクティブにしたときに何かが行われるという視覚的な手がかりを得ることができます。

```css
a:hover, input:hover, button:hover, select:hover,
a:focus, input:focus, button:focus, select:focus {
  font-weight: bold;
}
```

> **Note:** CSS を使用してデフォルトのフォーカススタイルを取り除く場合は、デザインに適した他のスタイルに置き換えてください — これは非常に有用なアクセシビリティツールであり、取り除くべきではありません。

#### キーボード・アクセシビリティを組み込む

時にはキーボード・アクセシビリティを失うことが避けられないこともあります。 意味論的にあまり良くないサイトを継承したかもしれませんし（`<div>` で作られたボタンを生成する恐ろしい {{glossary("CMS")}} に行き着くかもしれません）、HTML5 の {{htmlelement("video")}} 要素のようにキーボード・アクセシビリティが組み込まれていない複雑なコントロールを使用しているかもしれません（驚くべきことに、Opera は `<video>` 要素のデフォルトのブラウザーコントロールをタブ操作できる唯一のブラウザーです）。 次のようないくつかの選択肢があります。

1. `<button>` 要素（デフォルトでタブ移動可能）と JavaScript を使用してカスタムコントロールを作成し、それらの機能を関連付けます。 これについての良い例は、[クロスブラウザーのビデオプレーヤーの作成](/ja/docs/Web/Apps/Fundamentals/Audio_and_video_delivery/cross_browser_video_player)を参照してください。
2. JavaScript でキーボードショートカットを作成すると、キーボードの特定のキーを押すことで機能をアクティブにできます。 あらゆる目的に適応できるゲーム関連の例については、[デスクトップのマウスとキーボードのコントロール](/ja/docs/Games/Techniques/Control_mechanisms/Desktop_with_mouse_and_keyboard)を参照してください。
3. 偽のボタンの振る舞いのために、いくつかの興味深い戦術を使ってください。 例えば、[fake-div-buttons.html](http://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html) の例を見てください（[ソースコード](https://github.com/mdn/learning-area/blob/master/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html)を見る）。 ここでは、それぞれの属性に `tabindex="0"` という属性を与えることで（もっと有用な詳細については WebAIM の [tabindex の記事](http://webaim.org/techniques/keyboard/tabindex)（英語）を見てください）、偽の `<div>` ボタンにフォーカスできるようにしました（タブを介すことも含む）。 これにより、ボタンにタブ移動することはできますが、

    <kbd>Enter</kbd>

    &#x20;/&#x20;

    <kbd>Return</kbd>

    &#x20;キーでそれらをアクティブにすることはできません。 そのためには、次のちょっとした JavaScript トリックを追加する必要があります。

    ```js
    document.onkeydown = function(e) {
      if(e.keyCode === 13) { // The Enter/Return key
        document.activeElement.onclick(e);
      }
    };
    ```

    ここでは、`document` オブジェクトにリスナーを追加して、キーボードのボタンが押されたことを検出します。 イベントオブジェクトの [`keyCode`](/ja/docs/Web/API/KeyboardEvent/keyCode) プロパティを使ってどのボタンが押されたかをチェックし、

    <kbd>Return</kbd>

    &#x20;/&#x20;

    <kbd>Enter</kbd>

    &#x20;と一致するキーコードであれば、`document.activeElement.onclick()` を使用してボタンの `onclick` ハンドラに格納されている関数を実行します。 [`activeElement`](/ja/docs/Web/API/Document/activeElement) は現在ページにフォーカスしている要素を与えます。

> **Note:** この手法は、イベントハンドラ・プロパティ（`onclick` など）を使ってオリジナルのイベントハンドラを設定した場合にのみ機能します。 `addEventListener` は機能しません。 これは、機能を再構築するための非常に面倒な作業です。 それに他にも問題があるはずです。 そもそも正しい要素を正しい仕事に使うほうがよいでしょう。

#### 代替テキスト

代替テキストは、アクセシビリティにとって非常に重要です — ある人が視覚障碍または聴覚障碍を抱えているためにコンテンツを見たり聞いたりすることができなくなると、これが問題になります。 最も単純な代替テキストは、控え目な {{htmlattrxref("alt","img")}} 属性で、関連するコンテンツを含む全ての画像に含めるべきです。 これはスクリーンリーダーが拾ってユーザーに読み上げるために、ページ上にその意味と内容をうまく伝える画像の説明を含むべきです。

> **Note:** 詳しくは、[代替テキスト](/ja/docs/Learn/Accessibility/HTML#Text_alternatives)をお読みください。

欠落している代替テキストは、アクセシビリティ[監査ツール](#auditing_tools)を使用するなど、さまざまな方法でテストできます。

代替テキストは、動画と音声のコンテンツにとってはもう少し複雑です。 テキストトラック（字幕など）を定義し、動画の再生時にそれらを {{htmlelement("track")}} 要素と [WebVTT](/ja/docs/Web/API/Web_Video_Text_Tracks_Format) 形式の形式で表示する方法があります（詳細なチュートリアルについては、[HTML5 の動画へのキャプションと字幕の追加](/ja/docs/Web/Apps/Build/Manipulating_media/Adding_captions_and_subtitles_to_HTML5_video)を参照してください）。 これらの機能に対する[ブラウザーの互換性](/ja/Apps/Fundamentals/Audio_and_video_delivery/Adding_captions_and_subtitles_to_HTML5_video#Browser_Compatibility)はかなり良いのですが、音声用の代替テキストを提供したり、古いブラウザーをサポートしたりする場合は、ページのどこかや別のページに提示した単純なテキストトランスクリプトをお勧めします。

#### 要素の関係とコンテキスト

HTML には、他に存在しない要素間のコンテキストと関係を提供するように設計された特定の機能とベストプラクティスがあります。 最も一般的な 3 つの例は、リンク、フォームラベル、およびデータ表です。

アクセス可能なリンクテキストの鍵は、スクリーンリーダーを使用している人々が、ページ上の全てのリンクのリストを引き出すという共通の機能を使用することが多いということです。 この場合、リンクテキストはコンテキスト外で意味を成す必要があります。 例えば、「ここをクリック」、「ここをクリック」などのラベルが付いたリンクのリストは、アクセシビリティにとって本当に悪いものです。 リンクテキストはコンテキスト内でもコンテキスト外でも意味を成すのが得策です。

次に、フォームの {{htmlelement("label")}} 要素は、フォームをアクセス可能にすることを可能にする中心的な機能の 1 つです。 フォームの悩みは、各フォーム入力にどのデータを入力するべきかを示すためにラベルが必要なことです。 各ラベルを {{htmlelement("label")}} 内に含めて相方のフォーム入力に明確にリンクする必要があり（各 `<label>` の `for` 属性値はフォーム要素の `id` 値と一致する必要があります）、ソース順が完全に論理的ではなくても（これは公平であるべきです）、それは意味があります。

> **Note:** リンクテキストとフォームラベルの詳細については、[わかりやすいテキストラベル](/ja/docs/Learn/Accessibility/HTML#Meaningful_text_labels)を参照してください。

最後に、データ表について簡単に説明します。 基本的なデータ表は非常に簡単なマークアップで書くことができますが（bad-table.html の[ライブ](http://mdn.github.io/learning-area/accessibility/html/bad-table.html)と[ソース](https://github.com/mdn/learning-area/blob/master/accessibility/html/bad-table.html)を見る）、問題があります — スクリーンリーダーのユーザーがデータのグループとして行や列を関連付ける方法はありません — これを行うには、ヘッダー行がどれであるか、そしてそれらが行、列などを見出ししているかどうかを知る必要があります。 これはそのような表に対しては視覚的にしかできません。

代わりに punk-bands-complete.html の例（[ライブ](https://mdn.github.io/learning-area/css/styling-boxes/styling-tables/punk-bands-complete.html)、[ソース](https://github.com/mdn/learning-area/blob/master/css/styling-boxes/styling-tables/punk-bands-complete.html)）を見ると、表のヘッダー（{{htmlelement("th")}} と `scope` 属性）、{{htmlelement("caption")}} 要素など、いくつかのアクセシビリティ補助機能が働いていることがわかります。

> **Note:** アクセス可能な表の詳細については、[アクセス可能なデータ表](/ja/docs/Learn/Accessibility/HTML#Accessible_data_tables)を参照してください。

### CSS

CSS は HTML よりもはるかに少ない基本的なアクセシビリティ機能を提供する傾向がありますが、それでも誤って使用された場合、それはアクセシビリティにちょうど同じくらい多くの損害を与えることができます。 CSS に関するアクセシビリティのヒントをいくつかすでに説明しました。

- HTML においてさまざまなコンテンツをマークアップするには、正しい意味論的要素を使用してください。 別の視覚効果を作成したい場合は、CSS を使用します — 必要な外観を得るために HTML 要素を悪用しないでください。 例えば、もっと大きなテキストが必要な場合は、{{htmlelement("h1")}} 要素ではなく {{cssxref("font-size")}} を使用してください。
- ソース順が CSS なしで意味を成すことを確認してください。 常に CSS を使用することで後で好きなようにページをスタイルできます。
- ユーザーに機能に関する視覚的な手がかりを与えるため、ボタンやリンクのようなインタラクティブな要素に適切なフォーカス/ホバー/アクティブ状態が設定されていることを確認するべきです。 スタイル上の理由でデフォルトを取り除いた場合は、代替スタイルが含まれていることを確認してください。

考慮すべき点が他にもいくつかあります。

#### 色とカラーコントラスト

ウェブサイトの配色を選択するときは、テキスト（前景）のカラーコントラストが背景色とよく合うことを確認するべきです。 あなたのデザインはかっこいいかもしれませんが、色覚異常のような視覚障碍を持つ人々がコンテンツを読むことができないならば、それは良くありません。 配色に十分なコントラストがあるかどうかチェックするために WebAIM の[カラーコントラストチェッカー](http://webaim.org/resources/contrastchecker/)（英語）のようなツールを使ってください。

もう 1 つのヒントは、道標や情報を色だけに頼らないようにすることです。 これは、色が見えない人には良くないでしょう。 例えば、必須のフォームフィールドを赤でマークする代わりに、赤いアスタリスクでマークします。

> **Note:** コントラスト比が高いと、光沢のある画面を備えたスマートフォンやタブレットを使用している人は誰でも、日光のような明るい環境にいるときにページを読みやすくなります。

#### コンテンツを隠す

ビジュアルデザインでは、全てのコンテンツを一度に表示する必要がない多くの実例があります。 例えば、[タブ付き情報ボックスの例](http://mdn.github.io/learning-area/css/css-layout/practical-positioning-examples/info-box.html)（[ソースコード](https://github.com/mdn/learning-area/blob/master/css/css-layout/practical-positioning-examples/info-box.html)を見る）には 3 つの情報パネルがありますが、それらを重ねて[配置](/ja/docs/Learn/CSS/CSS_layout/Positioning)し、それぞれを表示するためにクリックできるタブを提供しています（キーボードからもアクセス可能です — 代わりに <kbd>Tab</kbd> と <kbd>Enter</kbd> / <kbd>Return</kbd> を使って選択することもできます）。

![](20191022144107.png)

スクリーンリーダーのユーザーは、このことを気にしません — コンテンツのソース順が意味を成す限り幸せで、全てに到達できます。 絶対配置（この例で使用されているような）は一般に視覚効果のためにコンテンツを隠す最も良いメカニズムの 1 つとして見られます、なぜならそれはスクリーンリーダーがそれに到達するのを止めないからです。

一方で、スクリーンリーダーからコンテンツを隠すので、{{cssxref("visibility")}}`:hidden` や {{cssxref("display")}}`:none` は使用しないでください。 正当な理由があるのでなければ、なぜこのコンテンツをスクリーンリーダーから隠したいのでしょうか。

> **Note:** [スクリーンリーダーのユーザーには見えないコンテンツ](http://webaim.org/techniques/css/invisiblecontent/)（英語）には、このトピックに関するもっと有用な詳細があります。

### JavaScript

JavaScript はアクセシビリティに関して CSS と同じ種類の問題を抱えています — それが悪用されたり、乱用されたりするとアクセシビリティの災害になる可能性があります。 JavaScript に関連するアクセシビリティの問題、主に意味論的 HTML の分野についてはすでに示唆しています — 例えば、リンクやボタンを適切に使用するなど、適切な意味論的 HTML を使用して機能を実装するべきです。 可能であれば、JavaScript コードで `<div>` 要素を使用して機能を偽造しないでください — エラーが発生しやすく、HTML の無料機能を使用するよりも手間がかかります。

#### 単純な機能

一般的に単純な機能は HTML だけで適切に機能するはずです — JavaScript は機能を強化するためにのみ使用されるべきであり、完全には組み込みません。 JavaScript の良い使い方には次のものが含まれます。

- クライアント側のフォーム検証を提供します。 これは、サーバーがデータをチェックするのを待たずに、フォームエントリに関する問題をユーザーにすばやく知らせるものです。 利用できない場合でもフォームは機能しますが、検証が遅くなる可能性があります。
- キーボードのみのユーザーがアクセスできる HTML5 の `<video>` のカスタムコントロールを提供します（前述したように、デフォルトのブラウザーコントロールはほとんどのブラウザーでキーボードからアクセスできません）。

> **Note:** WebAIM の[アクセス可能な JavaScript](http://webaim.org/techniques/javascript/)（英語）は、アクセス可能な JavaScript の考慮事項に関する有用な詳細をいくつか提供します。

より複雑な JavaScript による実装はアクセシビリティに問題をもたらす可能性があります — できる限りのことをする必要があります。 例えば、[WebGL](/ja/docs/Web/API/WebGL_API/Tutorial) を 100% を使用して書かれた複雑な 3D ゲームを視覚障碍者が利用できるようにすることは期待できませんが、マウス以外のユーザーが使用できるように[キーボードコントロール](/ja/docs/Games/Techniques/Control_mechanisms/Desktop_with_mouse_and_keyboard)を実装し、色覚異常のある人にも使えるように配色に十分なコントラストがあるようにすることができます。

#### 複雑な機能

アクセシビリティにとって問題となる主な分野の 1 つは、（日付の選択のような）複雑なフォームコントロールを含む複雑なアプリと、頻繁に増分的に更新される動的コンテンツです。

ネイティブではない複雑なフォームコントロールは、ネストされた `<div>` が多数含まれる傾向があり、ブラウザーがデフォルトでそれらをどう処理するかわからないため、問題があります。 自分でそれらを考案しているのなら、それらがキーボードからアクセスできることを確認する必要があります。 何らかのサードパーティ製フレームワークを使用している場合は、利用可能なオプションを慎重に検討して、飛びつく前にそれらがどれほどアクセス可能かを確認してください。 [Bootstrap](http://getbootstrap.com/) は、アクセシビリティにはかなり適しているように見えます。 例えば、Rhiana Heath による [Bootstrap をもう少しアクセス可能にする](https://www.sitepoint.com/making-bootstrap-accessible/)（英語）では、その問題のいくつかを調べ（主にカラーコントラストに関連した）、いくつかの解決策を検討しています。

定期的に更新される動的コンテンツは、スクリーンリーダーのユーザーがそれを見逃す可能性があるため、特に突然更新される場合は問題になる可能性があります。 [XMLHttpRequest](/ja/docs/Web/API/XMLHttpRequest) または [Fetch](/ja/docs/Web/API/Fetch_API) を使用して定期的に更新されるメインコンテンツパネルを備えた単一ページのアプリがある場合、スクリーンリーダーのユーザーはそれらの更新を見逃す可能性があります。

#### WAI-ARIA

そのような複雑な機能を使用する必要がありますか、それともごく普通の意味論的 HTML が代わりにやりますか？ 複雑なものが必要な場合は、[WAI-ARIA](https://www.w3.org/TR/wai-aria-1.1/)（Accessible Rich Internet Applications）を使用することを検討するべきです。 これは、ほとんどのブラウザーやスクリーンリーダーが理解できる複雑なフォームコントロールや更新パネルのような項目に（新しい HTML 属性の形式で）意味論を提供する仕様です。

複雑なフォームウィジェットを扱うには、さまざまな要素がウィジェット内でどのような役割を担っているか（例えば、タブなのか、タブパネルなのか）を示す `role`、コントロールが無効かどうかを示す `aria-disabled` などの ARIA 属性を使う必要があります。

定期的に更新されるコンテンツのリージョンを扱うには、更新されるリージョンを識別する `aria-live` 属性を使用できます。 その値は、次のようにスクリーンリーダーがどれほど緊急にそれを読み上げるべきかを示します。

- `off`: デフォルト。 更新はアナウンスされるべきではありません。
- `polite`: 更新はユーザーがアイドル状態の場合にのみアナウンスされるべきです。
- `assertive`: 更新はできるだけ早くユーザーにアナウンスされるべきです。
- `rude`: ユーザーに割り込んだとしても、更新はすぐにアナウンスされるべきです。

これが一例です。

```html
<p><span id="LiveRegion1" aria-live="polite" aria-atomic="false"></span></p>
```

Freedom Scientific の [ARIA（Accessible Rich Internet Applications）のライブリージョン](http://www.freedomscientific.com/Training/Surfs-up/AriaLiveRegions.htm)（英語）の例で実行中の例を見ることができます — 強調表示された段落はその内容を 10 秒ごとに更新し、スクリーンリーダーはユーザーにこれを読み上げるべきです。[ARIA のライブリージョン - Atomic](http://www.freedomscientific.com/Training/Surfs-up/AriaLiveRegionsAtomic.htm)（英語）は別の有用な例を提 ​​ 供しています。

ここでは WAI-ARIA を詳細にカバーするためのスペースはありません。 [WAI-ARIA の基本](/ja/docs/Learn/Accessibility/WAI-ARIA_basics)でもっと詳しく学ぶことができます。

## アクセシビリティツール

今まで（キーボードナビゲーションやカラーコントラストチェッカーのような）いくつかのテストのテクニックを含め、さまざまなウェブ技術に対するアクセシビリティの考慮事項について説明してきました。 次に、アクセシビリティのテストを実行するときに使用できる他のツールを見てみましょう。

### 監査ツール

あなたのウェブページを指さすことができる利用可能な監査ツールがいくつかあります。 それらはページを見て、ページに存在するアクセシビリティ問題のリストを返すでしょう。 例えば次のものが含まれます。

- [Tenon](https://tenon.io): 提供された URL でコードを調べて、メトリクス、WCAG 基準が影響を及ぼす WCAG 基準に沿った特定のエラー、および修正案を含むアクセシビリティエラーに関する結果を返す、かなりいいオンラインアプリです。
- [tota11y](http://khan.github.io/tota11y/): Khan Academy のアクセシビリティツールで、JavaScript ライブラリの形式を取り、ページに添付するいくつかのアクセシビリティツールを提供します。
- [Wave](http://wave.webaim.org/): ウェブアドレスを受け入れて、そのページの有用な注釈付きビューを強調表示したアクセシビリティの問題とともに返すオンラインのアクセシビリティテストツールです。

Tenon を使って例を見てみましょう。

1. [Tenon のホームページ](https://tenon.io)（英語）に行きます。
2. [bad-semantics.html](http://mdn.github.io/learning-area/accessibility/html/bad-semantics.html) の例の URL（または分析したい別のウェブページの URL）をページ上部のテキスト入力に入力して \[Analyze Your Webpage] を押します。
3. 次に示すように、error/description のセクションが見つかるまで下にスクロールします。

![](tenon-screenshot.png)

また、Tenon をプログラム的に使用するための API と同様に、探索できるいくつかのオプション（ページ上部の近くにある \[Show Options] リンクを参照）もあります。

> **Note:** このようなツールは、アクセシビリティの問題を全て自分で解決するのに十分ではありません。 全体像を把握するには、これらの組み合わせ、知識と経験、ユーザーテストなどが必要です。

### 自動化ツール

[Deque の aXe ツール](https://www.deque.com/products/axe/)（英語）は、前述した監査ツールよりも少しばかり進化しています。 他のものと同様に、ページをチェックしてアクセシビリティエラーを返します。 その最もすぐに役立つ形式は、おそらく次のブラウザー拡張機能です。

- [Chrome 用の aXe](http://bitly.com/aXe-Chrome)（英語）
- [Firefox 用の aXe](http://bit.ly/aXe-Firefox)（英語）

これらはブラウザー開発者ツールにアクセシビリティタブを追加します。 例えば、Firefox 用のバージョンをインストールし、それを使用して [bad-table.html](http://mdn.github.io/learning-area/accessibility/html/bad-table.html) の例を監査すると、次の結果が得られます。

![](aXe-screenshot.png)

aXe は `npm` を使ってもインストール可能で、[Grunt](http://gruntjs.com/) や [Gulp](http://gulpjs.com/) のようなタスクランナー、[Selenium](http://www.seleniumhq.org/) や [Cucumber](https://cucumber.io/) のような自動化フレームワーク、[Jasmin](http://jasmine.github.io/) のような単体テストフレームワークなどと統合することができます（やはり、詳細については[メインの aXe ページ](https://www.deque.com/products/axe/)（英語）を参照してください）。

### スクリーンリーダー

重度の視覚障碍者がウェブをどのように使用しているかに慣れるには、スクリーンリーダーを使ってテストする価値があります。 次のように利用可能なスクリーンリーダーは多数あります。

- [JAWS](http://www.freedomscientific.com/Products/Blindness/JAWS)（Windows）や [Window Eyes](http://www.gwmicro.com/window-eyes/)（Windows、販売終了）のような有料製品もあります。
- [NVDA](http://www.nvaccess.org/)（Windows）、[ChromeVox](http://www.chromevox.com/)（Chrome、Windows、Mac OS X）、[Orca](https://wiki.gnome.org/Projects/Orca)（Linux）などの無料製品もあります。
- [VoiceOver](http://www.apple.com/accessibility/osx/voiceover/)（Mac OS X、iOS）、[ChromeVox](http://www.chromevox.com/)（Chromebook 上）、および [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) （Android）など、一部はオペレーティングシステムに組み込まれています。

一般的に、スクリーンリーダーはホストオペレーティングシステム上で動作する独立したアプリで、ウェブページだけでなく他のアプリのテキストも読むことができます。 これは必ずしもそうとは限りませんが（ChromeVox はブラウザーの拡張機能です）、通常はそうです。 スクリーンリーダーは少し異なる方法で動作し、異なるコントロールを持つ傾向があるので、全ての詳細を得るためにはあなたが選んだスクリーンリーダーのドキュメントを参照しなければなりません — と言っても、それらは全て基本的に同じような方法で機能します。

いくつかの異なるスクリーンリーダーを使っていくつかのテストを行い、それらがどのように機能するのか、またどのようにテストするのかについての一般的な考えを説明しましょう。

> **Note:** WebAIM の[スクリーンリーダーの互換性のための設計](http://webaim.org/techniques/screenreader/)（英語）では、スクリーンリーダーの使用方法とスクリーンリーダーに最適な機能についての役立つ情報が提供されています。 いくつかの興味深いスクリーンリーダーの使用統計については、[第 6 回スクリーンリーダーのユーザー調査の結果](http://webaim.org/projects/screenreadersurvey6/#used)（英語）も参照してください。

#### VoiceOver

VoiceOver（VO）は Mac / iPhone / iPad には無料で含まれているので、あなたが Apple 製品を使っているならそれはデスクトップ/モバイルでテストするのに役に立ちます。 ここでは、MacBook Pro の Mac OS X でテストします。

オンにするには、<kbd>Cmd</kbd> + <kbd>Fn</kbd> + <kbd>F5</kbd> を押します。 今までに VO を使ったことがない場合は、ようこそ画面が表示され、そこで VO を起動するかどうかを選択できます。 また、使い方を学ぶためにかなり役に立つチュートリアルを実行することもできます。 再びオフにするには、もう一度 <kbd>Cmd</kbd> + <kbd>Fn</kbd> + <kbd>F5</kbd> を押します。

> **Note:** チュートリアルは少なくとも一度は実行するべきです — これは VO を学ぶ上で非常に便利な方法です。

VO がオンになっていると、ディスプレイはほぼ同じに見えますが、画面の左下に、現在選択されている VO に関する情報を含む黒いボックスが表示されます。 現在の選択範囲も黒枠で強調表示されます — この強調表示は **VO カーソル**と呼ばれます。

![](voiceover.png)

VO を使用するには、「VO 修飾キー」を多用します — これは、実際の VO キーボードショートカットに加えて、それらを機能させるために押す必要があるキーまたはキーの組み合わせです。 このような修飾キーを使用するのは、スクリーンリーダーに共通で、他のコマンドとコマンドが衝突しないようにするためです。 VO の場合、修飾キーは <kbd>CapsLock</kbd> または <kbd>Ctrl</kbd> + <kbd>Option</kbd> のいずれかです。

VO にはたくさんのキーボードコマンドがありますので、ここではそれら全てをリストしません。 ウェブページのテストに必要な基本的なものは、次の表のとおりです。 キーボードショートカットでは、<kbd>VO</kbd> は「VoiceOver 修飾キー」を意味します。

| キーボードショートカット                                             | 説明                                                                                                                                                         |
| -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <kbd>VO</kbd> + <kbd>矢印キー</kbd>                                  | VO カーソルを上下左右に移動します。                                                                                                                          |
| <kbd>VO</kbd> + <kbd>スペースバー</kbd>                              | VO カーソルで強調表示されている項目を選択/アクティブ化します。 これには、ローター（下記参照）で選択した項目が含まれます。                                    |
| <kbd>VO</kbd> + <kbd>Shift</kbd> + <kbd>下矢印</kbd>                 | （HTML の表やフォームなどの）項目のグループ内に入ります。 グループ内に入ると、通常どおり上記のコマンドを使用してそのグループ内の項目を移動して選択できます。 |
| <kbd>VO</kbd> + <kbd>Shift</kbd> + <kbd>上矢印</kbd>                 | グループから出ます。                                                                                                                                         |
| <kbd>VO</kbd> + <kbd>C</kbd>                                         | （表内の場合）現在の列のヘッダーを読み上げます。                                                                                                             |
| <kbd>VO</kbd> + <kbd>R</kbd>                                         | （表内の場合）現在の行のヘッダーを読み上げます。                                                                                                             |
| <kbd>VO</kbd> + <kbd>C</kbd> + <kbd>C</kbd>（2 つの連続した C）      | （表内の場合）ヘッダーを含む現在の列全体を読み上げます。                                                                                                     |
| <kbd>VO</kbd> + <kbd>R</kbd> + <kbd>R</kbd>（2 つの連続した R）      | （表内の場合）各セルに対応するヘッダーを含め、現在の行全体を読み上げます。                                                                                   |
| <kbd>VO</kbd> + <kbd>左矢印</kbd>、<kbd>VO</kbd> + <kbd>右矢印</kbd> | （日付の選択や時刻の選択などの一部の水平オプション内の場合）オプション間を移動します。                                                                       |
| <kbd>VO</kbd> + <kbd>上矢印</kbd>、<kbd>VO</kbd> + <kbd>下矢印</kbd> | （日付の選択や時刻の選択などの一部の水平オプション内の場合）現在のオプションを変更します。                                                                   |
| <kbd>VO</kbd> + <kbd>U</kbd>                                         | 簡単にナビゲーションできるように、見出し、リンク、フォームコントロールなどのリストを表示するローターを使用します。                                           |
| <kbd>VO</kbd> + <kbd>左矢印</kbd>、<kbd>VO</kbd> + <kbd>右矢印</kbd> | （ローター内の場合）ローターで利用可能な異なるリスト間を移動します。                                                                                         |
| <kbd>VO</kbd> + <kbd>上矢印</kbd>、<kbd>VO</kbd> + <kbd>下矢印</kbd> | （ローター内の場合）現在のローターリスト内の異なる項目間を移動します。                                                                                       |
| <kbd>Esc</kbd>                                                       | （ローター内の場合）ローターを終了します。                                                                                                                   |
| <kbd>Ctrl</kbd>                                                      | （VO が話している場合）読み上げを一時停止/再開します。                                                                                                       |
| <kbd>VO</kbd> + <kbd>Z</kbd>                                         | 読み上げの最後の部分をやり直します。                                                                                                                         |
| <kbd>VO</kbd> + <kbd>D</kbd>                                         | Mac の Dock に入るので、その中で実行するアプリを選択できます。                                                                                               |

これはたくさんのコマンドのように思えますが、慣れればそれほど悪くはありません。 VO は、特定の場所でどのコマンドを使用するかについて定期的に注意を促します。 今は VO で遊びましょう。 その後、[スクリーンリーダーのテスト](#screenreader_testing)のセクションで例のいくつかを試してみることができます。

#### NVDA

NVDA は Windows 専用で、インストールする必要があります。

1. [nvaccess.org](http://www.nvaccess.org/) からダウンロードしてください。 寄付をするか無料でダウンロードするかを選択できます。 ダウンロードできるようになる前にあなたのメールアドレスを与える必要もあるでしょう。
2. ダウンロードしたら、インストールします — インストーラをダブルクリックし、ライセンスに同意して指示に従います。
3. NVDA を起動するには、プログラムファイル/ショートカットをダブルクリックするか、キーボードショートカットの&#x20;

    <kbd>Ctrl</kbd>

    &#x20;\+&#x20;

    <kbd>Alt</kbd>

    &#x20;\+&#x20;

    <kbd>N</kbd>

    &#x20;を使用します。 起動すると NVDA にようこそダイアログが表示されます。 ここでは、いくつかのオプションから選択し、次に \[OK] ボタンを押して作業を進めます。

これで NVDA はあなたのコンピュータ上でアクティブになります。

NVDA を使用するには、「NVDA 修飾キー」を多用します — これは、実際の NVDA のキーボードショートカットに加えて、それらを機能させるために押す必要があるキーです。 このような修飾キーを使用するのは、スクリーンリーダーに共通で、他のコマンドとコマンドが衝突しないようにするためです。 NVDA の場合、修飾キーは <kbd>Insert</kbd>（デフォルト）、または <kbd>CapsLock</kbd>（\[OK] を押す前に NVDA へようこそダイアログボックスの最初のチェックボックスをオンにして選択できます）のいずれかになります。

> **Note:** NVDA は、VoiceOver よりも、それがどこにあるのか、また何をしているのかを強調する方法という点では微妙です。 あなたが見出しやリストなどをスクロールしているとき、あなたが選択している項目は一般的に微妙なアウトラインでハイライトされますが、これはいつも全てのことに当てはまるわけではありません。 完全に迷子になった場合は、<kbd>Ctrl</kbd> + <kbd>F5</kbd> を押して現在のページを更新し、もう一度上から始めることができます。

NVDA にはたくさんのキーボードコマンドがありますので、ここではそれら全てをリストしません。 ウェブページのテストに必要な基本的なものは、次の表のとおりです。 キーボードショートカットでは、<kbd>NVDA</kbd> は「NVDA 修飾キー」を意味します。

| キーボードショートカット                                                                           | 説明                                                                                                                                 |
| -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| <kbd>NVDA</kbd> + <kbd>Q</kbd>                                                                     | 起動している NVDA を再びオフにします。                                                                                               |
| <kbd>NVDA</kbd> + <kbd>上矢印</kbd>                                                                | 現在行を読み上げます。                                                                                                               |
| <kbd>NVDA</kbd> + <kbd>下矢印</kbd>                                                                | 現在位置から読み上げを始めます。                                                                                                     |
| <kbd>上矢印</kbd> と <kbd>下矢印</kbd>、または <kbd>Shift</kbd> + <kbd>Tab</kbd> と <kbd>Tab</kbd> | ページ内の前/次の項目に移動して、それを読み上げます。                                                                                |
| <kbd>左矢印</kbd> と <kbd>右矢印</kbd>                                                             | 現在の項目の前/次の文字に移動して、それを読み上げます。                                                                              |
| <kbd>Shift</kbd> + <kbd>H</kbd> と <kbd>H</kbd>                                                    | 前/次の見出しに移動して、それを読み上げます。                                                                                        |
| <kbd>Shift</kbd> + <kbd>K</kbd> と <kbd>K</kbd>                                                    | 前/次のリンクに移動して、それを読み上げます。                                                                                        |
| <kbd>Shift</kbd> + <kbd>D</kbd> と <kbd>D</kbd>                                                    | 前/次の文書のランドマーク（`<nav>` など）に移動して、それを読み上げます。                                                            |
| <kbd>Shift</kbd> + <kbd>1</kbd> 〜 <kbd>6</kbd> と <kbd>1</kbd> 〜 <kbd>6</kbd>                    | 前/次の見出し（レベル 1 〜 6）に移動して読み上げます。                                                                               |
| <kbd>Shift</kbd> + <kbd>F</kbd> と <kbd>F</kbd>                                                    | 前/次のフォーム入力に移動して、それにフォーカスを当てます。                                                                          |
| <kbd>Shift</kbd> + <kbd>T</kbd> と <kbd>T</kbd>                                                    | 前/次のデータ表に移動して、それにフォーカスを当てます。                                                                              |
| <kbd>Shift</kbd> + <kbd>B</kbd> と <kbd>B</kbd>                                                    | 前/次のボタンに移動して、ラベルを読み上げます。                                                                                      |
| <kbd>Shift</kbd> + <kbd>L</kbd> と <kbd>L</kbd>                                                    | 前/次のリストに移動して、その最初のリスト項目を読み上げます。                                                                        |
| <kbd>Shift</kbd> + <kbd>I</kbd> と <kbd>I</kbd>                                                    | 前/次のリスト項目に移動して、それを読み上げます。                                                                                    |
| <kbd>Enter</kbd> / <kbd>Return</kbd>                                                               | （リンク/ボタンまたは他のアクティブ化可能項目が選択されている場合）項目をアクティブ化します。                                        |
| <kbd>NVDA</kbd> + <kbd>スペースバー</kbd>                                                          | （フォームが選択されている場合）個々の項目を選択できるようにフォーム内に入るか、すでにフォームに入っている場合はフォームから出ます。 |
| <kbd>Shift</kbd> + <kbd>Tab</kbd> と <kbd>Tab</kbd>                                                | （フォーム内の場合）フォーム入力間を移動します。                                                                                     |
| <kbd>上矢印</kbd> と <kbd>下矢印</kbd>                                                             | （フォーム内の場合）フォーム入力の値を変更します（選択ボックスなどの場合）。                                                         |
| <kbd>スペースバー</kbd>                                                                            | （フォーム内の場合）選択値を選択します。                                                                                             |
| <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>矢印キー</kbd>                                             | （表が選択されている場合）表のセル間を移動します。                                                                                   |

#### スクリーンリーダーのテスト

スクリーンリーダーを使用することに慣れてきましたが、スクリーンリーダーがどのようにウェブページの良い機能と悪い機能に対処するかを理解するために、簡単なアクセシビリティテストを行うためにスクリーンリーダーを使用したいと思います。

- [good-semantics.html](http://mdn.github.io/learning-area/accessibility/html/good-semantics.html) を見て、見出しがスクリーンリーダーによってどのように見つけられ、ナビゲーションに使用できるかに注目してください。 では、[bad-semantics.html](http://mdn.github.io/learning-area/accessibility/html/bad-semantics.html) を見て、スクリーンリーダーがどのようにしてこの情報を取得できないのかに注目してください。 本当に長いページのテキストをナビゲートしようとすると、これがどれほど面倒なことになるか想像してください。
- [good-links.html](http://mdn.github.io/learning-area/accessibility/html/good-links.html) を見て、コンテキストから見たときにそれらがどのように意味をなすかに注目してください。 [bad-links.html](http://mdn.github.io/learning-area/accessibility/html/bad-links.html) ではそうではありません — それらは全てただの「ここをクリック」です。
- [good-form.html](http://mdn.github.io/learning-area/accessibility/html/good-form.html) を見て、`<label>` 要素を適切に使用することで、フォーム入力がラベルでどのように説明されるかに注目してください。 [bad-form.html](http://mdn.github.io/learning-area/accessibility/html/bad-form.html) では、それらは「空白」の行に沿って役に立たないラベルを取得します。
- [punk-bands-complete.html](http://mdn.github.io/learning-area/css/styling-boxes/styling-tables/punk-bands-complete.html) の例を見て、ヘッダーを正しく定義したので、スクリーンリーダーがどのようにしてコンテンツの列と行を関連付け、それらを全て読み取ることができるかを確認します。 [bad-table.html](http://mdn.github.io/learning-area/accessibility/html/bad-table.html) では、どのセルも関連付けることができません。 1 つのページに 1 つの表しかない場合、NVDA はやや奇妙なふるまいをするようです。 代わりに [WebAIM の表のテストのページ](http://webaim.org/articles/nvda/tables.htm)（英語）を試すことができます。
- 先ほど見た [WAI-ARIA のライブリージョンの例](http://www.freedomscientific.com/Training/Surfs-up/AriaLiveRegions.htm)（英語）を見て、スクリーンリーダーが絶えず更新されているセクションが更新されるたびにそれを読み上げていくかに注目してください。

### ユーザーテスト

上記のように、あなたはあなたのサイトのアクセシビリティ問題を決定するために自動化されたツールだけに頼ることはできません。 テスト計画を立てる際には、可能であればアクセシビリティユーザーグループをいくつか含めることをお勧めします（詳細については、コースの前半の[ユーザーテスト](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Testing_strategies#User_testing)のセクションを参照してください）。 必要に応じて、スクリーンリーダーを使用するユーザー、キーボードを使用するユーザー、聴覚を持たないユーザー、および他のグループも参加してください。

## アクセシビリティテストのチェックリスト

次のリストは、プロジェクトで推奨されるアクセシビリティテストを確実に実行したことを確認するためのチェックリストです。

1. HTML が意味論的にできるだけ正しいことを確認してください。 [監査ツール](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Accessibility#Auditing_tools)を使用して、[それを検証する](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/HTML_and_CSS#Validation)ことは良いスタートです。
2. CSS がオフになっているときにコンテンツが意味をなすことを確認してください。
3. 機能が[キーボードからアクセス可能であること](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Accessibility#Using_native_keyboard_accessibility)を確認してください。&#x20;

    <kbd>Tab</kbd>

    、

    <kbd>Return</kbd>

    &#x20;/&#x20;

    <kbd>Enter</kbd>

    &#x20;などを使ってテストします。

4. テキスト以外のコンテンツに[代替テキスト](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Accessibility#Text_alternatives)があることを確認してください。 [監査ツール](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Accessibility#Auditing_tools)はそのような問題を捉えるのに適しています。
5. 適切なチェックツールを使用して、サイトの[カラーコントラスト](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Accessibility#Color_and_color_contrast)が許容範囲内であることを確認してください。
6. [隠されたコンテンツ](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Accessibility#Hiding_content)がスクリーンリーダーに見えるようにしてください。
7. 可能な限り JavaScript がなくても機能が使えることを確認してください。
8. 適切な場合は、ARIA を使用してアクセシビリティを向上させます。
9. [監査ツール](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Accessibility#Auditing_tools)を通してサイトを運営してください。
10. スクリーンリーダーでテストしてください。
11. あなたが行ったことを言うために、あなたのサイトのどこかに見つけることができるアクセシビリティの方針/声明を含めてください。

## 助けを探す

あなたがアクセシビリティに関して遭遇するであろう他の多くの問題があります。 本当に知っておくべき最も重要なことは、オンラインで答えを見つける方法です。 HTML と CSS の記事の[助けを探すのセクション](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/HTML_and_CSS#Finding_help)を参考にしてください。

## まとめ

たぶん、この記事はあなたが遭遇するかもしれない主なアクセシビリティ問題とそれらをどうやってテストして克服するかについての良い下地を与えたでしょう。

次の記事では、機能の検出について詳しく説明します。

{{PreviousMenuNext("Learn/Tools_and_testing/Cross_browser_testing/JavaScript","Learn/Tools_and_testing/Cross_browser_testing/Feature_detection", "Learn/Tools_and_testing/Cross_browser_testing")}}

## このモジュール内の文書

- [はじめてのクロスブラウザーテスト](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Introduction)
- [テスト実行戦略](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Testing_strategies)
- [よくある HTML や CSS の問題を扱う](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/HTML_and_CSS)
- [よくある JavaScript の問題を扱う](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/JavaScript)
- [よくあるアクセシビリティの問題を扱う](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Accessibility)
- [機能検出の実装](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Feature_detection)
- [はじめての自動テスト](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Automated_testing)
- [テスト自動化環境をセットアップする](/ja/docs/Learn/Tools_and_testing/Cross_browser_testing/Your_own_automation_environment)
