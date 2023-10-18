<template>
    <q-dialog v-model="modelValueComputed">
      <q-card>
        <q-card-section class="text-h6">
          新しいアップデートがあります！
        </q-card-section>
        <q-card-section>
          <!-- href の前にコロンつけないとダメ -->
          <q-btn
            color="primary"
            :href="downloadLink"
            tag="a"
            target="_blank"
            label="ダウンロードページへ"
            @click="toDialogClosedState"
          />
          <q-btn label="後で" @click="toDialogClosedState" />
        </q-card-section>
      </q-card>
    </q-dialog>
  </template>
  
  <script setup lang="ts">
  import { computed } from "vue";
  
  const props =
    defineProps<{
      modelValue: boolean;
    }>();
  const emit =
    defineEmits<{
      (e: "update:modelValue", val: boolean): void;
    }>();
  
  const modelValueComputed = computed({
    get: () => props.modelValue,
    set: (val) => emit("update:modelValue", val),
  });
  
  const toDialogClosedState = () => {
    modelValueComputed.value = false;
  };
  
  const downloadLink = "https://voicevox.hiroshiba.jp/";
  </script>
  