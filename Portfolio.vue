<template>
  <div class="wrapper">
    <!-- Header -->
    <header class="header">
      <div class="container">
        <h1 class="header-logo">
          <a href="#">HYUN SEUNGYUN</a>
        </h1>
        <nav class="gnav">
          <ul class="gnav-list">
            <li class="gnav-item"><a href="#works">CAREER</a></li>
            <li class="gnav-item"><a href="#skill">SKILL</a></li>
            <li class="gnav-item"><a href="#about">ABOUT</a></li>
            <li class="gnav-item"><a href="#contact">CONTACT</a></li>
          </ul>
        </nav>
      </div>
    </header>

    <main class="content">
      <!-- MV -->
      <section class="mv">
        <div class="mv-container">
          <p class="mv-title">
            HYUN SEUNGYUN
          </p>
          <p class="mv-subtitle">PORTFOLIO</p>
          <p class="mv-text">
            ここはエンジニア <ruby>HYUN SEUNGYUN<rt>ヒョン スンユン</rt></ruby>のポートフォリオサイトです。<br>
            これまで携わってきたプロジェクトや、開発スキルをまとめています。<br>
            どうぞゆっくりとご覧ください。<br>
          </p>
        </div>
      </section>

      <!-- Works -->
      <section class="works section" id="works">
        <div class="container">
          <h2 class="title skill-title">CAREER</h2>
          <p class="title-sub title-sub-bold">キャリア・プロジェクト経験</p>
          <div class="works-list">
            <div
              v-for="(work, i) in works"
              :key="i"
              class="works-item"
              @click="openWorkModal(work)"
              role="button"
              tabindex="0"
              @keydown.enter.prevent="openWorkModal(work)"
              @keydown.space.prevent="openWorkModal(work)"
            >
              <div class="works-img">
                <img
                  :src="work.img"
                  :alt="work.name"
                  class="works-img-photo"
                  width="400"
                  height="300"
                />
              </div>
              <p class="works-name">
                <span
                  class="works-name-text"
                  role="button"
                  tabindex="0"
                  @click.stop="openWorkModal(work)"
                  @keydown.enter.prevent="openWorkModal(work)"
                  @keydown.space.prevent="openWorkModal(work)"
                >
                  {{ work.name }}
                </span>
              </p>
              <p class="works-info">{{ work.info }}</p>
            </div>
          </div>
        </div>
      </section>
      <WorkModal
        :open="isWorkModalOpen"
        :work="selectedWork"
        @close="closeWorkModal"
      />

      <!-- Skill -->
      <section class="skill section" id="skill">
        <div class="container">
          <h2 class="title skill-title">Development Skill Set</h2>
          <p class="title-sub">開発スキルセット</p>
          <p class="lead">
            以下はフルスタックエンジニアである私の、フロントエンド・バックエンド・DevOps／インフラの3領域における経験年数を、レーダーチャートで可視化したものです。（単位は年）<br>
            ＊業務で実際に使用した技術のみ掲載しております。
          </p>
          <div class="skill-category-list">
            <div
              v-for="category in skillCategories"
              :key="category.key"
              class="skill-category-card"
            >
              <h3 class="skill-category-title">{{ category.title }}</h3>
              <div class="skill-chart-wrapper">
                <canvas
                  class="skill-chart"
                  :aria-label="`${category.title}のレーダーチャート`"
                  role="img"
                  :ref="el => setChartRef(category.key, el)"
                ></canvas>
              </div>
              <div
                v-if="getCurrentSkill(category)"
                class="skill-slider"
                role="group"
                :aria-label="`${category.title}のスキル詳細`"
              >
                <button
                  class="skill-nav skill-nav-prev"
                  type="button"
                  aria-label="前のスキル"
                  @click="prevSkill(category.key)"
                >
                  <span aria-hidden="true">&#8592;</span>
                </button>
                <div class="skill-slide">
                  <div class="skill-slide-header">
                    <span class="skill-tag">{{ getCurrentSkill(category).name }}</span>
                    <span class="skill-years">{{ formatYears(getCurrentSkill(category).years) }}</span>
                  </div>
                  <p class="skill-text" v-html="getCurrentSkill(category).text"></p>
                </div>
                <button
                  class="skill-nav skill-nav-next"
                  type="button"
                  aria-label="次のスキル"
                  @click="nextSkill(category.key)"
                >
                  <span aria-hidden="true">&#8594;</span>
                </button>
              </div>
              <div class="skill-slider-dots">
                <button
                  v-for="(skill, idx) in category.skills"
                  :key="idx"
                  type="button"
                  class="skill-slider-dot"
                  :class="{ 'is-active': skillIndices[category.key] === idx }"
                  :aria-label="`${category.title} - ${skill.name}`"
                  :aria-current="skillIndices[category.key] === idx ? 'true' : 'false'"
                  @click="setSkill(category.key, idx)"
                ></button>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- About -->
      <section class="about section" id="about">
        <div class="container">
          <h2 class="title">ABOUT ME</h2>
          <div class="profile">
            <p class="profile-img">
              <img src="img/profile.jpg" alt="HYUN SEUNGYUN" />
            </p>
            <div class="profile-body">
              <p v-for="(p, i) in aboutTexts" :key="i">{{ p }}</p>
            </div>
          </div>
        </div>
      </section>

      <!-- Contact -->
      <section class="contact section" id="contact">
        <div class="container">
          <h2 class="title">CONTACT</h2>
          <p class="lead">
            お問い合わせは、<br class="sp-only">SNSかメールにてお願いいたします。
          </p>
          <div class="contact-list">
            <a class="contact-item" href="https://x.com/HYUNSEUNGY32604" target="_blank" rel="noopener">Twitter</a>
            <span class="contact-item">seungyun.hyun.20251021@gmail.com</span>
          </div>
        </div>
      </section>

      <div class="page-top" @click="scrollTop">
        <span class="material-icons-outlined">expand_less</span>
      </div>
    </main>

    <footer class="footer">
      <div class="copyright">&copy; HYUN SEUNGYUN</div>
    </footer>
  </div>
