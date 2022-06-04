<script context="module">
  import { getXboxNewsURL } from '../../static/src/js/utils.js';
  import { newsTemplate } from '../../static/src/js/templates.js';

  export async function load({ fetch }) {
    const news = await fetch(getXboxNewsURL()).then(res => res.json());
    return {
      props: {
        news,
      }
    };
  };
</script>

<script>
  export let news;
</script>

<svelte:head>
	<title>XStore: La tienda argenta de Xbox</title>
  <meta name="description" content="Explora la amplia colección de títulos de videojuegos en el catálogo de juegos de Xbox. Busca los éxitos de taquilla favoritos, las ofertas y nuevos lanzamientos en precios argentinos." />
</svelte:head>

<div class="news page page-fixed page-on">
  <div class="news-content page-content">
    <h2>Noticias recientes</h2>
    {#each news as n}
      {@html newsTemplate(n)}
    {/each}
  </div>
</div>
