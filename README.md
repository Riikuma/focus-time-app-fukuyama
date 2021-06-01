# テーブル設計

## usersテーブル

| Column             | Type    | Options                   |
| ------------------ | ------- | ------------------------- |
| email              | string  | null: false, unique: true |
| encrypted_password | string  | null: false               |
| username           | string  | null: false, unique: true |

### Association

- has_many :times
- has_many :templates


## timesテーブル

| Column           | Type       | Options           |
| ---------------- | ---------- | ----------------- |
| focus_time       | integer    | null: false       |
| break_time       | integer    | null: false       |
| number_of_cycles | integer    | null: false       |
| refresh_time     | integer    | null: false       |
| user             | references | foreign_key: true |

### Association

- belongs_to :user


## templatesテーブル

| Column        | Type       | Options           |
| ------------- | ---------- | ----------------- |
| template_name | string     | null: false       |
| user          | references | foreign_key: true |

### Association

- belongs_to :user
- has_many :template_tasks
- has_many :tasks, through: :template_tasks


## tasksテーブル

| Column     | Type       | Options           |
| ---------- | ---------- | ----------------- |
| task_name  | string     | null: false       |
| start_time | integer    | null: false       |
| end_time   | integer    | null: false       |

### Association

- has_many :template_tasks
- has_many :templates, through: :template_tasks


## template_tasksテーブル

| Column   | Type       | Options           |
| -------- | ---------- | ----------------- |
| template | references | foreign_key: true |
| task     | references | foreign_key: true |

### Association

- belongs_to :template
- belongs_to :task



# メモ

## タスク管理機能について

ユーザーテーブルとタスクテーブルの間にテンプレートテーブルを設け、使用するタスクの選択ができるようにする。

新規テンプレート作成でその日に必要なタスクを選択し保存する。
最後に使用するテンプレートを選択する。
例：今日は会議がないので会議タスクを外したテンプレート2を使用する。


## 時間管理機能について

何分集中、何分休憩、何回のサイクル後に何分の多めの休憩かを設定できる。
これ自体はログインしていなくても使用できるようにする。

また、この時間管理機能にはテンプレート選択を設けない。
保存を押すたびにユーザー情報に紐付いた時間設定が上書き保存されるようにする。

ログインをすると上記の設定を保存でき、かつタスク管理もできるようになる。

タスク管理機能実装は優先度低め。

タイマー  スタートボタン・リセットボタン ⇄ ポーズボタン
