<template>
  <transition name="work-modal-fade">
    <div
      v-if="open && work"
      class="work-modal-overlay"
      role="dialog"
      aria-modal="true"
      :aria-label="`${work.name}の詳細`"
      @click.self="handleClose"
    >
      <div class="work-modal-content">
        <button
          class="work-modal-switch work-modal-switch-prev"
          type="button"
          aria-label="前のタブ"
          :disabled="!canNavigateTabs"
          @click.stop="prevTab"
        >
          &#8592;
        </button>

        <button
          class="work-modal-switch work-modal-switch-next"
          type="button"
          aria-label="次のタブ"
          :disabled="!canNavigateTabs"
          @click.stop="nextTab"
        >
          &#8594;
        </button>

        <button
          class="work-modal-close"
          type="button"
          aria-label="閉じる"
          @click="handleClose"
        >
          &times;
        </button>
        <div v-if="activeTab === 0" class="work-modal-body">
          <img
            class="work-modal-image"
            :src="work.img"
            :alt="work.name"
            loading="lazy"
          />
          <h3 class="work-modal-title">{{ work.name }}</h3>
          <div class="work-modal-role" v-if="work.info">
            <template v-if="work.infoLabel">
              <span class="work-modal-role-label">{{ work.infoLabel }}</span>
              <span class="work-modal-role-text">{{ work.info }}</span>
            </template>
            <template v-else>
              {{ work.info }}
            </template>
          </div>
          <p
            v-if="work.description"
            class="work-modal-description"
            v-html="work.description"
          ></p>
        </div>
        <div v-else-if="activeTab === 1" class="work-modal-body work-modal-body-details">
          <h3 class="work-modal-title">{{ work.name }}</h3>
          <div class="work-modal-role" v-if="work.info">
            <template v-if="work.infoLabel">
              <span class="work-modal-role-label">{{ work.infoLabel }}</span>
              <span class="work-modal-role-text">{{ work.info }}</span>
            </template>
            <template v-else>
              {{ work.info }}
            </template>
          </div>
          <h4 class="work-modal-subtitle">{{ work.detailsLabel || "Details" }}</h4>
          <div
            v-if="!work.suppressDetailsContent"
            class="work-modal-description"
            v-html="renderDetails(work)"
          ></div>
          <div v-if="work.externalLink" class="work-modal-external">
            <a
              class="work-modal-external-link"
              :href="work.externalLink.url"
              target="_blank"
              rel="noopener"
            >
              {{ work.externalLink.text || "詳しく見る" }}
            </a>
          </div>
        </div>
        <div v-else class="work-modal-body work-modal-body-details">
          <h3 class="work-modal-title">{{ work.name }}</h3>
          <div class="work-modal-role" v-if="work.info">
            <template v-if="work.infoLabel">
              <span class="work-modal-role-label">{{ work.infoLabel }}</span>
              <span class="work-modal-role-text">{{ work.info }}</span>
            </template>
            <template v-else>
              {{ work.info }}
            </template>
          </div>
          <template v-if="work.tech">
            <h4 class="work-modal-subtitle">開発スタック</h4>
            <div
              class="work-modal-description"
              v-html="work.tech"
            ></div>
          </template>
          <div v-else class="work-modal-description work-modal-description-empty">
            <em>※準備中</em>
          </div>
        </div>
        <div class="work-modal-tabs" role="tablist" aria-label="プロジェクト詳細表示切替">
          <button
            v-for="tab in displayTabs"
            :key="tab.value"
            type="button"
            class="work-modal-tab-button"
            :class="{ 'is-active': activeTab === tab.value }"
            :aria-label="tab.label"
            :aria-selected="activeTab === tab.value"
            :tabindex="activeTab === tab.value ? 0 : -1"
            role="tab"
            @click="setTab(tab.value)"
            @keydown.enter.prevent="setTab(tab.value)"
            @keydown.space.prevent="setTab(tab.value)"
          >
            ●
          </button>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: "WorkModal",
  props: {
    open: {
      type: Boolean,
      default: false,
    },
    work: {
      type: Object,
      default: null,
    },
  },
  emits: ["close"],
  data() {
    return {
      activeTab: 0,
      tabs: [
        { value: 0, label: "概要タブ" },
        { value: 1, label: "詳細タブ" },
        { value: 2, label: "Techタブ" },
      ],
    };
  },
  watch: {
    open(value) {
      if (value) {
        this.activeTab = 0;
        this.lockScroll();
        window.addEventListener("keydown", this.onKeydown);
      } else {
        this.unlockScroll();
        window.removeEventListener("keydown", this.onKeydown);
      }
    },
    work() {
      this.activeTab = 0;
    },
  },
  mounted() {
    if (this.open) {
      this.activeTab = 0;
      this.lockScroll();
      window.addEventListener("keydown", this.onKeydown);
    }
  },
  computed: {
    displayTabs() {
      const tabs = [this.tabs[0]];
      if (this.work?.details || this.work?.description) {
        tabs.push(this.tabs[1]);
      }
      tabs.push(this.tabs[2]);
      return tabs;
    },
    tabCount() {
      return this.displayTabs.length;
    },
    canNavigateTabs() {
      return this.tabCount > 1;
    },
  },
  beforeUnmount() {
    this.unlockScroll();
    window.removeEventListener("keydown", this.onKeydown);
  },
  methods: {
    handleClose() {
      this.activeTab = 0;
      this.$emit("close");
    },
    prevTab() {
      if (!this.canNavigateTabs) return;
      this.activeTab = (this.activeTab - 1 + this.tabCount) % this.tabCount;
    },
    nextTab() {
      if (!this.canNavigateTabs) return;
      this.activeTab = (this.activeTab + 1) % this.tabCount;
    },
    setTab(value) {
      if (this.activeTab === value) return;
      this.activeTab = value;
    },
    onKeydown(event) {
      if (event.key === "Escape") {
        this.handleClose();
      } else if (event.key === "ArrowLeft") {
        this.prevTab();
      } else if (event.key === "ArrowRight") {
        this.nextTab();
      }
    },
    lockScroll() {
      document.body.style.overflow = "hidden";
    },
    unlockScroll() {
      document.body.style.overflow = "";
    },
    renderDetails(work) {
      if (work.suppressDetailsContent) {
        return "";
      }
      const details = work.details || "";
      const extra = work.extra ? `<strong>【詳細実績】</strong><br>${work.extra.replace(/\n/g, "<br>")}` : "";
      if (details && extra) {
        return `${details}<br><br>${extra}`;
      }
      if (!details && extra) {
        return extra;
      }
      return details || work.description || "";
    },
  },
};
</script>

