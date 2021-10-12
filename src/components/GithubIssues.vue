<template>
  <div class="container">
    <div>
      <div class="text-center mt-5">
        <h1>Vue.js + Github</h1>
        <p>Consultando a API do Github e exibindo os issues usando Vue.js.</p>
      </div>

      <div v-if="response.status === 'error'" class="alert alert-danger text-center">
        {{ response.message }}
      </div>

      <div class="row mt-5">
        <div class="col-8 row">
          <input type="text" class="form-control col" v-model="username" placeholder="user">
          <input type="text" class="form-control col" v-model="repository" placeholder="repository">
        </div>
        <div class="col-4 row">
          <button
            @click.prevent.stop="getIssues()"
            class="btn btn-success col-6"
          >Go</button>
          <button
            @click.prevent.stop="reset()"
            class="btn btn-danger col-6"
          >Limpar</button>
        </div>
      </div>
    </div>

    <br><br><br>

    <table class="table table-sm table-striped">
      <thead class="table-light">
        <tr>
          <td width="100">Número</td>
          <td>Título</td>
        </tr>
      </thead>
      <tbody
        v-if="!!!issues.length"
      >
        <tr v-if="!loaders.getIssues">
          <td class="text-center" colspan="4">Nenhum issues encontrado!</td>
        </tr>

        <tr
          v-if="loaders.getIssues"
        >
          <td colspan="2" class="text-center">
            <img src="/static/loading.svg" width="50">
          </td>
        </tr>
      </tbody>
      <tbody
        v-if="showIssues"
      >
        <tr
          v-for="issue in issues"
          :key="issue.id"
        >
          <td class="row">
            <router-link
              :to="{ name: 'GithubIssue', params: { name: username, repo: repository, issue: issue.number }}"
            >
              {{ issue.number }}
            </router-link>
          </td>
          <td>{{ issue.title }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'GithubIssues',
  created () {
    this.getLocalData()
  },
  data () {
    return {
      username: '',
      repository: '',
      issues: [],
      loaders: {
        getIssues: false
      },
      response: {
        status: '',
        message: ''
      }
    }
  },
  methods: {
    reset () {
      this.username = ''
      this.repository = ''
      localStorage.removeItem('github:issues')
    },
    getIssues () {
      if (this.username && this.repository) {
        this.loaders.getIssues = true
        const url = `https://api.github.com/repos/${this.username}/${this.repository}/issues`
        axios
          .get(url)
          .then(res => {
            this.issues = res.data
            localStorage.setItem('github:issues', JSON.stringify({ username: this.username, repository: this.repository }))
          })
          .catch((error) => {
            switch (error.response.status) {
              case 404:
                this.response = { status: 'error', message: 'Esse repositório não existe' }
                this.issues = []
                break
            }
          })
          .finally(() => {
            this.loaders.getIssues = false
          })
      }
    },
    getLocalData () {
      const localData = JSON.parse(localStorage.getItem('github:issues'))
      if (localData && localData.username && localData.repository) {
        this.username = localData.username
        this.repository = localData.repository
        this.getIssues()
      }
    }
  },
  computed: {
    showIssues () {
      return !!this.issues.length && !this.loaders.getIssues
    }
  }
}
</script>
