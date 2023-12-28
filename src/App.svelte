<script lang="ts">
  import { onMount } from 'svelte';
  import Button from './lib/Buttons.svelte';
  let question = '';
  let input = '';

  function onGo() {
    window.location.href = `/?q=${input || 'Say yes!'}`;
  }

  onMount(() => {
    const params = new URLSearchParams(window.location.search);
    const q = params.get('q');
    if (q) question = q;
  });
</script>

<div class="relative flex-1 flex flex-col gap-8 justify-center items-center">
  {#if question}
    <h1 class="font-extrabold text-2xl">{question}</h1>
    <div class="grid grid-cols-2 gap-4">
      <Button>Yes</Button>
      <Button dance>No</Button>
    </div>
  {:else}
    <div class="flex gap-4">
      <input
        class="px-2 rounded"
        placeholder="Write something..."
        type="text"
        value={input}
      />
      <Button onClick={onGo}>Go</Button>
    </div>
  {/if}
</div>
