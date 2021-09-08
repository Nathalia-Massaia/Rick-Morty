<template>
  <div class="inputSearch">
    <input
      class="
        px-2
        text-sm
        focus:ring-2 focus:ring-blue-400
        focus:outline-none
        rounded-sm
      "
      type="text"
      placeholder="search residents"
      @keyup="handleSearch($event)"
    />
  </div>
  <CharactersCard v-if="characters">
    <CharacterContent
      v-for="character in filteredCharacters"
      :key="character.id"
    >
      <ChatacterItem :character="character" />
    </CharacterContent>
  </CharactersCard>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import { Character } from '@/types';
import CharactersCard from './CharactersCard.vue';
import CharacterContent from './CharacterContent.vue';
import ChatacterItem from './CharacterItem.vue';

export default defineComponent({
  components: {
    CharactersCard,
    CharacterContent,
    ChatacterItem,
  },
  props: {
    characters: {
      required: true,
      type: Array as PropType<Character[]>,
    },
  },
  data() {
    return {
      filteredCharacters: new Array<Character>(),
    };
  },

  methods: {
    handleSearch(event: any) {
      const searchItem = event.target.value;

      this.filteredCharacters = this.characters.filter((char) =>
        char.name.toLowerCase().includes(searchItem.toLowerCase())
      );
      if (!this.filteredCharacters.length) {
        this.filteredCharacters = this.characters;
      }
    },
  },
  mounted() {
    if (!this.filteredCharacters.length) {
      this.filteredCharacters = this.characters;
    }
  },
});
</script>
<style>
.inputSearch {
  display: flex;
  justify-content: flex-end;
  padding: 10px;
  height: 3rem;
}
</style>
