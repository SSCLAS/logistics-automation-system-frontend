<template>
  <v-container>
    <v-responsive>
      <v-card title="로봇">
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
          <template v-slot:item.robot_status="{ item }">
            <span>{{ item.robot_status ? "활성화" : "활성화 안됨" }}</span>
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
    title: "로봇 이름",
    key: "robot_name",
    align: "center",
  },
  {
    title: "로봇 활성화",
    key: "robot_status",
    align: "center",
  },
  {
    key: "robot_image",
    align: "center",
  },
];

// 각 product_id에 대해 제품 정보를 가져오는 함수
async function fetchProductDetails(productUrl) {
  try {
    const response = await axios.get(productUrl);
    if (response.status === 200) {
      console.log("Product Data:", response.data); // Debugging: Product data
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

// Products 조회 함수
async function getProducts() {
  try {
    const deliverOrderResponse = await axios.get(
      "http://192.168.0.100:8000/Robot/"
    );
    if (deliverOrderResponse.status === 200) {
      const deliverOrders = deliverOrderResponse.data;
      console.log("Deliver Orders:", deliverOrders); // Debugging: Deliver orders

      // 각 deliver order에 대해 product 정보를 병합
      const productPromises = deliverOrders.map(async (order) => {
        const productData = await fetchProductDetails(order.product_id);
        return {
          ...order,
          product_name: productData ? productData.name : "Unknown",
        };
      });

      // 모든 product 정보를 병합하여 최종 데이터 설정
      product_list.value = await Promise.all(productPromises);
      console.log("Product List:", product_list.value); // Debugging: Final product list
      console.log("성공");
    } else {
      console.log("Failed to fetch deliver order data");
    }
  } catch (error) {
    console.log(error);
  }
}

getProducts();
</script>
