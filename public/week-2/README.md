### 1. Creating a Vue Application

#### 1.1. The application instance

Mọi ứng dụng Vue đều được bắt đầu xây dựng bằng cách tạo ra một **application instance** bằng phương thức `createApp()` do Vue cung cấp với đối số yêu cầu truyền vào đó là một **root component**

#### 1.2. The root component

**Root componenent** là component đầu tiên của ứng dụng, thường được đặt trong file `App.vue`

#### 1.3. Mouting the App

Sau khi đã tạo ứng dụng với `createApp()`, chúng ta cần gắn nó vào một phần tử trong DOM hay là DOM thật với phương thức `mount()` với đối số truyền vào là một chuỗi **selector** đóng vai trò là một **container** chứa nội dung, thành phần, giao diện của **root component**

Một điều cần lưu ý đó là `mount()` cần được gọi cuối cùng sau khi `createApp()` hay sử dụng **plugin**

### 2. Template Syntax

**Template Syntax** trong Vue3 là những cú pháp đặc biệt giúp:

- Hiển thị dữ liệu động trên giao diện HTML
- Ràng buộc (**binding**) dữ liệu từ Javascript vào các thuộc tính HTML
- Cung cấp các chỉ thị (**directives**) để thao tác với DOM, điều khiển trạng thái của giao diện một cách linh hoạt
- Lắng nghe sự kiện và xử lý logic trực tiếp trong template

#### 2.1. Text Interpolation (Nội suy văn bản)

- **Mục đích:** Hiển thị giá trị của các biến trong template bằng cú pháp **mustache** (`{{ }}`)
- **Ví dụ:**

```vue
<template>
  <p>Xin chào, {{ name }}!</p>
</template>

<script setup>
import { ref } from "vue";
const name = ref("Người dùng");
</script>
```

#### 2.2. Raw HTML

- **Mục đích:** Hiển thị HTML động từ dữ liệu
- **Ví dụ:**

```vue
<template>
  <div v-html="content"></div>
</template>

<script setup>
import { ref } from "vue";
const content = ref("<strong>Chào bạn!</strong>");
</script>
```

- **Chú ý:**

  - Ở đây có một điều chú ý tới **attribute** `v-html` được gọi là **directive** được bắt đầu với `v-` để thể hiện rằng đó là một **attribute** đặc biệt được cung cấp bới Vue
  - Khi sử dụng `v-html`, Vue sẽ lấy giá trị của `v-html` và chèn vào nội dung của phần tử HTML đó và với cơ chế **phản ứng lại thay đổi (reactivity)**, nếu giá trị của `v-html` thay đổi thì nội dung của phần tử cũng sẽ thay đổi

#### 2.3. Attribute Binding

- **Mục đích:** Ràng buộc giá trị của một biến với thuộc tính HTML.
  Ở trên chúng ta biết về **mustache** giúp hiển thị giá trị các biến trong HTML tuy nhiên với giá trị của một thuộc tính **attribute** ta cần dùng tới `v-bind` **directive**
- **Ví dụ:**

```vue
<template>
  <img v-bind:src="imageUrl" alt="Ảnh động vật" />
</template>

<script setup>
import { ref } from "vue";
const imageUrl = ref("https://example.com/dog.jpg");
</script>
```

- **Chú ý:**

  - Lúc này `v-bind` **directive** sẽ đưa ra chỉ thị là Vue hãy đồng bộ **attribute** `src` với thuộc tính **property** `imageUrl` của component này. Trong trường hợp `imageURl` là `null` hay `undefined`, thì **attribute** sẽ bị loại bỏ trong thành phần HTML
  - Cú pháp shorthand ngắn gọn hơn

  ```vue
  <img :src="imageUrl" alt="Ảnh động vật" />
  ```

#### 2.4. Boolean Attributes

- **Mục đích:** Tự động thêm hoặc loại bỏ thuộc tính nếu giá trị `true` hoặc `false`
- **Ví dụ:**

```vue
<template>
  <button :disabled="isDisabled">Nhấn vào đây</button>
</template>

<script setup>
import { ref } from "vue";
const isDisabled = ref(true);
</script>
```

#### 2.5. Using JavaScript Expressions

- **Mục đích:** Cho phép sử dụng các biểu thức Javascript trong Vue
- **Ví dụ:**

