<script setup lang="ts">
import { getStringSizeInBytes } from './text-statistics.service';
import { formatBytes } from '@/utils/convert';

const text = ref('');
</script>

<template>
  <c-card>
    <c-input-text v-model:value="text" multiline placeholder="你的文本..." rows="5" />

    <div mt-5 flex>
      <n-statistic label="字符计数" :value="text.length" flex-1 />
      <n-statistic label="字数" :value="text === '' ? 0 : text.split(/\s+/).length" flex-1 />
      <n-statistic label="行数" :value="text === '' ? 0 : text.split(/\r\n|\r|\n/).length" flex-1 />
      <n-statistic label="字节大小" :value="formatBytes(getStringSizeInBytes(text))" flex-1 />
    </div>
  </c-card>
</template>
