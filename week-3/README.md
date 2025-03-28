#### 1. Conditional Rendering

- Vue cung cấp directives giúp chúng ta hiển thị hoặc ẩn các thành phần HTML với một điều kiện nào đó
- Các directives: `v-if`, `v-else`, `v-else-if`, `v-show`

##### 1.1. `v-if`

```js
<script setup>
    const awesome = true;
</script>
<template>
    <h1 v-if="awesome">Vue is awesome!</h1>
</template>
```

Thẻ `h1` này sẽ được hiện thị khi giá trị của awesome trong directives `v-if` nhận giá trị **truthy**, bị ẩn khi là giá trị **falsy**

##### 1.2. `v-else`

```js
<script setup>
    const awesome = ref(false);
</script>
<template>
    <button @click="awesome = !awesome">Toggle</button>

    <h1 v-if="awesome">Vue is awesome!</h1>
    <h1 v-else>Oh no 😢</h1>
</template>
```

##### 1.3. `v-else-if`

```js
<script setup>
    const type = ref("B");
</script>
<template>
    <div v-if="type === 'A'">
    A
    </div>
    <div v-else-if="type === 'B'">
    B
    </div>
    <div v-else-if="type === 'C'">
    C
    </div>
    <div v-else>
    Not A/B/C
    </div>
</template>
```

-Một điều thú ví đó là chúng ta có thể sử dụng `v-if`, `v-else-if`, `v-else` trong `<template></template>`

##### 1.4. `v-show`

- `v-show` cũng có cơ chế hoạt động giống `v-if` tuy nhiên cách hiển thị của nó có phần khác biệt trong DOM và nó tác động tới thuộc tính CSS property
- `v-show` không hỗ trợ dùng cho `template`, không dùng được với `v-else`

#### 2. Form input binding

`v-model` là một **directive** trong Vue dùng để tạo **two-way data binding** giữa dữ liệu (data) trong Vue và các phần tử nhập liệu (`<input>`, `<textarea>`, `<select>`).

Nó giúp bạn đồng bộ dữ liệu giữa input và biến trong Vue mà không cần viết event handler thủ công.

##### 2.1. Input text

```js
<script setup>
import { ref } from "vue";

const username = ref("");
</script>

<template>
  <input v-model="username" placeholder="Nhập tên">
  <p>Tên bạn nhập: {{ username }}</p>
</template>
```

##### 2.2. Checkbox

```js
<script setup>
import { ref } from "vue";

const isChecked = ref(false);
</script>

<template>
  <input type="checkbox" v-model="isChecked">
  <p>Trạng thái: {{ isChecked }}</p>
</template>
```

Với checkbox nhiều lựa chọn, `v-model` sẽ liên kết với mảng

```js
<script setup>
import { ref } from "vue";

const selectedOptions = ref([]);
</script>

<template>
  <input type="checkbox" value="Vue" v-model="selectedOptions"> Vue
  <input type="checkbox" value="React" v-model="selectedOptions"> React
  <input type="checkbox" value="Angular" v-model="selectedOptions"> Angular

  <p>Bạn đã chọn: {{ selectedOptions }}</p>
</template>
```

##### 2.3. Radio

```js
<script setup>
import { ref } from "vue";

const gender = ref("");
</script>

<template>
  <input type="radio" value="Nam" v-model="gender"> Nam
  <input type="radio" value="Nữ" v-model="gender"> Nữ

  <p>Giới tính: {{ gender }}</p>
</template>
```

##### 2.4. Select

```js
<script setup>
import { ref } from "vue";

const selectedFruit = ref("apple");
</script>

<template>
  <select v-model="selectedFruit">
    <option value="apple">Táo</option>
    <option value="banana">Chuối</option>
    <option value="orange">Cam</option>
  </select>

  <p>Bạn chọn: {{ selectedFruit }}</p>
</template>
```

##### 2.5. Value Binding

`v-model` mặc định sẽ sử dụng thuộc tính value của các input. Tuy nhiên, trong một số trường hợp như **radio button, checkbox hoặc select**, bạn có thể sử dụng `:value` để định nghĩa giá trị của input.

