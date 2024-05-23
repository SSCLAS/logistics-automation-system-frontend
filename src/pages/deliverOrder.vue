<template>
  <div>
    <p>상품</p>
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

export default {
  components: {
    TableComponent,
  },
  setup() {
    const pageOneData = ref([]);
    const pageOneHeaders = ref([
      { text: "입고 명령 날짜", value: "deliver_order_order_date" },
      { text: "입고 진행 여부", value: "deliver_order_processing" },
      { text: "입고 완료 날짜", value: "deliver_order_complete_date" },
      { text: "상품 이름", value: "product_id" },
      { text: "로봇 이름", value: "robot_id" },
    ]);

    const fetchData = async () => {
      try {
        const [deliverOrderResponse, productResponse, robotResponse] =
          await Promise.all([
            axios.get("http://127.0.0.1:8000/Deliver_order/"),
            axios.get("http://127.0.0.1:8000/Product/"),
            axios.get("http://127.0.0.1:8000/Robot/"),
          ]);
        const deliverOrders = deliverOrderResponse.data;
        const products = productResponse.data;
        const robots = robotResponse.data;

        pageOneData.value = deliverOrders.map((order) => ({
          deliver_order_order_date:
            order.deliver_order_order_date || "입고 명령 날짜 없음",
          deliver_order_processing: order.deliver_order_processing ? "O" : "X",
          deliver_order_complete_date:
            order.deliver_order_complete_date || "입고 완료 날짜 없음",
          product_id: getProductById(order.product_id, products),
          robot_id: getRobotById(order.robot_id, robots), // 수정: 함수명 변경
        }));
      } catch (error) {
        console.error("데이터를 가져오는 중 에러가 발생했습니다:", error);
      }
    };

    const getProductById = (productId, products) => {
      const product = products.find((p) => p.url === productId);
      return product ? product.product_name : "상품 없음";
    };

    const getRobotById = (robotId, robots) => {
      const robot = robots.find((r) => r.url === robotId); // 수정: 변수명 변경
      return robot ? robot.robot_name : "로봇 없음"; // 수정: 반환값 변경
    };

    onMounted(fetchData);

    return {
      pageOneData,
      pageOneHeaders,
    };
  },
};
</script>
