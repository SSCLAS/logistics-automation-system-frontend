<template>
  <v-app style="background-color: #ffffff">
    <v-navigation-drawer width="244" permanent>
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
      <v-container fluid>
        <v-row>
          <v-col cols="9">
            <v-sheet min-height="70vh" rounded="lg" class="my-4">
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
];
</script>

<style scoped></style>
