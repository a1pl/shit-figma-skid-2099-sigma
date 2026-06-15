<script lang="ts">
  import DraggableBox from '$lib/components/DraggableBox.svelte';
</script>

<div class="layout">
  <aside class="sidebar"></aside>
  <div class="content">
    <DraggableBox />
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
  }

  .content {
    flex: 1;
    position: relative;
  }
</style>
