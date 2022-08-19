<!-- 用于编写当前组件的结构代码 -->
<template>
    <teleport to="#back">
        <div
        class="d-flex justify-content-center align-items-center h-100 w-100 loading-container"
        :style="{backgroundColor: background || ''}"
        >
            <div class="loading-content">
                <div class="spinner-border text-primary" role="status">
                    <span class="sr-only">
                        {{ text || 'loading' }}
                    </span>
                </div>
                <p v-if="text" class="text-primary small">{{text}}</p>
            </div>
        </div>
    </teleport>
</template>

<!-- 用于编写当前组件的业务代码 -->
<script lang="ts">
import { defineComponent, onUnmounted } from 'vue'
export default defineComponent({
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Loader',
  props: {
    text: {
      type: String
    },
    background: {
      type: String
    }
  },
  setup () {
    const node = document.createElement('div')
    node.id = 'back'
    document.body.appendChild(node)
    onUnmounted(() => {
      document.body.removeChild(node)
    })
  }
})
</script>

<!-- 用于编写当前组件的样式代码 -->
<style  scoped>
.loading-container {
  background: rgba(255, 255, 255, .5);
  z-index: 100;
  position: fixed;
  top: 0;
  left: 0;
}
.loading-container {
  text-align: center;
}
</style>
