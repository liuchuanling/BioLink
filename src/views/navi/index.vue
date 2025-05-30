<script lang="ts"></script>
<template>
  <div class="mb-8 text-center">
    <div
      class="text-4xl font-extrabold bg-gradient-to-r from-green-400 via-blue-400 to-purple-500 bg-clip-text text-transparent drop-shadow-sm"
    >
      资源导航
    </div>
    <p class="mt-2 text-base text-gray-700">以下是一些常用的生物信息学网站和数据库，点击卡片可直接访问。</p>
  </div>

  <div class="p-2">
    <div class="flex flex-wrap gap-4">
      <div
        v-for="site in sites"
        :key="site.name"
        class="flex flex-col items-center w-64 p-4 cursor-pointer bg-white border border-gray-200 rounded-lg transition-shadow hover:shadow-lg"
        @click="openSite(site.url)"
      >
        <img :src="site.img" :alt="site.name" class="w-20 h-20 mb-3 object-contain bg-white rounded" />
        <div>
          <h2 class="mb-2 text-lg font-semibold text-center">{{ site.name }}</h2>
          <p class="text-sm text-gray-600">{{ site.desc }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import naviData from "@/assets/json/navigate.json";

interface Site {
  name: string;
  url: string;
  img: string;
  desc: string;
}

const sites = ref<Site[]>([]);

const init = () => {
  sites.value = naviData.data.map((item: any) => ({
    name: item.name,
    url: item.url,
    img: item.img,
    desc: item.desc
  }));
};

const openSite = (url: string) => {
  window.open(url, "_blank");
};

init();
</script>
