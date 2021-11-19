<style>
	section {
		width: 12em;
		padding: 1em;
	}
	table{		
		 border-collapse: collapse;
		 background:lightgray;
	}
	.tablesome__row {
		position: relative;
		height: 1.5em;
		width: 10em;
		text-align: center;
		border: 1px solid black;
		margin: 0.2em;
		padding: 0.3em;
	}
	td, th{
		border: 1px solid blue;
	}
	
	.custom-shadow-item {
		position: absolute;
		top: 0; left:0; right: 0; bottom: 0;
		visibility: visible;
		background-color: #edf2f7;
    border: 2px dashed #cbd5e0;
		margin: 0;
	}
	
</style>
<script>
	import {dndzone, SOURCES, TRIGGERS, SHADOW_ITEM_MARKER_PROPERTY_NAME} from 'svelte-dnd-action';
	import {fade} from 'svelte/transition';
	import {flip} from 'svelte/animate';
	import {cubicIn} from 'svelte/easing';	
	const flipDurationMs = 200;
	
		
	let columns = [
		{ name: "NAME", format: "text", id: "2" },
		{ name: "POSITION", format: "text", id: "3" },
		{ name: "LOCATION", format: "text", id: "4" },
		{ name: "AGE", format: "number", id: "5" },
		{ name: "START DATE", format: "date", id: "6" },
		{ name: "SALARY", format: "number", id: "7" },
		{ name: "PROFILE PAGE LINK", format: "url", id: "8" },
	];

	let rows = [
		{
			content: [
				{
					type: "text",
					html: "Zorita Serrano",
					value: "Zorita Serrano",
				},
				{
					type: "text",
					html: "Software Engineer",
					value: "Software Engineer",
				},
				{ type: "text", html: "San Francisco", value: "San Francisco" },
				{ type: "number", html: "56", value: "56" },
				{
					type: "date",
					html: "26, September  2008",
					value: 1222387190000,
				},
				{ type: "number", html: "645750", value: "645750" },
				{
					type: "url",
					html: '<a class="tablesome__url" href="//company.com/zori…o" target="_blank">company.com/zorita-serrano</a>',
					value: "company.com/zorita-serrano",
					linkText: "",
				},
				,
			],
			rank_order: "0|100000:",
			record_id: 4028,
			stateRecordID: 1,
			id: 1,
		},
		{
			content: [
				{ type: "text", html: "Zenaida Frank", value: "Zenaida Frank" },
				{
					type: "text",
					html: "Software Engineer",
					value: "Software Engineer",
				},
				{ type: "text", html: "New York", value: "New York" },
				{ type: "number", html: "63", value: "63" },
				{
					type: "date",
					html: "16, October  2008",
					value: 1224115190000,
				},
				{ type: "number", html: "470600", value: "470600" },
				{
					type: "url",
					html: '<a class="tablesome__url" href="//company.com/zena…nk" target="_blank">company.com/zenaida-frank</a>',
					value: "company.com/zenaida-frank",
					linkText: "",
				},
				,
			],
			rank_order: "0|100008:",
			record_id: 4029,
			stateRecordID: 2,
			id: 2,
		},
		{
			content: [
				{ type: "text", html: "Yuri Berry", value: "Yuri Berry" },
				{
					type: "text",
					html: "Chief Marketing Officer",
					value: "Chief Marketing Officer",
				},
				{ type: "text", html: "New York", value: "New York" },
				{ type: "number", html: "40", value: "40" },
				{
					type: "date",
					html: "26, October  2008",
					value: 1224979190000,
				},
				{ type: "number", html: "470600", value: "470600" },
				{
					type: "url",
					html: '<a class="tablesome__url" href="https://company.co…berry" target="_blank">company.com/yuri-berry</a>',
					value: "https://company.com/yuri-berry",
					linkText: "",
				},
				,
			],
			rank_order: "0|10000g:",
			record_id: 4030,
			stateRecordID: 3,
			id: 3,
		},
		{
			content: [
				{
					type: "text",
					html: "Vivian Harrell",
					value: "Vivian Harrell",
				},
				{
					type: "text",
					html: "Marketing Designer",
					value: "Marketing Designer",
				},
				{ type: "text", html: "London", value: "London" },
				{ type: "number", html: "43", value: "43" },
				{
					type: "date",
					html: "13, November  2008",
					value: 1226534390000,
				},
				{ type: "number", html: "470600", value: "470600" },
				{
					type: "url",
					html: '<a class="tablesome__url" href="https://www.compan…l" target="_blank">company.com/vivian-harrell</a>',
					value: "https://www.company.com/vivian-harrell",
					linkText: "",
				},
				,
			],
			rank_order: "0|10000o:",
			record_id: 4031,
			stateRecordID: 4,
			id: 4,
		},
		{
			content: [
				{ type: "text", html: "Unity Butler", value: "Unity Butler" },
				{
					type: "text",
					html: "Office Manager",
					value: "Office Manager",
				},
				{ type: "text", html: "Edinburgh", value: "Edinburgh" },
				{ type: "number", html: "37", value: "37" },
				{
					type: "date",
					html: "13, November  2008",
					value: 1226534390000,
				},
				{ type: "number", html: "470600", value: "470600" },
				{
					type: "url",
					html: '<a class="tablesome__url" href="https://www.compan…ler" target="_blank">company.com/unity-butler</a>',
					value: "https://www.company.com/unity-butler",
					linkText: "",
				},
				,
			],
			rank_order: "0|10000w:",
			record_id: 4032,
			stateRecordID: 5,
			id: 5,
		},
		{
			content: [
				{
					type: "text",
					html: "Howard Hatfield",
					value: "Howard Hatfield",
				},
				{
					type: "text",
					html: "Software Engineer",
					value: "Software Engineer",
				},
				{ type: "text", html: "San Francisco", value: "San Francisco" },
				{ type: "number", html: "34", value: "34" },
				{
					type: "date",
					html: "11, December  2008",
					value: 1228953590000,
				},
				{ type: "number", html: "470600", value: "470600" },
				{
					type: "url",
					html: '<a class="tablesome__url" href="//company.com/howa…" target="_blank">company.com/howard-hatfield</a>',
					value: "company.com/howard-hatfield",
					linkText: "",
				},
				,
			],
			rank_order: "0|100014:",
			record_id: 4033,
			stateRecordID: 6,
			id: 6,
		},
		{
			content: [
				{ type: "text", html: "Jena Gaines", value: "Jena Gaines" },
				{
					type: "text",
					html: "Office Manager",
					value: "Office Manager",
				},
				{ type: "text", html: "Tokyo", value: "Tokyo" },
				{ type: "number", html: "53", value: "53" },
				{
					type: "date",
					html: "11, December  2008",
					value: 1228953590000,
				},
				{ type: "number", html: "470600", value: "470600" },
				{
					type: "url",
					html: '<a class="tablesome__url" href="https://www.compan…ines" target="_blank">company.com/jena-gaines</a>',
					value: "https://www.company.com/jena-gaines",
					linkText: "",
				},
				,
			],
			rank_order: "0|10001c:",
			record_id: 4034,
			stateRecordID: 7,
			id: 7,
		},
		{
			content: [
				{ type: "text", html: "Ashton Cox", value: "Ashton Cox" },
				{
					type: "text",
					html: "Junior Technical Author",
					value: "Junior Technical Author",
				},
				{ type: "text", html: "San Francisco", value: "San Francisco" },
				{ type: "number", html: "27", value: "27" },
				{
					type: "date",
					html: "19, December  2008",
					value: 1229644790000,
				},
				{ type: "number", html: "470600", value: "470600" },
				{
					type: "url",
					html: '<a class="tablesome__url" href="https://www.compan…n-cox" target="_blank">company.com/ashton-cox</a>',
					value: "https://www.company.com/ashton-cox",
					linkText: "",
				},
				,
			],
			rank_order: "0|10001k:",
			record_id: 4035,
			stateRecordID: 8,
			id: 8,
		},
		{
			content: [
				{ type: "text", html: "Sonya Frost", value: "Sonya Frost" },
				{
					type: "text",
					html: "Regional Director",
					value: "Regional Director",
				},
				{
					type: "text",
					html: "Regional Director",
					value: "Regional Director",
				},
				{ type: "number", html: "47", value: "47" },
				{
					type: "date",
					html: "13, December  2008",
					value: 1229126390000,
				},
				{ type: "number", html: "86000", value: "86000" },
				{
					type: "url",
					html: '<a class="tablesome__url" href="https://www.notion…notion.so/pauple/0005dcecf63d4973a08e&hellip;</a>',
					value: "https://www.notion.so/pauple/0005dcecf63d4973a08eed221293c591?v=bfa87ba971d64ebabe583430b1f8007e",
					linkText: "",
				},
				,
			],
			rank_order: "0|10001s",
			stateRecordID: 9,
			id: 9,
		},
	];
	
	function handleSort(e) {
		items = e.detail.rows;
	}
	
	let dragDisabled = true;
	let dropTargetStyle;
	
	function handleConsider(e) {
		const {items: newItems, info: {source, trigger}} = e.detail;
		rows = newItems;
		
		// Ensure dragging is stopped on drag finish via keyboard
		if (source === SOURCES.KEYBOARD && trigger === TRIGGERS.DRAG_STOPPED) {
			dragDisabled = true;
		}
	}
	
	function handleFinalize(e) {
		const {items: newItems, info: {source}} = e.detail;
		rows = newItems;
		// Ensure dragging is stopped on drag finish via pointer (mouse, touch)
		if (source === SOURCES.POINTER) {
			dragDisabled = true;
		}
	}
	
	function startDrag(e) {
		// preventing default to prevent lag on touch devices (because of the browser checking for screen scrolling)
		e.preventDefault();
		dragDisabled = false;
	}
	
	function handleKeyDown(e) {
		if ((e.key === "Enter" || e.key === " ") && dragDisabled) dragDisabled = false;
	}
	
	function transformDraggedElement(draggedEl, data, index) {				
		draggedEl.style.backgroundColor = "white";
	}
