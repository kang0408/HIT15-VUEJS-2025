#### 1. Watchers

Watcher là một hàm quan sát sự thay đổi của một biến reactive (như ref, reactive, hoặc biến data) và thực hiện hành động nào đó khi giá trị thay đổi.

Tại sao cần watch? Khi bạn muốn phản ứng lại thay đổi dữ liệu nhưng không cần tính giá trị mới (như computed).Thường dùng cho:

- Gửi API khi dữ liệu thay đổi.
- Theo dõi route params, query...

Chúng ta sẽ cùng làm một vài ví dụ để hiểu hơn về watcher để xem đầu đuôi nó như thế nào nhé🚀🚀.

Ta cùng xem đoạn code này nhé

```html
<script setup>
  import { ref } from "vue";

  const count = ref(0);

  function updateCount() {
    count.value++;
  }
</script>

<template>
  <h1>{{ count }}</h1>
  <button @click="updateCount">Update Count</button>
</template>
```

Chạy lên ta sẽ có kết quả như sau, bấm vào nút ta sẽ thấy số count tăng lên:

![alt text](img/week-5/image-1.png)

Bây giờ ta muốn watch để mỗi khi mà count thay đổi thì ta sẽ in ra console nhé:

```html
<script setup>
  import { ref, watch } from "vue";

  const count = ref(0);

  watch(count, (current) => {
    console.log("Current value of count:", current);
  });

  function updateCount() {
    count.value++;
  }
</script>

<template>
  <h1>{{ count }}</h1>
  <button @click="updateCount">Update Count</button>
</template>
```

Ở trên các bạn thấy ta dùng watch được import từ Vue, truyền vào 2 tham số:

- tham số đầu tiên là cái ref count, ta gọi nó là "source" - nguồn, ý là ta muốn theo dõi sự thay đổi của cái nguồn nào
- tham số thứ 2 là 1 cái callback, tham số trả về của callback là giá trị hiện tại của cái source mà ta đang watch

Sau đó ta lưu lại và chạy lên, bấm vào nút để tăng count ta sẽ thấy ở console in ra các giá trị của count thay đổi theo thời gian như sau:

![alt text](img/week-5/image-2.png)

Ví dụ tương tự với reactive():

```html
<script setup>
  import { reactive, watch } from "vue";

  const state = reactive({ count: 0 });

  watch(state, (current) => {
    console.log("Current value of count:", current);
  });

  function updateCount() {
    state.count++;
  }
</script>

<template>
  <h1>{{ state.count }}</h1>
  <button @click="updateCount">Update Count</button>
</template>
```

![alt text](img/week-5/image-3.png)

Ta cũng có thể watch nhiều source cùng một lúc bằng cách truyền vào 1 array chứa nhiều source, callback trả về sẽ tương ứng là 1 mảng chứa giá trị hiện tại của các source (theo thứ tự ta khai báo):

```html
<script setup>
  import { ref, watch } from "vue";

  const count1 = ref(0);
  const count2 = ref(0);

  watch([count1, count2], (current) => {
    console.log("Current value:", current);
  });

  function updateCounts() {
    count1.value++;
    count2.value += 2;
  }
</script>

<template>
  <h1>{{ count1 }} - {{ count2 }}</h1>
  <button @click="updateCounts">Update Count</button>
</template>
```

Chạy lên sẽ cho kết quả như sau:

![alt text](img/week-5/image-4.png)

Chú ý rằng watch sẽ chạy khi bất kì source nào thay đổi

Vậy giờ muốn watch tổng giá trị của count1 và count2 thì có được không nhỉ??? 🤔🤔🤔

Lại chả được quá ấy chớ, Vue support hết 😎😎, ta sửa lại chút nhé:

```html
<script setup>
  import { ref, watch } from "vue";

  const count1 = ref(0);
  const count2 = ref(0);

  watch(
    () => count1.value + count2.value,
    (current) => {
      console.log("Current value:", current);
    }
  );

  function updateCounts() {
    count1.value++;
    count2.value += 2;
  }
</script>

<template>
  <h1>{{ count1 }} - {{ count2 }}</h1>
  <button @click="updateCounts">Update Count</button>
</template>
```

