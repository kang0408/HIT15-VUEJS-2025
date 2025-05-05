## I. Routeing và Vue Router

### 1. Routing

**Routing** (định tuyến) là quá trình ánh xạ URL tới các thành phần giao diện (components) trong ứng dụng. Trong một SPA, routing giúp chuyển đổi giữa các "trang" mà không cần tải lại toàn bộ trang web, mang lại trải nghiệm mượt mà giống ứng dụng native.

Routing bao gồm:

- **Định nghĩa route**: Liên kết URL với các component.
- **Điều hướng**: Chuyển đổi giữa các route thông qua liên kết hoặc lập trình.
- **Quản lý lịch sử**: Sử dụng HTML5 History API để lưu trữ trạng thái điều hướng.

### 2. Vue Router

**Vue Router** là thư viện định tuyến chính thức của Vue.js, tích hợp chặt chẽ với Vue 3. Các tính năng nổi bật:

- Hỗ trợ **Composition API** và **Options API**.
- **Dynamic Routing**: Tạo route động dựa trên tham số.
- **Nested Routes**: Hỗ trợ các route lồng nhau.
- **Navigation Guards**: Kiểm soát quyền truy cập và hành vi điều hướng.
- **Lazy Loading**: Tải component theo nhu cầu để tối ưu hiệu suất.

---

## II. Getting Started: Bắt đầu với Vue Router

### 1. Config: Dùng mỗi SFC làm instance cho mỗi page

Để bắt đầu, bạn cần cài đặt Vue Router và cấu hình các route sử dụng **Single File Components (SFC)**.

#### Cài đặt Vue Router

Chạy lệnh sau để cài đặt Vue Router 4:

```bash
npm install vue-router@4
```

#### Cấu hình cơ bản

Tạo file `router/index.js` để định nghĩa các route:

```javascript
// router/index.js
import { createRouter, createWebHistory } from "vue-router";
import Home from "../views/Home.vue";
import About from "../views/About.vue";

const routes = [
  { path: "/", name: "Home", component: Home },
  { path: "/about", name: "About", component: About },
];

const router = createRouter({
  history: createWebHistory(),
  routes,
});

export default router;
```

**Giải thích**:

- `createRouter`: Tạo instance router.
- `createWebHistory`: Sử dụng HTML5 History API cho URL sạch (không có `#`).
- `routes`: Mảng các route, mỗi route ánh xạ một đường dẫn (`path`) tới một component.

#### Kích hoạt router trong ứng dụng

Trong file `main.js`, thêm router vào ứng dụng Vue:

```javascript
// main.js
import { createApp } from "vue";
import App from "./App.vue";
import router from "./router";

const app = createApp(App);
app.use(router);
app.mount("#app");
```

#### Tạo SFC cho các page

Tạo các file `Home.vue` và `About.vue` trong thư mục `views`:

```html
<!-- views/Home.vue -->
<script setup></script>

<template>
  <h1>Trang Chủ</h1>
</template>
```

```html
<!-- views/About.vue -->
<script setup></script>

<template>
  <h1>Giới Thiệu</h1>
</template>
```

### 2. Cách điều hướng

Vue Router cung cấp hai cách điều hướng chính:

- **Declarative Navigation**: Sử dụng component `<router-link>`.
- **Programmatic Navigation**: Sử dụng API của router.

#### Declarative Navigation

Sử dụng `<router-link>` để tạo liên kết điều hướng:

```vue
<!-- App.vue -->
<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav>
  <router-view />
</template>
```

**Giải thích**:

- `to`: Đường dẫn hoặc tên route để điều hướng.
- `<router-link>` tự động render thành thẻ `<a>` với các xử lý sự kiện để ngăn tải lại trang.

#### Programmatic Navigation

Sử dụng `router.push()` hoặc `router.replace()` trong mã JavaScript:

```html
<script setup>
  import { useRouter } from "vue-router";

  const router = useRouter();

  const goToAbout = () => {
    router.push("/about"); // Điều hướng tới /about
  };
</script>

<template>
  <button @click="goToAbout">Đi tới Giới Thiệu</button>
</template>
```

**Lưu ý**:

- `router.push()` thêm một bản ghi mới vào lịch sử trình duyệt.
- `router.replace()` thay thế bản ghi hiện tại, không tạo lịch sử mới.

### 3. Nơi hiển thị sau khi điều hướng

Component được ánh xạ với route sẽ hiển thị trong thẻ `<router-view>`:

```vue
<!-- App.vue -->
<script setup></script>

<template>
  <nav>
    <router-link to="/">Trang Chủ</router-link> |
    <router-link to="/about">Giới Thiệu</router-link>
  </nav>
  <router-view />
</template>
```

**Giải thích**:

- `<router-view>` là nơi Vue Router render component tương ứng với route hiện tại.
- Nếu không có route khớp, `<router-view>` sẽ không hiển thị gì.

---

## III. Các yếu tố quan trọng

### 1. Dynamic Route Matching: Tạo bộ định tuyến động

Dynamic routes cho phép truyền tham số qua URL, ví dụ: `/user/:id`.

