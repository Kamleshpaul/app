<script setup>
import { ref, nextTick, onMounted, onUnmounted } from "vue";
import { MagnifyingGlassIcon } from "@heroicons/vue/24/outline";
import { useGlobalSearchStore } from "@/store/global-search";

const showInput = ref(false);
const globalSearch = useGlobalSearchStore();
const inputRef = ref(null);

const toggleInputVisibility = async () => {
    showInput.value = !showInput.value;
    if (showInput.value) {
        await nextTick();
        inputRef.value?.focus();
    }
};

const handleKeydown = (event) => {
    if (event.key === "Escape") {
        showInput.value = false;
    }
};

onMounted(() => {
    window.addEventListener("keydown", handleKeydown);
});

onUnmounted(() => {
    window.removeEventListener("keydown", handleKeydown);
});
</script>

<template>
    <div
        :class="{ 'dropdown-open': showInput || globalSearch.search.length > 0 }"
        class="dropdown dropdown-left"
    >
        <button
            tabindex="0"
            :class="{
            '!text-primary bg-base-200': showInput
        }"
            class="px-1.5 py-2 hover:bg-base-200 text-base-content cursor-pointer rounded-md"
            @click="toggleInputVisibility"
        >
            <MagnifyingGlassIcon class="size-4" />
        </button>
        <div
            v-if="showInput"
            tabindex="0"
            class="dropdown-content z-200 menu p-2 bg-base-200 border border-base-content/10 shadow-lg rounded-box w-52 mt-[30px] !right-0"
        >
            <input
                ref="inputRef"
                v-model="globalSearch.search"
                type="text"
                class="grow input-sm rounded-md font-normal font-sans p-2"
                placeholder="Search"
            />
        </div>
    </div>
</template>
