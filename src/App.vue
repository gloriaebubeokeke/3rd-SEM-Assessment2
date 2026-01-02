<script setup lang="ts">
import { ref, computed } from "vue";

/* -----------------------------
   Markdown Converter Function
--------------------------------*/
function markdownConverter(markdown: string): string {
  if (!markdown) return "";

  let htmlOutput = markdown;

  htmlOutput = htmlOutput.replace(
    /```([\s\S]*?)```/g,
    "<pre><code>$1</code></pre>"
  );
  htmlOutput = htmlOutput.replace(/`([^`]+)`/g, "<code>$1</code>");
  htmlOutput = htmlOutput.replace(/^### (.*$)/gim, "<h3>$1</h3>");
  htmlOutput = htmlOutput.replace(/^## (.*$)/gim, "<h2>$1</h2>");
  htmlOutput = htmlOutput.replace(/^# (.*$)/gim, "<h1>$1</h1>");
  htmlOutput = htmlOutput.replace(/\*\*(.+?)\*\*/g, "<strong>$1</strong>");
  htmlOutput = htmlOutput.replace(/\*(.+?)\*/g, "<em>$1</em>");
  htmlOutput = htmlOutput.replace(
    /!\[([^\]]*)\]\(([^)]+)\)/g,
    '<img src="$2" alt="$1" />'
  );
  htmlOutput = htmlOutput.replace(
    /\[([^\]]+)\]\(([^)]+)\)/g,
    '<a href="$2" target="_blank">$1</a>'
  );
  htmlOutput = htmlOutput.replace(/\n/g, "<br />");

  return htmlOutput;
}

/* -----------------------------
   State (React useState → Vue ref)
--------------------------------*/
const markdown = ref<string>(`# Welcome to Markdown Preview

## This is a heading

You can write **bold text** and *italic text*.

### Features:
- Type markdown on the left
- See preview on the right
- Simple and easy to use

Try editing the text!`);

/* -----------------------------
   Computed HTML Output
--------------------------------*/
const htmlContent = computed(() => markdownConverter(markdown.value));
</script>

<template>
  <div className="min-h-screen bg-gray-100">
    <header className="bg-white shadow">
      <nav className="max-w-7xl mx-auto px-4 py-4">
        <h1 className="text-2xl font-bold text-gray-900">
          Markdown Preview App
        </h1>
      </nav>
    </header>

    <main className="max-w-7xl mx-auto px-4 py-6">
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <!-- Editor -->
        <section className="bg-white rounded-lg shadow p-4">
          <h2 className="text-lg font-semibold text-gray-900 mb-3">Editor</h2>
          <textarea
            v-model="markdown"
            className="w-full h-96 p-4 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 font-mono text-sm"
            placeholder="Type your markdown here..."
          ></textarea>
        </section>

        <!-- Preview -->
        <section className="bg-white rounded-lg shadow p-4">
          <h2 className="text-lg font-semibold text-gray-900 mb-3">Preview</h2>
          <div
            className="h-96 overflow-y-auto p-4 border border-gray-300 rounded-lg"
            v-html="htmlContent"
          ></div>
        </section>
      </div>
    </main>

    <footer className="max-w-7xl mx-auto px-4 py-6 text-center text-gray-600">
      <p>Built with Vue + TypeScript for Altschool Exam</p>
    </footer>
  </div>
</template>

<style scoped>
h1 {
  font-size: 2em;
  font-weight: bold;
  margin: 0.5em 0;
}

h2 {
  font-size: 1.5em;
  font-weight: bold;
  margin: 0.5em 0;
}

h3 {
  font-size: 1.2em;
  font-weight: bold;
  margin: 0.5em 0;
}

code {
  background: #f3f4f6;
  padding: 2px 6px;
  border-radius: 4px;
  font-family: monospace;
}

pre {
  background: #1f2937;
  color: #e5e7eb;
  padding: 1em;
  border-radius: 8px;
  overflow-x: auto;
  margin: 1em 0;
}

pre code {
  background: transparent;
  padding: 0;
}

strong {
  font-weight: bold;
}

em {
  font-style: italic;
}

a {
  color: #2563eb;
  text-decoration: underline;
}

img {
  max-width: 100%;
  height: auto;
  border-radius: 8px;
  margin: 1em 0;
}
</style>
