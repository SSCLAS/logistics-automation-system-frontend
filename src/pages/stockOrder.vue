<template>
  <v-container>
    <div>
      <div>
        <div class="text-h5">출고명령리스트</div>

        <v-data-table-server class="table table-striped">
          <thead :style="{ backgroundColor: '#343a40', color: 'white' }">
            <tr>
              <th>출고명령날짜</th>
              <th>출고 진행중</th>
              <th>출고완료날짜</th>
              <th>출고상품</th>
              <th>창고번호</th>
            </tr>
          </thead>
          <draggable v-model="stock_order" tag="tbody" item-key="id">
            <template #item="{ element }">
              <tr>
                <td>{{ element.stock_order_order_date }}</td>
                <td>{{ element.stock_order_processing ? "O" : "X" }}</td>
                <td>{{ element.stock_order_complete_date }}</td>
                <td>{{ element.product_id }}</td>
                <td>{{ element.ware_house_id }}</td>
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
}

const stock_order = ref([]);
const ware_houses = ref([]);
const products = ref<Product[]>([]);

const URL1 = "http://127.0.0.1:8000/Stock_order/";
const URL2 = "http://127.0.0.1:8000/Ware_house/";
const URL3 = "http://127.0.0.1:8000/Product/";

fetch(URL1)
  .then((res) => res.json())
  .then((json) => (stock_order.value = json));
fetch(URL2)
  .then((res) => res.json())
  .then((json) => (products.value = json));
fetch(URL3)
  .then((res) => res.json())
  .then((json) => (products.value = json));

function getProductById(productId: string) {
  const product = products.value.find((p) => p.url === productId);
  return product ? product.product_name : "상품 없음";
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
