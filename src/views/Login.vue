<!-- 用于编写当前组件的结构代码 -->
<template>
  <div class="login-page mx-auto p-3 w-330">
    <h5 class="my-4 text-center">登陆到者也</h5>
 <validate-form @form-submit="onFormSubmit">
    <div class="mb-3">
      <label class="form-label">邮箱地址</label>
      <validate-input
      :rules="emailRules"
      v-model="emailVal"
      type="text"
      placeholder="请输入邮箱地址"
      ref="inputRef"
      ></validate-input>
    </div>
    <div class="mb-3">
      <label class="form-label">密码</label>
      <validate-input type="password"
      :rules="passwordRules"
      v-model="passwordVal"
      placeholder="请输入密码"
      ></validate-input>
    </div>
    <template #submit>
      <button type="submit" class="btn btn-primary btn-block btn-large">登录</button>
    </template>
  </validate-form>
  </div>
</template>

<!-- 用于编写当前组件的业务代码 -->
<script lang="ts">
import { defineComponent, ref } from 'vue'
import { useStore } from 'vuex'
import { useRouter } from 'vue-router'
import ValidateForm from '../components/ValidateForm.vue'
import ValidateInput, { RulesProp } from '../components/ValidateInput.vue'
import createMessage from '../components/createMessage'
export default defineComponent({
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Login',
  components: {
    ValidateInput,
    ValidateForm
  },
  setup () {
    const emailVal = ref('')
    const router = useRouter()
    const store = useStore()
    const emailRules: RulesProp = [
      { type: 'required', message: '邮箱地址不能为空' },
      { type: 'email', message: '邮箱地址格式不正确' }
    ]
    const passwordVal = ref('')
    const passwordRules: RulesProp = [
      { type: 'required', message: '密码不能为空' }
    ]
    const onFormSubmit = (result: boolean) => {
      if (result) {
        const payload = {
          email: emailVal.value,
          password: passwordVal.value
        }
        store.dispatch('loginAndFetch', payload).then(() => {
          createMessage('登录成功 2秒后跳转首页', 'success')
          setTimeout(() => {
            router.push('/')
          }, 2000)
        }).catch(e => {
          console.log(e)
        })
      }
    }
    return {
      emailVal,
      emailRules,
      passwordVal,
      passwordRules,
      onFormSubmit
    }
  }
})
</script>

<!-- 用于编写当前组件的样式代码 -->
<style  scoped>

</style>
