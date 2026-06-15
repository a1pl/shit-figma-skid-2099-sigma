<script lang="ts">
// github.com/Blankeos/svelte-draggable-example/tree/main (stolen from here btw)
  import { spring } from 'svelte/motion';

  let x = $state(0);
  let y = $state(0);

  let isDragging = false;

  function draggable(node: HTMLDivElement) {
    let startX: number;
    let startY: number;

    node.onmousedown = (e) => {
      isDragging = true;
      startX = e.clientX;
      startY = e.clientY;
    };

    window.onmouseup = () => {
      isDragging = false;
    };

    window.onmousemove = (e) => {
      if (isDragging) {
        x += e.clientX - startX;
        y += e.clientY - startY;
        startX = e.clientX;
        startY = e.clientY;
      }
    };
  }

  let xy = spring({ x: 0, y: 0 }, { stiffness: 0.1, damping: 0.25 });
  $effect(() => {
    xy.set({ x, y });
  });
</script>

<div class="layout">
  <aside class="sidebar"></aside>
  <div class="content">
    <div use:draggable style:transform={`translate(${$xy.x}px, ${$xy.y}px)`} class="draggable">
      X: {x} Y: {y}
    </div>
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
    border-radius: 0;
  }

  .content {
    flex: 1;
    position: relative;
  }

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
