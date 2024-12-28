<script lang="ts">
	import markdownit from 'markdown-it';
	import highlightjs from 'markdown-it-highlightjs';
	import '../styles/solarized-light.css';

	import { Pane, Splitpanes } from 'svelte-splitpanes';

	// https://markdown-it.github.io/markdown-it/#MarkdownIt.new
	const md = markdownit().use(highlightjs);

	let result = $state();
	let editorContent = $state(`# Heading 1

Simple little Markdown editor and previewer.

Made using [Svelte](https://svelte.dev/), [Svelte-Splitpanes](https://github.com/orefalo/svelte-splitpanes), [markdown-it](https://github.com/markdown-it/markdown-it), and [highlight.js](https://highlightjs.org/) for [High Seas](https://highseas.hackclub.com/) as well as for my [FreeCodeCamp Front End Development Libraries Certification](https://www.freecodecamp.org/learn/front-end-development-libraries/). Enjoy!

\`\`\`cpp
int main() {
  std::cout << "Hello, World!\\n";
  return 0;
}
\`\`\`

This is an *emphasis* and **this text is bold and _italic_**.`);
</script>

<Splitpanes style="height: 100%;">
	<Pane snapSize={10}>
		<textarea id="editor" bind:value={editorContent}></textarea>
	</Pane>
	<Pane>
		<div class="preview-container">
			<div id="preview">{@html md.render(editorContent)}</div>
		</div>
	</Pane>
</Splitpanes>

<style lang="scss">
	.editor-container {
		height: 100%;
	}

	#editor {
		border: 0;
		width: calc(100% - 2em);
		height: calc(100% - 2em);
		padding: 1em;
		outline: 0;
		margin: 0;
		resize: none;
		font-size: 0.95rem;
	}

	.preview-container {
		background-color: #f8f8f8;
		height: 100%;
		padding: 1em;
		overflow: auto;
	}
</style>
