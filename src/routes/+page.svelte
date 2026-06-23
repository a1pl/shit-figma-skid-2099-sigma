<script lang="ts">
  import { droppable, type DragDropState } from '@thisux/sveltednd';
  import DraggableBox from '$lib/components/DraggableBox.svelte';

  type Container = 'sidebar' | 'zone';
  type Box = { id: number; label: string; container: Container };

  let nextId = 3;
  let boxes = $state<Box[]>([
    { id: 1, label: 'HEY THERE s9423854yu5e9rt8fdgu9', container: 'sidebar' },
    { id: 2, label: 'div', container: 'sidebar' }
  ]);

  // drop on empty
  function moveTo(target: Container) {
    return (state: DragDropState<Box>) => {
      const from = boxes.findIndex((b) => b.id === state.draggedItem.id);
      if (from === -1) return;
      const [moved] = boxes.splice(from, 1);
      moved.container = target;
      boxes.push(moved);
    };
  }

  // reordering for dropping onto a box
  function reorderAt(target: Box) {
    return (state: DragDropState<Box>) => {
      if (state.draggedItem.id === target.id) return;
      const from = boxes.findIndex((b) => b.id === state.draggedItem.id);
      if (from === -1) return;
      const [moved] = boxes.splice(from, 1);
      moved.container = target.container;
      let to = boxes.findIndex((b) => b.id === target.id);
      if (state.dropPosition === 'after') to++;
      boxes.splice(to, 0, moved);
    };
  }

  function spawn() {
    boxes.push({ id: nextId++, label: 'box ' + (nextId - 1), container: 'sidebar' });
  }
</script>

<div class="layout">
  <aside
    class="sidebar"
    use:droppable={{ container: 'sidebar', callbacks: { onDrop: moveTo('sidebar') } }}
  >
    <button class="spawn" onclick={spawn}>+ spawn box</button>
    {#each boxes.filter((b) => b.container === 'sidebar') as box (box.id)}
      <DraggableBox {box} direction="vertical" onReorder={reorderAt(box)} />
    {/each}
  </aside>

  <div class="right">
    <div
      class="page"
      use:droppable={{ container: 'zone', direction: 'grid', callbacks: { onDrop: moveTo('zone') } }}
    >
      {#each boxes.filter((b) => b.container === 'zone') as box (box.id)}
        <DraggableBox {box} direction="grid" onReorder={reorderAt(box)} />
      {/each}
    </div>
    <div class="content"></div>
  </div>
</div>

<style>
  .layout {
    display: flex;
    height: 100vh;
    width: 100vw;
    overflow: hidden;
  }

  .sidebar {
    width: 240px;
    flex-shrink: 0;
    background: #000;
    padding: 16px;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
    gap: 16px;
    overflow-y: auto;
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
  }

  .right {
    flex: 1;
    display: flex;
    flex-direction: column;
    min-width: 0;
  }

  /* page area */
  .page {
    width: 100%;
    flex-shrink: 0;
    min-height: 48px;
    background: #88c0d0;
    border: 1px solid #000;
    box-sizing: border-box;
    display: flex;
    flex-wrap: wrap;
    align-content: flex-start;
    gap: 16px;
    padding: 16px;
  }

  .content {
    flex: 1;
    position: relative;
  }
</style>
