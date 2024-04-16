## vue3 全局变量

main.ts 文件

```ts
app.config.globalProperties.$HOMEURL = "/home";
```

useGlobalProperties.ts 文件

```ts
import { getCurrentInstance, ComponentInternalInstance } from "vue";
export default () =>
  (getCurrentInstance() as ComponentInternalInstance).appContext.config
    .globalProperties;
```

.vue 文件

```vue
<script lang="ts">
import useGlobalProperties from "@/utils/useGlobalProperties";
...
const proxy = useGlobalProperties();

console.log(proxy.$HOMEURL) // "/home"
</script>
```
