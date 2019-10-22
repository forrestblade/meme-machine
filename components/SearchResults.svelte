<script>
  export let results;
  $: results;
</script>

{#each results as result (result.id)}
  <div class="fl pa2 w-50 w-20-ns">
      <a href={result.url} class="db aspect-ratio aspect-ratio--1x1 dim">
        <div style="background-image: url('{result.images.original.url}')" class="bg-center cover aspect-ratio--object">
        </div>
      </a>
  </div>
{/each}