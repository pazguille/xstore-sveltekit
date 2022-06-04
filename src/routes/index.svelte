<script context="module">
  import { getXboxURL } from '../../static/src/js/utils.js';
  import { sectionTemplate, gamepassSection, goldSection } from '../../static/src/js/templates.js';
  globalThis.dollar = 1;

  export async function load({ fetch }) {
    const sections = [
      {
        type: 'new',
        title: 'Salidos del horno',
        list: [],
      },
      {
        type: 'deals',
        title: 'Ahorrate unos mangos',
        list: [],
      },
      {
        type: 'coming',
        title: '¡Mirá lo que se viene!',
        list: [],
      },
      {
        type: 'best',
        title: 'Deberías jugarlos',
        list: [],
      },
      {
        type: 'most',
        title: 'Los más jugados',
        list: [],
      },
      {
        type: 'free',
        title: 'Gratarola',
        list: [],
      },
    ];

    await Promise.all(sections.map(async ({ type }) => {
      const games = await fetch(getXboxURL(type)).then(res => res.json());
      const section = sections.find(section => section.type === type);
      section.list.push(...games);
    }));

    const preloadLCP = sections[0].list[0];
    const lcp = preloadLCP.images.titledheroart ?
      (preloadLCP.images.titledheroart.url || preloadLCP.images.titledheroart[0].url)
      : preloadLCP.images.screenshot[0].url;

    return {
      props: {
        sections,
        lcp: lcp + '?w=630',
      }
    };
  };
</script>

<script>
  export let sections;
  export let lcp;
</script>

<svelte:head>
	<title>XStore: La tienda argenta de Xbox</title>
  <meta name="description" content="Explora la amplia colección de títulos de videojuegos en el catálogo de juegos de Xbox. Busca los éxitos de taquilla favoritos, las ofertas y nuevos lanzamientos en precios argentinos." />
  <link rel="preload" as="image" href={lcp} />
  <link rel="preload" as="fetch" href="https://www.dolarsi.com/api/api.php?type=valoresprincipales" crossorigin="anonymous" />
  <link rel="preload" as="fetch" href="https://api.xstoregames.com/api/games?list=new&skipitems=0" crossorigin="anonymous" />
  <link rel="preload" as="fetch" href="https://api.xstoregames.com/api/games?list=deals&skipitems=0" crossorigin="anonymous" />
  <link rel="preload" as="fetch" href="https://api.xstoregames.com/api/games?list=coming&skipitems=0" crossorigin="anonymous" />
  <link rel="preload" as="fetch" href="https://api.xstoregames.com/api/games?list=best&skipitems=0" crossorigin="anonymous" />
  <link rel="preload" as="fetch" href="https://api.xstoregames.com/api/games?list=free&skipitems=0" crossorigin="anonymous" />
  <link rel="preload" as="fetch" href="https://api.xstoregames.com/api/games?list=most&skipitems=0" crossorigin="anonymous" />
</svelte:head>

<div class="home">
  {#each sections as section, index}
    {@html sectionTemplate(section)}

    {#if index === 0}
      <notification-prompt hidden></notification-prompt>
    {/if}

    {#if index === 2}
      {@html gamepassSection()}
    {/if}

    {#if index === 4}
      {@html goldSection()}
    {/if}
  {/each}
</div>
