<template>
  <q-dialog
    v-model="modelValueComputed"
    maximized
    transition-show="jump-up"
    transition-hide="jump-down"
    class="accept-terms-dialog transparent-backdrop"
  >
    <q-layout container view="hHh Lpr lff" class="bg-background">
      <q-header class="q-py-sm">
        <q-toolbar>
          <div class="column">
            <q-toolbar-title class="text-display"
              >利用規約に関するお知らせ</q-toolbar-title
            >
          </div>

          <q-space />

          <div class="row items-center no-wrap">
            <q-btn
              unelevated
              label="同意せずに終了"
              color="toolbar-button"
              text-color="toolbar-button-display"
              class="text-no-wrap q-mr-md text-bold"
              @click="handler(false)"
            />

            <q-btn
              unelevated
              label="同意して使用開始"
              color="toolbar-button"
              text-color="toolbar-button-display"
              class="text-no-wrap text-bold"
              @click="handler(true)"
            />
          </div>
        </q-toolbar>
      </q-header>

      <q-page-container>
        <q-page>
          <p class="text-body1 q-mb-lg">
            多くの人が安心して VOICEVOX
            を使えるよう、利用規約への同意をお願いします。
          </p>
          <q-card flat bordered>
            <q-card-section>
              <div class="text-h5">利用規約</div>
            </q-card-section>

            <q-card-section>
              <!-- eslint-disable-next-line vue/no-v-html -->
              <div class="q-pa-md markdown markdown-body" v-html="terms" />
            </q-card-section>
          </q-card>
        </q-page>
      </q-page-container>
    </q-layout>
  </q-dialog>
</template>

<script setup lang="ts">
import { computed, ref, onMounted } from "vue";
import { useStore } from "@/store";
import { useMarkdownIt } from "@/plugins/markdownItPlugin";

const props =
  defineProps<{
    modelValue: boolean;
  }>();
const emit =
  defineEmits<{
    (e: "update:modelValue", value: boolean): void;
  }>();

const store = useStore();

const modelValueComputed = computed({
  get: () => props.modelValue,
  set: (val) => emit("update:modelValue", val),
});

const handler = (acceptTerms: boolean) => {
  store.dispatch("SET_ACCEPT_TERMS", {
    acceptTerms: acceptTerms ? "Accepted" : "Rejected",
  });
  !acceptTerms
    ? store.dispatch("CHECK_EDITED_AND_NOT_SAVE", { closeOrReload: "close" })
    : undefined;

  modelValueComputed.value = false;
};

const md = useMarkdownIt();
const terms = ref("");
onMounted(async () => {
  terms.value = md.render(await store.dispatch("GET_POLICY_TEXT"));
});
</script>

<style scoped lang="scss">
.q-page {
  padding: 3rem;
}
</style>
