<template>
  <v-container class="grey lighten-5">
    <v-row no-gutters>
      <v-col>
        <v-card>
          <form @submit.prevent="loadData($event)" class="request-form">
            <v-text-field
                label="Event ID"
                placeholder="1..."
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
                placeholder="5..."
                name="quantity"
                value="5"
                solo
            ></v-text-field>
            <v-btn
                color="primary"
                elevation="2"
                type="submit"
            >Get data</v-btn>
          </form>

        </v-card>
      </v-col>
      <v-col>

      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import * as axios from "axios";

export default {
  name: 'Home',

  data: () => ({
    pages: [],
    selectItems: ["Team", "Single"]
  }),
  methods: {

    queryBuilder(formElements) {
      return `https://api.gofundraise.com.au/v1/pages/search?
      eventcampaignid=${formElements.eventId.value}
      &pagetype=${formElements.type.value === "Team" ? "T" : "S"}
      &sortorder=desc&sortby=4&pagesize=${formElements.quantity.value})`;
    },

    loadData(event) {
      axios.get(this.queryBuilder(event.target.elements)).then((resp) => {
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

<style>

form.request-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}
form.request-form > div.v-input {
  width: 100%;
}

</style>
