<template>
  <v-app style="background-color: #ffffff">
    <v-navigation-drawer
      width="244"
      permanent
      style="background-color: #ffffff; border-right: 2px solid #ddd"
    >
      <a href="/"
        ><img src="@/images/1.png" alt="DCU Logo" class="dcu-logo"
      /></a>
      <v-container fluid class="d-flex justify-center align-center">
        <v-row class="my-4 justify-center align-center">
          <v-sheet
            rounded="lg"
            style="background-color: #2979ff; padding: 20px"
          >
            <h4 class="align-center" style="color: #ffffff">SSC</h4>
            <v-spacer class="my-2"></v-spacer>
            <p class="text-small" style="color: #ffffff; font-weight: bold">
              소속: <span style="font-weight: bold">대구가톨릭대학교</span>
            </p>
            <p class="text-small" style="color: #ffffff; font-weight: bold">
              과정: 캡스톤 프로젝트
            </p>
            <p class="text-small" style="color: #ffffff; font-weight: bold">
              [개발기간] 2024.03.04~2024.06.01
            </p>
          </v-sheet>
        </v-row>
      </v-container>
      <v-list dense>
        <v-list-item
          v-for="link in links"
          :key="link.text"
          :to="link.path"
          link
          class="list-item-spacing"
        >
          <v-list-item-icon>
            <v-icon :color="'black'">{{ link.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title
              class="d-flex align-center"
              style="color: black; font-weight: bold"
            >
              <span>{{ link.text }}</span>
              <v-spacer></v-spacer>
              <v-icon>mdi-chevron-right</v-icon>
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app style="background-color: #ffffff; height: 50px">
      <v-spacer></v-spacer>
      <div class="user-container">
        <v-avatar color="grey" class="mx-2">
          <v-img></v-img>
        </v-avatar>
        <div class="user-info">
          <span class="user-name">SSC 물류</span>
          <span class="user-level">Lv.10</span>
        </div>
      </div>
    </v-app-bar>

    <v-main>
      <v-container fluid class="d-flex justify-center align-center h-100">
        <v-row>
          <v-col cols="12">
            <v-sheet
              rounded="lg"
              class="my-4"
              style="background-color: #ffffff; border: 2px solid #ddd"
            >
              <router-view />
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup>
const links = [
  { text: "창고적재상태", path: "/", icon: "mdi-package-variant-closed" },
  { text: "제품", path: "/product", icon: "mdi-package" },
  { text: "입고이력", path: "/stockOrder", icon: "mdi-import" },
  { text: "출고이력", path: "/deliverOrder", icon: "mdi-export" },
  { text: "로봇", path: "/robot", icon: "mdi-robot" },
];
</script>

<style scoped>
/* 이미지 스타일 추가 */
.dcu-logo {
  width: 50%;
  height: auto;
  display: block;
  margin: 10px auto;
  padding-top: 15px;
}
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
/* 텍스트 스타일 조정 */
.align-center {
  text-align: center;
}
.text-small {
  font-size: 15px;
}
/* v-spacer 간격 조정 */
.my-2 {
  margin-top: 8px;
  margin-bottom: 8px;
}
/* 네비게이션 리스트 아이템 간격 조정 */
.list-item-spacing {
  margin-bottom: 13px; /* 각 리스트 아이템 사이의 간격 조정 */
  font-size: 12px;
}
/* 사용자 정보 스타일 */
.user-container {
  display: flex;
  align-items: center;
  margin-bottom: 14px; /* 아바타와 텍스트 사이의 간격 조정 */
}
.user-avatar {
  margin-bottom: 14px; /* 아바타 아래 간격 추가 */
}
.user-info {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-left: 8px;
  margin-right: 40px;
}
.user-name {
  color: black;
  font-size: 12px;
}
.user-level {
  color: black;
  font-size: 12px;
  margin-top: 2px; /* 텍스트 간격 조정 */
}
</style>
