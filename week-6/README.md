### 1. Lifecycle hook

##### Lifecycle là gì?

Mỗi khi một ứng dụng VueJS được tạo, một instance mới của component được tạo ra để giúp lập trình viên xây dựng hành vi mong muốn. Nó bao gồm các tùy chọn khác nhau như templates, methods, props, watchers, lifecycles, data và nhiều thứ khác

Lifecycle(Vòng đời) của một component là quá trình từ lúc được tạo ra, gắn vào DOM, được mounted(gắn kết), cập nhật dữ liệu, cho đến khi bị hủy bỏ.

Việc hiểu rõ các vòng đời này giúp lập trình thêm code, tác động vào các giai đoạn khác nhau để thực hiện các tác vụ như:

- **Khởi tạo dữ liệu:** Lấy dữ liệu từ server, khởi tạo các biến
- **Gắn kết DOM:** Thực hiện các thao tác với DOM sau khi component được render
- **Cập nhật giao diện:** Cập nhật giao diện khi thay đổi dữ liệu
- **Dọn dẹp:** Hủy bỏ các listener, timer trước khi component bị xóa khỏi DOM

##### Lifecycle hook

Lifecycle hook trong Vue.js là các phương thức được gọi tại những thời điểm khác nhau trong vòng đời của một component. Mỗi component trong Vue.js trải qua một vòng đời từ khi được khởi tạo, render, cập nhật, cho đến khi bị gỡ bỏ khỏi DOM. Trong quá trình này, Vue cung cấp các hook để lập trình viên có thể can thiệp và thực hiện các thao tác tại các giai đoạn cụ thể của vòng đời component

Các hook trong vòng đời của một component:
|Lifecycle hook|Mô tả|
|--|--|
|beforeCreate|Xảy ra trước tất cả hook trong vòng đời khác.|
|created|Component được khởi tạo và bạn có thể truy cập các thuộc tính của component|
|mounted|Component đã được gắn vào cây DOM, cho phép truy cập các phần tử DOM|
|beforeUpdate|Xảy ra khi hệ thống phản ứng của Vue phát hiện có sự thay đổi cần một lần render mới|
|updated|Xảy ra ngay khi cây DOM đã được cập nhật|
|beforeUnmmount|Xảy ra ngay trước khi một component bị gỡ bỏ khỏi DOM|
|unmounted|Xảy ra khi một component bị gỡ bỏ khỏi DOM|

![alt text](image.png)

##### Giai đoạn 1: Creation - Khởi tạo

- **beforeCreate**
  **beforeCreate** là giai đoạn đầu tiên trong quá trình tạo ra một instance Vue. Tại giai đoạn này, instance vẫn chưa hoàn thiện và chúng ta chưa thể truy cập vào dữ liệu, phương thức, hay các thuộc tính tính toán của component. Hãy hình dung như bạn đang chuẩn bị nguyên liệu để làm bánh, nhưng chưa bắt đầu nhào bột.

  Lifecycle hook **beforeCreate** được gọi trước khi Vue thiết lập các thuộc tính phản ứng (reactive), phương thức, và các trình lắng nghe sự kiện. Điều này có nghĩa là bạn không nên cố gắng truy cập vào các phần tử thuộc về component từ lifecycle hook này.

  !!!Lưu ý: Bạn có thể sử dụng **beforeCreate** để thiết lập một số biến toàn cục hoặc thực hiện các tác vụ khởi tạo ban đầu khác. Tuy nhiên, không nên cố gắng thao tác với DOM hoặc truy cập vào dữ liệu của component.

  Ví dụ: Sử dụng hook **beforeCreate** để tạo một cảnh báo, ghi vào console, và thử thay đổi thuộc tính dữ liệu ‘text’ nhưng không thành công.

  **App.vue**

  ```html
  <template>
    <h1>The 'beforeCreate' Lifecycle Hook</h1>

    <p>
      We can see the console.log() message from 'beforeCreate' lifecycle hook,
      but there is no effect from the text change we try to do to the Vue data
      property, because the Vue data property is not created yet.
    </p>

    <button @click="activeComp = !activeComp">Add/Remove Component</button>

    <div>
      <CompOne v-if="activeComp" />
    </div>
  </template>

  <script setup>
    import { ref } from "vue";
    import CompOne from "./CompOne.vue"; // Đường dẫn này tùy vào vị trí file của bạn

    const activeComp = ref(false);
  </script>

  <style>
    #app > div {
      border: dashed black 1px;
      border-radius: 10px;
      padding: 10px;
      margin-top: 10px;
      background-color: lightgreen;
    }

    #pResult {
      background-color: lightcoral;
      display: inline-block;
    }
  </style>
  ```

  **CompOne.vue**

  ```html
  <template>
    <h2>Component</h2>
    <p>This is the component</p>
    <p id="pResult">{{ text }}</p>
  </template>

  <script setup>
    import { ref, onBeforeMount } from "vue";

    const text = ref("...");

    // onBeforeMount gần tương đương với beforeCreate trong Vue 3
    onBeforeMount(() => {
      text.value = "initial text";
      console.log("beforeCreate: The component is not created yet.");
    });
  </script>
  ```

