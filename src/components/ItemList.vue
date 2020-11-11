<template>
  <v-card
      class="mx-auto"
  >

    <template v-if="!itemsArray.length">
      <div class="not-found-container">
        <span>Not found. Please, changes request parameters</span>
      </div>
    </template>

    <template v-else>
      <v-list three-line>
        <template v-for="(item) in itemsArray">
          <v-list-item
              :key="item.id"
          >
            <v-list-item-avatar>
              <v-img :src="item.imagePath"></v-img>
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title>
              <span v-bind:style="'font-size:'+fontSize+'px'">
                {{ item.creatorName }}
              </span>
              </v-list-item-title>
              <v-list-item-subtitle>
              <span v-bind:style="'font-size:'+((fontSize/100)*85)+'px'">
                {{ item.total }}
              </span>
              </v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </template>
  </v-card>
</template>

<script>
export default {
  name: "ItemList",
  props: {
    itemsArray: Array,
    fontSize: Number || 16
  },
  mounted() {

  }
}
</script>

<style scoped>
div.not-found-container {
  height: 100%;
  min-height: 310px;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
