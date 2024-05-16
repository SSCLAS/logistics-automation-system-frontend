<template>
  <v-app style="background-color: #eceff1">
    <v-navigation-drawer
      width="244"
      permanent
      style="background-color: #263238"
    >
      <h1 class="d-flex justify-center align-center">SSC</h1>
      <v-container fluid class="d-flex justify-center align-center">
        <v-row class="my-4 justify-center align-center">
          <v-sheet
            rounded="lg"
            style="background-color: #37474f; padding: 20px"
          >
            <h2 class="align-center">SSC개발팀</h2>
            <p>팀원</p>
            <p>김민정</p>
            <p>김승현</p>
            <p>김정훈</p>
            <p>박한백</p>
            <p>이상기</p>
            <p>임희정</p>
          </v-sheet>
        </v-row>
      </v-container>
      <v-list dense>
        <v-list-item
          v-for="link in links"
          :key="link.text"
          :to="link.path"
          link
        >
          <v-list-item-icon>
            <v-icon>{{ link.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>{{ link.text }}</v-list-item-title>
          </v-list-item-content>
          <v-list-item-action>
            <v-icon>mdi-chevron-right</v-icon>
          </v-list-item-action>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <v-container fluid class="d-flex justify-center align-center h-100">
        <v-row>
          <v-col cols="9">
            <v-sheet
              min-height="70vh"
              rounded="lg"
              class="my-4"
              style="background-color: #ffffff"
            >
              <router-view />
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
  <Footer />
</template>

<script setup>
const links = [
  { text: "적재현황", path: "/", icon: "mdi-package-variant-closed" },
  { text: "상품리스트", path: "/product", icon: "mdi-history" },
  { text: "출고명령", path: "/stockOrder", icon: "mdi-export" },
  { text: "입고명령", path: "/deliverOrder", icon: "mdi-import" },
  { text: "로봇", path: "/robot", icon: "mdi-robot" },
];
</script>

<style scoped>
/* v-list-item-content 내부 요소 수직 정렬 */
.v-list-item-content {
  display: flex;
  align-items: center;
}

/* v-list-item-action 내부 요소 수직 정렬 */
.v-list-item-action {
  display: flex;
  justify-content: center;
  align-items: center;
}

/* 팀원 목록을 중앙에 배치하기 위한 스타일 */
.align-center {
  text-align: center;
}

.d-flex {
  display: flex;
}

.flex-column {
  flex-direction: column;
}

.align-center {
  align-items: center;
}

.mb-4 {
  margin-bottom: 16px; /* 팀원 목록과 팀명 간 간격 조정 */
}
</style>
