<template>
  <div class="flex gap-2 h-full font-sans bg-gradient-to-br from-blue-50 via-cyan-50 to-white">
    <!-- 左侧：用户输入框 -->
    <section class="flex flex-col flex-[1.1] min-w-[280px] bg-white/90 rounded-lg shadow-2xl p-7 border border-blue-100">
      <div class="mb-6 flex items-center gap-2">
        <span class="inline-block w-1.5 h-6 bg-blue-500 rounded-sm"></span>
        <span class="text-xl font-bold text-blue-700 tracking-wide">用户输入</span>
      </div>
      <textarea
        v-model="userInput"
        placeholder="请输入您的问题..."
        @keyup.enter="sendMessage"
        rows="6"
        class="w-full p-3 mb-4 text-base resize-none bg-blue-50 border border-blue-200 rounded-xl focus:bg-white focus:border-cyan-500 outline-none transition"
      ></textarea>
      <button
        @click="sendMessage"
        class="px-6 py-2 mb-5 text-base font-semibold text-white cursor-pointer bg-gradient-to-r from-blue-600 to-cyan-400 rounded-xl shadow hover:from-blue-700 hover:to-cyan-500 transition"
      >
        发送
      </button>
      <div class="flex-1 max-h-[220px] mt-2 overflow-y-auto">
        <div
          v-for="(msg, idx) in userHistory"
          :key="idx"
          class="px-3 py-2 mb-2 text-base bg-cyan-50 border border-cyan-100 rounded-lg shadow-sm"
        >
          <span class="mr-1 font-medium text-blue-600">你：</span>{{ msg }}
        </div>
      </div>
    </section>

    <!-- 中间：AI返回框 -->
    <section
      class="flex flex-col flex-[2.2] min-w-[380px] bg-white/95 rounded-lg shadow-2xl p-8 border border-cyan-100 items-start justify-start"
    >
      <div class="mb-4 text-xl font-bold text-cyan-700 tracking-wide">AI 回复</div>
      <div
        v-if="aiResponse"
        class="min-h-[140px] px-5 py-5 mb-2 text-[1.12rem] text-gray-900 whitespace-pre-wrap bg-gradient-to-br from-cyan-50 to-blue-50 rounded-2xl shadow"
      >
        {{ aiResponse }}
      </div>
      <div v-else class="mt-12 text-[1.08rem] text-gray-400">等待您的提问...</div>
    </section>

    <!-- 右侧：翻译相关 -->
    <section
      class="flex flex-col flex-[1.1] min-w-[280px] bg-white/90 rounded-lg shadow-2xl p-7 border border-blue-100 justify-between"
    >
      <div class="mb-6">
        <div class="mb-4 text-xl font-bold text-blue-700 tracking-wide">事实科学翻译</div>
        <textarea
          v-model="factTranslation"
          placeholder="AI 事实科学翻译将在此显示"
          readonly
          rows="5"
          class="w-full min-h-[90px] p-3 text-base bg-cyan-50 border border-cyan-200 rounded-xl resize-none text-gray-900"
        ></textarea>
      </div>
      <div>
        <div class="mb-4 text-xl font-bold text-blue-700 tracking-wide">翻译纠正</div>
        <textarea
          v-model="correction"
          placeholder="AI 翻译纠正将在此显示"
          readonly
          rows="5"
          class="w-full min-h-[90px] p-3 text-base bg-cyan-50 border border-cyan-200 rounded-xl resize-none text-gray-900"
        ></textarea>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref } from "vue";

const userInput = ref("");
const userHistory = ref([]);
const aiResponse = ref("");
const factTranslation = ref("");
const correction = ref("");

import md5 from "md5";

const appid = "20250530002370098";
const key = "7IWFBvwJjG6OP2yv6Vj6";

function translateText(query, fromLang, toLang) {
  return new Promise((resolve, reject) => {
    const salt = Date.now();
    const sign = md5(appid + query + salt + key);
    const url = `https://fanyi-api.baidu.com/api/trans/vip/translate?q=${encodeURIComponent(
      query
    )}&from=${fromLang}&to=${toLang}&appid=${appid}&salt=${salt}&sign=${sign}`;

    fetch(url)
      .then(res => res.json())
      .then(data => {
        if (data && data.trans_result && data.trans_result[0]) {
          resolve(data.trans_result[0].dst);
        } else {
          reject(data);
        }
      })
      .catch(reject);
  });
}

async function sendMessage() {
  if (!userInput.value.trim()) return;
  userHistory.value.push(userInput.value);

  // AI回复模拟
  aiResponse.value = `AI回复: ${userInput.value}`;

  // 实时调用百度翻译API
  try {
    factTranslation.value = "翻译中...";
    correction.value = "翻译中...";
    // 事实科学翻译：中文->英文
    const fact = await translateText(userInput.value, "zh", "en");
    factTranslation.value = fact;
    // 翻译纠正：英文->中文
    const corr = await translateText(fact, "en", "zh");
    correction.value = corr;
  } catch (e) {
    factTranslation.value = "翻译失败";
    correction.value = "翻译失败";
  }

  userInput.value = "";
}
</script>

<!-- Tailwind CSS handles all styles, no <style> block needed -->
