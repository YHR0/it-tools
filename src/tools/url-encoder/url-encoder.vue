<script setup lang="ts">
import { useCopy } from '@/composable/copy';
import { useValidation } from '@/composable/validation';
import { isNotThrowing } from '@/utils/boolean';
import { withDefaultOnError } from '@/utils/defaults';

const encodeInput = ref('Hello world :)');
const encodeOutput = computed(() => withDefaultOnError(() => encodeURIComponent(encodeInput.value), ''));

const encodedValidation = useValidation({
  source: encodeInput,
  rules: [
    {
      validator: value => isNotThrowing(() => encodeURIComponent(value)),
      message: 'Impossible to parse this string',
    },
  ],
});

const { copy: copyEncoded } = useCopy({ source: encodeOutput, text: 'Encoded string copied to the clipboard' });

const decodeInput = ref('Hello%20world%20%3A)');
const decodeOutput = computed(() => withDefaultOnError(() => decodeURIComponent(decodeInput.value), ''));

const decodeValidation = useValidation({
  source: decodeInput,
  rules: [
    {
      validator: value => isNotThrowing(() => decodeURIComponent(value)),
      message: 'Impossible to parse this string',
    },
  ],
});

const { copy: copyDecoded } = useCopy({ source: decodeOutput, text: 'Decoded string copied to the clipboard' });
</script>

<template>
  <c-card title="编码">
    <c-input-text
      v-model:value="encodeInput"
      label="你的字符串 :"
      :validation="encodedValidation"
      multiline
      autosize
      placeholder="The string to encode"
      rows="2"
      mb-3
    />

    <c-input-text
      label="已编码的字符串:"
      :value="encodeOutput"
      multiline
      autosize
      readonly
      placeholder="已编码的字符串"
      rows="2"
      mb-3
    />

    <div flex justify-center>
      <c-button @click="copyEncoded()">
        Copy(复制)
      </c-button>
    </div>
  </c-card>
  <c-card title="解码">
    <c-input-text
      v-model:value="decodeInput"
      label="你要解码的字符串 :"
      :validation="decodeValidation"
      multiline
      autosize
      placeholder="The string to decode"
      rows="2"
      mb-3
    />

    <c-input-text
      label="已解码的字符串 :"
      :value="decodeOutput"
      multiline
      autosize
      readonly
      placeholder="Your string decoded"
      rows="2"
      mb-3
    />

    <div flex justify-center>
      <c-button @click="copyDecoded()">
        Copy(复制)
      </c-button>
    </div>
  </c-card>
</template>
