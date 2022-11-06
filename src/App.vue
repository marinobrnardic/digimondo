<template>
  <v-app>
    <v-main>
      <v-container>
        <v-data-table :headers="headers" :items="devices" :items-per-page="10">
          <template v-slot:item="row">
            <tr>
              <td align="start">{{ row.item.device.connector.type }}</td>
              <td align="center">{{ row.item.id }}</td>
              <td align="center">{{ row.item.device.data.region }}</td>
              <td align="end">
                <v-btn @click="getId(row.item.id)">Id</v-btn>
              </td>
            </tr>
          </template>
        </v-data-table>
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
        align: "center",
        sortable: true,
        value: "device.data.region",
      },
      { text: "CTA", align: "end", sortable: false, value: "cta" },
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
    getId(id) {
      window.alert(id);
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
