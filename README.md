**SSR probably breaks some implicit contract?**

- Runner accessed in onInvalidate callback: https://github.com/vuejs/vue-next/blob/ec8fd10cec61c33c7c8056413a1c609ac90e1215/packages/runtime-core/src/apiWatch.ts#L227
- Runner defined lower down: https://github.com/vuejs/vue-next/blob/ec8fd10cec61c33c7c8056413a1c609ac90e1215/packages/runtime-core/src/apiWatch.ts#L296

```js
/* <script setup> */
import { watchEffect } from 'vue'
watchEffect(onInvalidate => {
  onInvalidate(() => {})
})
</script>
```

```sh
% yarn dev
yarn run v1.22.5
$ node server
http://localhost:3000
[Vue warn]: Unhandled error during execution of watcher callback
  at <App>
[Vue warn]: Unhandled error during execution of setup function
  at <App>
ReferenceError: Cannot access 'runner' before initialization
    at onInvalidate (/Users/tj/src/github.com/tjk/watcheffect-runner-bug/node_modules/@vue/runtime-core/dist/runtime-core.cjs.js:2035:9)
    at /Users/tj/src/github.com/tjk/watcheffect-runner-bug/src/App.vue:10:7
    at callWithErrorHandling (/Users/tj/src/github.com/tjk/watcheffect-runner-bug/node_modules/@vue/runtime-core/dist/runtime-core.cjs.js:156:22)
    at getter (/Users/tj/src/github.com/tjk/watcheffect-runner-bug/node_modules/@vue/runtime-core/dist/runtime-core.cjs.js:2021:24)
    at doWatch (/Users/tj/src/github.com/tjk/watcheffect-runner-bug/node_modules/@vue/runtime-core/dist/runtime-core.cjs.js:2043:13)
    at Proxy.watchEffect (/Users/tj/src/github.com/tjk/watcheffect-runner-bug/node_modules/@vue/runtime-core/dist/runtime-core.cjs.js:1949:12)
    at setup (/Users/tj/src/github.com/tjk/watcheffect-runner-bug/src/App.vue:9:15)
    at _sfc_main.setup (/src/App.vue:29:23)
    at callWithErrorHandling (/Users/tj/src/github.com/tjk/watcheffect-runner-bug/node_modules/@vue/runtime-core/dist/runtime-core.cjs.js:156:22)
    at setupStatefulComponent (/Users/tj/src/github.com/tjk/watcheffect-runner-bug/node_modules/@vue/runtime-core/dist/runtime-core.cjs.js:6402:29)
```
