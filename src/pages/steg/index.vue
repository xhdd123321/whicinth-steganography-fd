<script setup lang="ts">
import CustomHeader from "@/components/CustomHeader.vue";
import { onBeforeMount } from "vue";
import { useUserStore } from "@/store/userStore";
import CustomFooter from "@/components/CustomFooter.vue";
const userStore = useUserStore();
onBeforeMount(() => {
  if (sessionStorage.getItem("store")) {
    userStore.setInfo(
      Object.assign(
        {},
        userStore.$state,
        JSON.parse(sessionStorage.getItem("store") || "")
      )
    );
  }

  window.addEventListener("beforeunload", () => {
    sessionStorage.setItem("store", JSON.stringify(userStore.$state));
  });
});
</script>

<template>
  <div class="common-layout">
    <a-layout>
      <a-layout-header><CustomHeader /></a-layout-header>
      <a-layout-content>
        <router-view v-slot="{ Component }">
          <keep-alive>
            <component
              :is="Component"
              :key="$route.name"
              v-if="$route.meta.keepAlive"
            ></component>
          </keep-alive>
          <component
            :is="Component"
            :key="$route.name"
            v-if="!$route.meta.keepAlive"
          ></component> </router-view
      ></a-layout-content>
      <a-layout-footer><CustomFooter /></a-layout-footer>
    </a-layout>
  </div>
</template>

<style scoped>
.common-layout {
  width: 100vw;
  height: 100vh;
}
</style>
