#### 1. Computed

Trước khi đi vào tìm hiểu `computed` chúng ta có một ví dụ nhỏ như sau:

```js
<script setup>
import reactive from 'vue'

const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
})
</script>
<template>
    <p>Has published books:</p>
    <span>{{ author.books.length > 0 ? 'Yes' : 'No' }}</span>
</template>
```

Trong `template`, vue cho phép chúng ta sử dụng các `expression` hay biểu thức toán học tuy nhiên trong trường chúng ta viết logic quá nhiều sẽ khiến code của chúng ta bị rối, khó nhìn và khó bảo trì.

Ở ví dụ trên chỉ là một ví dụ đơn giản nhưng khi chúng ta viết code nhiều hơn có thể sẽ gây ra tình trạng ở trên.

Nhìn vào ví dụ ở trên, chúng ta có thể thấy được rằng kết quả `Yes` hoặc `No` in lên màn dựa trên độ dài thuộc tính `books` của đối tượng `author`.

Cách này hoàn toàn đúng tuy nhiên nếu trường hợp chúng ta không muốn lặp lại nhiều lần dòng `author.books.length > 0 ? 'Yes' : 'No'` hoặc xử lý những logic phức tạp hơn thế trong template có liên quan tới `reactive data`, chúng ta ta nên sử dụng tới `computed` được cung cấp bởi Vue.

Và đây là kết quả sau khi sử dụng `computed`

```js
<script setup>
import { reactive, computed } from 'vue'

const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
})

// a computed ref
const publishedBooksMessage = computed(() => {
  return author.books.length > 0 ? 'Yes' : 'No'
})
</script>

<template>
  <p>Has published books:</p>
  <span>{{ publishedBooksMessage }}</span>
</template>
```

Nhìn ví dụ này, chúng ta có thể thấy rõ rằng mọi logic đều được xử lý trong script và có sự phân biệt rõ ràng, cụ thể nhiệm vụ giữa các phần

**Vậy thì yêu cầu và cơ chế hoạt động của `computed` là gì?**

Tiếp tục ở ví dụ sử dụng `computed` ở trên,

- **Yêu cầu:**
  - `computed` yêu cầu một tham số là một hàm, tức truyền vào đối số là một **`getter function`**
  - Gía trị trả về của `computed` là `computed ref`. Tương tự với `ref`, ta sẽ lấy ra kết quả bằng `.value`
- **Cơ chế hoạt động:**
  `computed` sẽ theo dõi sự thay đổi của các reactive mà nó phụ thuộc vào. Tức là Vue sẽ nhận biết được `publishBookMessage` phụ thuộc vào `author.books`, do đó mỗi khi `author.books` có sự thay đổi, nó sẽ cập nhật, binding dữ liệu trả về cho `publishBookMessage`

**Sự khác nhau giữa `computed` và phương thức method**
|Method|`computed`|
|--|--|
|Phương thức chỉ chạy khi được gọi|Nhận biết sự thay đổi của biến reactive và chạy lại|
|Các logic tính toán phụ thuộc vào biến reactive|Logic xử lý hành động hoặc nhiệm vụ bất kì|
|Chỉ tính lại khi vì có cơ chế `caching`|Luôn chạy mỗi lần render|

**Những điều nghiêm cấm khi sử dụng `computed`**

- Thay đổi state, các biến reactive
- Gọi API bất đồng bộ
- Tác động tới DOM
- Gía trị của `computed` chỉ có thể đọc, không thể thay đổi nó trực tiếp

#### 2. Class and Style binding

Khi chúng ta mong muốn các thành phần html sẽ nhận được **class** hay **style** theo một điều kiện nào đó, thay vì xử lý logic phức tạp với JS, Vue cung cấp cho ta khả năng binding class và style theo một điều kiện nào đó với directive là `v-bind:class` và `v-bind:style`, viết ngắn gọn hơn là `:class` và `:style`

##### 2.1. Class binding

- Chúng ta có ví dụ nhỏ như sau:

```js
<script setup>
  const isActive = ref(true)
  const hasError = ref(false)
</script>
<template>
  <div
    class="static"
    :class="{ active: isActive, 'text-danger': hasError }"
  ></div>
</template>
```

