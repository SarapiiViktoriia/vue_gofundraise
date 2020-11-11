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
        <template v-if="isLoading">
          <div class="preloader-container">
          <v-progress-circular
              indeterminate
              color="primary"
          ></v-progress-circular>
          </div>
        </template>
        <template v-else>
          <ItemList :items-array="pages" :type="currentType"></ItemList>
        </template>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import * as axios from "axios";
import ItemList from "@/components/ItemList";

export default {
  name: 'Home',
  components: {
    ItemList,
  },
  data: () => ({
    pages: [],
    selectItems: ["Team", "Single"],
    isLoading: true,
    currentType: Boolean
  }),
  methods: {
    queryBuilder(formElements) {
      this.currentType = formElements.type.value === "Team";
      return `https://api.gofundraise.com.au/v1/pages/search`+
          `?eventcampaignid=${formElements.eventId.value}`+
          `&pagetype=${formElements.type.value === "Team" ? "T" : "S"}`+
          `&sortorder=desc&sortby=4`+
          `&pagesize=${formElements.quantity.value}`;
    },

    responsElementsHandler(respElement) {
      let page = {
        id: respElement.Id,
        imagePath: respElement.ImagePath,
        creatorName: respElement.CreatorName
      }
      if(this.currentType) {
        page.teamTotal = respElement.Team.TeamTotal;
      } else {
        page.individualTotal = respElement.Total;
      }
      return page;
    },

    loadData(event) {
      this.isLoading = true;
      this.pages = [];
      console.log(this.queryBuilder(event.target.elements));
      axios.get(this.queryBuilder(event.target.elements)).then((resp) => {
        resp.data.Pages.forEach(elem => {
          this.pages.push(this.responsElementsHandler(elem))
        })
        this.isLoading = false;
      }).catch(error => console.log(error))
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
div.preloader-container {
  height: 100%;
  min-height: 310px;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
