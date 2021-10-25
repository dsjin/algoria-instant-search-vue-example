<template>
  <ais-instant-search :search-client="searchClient" index-name="demo_ecommerce">
    <div
      class="bg-primary p-5"
    >
      <div
        class="d-flex justify-content-center"
        style="position: relative"
      >
        <div
          style="width: 30em"
        >
          <ais-autocomplete>
            <template v-slot="{ currentRefinement, indices, refine }">
              <b-form-input
                id="button-1"
                :value="currentRefinement"
                @input="(e) => refine(e)"
                @focus="focus = true"
                @blur="focus = false"
              />
              <template v-if="currentRefinement && (focus || focusIn)">
                <div
                  class="bg-light pt-2 rounded border"
                  style="position: absolute; width: 30em; z-index:10; top:50px"
                  @mouseover="focusIn = true"
                  @mouseleave="focusIn = false"
                >
                  <ul v-for="index in indices" :key="index.indexId" style="list-style-type:none">
                    <template
                      v-if="index.hits.length !== 0"
                    >
                      <li>
                        <h3>Result</h3>
                        <ul>
                          <li v-for="hit in index.hits" :key="hit.objectID">
                            <div
                              @click="() => {
                                focusIn=false
                                refine(hit.name)
                              }"
                              style="cursor:pointer"
                            >
                              <ais-highlight attribute="name" :hit="hit" />
                            </div>
                          </li>
                        </ul>
                      </li>
                    </template>
                    <template
                      v-else
                    >
                      <li>
                        Sorry, nothing found for "{{ currentRefinement }}"
                      </li>
                    </template>
                  </ul>
                </div>
              </template>
            </template>
          </ais-autocomplete>
        </div>
      </div>
    </div>
    <div class="search-container mt-2 p-4">
      <div class="left-panel">
        <ais-clear-refinements>
          <template v-slot:resetLabel>Clear filters</template>
        </ais-clear-refinements>
        <h2>Brands</h2>
        <ais-refinement-list attribute="brand" searchable />
        <ais-configure :hitsPerPage="8" />
      </div>
      <div class="right-panel">
        <ais-search-box />
        <ais-powered-by />
        <ais-current-refinements :included-attributes="['brand']" />
        <ais-hits :escape-HTML="true">
          <template v-slot:item="{ item }">
            <img :src="item.image" align="left" :alt="item.name" />
            <h2
              class="fs-5"
            >
              <ais-highlight
                attribute="name"
                :hit="item"
                highlightedTagName="mark"
              />
            </h2>
            <div class="hit-name">
              <ais-highlight attribute="name" :hit="item"></ais-highlight>
            </div>
            <div class="hit-description">
              <ais-highlight
                attribute="description"
                :hit="item"
              ></ais-highlight>
            </div>
            <div class="hit-price">{{ item.price }}</div>
          </template>
        </ais-hits>
        <ais-pagination />
      </div>
    </div>
  </ais-instant-search>
</template>

<script lang="ts">
import Vue from 'vue'
import algoliasearch from 'algoliasearch/lite'
import 'instantsearch.css/themes/satellite-min.css'

export default Vue.extend({
  name: 'App',
  data () {
    return {
      searchClient: algoliasearch(
        'B1G2GM9NG0',
        'aadef574be1f9252bb48d4ea09b5cfe5'
      ),
      focus: false,
      focusIn: false
    }
  }
})
</script>

<style>
body {
  font-family: sans-serif;
  /* padding: 1em; */
}

.ais-Hits-item {
  display: flex;
  flex-direction: column;
}

.ais-Hits-list {
  margin-top: 0;
  margin-bottom: 1em;
  display: grid;
  grid-gap: 40px;
  grid-template-columns: 1fr;
}

.ais-InstantSearch .search-container {
  display: grid;
  grid-template-columns: 1fr 4fr;
  grid-gap: 1em;
}

.ais-Hits-item img {
  margin-right: 1em;
}
.hit-name {
  margin-bottom: 0.5em;
}
.hit-description {
  color: #888;
  font-size: 0.8em;
  margin-bottom: 0.5em;
}
@media (min-width: 680px){
  .ais-Hits-list {
    grid-template-columns: 1fr 1fr;
  }
}
@media (min-width: 900px){
  .ais-Hits-list {
    grid-template-columns: 1fr 1fr 1fr;
  }
}
@media (min-width: 1200px){
  .ais-Hits-list {
    grid-template-columns: 1fr 1fr 1fr 1fr;
  }
}
</style>
