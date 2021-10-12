<template>
  <div class="container">
    <div>
      <div class="text-center mt-5">
        <h1>Vue.js + Github</h1>
        <p>Consultando a API do Github e exibindo os issues usando Vue.js.</p>
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
        v-if="!!issues.length && !loaders.getIssues"
      >
        <tr
          v-for="issue in issues"
          :key="issue.id"
        >
          <td>{{ issue.number }}</td>
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
  data () {
    return {
      username: '',
      repository: '',
      issues: [],
      loaders: {
        getIssues: false
      }
    }
  },
  methods: {
    reset () {
      this.username = ''
      this.repository = ''
    },
    getIssues () {
      if (this.username && this.repository) {
        this.loaders.getIssues = true
        const url = `https://api.github.com/repos/${this.username}/${this.repository}/issues`
        axios
          .get(url)
          .then(res => {
            this.issues = res.data
          })
          .catch(() => {
            alert('Erro ao processar requisição.')
          })
          .finally(() => {
            this.loaders.getIssues = false
          })
      }
    }
  }
}
</script>