</template>

<script>
import WorkModal from "./WorkModal.vue";

export default {
  name: "Portfolio",
  components: {
    WorkModal,
  },
  data() {
    return {
      works: [
        {
          name: "クラウドサービス保守・開発",
          info: "Software Engineer",
          img: "img/cloud_service.png",
          description:
            "法人向けに光回線・閉域網・クラウド接続・モバイル通信など多様なネットワークサービスをワンストップで提供し、高速・高可用・高セキュリティな通信環境を実現するシステムの保守・開発を担当していました。",
          details:
            "・各種ネットワーク機器（ルータ等）の管理機能や複数プラットフォームから取得したパラメータ情報の可視化・表示機能を開発<br>・新規機能の追加、既存機能の改修、リファクタリング、テスト整備など、開発から保守運用まで一貫して対応<br>・ミドルウェアのEOL（サポート終了）や脆弱性などのリスクをチームリーダーへ報告し、対応を推進<br>・ドキュメントが不足しているプラットフォームについて、資料作成や手順書整備を実施し、チーム内の情報共有を強化<br>・上記に関連する各機能の実装および改修を担当",
          tech:
            "<strong style=\"font-size:1.08em;\">■ 使用言語</strong><br>　Ruby（main）、TypeScript、PHP<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ 環境</strong><br>　Linux／PostgreSQL<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ フレームワーク</strong><br>　Ruby on Rails<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ バージョン管理</strong><br>　GitLab<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ インフラ</strong><br>　AWS（EC2、S3、RDS、VPC、CloudWatch、etc...）<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ CI/CD</strong><br>　GitLab CI/CD<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ サーバ監視</strong><br>　CloudWatch、Zabbix<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ コミュニケーションツール</strong><br>　Slack、Microsoft Teams<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ ドキュメント管理</strong><br>　Redmine、SharePoint<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ 課題管理（BTS）</strong><br>　Redmine",
        },
        {
          name: "交通系ICカードシステム保守",
          info: "Infrastructure Engineer",
          img: "img/ic_card_system.png",
          description:
            "約500万人が利用する交通系ICカードの端末および交通関連システム向けプラットフォーム（FP）を提供するサービスの保守・運用を担当し、システムの安定稼働と品質維持に取り組んでいました。",
          details:
            "<strong style=\"font-size:1.08em;\">ミドルウェア／ファームウェアのアップデートおよび性能改善</strong><br>" +
            "・各ミドルウェア・ファームウェアのリリースノートを基に机上調査を実施し、FP・IC端末が最新安定版を利用しているかを確認<br>" +
            "・古いバージョン使用時は安定版へのアップデートを検討・実施し、非推奨メソッドや不適切なコーディング箇所を精査・修正<br>" +
            "・新機能追加時には適用可否の検討および技術報告書の作成を担当<br>" +
            "・保守・運用側で対応可能な修正は直接実施し、開発完了後に JUnit による単体テストを実施<br>" +
            "・全FP・IC端末を対象に JMeter による高負荷テストおよび Jenkins によるリグレッションテストを実施<br>" +
            "・Pythonツールを用いてテスト結果を可視化・分析し、レポートとして報告<br><br>" +
            "<strong style=\"font-size:1.08em;\">脆弱性調査および対応</strong><br>" +
            "・NVD／JVN／CVEなどの脆弱性情報を収集・要約し、交通系ICカード端末およびFPへの影響を調査<br>" +
            "・使用中の全ライブラリについてソースコードレベルで影響有無を確認<br>" +
            "・影響がある場合は修正箇所・影響範囲・危険性を明確化し、代替ライブラリや安全なメソッドを提示した修正案を作成<br>" +
            "・上層部承認後、該当箇所のソースコード修正を保守・運用側で実施",
          tech:
            "<strong style=\"font-size:1.08em;\">■ 使用言語</strong><br>　Java<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ 環境</strong><br>　Linux／MySQL・MariaDB<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ フレームワーク</strong><br>　Spring Framework<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ バージョン管理</strong><br>　Subversion（SVN）<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ インフラ／Webサーバ</strong><br>　Apache HTTP Server<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ アプリケーションサーバ</strong><br>　Apache Tomcat<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ CI／ビルド管理</strong><br>　Apache Maven<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ サーバ監視／管理</strong><br>　Tomcat Manager<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ テスト</strong><br>　JUnit（単体テスト）、JMeter（負荷テスト）<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ コミュニケーションツール</strong><br>　Slack<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ ドキュメント管理</strong><br>　Redmine（Wiki機能利用）<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ 課題管理（BTS）</strong><br>　Redmine",
        },
        {
          name: "車両診断システム向けグラフライブラリ開発",
          info: "Software Engineer",
          img: "img/charting_library.png",
          description:
            "自動車メーカー向け車両診断システム（OBD2ツール）のクラウド化プロジェクトにて、グラフライブラリの設計・開発を担当。要件定義からテストまで一貫して携わり、リアルタイム・履歴データの描画や設定機能を実装しました。",
          details:
            "<strong style=\"font-size:1.08em;\">要件定義</strong><br>" +
            "・お客様へのヒアリングを通じて利用シーンを把握し、機能要件（UI構成・画面設計）および非機能要件（ライセンス・データ保持・耐久性）を定義<br>" +
            "・要件定義書およびシーケンス図を作成し、レビューやデモプログラムを通じて合意形成を実施<br><br>" +
            "<strong style=\"font-size:1.08em;\">基本設計</strong><br>" +
            "・アーキテクチャ方針および進行ルールを策定し、WBSを用いて工数と要員を見積もり<br>" +
            "・Vue.js・Chart.js・Rubyを組み合わせたツール構成を採用し、ライセンス運用基準を定義<br>" +
            "・主要機能（グラフ生成・描画・設定・UI/UX）を整理し、クラス図・データフロー図を作成<br><br>" +
            "<strong style=\"font-size:1.08em;\">詳細設計</strong><br>" +
            "・システム全体を複数モジュールに分割し、役割と連携方法を明確化<br>" +
            "・データ取得・整形モジュールをRubyで実装し、正規化・補間・集約ロジック、タイムゾーン処理を定義<br>" +
            "・通知モジュールではObserverパターンを採用し、Subject／Observerの関係およびイベント種別を整理してリアルタイム更新を実現<br>" +
            "・Vue.jsコンポーネントとChart.jsラッパを組み合わせた描画モジュールを設計し、グラフ種別・凡例・軸スケール・描画手順（Observer → Vue → Chart.js）を詳細化<br>" +
            "・API入出力のJSONスキーマ、イベントペイロード、更新間隔・閾値を定義し、モジュール間の連携仕様を明確化<br>" +
            "・状態管理・エラー設計ではリトライ、タイムアウト、フォールバック処理、ログ出力、監視項目を定義し、安定稼働を考慮<br>" +
            "・環境依存項目（APIエンドポイント、更新周期、閾値、ライセンス表記など）を設定可能とし、環境ごとの柔軟な運用に対応<br>" +
            "・欠損データやスパイク、長時間稼働時の性能劣化を想定したテスト観点を整理し、信頼性を重視した設計を実施<br><br>" +
            "<strong style=\"font-size:1.08em;\">開発</strong><br>" +
            "・基本設計・詳細設計をもとに各モジュールを実装<br>" +
            "・Rubyによるデータ取得・整形処理を開発し、正規化・補間・集約ロジックを反映してリアルタイム性と精度を両立<br>" +
            "・Subject／Observer間のイベント通知処理を実装し、リアルタイム更新を実現<br>" +
            "・Vue.jsでグラフ描画・設定・UI/UX機能を構築し、Chart.jsと連携して多様なグラフ表示を実現<br><br>" +
            "<strong style=\"font-size:1.08em;\">テスト</strong><br>" +
            "・Jestを用いた単体テストを実施し、VueコンポーネントおよびChart.jsラッパのテスト項目（描画成否、軸スケール変更、凡例表示、イベント発火、エラーハンドリング）を作成<br>" +
            "・APIレスポンスをモック化し（JSONスキーマ準拠）、Observerイベントから描画完了までのフローを検証<br>" +
            "・JMeterを用いた長時間耐久テストを実施し、メモリリーク・CPU使用率・描画FPS・UI応答性（ズーム／パン）を監視<br>" +
            "・更新周期継続時の蓄積誤差やしきい値超過時の挙動を確認し、安定性を検証<br><br>" +
            "<strong style=\"font-size:1.08em;\">その他</strong><br>" +
            "・ユーザマニュアルを作成し、機能説明および操作手順を整理・共有",
          tech:
            "<strong style=\"font-size:1.08em;\">■ 使用言語</strong><br>　TypeScript（メイン）、Ruby<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ 環境</strong><br>　Windows<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ フレームワーク</strong><br>　Vue.js<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ バージョン管理</strong><br>　GitLab<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ アプリケーションサーバ</strong><br>　Node.js<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ CI／ビルド管理</strong><br>　GitLab CI／npm<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ テスト</strong><br>　Jest<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ コミュニケーションツール</strong><br>　Slack<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ ドキュメント管理</strong><br>　GitLab Wiki<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ 課題管理（BTS）</strong><br>　GitLab Issues",
        },
        {
          name: "SFA & 業務システム保守・開発",
          info: "Software Engineer",
          img: "img/sfa_system.png",
          description:
            "通信事業者向けに提供される法人ネットワーク管理ポータルシステムおよび社内営業支援システム（SFA、Sales Force Automation)の2つのシステムについて、開発・保守を並行して担当していました。",
          details:
            "・お客様の要望通り画面、バッグエンド処理を実装する",
          tech:
            "<strong style=\"font-size:1.08em;\">■ 使用言語</strong><br>　Ruby（メイン）、TypeScript<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ 環境</strong><br>　Windows<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ フレームワーク</strong><br>　Ruby on Rails／Vue.js<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ バージョン管理</strong><br>　GitLab<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ インフラ</strong><br>　AWS（EC2、S3、RDS、CloudWatch など）<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ CI／CD</strong><br>　GitLab CI／CD<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ サーバ監視</strong><br>　Amazon CloudWatch<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ コミュニケーションツール</strong><br>　Slack／Microsoft Teams<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ ドキュメント管理</strong><br>　GitLab Wiki／SharePoint<br><br>" +
            "<strong style=\"font-size:1.08em;\">■ 課題管理（BTS）</strong><br>　GitLab Issues",
        },
        {
          name: "その他",
          info: "Blog, Personal Projects、etc",
          hideOverviewInfo: true,
          img: "img/etc.png",
          description:
            "ブログや小規模の個人プロジェクト、技術ナレッジの発信を通じて、自己学習と知識共有に取り組んでいます。",
          detailsLabel: "ブログ",
          details:
            "・Nuxt.js/Next.js を用いた技術ブログの制作とコンテンツ更新\n・小規模なAPIサーバーやCLIツール開発による技術検証\n・最新のフロントエンド/インフラ技術を取り入れたPoC作成\n・情報発信とアウトプットを通じた知識の定着と共有",
          suppressDetailsContent: true,
          externalLink: {
            url: "https://qiita.com/ehthkdia4007",
            text: "HYUN SEUNGYUNのQiitaの記事を見る",
          },
        },
      ],
      skillCategories: [
        {
          key: "frontend",
          title: "Frontend",
          colors: {
            border: "rgba(52, 152, 219, 0.4)",
            borderTransparent: "rgba(52, 152, 219, 0.75)",
            background: "rgba(52, 152, 219, 0.18)",
            pointTransparent: "rgba(41, 128, 185, 0.75)",
          },
          labels: ["TypeScript", "Vue.js", "HTML / CSS", "jQuery", "Nuxt.js", "React"],
          years: [3, 3, 3, 1, 2, 1],
          skills: [
            { name: "TypeScript", years: 3, text: "型安全性を重視した設計と保守性の高い実装を心がけています。<br>GenericsやUtility型を用いた柔軟な型設計や、Vue / React / Node.jsでの開発にも対応可能です。<br>既存JavaScriptのTypeScript移行や、Prettierを用いたコードの可読性向上にも取り組んでいました。" },
            { name: "Vue.js", years: 3, text: "SPA開発での実務経験があり、保守性と拡張性を重視した設計を心がけています。<br>コンポーネント設計や状態管理を通じて、再利用性の高いUIの実現を目指して開発していました。" },
            { name: "HTML / CSS", years: 3, text: "UI実装の基礎技術として、アクセシビリティやレスポンシブ対応を考慮したコーディングを行います。<br>可読性・再利用性を意識した構造設計が強みです。" },
            { name: "jQuery", years: 1, text: "既存システムの保守や機能改修など、レガシー環境での対応に活用してきました。<br>既存資産を活かした安定的な運用改善に貢献しています。" },
            { name: "Nuxt.js", years: 1, text: "Vueベースのフレームワークとして、SPAやSSR構成の開発経験があります。<br>API連携やデータフロー設計を通じて、効率的なフロントエンド構築を行っています。" },
            { name: "React", years: 1, text: "SPA開発において、コンポーネント設計やルーティング構成を中心に実装経験があります。<br>TypeScriptとの併用による型安全な開発にも対応可能です。" },
          ],
        },
        {
          key: "backend",
          title: "Backend",
          colors: {
            border: "rgba(231, 76, 60, 0.4)",
            borderTransparent: "rgba(231, 76, 60, 0.75)",
            background: "rgba(231, 76, 60, 0.18)",
            pointTransparent: "rgba(192, 57, 43, 0.75)",
          },
          labels: ["Ruby ", "PostgreSQL", "Redis", "REST API", "HTTP", "Node.js"],
          years: [3, 2, 2, 1, 3, 1],
          skills: [
            { name: "Ruby", years: 3, text: "Ruby on Railsによる高速なWebアプリ開発と、RSpecを用いた自動テスト設計を得意としています。<br>保守性・拡張性を両立したバックエンド構築が可能です。" },
            { name: "PostgreSQL", years: 2, text: "商用・OSS問わず利用されるRDBMSで、設計・最適化・運用まで一貫して対応してきました。<br>パフォーマンスとデータ整合性を重視した設計を行います。" },
            { name: "Redis", years: 2, text: "キャッシュ管理やセッション制御など、パフォーマンス改善を目的とした導入経験があります。<br>高トラフィック環境での応答速度向上に活用しています。" },
            { name: "REST API", years: 1, text: "フロントエンドとの通信を意識したAPI設計・実装を行っています。<br>HTTPメソッドやレスポンス構造の統一により、わかりやすいAPI設計を実現しています。" },
            { name: "HTTP", years: 3, text: "API通信の基礎であるHTTP仕様を理解し、全層での設計・デバッグに活かしています。<br>リクエスト/レスポンスの制御を通じて安定した通信を実現します。" },
            { name: "Node.js", years: 1, text: "軽量なAPI・バッチ処理・自動化スクリプトなど、幅広い用途で活用しています。<br>バックエンドツール開発や補助タスクの効率化に貢献しています。" },
          ],
        },
        {
          key: "devops",
          title: "DevOps / Infrastructure",
          colors: {
            border: "rgba(39, 174, 96, 0.4)",
            borderTransparent: "rgba(39, 174, 96, 0.75)",
            background: "rgba(39, 174, 96, 0.18)",
            pointTransparent: "rgba(30, 132, 73, 0.75)",
          },
          labels: ["AWS", "Docker", "Git / GitLab", "Redmine", "Zabbix", "Slack / Teams"],
          years: [2, 1, 3, 2, 0.5, 3],
          skills: [
            { name: "AWS ", years: 2, text: "EC2 / S3 / Lambda / VPC など、さまざまな AWS サービスを活用した開発・運用を担当していました。インフラからアプリケーション層まで幅広く対応し、構成設計・運用・監視を通じて、スケーラビリティとコスト効率を両立した環境構築を得意としています。" },
            { name: "Docker", years: 1, text: "開発・検証・本番環境間での再現性確保を目的に活用しています。<br>CI/CDパイプラインの効率化にも貢献しています。" },
            { name: "Git / GitLab", years: 3, text: "チーム開発でのバージョン管理やブランチ戦略策定を担当。<br>GitLab CIを用いた自動テスト・デプロイフローの構築経験があります。" },
            { name: "Redmine", years: 2, text: "タスク管理や進行可視化を通じて、開発プロセスの改善とチーム全体の生産性向上に貢献しています。" },
            { name: "Zabbix", years: 0.5, text: "サーバ監視を通じて稼働安定化や障害予防に取り組んでいます。<br>アラート設計や閾値設定による早期検知体制の構築を経験しています。" },
            { name: "Slack / Microsoft Teams", years: 3, text: "チーム間の情報共有と自動通知連携を構築し、開発フローの効率化に寄与しています。<br>Bot連携やCI通知によるリアルタイムな情報伝達を実現しています。" },
          ],
        },
      ],
      skillIndices: {
        frontend: 0,
        backend: 0,
        devops: 0,
      },
      chartInstances: {},
      chartRefs: {},
      aboutTexts: [
        "韓国出身のバックエンド寄りのフルスタックエンジニアです。2016年4月に日本の大学へ進学し、2022年4月に日系SIer企業へ入社しました。",
        "高校時代に交換留学で出会った日本人の友人がきっかけで、日本への留学を決意しました。大学では経済学を専攻しておりましたが、「プログラミングを学び、誰かの役に立つものを作りたい」という想いから、IT業界への道を選びました。",
        "外国人かつ未経験でのスタートは大変でしたが、先輩や上司、チームメンバーなど多くの方々に支えられ、少しずつ成長することができました。まだ学ぶことは多いですが、チャレンジ精神と探求心を大切にしながら、日々成長を続けて行きたいと思います！🔥",
      ],
      selectedWork: null,
      isWorkModalOpen: false,
      selectedWorkIndex: null,
    };
  },
  methods: {
    scrollTop() {
      window.scrollTo({ top: 0, behavior: "smooth" });
    },
    openWorkModal(work) {
      this.selectedWork = work;
      this.isWorkModalOpen = true;
    },
    closeWorkModal() {
      this.isWorkModalOpen = false;
      this.selectedWork = null;
    },
    getSkillsByKey(key) {
      const category = this.skillCategories.find((cat) => cat.key === key);
      return category ? category.skills : [];
    },
    getCurrentSkill(category) {
      const index = this.skillIndices[category.key] ?? 0;
      return category.skills[index] || category.skills[0] || null;
    },
    setSkill(key, index) {
      const skills = this.getSkillsByKey(key);
      if (!skills.length) {
        return;
      }
      const safeIndex = Math.max(0, Math.min(index, skills.length - 1));
      this.skillIndices[key] = safeIndex;
    },
    prevSkill(key) {
      const skills = this.getSkillsByKey(key);
      const count = skills.length;
      if (!count) {
        return;
      }
      const current = this.skillIndices[key] ?? 0;
      this.skillIndices[key] = (current - 1 + count) % count;
    },
    nextSkill(key) {
      const skills = this.getSkillsByKey(key);
      const count = skills.length;
      if (!count) {
        return;
      }
      const current = this.skillIndices[key] ?? 0;
      this.skillIndices[key] = (current + 1) % count;
    },
    setChartRef(key, el) {
      if (el) {
        this.chartRefs[key] = el;
      }
    },
    formatYears(value) {
      if (value === undefined || value === null || value === "") {
        return "";
      }
      return `${value}年`;
    },
    renderSkillCharts() {
      if (typeof Chart === "undefined") {
        console.error("Chart.js is not loaded.");
        return;
      }

      this.skillCategories.forEach((category) => {
        const canvas = this.chartRefs[category.key];
        if (!canvas) {
          return;
        }

        if (this.chartInstances[category.key]) {
          this.chartInstances[category.key].destroy();
        }

        this.chartInstances[category.key] = new Chart(canvas, {
          type: "radar",
          data: {
            labels: category.labels,
            datasets: [
              {
                label: `${category.title} の経験年数`,
                data: category.years,
                backgroundColor: category.colors.background,
                borderColor: category.colors.borderTransparent,
                pointBackgroundColor: category.colors.pointTransparent,
                pointBorderColor: category.colors.pointTransparent,
                pointHoverBackgroundColor: category.colors.pointTransparent,
                pointHoverBorderColor: category.colors.pointTransparent,
              },
            ],
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
              r: {
                beginAtZero: true,
                min: 0,
                max: 3,
                ticks: {
                  stepSize: 0.5,
                  callback: (value) => `${value}`,
                  showLabelBackdrop: false,
                },
                grid: {
                  color: "rgba(0, 0, 0, 0.1)",
                },
                angleLines: {
                color: "rgba(0, 0, 0, 0.1)",
                },
                pointLabels: {
                  font: {
                    size: 12,
                  },
                },
              },
            },
            plugins: {
              legend: {
                display: false,
              },
              tooltip: {
                callbacks: {
                label: (context) => `${context.label}: ${context.raw}`,
                },
              },
            },
          },
        });
      });
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.renderSkillCharts();
    });
  },
  beforeUnmount() {
    Object.values(this.chartInstances).forEach((chart) => {
      if (chart) chart.destroy();
    });
  },
};
</script>

