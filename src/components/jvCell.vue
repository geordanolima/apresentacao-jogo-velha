<template>
  <q-btn
    class="cell"
    :class="{ganhou:value.ganhou}"
    :disabled="value.exibir"
    @click="jogar"
    :label="value.exibir"
  />

</template>

<script>
export default {
  name: 'jvCell',
  props: ['value'],
  methods: {
    jogar () {
      this.$emit('jogar', this.value)
    }
  }
}
</script>

<style lang="scss">
.cell {
  cursor: pointer;
  float: left;
  border: solid 1px rgb(66, 66, 66);
  width: 140px;
  height: 140px;
  text-align: center;
  line-height: 100px;
  font-size: 60px;
  color: #fee;
  background: rgba($color: #424242, $alpha: 0.4);
}
.cell:hover {
  background-color: rgb(131, 131, 131);
}
.ganhou {
  background-color: #aaffaa;
}
.ganhou:hover {
  background-color: #aaffaa;
}
</style>