Ở trên, ta để ý rằng ở tham số đầu tiên truyền vào watch ta đã sửa thành 1 cái callback, callback này trả về giá trị là tổng value của count1 và count2, và vì tổng này là number nên current cũng sẽ cho ta number, chạy lên ta thấy như sau:

![alt text](img/week-5/image-5.png)

Ta cũng có thể mix chúng với nhau như sau nhé:

```html
<script setup>
  import { ref, watch } from "vue";

  const count1 = ref(0);
  const count2 = ref(0);
  const count3 = ref(0);

  watch([() => count1.value + count2.value, count3], (current) => {
    console.log("Current value:", current);
  });

  function updateCounts() {
    count1.value++;
    count2.value += 2;
    count3.value += 3;
  }
</script>

<template>
  <h1>{{ count1 }} - {{ count2 }} - {{ count3 }}</h1>
  <button @click="updateCounts">Update Count</button>
</template>
```

Các bạn tự chạy lên và test coi sao nhé 😉

Có 1 chú ý quan trọng là chúng ta không thể watch trực tiếp giá trị của reactive state kiểu như sau:

```js
const count = ref(0);

watch(count.value, (current) => {
  console.log("Current value of count:", current);
});

const obj = reactive({ count: 0 });

watch(obj.count, (count) => {
  console.log(`count is: ${count}`);
});
```

Khi chạy lên ta sẽ thấy warning như sau:

![alt text](img/week-5/image-6.png)

Lí do bởi vì: watcher chỉ chạy với reactive state, khi ta truyền thẳng giá trị của nó vào thì nó chỉ là giá trị JS thường và không reactive gì cả, ở ví dụ trên thì ta đang đơn giản là watch mỗi số 0, nó không reactive. Do vậy các bạn chú ý điều này cho mình thật kĩ nhé.

Và cách giải quyết cho vấn đề trên là ta sẽ dùng callback như sau:

```js
const count = ref(0);

watch(
  () => count.value,
  (current) => {
    console.log("Current value of count:", current);
  }
);

const obj = reactive({ count: 0 });

watch(
  () => obj.count,
  (count) => {
    console.log(`count is: ${count}`);
  }
);
```

Ta muốn vừa watch vừa so sánh giá trị hiện tại và giá trị cũ thì làm như sau nhé:

```js
const count = ref(0);

watch(
  () => count.value,
  (current, old) => {
    console.log("Current value of count:", current, old);
  }
);
```

Tham số thứ 2 của callback trả về sẽ là giá trị cũ. Di dỉ dì di cái gì Vue cũng có 😂😂

Và bởi vì watcher dành cho reactive state, nên computed ta cũng watch được luôn 😎😎:

```html
<script setup>
  import { ref, watch, computed } from "vue";

  const count = ref(0);
  const double = computed(() => {
    return count.value * 2;
  });

  watch(double, (current) => {
    console.log("Current value of double:", current);
  });

  function updateCount() {
    count.value++;
  }
</script>

<template>
  <h1>{{ count }}</h1>
  <button @click="updateCount">Update Count</button>
</template>
```

Các bạn chạy lên và xem kết quả nhé:

![alt text](img/week-5/image-7.png)

##### Deep watch, Eager watch

##### 1. Deep watch

Bây giờ ta có đoạn code sau:

```html
<script setup>
  import { ref, watch } from "vue";

  const state = ref({
    a: {
      b: {
        c: {
          d: 1,
        },
      },
    },
  });

  watch(state, (current) => {
    console.log(current);
  });

  function updateState() {
    state.value.a = 1;
  }
</script>

<template>
  <h1>{{ JSON.stringify(state) }}</h1>
  <button @click="updateState">Update</button>
</template>
```

Lưu lại, F5 trình duyệt và bấm nút để cập nhật state ta sẽ thấy rằng UI đã cập nhật, nhưng không thấy watch chạy 😮😮😮

