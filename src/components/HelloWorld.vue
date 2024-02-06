<script setup lang="ts">
import { ref } from 'vue';

const simpleText = ref('');
const wildcardText = ref('');

const convert = () => {
    wildcardText.value = '';

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

    const json = JSON.stringify(output);
    animateText(json, 5, 20);
};

const animateText = (text: string, interval: number = 100, chunkSize: number = 5) => {
    let i = 0;
    const timer = setInterval(() => {
        if (i < text.length) {
            const chunk = text.substring(i, i + chunkSize);
            wildcardText.value += chunk;
            i += chunkSize;
        } else {
            clearInterval(timer);
        }
    }, interval);
};

const save = (textData: string, fileName: string) => {
    // テキストデータをBlobとしてダウンロード用に準備
    const blob = new Blob([textData], { type: 'text/plain' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `${fileName}.json`;
    a.click();
    URL.revokeObjectURL(url);
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
            <div>
                <h2>Wildcard JSON</h2>
                <textarea v-model="wildcardText" style="height: 75vh"></textarea>
            </div>
            <button @click="save(wildcardText, 'wildcards')">SAVE JSON</button>
        </div>
    </div>
</template>

<style scoped>
h2 {
    color: azure;
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

button {
    font-family: inherit;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    background-image: linear-gradient(to right, #4facfe, #00f2fe);
    color: azure;
    font-size: 16px;
    cursor: pointer;
    text-transform: uppercase;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
}

button:hover {
    background-image: linear-gradient(to right, #00f2fe, #4facfe);
    box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15);
}

textarea {
    font-family: inherit;
    width: 100%;
    border-radius: 5px;
    background-color: rgba(255, 255, 255, 0.8);
    font-size: 12px;
    color: #333;
    box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
    transition:
        border-color 0.3s ease,
        box-shadow 0.3s ease;
    resize: none;
}

textarea:focus {
    box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.2);
    outline: none;
}

::-webkit-scrollbar {
    width: 5px;
}

::-webkit-scrollbar-track {
    background: #f1f1f1;
    border-radius: 10px;
}

::-webkit-scrollbar-thumb {
    background: linear-gradient(to bottom, rgb(90, 90, 90), gray);
    border-radius: 10px;
}

::-webkit-scrollbar-thumb:hover {
    background: linear-gradient(to bottom, lightgray, rgb(90, 90, 90));
}
</style>
