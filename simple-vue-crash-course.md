Just making this one in case you guys are not familiar with vue, I'll explain some main differences between vue and react.

1. Vue does not run it's setup function on every render, but only when the component is mounted
2. `v-model="something"` prop expands to `:modelValue="something" @update:modelValue="($e)=> something = $e"` it's just syntactic sugar
3. `reactive` and `ref` are how vue declares reactive variables. `reactive` creates a proxy while `ref` works on an observer pattern as fat as I remember
4. `ref` has to be used with `.value` inside the `script` tag and has to be used without it inside the `template` tag. (that's because the complier wraps each ref with a function called `unref` at compile time)
5. `defineModel` and `defineProps` are syntactic sugar as well. The first one just got introduced very recently.
6. Unlike react with `memo` variables `computed` variables don't need to be tracked in an array at the end as they are automatically tracked. Any reactive variable will call the function essentially.

We can go over more in the meeting.