Lí do là bởi vì ở đây ta đang watch cả cái value của ref, tức là chỉ khi nào ta gán lại value bằng giá trị khác thì watch mới biết, ví dụ như sau sẽ chạy:

```js
function updateState() {
  state.value = {}; // ==>> gán lại giá trị cho cả state.value
}
```

Còn nếu ta chỉ update 1 thuộc tính nào đó mãi bên trong thì Vue không biết được, ví dụ `state.value.a = 1`.

Và để nói với Vue rằng "ê cu, cu phải theo dõi mọi thay đổi ở mọi level của state của anh", thì ta cần truyền thêm option `deep: true`, ta sửa lại code như sau nhé:

```html
<script setup>
  import { ref, watch } from "vue";

  const state = ref({
    a: {
      b: {
        c: {
          d: 1,
        },
      },
    },
  });

  watch(
    state,
    (current) => {
      console.log(current);
    },
    { deep: true }
  );

  function updateState() {
    state.value.a = 1;
  }
</script>

<template>
  <h1>{{ JSON.stringify(state) }}</h1>
  <button @click="updateState">Update</button>
</template>
```

Sau đó ta chạy lên sẽ thấy như sau:

![alt text](img/week-5/image-8.png)

Khi ta watch deep thì Vue sẽ theo dõi mọi thay đổi ở bất kì thuộc tính ở bất kì level nào trong state của chúng ta

!!! **\_\_** Nhưng các bạn cần đặc biệt chú ý rằng, khi watch deep thì Vue sẽ duyệt qua toàn bộ state của chúng ta để xem cái nào thay đổi, do vậy nếu bạn có 1 cái state mà nó là object rất lớn kiểu vài lồng cả trăm/nghìn level thì có thể sẽ ảnh hưởng tới performance đó nhé

##### 2. Eager watch (Immediate watch)

Mặc định thì watch nó sẽ chỉ chạy khi reactive state thay đổi, nhưng cũng có những trường hợp ta muốn vừa vô phát là watch chạy ngay phát đầu kể cả state chưa đổi, kiểu fetch data từ server on load chẳng hạn. Trong trường hợp đó ta sẽ dùng tới option immediate nhé:

```html
<script setup>
  import { ref, watch } from "vue";

  const count = ref(0);

  watch(
    count,
    (current) => {
      console.log("Current value of count:", current);
    },
    { immediate: true }
  );

  function updateCount() {
    count.value++;
  }
</script>

<template>
  <h1>{{ count }}</h1>
  <button @click="updateCount">Update Count</button>
</template>
```

F5 lại và ta sẽ thấy ngay từ ban đầu watch đã chạy và in ra console:

![alt text](img/week-5/image-9.png)

Người ta hay gọi đây là eager watch, ý là watch 1 cách "háo hức, chủ động" 😄, mà mình thấy lúc dịch ra tiếng Việt nó kì quá, trong khi option của Vue để là immediate, nên mình cứ dùng từ Immediate cho dễ liên tưởng

##### watchEffect()

Tiếp theo ta cùng xem đoạn code sau nhé:

```html
<script setup>
  import { ref, watch } from "vue";

  const count = ref(1);

  watch(
    count,
    async (current) => {
      const response = await fetch(
        `https://jsonplaceholder.typicode.com/todos/${current}`
      );
      const json = await response.json();
      console.log(json);
    },
    { immediate: true }
  );

  function updateCount() {
    count.value++;
  }
</script>

<template>
  <h1>{{ count }}</h1>
  <button @click="updateCount">Update Count</button>
