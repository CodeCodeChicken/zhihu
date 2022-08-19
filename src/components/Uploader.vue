<!-- 用于编写当前组件的结构代码 -->
<template>
  <div class="file-upload">
    <div class="file-upload-container" @click.prevent="triggerUpload" v-bind="$attrs">
    <slot v-if="fileStatus === 'loading'" name="loading">
    <button class="btn btn-primary" disabled>正在上传...</button>
    </slot>
    <slot v-else-if="fileStatus === 'success'" name="uploaded" :uploadedData="uploadedData">
    <button class="btn btn-primary" >上传成功</button>
    </slot>
    <slot v-else name="default">
    <button class="btn btn-primary" >点击上传</button>
    </slot>
    </div>
    <input type="file"
      class="file-input d-none"
      ref="fileInput"
      @change="handleFileChange">
  </div>
</template>

<!-- 用于编写当前组件的业务代码 -->
<script lang="ts">
import axios from 'axios'
import { defineComponent, PropType, ref, watch } from 'vue'
type UploadStatus = 'ready' | 'loading' | 'success' | 'error'
type CheckFunction = (file: File) => boolean
export default defineComponent({
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Uploader',
  props: {
    action: {
      type: String,
      required: true
    },
    beforeUpload: {
      type: Function as PropType<CheckFunction>
    },
    uploaded: {
      type: Object
    }
  },
  inheritAttrs: false,
  emits: ['file-uploaded', 'file-uploaded-error'],
  setup (props, context) {
    const fileInput = ref<null | HTMLInputElement>(null)
    console.log(props.uploaded)
    const fileStatus = ref<UploadStatus>(props.uploaded ? 'success' : 'ready')
    const uploadedData = ref(props.uploaded)
    watch(() => props.uploaded, (newValue) => {
      if (newValue) {
        fileStatus.value = 'success'
        uploadedData.value = newValue
      }
    })
    const triggerUpload = () => {
      if (fileInput.value) {
        fileInput.value.click()
      }
    }
    const handleFileChange = (e:Event) => {
      const currentTarget = e.target as HTMLInputElement
      if (currentTarget.files) {
        const files = Array.from(currentTarget.files)
        if (props.beforeUpload) {
          const result = props.beforeUpload(files[0])
          if (!result) {
            // eslint-disable-next-line no-useless-return
            return
          }
        }
        fileStatus.value = 'loading'
        const formData = new FormData()
        formData.append('file', files[0])
        axios.post(props.action, formData, {
          headers: { 'Content-Type': 'multipart/form-data' }
        }).then(resp => {
          fileStatus.value = 'success'
          uploadedData.value = resp.data
          context.emit('file-uploaded', resp.data)
        }).catch((error) => {
          fileStatus.value = 'error'
          context.emit('file-uploaded-error', { error })
        }).finally(() => {
          if (fileInput.value) {
            fileInput.value.value = ''
          }
        })
      }
    }
    return {
      handleFileChange,
      triggerUpload,
      fileInput,
      fileStatus,
      uploadedData
    }
  }
})
</script>

<!-- 用于编写当前组件的样式代码 -->
<style  scoped>
</style>
