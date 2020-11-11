<template>
  <v-card
      class="mx-auto"
  >
    <v-list three-line>
      <template v-for="(item) in itemsArray">
        <v-list-item
            :key="item.id"
        >
          <v-list-item-avatar>
            <v-img :src="item.imagePath"></v-img>
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-title v-html="item.creatorName"></v-list-item-title>
            <v-list-item-subtitle v-html="type ? ('Team total: '+item.teamTotal) : ('Individual total: ' + item.individualTotal)"></v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </template>
    </v-list>
  </v-card>
</template>

<script>
export default {
name: "ItemList",
  props: {
    itemsArray: Array,
    type: String
  },
  mounted() {

  }
}
</script>

<style scoped>

</style>
