Quill文本框的引用
====

### [quill.js富文本编辑器的官网地址](https://quilljs.com/)
### [quill.js富文本编辑器配置参数地址](https://quilljs.com/docs/configuration/)
### [quill.js富文本编辑器API方法地址](https://quilljs.com/docs/api/)

# vue中使用
vue中有封装好的quill组件  
下面使用vue封装的quill组件实现，简单的输入文本的例子
```
<template>
  <div>
    <quill-editor ref="myTextEditor" v-model.trim="replyCN"></quill-editor>
  </div>
</template>

import { quillEditor } from 'vue-quill-editor'
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'

export default {
  components: {
    quillEditor
  },
  data () {
    return {
      replyCN: ''
    }
  }
}
```