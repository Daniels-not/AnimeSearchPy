<template>
  <div class="app">
    <header>
      <div class="four">
        <h1><span>Created by Daniels</span> Anime <em>Search</em> Py</h1>
      </div>
    </header>

    <form class="search-box" @submit.prevent="handleSearch">
      <input
        class="search-field"
        type="text"
        placeholder="I am looking for ... "
        v-model="dataSet"
        required
      />
    </form>

    <main>
      <div class="cards" v-if="animeList.length > 0">
        <card v-for="anime in animeList" :key="anime.mal_id" :anime="anime" />
      </div>
      <div class="no-results" v-else>
        <h1>Sorry, ) : No results</h1>
      </div>
    </main>
  </div>
</template>

<script>
import { ref } from "vue";
import card from "./components/card.vue";
export default {
  setup() {
    const dataSet = ref("");
    const animeList = ref([]);

    const handleSearch = async () => {
      animeList.value = await fetch(
        `https://api.jikan.moe/v3/search/anime?q=${dataSet.value}`
      )
        .then((res) => res.json())
        .then((data) => data.results);

      dataSet.value = "";
    };

    return {
      dataSet,
      animeList,
      handleSearch,
    };
  },
  components: {
    card,
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Lato:wght@300;400;700&display=swap");
$primary_styles: (
  "primary_color": hsl(220, 80%, 48%),
  "text_color": hsl(0, 0%, 3%),
  "span_color": rgba(0, 0, 0, 0.5),
  "under_line": hsl(0, 0%, 47%),
  "text_input_color": hsl(0, 0%, 20%),
  "text_box_bg": hsl(0, 0%, 95%),
  "placeholder": hsl(0, 0%, 67%),
  "valid": hsl(0, 0%, 100%),
  "valid_bg": hsl(0, 0%, 19%),
);

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Lato", sans-serif;
}

a {
  text-decoration: none;
}

header {
  text-align: center;
  padding-top: 2rem;
  padding-bottom: 4rem;

  h1 {
    position: relative;
    padding: 0;
    margin: 0;
    font-weight: 300;
    font-size: 3rem;
    color: map-get($primary_styles, text_color);
    text-align: center;
    padding-bottom: 0.7em;
    -webkit-transition: all 0.4s ease 0s;
    -o-transition: all 0.4s ease 0s;
    transition: all 0.4s ease 0s;

    &:before {
      position: absolute;
      left: 0;
      bottom: 0;
      width: 4rem;
      height: 0.1rem;
      content: "";
      left: 50%;
      margin-left: -55px;
      background-color: map-get($primary_styles, under_line);
    }

    span {
      display: block;
      font-size: 0.5em;
      line-height: 1.3;
      font-weight: 300;
      word-spacing: 3px;
      line-height: 2em;
      padding-bottom: 0.35em;
      color: map-get($primary_styles, span_color);
    }

    em {
      font-style: normal;
      font-weight: 600;
    }
  }
}

.search-box {
  display: flex;
  justify-content: center;
  padding-inline: 2rem;

  .search-field {
    appearance: none;
    background: none;
    border: none;
    outline: none;

    background-color: map-get($primary_styles, text_box_bg);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);

    display: block;
    width: 100%;
    max-width: 37.5rem;
    padding: 1rem;
    border-radius: 0.3rem;

    color: map-get($primary_styles, text_input_color);
    font-size: 1.2rem;
    transition: 0.4s;

    &::placeholder {
      color: map-get($primary_styles, placeholder);
    }

    &:focus,
    &:valid {
      color: map-get($primary_styles, valid);
      background-color: map-get($primary_styles, valid_bg);
      box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.15);
    }
  }
}

main {
  max-width: 1200px;
  margin: 0 auto;
  padding-inline: 30px;
  padding-block: 30px;
  .cards {
    display: flex;
    flex-wrap: wrap;
    margin: 0 -8px;
  }
}
</style>
