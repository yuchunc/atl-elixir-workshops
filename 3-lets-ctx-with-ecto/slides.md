footer: Mickey Chen
slidenumbers: true
theme: Courier, 3

# Let's Context w/ Ecto

---

### 上次忘了講的部分：action_fallback

```elixir
defmodule MyController do
  use Phoenix.Controller

  action_fallback MyFallbackController

  def show(conn, %{"id" => id}, current_user) do
    with {:ok, post} <- Blog.fetch_post(id),
         :ok <- Authorizer.authorize(current_user, :view, post) do

      render(conn, "show.json", post: post)
    end
  end
end

```

---

```elixir
defmodule MyFallbackController do
  use Phoenix.Controller

  def call(conn, {:error, :not_found}) do
    conn
    |> put_status(:not_found)
    |> put_view(MyErrorView)
    |> render(:"404")
  end

  def call(conn, {:error, :unauthorized}) do
    conn
    |> put_status(403)
    |> put_view(MyErrorView)
    |> render(:"403")
  end
end
```

---

## Goals

- TDD Our Contexts
- Create Business Logic
- Ecto in Context

---

# No Slides, just hands-on!!

![inline](https://media.giphy.com/media/3ogwFMaJh9SIPOz1Cw/giphy.gif)

---

## Mob Programming

![inline](https://www.youtube.com/watch?v=jhImlR4_Vto)

---

### Mob Programming = Work on Working Together

---

## 我們先以這些規則開始

- 一次一個人 code
- 其他人請把電腦關起來

^
15 min cycle
查資料除外

---

## From last time...

---
[.autoscale: true]

### Spec, adjusted...

- ~~大廳~~
- 多人聊天室
- 跨聊天室推播
- 使用者 CRUD
- Chat Filtering
- 回覆訊息 (line messenger like)
- Emoji Reaction

---

### Contexts

- Account
  - User
- Chat
  - Room
  - Reply
  - Message Filtering
- Reactions

---

# Pick Your Project

- https://bulmatemplates.github.io/bulma-templates/
- https://tailwindcomponents.com/
- https://material-ui.com/getting-started/templates/
- https://startbootstrap.com/themes/

### 開好 Repo 後，請把 url slack 給我

---

# 3Q
## 麻煩把建議用 Slack 丟給我

#### ~~(1/10 號寫的，希望我們不會被禿子拖下水...🙏)~~
