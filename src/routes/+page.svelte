<script lang="ts">
	import markdownit from 'markdown-it';
	import highlightjs from 'markdown-it-highlightjs';
	import Toggle from 'svelte-toggle';

	import { Pane, Splitpanes } from 'svelte-splitpanes';
	import { onMount } from 'svelte';

	// https://markdown-it.github.io/markdown-it/#MarkdownIt.new
	const md = markdownit().use(highlightjs);

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

	let liveReload: boolean = $state(true);
	liveReload = false; // on:toggle fires on page load
	// svelte-ignore state_referenced_locally
	let editorContentFreezed = editorContent;
	let theme: string = $state('Light');
</script>

<link rel="stylesheet" href={theme === 'Light' ? '/styles/solarized-light.css' : ''} />

<div class={`container ${theme === 'Dark' && 'container-dark'}`}>
	<nav>
		<div class="nav-item">
			Live reload <Toggle
				hideLabel
				on:toggle={() => {
					liveReload = !liveReload;
					editorContentFreezed = editorContent;
				}}
			></Toggle>
		</div>
		<div class="nav-item">
			Theme
			<select name="theme" id="theme" bind:value={theme}>
				<option value="Light">Light</option>
				<option value="Dark">Dark</option>
			</select>
		</div>
	</nav>

	<Splitpanes>
		<Pane snapSize={10}>
			<textarea
				id="editor"
				bind:value={editorContent}
				class={theme === 'Light' ? '' : 'editor-dark'}
			></textarea>
		</Pane>
		<Pane>
			{#if theme === 'Light'}
				<style>
					.hljs {
						display: block;
						overflow-x: auto;
						padding: 0.5em;
						background: #fdf6e3;
						color: #657b83;
					}
					.hljs-comment,
					.hljs-quote {
						color: #93a1a1;
					}
					.hljs-addition,
					.hljs-keyword,
					.hljs-selector-tag {
						color: #859900;
					}
					.hljs-doctag,
					.hljs-literal,
					.hljs-meta .hljs-meta-string,
					.hljs-number,
					.hljs-regexp,
					.hljs-string {
						color: #2aa198;
					}
					.hljs-name,
					.hljs-section,
					.hljs-selector-class,
					.hljs-selector-id,
					.hljs-title {
						color: #268bd2;
					}
					.hljs-attr,
					.hljs-attribute,
					.hljs-class .hljs-title,
					.hljs-template-variable,
					.hljs-type,
					.hljs-variable {
						color: #b58900;
					}
					.hljs-bullet,
					.hljs-link,
					.hljs-meta,
					.hljs-meta .hljs-keyword,
					.hljs-selector-attr,
					.hljs-selector-pseudo,
					.hljs-subst,
					.hljs-symbol {
						color: #cb4b16;
					}
					.hljs-built_in,
					.hljs-deletion {
						color: #dc322f;
					}
					.hljs-formula {
						background: #eee8d5;
					}
					.hljs-emphasis {
						font-style: italic;
					}
					.hljs-strong {
						font-weight: 700;
					}
				</style>
			{:else}
				<style>
					.hljs {
						display: block;
						overflow-x: auto;
						padding: 0.5em;
						background: #002b36;
						color: #839496;
						-webkit-text-size-adjust: none;
					}
					.hljs-comment,
					.diff .hljs-header,
					.hljs-doctype,
					.hljs-pi,
					.lisp .hljs-string {
						color: #586e75;
					}
					.hljs-keyword,
					.hljs-winutils,
					.method,
					.hljs-addition,
					.css .hljs-tag,
					.hljs-request,
					.hljs-status,
					.nginx .hljs-title {
						color: #859900;
					}
					.hljs-number,
					.hljs-command,
					.hljs-string,
					.hljs-tag .hljs-value,
					.hljs-rule .hljs-value,
					.hljs-doctag,
					.tex .hljs-formula,
					.hljs-regexp,
					.hljs-hexcolor,
					.hljs-link_url {
						color: #2aa198;
					}
					.hljs-title,
					.hljs-localvars,
					.hljs-chunk,
					.hljs-decorator,
					.hljs-built_in,
					.hljs-identifier,
					.vhdl .hljs-literal,
					.hljs-id,
					.css .hljs-function,
					.hljs-name {
						color: #268bd2;
					}
					.hljs-attribute,
					.hljs-variable,
					.lisp .hljs-body,
					.smalltalk .hljs-number,
					.hljs-constant,
					.hljs-class .hljs-title,
					.hljs-parent,
					.hljs-type,
					.hljs-link_reference {
						color: #b58900;
					}
					.hljs-preprocessor,
					.hljs-preprocessor .hljs-keyword,
					.hljs-pragma,
					.hljs-shebang,
					.hljs-symbol,
					.hljs-symbol .hljs-string,
					.diff .hljs-change,
					.hljs-special,
					.hljs-attr_selector,
					.hljs-subst,
					.hljs-cdata,
					.css .hljs-pseudo,
					.hljs-header {
						color: #cb4b16;
					}
					.hljs-deletion,
					.hljs-important {
						color: #dc322f;
					}
					.hljs-link_label {
						color: #6c71c4;
					}
					.tex .hljs-formula {
						background: #073642;
					}
				</style>
			{/if}
			<div class={`preview-container ${theme === 'Dark' && 'preview-dark'}`}>
				{#if liveReload}
					<div id="preview">{@html md.render(editorContent)}</div>
				{:else}
					<div id="preview">{@html md.render(editorContentFreezed)}</div>
				{/if}
			</div>
		</Pane>
	</Splitpanes>
</div>

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

	.preview-dark {
		color: white;
		background-color: #253035;

		:global(a) {
			&:link {
				color: lightblue;
			}

			&:visited {
				color: lightpink;
			}

			&:focus,
			&:active {
				color: lightgreen;
			}
		}
	}

	.container-dark {
		nav {
			color: white;
			background-color: #181818;

			#theme {
				color: white;
				border-color: #505050;
				background-color: #303030;
			}
		}

		:global(.splitpanes__splitter) {
			background-color: #404040 !important;
			border-left-color: #202020 !important;
		}
	}

	.editor-dark {
		background-color: #202020;
		color: white;
	}

	nav {
		background-color: #e8e8e8;
		height: 3rem;
		display: flex;
		justify-content: space-between;
		padding: 0 1em;
	}

	.nav-item {
		font-size: 1.2rem;
		display: flex;
		align-items: center;
		gap: 0 0.5em;
	}

	#theme {
		margin: 0.5em 0;
		border-radius: 0.5rem;
		font-size: 1rem;
		border: 1px solid #c0c0c0;
		padding: 0.375em;
		outline: none;
	}

	.container {
		height: 100%;
	}
</style>
