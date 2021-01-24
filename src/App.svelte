<script>
  import { uniq, intersection } from 'ramda'
  import { Alert, Row, Col } from 'svelte-materialify'
  import Button from './Button.svelte'
  import data from './data.json'

  const vegetables = Object.keys(data)

  let selected = []

  const handleClick = vegetable => {
    if (selected.includes(vegetable)) {
      selected = selected.filter(veg => veg !== vegetable)
    } else {
      selected = [...selected, vegetable]
    }
  }

$: enemies = uniq(selected.reduce((enemies, veg) => {
  return [...enemies, ...data[veg].enemies]
}, []))

$: allFriends = uniq(selected.reduce((friends, veg) => {
  return [...friends, ...data[veg].friends]
}, []))
$: friends = allFriends.filter(friend => !enemies.includes(friend))
</script>

<main>
  <h1>Friendly Companions</h1>
  {#each vegetables as veg}
    <Button enemy={enemies.includes(veg)} friend={friends.includes(veg)} selected={selected.includes(veg)} handleClick={() => handleClick(veg)} {veg} />
  {/each}
    <Col>
      {#if intersection(enemies, selected).length > 0}
        <Alert class="red-text" border="top" coloredBorder>
          Some of the plants you've selected don't play nicely together.
        </Alert>
      {/if}
  <Row>
    <Col>
    <h2>Friends</h2>
    <ul>
      {#each friends as friend}
        <li>{friend}</li>
      {/each}
    </ul>
    </Col>
    <Col>
    <h2>Enemies</h2>
    <ul>
      {#each enemies as enemy}
        <li>{enemy}</li>
      {/each}
    </ul>
    </Col>
  </Row>
    </Col>

  <h2>Your Vegetables</h2>
  <table>
    <thead>
      <tr><th></th><th>Friends</th><th>Enemies</th></tr>
    </thead>
    <tbody>
      {#each selected as veg}
        <tr><td>{veg}</td>
          <td>
            <ul>
              {#each data[veg].friends as friend}
                <li>{friend}</li>
              {/each}
            </ul>
          </td>
          <td>
            <ul>
              {#each data[veg].enemies as enemy}
                <li>{enemy}</li>
              {/each}
            </ul>
          </td>
        </tr>
      {/each}
    </tbody>
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

  ul {
    margin: auto 2rem;
    list-style-type: none;
    padding: 0;
    margin: 0;
  }

  @media (max-width: 640px) {
    main {
      max-width: none;
    }

    div.row {
      width: 100%;
      margin: auto 1rem;
    }
  }
</style>