- **created**
  Giai đoạn **created** là thời điểm mà một component Vue đã được khởi tạo hoàn chỉnh. Tại đây, tất cả các dữ liệu, phương thức, thuộc tính tính toán (computed properties) và các trình lắng nghe sự kiện (event listeners) đã sẵn sàng để sử dụng. Hãy hình dung như một chiếc bánh đã được nhào xong và các nguyên liệu đã được chuẩn bị đầy đủ, sẵn sàng để đưa vào lò nướng.

  Lifecycle hook **created** được gọi sau khi component khởi tạo, xảy ra sau “beforeCreate” và trước “beforeMount” vì đây là thời điểm thích hợp để:

  - **Truy cập dữ liệu**: Bạn có thể truy cập và thao tác với dữ liệu của component.
  - **Gọi các phương thức**: Bạn có thể gọi các phương thức đã được định nghĩa trong component.
  - **Thực hiện các tác vụ khởi tạo**: Ví dụ, thực hiện các yêu cầu HTTP để lấy dữ liệu từ server và cập nhật dữ liệu của component.

  Tuy nhiên, bạn không nên cố gắng truy cập trực tiếp vào các phần tử DOM từ giai đoạn **created** vì DOM chưa được gắn vào (mount) vào trang web tại thời điểm này.

  Ví dụ: Sử dụng hook **created** để thay đổi thuộc tính dữ liệu ‘text’.

##### Giai đoạn 2: Mount – Thao tác với cây DOM

Giai đoạn gắn kết (Mounting) là giai đoạn mà component Vue được kết nối với DOM (Document Object Model), tức là nó được hiển thị trên màn hình. Trong giai đoạn này, Vue sẽ thay thế một phần tử trong DOM bằng cấu trúc HTML được định nghĩa trong template của component.

- **Before mount**
  Trước khi component được gắn vào DOM, Vue sẽ kiểm tra xem template có sẵn hay không. Giai đoạn này chủ yếu dùng để chuẩn bị một số việc trước khi component được hiển thị. Chúng ta nên tránh cố gắng truy cập các phần tử DOM từ hook vòng đời beforeMount, vì các phần tử DOM không thể truy cập được cho đến khi component được gắn kết.

  Ví dụ: Sử dụng hook mounted để đặt con trỏ chuột vào trường nhập liệu ngay khi component được gắn vào.

  ```html
  <template>
    <h2>Example: onBeforeMount</h2>
    <p>{{ message }}</p>
  </template>

  <script setup>
    import { ref, onBeforeMount } from "vue";

    const message = ref("Initial text");

    onBeforeMount(() => {
      console.log(
        "onBeforeMount: Component is about to be mounted to the DOM."
      );
      message.value = "Updated before mount";
    });
  </script>
  ```

- **Mounted**
  Sau khi component đã được gắn vào DOM, Vue sẽ gọi hook **mounted**. Đây là thời điểm thích hợp để bạn thực hiện các thao tác liên quan đến DOM, như thêm sự kiện, truy cập các phần tử DOM, hoặc thực hiện các yêu cầu AJAX để lấy dữ liệu.

  Ví dụ: Sử dụng hook **mounted** để đặt con trỏ vào bên trong trường nhập liệu ngay khi component được gắn vào DOM.

  ```html
  <template>
    <h2>Example: onMounted</h2>
    <p>{{ message }}</p>
  </template>

  <script setup>
    import { ref, onMounted } from "vue";

    const message = ref("Loading...");

    // Hàm sẽ chạy sau khi component đã gắn vào DOM
    onMounted(() => {
      console.log("onMounted: Component has been mounted to the DOM.");
      message.value = "Component is mounted!";
    });
  </script>
  ```

