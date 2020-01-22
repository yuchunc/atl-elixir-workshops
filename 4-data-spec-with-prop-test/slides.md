footer: Mickey Chen
slidenumbers: true
theme: Next 5

# Data Specification with Norm
## with a sprinkle of Generative Testing
### (aka. Property-based Testing)

---

## 如果隨時覺得我有 #IAmSmarterThenYou
### 決大部分時候我也不是全懂~ XD

^ 第一次用 Norm
還在摸索 Best Practice

---

[.autoscale: true]

# Erlang 已經有 type system 啦 為甚麼還要用另一個 Lib？

---

[.build-lists: true]

- Types 沒辦法符合所有的 use case
- 做一點點事情卻要包一堆 Type Structs
- 整個 System 都需要 Types
- **Dialyzer 是「樂觀型別」**

^ 只有明確知道是錯的才會報錯

---

## 那麼喜歡 Types 去寫 Haskell 就好啦！！😾

---

### 在 BEAM 上的 ML 語言 [Alpaca](https://github.com/alpaca-lang/alpaca/)

![inline](https://avatars2.githubusercontent.com/u/24440856?s=200&v=4)

^
Type System of Haskell
Hindley–Milner (HM) typesystem

---

[.build-lists: true]

## 幾種達到「正確」系統的方式

- Type System
- Testing
- **Design by Contract (DbC)**
- ...etc

---

### DbC 的基本規則

[Hoare Logic](https://en.wikipedia.org/wiki/Hoare_logic)

$$
\{P\}C\{Q\}
$$

^
有很多規則
TDD 的 assertion

---

[.build-lists: true]

# Enters Norm

- Data Specification ~~Only~~
- Can Do Contract Now (YAY)
- Generative Test

---

## Example

```elixir
defmodule Colors do
  use Norm

  def rgb(), do: spec(is_integer() and &(&1 in 0..255))

  def hex(), do: spec(is_binary() and &String.starts_with?(&1, "#"))

  @contract rgb_to_hex(r :: rgb(), g :: rgb(), b :: rgb()) :: hex()
  def rgb_to_hex(r, g, b) do
    # ...
  end
end
```

---

# 用看看吧！

---

## 有沒有很像 Croma 的 `v[Foo.t]`?
![inline](https://www.stickpng.com/assets/images/580b585b2edbce24c47b2a2c.png)

---

## 請不要打我
![inline](https://www.pikpng.com/pngl/b/35-355555_disgusted-meme-face-png-no-rage-face-png.png)

---

[.build-lists: true]

## 幾個很不一樣的地方

- 不用先宣告 Type Strcut
- 可以從資料裡面「挑選」你要比對的資料
- 馬上就能得到價值（Value）
- **把已經宣告的 Data Spec 拿來產生測試資料！**

---

# Generative Testing
### (aka. Property-based Testing)

---

> Writing tests is a waste of time

-- John Hughes

^
很多 FP 的研究
QuickCheck
Introducing HDD - Jose Valim

---

> Writing tests is a waste of time. Generate them!

-- John Hughes

^Example based test 能測得很有限

---

# 應該都看過這類的測試

```elixir
test "multiple complex edge cases" do
  for req <- [...] do
    # lots of complex logic here
    assert Foo.bar(req)
  end
end
```

---

### Don't Write Tests
![inline](https://www.youtube.com/watch?v=hXnS_Xjwk2Y)

---

## Norm with Stream Data

`stream_data` https://github.com/whatyouhide/stream_data

```elixir
input_data = schema(%{"user" => schema(%{"name" => spec(is_binary())})})

property "users can update names" do
  check all input <- gen(input_data) do
    assert :ok == update_user(input)
  end
end
```

---

# Let's ~~Write~~ Generate Some Tests

---

## Remove Contracts in Production

```elixir
config :norm, enable_contracts: Mix.env != :prod
```

---

# Thank you!
