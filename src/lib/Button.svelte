<script lang="ts">
  export let onClick: () => void | undefined = () => {};
  let ref: HTMLButtonElement;
  export let bounce = false;

  function handleOnClick() {
    onClick();
  }
</script>

<button
  bind:this={ref}
  on:click={handleOnClick}
  class={`
    ${bounce ? 'animate-bounce' : ''}
    bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500
    rounded-lg px-4 py-2 text-lg
  `}
>
  <slot />
</button>