#### Cấu hình route động:

```javascript
// router/index.js
const routes = [
  {
    path: "/user/:id",
    name: "User",
    component: () => import("../views/User.vue"),
  },
];
```

#### Sử dụng tham số trong component:

```vue
<!-- views/User.vue -->
<script setup>
import { useRoute } from "vue-router";

const route = useRoute();
const userId = route.params.id;
</script>

<template>
  <h1>Người dùng ID: {{ userId }}</h1>
</template>
```

**Giải thích**:

- `:id` là tham số động, có thể là bất kỳ giá trị nào.
- `useRoute()` trả về đối tượng route hiện tại, chứa thông tin như `params`, `query`, v.v.

**Lưu ý**:

- Để tối ưu hiệu suất, sử dụng **lazy loading** với `() => import()` cho component.

#### Multiple Params

Vue Router hỗ trợ nhiều tham số động:

```js
const routes = [
  { path: "/user/:id/post/:postId", name: "UserPost", component: UserPost },
];
```

```html
<!-- UserPost.vue -->
<script setup>
  import { useRoute } from "vue-router";

  const route = useRoute();
  const userId = route.params.id;
  const postId = route.params.postId;
</script>

<template>
  <h1>Người dùng: {{ userId }} - Bài viết: {{ postId }}</h1>
</template>
```

#### Catch All / 404 Not Found Route

Để xử lý các đường dẫn không khớp, sử dụng route catch-all:

```js
const routes = [
  { path: "/:pathMatch(.*)*", name: "NotFound", component: NotFound },
];
```

Lưu ý:

    - `/:pathMatch(.*)*` khớp với mọi đường dẫn không được định nghĩa.
    - Sử dụng `route.params.pathMatch` để truy cập phần đường dẫn không khớp.
    - Đặt route catch-all ở cuối danh sách routes để ưu tiên các route khác.

### 2. Named Routes: Đặt tên cho các route

Named routes cho phép điều hướng bằng tên thay vì đường dẫn, tăng tính linh hoạt.

#### Ví dụ

Định nghĩa named route:

```javascript
// router/index.js
const routes = [
  {
    path: "/user/:id",
    name: "User",
    component: () => import("../views/User.vue"),
  },
];
```

Điều hướng bằng tên:

```html
<script setup>
  import { useRouter } from "vue-router";

  const router = useRouter();

  const goToUser = () => {
    router.push({ name: "User", params: { id: "123" } });
  };
</script>

<template>
  <router-link :to="{ name: 'User', params: { id: '123' } }"
    >Xem người dùng</router-link
  >
  <button @click="goToUser">Đi tới người dùng</button>
</template>
```

**Lưu ý**:

- Khi sử dụng named routes với params, luôn truyền `params` trong object.

### 3. Nested Routes: Page in page

Nested routes cho phép hiển thị các component con bên trong component cha.

#### Ví dụ

Cấu hình nested routes:

```javascript
// router/index.js
const routes = [
  {
    path: "/user/:id",
    name: "User",
    component: () => import("../views/User.vue"),
    children: [
      {
        path: "profile",
        name: "UserProfile",
        component: () => import("../views/UserProfile.vue"),
      },
      {
        path: "settings",
        name: "UserSettings",
        component: () => import("../views/UserSettings.vue"),
      },
    ],
  },
];
```

Component cha chứa `<router-view>` để hiển thị component con:

```html
<!-- views/User.vue -->
<script setup>
  import { useRoute } from "vue-router";

  const route = useRoute();
</script>

<template>
  <h1>Người dùng: {{ route.params.id }}</h1>
  <router-link :to="`/user/${route.params.id}/profile`">Hồ sơ</router-link> |
  <router-link :to="`/user/${route.params.id}/posts`">Bài viết</router-link>
  <router-view />
</template>
```

**Giải thích**:

- `children` định nghĩa các route con.
- `<router-view>` trong component cha hiển thị component con.

### 4. Route Meta Fields

Meta fields cho phép gắn thông tin bổ sung vào route, thường dùng để kiểm soát quyền truy cập.

#### Ví dụ

Thêm meta field:

```javascript
// router/index.js
const routes = [
  {
    path: "/admin",
    name: "Admin",
    component: () => import("../views/Admin.vue"),
    meta: { requiresAuth: true },
  },
];
```

Sử dụng meta trong navigation guard:

```javascript
// router/index.js
router.beforeEach((to, from, next) => {
  if (to.meta.requiresAuth && !isAuthenticated()) {
    next("/login");
  } else {
    next();
  }
});
```

**Giải thích**:

- `meta` là một object chứa thông tin tùy chỉnh.
- Navigation guard kiểm tra `meta` để quyết định hành vi điều hướng.

### 5. Navigation Guards: Bảo vệ điều hướng

Navigation guards kiểm soát quá trình điều hướng, ví dụ: yêu cầu đăng nhập hoặc xác nhận trước khi rời trang.

Các loại guard:

- **Global Guards**: Áp dụng cho toàn bộ router.
- **Per-Route Guards**: Áp dụng cho một tuyến đường cụ thể.
- **In-Component Guards**: Định nghĩa trong thành phần.

