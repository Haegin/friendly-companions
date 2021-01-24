<script>
  import { Button } from 'svelte-materialify'

  export let handleClick
  export let veg
  export let selected
  export let friend
  export let enemy

$: klass = `${enemy ? 'red' : friend ? 'light-green' : 'amber'} ${!selected && enemy ? 'white-text' : ''}`
</script>

<main>
  <Button class={klass} outlined={selected} on:click={handleClick}>{veg}</Button>
</main>

<style>
  main {
    display: inline;
    margin-right: 1rem;
  }
</style>
