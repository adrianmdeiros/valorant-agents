<template>
  <div class="h-full w-full dark:bg-neutral-950 dark:text-neutral-100 bg-neutral-200 text-neutral-950">
    <header class="flex justify-between items-center px-9 py-8 w-full">
      <h1 class="text-3xl font-bold text-neutral-100">VUE VALORANT AGENTS</h1>
    </header>
    <main class="px-4 py-8 min-h-screen">
      <ul class="flex gap-4 flex-wrap lg:grid grid-cols-3">
        <li class="border-2 border-opacity-15 border-neutral-800 rounded-xl w-full h-fit"
          v-for="valorantAgent in valorantAgents" :key="valorantAgent.uuid">
          <img class="object-top object-cover w-full h-80" :src="valorantAgent.bustPortrait" alt="Foto do agente" />
          <div
            class="bg-neutral-100 p-8 dark:text-neutral-100 text-neutral-900 dark:bg-neutral-900 border-t-2 rounded-xl border-neutral-800 border-opacity-10 dark:border-neutral-800">
            <div class="flex items-center justify-between mb-10">
              <h2 class="text-2xl font-bold dark:text-neutral-100">
                {{ valorantAgent.displayName }}
              </h2>
              <p class="rounded-border-2 border-neutral-800 text-start font-bold italic">
                {{ valorantAgent.role?.displayName }}
              </p>
            </div>
            <p class="px-4 rounded-border-2 border-neutral-800 text-start mb-10">
              {{ valorantAgent.description }}
            </p>
            <h2 class="mt-6 font-bold">Habilidades</h2>
            <details
              class="mt-6 border-2 border-neutral-800 hover:border-neutral-100 p-4 items-center rounded-xl cursor-pointer"
              v-for="abilitie in valorantAgent.abilities" :key="abilitie?.slot">
              <summary class="flex items-center gap-4 font-bold">
                <img :src="abilitie.displayIcon" alt="Ícone da habilidade" class="h-10" />
                {{ abilitie.displayName }}
              </summary>
              <p class="mt-6">
                {{ abilitie.description }}
              </p>
            </details>
          </div>
        </li>
        <div v-if="isLoading"
          className="border-indigo-500 h-6 w-6 animate-spin rounded-full border-2 border-t-neutral-100"></div>
      </ul>
    </main>
    <footer class="grid place-content-center w-full h-24">
      <p class="font-bold">
        Made with ❤ by
        <a class="text-blue-600 hover:text-blue-700" href="https://github.com/adrianmdeiros">adrianmdeiros</a>
      </p>
    </footer>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

interface ValorantAgent {
  uuid: string;
  bustPortrait: string;
  displayName: string;
  role: {
    displayName: string;
  };
  description: string;
  abilities: Abilities[];
}

interface Abilities {
  slot: string;
  displayName: string;
  description: string;
  displayIcon: string;
}

export default defineComponent({
  data() {
    return {
      valorantAgents: [] as ValorantAgent[],
      isLoading: true,
    };
  },
  created() {
    this.getData();
  },
  methods: {
    getData() {
      fetch(
        "https://valorant-api.com/v1/agents?language=pt-BR&isPlayableCharacter=true"
      )
        .then((response) => response.json())
        .then((data) => {
          this.valorantAgents = data.data
        })
        .then(() => (this.isLoading = false))
        .catch((e) => console.error(e.message));
    },
  },
});
</script>
