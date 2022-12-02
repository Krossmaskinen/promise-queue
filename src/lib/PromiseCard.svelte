<script>
  export let id = 0;
  export let delay = 0;
  export let active = false;

  // let timePassed = delay;
  // let intervalTimer;
  // let intervalMs = 100;

  // $: if (active) {
  //   intervalTimer = setInterval(() => {
  //     timePassed -= intervalMs;

  //     if (timePassed < intervalMs) {
  //       clearInterval(intervalTimer);
  //     }
  //   }, intervalMs);
  // }2
  
</script>

<div>
  <h3>{id}</h3>
  {#if delay}
    <span>Delay: {delay}</span>
    <br/>
  {/if}
  <span>Active: {active}</span>
</div>

<style>
  div {
    background-color: rgb(192, 192, 192);
    color: rgb(46, 46, 46);
    padding: 0.2rem;
    margin: 0.2rem;
  }
</style>