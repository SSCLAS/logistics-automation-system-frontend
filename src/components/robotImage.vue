<template>
  <v-card class="mx-auto" max-width="344">
    <v-img height="400px" :src="robot_list[0].robot_image" cover></v-img>

    <v-card-title>
      {{ robot_list[0].robot_name }}
    </v-card-title>

    <v-card-subtitle>
      {{ robot_list[0].robot_name }}
    </v-card-subtitle>

    <v-card-actions>
      <v-spacer></v-spacer>

      <v-btn
        :icon="show ? 'mdi-chevron-up' : 'mdi-chevron-down'"
        @click="show = !show"
      ></v-btn>
    </v-card-actions>

    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>

        <v-card-text> {{ robot_list[0].robot_name }} </v-card-text>
      </div>
    </v-expand-transition>
  </v-card>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";

const robot_list = ref({});
async function getRobots() {
  try {
    const response = await axios.get("http://192.168.0.100:8000/Robot/");
    if (response.status == 200) {
      robot_list.value = response.data;
      console.log("성공");
    } else {
      console.log("Failed to fetch product data");
    }
  } catch (error) {
    console.log(error);
  }
}

getRobots();
</script>