```vue
<template>
  <p>{{ message.toUpperCase() }}</p>
  <p>{{ 10 + 5 }}</p>
</template>

<script setup>
import { ref } from "vue";
const message = ref("hello");
</script>
```

```vue
<time :title="toTitleDate(date)" :datetime="date">
  {{ formatDate(date) }}
</time>
```

- **Lưu ý:** Không thể sử dụng câu lệnh `if`, `for`, `while` trong `{{ }}`

#### 2.6. Directives

- **Mục đích:** Vue cung cấp các **directives** với kí tự `v-` ở đầu. Với công dụng giúp điều khiển DOM theo logic của Vue
- **Một số directive quan trọng:**
  | Directive | Mô tả |
  |:-----|--------:|
  | `v-if`/ `v-else-if` / `v-else` | Hiển thị có điều kiện |
  | `v-for` | Lặp danh sách |
  | `v-bind` | Ràng buộc dữ liệu |
  | `v-model` | Liên kết dữ liệu hai chiều |
  | `v-on` | Lắng nghe sự kiện |
- **Ví dụ:**

```vue
<template>
  <p v-if="isVisible">Hiển thị nội dung này!</p>
</template>

<script setup>
import { ref } from "vue";
const isVisible = ref(true);
</script>
```

#### 2.7. Arguments & Dynamic Arguments

- **Đối số cố định**: Cho phép truyền một thuộc tính cụ thể vào directive.

```vue
<template>
  <img :src="imageUrl" alt="Ảnh động vật" />
</template>

<script setup>
import { ref } from "vue";
const imageUrl = ref("https://example.com/dog.jpg");
</script>
```

- **Đối số động:** Cho phép truyền biến vào directive để chỉ định thuộc tính linh hoạt. Lưu ý đối số động không thể chứa giá trị `null` hoặc `undefined`

```vue
<template>
  <a :[attribute]="url">Click vào đây</a>
</template>

<script setup>
import { ref } from "vue";
const attribute = ref("href");
const url = ref("https://vuejs.org");
</script>
```

#### 2.8. Modifiers

- **Mục đích:** Thay đổi hành vi mặc định của **directive** với kí tự `.` + hành vi
- **Ví dụ**

```vue
<template>
  <form @submit.prevent="handleSubmit">
    <button type="submit">Gửi</button>
  </form>
</template>

<script setup>
const handleSubmit = () => alert("Biểu mẫu được gửi!");
</script>
```

#### 2.9. Full directive syntax

![alt text](img/week-2/image.png)

### 3. Reactivity fundamentals

Vue3 sử dụng **Reactivity System** để nhận biết sự thay đổi từ đó giúp tự động cập nhật dự liệu trong giao diện người dùng khi có sự thay đổi. `ref()` và `reactive()` sẽ giúp ta thực hiện điều này

#### 3.1. Ref

- `ref()` được sử dụng để tạo ra một biến phản ứng (**reaction variable**). Nó hoạt động với tất cả các kiểu dữ liệu như **string**, **number**, **boolean**, **object**, **array**
- **Cách dùng:**

  ```vue
  <script setup>
  import { ref } from "vue";

  // Tạo một biến phản ứng
  const count = ref(0);

  // Hàm tăng giá trị biến
  const increment = () => {
    count.value++; // Phải dùng .value để truy cập giá trị
  };
  </script>

  <template>
    <div>
      <p>Giá trị count: {{ count }}</p>
      <button @click="incrememnt">Tăng</button>
    </div>
  </template>
  ```

  - Lúc này biến `count` có giá trị mặc định là `0`. Đồng thời khi count thay đổi tằng dần thông qua hàm `increment`, Vue sẽ tự động cập nhật lại giao diện với giá trị biến `count` sau khi tăng
  - Để có thể truy cập giá trị của `count` thì ta phải sử dụng `.value`
  - Còn trong template, ta chỉ cần sử dụng tới `{{ count }}` mà không cần `.value` nhờ vào cơ chế **unwrap** của Vue

