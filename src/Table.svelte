<script>
  import {onMount} from 'svelte';

  export let data;
  export let headings;

  // Support for drag and drop of table rows was heavily inspired by
  // https://htmldom.dev/drag-and-drop-table-row/

  let draggingEl;
  let draggingRowIndex;
  let isDraggingStarted = false;
  let list;
  let placeholder;
  let table;
  let currentElementIndex;
  let aboveElementIndex;
  let belowElementIndex;

  // holds current position of mouse relative to dragging element
  let x = 0;
  let y = 0;

  function cloneTable() {
    const rect = table.getBoundingClientRect();
    const width = parseInt(window.getComputedStyle(table).width);

    list = document.createElement('div');
    list.classList.add('clone-list');
    list.style.position = 'absolute';
    list.style.left = `${rect.left}px`;
    list.style.top = `${rect.top}px`;
    table.parentNode.insertBefore(list, table);

    // Hide the original table.
    table.style.visibility = 'hidden';

    for (const row of table.querySelectorAll('tr')) {
      // Create a new table from given row.
      const item = document.createElement('div');
      item.classList.add('draggable');

      const newTable = document.createElement('table');
      newTable.setAttribute('class', 'clone-table');
      newTable.style.width = `${width}px`;

      const newRow = document.createElement('tr');
      for (const cell of row.children) {
        const newCell = cell.cloneNode(true);
        newCell.style.width = `${parseInt(
          window.getComputedStyle(cell).width
        )}px`;
        newRow.appendChild(newCell);
      }

      newTable.appendChild(newRow);
      item.appendChild(newTable);
      list.appendChild(item);	  
    }
  }

  function getStyle(heading) {
    const {width} = heading;
    return width ? `width: ${width}px` : '';
  }

  function isAbove(nodeA, nodeB) {
    // Get the bounding rectangle of nodes.
    const rectA = nodeA.getBoundingClientRect();
    const rectB = nodeB.getBoundingClientRect();

    return rectA.top + rectA.height / 2 < rectB.top + rectB.height / 2;
  }

  function mouseDownHandler(e){
    // Get table row containing target.
    const target = e.target;
    let originalRow = target.parentNode;
    while (originalRow.nodeName !== 'TR') {
      originalRow = originalRow.parentNode;
    }

    const trs = Array.from(table.querySelectorAll('tr'));
    draggingRowIndex = trs.indexOf(originalRow);

    // Determine mouse position.
    x = e.clientX;
    y = e.clientY;

    // Attach the listeners to document.
    document.addEventListener('mousemove', mouseMoveHandler);
    document.addEventListener('mouseup', mouseUpHandler);
  }

  function mouseMoveHandler(e) {	
    if (!isDraggingStarted) {
      isDraggingStarted = true;

      cloneTable();

      draggingEl = list.children.item(draggingRowIndex);
      draggingEl.classList.add('dragging');

      placeholder = document.createElement('div');
      placeholder.classList.add('placeholder');
      draggingEl.parentNode.insertBefore(placeholder, draggingEl.nextSibling);
    }

    // Set position for dragging element.
    draggingEl.style.position = 'absolute';
    draggingEl.style.top = `${draggingEl.offsetTop + e.clientY - y}px`;
    draggingEl.style.left = `${draggingEl.offsetLeft + e.clientX - x}px`;

    // Reassign position of mouse.
    x = e.clientX;
    y = e.clientY;

    // The current order is prevEle, draggingEl, placeholder, nextEle.
    const prevEle = draggingEl.previousElementSibling;
    const nextEle = placeholder.nextElementSibling;

	// console.log("Previous Element");
	// console.log(prevEle);
	// console.log("Next Element");
	// console.log(nextEle);

    // If the dragging element is above the previous element
    // and the user moves the dragging element to the top,
    // don't allow to drop above the header
    // (which doesn't have `previousElementSibling`).
    if (
      prevEle &&
      prevEle.previousElementSibling &&
      isAbove(draggingEl, prevEle)
    ) {
      // current order -> new order
      // prevEle       -> placeholder
      // draggingEl    -> draggingEl
      // placeholder   -> prevEle
      swapElements(placeholder, draggingEl);
      swapElements(placeholder, prevEle);
      return;
    }

    // If the dragging element is below the next element
    // and the ser moves the dragging element to the bottom ...
    if (nextEle && isAbove(nextEle, draggingEl)) {
      // current order -> new order
      // draggingEl    -> nextEle
      // placeholder   -> placeholder
      // nextEle       -> draggingEl
      swapElements(nextEle, placeholder);
      swapElements(nextEle, draggingEl);
    }
  }

  function mouseUpHandler() {
    if (!placeholder) return; // not dragging

    // Remove placeholder.
    if (placeholder) placeholder.parentNode.removeChild(placeholder);

    draggingEl.classList.remove('dragging');
    draggingEl.style.removeProperty('top');
    draggingEl.style.removeProperty('left');
    draggingEl.style.removeProperty('position');

    const endRowIndex = Array.from(list.children).indexOf(draggingEl);

	// console.log("endRowIndex");
	// console.log(endRowIndex);
	// console.log("draggingRowIndex");
	// console.log(draggingRowIndex);

    isDraggingStarted = false;

    // Remove list element.
    list.parentNode.removeChild(list);
	


    // Move dragged row to endRowIndex.
    const rows = Array.from(table.querySelectorAll('tr'));
	console.log("currentElement");
	console.log( rows[endRowIndex]);

	console.log("aboveElement");
	console.log(rows[endRowIndex - 1]);

	console.log("belowElement");
	console.log(rows[endRowIndex + 1]);
	document.body.contains(rows[endRowIndex]);
	currentElementIndex = (document.body.contains(rows[endRowIndex]))? rows[endRowIndex].getAttribute("state-index") : null; 
	aboveElementIndex = (document.body.contains(rows[endRowIndex].previousSibling))? rows[endRowIndex].previousSibling.getAttribute("state-index") : null;
	belowElementIndex = (document.body.contains(rows[endRowIndex].nextSibling))? rows[endRowIndex].nextSibling.getAttribute("state-index"): null;

	// console.log("currentElementIndex");
	// console.log(currentElementIndex);

	// console.log("aboveElementIndex");
	// console.log(aboveElementIndex);

	// console.log("belowElementIndex");
	// console.log(belowElementIndex);

	if(draggingRowIndex > endRowIndex){		
		rows[endRowIndex].parentNode.insertBefore(
          rows[draggingRowIndex],
          rows[endRowIndex]
        )
	}else{
		rows[endRowIndex].parentNode.insertBefore(
          rows[draggingRowIndex],
          rows[endRowIndex].nextSibling
        )
	}
   
    // Bring back the table.
    table.style.removeProperty('visibility');

    // Remove handlers of mousemove and mouseup.
    document.removeEventListener('mousemove', mouseMoveHandler);
    document.removeEventListener('mouseup', mouseUpHandler);
  }

  function swapElements(elementA, elementB) {
    const siblingA =
      elementA.nextSibling === elementB ? elementA : elementA.nextSibling;

    // Move elementA to before the elementB.
    elementB.parentNode.insertBefore(elementA, elementB);

    // Move elementB to before the sibling of elementA.
    elementA.parentNode.insertBefore(elementB, siblingA);
  }

  onMount(() => {
    table.querySelectorAll('tr').forEach((row, index) => {
      // Ignore the header so the user cannot move it.
      if (index === 0) return;

      // Allow dragging only by first cell of each row.
      const firstCell = row.firstElementChild;
      firstCell.classList.add('draggable');
      firstCell.addEventListener('mousedown', mouseDownHandler);
    });
  });

  function getRowLexoIndex(){
	  aboveElement = "";
	  belowElement = "";
	  currentElement = "between above and below";
  }

