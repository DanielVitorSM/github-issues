<template>
  <div class="container">
    <div v-if="!loaders.getIssue">
      <h2>{{ issue.title }}</h2>
      <p>{{ issue.body }}</p>
      <a
        href="javascript:history.go(-1)"
        class="btn btn-primary"
      > Voltar </a>
    </div>
    <div v-if="loaders.getIssue">
      <img
        src="/static/loading.svg"
        width="30"
      >
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'GithubIssue',
  created () {
    this.getIssue()
  },
  data () {
    return {
      issue: {},
      loaders: {
        getIssue: false
      }
    }
  },
  methods: {
    getIssue () {
      this.loaders = true
      const url = `https://api.github.com/repos/${this.$route.params.name}/${this.$route.params.repo}/issues/${this.$route.params.issue}`
      axios
        .get(url)
        .then(res => {
          this.issue = res.data
        })
        .catch(() => {
          alert('Erro ao processar requisição.')
        })
        .finally(() => {
          this.loaders.getIssue = false
        })
    }
  }
}
</script>