- Tại sao dùng `ref()`
  Với Javascript thuần túy, javascript không hề nhận biết được có sự thay đổi nào với một biến nào đó, thay vào đó chúng ta sẽ phải thực hiện thêm một vài câu lệnh để hiển thị dữ liệu mới lên màn hình

  Để giải quyết vấn đề trên thì Vue cung cấp cho chúng ta `ref()` để Vue giúp chúng ta theo dõi sự thay đổi đồng thời cập nhật lại dữ liệu mới lên giao diện

  Khi component render lần đầu, Vue sẽ theo dõi (track) tất cả các ref được sử dụng. Khi ref thay đổi giá trị, Vue sẽ kích hoạt (trigger) cập nhật lại component để hiển thị giá trị mới.

- Tại sao lại phải `.value`?
  Vue sử dụng một cơ chế đặc biệt **dependency-tracking based reactivity system** để theo dõi sự thay đổi

  ```js
  // pseudo code, not actual implementation
  const myRef = {
    _value: 0,
    get value() {
      track();
      return this._value;
    },
    set value(newValue) {
      this._value = newValue;
      trigger();
    },
  };
  ```

  - Khi chúng ta `.value`, Vue sẽ gọi `getter` để lấy giá trị
  - Khi chúng ta `.value = 10`, Vue sẽ gọi tới `setter` để cập nhật giá trị và kích hoạt trigger

- **Deep reactivity**
  Vue có thể theo dõi thay đổi sâu trên **object** và **array** trong `ref()`

  ```js
  <script setup>
  import { ref } from 'vue'

  const numbers = ref([1, 2, 3])

  const addNumber = () => {
    numbers.value.push(4)  // numbers.value phải được cập nhật đúng cách
  }
  </script>

  <template>
    <div>
      <p>Danh sách số: {{ numbers }}</p>
      <button @click="addNumber">Thêm số</button>
    </div>
  </template>
  ```

#### 3.2. Reactive

Có một cách khác để tạo ra trạng thái phản ứng với `reactive()`. Tuy nhiên `reactive()` sẽ tạo ra một **reaction object** và theo dõi tất cả các thuộc tính của đối tượng đó. Để có thể truy cập dữ liệu, không cần sử dụng `.value` và có thể truy cấp bình thường như một đối tượng

```js
<script setup>
import { reactive } from 'vue'

const user = reactive({ name: 'Alice', age: 25 })

const updateName = () => {
  user.name = 'Bob'  // Không cần .value
}
</script>

<template>
  <div>
    <p>Tên: {{ user.name }}</p>
    <button @click="updateName">Đổi tên</button>
  </div>
</template>
```

- **Một số giới hạn của reactive:**
  - **Giới hạn kiểu dữ liệu**: Chỉ có thể hoạt động với kiểu dữ liệu đối tượng như **objects**, **arrays**. Không thể nhận giá trị là kiểu nguyên thủy **primitive types** như **string**, **number** hoặc **boolean**
  - **Không thể gán một biến mới**: Sau khi tạo một biến trạng thái phản ứng với `reactive()`, ta không thể gán lại biến đó một đối tượng mới hay một trạng thái phản ứng mới
  - **Destructuring không vui**: Bởi vì là một đối tượng, ta có thể destructuring để lấy ra nhanh các thuộc tính trong đối tượng. Tuy nhiên sau khi lấy ra được các thuộc tính đó, Vue sẽ không thể theo dõi được sự thay đổi của các thuộc tính đó nữa

#### 3.3. Additional Ref Unwrapping Details

Vue có cơ chế **unwrap ref** giúp `reactive()` và `ref()` có thể kết hợp với nhau.

Ví dụ: Khi sử dụng `reactive()` chứa `ref()`, Vue sẽ tự động unwrap.

```js
<script setup>
import { reactive, ref } from 'vue'

const user = reactive({
  name: ref('Alice'),
  age: 25
})

console.log(user.name)  // ✅ Không cần user.name.value
</script>
```

#### 3.3. Khi nào dùng ref và reactive

| `ref()`                                                                    |                                         `reactive()` |
| :------------------------------------------------------------------------- | ---------------------------------------------------: |
| Phù hợp với kiểu dữ liệu nguyên thủy (**string**, **number**, **boolean**) |                Phù hợp với **Object** hoặc **Array** |
| Cần `.value` để lấy ra giá trị                                             |                                   Không cần `.value` |
| Có thể gán lại                                                             | Không thể gán lại, chỉ cập nhật thuộc tính bên trong |

### 4. Deploy Vercel
