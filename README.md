### GraphQLについて
- RestfullなAPIだとサーバーで定義したエンドポイントごとに決められたレスポンスが返ってくる. 必要な情報のみが欲しい時は新しくエンドポイントを作るか, 不必要な情報も含めて取得するしかない. これだと実装コストが高くなったり, 色々面倒.
- GraphQLだと, フロントがリクエストした内容に対してサーバーが柔軟にレスポンスしてくれるのでフロント-サーバー間の連携がしやすくなる.
- GraphQL プレイグラウンド(https://graphql-demo.mead.io/) を触ってみると直感的に分かる. スキーマでクライアントが操作できるクエリや様々な型を定義する.
- スキーマはあくまで定義のみで実際のデータ操作は行わない. 実際のデータ操作を行うのがリゾルバ. リゾルバの実態は特定のフィールドのデータを返す関数（type Queryの中身）

### gqlgen
- デフォルトだと`schema.graphqls`にスキーマが書かれてる.
- `gqlgen generate`でスキーマファイル（**`graph/schema.graphqls`**）をモデル（**`graph/model/*`**)と比較し, 可能な限りモデルに直接バインドする. **`graph/schema.resolvers.go`** で`panic(fmt.Errorf("not implemented"))`になっているやつは, 自分で実装が必要.
- `graph/resolver.go` に構造体としてドメインを格納
- `go run server.go`したら勝手にlocalhostでplayground起動してる！すごい！
- 書き込みはmutationで読み込みはquery
- リゾルバーの引数には XXInput という名前で input オブジェクトを使用する.
- `gqlgen.yml`を参照してコードを自動生成するので, model.goに型を定義し、`gqlgen.yml`を編集する.
- `models_gen.go`はGraphQLのモデル定義をGolangで扱えるようにした構造体（手動で変更はしない）

#### 手順
- まず`schema.graphqls`にスキーマを書く
- `go run github.com/99designs/gqlgen init`で自動生成 （スキーマに対応した良い感じのメソッドとモデルをresolverとmodels_genに生成してくれる）
- 実装されていないresolverの詳細を実装する
