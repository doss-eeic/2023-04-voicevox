<template>
    <q-dialog v-if="props.isUpdateAvailable" v-model="showUpdateNotificationDialog">
        <q-card>
            <q-card-section class="text-h6">
                新しいアップデートがあります！
            </q-card-section>
            <q-card-section>
                <!-- href の前にコロンつけないとダメ -->
                <q-btn color="primary" :href="props.downloadLink" tag="a" target="_blank" label="ダウンロードページへ"
                    @click="closeUpdateNotificationDialog" />
                <q-btn label="後で" @click="closeUpdateNotificationDialog" />
            </q-card-section>
        </q-card>
    </q-dialog>
</template>

<script setup lang="ts">
import { ref } from "vue";

const props =
    defineProps<{
        downloadLink: string;
        isUpdateAvailable: boolean;
    }>();

const showUpdateNotificationDialog = ref(true);

const closeUpdateNotificationDialog = () => {
    showUpdateNotificationDialog.value = false;
};
</script>
