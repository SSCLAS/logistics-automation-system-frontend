<template>
  <div>
    <TableComponent
      :items="pageOneData"
      :headers="pageOneHeaders"
      title="로봇 현황"
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
      { text: "로봇 이름", value: "robot_name" },
      { text: "로봇 활성화 여부", value: "robot_status" },
    ]);

    const fetchData = async () => {
      try {
        const [robotResponse] = await Promise.all([
          axios.get("http://127.0.0.1:8000/Robot/"),
        ]);
        const robots = robotResponse.data;

        pageOneData.value = robots.map((r) => ({
          robot_name: r.robot_name || "없음",
          robot_status: r.robot_status ? "O" : "X",
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
