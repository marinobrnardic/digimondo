<template>
  <v-app>
    <v-main>
      <v-container>
        <v-data-table
          :headers="headers"
          :items="devices"
          :items-per-page="10"
        ></v-data-table>
        <v-btn v-if="prevPage" @click="fetchData(prevPage)" class="mr-4"
          >Previous</v-btn
        >
        <v-btn v-if="nextPage" @click="fetchData(nextPage)">Next</v-btn>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data: () => ({
    devices: [],
    nextPage: "",
    prevPage: "",
    headers: [
      {
        text: "Title",
        align: "start",
        sortable: true,
        value: "device.connector.type",
      },
      { text: "External Id", align: "center", sortable: true, value: "id" },
      {
        text: "Region",
        align: "end",
        sortable: true,
        value: "device.data.region",
      },
    ],
  }),
  methods: {
    storeData(res) {
      this.devices = res.data.data;
      this.nextPage = res.data.pagination.next ? res.data.pagination.next : "";
      this.prevPage = res.data.pagination.previous
        ? res.data.pagination.previous
        : "";
    },
    fetchData(url) {
      axios
        .get(url, {
          headers: {
            "x-api-key": "2C2A4B8C-1E11-4C1F-996B-25DD99ED0CC6",
            "Content-Type": "application/json",
            "Access-Control-Allow-Origin": "*",
          },
        })
        .then((res) => this.storeData(res));
    },
  },
  mounted() {
    axios
      .get("https://xapi.stage.niota.io/xapi/v1/virtual-devices/", {
        headers: {
          "x-api-key": "2C2A4B8C-1E11-4C1F-996B-25DD99ED0CC6",
          "Content-Type": "application/json",
          "Access-Control-Allow-Origin": "*",
        },
      })
      .then((res) => this.storeData(res));
  },
};
</script>
