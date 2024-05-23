<template>
  <div>
    <TableComponent
      :items="pageOneData"
      :headers="pageOneHeaders"
      title="입고 현황"
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
      { text: "입고 명령 일자", value: "stock_order_order_date" },
      { text: "입고 상황", value: "stock_order_processing" },
      { text: "입고 완료 일자", value: "stock_order_complete_date" },
      { text: "상품 이름", value: "product_id" },
      { text: "창고 이름", value: "ware_house_name" }, // 창고 이름으로 수정
    ]);

    const fetchData = async () => {
      try {
        const [wareHouseResponse, productResponse, stockOrderResponse] =
          await Promise.all([
            axios.get(API_NAME['Ware_house']),
            axios.get(API_NAME['Product']),
            axios.get(API_NAME['Stock_order']),
          ]);

        const wareHouses = wareHouseResponse.data;
        const products = productResponse.data;
        const stockOrders = stockOrderResponse.data;

        pageOneData.value = stockOrders.map((order) => ({
          stock_order_order_date: order.stock_order_order_date || "없음",
          stock_order_processing: order.stock_order_processing ? "진행" : "완료",
          stock_order_complete_date: order.stock_order_complete_date || "없음",
          product_id: getProductById(order.product_id, products),
          ware_house_name: getWareHouseName(order.ware_house_id, wareHouses), // 창고 이름 매핑
        }));
      } catch (error) {
        console.error("데이터를 가져오는 중 에러가 발생했습니다:", error);
      }
    };

    const getProductById = (productId, products) => {
      const product = products.find((p) => p.url === productId);
      return product ? product.product_name : "상품 없음";
    };

    const getWareHouseName = (wareHouseId, wareHouses) => {
      const wareHouse = wareHouses.find((wh) => wh.url === wareHouseId);
      return wareHouse ? wareHouse.ware_house_name : "창고 없음";
    };

    onMounted(fetchData);

    return {
      pageOneData,
      pageOneHeaders,
    };
  },
};
</script>
