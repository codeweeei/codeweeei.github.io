---
title: vue2-editor富文本编辑器
date: 2021/3/1
tags: [富文本编辑器]
categories: Vue
---

## vue2-editor 富文本编辑器的使用

### 安装

`npm install vue2-editor`

### 使用

- 可以直接把需要的文本内容复制在富文本编辑器里，保存至服务器之后，会将其解析为 html 文件，一些样式也能保存，但注意图片会解析为 base64 的格式（体积过大），会导致页面加载很慢

```html
<template>
  <div id="app">
    <vue-editor v-model="content"></vue-editor>
  </div>
</template>

<script>
  import { VueEditor } from "vue2-editor"

  export default {
    components: {
      VueEditor,
    },

    data() {
      return {
        content: "<h1>Some initial content</h1>",
      }
    },
  }
</script>
```

### 图片上传

```html
<!-- 富文本编辑器 -->
<template>
  <vue-editor
    useCustomImageHandler
    @image-added="handleImageAdded"
    v-model="model.body"
  >
  </vue-editor>
</template>
<script>
  import { VueEditor } from "vue2-editor"

  export default {
    components: {
      VueEditor,
    },
    methods: {
      //富文本编辑器自定义上传文件，可鼠标指定上传位置
      async handleImageAdded(file, Editor, cursorLocation, resetUploader) {
        const formData = new FormData()
        formData.append("file", file)
        //将formData上传至图片接口
        this.$http.post("upload", formData)
        //从接口返回上传的图片的url
        const url = await res.data.url
        //在鼠标指定位置处添加image标签（url路径）
        Editor.insertEmbed(cursorLocation, "image", url)
        resetUploader()
      },
    },
  }
</script>
```

> 此时上传的图片文件大小就会大大减小，增加页面加载速度
