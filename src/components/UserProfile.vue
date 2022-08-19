<!-- 用于编写当前组件的结构代码 -->
<template>
  <div class="user-profile-component">
    <div class="d-flex align-items-center">
      <img :src="fitUrl" :alt="user.nickName" class="rounded-circle img-thumbnail">
      <div class="detail ml-2">
        <h6 class="d-block mb-0">{{user.nickName}}</h6>
        <span class="text-truncate text-muted d-block">{{user.description}}</span>
      </div>
    </div>
  </div>
</template>
<!-- 用于编写当前组件的业务代码 -->
<script lang="ts">
import { computed, defineComponent, PropType } from 'vue'
import { UserProps } from '../store'
import { addColumnAvatar } from '../helper'
export default defineComponent({
  name: 'UserProfile',
  props: {
    user: {
      type: Object as PropType<UserProps>,
      required: true
    }
  },
  setup (props) {
    const fitUrl = computed(() => {
      addColumnAvatar(props.user, 50, 50)
      const { avatar } = props.user
      return avatar && avatar.fitUrl
    })
    return {
      fitUrl
    }
  }
})
</script>

<!-- 用于编写当前组件的样式代码 -->
<style  scoped>
.user-profile-component img {
  width: 50px;
  height: 50px;
  /* border: 1px solid #ccc;
  border-radius: 50px; */
}
</style>
