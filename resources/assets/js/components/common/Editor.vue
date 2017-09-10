<template>
  <div class="form-group">
    <div id="editor" class="form-control">
      <p v-html="value"></p>
    </div>
  </div>
</template>

<script>
  import Quill from 'quill'

  export default {
    props: {
      value: {},
      content: {
        type: String,
        default: () => ''
      },
      placeholder: {
        type: String,
        default: () => ''
      }
    },
    data () {
      return {
        editor: null
      }
    },
    mounted () {
      const editorEl = this.$el.querySelectorAll('#editor')[0]
      this.editor = new Quill(editorEl, {
        modules: {
          toolbar: [
            ['bold', 'italic', 'link', 'code-block']
          ]
        },
        placeholder: this.placeholder,
        theme: 'snow'
      })

      this.editor.on('text-change', () => {
        let content = this.editor.root.innerHTML.replace('<p><br></p>', '')
        this.$emit('input', content)
      })
    },
    methods: {
      setText(n, o) {
        if (n === '' && n !== o) {
          this.editor.setText('')
        }
      }
    },
    watch: {
      'value': 'setText'
    }
  }
</script>