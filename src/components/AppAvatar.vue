<template>
    <div class="avatar">
      <img :src="resumeText">
    </div>
</template>

<script>
export default {
  props: ['resumeText'],
  data () { return {} }
}
</script>

<style scoped>
</style>
