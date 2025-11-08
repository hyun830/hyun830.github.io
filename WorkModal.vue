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
          <div class="work-modal-role">{{ work.info }}</div>
          <p
            v-if="work.description"
            class="work-modal-description"
            v-html="work.description"
          ></p>
        </div>
        <div v-else class="work-modal-body">
          <h3 class="work-modal-title">{{ work.name }}</h3>
          <div class="work-modal-role">{{ work.info }}</div>
          <div
            class="work-modal-description"
            v-html="renderDetails(work)"
          ></div>
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
      if (this.work?.details) {
        return this.tabs;
      }
      return [this.tabs[0]];
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
    setTab(value) {
      if (this.activeTab === value) return;
      if (value === 1 && !this.work?.details && !this.work?.description) return;
      this.activeTab = value;
    },
    onKeydown(event) {
      if (event.key === "Escape") {
        this.handleClose();
      }
    },
    lockScroll() {
      document.body.style.overflow = "hidden";
    },
    unlockScroll() {
      document.body.style.overflow = "";
    },
    renderDetails(work) {
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

.work-modal-description {
  line-height: 1.75;
  color: #4a4a4a;
  max-height: 120px;
  overflow-y: auto;
}

.work-modal-body {
  text-align: left;
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

