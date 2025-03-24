<script setup>
// filepath: /Users/geekmister/Desktop/Workspace/Repositories/Geekmister/AINavigation/AINavigation.git/src/components/MarkdownViewer.vue

import { ref, onMounted } from 'vue';
import MarkdownIt from 'markdown-it';

const markdownContent = ref('');
const md = new MarkdownIt();

// 加载指定的 Markdown 文件
const loadMarkdown = async (fileName) => {
  try {
    const response = await fetch(`/docs/${fileName}`);
    if (!response.ok) {
      throw new Error(`Failed to fetch ${fileName}`);
    }
    const text = await response.text();
    markdownContent.value = md.render(text);
  } catch (error) {
    console.error('Error loading markdown file:', error);
    markdownContent.value = '<p>Error loading markdown content.</p>';
  }
};

// 默认加载 README.md
onMounted(() => {
  loadMarkdown('README.md');
});
</script>

<template>
  <div>
    <h1>Markdown Viewer</h1>
    <div v-html="markdownContent" class="markdown-content"></div>
  </div>
</template>

<style scoped>
.markdown-content {
  font-family: Arial, sans-serif;
  line-height: 1.6;
  padding: 1rem;
  background-color: #f9f9f9;
  border: 1px solid #ddd;
  border-radius: 4px;
}
</style>