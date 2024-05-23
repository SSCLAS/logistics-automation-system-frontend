<template>
  <v-data-table :items="pageOneData" :headers="pageOneHeaders">
    <template v-slot:item.deliver_order_date="{ item }">
      <v-checkbox v-model="item.deliver_order_date">
      </v-checkbox>
    </template>
  </v-data-table>
</template>

<script>
import axios from "axios";
import { ref, onMounted } from "vue";

const API_NAME = {
  Product: "http://192.168.0.100:8000/Product/",
  Ware_house: "http://192.168.0.100:8000/Ware_house/"
};

export default {
  setup() {
    const pageOneData = ref([]);
    const pageOneHeaders = [
      { title: "창고 이름", align: "center", key: "ware_house_name" },
      { title: "사용 여부", align: "center", key: "ware_house_status" },
      { title: "상품 이름", align: "center", key: "product_id" },
      { title: "출고 요청", align: "center", key: "deliver_order_date" }
    ];

    const fetchData = async () => {
      try {
        const [wareHouseResponse, productResponse] = await Promise.all([
          axios.get(API_NAME['Ware_house']),
          axios.get(API_NAME['Product'])
        ]);

        const wareHouses = wareHouseResponse.data;
        const products = productResponse.data;

        pageOneData.value = wareHouses.map((wh) => ({
          ware_house_name: wh.ware_house_name || "없음",
          ware_house_status: wh.ware_house_status ? "O" : "X",
          product_id: getProductById(wh.product_id, products),
          deliver_order_date: wh.deliver_order_date
        }));
      } catch (error) {
        console.error("데이터를 가져오는 중 에러가 발생했습니다:", error);
      }
    };

    const getProductById = (productId, products) => {
      const product = products.find((p) => p.url === productId);
      return product ? product.product_name : "상품 없음";
    };

    onMounted(fetchData);

    return {
      pageOneData,
      pageOneHeaders
    };
  },
};
</script>