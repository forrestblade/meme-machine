<script>
  import GifSearch from "./GifSearch.svelte";
  import SearchResults from "./SearchResults.svelte";
import quotesGo from "quotes-go";

  const UNSPLASH_ACCESS_KEY = "Z2IU9ynLCIABokBbv8xQE3rzuwZHRWsW";

  let searchQuery = "";
  $: hidden = false;
  let searchTerm = null;
  let searchResults = [];
  let isLoading = false;

  function handleSubmit() {
    searchTerm = searchQuery.trim();
    searchResults = [];

    if (!searchTerm) return;

    searchGifs(searchTerm);
  }

  function searchGifs(search) {
    isLoading = true;

    const endpoint = `https://api.giphy.com/v1/gifs/search?api_key=Z2IU9ynLCIABokBbv8xQE3rzuwZHRWsW&q=${search}&limit=25&offset=0&rating=G&lang=en`;

    fetch(endpoint)
      .then(response => {
        if (!response.ok) {
          throw Error(response.statusText);
        }
        return response.json();
      })
      .then(data => {
        if (data.total === 0) {
          alert("No gifs were found for your search query.");
          return;
        }

        searchResults = [...searchResults, ...data.data];
        const header = document.querySelector('div.header')
        if (!header.classList.contains('hidden')) {
          hidden = true
        }
        const randomQuote = quotesGo.getRandomQuote();
        document.querySelector('p.tc').innerText = `${randomQuote.text} - ${randomQuote.author.name}` 
      })
      .catch(() => alert("An error occured!"))
      .finally(() => {
        isLoading = false;
        
      });
  }
</script>

<main id="app">
  <div class="{hidden ? 'header hidden': 'header'}">
    <img src="images/peppe.png" alt="peppe"/>
    <h1>Welcome to the Meme Machine</h1>
  </div>
    <p class="tc">
      Search for memes here or checkout where
      <a href="https://giphy.com/" target="_blank" rel="noopener"> they come from</a>.
    </p>	
    <GifSearch bind:query={searchQuery} handleSubmit={handleSubmit}/>
    <div class="cf pa4">
      <SearchResults results={searchResults} />
    </div>
</main>

<style>
  #app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    /* margin-top: 60px; */
  }

  .hidden {
    display: none;
    opacity: 0;
    transition: display 0s 2s, opacity 2s linear;
  }
</style>
