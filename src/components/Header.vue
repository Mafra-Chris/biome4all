<template>
  <header>
    <div class="flex justify-between items-center">
      <img
        src="../assets/logo-biome4all-branco.png"
        class="logo-color h-16 py-2"
        alt="Logo Biome"
      />
    </div>
    <div class="px-6 md:px-14">
      <hr class="border-b-1" />
    </div>
  </header>
</template>

<script setup lang="ts"></script>

<style scoped>
.logo-color {
  filter: invert(28%) sepia(18%) saturate(1270%) hue-rotate(66deg)
    brightness(51%) contrast(88%);
}
</style>
