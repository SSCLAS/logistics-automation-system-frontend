<template>
  <div>
    <TableComponent
      :items="pageOneData"
      :headers="pageOneHeaders"
      title="적재 현황"
    />
  </div>
</template>

<script>
import axios from "axios";
import TableComponent from "../components/table.vue";
import { ref, onMounted } from "vue";

export default {
  components: {
    TableComponent,
  },
  setup() {
    const pageOneData = ref([]);
    const pageOneHeaders = ref([
      { text: "창고 이름", value: "ware_house_name" },
      { text: "창고 상태", value: "ware_house_status" },
      { text: "제품 이름", value: "product_id" }, // 제품 이름으로 헤더 수정
    ]);

    const fetchData = async () => {
      try {
        const [wareHouseResponse, productResponse] = await Promise.all([
          axios.get("http://127.0.0.1:8000/Ware_house/"),
          axios.get("http://127.0.0.1:8000/Product/"),
        ]);
        const wareHouses = wareHouseResponse.data;
        const products = productResponse.data;

        pageOneData.value = wareHouses.map((wh) => ({
          ware_house_name: wh.ware_house_name || "없음",
          ware_house_status: wh.ware_house_status ? "O" : "X",
          product_id: getProductById(wh.product_id, products),
        }));
      } catch (error) {
        console.error("데이터를 가져오는 중 에러가 발생했습니다:", error);
      }
    };

    const getProductById = (productId, products) => {
      const product = products.find((p) => p.url === productId); // 제품을 URL로 찾기
      return product ? product.product_name : "상품 없음";
    };

    onMounted(fetchData);

    return {
      pageOneData,
      pageOneHeaders,
    };
  },
};
</script>
