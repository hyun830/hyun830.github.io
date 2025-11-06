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
            ユーザーとお客様が心から満足できる<br class="sp-only">
            サイトをお作りいたします。<br>
            設計・実装・テストはおまかせください。<br>
          </p>
        </div>
      </section>

      <!-- Works -->
      <section class="works section" id="works">
        <div class="container">
          <h2 class="title">WORKS</h2>
          <div class="works-list">
            <div
              v-for="(work, i) in works"
              :key="i"
              class="works-item"
            >
              <div class="works-img">
                <img :src="work.img" :alt="work.name" />
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
          <h2 class="title">SKILL</h2>
          <div class="skill-list">
            <div
              v-for="(skill, i) in skills"
              :key="i"
              class="skill-item"
            >
              <p class="skill-img"><img src="img/icon_skill.png" alt=""></p>
              <div class="skill-body">
                <h3 class="skill-name">{{ skill.name }}</h3>
                <p class="skill-text">{{ skill.text }}</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- About -->
      <section class="about section" id="about">
        <div class="container">
          <h2 class="title">ABOUT</h2>
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
        { name: "デモプログラムを追加", info: "Design / Coding(Responsive)", img: "img/works-sample-thumb.jpg" },
        { name: "デモプログラムを追加", info: "Design / Coding / WordPress", img: "img/works-dummy-thumb.jpg" },
        { name: "デモプログラムを追加", info: "Design", img: "img/works-dummy-thumb.jpg" },
        { name: "デモプログラムを追加", info: "Banner Design", img: "img/works-dummy-thumb.jpg" },
      ],
      skills: [
        { name: "HTML5 / CSS3", text: "基本的なWebマークアップとレスポンシブデザイン対応が可能です。" },
        { name: "WordPress", text: "テーマ開発・カスタマイズ・API連携に対応。" },
        { name: "JavaScript", text: "jQueryやVue.jsを用いた動的サイト構築。" },
        { name: "Photoshop", text: "画像加工・Webデザイン対応。" },
        { name: "Illustrator", text: "ロゴ・アイコンデザイン制作可能。" },
        { name: "XD", text: "UI/UXデザインプロトタイプ設計可能。" },
      ],
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
  },
};
</script>

<style scoped>
@import "@/assets/css/ress.css";
@import "@/assets/css/style.css";
</style>