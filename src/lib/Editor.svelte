<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import { Editor } from '@tiptap/core';
	import StarterKit from '@tiptap/starter-kit';
	import Heading from '@tiptap/extension-heading';
	import BulletList from '@tiptap/extension-bullet-list';
	import FloatingMenu from '@tiptap/extension-floating-menu';

	let element: HTMLElement;
	let editor: Editor;
	let menu: HTMLElement;

	onMount(() => {
		editor = new Editor({
			element: element,
			extensions: [
				StarterKit,
				Heading.configure({ levels: [1] }),
				FloatingMenu.configure({
					element: menu
				})
			],
			autofocus: true,
			content: '<p>Hello World! 🌍️ </p>',
			editable: true,
			injectCSS: false,
			editorProps: {
				attributes: {
					class: 'prose prose-sm sm:prose-base lg:prose-lg xl:prose-2xl focus:outline-none'
				}
			},
			onTransaction: () => {
				// force re-render so `editor.isActive` works as expected
				editor = editor;
			}
		});
	});

	onDestroy(() => {
		if (editor) {
			editor.destroy();
		}
	});

	const toggleHeading = (level: 1 | 2 | 3) => {
		if (editor) {
			editor.chain().focus().toggleHeading({ level }).run();
		}
	};
</script>

<div bind:this={element} />
<div bind:this={menu} class="flex flex-row gap-2 px-2 rounded bg-stone-300">
	<button on:click={() => toggleHeading(1)}>H1</button>
	<button on:click={() => {}}>To-do List</button>
	<button>Bullet list</button>
</div>
