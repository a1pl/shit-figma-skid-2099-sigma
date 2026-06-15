<script lang="ts">
  import { spring } from 'svelte/motion';

  let x = $state(0);
  let y = $state(0);
  let isDragging = false;

  function draggable(node: HTMLDivElement) {
    let startX: number;
    let startY: number;

    const onMouseDown = (e: MouseEvent) => {
      isDragging = true;
      startX = e.clientX;
      startY = e.clientY;
    };

    const onMouseUp = () => {
      isDragging = false;
    };

    const onMouseMove = (e: MouseEvent) => {
      if (isDragging) {
        x += e.clientX - startX;
        y += e.clientY - startY;
        startX = e.clientX;
        startY = e.clientY;
      }
    };

    node.addEventListener('mousedown', onMouseDown);
    window.addEventListener('mouseup', onMouseUp);
    window.addEventListener('mousemove', onMouseMove);

    return {
      destroy() {
        node.removeEventListener('mousedown', onMouseDown);
        window.removeEventListener('mouseup', onMouseUp);
        window.removeEventListener('mousemove', onMouseMove);
      }
    };
  }

  let xy = spring({ x: 0, y: 0 }, { stiffness: 0.1, damping: 0.25 });
  $effect(() => {
    xy.set({ x, y });
  });

  let { children }: { children?: import('svelte').Snippet } = $props();
</script>

{#snippet defaultContent()}
  X: {x} Y: {y}
{/snippet}

<div use:draggable style:transform={`translate(${$xy.x}px, ${$xy.y}px)`} class="draggable">
  {@render (children ?? defaultContent)()}
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
    top: 100px;
    left: 100px;
  }

  .draggable:active {
    cursor: grabbing;
  }
</style>
