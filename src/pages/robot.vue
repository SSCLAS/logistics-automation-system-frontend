<template>
  <v-container>
    <div>
      <div>
        <div class="text-h5">로봇 현황</div>

        <v-data-table-server class="table table-striped">
          <thead :style="{ backgroundColor: '#343a40', color: 'white' }">
            <tr>
              <th>로봇이름</th>
              <th>로봇 활성화</th>
            </tr>
          </thead>
          <draggable v-model="robots" tag="tbody" item-key="id">
            <template #item="{ element }">
              <tr>
                <td>{{ element.robot_name }}</td>
                <td>{{ element.robot_status ? "O" : "X" }}</td>
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

const robots = ref([]);

const URL1 = "http://127.0.0.1:8000/Robot/";

fetch(URL1)
  .then((res) => res.json())
  .then((json) => (robots.value = json));
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
