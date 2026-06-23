<script lang="ts">
  import { draggable, droppable, type DragDropState } from '@thisux/sveltednd';

  type Box = { id: number; label: string; container: 'sidebar' | 'zone' };

  let {
    box,
    direction = 'grid',
    onReorder
  }: {
    box: Box;
    direction?: 'vertical' | 'horizontal' | 'grid';
    onReorder: (state: DragDropState<Box>) => void;
  } = $props();
</script>

<div
  use:draggable={{ container: box.container, dragData: box }}
  use:droppable={{ container: String(box.id), direction, callbacks: { onDrop: onReorder } }}
  class="draggable"
>
  {box.label}
</div>

<style>
  .draggable {
    position: relative;
    background: #d97706;
    color: white;
    width: 150px;
    height: 150px;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding: 8px;
    box-sizing: border-box;
    cursor: grab;
    user-select: none;
  }

  .draggable:active {
    cursor: grabbing;
  }

  /* remove edge lines */
  .draggable:global(.drop-before)::before,
  .draggable:global(.drop-after)::after,
  .draggable:global(.drop-left)::before,
  .draggable:global(.drop-right)::after {
    display: none !important;
    content: none !important;
  }
</style>
