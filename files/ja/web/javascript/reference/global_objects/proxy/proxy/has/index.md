---
title: handler.has()
slug: Web/JavaScript/Reference/Global_Objects/Proxy/Proxy/has
---

{{JSRef}}

**`handler.has()`** は {{jsxref("Operators/in", "in")}} 演算子に対するトラップです。

{{EmbedInteractiveExample("pages/js/proxyhandler-has.html", "taller")}}

## 構文

```
const p = new Proxy(target, {
  has: function(target, prop) {
  }
});
```

### 引数

次の引数は `has()` メソッドに渡されます。 `this` はハンドラーにバインドされます。

- `target`
  - : ターゲットオブジェクトです。
- `prop`
  - : 存在を確認するプロパティ名です。

### 返値

`has` メソッドは真偽値を返さなければなりません。

## 解説

**`handler.has()`** メソッドは {{jsxref("Operators/in", "in")}} 演算子に対するトラップです。

### 介入

このトラップは下記の操作に介入できます。

- プロパティクエリ: `foo in proxy`
- 継承したプロパティクエリ: `foo in Object.create(proxy)`
- `with` チェック: `with(proxy) { (foo); }`
- {{jsxref("Reflect.has()")}}

### 不変条件

以下の不変条件に違反している場合、プロキシは {{jsxref("TypeError")}} を発生します。

- プロパティがターゲットオブジェクトの設定不可の独自プロパティとして存在する場合、存在しないとして報告されてはいけません。
- プロパティがターゲットオブジェクトの独自プロパティとして存在し、そのターゲットオブジェクトが拡張不可の場合、存在しないとして報告されてはいけません。

## 例

### in 演算子のトラップ

次のコードでは {{jsxref("Operators/in", "in")}} 演算子をトラップします。

```js
const p = new Proxy({}, {
  has: function(target, prop) {
    console.log('called: ' + prop);
    return true;
  }
});

console.log('a' in p); // "called: a"
                       // true
```

次のコードでは不変条件に違反します。

```js example-bad
const obj = { a: 10 };
Object.preventExtensions(obj);

const p = new Proxy(obj, {
  has: function(target, prop) {
    return false;
  }
});

'a' in p; // TypeError is thrown
```

## 仕様書

| 仕様書                                                                                                                                                   |
| -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| {{SpecName('ESDraft', '#sec-proxy-object-internal-methods-and-internal-slots-hasproperty-p', '[[HasProperty]]')}} |

## ブラウザーの互換性

{{Compat("javascript.builtins.Proxy.handler.has")}}

## 関連情報

- {{jsxref("Proxy")}}
- {{jsxref("Proxy.handler", "handler")}}
- {{jsxref("Operators/in", "in")}} 演算子
- {{jsxref("Reflect.has()")}}
