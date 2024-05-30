<template>
  <v-container>
    <v-responsive>
      <v-card title="출고 상태">
        <template v-slot:text>
          <v-text-field
            v-model="search"
            label="Search"
            prepend-inner-icon="mdi-magnify"
            variant="outlined"
            hide-details
            single-line
          ></v-text-field>
        </template>

        <v-data-table
          :items="product_list"
          :headers="product_header"
          :search="search"
        >
          <template v-slot:item.stock_order_processing="{ item }">
            <span>{{
              item.stock_order_processing ? "진행중" : "진행안됨"
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
    title: "출고 명령 날짜",
    key: "deliver_order_order_date",
    align: "center",
    value: (item) =>
      `${date.format(item.deliver_order_order_date, "keyboardDateTime")}`,
  },
  {
    title: "출고 진행 상태",
    key: "deliver_order_processing",
    align: "center",
  },
  {
    title: "출고 완료 날짜",
    key: "deliver_order_complete_date",
    align: "center",
    value: (item) =>
      item.deliver_order_complete_date
        ? `${date.format(item.deliver_order_complete_date, "keyboardDateTime")}`
        : "",
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
      "http://192.168.0.100:8000/Deliver_order/"
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
      console.log("Product List:", product_list.value);
    } else {
      console.log("Failed to fetch stock order data");
    }
  } catch (error) {
    console.log(error);
  }
}

getProducts();
</script>

<style scoped>
.card {
  background: #ffffff;
}
.title {
  font-size: 36px;
  font-weight: bold;
  color: black;
}
.field {
  margin-bottom: 20px;
  margin-left: 20px;
  margin-right: 20px;
  color: black;
}
.table {
  font-size: 18px;
  background: #ffffff;
  color: black;
  font-weight: bolder;
}
.headerClasses {
  font-size: 24px; /* 헤더 글씨 크기 설정 */
  font-weight: bold; /* 헤더 글씨 두껍게 설정 */
}
</style>
