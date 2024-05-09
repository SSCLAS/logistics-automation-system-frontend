<template>
  <v-container>
    <div>
      <div>
        <div class="text-h5">현재 적재 기록</div>

        <v-data-table-server class="table table-striped">
          <thead :style="{ backgroundColor: '#343a40', color: 'white' }">
            <tr>
              <th>상품명</th>
              <th>상품입고날짜</th>
              <th>상품회사</th>
              <th>상품가격</th>
            </tr>
          </thead>
          <draggable v-model="products" tag="tbody" item-key="id">
            <template #item="{ element }">
              <tr>
                <td>{{ element.product_name }}</td>
                <td>{{ element.product_date }}</td>
                <td>{{ element.product_manufacture }}</td>
                <td>{{ element.product_price }}</td>
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

const products = ref<Product[]>([]);

const URL2 = "http://127.0.0.1:8000/Product/";

fetch(URL2)
  .then((res) => res.json())
  .then((json) => (products.value = json));
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