Trong ví dụ ở trên, `:class="{ active: isActive, 'text-danger': hasError }"` đây chính là cú pháp sử dụng class binding trong template. Chúng ta sẽ truyền vào class binding một đối tượng object với các key là tên class

Khi này nếu `isActive` có giá trị true, thẻ `div` sẽ có thêm class là `active`, còn `hasError` đang là false, thẻ `div` sẽ không có class `text-danger`

- Ta có ví dụ nhỏ tiếp theo như sau:

```js
<script setup>
  const classObject = reactive({
    active: true,
    'text-danger': false
  })
</script>
<template>
  <div :class="classObject"></div>
</template>
```

Ngoài cách viết đối tượng class trực tiếp trong template, ta có thể tạo một đối tượng classObject trong script và truyền class đó vào `:class` trong template. Kết quả chúng ta nhận được tương tự như ví dụ trên

- Chúng ta có thể kết hợp `computed` đã học ở bên trên:

```js
<script setup>
  const isActive = ref(true)
  const error = ref(null)

  const classObject = computed(() => ({
    active: isActive.value && !error.value,
    'text-danger': error.value && error.value.type === 'fatal'
  }))
</script>
<template>
  <div :class="classObject"></div>
</template>
```

- Ở ví dụ tiếp theo, chúng ta có thể binding class bằng một mảng array như sau:

```js
<script setup>
  const activeClass = ref('active active-blue')
  const errorClass = ref('text-danger')
</script>
<template>
  <div :class="[activeClass, errorClass]"></div></template>
```

Lúc này chúng ta sẽ nhận được kết quả như sau:

```js
<div class="active active-blue text-danger"></div>
```

#### 2.2. Style binding

Cú pháp binding style khá giống với cách ta binding class

- Ví dụ đầu tiên:

```js
<script setup>
  const activeColor = ref('red')
  const fontSize = ref(30)
</script>
<template>
  <div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
</template>
```

Lúc này ta sẽ nhận được kết quả như sau:

```js
<div style="color: red; font-size: 30px;"></div>
```

Có một điều chúng ta cần chú ý ở đây là tên các thuộc tính được viết theo kiểu **camelCase** nhưng chúng ta hoàn toàn có thể viết theo kiểu **kebab-cased**

```js
<template>
  <div :style="{ color: activeColor, 'font-size': fontSize + 'px' }"></div>
</template>
```

- Tương tự như binding class, chúng ta có thể tạo ra một styleObject trong script và dùng nó trong template

```js
<script setup>
  const styleObject = reactive({
    color: 'red',
    fontSize: '30px'
  })
</script>
<template>
  <div :style="styleObject"></div>
</template>
```

- Tương tự như binding class thì chúng ta có thể bind `:style` với một mảng các thuộc tính

#### 3. Components basic

- Component là những khối giao diện độc lập và tái sử dụng được. Thay vì viết tất cả giao diện trong một file lớn, chúng ta có thể chia nhỏ nó thành các thành phần nhỏ hơn như: Header, Footer, Button,... Và mỗi component này đều có phần đuôi mở rộng là `.vue` và gồm ba phần `<script></script>`, `<template></template>`, `<style></style>`

![alt text](img/week-4/image.png)

- Tạo và sử dụng một component như thế nào?

Chúng ta chỉ cần tạo ra một file `.vue` và xây dựng logic, giao diện tương ứng cho component đó

```js
<script setup>
import { ref } from "vue";

const count = ref(0);
</script>

<template>
  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
```

Để có thể sử dụng component vừa tạo ta chỉ cần import nó và sử dụng nó trong template là được rồi

```js
<script setup>
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <HelloWorld />
</template>
```

- Khi bạn sử dụng component trong template, bạn hoàn toàn có thể áp dụng các directive như:
  - `v-if`, `v-else`, `v-show`: Điều kiện hiển thị
  - `v-for`: Lặp component
  - `v-model`: Binding dữ liệu hai chiều
  - `v-bind`: Truyền props
  - `v-on`: Lắng nghe sự kiện
