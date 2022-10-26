<script lang="ts">

  type Files = { [key: string]: string | Files }

  export let files: Files = { }
</script>

<div>
  {#each Object.entries(files) as file}
    {#if typeof file[1] === "string"}
      <p>{file[0]}</p>
    {:else}
      <svelte:self files={file[1]}></svelte:self>
    {/if}
  {/each}
</div>

<style>
  div {
    border-right: 1px solid;
    padding-right: 10px;
    padding-left: 10px;
  }
</style>