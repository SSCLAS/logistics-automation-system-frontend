<template>
  <v-container>
    <v-responsive>
      <v-card class="card">
        <v-card-title class="title">창고</v-card-title>
        <v-text-field
          class="field"
          v-model="search"
          label="Search"
          prepend-inner-icon="mdi-magnify"
          variant="outlined"
          hide-details
          single-line
        ></v-text-field>
        <v-data-table
          class="table"
          :items="product_list"
          :headers="product_header"
          :search="search"
          outlined
        >
          <template v-slot:header="{ props }">
            <tr>
              <th
                v-for="header in props.headers"
                :key="header.text"
                :class="headerClasses"
              >
                {{ header.text }}
              </th>
            </tr>
          </template>
          <template v-slot:item.actions="{ item }">
            <template v-if="item.product_name">
              <v-btn
                color="#3d5afe"
                :disabled="item.deliver_ordered"
                @click="update(item.product_id, item.deliver_ordered)"
              >
                <span>출고</span>
              </v-btn>
            </template>
          </template>

          <template v-slot:item.deliver_ordered="{ item }">
            <template v-if="item.deliver_ordered">
              <span>{{ item.deliver_ordered ? "출고지시" : "출고전" }}</span>
            </template>
          </template>
        </v-data-table>
      </v-card>
    </v-responsive>
  </v-container>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import { useDate } from "vuetify";
import "../components/table.css";

const date = useDate();
const search = ref("");
const product_list = ref([]);
const product_header = [
  { title: "창고", key: "ware_house_name", align: "center" },
  { title: "제품이름", key: "product_name", align: "center" },
  { title: "제품가격", key: "product_price", align: "center" },
  { title: "제품 출고 상태", key: "deliver_ordered", align: "center" },
  { title: "출고", key: "actions", align: "center", sortable: false },
];

async function getProducts() {
  try {
    const response = await axios.get("http://192.168.0.100:8000/Ware_house/");
    if (response.status === 200) {
      product_list.value = response.data;
      console.log("성공");
    }
  } catch (error) {
    console.error(error);
  }
}

async function update(product_id, status) {
  try {
    const response = await axios.post(
      "http://192.168.0.100:8000/Deliver_order/",
      {
        product_id: product_id,
        robot_id: "http://192.168.0.100:8000/Robot/1/",
      }
    );

    if (response.status === 201) {
      console.log("OK");
      getProducts();
    }
  } catch (error) {
    console.error(error);
  }
}

getProducts();
</script>
