<template>
  <v-app>
    <v-main>
      <v-container>
        <v-data-table :headers="headers" :items="devices" :items-per-page="10">
          <template v-slot:item="row">
            <tr>
              <td align="start">{{ row.item.meta.name }}</td>
              <td align="center">{{ row.item.id }}</td>
              <td align="center">{{ row.item.meta.createdAt }}</td>
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
      <v-container>
        <v-btn @click="fetchSortedData('id', '-')" class="mr-4"
          >Sort Id Desc</v-btn
        >
        <v-btn @click="fetchSortedData('id', '%2b')">Sort Id Asc</v-btn>
      </v-container>
      <v-container>
        <v-btn @click="fetchSortedData('name', '-')" class="mr-4"
          >Sort Name Desc</v-btn
        >
        <v-btn @click="fetchSortedData('name', '%2b')">Sort Name Asc</v-btn>
      </v-container>
      <v-container>
        <v-btn @click="fetchSortedData('createdAt', '-')" class="mr-4"
          >Sort created Desc</v-btn
        >
        <v-btn @click="fetchSortedData('createdAt', '%2b')"
          >Sort created Asc</v-btn
        >
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
        text: "Name",
        align: "start",
        sortable: true,
        value: "device.meta.name",
      },
      { text: "External Id", align: "center", sortable: true, value: "id" },
      {
        text: "Created At",
        align: "center",
        sortable: true,
        value: "device.meta.createdAt",
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
    fetchSortedData(sortingParam, direction) {
      axios
        .get(
          "https://xapi.stage.niota.io/xapi/v1/virtual-devices/?sort=" +
            direction +
            sortingParam,
          {
            headers: {
              "x-api-key": "2C2A4B8C-1E11-4C1F-996B-25DD99ED0CC6",
              "Content-Type": "application/json",
              "Access-Control-Allow-Origin": "*",
            },
          }
        )
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
