<template>
  <v-footer absolute padless style="background-color: #263238; color: white">
    <v-col class="text-center">
      © 2024 캡스톤 프로젝트. All rights reserved.
    </v-col>
  </v-footer>
</template>

<script>
export default {
 name : "Footer"
};
</script>

<style scoped>
v.footer {
  background-color: #263238;
  color: white;
  width: 100%; /* Footer가 전체 너비를 차지하도록 설정 */
  bottom: 0; /* 페이지 하단에 위치 */
}
</style>
