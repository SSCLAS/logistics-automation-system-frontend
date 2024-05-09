<template>
  <v-container>
    <div>
      <div>
        <div class="text-h5">현재 입고 현황</div>

        <v-data-table-server class="table table-striped">
          <thead :style="{ backgroundColor: '#343a40', color: 'white' }">
            <tr>
              <th>입고명령날짜</th>
              <th>입고 진행중</th>
              <th>입고완료날짜</th>
              <th>입고상품명</th>
              <th>작업로봇</th>
            </tr>
          </thead>
          <draggable v-model="deliver_orders" tag="tbody" item-key="id">
            <template #item="{ element }">
              <tr>
                <td>{{ element.deliver_order_order_date }}</td>
                <td>{{ element.deliver_order_processing ? "O" : "X" }}</td>
                <td>{{ element.deliver_order_complete_date }}</td>
                <td>{{ getProductById(element.product_id) }}</td>
                <td>{{ getRobotName(element.robot_name) }}</td>
              </tr>
            </template>
          </draggable>
        </v-data-table-server>
      </div>
      <rawDisplayer :value="list" title="List" />
    </div>
  </v-container>
</template>

<script lang="ts" setup>
import draggable from "vuedraggable";
import { ref } from "vue";

interface Product {
  url: string;
  product_name: string;
  robotName: string;
}

const deliver_orders = ref([]);
const products = ref<Product[]>([]);
const robots = ref<Product[]>([]);

const URL1 = "http://127.0.0.1:8000/Deliver_order/";
const URL2 = "http://127.0.0.1:8000/Product/";
const URL3 = "http://127.0.0.1:8000/Robot/";

fetch(URL1)
  .then((res) => res.json())
  .then((json) => (deliver_orders.value = json));
fetch(URL2)
  .then((res) => res.json())
  .then((json) => (products.value = json));
fetch(URL3)
  .then((res) => res.json())
  .then((json) => (robots.value = json));

function getProductById(productId: string) {
  const product = products.value.find((p) => p.url === productId);
  return product ? product.product_name : "상품 없음";
}
function getRobotName(robotName: string) {
  const robot = robots.value.find((r) => r.url === robotName);
  return robot ? robot.robot_name : "로봇 없음";
}
</script>

<style scoped>
.v-container.home {
  max-width: 1200px;
  margin: auto;
  padding: 20px;
}

.text-h3 {
  margin-bottom: 20px;
}

.table {
  margin-top: 20px;
}
</style>