<style scoped>
.lead {
  margin-top: 12px;
  line-height: 1.8;
}

.skill-title {
  margin-bottom: 6px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #333;
}

.works .skill-title {
  margin-bottom: 4px;
}

.title-sub {
  margin: 4px auto 18px;
  color: #555;
  font-size: 0.85rem;
  letter-spacing: 0.08em;
  text-align: center;
}

.title-sub-bold {
  color: #333;
  font-weight: 700;
}

.mv-title {
  font-size: 60px;
  font-weight: bold;
  line-height: 1.2;
  margin-bottom: 5px;
  letter-spacing: 0.1em;
}

.mv-subtitle {
  font-size: 20px;
  font-weight: bold;
  line-height: 1;
  margin-bottom: 40px;
  letter-spacing: 0.08em;
}

.works-img-photo {
  width: 300px;
  height: 300px;
  max-width: 100%;
  object-fit: cover;
  display: block;
  margin: 0 auto 12px;
  border-radius: 8px;
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.14);
}

.works-name,
.works-info {
  text-align: center;
  width: 100%;
}

.works-name {
  margin-top: 0;
}

.works-name-text {
  cursor: pointer;
  text-decoration: underline;
  text-decoration-color: transparent;
  transition: text-decoration-color 0.2s ease;
}