##### Giai đoạn 3: Update – Cập nhật

Giai đoạn cập nhật bao gồm các hook beforeUpdated và updated, khá hữu ích trong việc gỡ lỗi, thay đổi trạng thái phản ứng của component hoặc xảy ra render lại.

- **Before update**
  Đây là thời điểm ngay trước khi giao diện được cập nhật. Bạn có thể tận dụng giai đoạn này để kiểm tra xem dữ liệu mới có hợp lệ không hoặc thực hiện một số chuẩn bị trước khi giao diện thay đổi.

  Một điều đặc biệt về hook beforeUpdate là bạn có thể thực hiện thay đổi cho ứng dụng mà không cần kích hoạt bản cập nhật mới, nhờ đó tránh được vòng lặp vô hạn. Đó là lý do tại sao không nên thực hiện thay đổi cho dữ liệu reactive trong hook updated, vì với hook đó, một vòng lặp vô hạn sẽ được tạo ra.

  ```html
  <template>
    <h2>Example: onBeforeUpdate</h2>
    <p>Current count: {{ count }}</p>
    <button @click="increment">Increment</button>
  </template>

  <script setup>
    import { ref, onBeforeUpdate } from "vue";

    const count = ref(0);

    onBeforeUpdate(() => {
      console.log("onBeforeUpdate: Component is about to be updated!");
    });

    const increment = () => {
      count.value++;
    };
  </script>
  ```

- **Updated**
  Hook Updated là giai đoạn sau khi giao diện đã được cập nhật xong. DOM được cập nhật dưới hook này và các hoạt động phụ thuộc vào DOM đã có thể thực hiện như thêm sự kiện, cuộn đến một phần tử cụ thể,…

  ```html
  <template>
    <h2>Example: onUpdated</h2>
    <p>Current count: {{ count }}</p>
    <button @click="increment">Increment</button>
  </template>

  <script setup>
    import { ref, onUpdated } from "vue";

    const count = ref(0);

    onUpdated(() => {
      console.log("onUpdated: Component has been updated!");
    });

    const increment = () => {
      count.value++;
    };
  </script>
  ```

##### Giai đoạn 4: Unmount – Gỡ bỏ

Unmount là một giai đoạn trong vòng đời của một component Vue, diễn ra khi component đó bị xóa khỏi DOM. Nói cách khác, khi một component không còn được hiển thị trên giao diện người dùng nữa, thì nó sẽ trải qua giai đoạn unmount.

- **Before Unmount**
  Đây là giai đoạn trước khi component bị gỡ bỏ khỏi DOM. Ở giai đoạn này, bạn có thể thực hiện các thao tác như hủy bỏ các sự kiện, ngừng lắng nghe thay đổi dữ liệu, hoặc dọn dẹp tài nguyên như bộ hẹn giờ, yêu cầu API đang chờ, hoặc kết nối WebSocket.

  Tại thời điểm “beforeUnmount”, component vẫn hoàn toàn hoạt động. Tất cả các thuộc tính, computed properties, và methods vẫn có thể truy cập được. Khi một component cha bị unmount, tất cả các component con của nó cũng sẽ bị unmount.

  ```html
  <template>
    <h2>Example: onBeforeUnmount</h2>
    <p>Current count: {{ count }}</p>
    <button @click="increment">Increment</button>
    <button @click="toggleComponent">Toggle Component</button>

    <div v-if="isVisible">
      <p>Component will be unmounted soon...</p>
    </div>
  </template>

  <script setup>
    import { ref, onBeforeUnmount } from "vue";

    const count = ref(0);
    const isVisible = ref(true);

    const increment = () => {
      count.value++;
    };

    const toggleComponent = () => {
      isVisible.value = !isVisible.value;
    };

    // This will run just before the component is unmounted
    onBeforeUnmount(() => {
      console.log(
        "onBeforeUnmount: Component is about to be removed from the DOM."
      );
    });
  </script>
  ```

