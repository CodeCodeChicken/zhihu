
<!-- 用于编写当前组件的结构代码 -->
<template>
<li class="dropdown-option"
:class="{'is-disabled':disabled}">
    <slot></slot>
</li>
</template>

<!-- 用于编写当前组件的业务代码 -->
<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  props: {
    disabled: {
      type: Boolean,
      default: false
    }
  }
})
</script>

<!-- 用于编写当前组件的样式代码 -->
<style>
.dropdown-option.is-disabled * {
  color: #6c757d;
  pointer-events: none;
  background-color: transparent;
}
</style>
