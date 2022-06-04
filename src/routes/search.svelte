<script context="module">
  import { searchXboxURL } from '../../static/src/js/utils.js';
  import { gameCardTemplate, emptyList } from '../../static/src/js/templates.js';
  globalThis.dollar = 1;

  export async function load({ fetch, url }) {
    const q = url.searchParams.get('q');
    const games = await fetch(searchXboxURL(q)).then(res => res.json());

    return {
      props: {
        games,
      }
    };
  };
</script>

<script>
  export let games;
</script>

<svelte:head>
	<title>XStore: La tienda argenta de Xbox</title>
  <meta name="description" content="Explora la amplia colección de títulos de videojuegos en el catálogo de juegos de Xbox. Busca los éxitos de taquilla favoritos, las ofertas y nuevos lanzamientos en precios argentinos." />
</svelte:head>

<div class="results list page page-on">
  <div class="results-content page-content">
    {#if games.length !== 0}
      {#each games as game}
        {@html gameCardTemplate(game)}
      {/each}
    {:else}
      {@html emptyList()}
    {/if}
  </div>
</div>
