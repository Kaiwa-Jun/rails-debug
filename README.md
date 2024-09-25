- **`puts` （ログに値などを出力）OK**
    - コンソールに値を出力して、処理の進行を確認
    
    ```ruby
    ① エラー文からapp/views/tasks/index.html.erbの7行目が怪しい
    ② each が nil（NilClass）に対して呼び出されている（nilなのは@tasks）
    ③ @tasksを探す（コントローラーとピンとくる or 検索する）
    ④ indexで@tasksが作られているか確認したい（puts "Loaded @tasks: #{@tasks.inspect}"を仕込む）
    ⑤ ログが出てこないため、@tasksが適切に定義されていないことが分かる
    
    class TasksController < ApplicationController
      def new
        puts "called index"
        @taskss = Task.all  # @tasks = Task.all に修正
        puts "Loaded @tasks: #{@task.inspect}"
      end
    end
    
    出力
    Loaded tasks: #<ActiveRecord::Relation [#<Task id: 1, name: "aaa", description: "aaa", status: "aaa", created_at: "2024-09-25 05:27:19.632497000 +0000", updated_at: "2024-09-25 05:27:19.632497000 +0000">, #<Task id: 2, name: "bbb", description: "bbb", status: "bbb", created_at: "2024-09-25 05:27:25.425605000 +0000", updated_at: "2024-09-25 05:27:25.425605000 +0000">]>
    ```
    
- **`Rails.logger.debug`（ログに値などを出力）OK**
    - ログに変数の値や処理の進行状況を出力して確認
    
    ```ruby
    class TasksController < ApplicationController
      def new
        Rails.logger.debug "Loaded @tasks: #{@tasks.inspect}"
        @taskss = Task.all  # @tasks = Task.all に修正
      end
    end
    
    出力
    Loaded tasks: #<ActiveRecord::Relation [#<Task id: 1, name: "aaa", description: "aaa", status: "aaa", created_at: "2024-09-25 05:27:19.632497000 +0000", updated_at: "2024-09-25 05:27:19.632497000 +0000">, #<Task id: 2, name: "bbb", description: "bbb", status: "bbb", created_at: "2024-09-25 05:27:25.425605000 +0000", updated_at: "2024-09-25 05:27:25.425605000 +0000">]>
    ```

- **`rails console` （データベースやモデルを直接操作して確認）OK**
    - 実際のデータを使って動作確認
    
    ```
    **$railsだけの場合$**
    $ rails console
    > Task.find(1)
    
    **Dockerを使っている場合**
    $ docker-compose run web sh
    # rails console
    > Task.find(1)
    
    データの追加、削除時にデータベースも期待通りに変更されるかを見たりできる
    
    ●ユースケース
    タスク追加したが、一覧画面に追加したはずのタスクが表示されない
    データベースを確認したら、保存はできている
    ↓
    データベースから取得する、それを表示する過程に問題がありそう
    ```
