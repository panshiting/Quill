Quill文本框的引用
====

### [quill.js富文本编辑器的官网地址](https://quilljs.com/)
### [quill.js富文本编辑器配置参数地址](https://quilljs.com/docs/configuration/)
### [quill.js富文本编辑器API方法地址](https://quilljs.com/docs/api/)

# react中使用
quill的上传视频是通过填写视频链接即可，当前例子中包含有自定义的上传图片方法
```
import Quill from 'quill'
import 'quill/dist/quill.snow.css'

class aa extends Component {
  this.state = {
    value: ''
  }
  this.editor = null
  this.quillObj = null

  // 创建富文本(需要在componentDidMount中初始化)
  addQuill () {
    // 获取使用富文本的元素
    const textbox = this.refs.textarea 
    // 富文本的基本参数
    const toolbarOptions = [
        ['bold', 'italic', 'underline', 'strike'],        // toggled buttons
        ['blockquote', 'code-block'],
        [{ 'header': 1 }, { 'header': 2 }],               // custom button values
        [{ 'list': 'ordered'}, { 'list': 'bullet' }],
        [{ 'script': 'sub'}, { 'script': 'super' }],      // superscript/subscript
        [{ 'indent': '-1'}, { 'indent': '+1' }],          // outdent/indent
        [{ 'direction': 'rtl' }],
        [{ 'size': ['small', false, 'large', 'huge'] }],  // custom dropdown
        [{ 'header': [1, 2, 3, 4, 5, 6, false] }],
        [{ 'color': [] }, { 'background': [] }],          // dropdown with defaults from theme
        [{ 'font': [] }],
        [{ 'align': [] }],
        ['image', 'video'],
        ['clean']                                         // remove formatting button
      ]
      // 配置项
      const options = {
        debug: 'warn',
        modules: {
          toolbar: toolbarOptions
        },
        placeholder: '请输入文本...',
        readOnly: false,
        theme: 'snow'
      }
      let editor =this.editor= new Quill(textbox,options)
      let {value}=this.state
      // 初始化富文本的值
      if (value) editor.clipboard.dangerouslyPasteHTML(value)
      // 绑定富文本值的变化
      editor.on('text-change', this.handleChange.bind(this))
      // 图片上传时候需要该对象
      this.quillObj = editor
      let toolbar = editor.getModule('toolbar')
      // 重写富文本图片上传方法（通过点击富文本的上传图片按钮，其实际上是调用ID为quillFile的元素上传图片，再将该值赋给富文本框）
      toolbar.addHandler('image', () => {
        document.getElementById('quillFile').click()
      })
  }

  // 绑定富文本的输入框
  handleChange () {
    let {value} = this.state
    value = this.editor.root.innerHTML
    this.setState({value},()=> {console.log(this.state.value)})
  }
  
  // 上传图片
  getUrl () {
    // 获取光标的位置
    let addRange = this.quillObj.getSelection()
    let fileobj = document.getElementById('quillFile').files[0]
    let formFile = new FormData()
    formFile.append('file', fileobj)
    // 上传图片
    this.$ajax.uploadFile(formFile).then(res => {
      if (res.code === 200) {
      // insertEmbed是将图片插入到富文本框中，插入的位置为上传图片前光标所在位置
        this.quillObj.insertEmbed(addRange !== null ? addRange.index : 0, 'image', res.data)
      } else {
        Message.error('图片上传失败！')
      }
    }).catch(() => {
      Message.error('网络错误')
    })
  }

  render () {
    return (
      <InputFile id="quillFile" type="file" onChange={this.getUrl.bind(this)}></InputFile>
    )
  }
}
export default withRouter(skuOperate)
```