</script>
<section>
	<table>
    <thead>
      <tr>
        <th>Drag</th>
       {#each columns as column, ii}
				<th
					class="tablesome__column"
					data-column-id={column["id"]}
					data-column-format={column["format"]}
				>
					{column["name"]}
				</th>
				{/each}
      </tr>
    </thead>
    <tbody use:dndzone="{{ items: rows, dragDisabled, flipDurationMs, transformDraggedElement }}"
	on:consider="{handleConsider}"
	on:finalize="{handleFinalize}"
	>
      {#each rows as row(row.id)}
        <tr
						class="tablesome__row"		
						animate:flip={{duration:flipDurationMs}}
					>
          <td 
					 tabindex={dragDisabled? 0 : -1} 
					 aria-label="drag-handle"
					 class="handle" 
					 style={dragDisabled ? 'cursor: grab' : 'cursor: grabbing'}
					 on:mousedown={startDrag} 
					 on:touchstart={startDrag}
					 on:keydown={handleKeyDown}
					>☰</td>         
					{#each row["content"] as cell, cellIndex}
					<td class="tablesome__cell">
						{cell["value"]}
					</td>
					{/each}
					{#if row[SHADOW_ITEM_MARKER_PROPERTY_NAME]}
					<div in:fade={{duration:200, easing: cubicIn}} class='custom-shadow-item'></div>
				{/if}
        </tr>
				
      {/each}
    </tbody>
  </table>

</section>
