<script context="module">
  import { gameXboxURL } from '../../../static/src/js/utils.js';
  import { gameDetailTemplate } from '../../../static/src/js/templates.js';
  globalThis.dollar = 1;

  export async function load({ params, fetch }) {
    const gameId = params.game.split('_')[1];

    if (!gameId) {
      return {
        status: 301,
        redirect: 'https://xstoregames.com',
      };
    }

    const game = await fetch(gameXboxURL(gameId)).then(res => res.json()).then(game => game[0]);

    const lcp = game.images.titledheroart ?
      (game.images.titledheroart.url || game.images.titledheroart[0].url)
      : game.images.screenshot ? game.images.screenshot[0].url
      : game.images.superheroart.url;

    return {
      props: {
        game,
        lcp: lcp + '?w=1000&q=70',
      }
    };
  };
</script>

<script>
  export let game;
  export let lcp;
</script>

<svelte:head>
	<title>{`${game.title} | XStore`}</title>
  <meta name="description" content={`${game.title}: ${game.description.split('.')[0].replace(/\n/gi, '')}.`} />
  <link rel="preload" as="image" href={lcp} />
</svelte:head>

<div class="detail page page-on">
  <div class="detail-content page-content">
    {@html gameDetailTemplate(game)}
  </div>
</div>
