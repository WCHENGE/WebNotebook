c# deVue3.2 组合式基本使用

## 组件的 props 标注类型

通过泛型参数来定义 props 的类型：

```vue
<script setup lang="ts">
const props = defineProps<{
  foo: string
  bar?: number
}>()
</script>
```

Props 解构默认值：

```vue
<script setup lang="ts">
// 对 defineProps() 的响应性解构
// 默认值会被编译为等价的运行时选项
const { foo, bar = 100 } = defineProps<{
  foo: string
  bar?: number
}>()
</script>
```
使用 withDefaults 辅助函数，类型声明时的默认 props 值: 

```vue
export interface Props {
  msg?: string
  labels?: string[]
}

const props = withDefaults(defineProps<Props>(), {
  msg: 'hello',
  labels: () => ['one', 'two']
})
```



## 组件的 emits 标注类型

`emit` 函数的类型标注，可以通过运行时声明或是类型声明进行：（提倡基于类型声明）

```vue
<script setup lang="ts">
// 运行时
const emit = defineEmits(['change', 'update'])

// 基于类型
const emit = defineEmits<{
  (e: 'change', id: number): void
  (e: 'update', value: string): void
}>()
</script>
```

## `ref()` 标注类型

ref 会根据初始化时的值推导其类型：

```vue
import { ref } from 'vue'

// 推导出的类型：Ref<number>
const year = ref(2020)

// => TS Error: Type 'string' is not assignable to type 'number'.
year.value = '2020'
```

或者，在调用 `ref()` 时传入一个泛型参数，来覆盖默认的推导行为：

```vue
// 得到的类型：Ref<string | number>
const year = ref<string | number>('2020')

year.value = 2020 // 成功！
```

## `reactive()` 标注类型

显式地标注一个 `reactive` 变量的类型，可以使用接口：

```vue
import { reactive } from 'vue'

interface Book {
  title: string
  year?: number
}

const book: Book = reactive({ title: 'Vue 3 指引' })
```

> 不推荐使用 `reactive()` 的泛型参数，因为处理了深层次 ref 解包的返回值与泛型参数的类型不同。

## `computed()` 标注类型

```vue
const double = computed<number>(() => {
  // 若返回值不是 number 类型则会报错
})
```







### app.config.globalProperties

```vue
// 定义
const app = Vue.createApp({})
app.config.globalProperties.$http =()=>{}

// 使用
import { getCurrentInstance, onMounted } from "vue";
const{ ctx }=getCurrentInstance();//获取上下文实例，ctx=vue2的this
```









