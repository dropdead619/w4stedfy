<script setup lang="ts">
const isDark = useDark();
const toggleDark = useToggle(isDark);
</script>

<template>
  <div class="flex justify-between w-full h-12 py-2 pl-16 align-middle bg-white shadow-md dark:bg-theme-dark-4 pr-11 dark:shadow-sm dark:shadow-slate-50" />
  <div class="fixed top-0 left-0 right-0 z-10 flex justify-between w-full h-12 py-2 pl-16 align-middle bg-white shadow-md dark:bg-theme-dark-4 pr-11 dark:shadow-sm dark:shadow-slate-50">
    <div class="fixed flex mt-2 left-4">
      <IconBack class="w-8 mr-1 cursor-pointer" @click="$router.go(-1)" />
    </div>
    <div class="text-xl sm:text-2xl dark:text-slate-50">
      <RouterLink :to="{ name: 'Bands' }" class="flex font-bold">
        <img
          class="w-8 mr-2"
          src="../assets/logo.webp"
          alt="logo"
        >
        w4stedfy
      </RouterLink>
    </div>
    <BaseButton class="py-1 bg-primary" @click="toggleDark()">
      <IconMoon v-if="isDark" />
      <IconSun v-else />
    </BaseButton>
  </div>
</template>
