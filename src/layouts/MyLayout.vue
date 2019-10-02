<template>
  <q-layout view="lHh Lpr lFf" style="background:#303030">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          @click="leftDrawerOpen = !leftDrawerOpen"
          icon="menu"
          aria-label="Menu"
        />

        <q-toolbar-title>
          Velha
        </q-toolbar-title>

        <div>v1.0.0</div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      content-class="bg-grey-9"
      style="color:white"
    >
      <q-list>
        <q-item class="justify-center">
            <img src="../assets/jv.png" width="200px">
        </q-item>
        <q-item-label header>Jogo da velha</q-item-label>
        <q-item clickable to="/jv">
          <q-item-section avatar>
            <q-icon name="sports_esports" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Novo jogo</q-item-label>
          </q-item-section>
        </q-item>
        <q-item clickable to="/ranking">
          <q-item-section avatar>
            <q-icon name="poll" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Ranking</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: 'MyLayout',

  data () {
    return {
      leftDrawerOpen: false
    }
  }
}
</script>
<style lang="scss">
    .q-drawer--left.q-drawer--bordered{
        border-right: 1px solid #303030;
    }
    .q-drawer {
        width: 250px;
    }
    .q-item.q-router-link--active{
        color: #fff !important;
    }
</style>
