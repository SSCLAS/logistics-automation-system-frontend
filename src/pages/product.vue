<template>
  <div>
    <TableComponent
      :items="pageOneData"
      :headers="pageOneHeaders"
      title="상품 리스트"
    />
  </div>
</template>

<script>
import axios from "axios";
import TableComponent from "../components/table.vue";
import { ref, onMounted } from "vue";

const API_NAME={
    "Robot": "http://192.168.0.100:8000/Robot/",
    "Product": "http://192.168.0.100:8000/Product/",
    "Ware_house": "http://192.168.0.100:8000/Ware_house/",
    "Deliver_order": "http://192.168.0.100:8000/Deliver_order/",
    "Stock_order": "http://192.168.0.100:8000/Stock_order/"
}


export default {
  components: {
    TableComponent,
  },
  setup() {
    const pageOneData = ref([]);
    const pageOneHeaders = ref([
      { text: "상품 이름", value: "product_name" },
      { text: "생산 날짜", value: "product_date" },
      { text: "상품 제조 회사", value: "product_manufacture" }, 
      { text: "상품 가격", value: "product_price" },
    ]);

    const fetchData = async () => {
      try {
        const [productResponse] = await Promise.all([
          axios.get(API_NAME['Product']),
        ]);
        const products = productResponse.data;

        pageOneData.value = products.map((wh) => ({
          product_name: wh.product_name || "상품 이름 없음",
          product_date: wh.product_date || "생산 날짜 없음",
          product_manufacture: wh.product_manufacture || "제조 회사 없음",
          product_price: wh.product_price || "상품 가격 없음",
        }));
      } catch (error) {
        console.error("데이터를 가져오는 중 에러가 발생했습니다:", error);
      }
    };

    onMounted(fetchData);

    return {
      pageOneData,
      pageOneHeaders,
    };
  },
};
</script>
