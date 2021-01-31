<script>
  import { uniq, intersection } from "ramda";
  import { Alert, Row, Col } from "svelte-materialify";

  import Button from "./Button.svelte";
  import List from "./List.svelte";
  import data from "./data.json";

  const vegetables = Object.keys(data);

  let selected = [];

  const handleClick = (vegetable) => {
    if (selected.includes(vegetable)) {
      selected = selected.filter((veg) => veg !== vegetable);
    } else {
      selected = [...selected, vegetable];
    }
  };

  $: enemies = uniq(
    selected.reduce((enemies, veg) => {
      return [...enemies, ...data[veg].enemies];
    }, [])
  );

  $: allFriends = uniq(
    selected.reduce((friends, veg) => {
      return [...friends, ...data[veg].friends];
    }, [])
  );
  $: friends = allFriends.filter((friend) => !enemies.includes(friend));
</script>

<main>
  <h1>Friendly Companions</h1>
  {#each vegetables as veg}
    <Button
      enemy={enemies.includes(veg)}
      friend={friends.includes(veg)}
      selected={selected.includes(veg)}
      handleClick={() => handleClick(veg)}
      {veg}
    />
  {/each}
  <Col>
    {#if selected.length > 0}
      {#if intersection(enemies, selected).length > 0}
        <Alert class="red-text" border="top" coloredBorder>
          Some of the plants you've selected don't play nicely together.
        </Alert>
      {/if}
      <Row>
        <Col>
          <List
            title="Friends"
            items={friends}
            color="green"
            icon="heart"
            {selected}
            {handleClick}
          />
        </Col>
        <Col>
          <List
            title="Enemies"
            items={enemies}
            color="red"
            icon="skull"
            {selected}
            {handleClick}
          />
        </Col>
      </Row>
    {:else}
      <Alert class="primary-text" text>
        Pick some plants and get planting!
      </Alert>
    {/if}
  </Col>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 640px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 3rem;
    font-weight: 100;
  }

  h2 {
    font-size: 3rem;
  }

  @media (max-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
