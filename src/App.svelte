<script lang="ts">
  import { onMount } from 'svelte';
  import Button from './lib/Buttons.svelte';
  import confetti from 'canvas-confetti';
  let question = '';
  let input = '';

  function onGo() {
    console.log(input);
    window.location.href = `/?q=${input || 'Say yes!'}`;
  }

  let celebrating = false;
  function onYes() {
    if (celebrating) return;
    celebrating = true;
    const runConfetti = () => {
      confetti({
        particleCount: 100,
        spread: 70,
        origin: { y: 0.6 }
      });
    };
    runConfetti();
    setInterval(runConfetti, 2000);
  }

  onMount(() => {
    const params = new URLSearchParams(window.location.search);
    const q = params.get('q');
    if (q) question = q;
  });
</script>

<div class="relative flex-1 flex flex-col gap-8 justify-center items-center">
  {#if celebrating}
    <div class="grid grid-cols-2 h-[20rem]">
      <img
        class="h-full w-auto object-cover"
        src="https://i.giphy.com/K3Sbp8fOgKye4.webp"
        alt="lisa dancing"
      />
      <img
        class="h-full w-auto object-cover"
        src="https://i.giphy.com/blSTtZehjAZ8I.webp"
        alt="kid dancing"
      />
    </div>
  {:else if question}
    <h1 class="font-extrabold text-2xl">{question}</h1>
    <div class="grid grid-cols-2 gap-4">
      <Button onClick={onYes}>Yes</Button>
      <Button dance>No</Button>
    </div>
  {:else}
    <div class="flex gap-4">
      <input
        class="px-2 rounded"
        placeholder="Write something..."
        type="text"
        bind:value={input}
      />
      <Button onClick={onGo}>Go</Button>
    </div>
  {/if}
</div>