#### Global Guards

```js
// router/index.js
router.beforeEach((to, from, next) => {
  console.log(`Điều hướng từ ${from.path} đến ${to.path}`);
  next();
});
```

#### Per-Route Guard

```js
// router/index.js
const routes = [
  {
    path: "/admin",
    name: "Admin",
    component: Admin,
    beforeEnter: (to, from, next) => {
      if (!isAuthenticated()) {
        next("/login");
      } else {
        next();
      }
    },
  },
];
```

#### In-Component Guard

```html
<!-- views/Admin.vue -->
<script>
  beforeRouteEnter(to, from, next) {
    if (!isAuthenticated()) {
      next("/login");
    } else {
      next();
    }
  },
  beforeRouteUpdate(to, from, next) {
    // Gọi khi tham số thay đổi nhưng vẫn ở cùng thành phần
    next();
  },
  beforeRouteLeave(to, from, next) {
    // Gọi khi rời khỏi thành phần
    const answer = window.confirm("Bạn có muốn rời khỏi trang này?");
    if (answer) {
      next();
    } else {
      next(false);
    }
  },
</script>
```

- `beforeEach`: Chạy trước mỗi lần điều hướng.
- `afterEach`: Chạy sau khi điều hướng hoàn tất.

#### Ví dụ: beforeEach và afterEach

```javascript
// router/index.js
import { createRouter, createWebHistory } from "vue-router";

const routes = [
  { path: "/", name: "Home", component: () => import("../views/Home.vue") },
  {
    path: "/admin",
    name: "Admin",
    component: () => import("../views/Admin.vue"),
    meta: { requiresAuth: true },
  },
];

const router = createRouter({
  history: createWebHistory(),
  routes,
});

// Giả lập hàm kiểm tra đăng nhập
const isAuthenticated = () => false;

// beforeEach
router.beforeEach((to, from, next) => {
  if (to.meta.requiresAuth && !isAuthenticated()) {
    next({ name: "Home" }); // Chuyển hướng nếu chưa đăng nhập
  } else {
    next(); // Tiếp tục điều hướng
  }
});

// afterEach
router.afterEach((to, from) => {
  console.log(`Đã điều hướng từ ${from.path} tới ${to.path}`);
});

export default router;
```

**Giải thích**:

- `beforeEach`: Kiểm tra quyền truy cập trước khi điều hướng. `next()` cho phép tiếp tục, `next('/path')` chuyển hướng.
- `afterEach`: Ghi log hoặc thực hiện hành động sau điều hướng, không có `next`.

**Lưu ý**:

- Tránh tạo vòng lặp vô hạn trong guard (ví dụ: gọi `next('/path')` lặp lại).
- `afterEach` không thể thay đổi điều hướng.

### 6. Programmatic Navigation: Điều hướng theo hướng lập trình

Điều hướng lập trình cho phép chuyển hướng thông qua mã JavaScript, sử dụng useRouter.

Các phương thức chính:

- **router.push()**: Thêm một bản ghi mới vào lịch sử trình duyệt.
- **router.replace()**: Thay thế bản ghi hiện tại, không thêm vào lịch sử.
- **router.go(n)**: Di chuyển tiến hoặc lùi trong lịch sử (n là số nguyên).

#### Ví dụ

```vue
<script setup>
import { useRouter } from "vue-router";

const router = useRouter();

const goToUser = () => {
  router.push({ name: "User", params: { id: "123" } });
};

const replaceToHome = () => {
  router.replace("/");
};
</script>

<template>
  <button @click="goToUser">Xem người dùng</button>
  <button @click="replaceToHome">Thay thế về trang chủ</button>
</template>
```

**Lưu ý**:

- Sử dụng `try/catch` khi điều hướng để xử lý lỗi:

```javascript
try {
  await router.push("/invalid");
} catch (err) {
  console.error("Lỗi điều hướng:", err);
}
```

### 7. Scroll Behavior: Hành vi cuộn trang

Vue Router cho phép tùy chỉnh hành vi cuộn khi điều hướng, ví dụ: cuộn về đầu trang hoặc khôi phục vị trí cuộn.

#### Ví dụ

Cấu hình scroll behavior:

```javascript
// router/index.js
import { createRouter, createWebHistory } from 'vue-router';

const router = createRouter({
  history: createWebHistory(),
  routes: [...],
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition; // Khôi phục vị trí cuộn khi quay lại
    }
    if (to.hash) {
      return { el: to.hash }; // Cuộn tới anchor
    }
    return { top: 0 }; // Cuộn về đầu trang
  },
});

export default router;
```

**Giải thích**:

- `scrollBehavior` nhận 3 tham số: `to`, `from`, `savedPosition`.
- Trả về object `{ top: 0 }` để cuộn về đầu, hoặc `{ el: '#id' }` để cuộn tới phần tử cụ thể.

**Lưu ý**:

- Để hỗ trợ cuộn mượt, thêm CSS:

```css
html {
  scroll-behavior: smooth;
}
```