```js
<script setup>
import { ref } from "vue";

const gender = ref("male");
</script>

<template>
  <input type="radio" v-model="gender" :value="'male'"> Nam
  <input type="radio" v-model="gender" :value="'female'"> Nữ
  <p>Giới tính: {{ gender }}</p>
</template>
```

```js
<script setup>
import { ref } from "vue";

const agree = ref("no");
</script>

<template>
  <input type="checkbox" v-model="agree" :true-value="'yes'" :false-value="'no'">
  <p>Đồng ý: {{ agree }}</p>
</template>
```

##### 2.6. Modifiers

- `.trim`: giúp loại bỏ khoảng trắng đầu/cuối khi nhập

```js
<input v-model.trim="username" placeholder="Nhập tên">
```

- `.numder`: chuyển input từ chuỗi thành số

```js
<input v-model.number="age" type="number">
```

- `.lazy`: chỉ cập nhật dữ liệu khi mất focus

```js
<input v-model.lazy="email">
```

#### 3. Event Handling

- Vue cung cấp directive `v-on` hay cú pháp ngắn hơn là `@` giúp ta thực hiện lắng nghe sự kiện thuận tiện hơn
- Gía trị xử lý có thể là:

  - **inline handlers:** sử lý JS trực tiếp

  ```js
  <script setup>
      const count = ref(0)
  </script>
  <template>
      <button @click="count++">Add 1</button>
      <p>Count is: {{ count }}</p>
  </template>
  ```

  - **method handlers:** là tên hoặc đường dẫn của phương thức được định nghĩa trong component

  ```js
  <script setup>
      const name = ref('Vue.js')

        function greet(event) {
        alert(`Hello ${name.value}!`)
        // `event` is the native DOM event
        if (event) {
            alert(event.target.tagName)
        }
        }
  </script>
  <template>
        <!-- `greet` is the name of the method defined above -->
        <button @click="greet">Greet</button>
  </template>
  ```

- **Event modifier**
  Event Modifier là các từ khóa giúp ngăn chặn hoặc điều chỉnh hành vi mặc định của sự kiện trong Vue. Điều này giúp bạn tránh phải viết event.preventDefault() hay event.stopPropagation() trong JavaScript thuần.

  - `.prevent`: ngăn chặn hành vi mặc định
  - `.once`: chỉ chạy sự kiện một lần
  - `.self`: chỉ chạy sự kiện nếu click đúng vào phần tử đó

- **Key modifier**
  Key Modifiers giúp bạn xử lý sự kiện bàn phím (keydown, keyup, keypress) một cách nhanh chóng mà không cần phải kiểm tra event.key thủ công.

  ```js
  <input @keyup.enter="submit" placeholder="Nhấn Enter để gửi">
  <input @keyup.ctrl.enter="submitWithCtrl" placeholder="Nhấn Ctrl + Enter để gửi">
  <input @keyup.alt.s="save" placeholder="Nhấn Alt + S để lưu">
  <input @keyup.esc="cancel" placeholder="Nhấn ESC để hủy">
  <input @keyup.exact="onlySingleKey" placeholder="Nhấn 1 phím bất kỳ (không kèm tổ hợp)">
  ```

  | Modifier                           | Chức năng                         |
  | ---------------------------------- | --------------------------------- |
  | `.enter`                           | Nhấn Enter                        |
  | `.tab`                             | Nhấn Tab                          |
  | `.delete`                          | Nhấn Delete (Backspace cũng tính) |
  | `.esc`                             | Nhấn Escape                       |
  | `.space`                           | Nhấn Space                        |
  | `.up`, `.down`, `.left`, `.right`  | Các phím mũi tên                  |
  | `.ctrl`, `.alt`, `.shift`, `.meta` | Tổ hợp phím                       |
  | `.exact`                           | Chỉ chạy nếu đúng tổ hợp phím     |

#### 4. List Rendering

- `v-for` là một directive trong Vue.js giúp lặp qua danh sách dữ liệu và render ra nhiều phần tử trên giao diện.

- **Cú pháp cơ bản:**

  ```vue
  <li v-for="item in items" :key="item.id">{{ item.name }}</li>
  ```

- Mỗi phần tử được lặp lại phải có thuộc tính `:key` duy nhất để Vue tối ưu hóa việc render.

##### **1. Cú pháp cơ bản của v-for**

- **Lặp qua một mảng**

