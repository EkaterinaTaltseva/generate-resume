<template>
  <form class="card card-w30" @submit.prevent="submitResume" >

    <div class="form-control">
      <label for="type">Тип блока</label>

      <select id="type" v-model="resType">
        <option value="title">Заголовок</option>
        <option value="subtitle">Подзаголовок</option>
        <option value="avatar">Аватар</option>
        <option value="text">Текст</option>
      </select>

    </div>
    <div class="form-control">
      <label for="value">Значение</label>
      <textarea id="value" rows="3" v-model.trim="resText"></textarea>

    </div>
    <button class="btn primary" :disabled="buttonAddActive">Добавить</button>
  </form>
</template>

<script>
export default {
  props: ['resumeType', 'resumeText', 'lengthText'],
  emits: ['submitResumeForm'],
  data () {
    return {
      resText: this.resumeText,
      resType: this.resumeType
    }
  },
  methods: {
    submitResume () {
      this.$emit('submitResumeForm', this.resText, this.resType)
      this.resText = ''
      this.resType = 'title'
    }
  },
  computed: {
    buttonAddActive () {
      return this.resText.length < this.lengthText || false
    }
  }
}
</script>

<style scoped>

</style>