- **Unmounted**
  Đây là giai đoạn khi component đã hoàn toàn bị gỡ khỏi DOM. Trong Vue.js 3, bạn có thể sử dụng hook unmounted để kiểm tra hoặc thực hiện thao tác khi DOM đã bị xóa.

  Ví dụ: Sử dụng hook unmounted để tạo một cảnh báo khi component bị gỡ bỏ khỏi DOM.

  ```html
  <template>
    <h2>Example: onUnmounted</h2>
    <p>Current count: {{ count }}</p>
    <button @click="increment">Increment</button>
    <button @click="toggleComponent">Toggle Component</button>

    <div v-if="isVisible">
      <p>The component will be unmounted soon...</p>
    </div>
  </template>

  <script setup>
    import { ref, onUnmounted } from "vue";

    const count = ref(0);
    const isVisible = ref(true);

    const increment = () => {
      count.value++;
    };

    const toggleComponent = () => {
      isVisible.value = !isVisible.value;
    };

    // This will run once the component has been unmounted
    onUnmounted(() => {
      console.log("onUnmounted: Component has been removed from the DOM.");
    });
  </script>
  ```

-> Nguồn tham khảo ở đây: [Lifecycle trong Vuejs](https://itviec.com/blog/lifecycle-vuejs-la-gi/)

| **Hook**                | **Mô Tả**                                                                                                                                  | **Khi nào được gọi**                                       |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------- |
| **`onBeforeMount()`**   | Được gọi **trước khi component được mount vào DOM**.                                                                                       | Trước khi component được gắn vào DOM lần đầu tiên.         |
| **`onMounted()`**       | Được gọi **sau khi component được mount vào DOM**. Đây là nơi bạn thực hiện các thao tác liên quan đến DOM hoặc thao tác bất đồng bộ.      | Sau khi component đã được mount vào DOM.                   |
| **`onBeforeUpdate()`**  | Được gọi **trước khi component update** (khi dữ liệu thay đổi). Có thể dùng để chuẩn bị hoặc kiểm tra các thay đổi trước khi cập nhật DOM. | Trước khi component render lại (update).                   |
| **`onUpdated()`**       | Được gọi **sau khi component đã được cập nhật** (sau khi re-render).                                                                       | Sau khi component được render lại và DOM đã được cập nhật. |
| **`onBeforeUnmount()`** | Được gọi **trước khi component bị unmount** khỏi DOM. Thường dùng để dọn dẹp, hủy đăng ký sự kiện, hoặc hủy các tác vụ bất đồng bộ.        | Trước khi component bị gỡ bỏ khỏi DOM.                     |
| **`onUnmounted()`**     | Được gọi **sau khi component bị unmounted** khỏi DOM. Đây là nơi để thực hiện dọn dẹp tài nguyên, hủy các tác vụ bất đồng bộ.              | Sau khi component đã bị gỡ bỏ khỏi DOM.                    |

### 2. Event / Emit

Trong Vue 3, **Event** và **Emit** là hai khái niệm quan trọng để truyền dữ liệu và tương tác giữa các component. Chúng giúp các component con giao tiếp với component cha, giúp duy trì tính tái sử dụng và khả năng mở rộng của các ứng dụng Vue.

##### 2.1 **Event** trong Vue 3

**Event** trong Vue là một cơ chế để gửi tín hiệu hoặc thông báo từ **component con** tới **component cha**. Để làm điều này, component con sẽ phát ra một sự kiện mà component cha có thể lắng nghe.

##### 2.2 **Emit** trong Vue 3

**Emit** là một phương thức được sử dụng trong **component con** để phát ra một sự kiện. Sau khi sự kiện được phát ra, **component cha** có thể lắng nghe và xử lý sự kiện đó.

Cấu trúc phát sự kiện trong Vue rất đơn giản, bạn chỉ cần sử dụng phương thức `emit()` trong component con để phát ra một sự kiện và truyền dữ liệu nếu cần.

##### 2.3 **Cách sử dụng Event/Emit**

**2.3.1. Emit sự kiện từ Component Con**

Để phát sự kiện từ **component con**, ta sử dụng phương thức `emit()` mà Vue 3 cung cấp.

**Ví dụ: Component Con Emit sự kiện**

```html
<template>
  <button @click="sendDataToParent">Click me to send data</button>
</template>

<script setup>
  import { defineEmits } from "vue";

  const emit = defineEmits();

  const sendDataToParent = () => {
    emit("customEvent", "Hello Parent!");
  };
</script>
```

Trong ví dụ này:

- Khi người dùng nhấn vào nút, phương thức `sendDataToParent` sẽ phát ra sự kiện `'customEvent'` và truyền dữ liệu `'Hello Parent!'` tới component cha.

**2.3.2. Lắng nghe sự kiện trong Component Cha**

Component cha sẽ **lắng nghe** sự kiện được phát ra từ component con bằng cách sử dụng cú pháp `@event-name="method"` hoặc `v-on:event-name="method"`.

**Ví dụ: Component Cha lắng nghe sự kiện**

```html
<template>
  <ChildComponent @customEvent="handleCustomEvent" />
</template>

<script setup>
  import ChildComponent from "./ChildComponent.vue";

  const handleCustomEvent = (message) => {
    console.log("Received from child:", message);
  };
</script>
```

Trong ví dụ này:

- Component cha sử dụng `@customEvent="handleCustomEvent"` để lắng nghe sự kiện `'customEvent'` từ component con.
- Khi sự kiện được phát ra, phương thức `handleCustomEvent` trong component cha sẽ nhận dữ liệu và xử lý nó.

**2.3.3. Truyền Dữ Liệu từ Component Con sang Component Cha**

Dữ liệu có thể được truyền từ component con tới component cha thông qua `emit` khi phát ra sự kiện. Trong trường hợp này, dữ liệu là đối số thứ hai được truyền cùng với tên sự kiện.

**Ví dụ: Truyền dữ liệu từ Component Con sang Component Cha**

```html
<!-- Component Con -->
<template>
  <button @click="sendDataToParent">Click me to send data</button>
</template>

<script setup>
  import { defineEmits } from "vue";

  const emit = defineEmits();

  const sendDataToParent = () => {
    const data = { name: "Vue 3", type: "Framework" };
    emit("customEvent", data);
  };
</script>
```

```html
<!-- Component Cha -->
<template>
  <ChildComponent @customEvent="handleCustomEvent" />
</template>

<script setup>
  import ChildComponent from "./ChildComponent.vue";

  const handleCustomEvent = (data) => {
    console.log("Received from child:", data);
  };
</script>
```

Khi người dùng nhấn vào nút trong **Component Con**, một đối tượng dữ liệu sẽ được gửi qua sự kiện `'customEvent'`. Component cha nhận được đối tượng này và có thể sử dụng nó trong phương thức `handleCustomEvent`.

##### 3.4 **Tùy Chọn `defineEmits`**

Vue 3 sử dụng **`defineEmits`** trong Composition API để khai báo sự kiện mà component con có thể phát ra. Việc này giúp bạn dễ dàng quản lý các sự kiện trong component mà không cần phải định nghĩa một đối tượng `methods` như trong Vue 2.

- **`defineEmits()`** cho phép khai báo các sự kiện mà component có thể phát ra.
- Đối số của `defineEmits()` có thể là một mảng tên sự kiện hoặc một đối tượng với tên sự kiện và các tùy chọn khác như `validator` để xác nhận giá trị.

**Ví dụ:**

```html
<script setup>
  import { defineEmits } from "vue";

  const emit = defineEmits(["customEvent"]);
</script>
```

Hoặc, sử dụng **`defineEmits`** với **validator**:

```html
<script setup>
  const emit = defineEmits({
    customEvent: (payload) => typeof payload === 'string'

    // No validation
    click: null,

    // Validate submit event
    submit: ({ email, password }) => {
      if (email && password) {
        return true;
      } else {
        console.warn("Invalid submit event payload!");
        return false;
      }
    },
  });

  function submitForm(email, password) {
    emit("submit", { email, password });
  }
</script>
```

##### 2.6 **Phát nhiều sự kiện**

Một component có thể phát nhiều sự kiện bằng cách gọi `emit()` nhiều lần. Bạn có thể phát ra bất kỳ số lượng sự kiện nào trong một component con.

**Ví dụ:**

```html
<script setup>
  const emit = defineEmits();

  const sendMultipleEvents = () => {
    emit("firstEvent", "Data for first event");
    emit("secondEvent", "Data for second event");
  };
</script>
```
