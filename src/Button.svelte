<script lang="ts">
  const { word, select, selected } = $props();
</script>

<style>
  .word.selected {
    background-color: #61635a;
    color: white;
  }

  .word {
    text-transform: uppercase;
    height: 80px;
    width: 80px;
    cursor: pointer;
    font-weight: 600;
    font-size: 1em;
    background-color: #f2f5e2;
    border-radius: 5px;
    display: flex;
    justify-content: center;
    align-items: center;
    border: none;
  }

  .word:focus {
    outline:none;
    box-shadow: none;
  }

</style>

<button 
  class={["word", { selected }]}
  onclick={() => select(word)}
>
  {word}
</button>

