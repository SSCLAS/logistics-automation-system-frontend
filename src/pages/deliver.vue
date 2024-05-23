<template>
    <div>
      <p>상품</p>
      <TableComponent
        :items="pageOneData"
        :headers="pageOneHeaders"
        title="출고 현황"
      />
      {{ deliver_data }}
    </div>
</template>

<script setup>
import { ref } from "vue";
var deliver_data = [];
const API_NAME={
    "Robot": "http://192.168.0.100:8000/Robot/",
    "Product": "http://192.168.0.100:8000/Product/",
    "Ware_house": "http://192.168.0.100:8000/Ware_house/",
    "Deliver_order": "http://192.168.0.100:8000/Deliver_order/",
    "Stock_order": "http://192.168.0.100:8000/Stock_order/"
}

fetch.get(API_NAME["Deliver_order"])
    .then(res => res.json())
    .then(json => deliver_data.value = json)
</script>