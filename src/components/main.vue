<template>
  <v-app style="background-color: #eceff1">
    <!-- 네비게이션 드로어 -->
    <v-navigation-drawer
      width="244"
      permanent
      style="background-color: #263238"
    >
      <!-- 로고 이미지 -->
      <a href="/"
        ><img src="@/images/dcu.png" alt="DCU Logo" class="dcu-logo"
      /></a>
      <!-- 팀 정보 -->
      <v-container fluid class="d-flex justify-center align-center">
        <v-row class="my-4 justify-center align-center">
          <v-sheet
            rounded="lg"
            style="background-color: #37474f; padding: 20px"
          >
            <!-- 팀 정보 텍스트 -->
            <h4 class="align-center">SSC개발팀</h4>
            <v-spacer class="my-2"></v-spacer>
            <p class="text-small">소속: 대구가톨릭대학교</p>
            <p class="text-small">과정: 캡스톤 프로젝트</p>
            <p class="text-small">[개발기간] 2024.03.04~2024.06.01</p>
          </v-sheet>
        </v-row>
      </v-container>
      <!-- 네비게이션 링크 리스트 -->
      <v-list dense>
        <v-list-item
          v-for="link in links"
          :key="link.text"
          :to="link.path"
          link
          class="list-item-spacing"
        >
          <v-list-item-icon>
            <v-icon>{{ link.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title class="d-flex align-center">
              <span>{{ link.text }}</span>
              <v-spacer></v-spacer>
              <v-icon>mdi-chevron-right</v-icon>
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <!-- 앱 바 -->
    <v-app-bar app style="background-color: #ffffff; height: 50px">
      <v-spacer></v-spacer>
      <!-- 사용자 프로필 -->
      <div class="user-container">
        <v-avatar color="grey" class="mx-2">
          <v-img></v-img>
        </v-avatar>
        <div class="user-info">
          <span class="user-name">SSC물류</span>
          <span class="user-level">Lv.10</span>
        </div>
      </div>
    </v-app-bar>

    <!-- 메인 콘텐츠 -->
    <v-main>
      <v-container fluid class="d-flex justify-center align-center h-100">
        <v-row>
          <v-col cols="9">
            <v-sheet
              min-height="70vh"
              rounded="lg"
              class="my-4"
              style="background-color: #ffffff"
              :elevation="8"
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
  { text: "적재현황", path: "/", icon: "mdi-package-variant-closed" },
  { text: "상품리스트", path: "/product", icon: "mdi-history" },
  { text: "출고명령", path: "/stockOrder", icon: "mdi-export" },
  { text: "입고명령", path: "/deliverOrder", icon: "mdi-import" },
  { text: "로봇", path: "/robot", icon: "mdi-robot" },
  { text: "스캔", path: "/scan", icon: "mdi-camera" },
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