</template>
```

Ở trên các bạn thấy là mình watch `count`, khi value của count thay đổi thì ta sẽ gọi 1 cái API và in ra response ở console. Mình dùng option immediate để cho nó chạy ngay lúc đầu 1 phát luôn.

Các bạn để ý rằng ta đang watch `count`, và ở trong callback ta cũng dùng tới giá trị của `count` luôn (đoạn `${current}`)

Vue cho chúng ta một giải pháp để đơn giản hoá việc này với `watchEffect`. Ta sửa lại code một chút như sau nhé:

```html
<script setup>
  import { ref, watchEffect } from "vue";

  const count = ref(1);

  watchEffect(async () => {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/todos/${count.value}`
    );
    const json = await response.json();
    console.log(json);
  });

  function updateCount() {
    count.value++;
  }
</script>

<template>
  <h1>{{ count }}</h1>
  <button @click="updateCount">Update Count</button>
</template>
```

Với `watchEffect()` ta để ý rằng không có source nữa, mà chỉ còn callback thôi, và callback này chạy ngay lập tức giống như option immediate: true vậy. Và Vue sẽ tự theo dõi tất cả những reactive state mà ta có ở trong cái callback luôn.

Vi diệu thế nhỉ, tự biết trong callback có gì mà track luôn 🤩🤩🤩🤩

Ở đây ta cần chú ý là, Vue sẽ theo dõi tất cả các reactive state mà ở callback ta truyền vào watchEffect() trong quá trình thực thi đồng bộ (sync), do vậy với các reactive state mà được truy cập sau await đầu tiên thì Vue sẽ không track được.

Nghe khó hiểu thế nhỉ 🤔🤔🤔🤔

Âu câu ta cùng xem ví dụ để hiểu hơn nhé:

```js
// Case 1: count2 thay đổi watchEffect sẽ không chạy
// vì count2 được truy cập sau await đầu tiên
const count1 = ref(1);
const count2 = ref(1);

watchEffect(async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${count1.value}`
  );
  const json = await response.json();
  console.log(json);
  console.log(count2.value, "++++");
});

// Case 2: tương tự trường hợp này cũng sẽ không chạy khi count2 thay đổi
const count1 = ref(1);
const count2 = ref(1);

watchEffect(async () => {
  fetch(`https://jsonplaceholder.typicode.com/todos/${count1.value}`)
    .then((res) => res.json())
    .then((json) => {
      console.log(json);
      console.log(count2.value, "++++");
    });
});

// Case 3: ✅ Chạy khi count2 thay đổi vì ta truy cập count2 trước await đầu tiên
const count1 = ref(1);
const count2 = ref(1);

