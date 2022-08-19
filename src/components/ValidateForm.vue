<!-- 用于编写当前组件的结构代码 -->
<template>
  <form class="validate-form-container">
    <slot name="default"></slot>
    <div class="submit-area" @click.prevent="submitForm">
        <slot name="submit">
            <button type="submit" class="btn btn-primary">提交</button>
        </slot>
    </div>
  </form>
</template>

<!-- 用于编写当前组件的业务代码 -->
<script lang="ts">
import { defineComponent, onUnmounted } from 'vue'
import mitt from 'mitt'
type ValidateFunc = () => boolean;
type Events = {
  'form-item-created': ValidateFunc
}
export const emitter = mitt<Events>()
export default defineComponent({
  emits: ['form-submit'],
  setup (props, context) {
    let funcArr: ValidateFunc[] = []
    const submitForm = () => {
      const result = funcArr.map(func => func()).every(result => result)
      context.emit('form-submit', result)
    }
    const callback = (func:ValidateFunc) => {
      funcArr.push(func)
    }
    emitter.on('form-item-created', callback)
    onUnmounted(() => {
      emitter.off('form-item-created', callback)
      funcArr = []
    })
    return {
      submitForm
    }
  }
})
</script>

<!-- 用于编写当前组件的样式代码 -->
<style  scoped>

</style>
