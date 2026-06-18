<script lang="ts">
  import { useDraggable } from '@dnd-kit-svelte/svelte';

  let {
    id,
    x = 0,
    y = 0,
    children
  }: {
    id: string | number;
    x?: number;
    y?: number;
    children?: import('svelte').Snippet;
  } = $props();

  const { ref, isDragging } = useDraggable({ id: () => id });
</script>

<div
  {@attach ref}
  class="draggable"
  class:dragging={isDragging.current}
  style:left={x + 'px'}
  style:top={y + 'px'}
>
  {@render children?.()}
</div>

<style>
  .draggable {
    background: #d97706;
    color: white;
    width: 150px;
    height: 150px;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: grab;
    user-select: none;
    position: absolute;
    z-index: 10;
    touch-action: none;
  }

  .draggable.dragging {
    cursor: grabbing;
    z-index: 100;
  }
</style>
