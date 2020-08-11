<script>
	import { onMount } from "svelte";	
	import { createEventDispatcher } from 'svelte';
	import EditorJS from '@editorjs/editorjs';
	import ImageTool from '@editorjs/image';
	import Header from '@editorjs/header';
	import Table from '@editorjs/table';
	import Embed from '@editorjs/embed';
	import Quote from '@editorjs/quote';
	import Code from '@editorjs/code';
	import List from '@editorjs/list';
	import Delimiter from '@editorjs/delimiter';
	import Checklist from '@editorjs/checklist';
	
	let editor;
	
	export let holder = 'editorjs';
	export let data = {};
	
	export let tools = {
    header: {
      class: Header,
      inlineToolbar : true
    },
		embed: Embed,
		quote: Quote,
		code: Code,
		delimiter: Delimiter,
		table: Table,
		list: List,
		checklist:Checklist,
		image: {
			class: ImageTool,
			config: {
				uploader: {
					uploadByFile(file){
						return _getBase64(file, function(e) {}).then(data => {
							return { success: 1, file: { url: data }}
						})
					}
				}
			}
		}
	}
	
	onMount(() => {
		editor = new EditorJS({
			holder,
			data,
			tools,
			hideToolbar: false,
			onReady: () => {console.log(` Editor.js is ready to work!`)},
   		//onChange: () => {console.log(`Editor's content changed!`)}
		})
	});
	
	function _getBase64(file, onLoadCallback) {
		return new Promise(function(resolve, reject) {
			var reader = new FileReader();
			reader.onload = function() { return resolve(reader.result ); };
			reader.onerror = reject;
			reader.readAsDataURL(file);
		});
	}
	
	let dispatch = createEventDispatcher ();
		
	let onSave = () => {
		editor.save().then(savedData => dispatch('save', savedData));
	}
</script>

<div id="editorjs"></div>

<button on:click={onSave}>Save =</button>