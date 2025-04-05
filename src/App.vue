<template>
  <ui-layout>
    <ui-top-app-bar app content-selector="#main-content" title="Contact Book" @nav="openDrawer = true">
      <template #toolbar="{ toolbarItemClass }">
        <ui-icon-button :class="toolbarItemClass" icon="home" @click="goToHome"></ui-icon-button>
        <ui-icon-button :class="toolbarItemClass" icon="person_add" @click="goToAdd"></ui-icon-button>
      </template>
    </ui-top-app-bar>
    <ui-drawer v-model="openDrawer" type="modal">
      <ui-drawer-header>
        <ui-drawer-title>Contact Book</ui-drawer-title>
      </ui-drawer-header>
      <ui-drawer-content>
        <ui-list>
          <ui-item @click="goToHome">
            <ui-item-first-content><ui-icon>home</ui-icon></ui-item-first-content>
            <ui-item-text-content>Home</ui-item-text-content>
          </ui-item>
          <ui-item @click="goToAdd">
            <ui-item-first-content><ui-icon>person_add</ui-icon></ui-item-first-content>
            <ui-item-text-content>Add Contact</ui-item-text-content>
          </ui-item>
        </ui-list>
      </ui-drawer-content>
    </ui-drawer>
    <ui-content>
      <div id="main-content">
        <router-view />
      </div>
    </ui-content>
  </ui-layout>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const openDrawer = ref(false);
const router = useRouter();

const goToHome = () => {
  openDrawer.value = false;
  router.push({ name: 'ContactList' });
};

const goToAdd = () => {
  openDrawer.value = false;
  router.push({ name: 'AddContact' });
};
</script>

<style scoped>
#main-content {
  padding: 20px;
}

.ui-icon-button {
  cursor: pointer;
}
</style>
