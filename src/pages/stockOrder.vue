<template>
  <v-container>
    <v-responsive>
      <v-card>
        <v-card-title class="title">입고 상태</v-card-title>
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
          <template v-slot:item.deliver_order_processing="{ item }">
            <span>{{
              item.deliver_order_processing ? "진행중" : "진행안됨"
            }}</span>
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
  {
    title: "제품 이름",
    key: "product_name",
    align: "center",
  },
  {
    title: "입고 명령 날짜",
    key: "stock_order_order_date",
    align: "center",
    value: (item) =>
      `${date.format(item.stock_order_order_date, "keyboardDateTime")}`,
  },
  {
    title: "입고 진행 상태",
    key: "stock_order_processing",
    align: "center",
  },
  {
    title: "입고 완료 날짜",
    key: "stock_order_complete_date",
    align: "center",
    value: (item) =>
      `${date.format(item.deliver_order_complete_date, "keyboardDateTime")}`,
  },
];

async function fetchProductDetails(productUrl) {
  try {
    const response = await axios.get(productUrl);
    if (response.status === 200) {
      return response.data;
    } else {
      console.log(`Failed to fetch product data from ${productUrl}`);
      return null;
    }
  } catch (error) {
    console.log(error);
    return null;
  }
}

async function getProducts() {
  try {
    const stockOrderResponse = await axios.get(
      "http://192.168.0.100:8000/Stock_order/"
    );
    if (stockOrderResponse.status === 200) {
      const stockOrders = stockOrderResponse.data;

      const productPromises = stockOrders.map(async (order) => {
        const productData = await fetchProductDetails(order.product_id);
        return {
          ...order,
          product_name: productData ? productData.product_name : "Unknown",
        };
      });

      product_list.value = await Promise.all(productPromises);
      console.log("Product List:", product_list.value); // Debugging: Final product list
    } else {
      console.log("Failed to fetch stock order data");
    }
  } catch (error) {
    console.log(error);
  }
}

getProducts();
</script>
