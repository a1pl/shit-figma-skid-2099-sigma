<script lang="ts">
  import DraggableBox from '$lib/components/DraggableBox.svelte';
</script>

<div class="layout">
  <aside class="sidebar"></aside>
  <div class="content">
    <DraggableBox> h1</DraggableBox>
    <DraggableBox> div</DraggableBox>
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
    z-index: 67;
    position: relative;
  }

  .content {
    flex: 1;
    position: relative;
  }
</style>