<style scoped>
.work-modal-fade-enter-active,
.work-modal-fade-leave-active {
  transition: opacity 0.2s ease;
}

.work-modal-fade-enter-from,
.work-modal-fade-leave-to {
  opacity: 0;
}

.work-modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(14, 24, 41, 0.75);
  backdrop-filter: blur(3px);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 24px;
  z-index: 1000;
}

.work-modal-content {
  position: relative;
  width: 640px;
  max-width: 90vw;
  background: #fff;
  border-radius: 16px;
  padding: 28px;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.25);
  overflow: hidden;
  max-height: 480px;
  overflow-y: auto;
}

.work-modal-close {
  position: absolute;
  top: 16px;
  right: 16px;
  width: 36px;
  height: 36px;
  border: none;
  border-radius: 50%;
  background: rgba(0, 0, 0, 0.08);
  color: #333;
  font-size: 20px;
  cursor: pointer;
  transition: background 0.2s ease;
}

.work-modal-close:hover {
  background: rgba(0, 0, 0, 0.16);
}

.work-modal-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  border-radius: 12px;
  margin-bottom: 24px;
}

.work-modal-title {
  font-size: 1.6rem;
  margin-bottom: 8px;
  letter-spacing: 0.05em;
}

.work-modal-role {
  font-size: 1rem;
  font-weight: 600;
  color: #1ca9e3;
  margin-bottom: 16px;
}

.work-modal-role-label {
  display: inline-block;
  margin-right: 8px;
  font-weight: 700;
  color: #355c7d;
}

.work-modal-role-text {
  color: #1ca9e3;
}

.work-modal-external {
  margin-top: 20px;
}

.work-modal-external-link {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  font-weight: 600;
  color: #1a73e8;
  text-decoration: none;
  border-bottom: 1px solid rgba(26, 115, 232, 0.4);
  padding-bottom: 2px;
  transition: color 0.2s ease, border-color 0.2s ease;
}

.work-modal-external-link::after {
  content: "↗";
  font-size: 0.9em;
}

.work-modal-external-link:hover {
  color: #0b57d0;
  border-color: rgba(11, 87, 208, 0.7);
}

.work-modal-subtitle {
  font-size: 1.1rem;
  font-weight: 700;
  margin-bottom: 12px;
  letter-spacing: 0.04em;
  color: #2c3e50;
}

.work-modal-description {
  line-height: 1.75;
  color: #4a4a4a;
  max-height: none;
  overflow-y: visible;
}

.work-modal-description-empty {
  min-height: 120px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #7f8c8d;
}

.work-modal-body {
  text-align: left;
}

.work-modal-body-details {
  min-height: 260px;
}

.work-modal-tabs {
  margin-top: 12px;
  display: flex;
  justify-content: center;
  gap: 12px;
}

.work-modal-tab-button {
  background: transparent;
  border: none;
  color: rgba(51, 51, 51, 0.4);
  font-size: 1.4rem;
  line-height: 1;
  padding: 0;
  cursor: pointer;
  transition: color 0.2s ease, transform 0.2s ease;
}

.work-modal-tab-button:focus-visible {
  outline: 2px solid #1ca9e3;
  outline-offset: 4px;
}

.work-modal-tab-button.is-active {
  color: #1ca9e3;
  transform: scale(1.05);
}

.work-modal-switch {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 42px;
  height: 42px;
  border: none;
  border-radius: 50%;
  background: rgba(28, 169, 227, 0.12);
  color: #1ca9e3;
  font-size: 1.4rem;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.2s ease, transform 0.2s ease;
}

.work-modal-switch:disabled {
  cursor: not-allowed;
  opacity: 0.3;
}

.work-modal-switch:not(:disabled):hover,
.work-modal-switch:not(:disabled):focus-visible {
  background: rgba(28, 169, 227, 0.24);
  transform: translateY(-50%) scale(1.05);
  outline: none;
}

.work-modal-switch-prev {
  left: 12px;
}

.work-modal-switch-next {
  right: 12px;
}

@media (max-width: 900px) {
  .work-modal-switch-prev {
    left: 8px;
  }

  .work-modal-switch-next {
    right: 8px;
  }
}

@media (max-width: 640px) {
  .work-modal-content {
    width: 90vw;
    padding: 24px 20px;
  }

  .work-modal-image {
    height: 220px;
  }
}
</style>

