<template>
  <div class="container column" v-cloak >

    <app-form     :resumeType = "resumeType"
                  :resumeText = "resumeText"
                  :lengthText = "lengthText"
                  @submitResumeForm = "submitResumeForm"
    ></app-form>

    <div class="card card-w70">
      <component  v-for = "item in fullResume"
                  :resumeText = "item.resumeText"
                  :key = "item.id"
                  :is = "'app-' + item.resumeType"
      >
      </component>
      <h3 v-if="!fullResume.length" >Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>

  <div class="container">
    <p v-if="loading === ''" >
      <button class="btn primary" @click="loadComments" >Загрузить комментарии</button>
    </p>

    <app-loading v-else-if="loading === 'start'"></app-loading>

    <div class="card" v-else-if="loading === 'end'" >
      <h2>Комментарии</h2>
      <app-comment :comments="comments" ></app-comment>
    </div>
  </div>
</template>
<script>
import AppAvatar from '@/components/AppAvatar'
import AppTitle from '@/components/AppTitle'
import AppSubtitle from '@/components/AppSubtitle'
import AppText from '@/components/AppText'
import AppForm from '@/components/AppForm'
import AppComment from '@/components/AppComment'
import AppLoading from '@/components/AppLoading'
export default {
  data () {
    return {
      resumeType: 'title',
      resumeText: '',
      lengthText: 3,
      fullResume: [],
      comments: [],
      loading: ''
    }
  },
  methods: {
    async submitResumeForm (Text, Type) {
      const response = await fetch('https://vue-auto-resume-default-rtdb.europe-west1.firebasedatabase.app/item.json', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          resumeText: Text,
          resumeType: Type
        })
      })
      const firebaseData = await response.json()
      this.fullResume.push({
        id: firebaseData.name,
        resumeType: Type,
        resumeText: Text
      })
    },
    loadComments () {
      this.loading = 'start'
      // GET запрос комментариев
      this.comments = []
      fetch('https://jsonplaceholder.typicode.com/comments?_limit=42')
        .then(response => response.json())
        .then(data => {
          this.comments = Object.keys(data).map(key => {
            return data[key]
          })
        })
      setTimeout(() => {
        this.loading = 'end'
        return true
      }, 500)
    }
  },
  mounted () {
    // GET запрос резюме
    fetch('https://vue-auto-resume-default-rtdb.europe-west1.firebasedatabase.app/item.json')
      .then(response => response.json())
      .then(data => {
        this.fullResume = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
      })
  },
  computed: {},
  components: {
    AppAvatar,
    AppTitle,
    AppSubtitle,
    AppText,
    AppForm,
    AppComment,
    AppLoading
  }
}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }

  [v-cloak] {
    display: none;
  }
</style>
