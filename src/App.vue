<template>
  <div id="app">
    <cardtable :rows="4" :columns="6"></cardtable>
  </div>
</template>

<script>
import CardTable from './components/CardTable.vue'

export default {
  name: 'App',
  components: {
    cardtable: CardTable,
  }
}
</script>


