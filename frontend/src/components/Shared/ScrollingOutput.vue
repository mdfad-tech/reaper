<script lang="ts" setup>
import { onMounted, onUpdated, PropType, ref } from 'vue'

defineProps({
  lines: { type: Array as PropType<string[]>, required: true },
})

const terminal = ref(null)

function scrollToBottom() {
  if (!terminal.value) {
    return
  }
  const el = terminal.value as HTMLElement
  el.scrollTop = el.scrollHeight
}

onUpdated(scrollToBottom)
onMounted(scrollToBottom)
</script>

<template>
  <div class="h-full p-1 bg-black">
              <pre ref="terminal" class="whitespace-pre-wrap h-full overflow-auto"><code><template
                  v-for="line in lines">{{
                  line
                }}</template></code></pre>
  </div>
</template>
