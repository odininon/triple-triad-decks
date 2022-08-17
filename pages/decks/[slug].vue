<template>
  <p>{{ deck }}</p>
</template>

<script setup>
const route = useRoute();
const supabase = useSupabaseClient();

const user = useUser();
const deck = ref(null);

let { data, error } = await supabase
  .from("decks")
  .select("id,name,user_id")
  .eq("id", route.params.slug)
  .single();

if (data) {
  deck.value = data;
}

if (error) {
  navigateTo({
    path: "/",
  });
}

console.log(data);
</script>