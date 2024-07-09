<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import { Editor } from '@tiptap/core';
	import StarterKit from '@tiptap/starter-kit';
	import Heading from '@tiptap/extension-heading';
	import BulletList from '@tiptap/extension-bullet-list';
	import FloatingMenu from '@tiptap/extension-floating-menu';
	import ListItem from '@tiptap/extension-list-item';
    import TaskList from '@tiptap/extension-task-list';
    import TaskItem from '@tiptap/extension-task-item';

	let element: HTMLElement;
	let editor: Editor;
	let menu: HTMLElement;

	onMount(() => {
		editor = new Editor({
			element: element,
			extensions: [
				StarterKit,
                ListItem,
                TaskList.configure({
                    HTMLAttributes: {
                        class: 'pl-1 py-2',
                    }
                }),
                TaskItem.configure({
                    nested: true,
                    HTMLAttributes: {
                        class: 'flex items-start gap-2 pb-2',
                    }
                }),
				BulletList.configure({
                    HTMLAttributes: {
                        class: 'list-disc pl-5',
                    }
                }),
				Heading.configure({
					levels: [1],
					HTMLAttributes: {
						class: 'text-2xl font-bold'
					}
				}),
				FloatingMenu.configure({
					element: menu
				})
			],
			autofocus: true,
			content: '<h1 class="text-2xl font-bold">My to-do list </h1>',
			editable: true,
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

    const toggleTaskList = () => {
		if (editor) {
			editor.chain().focus().toggleTaskList().run();
		}
	};

    const toggleBulletList = () => {
		if (editor) {
			editor.chain().focus().toggleBulletList().run();
		}
	};
</script>

<div bind:this={element} />
<div bind:this={menu} class="flex flex-row gap-2 px-2 rounded bg-stone-300">
	<button on:click={() => toggleHeading(1)}>H1</button>
    <button on:click={() => toggleTaskList()}>Task List</button>
	<button on:click={() => toggleBulletList()}>Bullet List</button>
</div>