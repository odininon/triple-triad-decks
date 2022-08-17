<template>
  <div>
    <DeckListItem
      v-for="deck in decks"
      :key="deck.id"
      :deck="deck"
      @delete="deleteDeck"
    ></DeckListItem>
    <button @click="createNewDeck">Create New Deck</button>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();

const loading = ref(true);
const decks = ref([]);

loading.value = true;
const user = useUser();
let { data } = await supabase
  .from("decks")
  .select("id,name")
  .eq("user_id", user.value.id);

if (data) {
  decks.value = data;
}

async function deleteDeck(deckId) {
  await supabase.from("decks").delete().eq("id", deckId);
  decks.value = decks.value.filter((deck) => deck.id != deckId);
}

loading.value = false;

async function createNewDeck() {
  try {
    loading.value = true;
    const user = useUser();
    const updates = {
      user_id: user.value.id,
      name: "New Deck",
    };
    let { data, error } = await supabase.from("decks").insert(updates);
    if (data) {
      decks.value.push(...data);
    }
    if (error) throw error;
  } catch (error) {
    alert(error.message);
  } finally {
    loading.value = false;
  }
}
</script>