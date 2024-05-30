<template>
  <v-container>
    <v-row>
      <v-col
        v-for="(robot, index) in robot_list"
        :key="index"
        cols="12"
        sm="6"
        md="4"
      >
        <v-card class="mx-auto" max-width="344">
          <v-img height="200" :src="robot.robot_image" cover></v-img>

          <v-card-title> 로봇명: {{ robot.robot_name }} </v-card-title>

          <v-card-subtitle>
            {{ robot.robot_status }}
          </v-card-subtitle>

          <v-card-actions>
            <v-spacer></v-spacer>

            <v-btn
              :icon="show[index] ? 'mdi-chevron-up' : 'mdi-chevron-down'"
              @click="toggleShow(index)"
            ></v-btn>
          </v-card-actions>

          <v-expand-transition>
            <div v-show="show[index]">
              <v-divider></v-divider>
              <v-card-text> {{ robot.robot_describtion }} </v-card-text>
            </div>
          </v-expand-transition>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";

const robot_list = ref([]);
const show = ref([]);

async function getRobots() {
  try {
    const response = await axios.get("http://192.168.0.100:8000/Robot/");
    if (response.status === 200) {
      robot_list.value = response.data;
      show.value = response.data.map(() => false); // Initialize show array with false for each robot
      console.log("성공");
    } else {
      console.log("Failed to fetch product data");
    }
  } catch (error) {
    console.log(error);
  }
}

function toggleShow(index) {
  show.value[index] = !show.value[index];
}

getRobots();
</script>

<style scoped>
.v-card {
  margin-bottom: 20px;
}
</style>
