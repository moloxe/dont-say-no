<script lang="ts">
  export let onClick: () => void | undefined = () => {};
  let ref: HTMLButtonElement;
  let dancing = false;

  function easeInOut(t: number) {
    t = Math.max(0, Math.min(1, t));
    return 0.5 * (1 - Math.cos(Math.PI * t));
  }

  function handleOnClick() {
    onClick();
    dancing = true;

    var {
      top: initY,
      left: initX,
      width,
      height
    } = ref.getBoundingClientRect();
    const targetX = (window.innerWidth - width) * Math.random();
    const targetY = (window.innerHeight - height) * Math.random();
    const distance = Math.sqrt((targetX - initX) ** 2 + (targetY - initY) ** 2);

    let ini = 0;
    let end = 0;
    const step = (time: number) => {
      if (end === 0) {
        ini = time;
        end = time + distance;
      }
      let progress = (time - ini) / (end - ini);
      progress = easeInOut(progress);
      ref.style.top = `${initY * (1 - progress) + targetY * progress}px`;
      ref.style.left = `${initX * (1 - progress) + targetX * progress}px`;
      if (time < end) window.requestAnimationFrame(step);
    };

    window.requestAnimationFrame(step);
  }
</script>

<button
  bind:this={ref}
  on:click={handleOnClick}
  class={`
    ${dancing ? 'absolute' : ''}
    animate-bounce bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500
    rounded-lg px-4 py-2 text-lg
  `}
>
  <slot />
</button>
