---
title: WritableStream
slug: Web/API/WritableStream
---

{{SeeCompatTable}}{{APIRef("Streams")}}

[Streams API](/ja/docs/Web/API/Streams_API) の **`WritableStream`** インターフェイスは、ストリーミングデータをシンクと呼ばれる宛先に書き込むための標準的な抽象化を提供します。 このオブジェクトには、バックプレッシャーとキューイングが組み込まれています。

## コンストラクター

- {{domxref("WritableStream.WritableStream", "WritableStream()")}}
  - : 新しい `WritableStream` オブジェクトを作成します。

## プロパティ

- {{domxref("WritableStream.locked")}} {{readonlyinline}}
  - : `WritableStream` がライターにロックされているかどうかを示すブール値。

## メソッド

- {{domxref("WritableStream.abort()")}}
  - : ストリームを中止し、プロデューサーがストリームに正常に書き込むことができなくなり、キューに入れられた書き込みが破棄されてすぐにエラー状態に移行することを通知します。
- {{domxref("WritableStream.close()")}}
  - : ストリームを閉じます。
- {{domxref("WritableStream.getWriter()")}}
  - : {{domxref("WritableStreamDefaultWriter")}} の新しいインスタンスを返し、そのインスタンスにストリームをロックします。 ストリームがロックされている間、このライターが開放されるまで他のライターを取得することはできません。

## 例

次の例は、このインターフェイスのいくつかの機能を示しています。 カスタムのシンクと API 提供のキューイング戦略を使用した `WritableStream` の作成を示しています。 次に、`sendMessage()` という関数を呼び出し、新しく作成したストリームと文字列を渡します。 この関数内で、{{domxref("WritableStreamDefaultWriter")}} のインスタンスを返すストリームの `getWriter()` メソッドを呼び出します。 `forEach()` 呼び出しを使用して、文字列の各チャンクをストリームに書き込みます。 最後に、`write()` および `close()` は、チャンクとストリームの成功または失敗に対処するための promise を返します。

```js
const list = document.querySelector('ul');

function sendMessage(message, writableStream) {
  // defaultWriter は WritableStreamDefaultWriter 型です
  const defaultWriter = writableStream.getWriter();
  const encoder = new TextEncoder();
  const encoded = encoder.encode(message, { stream: true });
  encoded.forEach((chunk) => {
    defaultWriter.ready
      .then(() => {
        return defaultWriter.write(chunk);
      })
      .then(() => {
        console.log("Chunk written to sink.");
      })
      .catch((err) => {
        console.log("Chunk error:", err);
      });
  });
  // ライターを閉じる前にすべてのチャンクが
  // 確実に書き込まれるように、ready を再度呼び出します。
  defaultWriter.ready
    .then(() => {
      defaultWriter.close();
    })
    .then(() => {
      console.log("All chunks written");
    })
    .catch((err) => {
      console.log("Stream error:", err);
    });
}

const decoder = new TextDecoder("utf-8");
const queuingStrategy = new CountQueuingStrategy({ highWaterMark: 1 });
let result = "";
const writableStream = new WritableStream({
  // シンクの実装
  write(chunk) {
    return new Promise((resolve, reject) => {
      var buffer = new ArrayBuffer(2);
      var view = new Uint16Array(buffer);
      view[0] = chunk;
      var decoded = decoder.decode(view, { stream: true });
      var listItem = document.createElement('li');
      listItem.textContent = "Chunk decoded: " + decoded;
      list.appendChild(listItem);
      result += decoded;
      resolve();
    });
  },
  close() {
    var listItem = document.createElement('li');
    listItem.textContent = "[MESSAGE RECEIVED] " + result;
    list.appendChild(listItem);
  },
  abort(err) {
    console.log("Sink error:", err);
  }
}, queuingStrategy);

sendMessage("Hello, world.", writableStream);
```

完全なコードは、[単純なライターの例](https://mdn.github.io/dom-examples/streams/simple-writer/)にあります。

### バックプレッシャー

API でのバックプレッシャーのサポート方法により、コードでの実装は明白ではありません。 バックプレッシャーがどのように実装されているかを確認するには、3 つのことを探します。

- カウント戦略の作成時に設定される `highWaterMark` プロパティ（行 35）は、`WritableStream` インスタンスが 1 回の `write()` 操作で処理するデータの最大量を設定します。 この例では、`defaultWriter.write()` に送信できるデータの最大量です（11 行目）。
- `defaultWriter.ready` プロパティは、シンク（`WritableStream` コンストラクターの最初のプロパティ）がデータの書き込みを完了すると解決するプロミスを返します。 データソースは、さらにデータを書き込む（11 行目）か、`close()` （24 行目）を呼び出すことができます。 `close()` の呼び出しが早すぎると、データの書き込みが妨げられる可能性があります。 このため、この例では `defaultWriter.ready` を 2 回呼び出しています（9 行目と 22 行目）。
- シンクの `write()` メソッドによって返される {{jsxref("Promise")}}（40 行目）は、`WritableStream` とそのライターに、いつ `defaultWriter.ready` を解決するかを伝えます。

## 仕様

| 仕様                                                                 | 状態                         | コメント |
| -------------------------------------------------------------------- | ---------------------------- | -------- |
| {{SpecName('Streams','#ws-class','WritableStream')}} | {{Spec2('Streams')}} | 初期定義 |

## ブラウザーの互換性

{{Compat("api.WritableStream")}}

## 関連情報

- [WHATWG Stream Visualiser](https://whatwg-stream-visualizer.glitch.me/): 読み取り可能なストリーム、書き込み可能なストリーム、および変換ストリームの基本的な視覚化。
