<script>
  import { List, Subheader, ListItem, ListItemGroup,Icon } from 'svelte-materialify'

  export let handleClick
  export let title
  export let items
  export let selected
  export let icon
  export let color

$: iconClass = `mdi mdi-${icon} ${color}`
</script>

<main>
  <List dense disabled class="elevation-2">
    <Subheader>{title}</Subheader>
      <ListItemGroup class="blue-text" value={selected}>
      {#each items as item}
        <ListItem>
          <span slot="prepend">
            <Icon class={iconClass} />
          </span>
          {item}
        </ListItem>
      {/each}
    </ListItemGroup>
  </List>
</main>

<style>
</style>