.works-name-text:focus,
.works-name-text:hover {
  text-decoration-color: currentColor;
}

.works-name-text:focus-visible {
  outline: 2px solid #1ca9e3;
  outline-offset: 3px;
}

.works-item {
  cursor: pointer;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.skill-category-list {
  display: grid;
  gap: 32px;
  margin-top: 32px;
}

@media (min-width: 1024px) {
  .skill-category-list {
    grid-template-columns: repeat(3, minmax(0, 1fr));
  }
}

.skill-category-card {
  padding: 24px;
  border-radius: 16px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.96), rgba(240, 247, 255, 0.96));
  box-shadow: 0 16px 40px rgba(12, 36, 97, 0.12);
  display: flex;
  flex-direction: column;
  min-height: 100%;
}

.skill-category-title {
  font-size: 1.35rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  margin-bottom: 16px;
}

.skill-chart-wrapper {
  position: relative;
  width: 100%;
  min-height: 260px;
  margin-bottom: 20px;
}

.skill-chart {
  width: 100%;
  height: 260px;
}

.skill-description-list {
  margin-top: auto;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.skill-description-item {
  padding: 14px 16px;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.75);
  border: 1px solid rgba(0, 0, 0, 0.05);
  backdrop-filter: blur(2px);
}

.skill-description-header {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  gap: 12px;
  margin-bottom: 6px;
}

