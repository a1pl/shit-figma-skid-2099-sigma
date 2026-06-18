<script lang="ts">
  import { DragDropProvider, type DragDropEvents } from '@dnd-kit-svelte/svelte';
  import DraggableBox from '$lib/components/DraggableBox.svelte';
  import DropZone from '$lib/components/DropZone.svelte';

  const BOX = 150;
  const PAD = 16;
  const GAP = 16;
  const EMPTY_H = 48;
  const ZONE_ID = 'zone';

  let layoutEl: HTMLDivElement;
  let zoneEl: HTMLDivElement;
  let zoneHeight = $state(EMPTY_H);

  let nextId = 3;
  let boxes = $state([
    { id: 1, label: 'HEY THERE s9423854yu5e9rt8fdgu9', x: 40, y: 80, inZone: false },
    { id: 2, label: 'div', x: 40, y: 260, inZone: false }
  ]);

  function restack() {
    const zone = zoneEl.getBoundingClientRect();
    const layout = layoutEl.getBoundingClientRect();
    const innerLeft = zone.left - layout.left + PAD;
    const innerTop = zone.top - layout.top + PAD;
    const usableW = zone.width - PAD * 2;
    const cols = Math.max(1, Math.floor((usableW + GAP) / (BOX + GAP)));

    const placed = boxes.filter((b) => b.inZone);
    placed.forEach((b, i) => {
      b.x = innerLeft + (i % cols) * (BOX + GAP);
      b.y = innerTop + Math.floor(i / cols) * (BOX + GAP);
    });

    const rows = Math.ceil(placed.length / cols);
    zoneHeight = placed.length ? PAD * 2 + rows * BOX + (rows - 1) * GAP : EMPTY_H;
  }

  const onDragEnd: DragDropEvents['dragend'] = (event) => {
    if (event.canceled) return;
    const { source, target, transform } = event.operation;
    const b = boxes.find((b) => b.id === source?.id);
    if (!b) return;

    b.x += transform.x;
    b.y += transform.y;

    if (target?.id === ZONE_ID) {
      b.inZone = true;
      restack();
    } else if (b.inZone) {
      b.inZone = false;
      restack();
    }
  };

  function spawn() {
    boxes.push({
      id: nextId++,
      label: 'box ' + (nextId - 1),
      x: 40,
      y: 80 + ((boxes.length * 30) % 400),
      inZone: false
    });
  }
</script>

<DragDropProvider {onDragEnd}>
  <div class="layout" bind:this={layoutEl}>
    <aside class="sidebar">
      <button class="spawn" onclick={spawn}>+ spawn box</button>
    </aside>
    <div class="right">
      <DropZone id={ZONE_ID} height={zoneHeight} bindEl={(el) => (zoneEl = el)} />
      <div class="content"></div>
    </div>

    {#each boxes as box (box.id)}
      <DraggableBox id={box.id} x={box.x} y={box.y}>
        {box.label}
      </DraggableBox>
    {/each}
  </div>
</DragDropProvider>

<style>
  .layout {
    display: flex;
    height: 100vh;
    width: 100vw;
    overflow: hidden;
    position: relative;
  }

  .sidebar {
    width: 240px;
    flex-shrink: 0;
    background: #000;
    padding: 16px;
    box-sizing: border-box;
  }

  .spawn {
    width: 100%;
    padding: 10px;
    background: #d97706;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font: inherit;
    position: relative;
    z-index: 20;
  }

  .right {
    flex: 1;
    display: flex;
    flex-direction: column;
    min-width: 0;
  }

  .content {
    flex: 1;
    position: relative;
  }
</style>
