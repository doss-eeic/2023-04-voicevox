<template>
  <q-dialog v-if="!isUpdateAvailable" v-model="showDialog">
    <q-card class="q-py-sm q-px-md">
      <q-card-section align="center">
        <div class="text-h6">
          <q-icon name="info" color="primary" />アップデート通知
        </div>
        <p>
          新しいバージョンが利用可能です。<br />
          公式サイトから最新版をダウンロードできます。
        </p>
      </q-card-section>
      <q-card-actions align="center">
        <q-btn
          padding="xs md"
          label="キャンセル"
          unelevated
          color="surface"
          text-color="display"
          class="q-mt-sm"
          @click="closeDialog()"
        />
        <q-btn
          padding="xs md"
          label="詳細"
          unelevated
          color="primary"
          text-color="display-on-primary"
          class="q-mt-sm"
          @click="openUpdateInfoDialog()"
        />
        <q-btn
          padding="xs md"
          label="公式サイトを開く"
          unelevated
          color="primary"
          text-color="display-on-primary"
          class="q-mt-sm"
          @click="
            openOfficialWebsite();
            closeDialog();
          "
        />
      </q-card-actions>
    </q-card>
  </q-dialog>

  <q-dialog
    no-esc-dismiss
    no-shake
    transition-show="none"
    transition-hide="none"
    :model-value="showUpdateInfoDialog"
    @update:model-value="closeUpdateInfoDialog"
  >
    <q-card class="q-py-sm q-px-md">
      <h5>アップデート履歴</h5>
      <template v-for="(info, infoIndex) of updateInfos" :key="infoIndex">
        <div v-if="semver.lt(DEBUGcurrentVersion, info.version)">
          <h6>バージョン {{ info.version }}</h6>
          <ul>
            <template
              v-for="(item, descriptionIndex) of info.descriptions"
              :key="descriptionIndex"
            >
              <li>{{ item }}</li>
            </template>
          </ul>
        </div>
      </template>
      <q-card-actions align="center">
        <q-btn
          padding="xs md"
          label="閉じる"
          unelevated
          color="surface"
          text-color="display"
          class="q-mt-sm"
          @click="closeUpdateInfoDialog()"
        />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script setup lang="ts">
import { computed, ref } from "vue";
import semver from "semver";
import { UpdateInfo as UpdateInfoObject } from "@/type/preload";
import { useStore } from "@/store";
import { useFetchLatestVersion } from "@/composables/useFetchLatestVersion";

const store = useStore();

const updateInfos = ref<UpdateInfoObject[]>();
store.dispatch("GET_UPDATE_INFOS").then((obj) => (updateInfos.value = obj));

const { isCheckingFinished, currentVersion, latestVersion } =
  useFetchLatestVersion();
const DEBUGcurrentVersion = ref("0.13.0");
const isUpdateAvailable = computed(() => {
  return isCheckingFinished.value && latestVersion.value !== "";
});

const showDialog = ref<boolean>(true);

const closeDialog = () => {
  showDialog.value = false;
};

const showUpdateInfoDialog = ref<boolean>(false);

const openUpdateInfoDialog = () => {
  showUpdateInfoDialog.value = true;
};
const closeUpdateInfoDialog = () => {
  showUpdateInfoDialog.value = false;
};

const openOfficialWebsite = () => {
  window.open("https://voicevox.hiroshiba.jp/", "_blank");
};
</script>
