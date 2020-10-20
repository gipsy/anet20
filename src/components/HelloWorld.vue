<template>
  <section>
    <p class="content"><b>Selected:</b> {{ selectedItem }}</p>
    <b-field label="Find or add the town">
      <b-autocomplete
        rounded
        clearable
        :loading="isFetching"
        v-model="userInput"
        ref="autocomplete"
        :data="filteredSuggestions"
        placeholder="e.g. Geneve"
        @select="option => (selectedItem = option)"
        @input="onChange"
      >
        <template slot="header">
          <a @click="addTown">
            <span> Add current town </span>
          </a> 
        </template>
        <template v-if="error" slot="empty">Error occured on server</template>
        <template v-else slot="empty">No results for {{userInput}}</template>
      </b-autocomplete>
    </b-field>
  </section>
</template>

<script>
import { useTownAutocomplete } from '@wellcode/core';
export default {
  setup() {
    const proxy = 'https://cors-anywhere.herokuapp.com/';
    const apiUrl = `${proxy}https://webservices.post.ch:17023/IN_SYNSYN_EXT/REST/v1/autocomplete2`
    return {
      ...useTownAutocomplete(apiUrl)
    }
  },
  created() {
  },
  methods: {
    addTown() {
      this.$refs.autocomplete.setSelected(this.userInput)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
