<template>
  <v-container>
    <div>
      <div>
        <div class="text-h5">현재 적재 현황</div>

        <v-data-table-server class="table table-striped">
          <thead :style="{ backgroundColor: '#343a40', color: 'white' }">
            <tr>
              <th>창고번호</th>
              <th>적재상태</th>
              <th>적재상품</th>
              <th>적재날짜</th>
            </tr>
          </thead>
          <draggable v-model="ware_houses" tag="tbody" item-key="id">
            <template #item="{ element }">
              <tr>
                <td>{{ element.ware_house_name }}</td>
                <td>{{ element.ware_house_status ? "O" : "X" }}</td>
                <td>{{ getProductById(element.product_id) }}</td>
                <td>{{ getProductById(element.product_date) }}</td>
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

const ware_houses = ref([]);
const products = ref<Product[]>([]);

const URL1 = "http://127.0.0.1:8000/Ware_house/";
const URL2 = "http://127.0.0.1:8000/Product/";

fetch(URL1)
  .then((res) => res.json())
  .then((json) => (ware_houses.value = json));
fetch(URL2)
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
