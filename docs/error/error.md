# 誤差

## 桁落ち

値がほぼ等しい数値の減算によって有効桁数減少することを **桁落ち** と呼びます。

以下のプログラムを実行してみましょう。

```java
void setup() {
  noLoop();
}

void draw() {
  float a = 12345.7;
  float b = 12345.6;
  printBits(a);
  printBits(b);
  printBits(a - b);

  float c = 0.6;
  printBits(c);
  printBits(a - c);
}

void printBits(float v) {
  int bits = Float.floatToIntBits(v);

  int s = bits < 0 ? 1 : 0;
  int e = (bits & 0x7f800000) >> 23;
  int f = bits & 0x007fffff;

  println(s, hex(e), hex(f), v);
}
```

実行結果は以下のようになります。

```console
0 0000008C 0040E6CD 12345.7
0 0000008C 0040E666 12345.6
0 0000007B 004E0000 0.10058594
0 0000007E 0019999A 0.6
0 0000008C 0040E467 12345.101
```

a - c では指数部の値が保たれているのに対して、a - b のときは指数部の値が小さくなり、有効桁数が減っていることが確認できます。

## 情報落ち

絶対値の大きい数値と小さい数値の加減算をした場合に生じる誤差を **情報落ち** と呼びます。
情報落ちでは絶対値が小さい数値が正しく演算結果に反映されません。

以下のプログラムを実行してみましょう。

```java
float a = 1e10;
float b = 1e5;
float c = 1e1;
println("a + b = " + (a + b));
println("a + c = " + (a + c));
```

実行結果は以下の通りになります。

```console
a + b = 1.00001004E10
a + c = 1.0E10
```

a + c では絶対値の小さい c の値が加算に反映されていません。

## 丸め誤差

丸め誤差は、実数を有限の桁数（bit 数）で表す際に、表現可能な桁数を超えた部分を四捨五入や切り上げ切り捨てによって丸めたことによって生じる誤差です。

前述の通り、0.1 は丸め誤差を生じます。

## 打ち切り誤差

**打ち切り誤差** は、無限級数のように表される数を有限回の繰り返しで打ち切ることにより生じる誤差です。
数値計算アルゴリズムの設計に大きく関わります。

## オーバーフロー・アンダーフロー

演算結果が表現可能な数値の範囲を超えた場合に誤差が生じます。
数値の絶対値が表現可能な最大値を超えた場合に **オーバーフロー**、最小値を下回った場合に **アンダーフロー** と呼びます。
