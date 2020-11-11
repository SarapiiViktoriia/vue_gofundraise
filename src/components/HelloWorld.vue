<template>
  <v-container class="grey lighten-5">
    <v-row no-gutters>
      <v-col>
        <v-card>
          <form @submit.prevent="loadData($event)">
            <v-text-field
                label="Event ID"
                placeholder="Placeholder"
                solo
                name="eventId"
            ></v-text-field>
            <v-select
                :items="selectItems"
                label="Type"
                name="type"
                solo
            ></v-select>
            <v-text-field
                label="Quantity"
                placeholder="Placeholder"
                name="quantity"
                solo
            ></v-text-field>
            <v-btn
                color="primary"
                elevation="2"
                type="submit"
            ></v-btn>
          </form>

        </v-card>
      </v-col>
      <v-col>
        <v-card
            max-width="450"
            class="mx-auto"
        >
          <v-list three-line>
            <template v-for="(item) in pages">
              <v-list-item
                  :key="item.id"
              >
                <v-list-item-avatar>
                  <v-img :src="item.imagePath"></v-img>
                </v-list-item-avatar>

                <v-list-item-content>
                  <v-list-item-title v-html="item.creatorName"></v-list-item-title>
                  <v-list-item-subtitle v-html="item.totalRaised + '/' + item.raiseTarget"></v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </template>
          </v-list>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import * as axios from "axios";

export default {
  name: 'HelloWorld',

  data: () => ({
    pages: [],
    selectItems: ["T", "S"]
  }),
  methods: {
    loadData(event) {
      let formElements = event.target.elements;
      axios.get(`https://api.gofundraise.com.au/v1/pages/search?eventcampaignid=${formElements.eventId.value}&pagetype=&sortorder=desc&sortby=4&pagesize=${formElements.quantity.value})`).then((resp) => {
        resp.data.Pages.forEach(elem => {
          this.pages.push({
            id: elem.Id,
            imagePath: elem.ImagePath,
            creatorName: elem.CreatorName,
            totalRaised: elem.Total,
            raiseTarget: elem.RaiseTarget
          })
        })
      })
    }
  },
  mounted() {

  }
}
</script>
