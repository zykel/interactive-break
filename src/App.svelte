<script>
  import svelteLogo from './assets/svelte.svg'
  import viteLogo from '/vite.svg'
  import Counter from './lib/Counter.svelte'
</script>

<main>

  <div id="other-content-container">

  </div>

  <div id="counter-container">
    <Counter />
  </div>


</main>

<style>
  main {
    position: relative;
  }

  #counter-container {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 2;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  #other-content-container{
    z-index: 1;
    position: absolute;
    /* background-color: aquamarine; */
    width: 100vw;
    height: 100vh;
    overflow: hidden;
  }
</style>
