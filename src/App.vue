<template>
  <div class="app">
    <header class="header">
      <div class="container">
        <div class="logo">
          <img
            src="https://vuejs.org/images/logo.png"
            alt="Vue.js Logo"
            class="vue-logo"
          />
          <h1>HIT15 VUEJS PRIVATE 2025</h1>
        </div>
        <button
          v-if="markdownContent"
          @click="markdownContent = null"
          class="back-button"
        >
          <i class="icon">←</i> Quay lại
        </button>
      </div>
    </header>

    <main class="container">
      <transition name="fade" mode="out-in">
        <div v-if="!markdownContent" class="weeks-grid">
          <div
            v-for="week in weeks"
            :key="week.id"
            class="week-card"
            @click="getDetailHandler(week.id)"
          >
            <div>
              <div class="week-number">{{ week.name }}</div>
              <ul class="week-description">
                <li v-for="ct in week.content">{{ ct }}</li>
              </ul>
            </div>
            <div class="card-footer">
              <span class="view-details">View Details →</span>
            </div>
          </div>
        </div>

        <div v-else class="week-detail">
          <div class="topics-container">
            <div class="markdown-body" v-html="markdownContent"></div>
          </div>
        </div>
      </transition>
    </main>

    <footer class="footer">
      <div class="container">
        <img src="/logo-02.png" width="56px" height="56px" alt="HIT LOGO" />
        <p>Vue.js Study Plan © 2025</p>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { marked } from "marked";
import "github-markdown-css/github-markdown.css";

const markdownContent = ref("");

const getDetailHandler = async (id) => {
  const res = await fetch(`week-${id}/README.md`);
  const text = await res.text();
  markdownContent.value = marked(text);
};

const weeks = ref([
  {
    id: 1,
    name: "Tuần 1",
    content: [
      "Giới thiệu về Vue (Vue là gì? Vue làm gì?)",
      "Cài đặt môi trường Node, Vite",
      "Tạo ứng dụng Vuejs đầu tiên với Vite",
      "Cấu trúc thư mục trong Vue",
      "Các khái niệm (lý thuyết) cần biết",
    ],
  },
  {
    id: 2,
    name: "Tuần 2",
    content: [
      "Một ứng dụng Vue được tạo ra như thế nào?(Creating a Vue Application)",
      "Template Syntax trong Vue (Là gì? Làm gì?)",
      "Reactivity fundamentals",
      "Deploy Vercel",
    ],
  },
  {
    id: 3,
    name: "Tuần 3",
    content: [
      "Conditional Rendering",
      "Form input binding",
      "Event Handling",
      "List Rendering",
    ],
  },
  {
    id: 4,
    name: "Tuần 4",
    content: ["Computed", "Class and Style Binding", "Components basic"],
  },
  {
    id: 5,
    name: "Tuần 5",
    content: ["Watchers", "Props"],
  },
  {
    id: 6,
    name: "Tuần 6",
    content: ["Lifecycle hooks", "Event emit"],
  },
  {
    id: "6-fetch-api",
    name: "Tuần 6 (Tiếp)",
    content: ["Fetch API"],
  },
  {
    id: 7,
    name: "Tuần 7",
    content: ["Slots", "Provide/Inject"],
  },
  {
    id: 8,
    name: "Tuần 8",
    content: ["Vue router"],
  },
]);
</script>

<style scoped>
.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.container {
  width: 100%;
  max-width: 1300px;
  margin: 0 auto;
  padding: 0 1rem;
}
/* Header */
.header {
  background-color: var(--secondary);
  color: white;
  padding: 1.5rem 0;
  box-shadow: var(--shadow);
  position: sticky;
  top: 0;
  z-index: 10;
}

.header .container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.logo {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.vue-logo {
  height: 2.5rem;
  width: auto;
}

.logo h1 {
  font-size: 1.5rem;
  font-weight: 600;
}

.back-button {
  background: var(--primary);
  border: none;
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.9rem;
  transition: var(--transition);
}

.back-button:hover {
  background: rgba(255, 255, 255, 0.3);
}

.icon {
  font-style: normal;
}

/* Main content */
main {
  flex: 1;
  padding: 2rem 0;
}

/* Weeks grid */
.weeks-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
  margin-top: 1rem;
}

.week-card {
  background-color: var(--card-bg);
  border-radius: 8px;
  box-shadow: var(--shadow);
  padding: 1.5rem;
  cursor: pointer;
  transition: var(--transition);
  border: 1px solid var(--border);
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  /* align-items: end; */
}

.week-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
  border-color: var(--primary-light);
}

.week-number {
  display: inline-block;
  background-color: var(--primary-light);
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 4px;
  font-size: 0.8rem;
  font-weight: 600;
  margin-bottom: 1rem;
}

.week-card h2 {
  font-size: 1.25rem;
  margin-bottom: 0.5rem;
  color: var(--text);
}

.week-card p {
  color: var(--text-light);
  font-size: 0.95rem;
  margin-bottom: 1.5rem;
}

.card-footer {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  margin-top: 1rem;
}

.topics-count {
  font-size: 0.85rem;
  color: var(--text-light);
  background-color: var(--background);
  padding: 0.25rem 0.5rem;
  border-radius: 4px;
}

.view-details {
  color: var(--primary);
  font-size: 0.9rem;
  font-weight: 500;
}

/* Week detail */
.week-detail {
  background-color: var(--card-bg);
  border-radius: 8px;
  box-shadow: var(--shadow);
  padding: 2rem;
  margin-top: 1rem;
}

.week-header {
  margin-bottom: 2rem;
  border-bottom: 1px solid var(--border);
  padding-bottom: 1rem;
}

.week-header h2 {
  font-size: 1.75rem;
  color: var(--primary-dark);
  margin-bottom: 0.5rem;
}

.week-description {
  font-size: 1.1rem;
  color: var(--text-light);
}

.week-description {
  list-style-type: none;
  padding-left: 0.5rem;
}

.week-description {
  margin-bottom: 0.5rem;
  position: relative;
  padding-left: 1.5rem;
}

.week-description li:before {
  content: "•";
  color: var(--primary);
  font-weight: bold;
  position: absolute;
  left: 0;
}
/* Footer */
.footer {
  background-color: var(--secondary);
  color: white;
  padding: 1.5rem 0;
  margin-top: 2rem;
  text-align: center;
}

/* Transitions */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* Responsive */
@media (max-width: 768px) {
  .header .container {
    flex-direction: column;
    gap: 1rem;
  }

  .weeks-grid {
    grid-template-columns: 1fr;
  }

  .topic-card {
    flex-direction: column;
    gap: 1rem;
  }

  .week-detail {
    padding: 1.5rem;
  }

  .logo h1 {
    font-size: 1.2rem;
  }

  .vue-logo {
    height: 2rem;
  }
}
</style>