```vue
<ul>
  <li v-for="(name, index) in names" :key="index">
    {{ index + 1 }}. {{ name }}
  </li>
</ul>
```

**Kết quả:**

1. An
2. Bình
3. Châu
4. Dũng

- **Lặp qua một object**

```vue
<ul>
  <li v-for="(value, key) in user" :key="key">
    {{ key }}: {{ value }}
  </li>
</ul>

<script setup>
const user = {
  name: "Nguyễn Văn A",
  age: 25,
  job: "Developer",
};
</script>
```

**Kết quả:**

name: Nguyễn Văn A
age: 25
job: Developer

- **Lặp qua một object với index**

```vue
<ul>
  <li v-for="(value, key, index) in user" :key="key">
    {{ index }} - {{ key }}: {{ value }}
  </li>
</ul>
```

**Kết quả:**
0 - name: Nguyễn Văn A  
1 - age: 25  
2 - job: Developer

- **Lặp qua một số nguyên (Render N lần)**

```vue
<ul>
  <li v-for="n in 5" :key="n">Phần tử số {{ n }}</li>
</ul>
```

**Kết quả:**

Phần tử số 1
Phần tử số 2
Phần tử số 3
Phần tử số 4
Phần tử số 5

##### **2. Kết hợp v-for với v-if**

Có thể dùng `v-if` trong `v-for`, nhưng tốt hơn là nên filter dữ liệu trước khi render.
**Cách sai:**

```vue
<li v-for="item in items" v-if="item.active" :key="item.id">
  {{ item.name }}
</li>
```

Vue sẽ kiểm tra `v-if` **mỗi lần render**, không tối ưu.

**Cách đúng:**

```vue
<li v-for="item in activeItems" :key="item.id">
  {{ item.name }}
</li>

<script setup>
import { computed, ref } from "vue";

const items = ref([
  { id: 1, name: "Item 1", active: true },
  { id: 2, name: "Item 2", active: false },
  { id: 3, name: "Item 3", active: true },
]);

const activeItems = computed(() => items.value.filter((item) => item.active));
</script>
```

**Vue chỉ render những item cần thiết, tối ưu hơn.**

##### **3. Sử dụng v-for trong component**

- **Truyền dữ liệu vào component**
  Khi sử dụng `v-for` với component, cần truyền `:key` và props.

```vue
<UserCard v-for="user in users" :key="user.id" :user="user" />

<script setup>
import { ref } from "vue";
import UserCard from "./UserCard.vue";

const users = ref([
  { id: 1, name: "Alice", age: 22 },
  { id: 2, name: "Bob", age: 25 },
]);
</script>
```

**Component `UserCard.vue`:**

```vue
<template>
  <div>
    <h3>{{ user.name }}</h3>
    <p>Tuổi: {{ user.age }}</p>
  </div>
</template>

<script setup>
defineProps(["user"]);
</script>
```

**Giúp tái sử dụng component cho từng user.**

##### **4. Sử dụng v-for để render table**

```vue
<table>
  <thead>
    <tr>
      <th>#</th>
      <th>Tên</th>
      <th>Tuổi</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(user, index) in users" :key="user.id">
      <td>{{ index + 1 }}</td>
      <td>{{ user.name }}</td>
      <td>{{ user.age }}</td>
    </tr>
  </tbody>
</table>
```

📌 **Hiển thị danh sách trong bảng gọn gàng.**

##### **5.Tổng kết**

| Trường hợp                 | Cú pháp                                 |
| -------------------------- | --------------------------------------- |
| Lặp qua mảng               | `v-for="item in items"`                 |
| Lặp qua mảng với index     | `v-for="(item, index) in items"`        |
| Lặp qua object             | `v-for="(value, key) in object"`        |
| Lặp qua object với index   | `v-for="(value, key, index) in object"` |
| Lặp số lần cố định         | `v-for="n in 5"`                        |
| Kết hợp v-for và component | `v-for="item in items" :key="item.id"`  |

##### **6. Lưu ý quan trọng**

- **Luôn đặt `:key`** khi dùng `v-for` để tránh lỗi render.
- **Tránh dùng `v-if` trong `v-for`**, hãy filter dữ liệu trước khi render.
- **Sử dụng component trong `v-for`** để tăng tính tái sử dụng.
