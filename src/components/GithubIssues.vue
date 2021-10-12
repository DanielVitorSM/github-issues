<template>
  <div class="container w-50">
    <div>
      <div class="text-center mt-5">
        <h1>Vue.js + Github</h1>
        <p>Consultando a API do Github e exibindo os issues usando Vue.js.</p>
      </div>
      <div class="row mt-5">
        <div class="col-8 row">
          <input type="text" class="form-control col" name="user" placeholder="user">
          <input type="text" class="form-control col" name="repository" placeholder="repository">
        </div>
        <div class="col-4 row">
          <button class="btn btn-success col-6">Go</button>
          <button class="btn btn-danger col-6">Limpar</button>
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
      <tbody>
        <tr>
          <td class="text-center" colspan="4">Nenhum issues encontrado!</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'GithubIssues',
  data () {
    return {

    }
  }
}
</script>

<style>

</style>
