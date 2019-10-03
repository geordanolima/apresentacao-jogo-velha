<template>
    <div class="container justify-center items-center col-11">
        <q-table
            title="Ranking"
            :data="tableData"
            :columns="columns"
            row-key="name"
        />
    </div>
</template>

<script>
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
          field: 'nome',
          sortable: true
        },
        {
          name: 'vitorias',
          required: true,
          label: 'Quantidade de vitórias',
          field: 'nome',
          sortable: true
        }
      ]
    }
  }
}
</script>

<style>

</style>
