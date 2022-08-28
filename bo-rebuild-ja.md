<!-- Notion ticket: https://www.notion.so/shippioinc/Shippio-backoffice-system-rebuild-e1499ed38f63454c96b5c1cb76c8c72b -->

## この記事について
Shippio でフロントエンドを担当しているTKです👋<br>
Shippio エンジニアチームのブログ発足、フロントエンドチームの初記事となります🎉<br>
はじめての記事はどんな内容にしようか悩みましたが、私が現在担当している、システムをゼロから構築するというエキサイティングなプロジェクトにて採用したテックスタックや、ディレクトリ構成を紹介したいと思います。<br>

## Shippio で開発しているサービス

まず最初に、Shippio でどんなサービスを開発しているのかに触れておきますが、一言で言うなら、**貿易業務を効率化するウェブサービス**です。<br>
また、ただウェブサービスを提供しているだけでなく、**Shippio のオペレーターによるフォワーディング業務**も Shippio ウェブサービス上で提供しています。<br>

<FOの画像入れる？>

Shippio について、上記だけではあまりピンとこないかと思いますので、詳細については、ぜひホームページを見てみてください👇<br>
https://www.shippio.io/en/

## バックオフィス刷新プロジェクト

私が今取り組んでいるシステム刷新ですが、上記で記載したサービスの後者にあたる、フォーワーディング業務をウェブサービス上で提供するための、Shippio のオペレーターが使う管理システムです。<br>

### システムの前提要件

プロダクトの要件は以下となります。<br>
- Shippio オペレーターがフォワーディング業務を行うための、管理システム
- SEOの必要性はない
- メインで想定する端末はPC
- IEはサポート外

### システム構築で意識したこと

旧バックオフィスの課題点などを踏まえ、システム構築では以下の点を特に意識しました。<br>

1. 保守性の高いシステム
2. 開発者が開発しやすいシステム

それでは、採用したテックスタック、ディレクトリ構成を紹介します。
## テックスタックの紹介

### 意識したこと

- 要件にあったツールの選定
- 使い慣れたツールだけでなく、評価の高い新しいツールの積極的採用
- TypeScriptの導入(今まではJavaScriptだった)
- 依存パッケージの脆弱性に早めに対処するためのツールを導入

### 図
### パッケージそれぞれ

- フレームワーク： React
- 言語 : TypeScript
- ビルドツール : Vite
- CI/CD : Circle CI
- ホスティング : Firebase
- Linter/Formatter : ESLint/Prettier/Stylelint/husky/lint-staged
- グローバルステート管理 : React Context API
- ルーター : React Router
- コンポーネントライブラリ : MUI
  - テーブルライブラリ : Tanstack Table
- CSS : CSS Modules
- GraphQL Client : Apollo Client
  - graphql-codegen
- Unit Test : Vitest
- Error tracking : rollbar
- Package update management : dependabot

## フォルダ構成

次に、フォルダ構成を紹介します。<br>

### 意識したこと

- フォルダの役割を明確にする
- DDD(Domain Driven Design) を意識し、ドメインごと(以下、`model` ディレクトリにあたる)にコンポーネントを仕分けることで、コンポーネントの再利用性を高める
- 運用中、コンポーネントの移動が少なくなるようにする
- 外部ライブラリへの密結合を避ける
- ディレクトリ階層の深さにルールを設け、予測可能、見通しをよくする
  - Barrel ファイルを利用、エリアスの設定で、import の際のパスも短く、わかりやすく

```

src
├─ assets/                                  ## icons, global css
├─ components/
│  ├─ pages/                                 ## has concerns to page
│  │  ├─ Top/
│  │  │  ├─ Top.tsx
│  │  │  ├─ Top.module.css
│  │  │  └─ index.ts                        ## barrel
│  ├─ models/                                ## has concerns to model/domain/entity
│  │  └─ user/
│  │    └─ UserAvatar/
│  │       ├─ __tests__
│  │       │  ├─ userAvatarHelper.test.ts
│  │       │  └─ useAvatorHook.test.ts
│  │       ├─ UserAvator.tsx                ## testable funcitons will be all in helper for testability
│  │       ├─ UserAvatar.module.css
│  │       ├─ userAvatar.helper.tsx          ## funcitons used in the main component
│  │       ├─ useAvatarHook.tsx             ## this can be "hook" directory if multiple hooks
│  │       └─ index.ts
│  ├─ ui/                                   ## has NO concerns to model, only UI
│  │  └─ Button/
│  │     ├─ Button.tsx
│  │     ├─ Button.module.css
│  │     └─ index.ts
│  └─ functional/                           ## Neither has concerns to model nor has UI
│     └─ KeyBind/
│        ├─ KyeBind.tsx
│        └─ index.ts
├─ constants/
├─ generated/                               ## generated code
├─ graphql/
├─ hooks/                                   ## common hooks
├─ lib/                                     ## interface to functions from external liberaries
├─ providers/
├─ routes/                                  ## routes
├─ styles/                                  ## global css
├─ types/
├─ utils/                                   ## utils
```

上記のディレクトリ構成ですが、以下の記事を参考にしました。(日本語の記事です 🙏)<br>
https://zenn.dev/yoshiko/articles/99f8047555f700

## 最後に

今回は、バックオフィスシステム刷新プロジェクトにて採用したテックスタックとフォルダ構成の紹介をしました。
それぞれのツールの選定理由や、フォルダ構成の細かい説明などの深掘りや、運用してからの振り返り（良かったこと、悪かったこと）など、今後の記事でフォローアップできたらなと考えています。<br>

<!-- もう少し具体的には、
- 要件にあった、良いツールを選定する（使い慣れたツールだけでなく、評価の高い新しいツールの積極的採用）
- 役割が明確で、開発がしやすい、ディレクトリ構造を採用する（後述）
- TypeScriptの導入(今まではJavaScriptだった)
- 依存パッケージの脆弱性に早めに対処するためのツールを導入
- 外部ライブラリとの密結合を避けるためのモジュール化
- チーム課題として取り組んでいる、テストカバレッジを上げる -->

<!-- ### 今までの課題

- フォルダ構成に明確なルールがなく、統一性を欠いていた
  - ディレクトリ階層の深さにルールがなく、かなり深い階層で見通しが悪くなっていた
- ページごとにフォルダ分けをしていたが、コンポーネントの見通しが悪いことにより再利用性が低かった
- 外部ライブラリに密結合してしまっていた -->