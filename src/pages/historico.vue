<template>
    <div class="container justify-center items-center col-11">
        <q-table
            title="Ranking teste"
            :data="tableData"
            :columns="columns"
            row-key="name"
        />
    </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'historico',

  data () {
    return {
      tableData: [],
      columns: [
        {
          name: 'nome',
          required: true,
          label: 'Nome do jogador',
          align: 'left',
          field: 'usuario',
          sortable: true,
          format (value) {
            return value ? value.nome : ''
          }
        },
        {
          name: 'vitorias',
          required: true,
          label: 'Quantidade de vitórias',
          field: 'qtd_vitorias',
          sortable: true
        }
      ]
    }
  },
  mounted () {
    axios.get(
      '/partidas/ranking/', {
        baseURL: 'http://127.0.0.1:8000'
      })
      .then(response => {
        if (response.data) {
          this.tableData = response.data
        }
      })
  }
}
</script>

<style>

</style>
