<script>
  import { createEventDispatcher } from 'svelte';
  import meetups from '../../meetups-store.js';
  import Button from '../UI/Button.svelte';
  import Badge from '../UI/Badge.svelte';
  import LoadingSpinner from '../UI/LoadingSpinner.svelte';

  export let id;
  export let title;
  export let subtitle;
  export let imageUrl;
  export let description;
  export let address;
  export let isFav;

  let isLoading = false;

  const dispatch = createEventDispatcher();

  function toggleFavorite() {
    isLoading = true;
    fetch(`https://svelte-meetup-52a4e.firebaseio.com/meetups/${id}.json`, {
      method: 'PATCH',
      body: JSON.stringify({ isFavorite: !isFav }),
      headers: { 'Content-Type': 'application/json' },
    })
      .then((res) => {
        if (!res.ok) {
          throw new Error('An error occurred, please try again.');
        }
        isLoading = false;
        meetups.toggleFavorite(id);
      })
      .catch((err) => {
        isLoading = false;
        console.log(err);
      });
  }
</script>

<article>
  <header>
    <h1>
      {title}
      {#if isFav}
        <Badge>FAVORITE</Badge>
      {/if}
    </h1>
    <h2>{subtitle}</h2>
    <p>{address}</p>
  </header>
  <div class="image">
    <img src={imageUrl} alt={title} />
  </div>
  <div class="content">
    <p>{description}</p>
  </div>
  <footer>
    <Button href="/{id}">Show Details</Button>
    <Button type="button" mode="outline" on:click={() => dispatch('edit', id)}>
      Edit
    </Button>
    {#if isLoading}
      <span>Working...</span>
    {:else}
      <Button
        mode="outline"
        color={isFav ? null : 'success'}
        type="button"
        on:click={toggleFavorite}>
        {isFav ? 'Unfavorite' : 'Favorite'}
      </Button>
    {/if}
  </footer>
</article>

<style>
  article {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.25);
    border-radius: 5px;
    background: #fff;
    margin: 1rem;
  }

  .content {
    height: 4rem;
  }

  header,
  .content,
  footer {
    padding: 1rem;
  }

  .image {
    width: 100%;
    height: 14rem;
  }

  .image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  h1 {
    font-size: 1.25rem;
    margin: 0.5rem 0;
    font-family: 'Roboto', sans-serif;
  }

  h2 {
    font-size: 1rem;
    color: #808080;
    margin: 0.5rem 0;
  }

  p {
    font-size: 1.25rem;
    margin: 0;
  }

  div {
    text-align: left;
  }
</style>
