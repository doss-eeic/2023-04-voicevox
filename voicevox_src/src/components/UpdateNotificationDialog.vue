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
    <q-card class="q-py-none q-px-md" style="height: 40%">
      <q-card-section>
        <div class="scrollable-area">
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
        </div>
      </q-card-section>
      <div class="button-area">
        <q-btn
          padding="xs md"
          label="閉じる"
          unelevated
          color="surface"
          text-color="display"
          class="q-mt-sm"
          @click="closeUpdateInfoDialog()"
        />
      </div>
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

<style scoped lang="scss">
@use '@/styles/colors' as colors;

.scrollable-area {
  overflow-y: auto;
}
.scrollable-area h5 {
  margin: 10px 0;
}
.scrollable-area h6 {
  margin: 15px 0;
}
.button-area {
  position: sticky;
  height: 50px; /* ボタン領域の高さを指定 */
  background-color: colors.$background; /* ボタン領域の背景色を設定（必要に応じて調整） */
  border-top: 1px solid colors.$splitter; /* ボタン領域の上部に線を引く */
  padding-bottom: 10px;
  bottom: 0px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