</script>

<section>
  <table bind:this={table}>
    <thead>
      <tr>
        <th>Drag</th>
        {#each headings as heading}
          <th style={getStyle(heading)}>{heading.title}</th>
        {/each}        
      </tr>
    </thead>
    <tbody>
      {#each data as obj, index}
        <tr state-index={index}>
          <td class="drag">☰</td>
          {#each headings as heading}
            <td>
				{data[index][heading.property]}
            </td>
          {/each}
        </tr>
      {/each}
    </tbody>
  </table>
</section>

<style>
  .drag {
    text-align: center;
  }

  section {
    --row-height: 52px;
  }

  section :global(.clone-table),
  section :global(table) {
    border-collapse: collapse;
  }

  section :global(.draggable) {
    cursor: ns-resize;
    user-select: none;
  }

  section :global(.dragging) {
    background: white;
    z-index: 999; /* probably don't need this */
  }

  section :global(.placeholder) {
    --border-width: 2px;
    --fudge: 3px;
    background-color: #edf2f7;
    border: var(--border-width) dashed #cbd5e0;
    height: calc(var(--row-height) - 2 * var(--border-width) - var(--fudge));
  }

  section :global(td),
  section :global(th) {
    border: 1px solid lightgray;
    padding: 0.5rem;
  }

  section :global(.clone-list td) {
    border-top-width: 0;
  }

  section :global(.dragging td) {
    border-top-width: 1px;
  }

  section :global(tr) {
    height: var(--row-height);
  }

</style>
