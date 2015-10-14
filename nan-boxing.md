NAN-BOXING
==============

---

NaN-boxingの話の前に。。。

---

javascriptの変数を表現する構造を書こう!

---

素朴に実装すると、

```c
enum type { DOUBLE, INT, OBJECT, ...  };

struct jsvalue {
  enum type type;
  union {
    double uDouble;
    int uInt;
    void *uObject;
    ...
  } value;
};
```

---

環境によるけれど少なくとも64ビットより大きいアロケーションが必要

---

# NaN-boxingとは

---

double型のNaNを利用して、変数のアロケーションを64ビットに収めるテクニック

---

# IEEE-754

[浮動小数点数演算標準](https://ja.wikipedia.org/wiki/IEEE_754)

---

NaNは

指数部の各ビットがすべて1で、仮数部が0でない数

---

それってつまり、、、

**doubleの表現する2^52ものパターンがNaNになる！**

---

NaN-boxing実装で考えること

* 計算でNaNになった場合、どのNaNにまとめるか？(正規化)
  * qNaN, sNaNのこと
* NaNの52ビットのうち、型識別のビットをどこに埋め込むか?

---

ちょっと待った！

---

64ビット環境だとポインタのサイズ64ビットだからNaN-boxingできなくね？

---

64ビット環境でもNaN-boxingできる！

ときがあります。

---

* だいたいのCPUとOSでは64ビット環境でも下位48ビットしか使用していないらしい
* なので、52ビットに収まるので、NaN-boxing可能らしい
* （例外として、solarisだと無理らしい）
