<script lang="ts" setup>
import { ref, onMounted } from "vue";
import type { Ref } from "vue";

type Geolocation = {
    latitude: number;
    longitude: number;
};
const coords: Ref<Geolocation | undefined> = ref();

const geolocationBlockedByUser: Ref<boolean> = ref(false);

const getGeolocation = async (): Promise<void> => {
    await navigator.geolocation.getCurrentPosition(
        async (position: { coords: Geolocation }) => {
            coords.value = position.coords
        },
        (error: { message: string }) => {
            geolocationBlockedByUser.value = true;
            console.error(error.message);
        }
    );
};

onMounted(async () => {
    await getGeolocation();
});
</script>

<template>
    <div>
        <div v-if="coords && !geolocationBlockedByUser">{{ coords.latitude }} {{ coords.longitude }}</div>
        <div v-if="geolocationBlockedByUser">User denied access</div>
    </div>
</template>