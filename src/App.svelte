<script>
  import AutoComplete from "simple-svelte-autocomplete";
  let myValue = null;

  async function getItems(keyword) {
    const url =
      "https://localhost:5001/Movies?term=" + encodeURIComponent(keyword);

    const response = await fetch(url);
    const json = await response.json();

    return json;
  }

  $: if (myValue) {
    console.log("yes");
    getItem();
  }

  async function getItem() {
    const url =
      "https://localhost:5001/Movies/" + encodeURIComponent(myValue.movieName);

    const response = await fetch(url);
    const json = await response.json();

    console.log(json);
    return json;
  }
</script>

<div class="wraper">
  <AutoComplete
    class="search"
    searchFunction={getItems}
    delay="200"
    placeholder="search movies"
    localFiltering="false"
    labelFieldName="movieName"
    valueFieldName="id"
    labelFunction={(movie) => movie.movieName + "-" + movie.year}
    bind:selectedItem={myValue}
  />
  {#if myValue}
    <div class="results">
      <p>{myValue.movieName} - {myValue.year}</p>
      <p>{myValue.genre}</p>
      <p>clicks: {myValue.clicks}</p>
      <p>views: {myValue.views}</p>
    </div>
  {/if}
</div>

<style>
  .wraper {
    margin: auto;
    width: 60%;
    min-width: 1000px;
    padding: 10px;
  }

  .wraper :global(.autocomplete) {
    margin-top: 40px;
  }
  .wraper :global(input) {
    /* color: red; */
    padding: 10px !important;
    width: 990px !important;
  }
  .results {
    margin-top: 70px;
  }
</style>
