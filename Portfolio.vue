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
            <li class="gnav-item"><a href="#works">WORKS</a></li>
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
          <p class="mv-title">HYUN SEUNGYUN</p>
          <p class="mv-subtitle">PORTFOLIO</p>
          <p class="mv-text">
            ここはエンジニア HYUN SEUNGYUNのポートフォリオサイトです。<br>
            これまで携わってきた開発実績や、培ってきたスキルをまとめています。<br>
            どうぞゆっくりとご覧ください。<br>
          </p>
        </div>
      </section>

      <!-- Works -->
      <section class="works section" id="works">
        <div class="container">
          <h2 class="title skill-title">WORKS</h2>
          <p class="title-sub">プロジェクト経験</p>
          <div class="works-list">
            <div
              v-for="(work, i) in works"
              :key="i"
              class="works-item"
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
              <p class="works-name">{{ work.name }}</p>
              <p class="works-info">{{ work.info }}</p>
            </div>
          </div>
        </div>
      </section>

      <!-- Skill -->
      <section class="skill section" id="skill">
        <div class="container">
          <h2 class="title skill-title">Development Skill Set</h2>
          <p class="title-sub">開発スキルセット</p>
          <p class="lead">
            フロントエンド・バックエンド・DevOps／インフラの3領域における経験年数をレーダーチャート化しました（単位：年）。<br>
            現場で培った知見をベースに、継続的な学習と改善が習慣になっています。
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
              <ul class="skill-description-list">
                <li
                  v-for="(skill, i) in category.skills"
                  :key="i"
                  class="skill-description-item"
                >
                  <div class="skill-description-header">
                    <span class="skill-tag">{{ skill.name }}</span>
                    <span class="skill-years">{{ formatYears(skill.years) }}</span>
                  </div>
                  <p class="skill-text" v-html="skill.text"></p>
                </li>
              </ul>
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
            <a class="contact-item" href="https://twitter.com/" target="_blank" rel="noopener">Twitter</a>
            <a class="contact-item" href="https://www.instagram.com/" target="_blank" rel="noopener">Instagram</a>
            <span class="contact-item">xxxxxx@gmail.com</span>
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
export default {
  name: "Portfolio",
  data() {
    return {
      works: [
        { name: "クラウドサービス保守・開発", info: "Software Engineer", img: "img/cloud_service.png" },
        { name: "交通系ICカードシステム保守", info: "Infrastructure Engineer", img: "img/ic_card_system.png" },
        { name: "車両診断システム向けグラフライブラリ開発", info: "Software Engineer", img: "img/charting_library.png" },
        { name: "SFA & 業務システム保守・開発", info: "Software Engineer", img: "img/sfa_system.png" },
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
            { name: "Ruby (Rails / RSpec)", years: 3, text: "Ruby on Railsによる高速なWebアプリ開発と、RSpecを用いた自動テスト設計を得意としています。<br>保守性・拡張性を両立したバックエンド構築が可能です。" },
            { name: "PostgreSQL", years: 2, text: "商用・OSS問わず利用されるRDBMSで、設計・最適化・運用まで一貫して対応してきました。<br>パフォーマンスとデータ整合性を重視した設計を行います。" },
            { name: "Redis", years: 2, text: "キャッシュ管理やセッション制御など、パフォーマンス改善を目的とした導入経験があります。<br>高トラフィック環境での応答速度向上に活用しています。" },
            { name: "REST / RESTful API", years: 1, text: "フロントエンドとの通信を意識したAPI設計・実装を行っています。<br>HTTPメソッドやレスポンス構造の統一により、わかりやすいAPI設計を実現しています。" },
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
            { name: "AWS (EC2 / S3 / Lambda / VPC等)", years: 2, text: "インフラからアプリ層まで幅広く担当し、構成設計・運用・監視までを一貫して対応。<br>スケーラビリティとコスト効率を意識したAWS活用を得意としています。" },
            { name: "Docker", years: 1, text: "開発・検証・本番環境間での再現性確保を目的に活用しています。<br>CI/CDパイプラインの効率化にも貢献しています。" },
            { name: "Git / GitLab", years: 3, text: "チーム開発でのバージョン管理やブランチ戦略策定を担当。<br>GitLab CIを用いた自動テスト・デプロイフローの構築経験があります。" },
            { name: "Redmine", years: 2, text: "タスク管理や進行可視化を通じて、開発プロセスの改善とチーム全体の生産性向上に貢献しています。" },
            { name: "Zabbix", years: 0.5, text: "サーバ監視を通じて稼働安定化や障害予防に取り組んでいます。<br>アラート設計や閾値設定による早期検知体制の構築を経験しています。" },
            { name: "Slack / Microsoft Teams", years: 3, text: "チーム間の情報共有と自動通知連携を構築し、開発フローの効率化に寄与しています。<br>Bot連携やCI通知によるリアルタイムな情報伝達を実現しています。" },
          ],
        },
      ],
      chartInstances: {},
      chartRefs: {},
      aboutTexts: [
        "韓国出身。日本の大学卒業後、日系SIerでシステムエンジニアとして活動中。",
        "Web技術・クラウド・UI改善を中心に開発を担当。ユーザー目線の設計を大切にしています。",
      ],
    };
  },
  methods: {
    scrollTop() {
      window.scrollTo({ top: 0, behavior: "smooth" });
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
}

.works-name {
  margin-top: 0;
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
</style>