<script setup lang="ts">
import { ref } from 'vue';

const simpleText = ref('');
const wildcardText = ref('');

const convert = () => {
    const output: Record<string, string[]> = {};

    const lines = simpleText.value.split('\n');
    let currentKey = '';
    lines.forEach((line) => {
        if (!line || /^\s*$/gi.test(line)) {
            return;
        } else if (!line.startsWith(':') && line.endsWith(':')) {
            currentKey = line.slice(0, -1);
            output[currentKey] = [];
        } else {
            if (currentKey) {
                output[currentKey].push(line);
            }
        }
    });

    wildcardText.value = JSON.stringify(output);
};
</script>

<template>
    <div class="container">
        <div class="text-container">
            <h2>Simple Editor</h2>
            <textarea v-model="simpleText" style="height: 80vh"></textarea>
        </div>

        <button @click="convert">Convert ⇒</button>

        <div class="text-container">
            <h2>Wildcard JSON</h2>
            <textarea v-model="wildcardText" style="height: 80vh"></textarea>
        </div>
    </div>
</template>

<style scoped>
textarea {
    font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
}
.container {
    display: flex;
    align-items: center;
    justify-content: space-around;
}

.text-container {
    display: flex;
    flex-direction: column;
    width: 40%;
}
</style>