watchEffect(async () => {
  console.log(count2.value, "++++");
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${count1.value}`
  );
  const json = await response.json();
  console.log(json);
});
```

Ủa tại sao lại phải là trước cái await đầu tiên, với cả như case 2 dùng Promise mà không có await thì ai biết lối nào mà lần?? 🙄🙄🙄

Thực tế thì đây liên quan tới JS Event loop và cách Vue implement watchEffect()

##### watch() hay watchEffect()???

Okie tạm tạm hiểu watch và watchEffect rồi, nhưng vẫn không hiểu là khi nào nên dùng cái nào 🧐🧐

Gòi gòi đây, ta cùng điểm qua sự khác nhau giữa chúng và khi nào dùng cái nào nhé:

- `watch()`: chỉ track những thứ ta khai báo ở source, không track có gì bên trong callback, tách biệt 2 phần rõ rệt: source và callback. Kiểu này sẽ cho ta kiểm soát được chính xác những gì ta cần watch, và khi nào thì cái callback sẽ chạy
- `watchEffect()`: kiểu này thì gộp cả 2 phần bên trên của watch() lại làm một, code sẽ (có thể) gọn hơn và tiện hơn

2 điểm lợi khác của `watchEffect`:

- trường hợp ta cần watch nhiều source thì dùng watchEffect code có thể sẽ nom gọn và đẹp hơn. Thậm chí với trường hợp nhiều source có khi ta lại quên mất 1 source nào đó trong mớ code loằng ngoằng, khi đó sẽ khá khó debug
- giả sử ta có 1 object to, lồng nhiều cấp, và ta chỉ muốn watch một số thuộc tính bất kì, thì dùng watchEffect sẽ cho performance tốt hơn, vì nó không cần duyệt toàn bộ cả cái object to đấy giống như watch + { deep: true }

##### Nâng cao chút

Thời điểm chạy callback
Giả sử giờ ta muốn kiểm tra xem là tại thời điểm callback chạy thì trên UI HTML đoạn text in ra có đúng bằng giá trị của count hay không. Ta làm như sau:

```html
<script setup>
  import { ref, watch } from "vue";

  const count = ref(0);

  watch(count, (current) => {
    const textContent = document.getElementById("count").textContent;
    const currentStr = current.toString();
    console.log(
      textContent === currentStr,
      "textContent: " + textContent,
      "current: " + current
    );
  });

  function updateCount() {
    count.value++;
  }
</script>

<template>
  <h1 id="count">{{ count }}</h1>
  <button @click="updateCount">Update Count</button>
</template>
```

Lưu lại và quay trở lại trình duyệt F5 test thử xem nhé:

![alt text](img/week-5/image-10.png)

Ô????😲😲😲, hiện tại trên UI hiển thị là 6 đúng, nhưng khi ta getElementById lại ra 5?????

Ủa ủa? cái gì vậy nhỉ????

Bình tĩnh các bạn ơi 😂😂

Thực tế là bởi vì callback được chạy trước khi Vue thực hiện update component, do vậy tại thời điểm chạy thì ở DOM HTML vẫn là giá trị trước đó (giá trị cũ)

Nếu ta muốn callback được chạy sau khi Vue update component thì ta chỉ cần thêm option { flush: 'post' } vào là được:

```js
watch(
  count,
  (current) => {
    const textContent = document.getElementById("count").textContent;
    const currentStr = current.toString();
    console.log(
      textContent === currentStr,
      "textContent: " + textContent,
      "current: " + current
    );
  },
  { flush: "post" }
);
```

Quay trở lại trình duyệt F5 và test ta sẽ thấy rằng giá trị khi ta getElementById đã bằng với giá trị hiện tại của count ở trong callback:

![alt text](img/week-5/image-11.png)

Tương tự với watchEffect:

```js
watchEffect(callback, {
  flush: "post",
});
```

Và Vue cũng cung cấp cho ta một function watchPostEffect để tiện cho watchEffect luôn:

```js
import { watchPostEffect } from "vue";

watchPostEffect(() => {
  /_ executed after Vue updates _/;
});
```

##### Dừng watcher

Mặc định thì watcher sẽ ăn theo vòng đời của component, khi component bị destroy thì watcher cũng tiêu luôn. Và thường ta cũng chả quan tâm tới việc này 😂

Nhưng nếu watcher được khai báo async (bất đồng bộ): trong setTimeout, setInterval, Promise, API call....

Thì khi đó Vue sẽ không biết có những watcher nào, bởi vì Vue chỉ biết tới những watcher được khai báo sync (đồng bộ).

Do vậy với các watcher được khai báo async thì ta sẽ phải tự stop chúng nó nếu không ta sẽ có memory leak. Ví dụ:

```js
<script setup>
  import {watchEffect} from 'vue' // ✅ Đồng bộ -> tự stop watchEffect(() => {})
  // ⛔️ Bất đồng bộ -> ta phải tự stop setTimeout(() => {watchEffect(() => {})},
  100)
</script>
```

Và để stop watcher thủ công thì ta làm như sau:

```js
const unwatch = watchEffect(() => {});

// stop thủ công, áp dụng cho tất cả các loại watcher
// watch(), watchEffect(), watchPostEffect(),...
unwatch();
```

Theo mình thấy thì rất rất rất ít khi và gần như không bao giờ ta cần dùng tới cách này, code production không cẩn thận bị memory leak debug thì khổ 😃

##### So sánh watcher và computed

Đến bước này thì ta có thể thắc mắc: vậy thay vì dùng watcher để theo dõi sự thay đổi của reactive state thì ta dùng computed property được không nhỉ? bởi vì khi reactive state thay đổi thì compute cũng chạy lại mà.

Ví dụ như sau thì có chạy không nhỉ?:

```html
<script setup>
  import { ref, watch, computed } from "vue";

  const count = ref(0);

  watch(count, (current) => {
    console.log("Current value of count:", current);
  });

  const dummyComputed = computed(() => {
    console.log("[Computed]Current value of count:", count.value);
    return count.value * 2;
  });

  function updateCount() {
    count.value++;
  }
</script>

<template>
  <h1>{{ count }}</h1>
  <button @click="updateCount">Update Count</button>
</template>
```

Câu trả lời là không 😃, bởi vì hiện tại dummyComputed không được sử dụng ở đâu cả nên nó sẽ không bao giờ chạy, thay vào đó ta phải sử dụng nó ở <template></template> (show nó ra) hoặc dùng 1 cái watch để watch nó thì nó mới chạy.

Vì vậy ta nên dùng computed và watcher cho đúng trường hợp và đúng mục đích tránh gây nhầm lẫn, khó hiểu cho đồng đội đọc code, và có thể sinh ra bug nhé 😉

-> Nguồn tham khảo 👉 [ở đây](https://viblo.asia/p/bai-6-su-dung-watcher-trong-vuejs-YWOZr00P5Q0)

#### 3. Props

Props là cách truyền dữ liệu từ component cha xuống component con trong Vue

- **Khai báo Props**

  ```html
  <script setup>
    defineProps(["foo"]);
    const props = defineProps(["foo"]);
    console.log(props.foo);
  </script>
  ```

  Ngoài cách khai báo props là một mảng các chuỗi thì ta có thể sử dụng cú pháp object

  ```html
  <script setup>
    defineProps({
      title: String,
      likes: Number,
    });

    const props = defineProps({
      title: String,
      likes: Number,
    });

    const props = defineProps({
      title: {
        type: String,
        required: true,
      },
      likes: {
        type: Number,
        default: 50,
        required: true,
      },
    });
  </script>
  ```

- **Truyền props**

  Một điều cần chú ý đó là đặt tên các props theo kiểu **camelCase**, còn khi truyền props ta sẽ viết theo kiểu `kebab-case`

  Ta có component `Child.vue` như sau:

  ```html
  <script setup>
    import { defineProps } from "vue";
    defineProps({
      greetingMessage: String,
    });
  </script>
  <template>
    <p>{{ greetingMessage }}</p>
  </template>
  ```

  Trong đoạn code trên, ta định nghĩa ra prop là `greetingMessage` có kiểu là String. Lúc này ta sẽ dùng component Child.vue và truyền props từ component `Parent.vue` như sau:

  ```html
  <script>
    import Child from "./Child.vue";
  </script>
  <template>
    <Child greeting-message="Hello world" />
  </template>
  ```

  Kết quả nhận được như sau:
  ![alt text](img/week-5/image-12.png)

  Chúng ta có một ví dụ khác như sau:

  Ở component `Child.vue`:

  ```html
  <script setup>
    import { defineProps } from "vue";

    const props = defineProps({
      number: Number,
    });
  </script>

  <template>
    <p>Số nhận được từ component cha là: {{ number }}</p>
  </template>
  ```

  Và ta sử dụng component `Child.vue` ở `Parent.vue` như sau:

  ```html
  <script setup>
    import { ref } from "vue";
    import Child from "./components/Child.vue";

    const num = ref(0);
    const addHandler = () => {
      num.value++;
    };
  </script>

  <template>
    <div>
      <Child :number="num" />
      <button @click="addHandler">Add</button>
    </div>
  </template>
  ```

  Kết quả nhận được sau mỗi lần nhấn button `Add`:

  ![alt text](img/week-5/image-13.png)

- **Static và Dynamic Props**

  Chúng ta có thể truyền props tĩnh hoặc động

  Đây là ví dụ của prop tĩnh:

  ```html
  <BlogPost title="My journey with Vue" />
  ```

  Còn đây là ví dụ về prop động, những giá trị truyền vào props mà có thể thay đổi, tác động vào khiến nó thay đổi thì là props động

  ```html
  <!-- Dynamically assign the value of a variable -->
  <BlogPost :title="post.title" />

  <!-- Dynamically assign the value of a complex expression -->
  <BlogPost :title="post.title + ' by ' + post.author.name" />
  ```

- **Truyền các kiểu dữ liệu khác**
  Ngoài kiểu dữ liệu là **string** ở trên, ta có thể truyền các kiểu dữ liệu khác sau

  - **Number**

  ```html
  <!-- Even though `42` is static, we need v-bind to tell Vue that -->
  <!-- this is a JavaScript expression rather than a string.       -->
  <BlogPost :likes="42" />

  <!-- Dynamically assign to the value of a variable. -->
  <BlogPost :likes="post.likes" />
  ```

  Kiểu **Number** có chút đặc biệt, nếu ta truyền trực tiếp một số vào prop, mặc dù nó là dữ liệu tĩnh nhưng vue vẫn sẽ hiểu đó là một **string**, vì vậy ta cần sử dụng `v-bind` hay `:` để vue hiểu được đó là số

  - **Boolean**

  ```html
  <!-- Including the prop with no value will imply `true`. -->
  <BlogPost is-published />

  <!-- Even though `false` is static, we need v-bind to tell Vue that -->
  <!-- this is a JavaScript expression rather than a string.          -->
  <BlogPost :is-published="false" />

  <!-- Dynamically assign to the value of a variable. -->
  <BlogPost :is-published="post.isPublished" />
  ```

  - **Arrays**

  ```html
  <!-- Even though the array is static, we need v-bind to tell Vue that -->
  <!-- this is a JavaScript expression rather than a string.            -->
  <BlogPost :comment-ids="[234, 266, 273]" />

  <!-- Dynamically assign to the value of a variable. -->
  <BlogPost :comment-ids="post.commentIds" />
  ```

  - **Object**

  ```html
  <!-- Even though the object is static, we need v-bind to tell Vue that -->
  <!-- this is a JavaScript expression rather than a string.             -->
  <BlogPost
    :author="{
      name: 'Veronica',
      company: 'Veridian Dynamics'
    }"
  />

  <!-- Dynamically assign to the value of a variable. -->
  <BlogPost :author="post.author" />
  ```

- **Binding props bằng đối tượng**

  Trong trường hợp nếu chúng ta muốn truyền các thuộc tính trong một đối tượng như là các prop thì ta có thể sử dụng `v-bind` thay vì `:prop-name`.
  Ta có ví dụ như sau. Đây là component `Child.vue`:

  ```html
  <script setup>
    import { defineProps } from "vue";

    const props = defineProps({
      name: String,
      age: Number,
      address: String,
    });
  </script>
  ```

  Còn đây là khi ta sử dụng trong `Parent.vue`

  ```html
  <script setup>
    import Child from "./components/Child.vue";

    const obj = {
      name: Khang,
      address: "Viet nam",
      age: 21,
    };
  </script>

  <template>
    <Child v-bind="obj" />
  </template>
  ```

  Cách truyền props trên tương tự như sau:

  ```html
  <template>
    <Child :name="obj.name" :age="obj.address" :address="obj.address" />
  </template>
  ```

- **One-way data flow**

  Tất cả các thuộc tính truyền từ cha xuống con đều là dữ liệu một chiều. Tức là khi giá trị ở cha thay đổi thì giá trị prop ở con sẽ nhận được giá trị mới nhất cha. Tuy nhiên chúng ta sẽ không thể tác động, gán lại giá trị props ở con. Có thể hiểu là giá trị props ở con ta chỉ có thể đọc được.

  Nhưng chúng ta cũng có vài cách để tùy chính các prop ở con:

  - Cách 1:

  ```js
  const props = defineProps(["initialCounter"]);

  // counter only uses props.initialCounter as the initial value;
  // it is disconnected from future prop updates.
  const counter = ref(props.initialCounter);
  ```

  Ở cách này thì giá trị prop sẽ làm giá trị khởi tạo cho một reactive data, một biến reactive

  - Cách 2:

  ```js
  const props = defineProps(["size"]);

  // computed property that auto-updates when the prop changes
  const normalizedSize = computed(() => props.size.trim().toLowerCase());
  ```

  Trong cách này thì prop sẽ là biến mà `computed` phụ thuộc vào và trả về một giá trị mới nào đó và gán cho `normalizedSize`