.skill-tag {
  font-weight: 600;
  font-size: 1rem;
}

.skill-years {
  font-size: 1.00rem;
  color: #7f8c8d;
}

.skill-text {
  line-height: 1.6;
  font-size: 0.95rem;
}

.profile-img {
  width: 200px;
  height: 200px;
  overflow: hidden;
}

.profile-img img {
  width: 100%;
  height: 100%;
  display: block;
  object-fit: cover;
}

.skill-slider {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-top: 12px;
}

.skill-nav {
  width: 40px;
  height: 40px;
  border: none;
  border-radius: 50%;
  background: rgba(28, 169, 227, 0.12);
  color: #1ca9e3;
  font-size: 1.4rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease, transform 0.2s ease;
}

.skill-nav:hover,
.skill-nav:focus-visible {
  background: rgba(28, 169, 227, 0.2);
  transform: translateY(-1px);
  outline: none;
}

.skill-slide {
  flex: 1;
  background: rgba(255, 255, 255, 0.85);
  border: 1px solid rgba(0, 0, 0, 0.05);
  border-radius: 14px;
  padding: 18px;
  box-shadow: 0 10px 30px rgba(12, 36, 97, 0.08);
  min-height: 160px;
}

.skill-slide-header {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  gap: 12px;
  margin-bottom: 10px;
}

.skill-slider-dots {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 16px;
}

.skill-slider-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: rgba(28, 169, 227, 0.2);
  border: none;
  cursor: pointer;
  transition: background 0.2s ease, transform 0.2s ease;
}

.skill-slider-dot.is-active {
  background: #1ca9e3;
  transform: scale(1.1);
}

.skill-slider-dot:focus-visible {
  outline: 2px solid #1ca9e3;
  outline-offset: 3px;
}

</style>