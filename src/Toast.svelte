<script lang="ts">
  const { message } = $props();
</script>

<style>
  .toast {
    position: fixed;
    top: 20%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #61635a;
    color: white;
    padding: 10px 20px;
    border-radius: 5px;
    font-size: 1.2em;
    z-index: 1000;
  }
</style>

{#if message}
  <div class="toast">
    {message}
  </div>
{/if}
