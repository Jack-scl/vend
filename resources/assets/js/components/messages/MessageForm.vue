<template>
  <div class="mb-md">
    <form @submit.prevent="save()" class="message-form">
      <error-message ref="errorMessage"/>
      <editor v-model="message.message" :placeholder="$t('messages.form.message')"/>
      <div class="media-grid">
        <file-attachment v-for="file in message.files" v-bind:key="file.id" :file="file"/>
      </div>
      <uploader type="message" :compact="true" :uploaded="attachFile" :text="$t('uploader.attach')"/>
      <button class="btn btn-primary pull-right">{{ $t('common.save') }}</button>
    </form>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex'
  import Uploader from '../common/Uploader.vue'
  import Editor from '../common/Editor.vue'
  import ErrorMessage from '../common/ErrorMessage.vue'
  import FileAttachment from '../files/FileAttachment.vue'

  export default {
    components: {
      Uploader,
      ErrorMessage,
      FileAttachment,
      Editor
    },
    data() {
      return {
        message: {
          message: '',
          files: []
        }
      }
    },
    methods: {
      save () {
        this.$store.dispatch('saveMessage', {
          topicId: this.$route.params.id,
          message: this.message.message,
          files: this.message.files,
          urlParams: {topicId: this.$route.params.id}
        })
          .then(() => {
            this.message.message = ''
            this.message.files = []
          })
          .catch(err => {
            this.$refs.errorMessage.set(err)
          })
      },
      attachFile (file) {
        this.message.files.push(file)
      }
    }
  }
</script>