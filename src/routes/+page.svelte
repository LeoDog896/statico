<script lang="ts">

  import Files from '$lib/Files.svelte';
	import type monaco from 'monaco-editor';
	import { onMount } from 'svelte';
	import editorWorker from 'monaco-editor/esm/vs/editor/editor.worker?worker';
	import jsonWorker from 'monaco-editor/esm/vs/language/json/json.worker?worker';
	import cssWorker from 'monaco-editor/esm/vs/language/css/css.worker?worker';
	import htmlWorker from 'monaco-editor/esm/vs/language/html/html.worker?worker';
	import tsWorker from 'monaco-editor/esm/vs/language/typescript/ts.worker?worker';

	let divEl: HTMLDivElement;
	let editor: monaco.editor.IStandaloneCodeEditor;
	let Monaco;

	onMount(async () => {
		// @ts-ignore
		self.MonacoEnvironment = {
			getWorker: function (_moduleId: any, label: string) {
				if (label === 'json') {
					return new jsonWorker();
				}
				if (label === 'css' || label === 'scss' || label === 'less') {
					return new cssWorker();
				}
				if (label === 'html' || label === 'handlebars' || label === 'razor') {
					return new htmlWorker();
				}
				if (label === 'typescript' || label === 'javascript') {
					return new tsWorker();
				}
				return new editorWorker();
			}
		};

		Monaco = await import('monaco-editor');
		editor = Monaco.editor.create(divEl, {
			value: ['<p>Hello World!</p>'].join('\n'),
			language: 'html',
      automaticLayout: true
		});

		return () => {
			editor.dispose();
		};
	});
</script>

<div class="container">
  <Files files={{
    "index.html": "<p>Hello World!</p>"
  }}></Files>
  <div class="editor" bind:this={divEl} />
</div>
<style>
	:global(body) {
		margin: 0;
		padding: 0;
	}

  div.container {
    display: grid;
    grid-template-columns: min-content 1fr;
    width: 100vw;
    height: 100vh;
  }

	div.editor {
    display: block;
    position: relative;
		height: 100vh;
	}
</style>
