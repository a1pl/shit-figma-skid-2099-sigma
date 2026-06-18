<script lang="ts">
  import { useDroppable } from '@dnd-kit-svelte/svelte';

  let {
    id,
    height,
    bindEl
  }: {
    id: string | number;
    height: number;
    bindEl?: (el: HTMLDivElement) => void;
  } = $props();

  const { ref, isDropTarget } = useDroppable({ id: () => id });

  function track(node: HTMLDivElement) {
    bindEl?.(node);
  }
</script>

<div
  {@attach ref}
  {@attach track}
  class="dropzone"
  class:over={isDropTarget.current}
  style:height={height + 'px'}
></div>

<style>
  .dropzone {
    width: 100%;
    flex-shrink: 0;
    background: #88c0d0;
    border: 1px solid #000;
    box-sizing: border-box;
    border-radius: 0;
    transition:
      height 0.15s ease,
      background 0.1s ease;
  }

  .dropzone.over {
    background: #a3d4e0;
  }
</style>
