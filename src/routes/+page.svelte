<script lang="ts">
  import App from '$lib/components/App.svelte'
</script>

<div class="viewport">
  <App />
</div>

<style>
  :global(body) {
    margin: 0;
  }

  div {
    width: 100vw;
    height: 100vh;
    background: white;
  }

  .viewport {
    position: relative;
    width: 100vw;
    height: 100vh;
  }
</style>
