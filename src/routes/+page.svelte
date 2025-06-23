<script lang="ts">
  import { Plus, Users } from 'lucide-svelte';
  import { avatarOptions, user } from '$lib/stores/user-store.svelte';

  let lobbyCode = $state('1234');
</script>

<svelte:head>
  <title>Trunkenbolde</title>
  <meta name="Trunkenbolde" content="Gemeinsame Ausgaben im Blick"/>
</svelte:head>

<div class="text-center mb-8 pt-12">
  <div class="text-6xl mb-4 animate-bounce">🍻</div>
  <h1 class="text-4xl font-bold text-white mb-2 drop-shadow-lg">Trunkenbolde</h1>
  <p class="text-orange-100">Gemeinsame Ausgaben im Blick</p>
</div>

<div class="bg-white/90 backdrop-blur-sm rounded-2xl p-6 mb-6 shadow-xl">
  <h2 class="text-xl font-semibold mb-4 text-gray-800">Dein Profil</h2>

  <div class="mb-4">
    <label for="name" class="block text-sm font-medium text-gray-700 mb-2">Dein Name</label>
    <input
        name="name"
        type="text"
        bind:value={user.name}
        placeholder="Name eingeben..."
        class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-orange-500 focus:border-transparent transition-all"
        maxLength={20}
    />
  </div>

  <div class="mb-6">
    <label for="avatar" class="block text-sm font-medium text-gray-700 mb-2">Avatar wählen</label>
    <div class="grid grid-cols-5 gap-2">
      {#each avatarOptions as option (option)}
        <button onclick={() => user.avatar = option}
                class="text-2xl p-3 rounded-lg border-2 transition-all transform hover:scale-105
									{user.avatar === option ?
									'border-orange-500 bg-orange-50 scale-105' :
									'border-gray-200 hover:border-orange-300'}">
          {option}
        </button>
      {/each}
    </div>
  </div>
</div>

<div class="space-y-4">
  <a href="/lobby/{lobbyCode}"
     class:opacity-50={!user.name.trim()}
     class:cursor-not-allowed={!user.name.trim()}
     class:pointer-events-none={!user.name.trim()}
     class="w-full bg-gradient-to-r from-green-500 to-green-600 hover:from-green-600 hover:to-green-700 disabled:from-gray-400 disabled:to-gray-500 text-white font-semibold py-4 px-6 rounded-xl transition-all transform hover:scale-105 disabled:hover:scale-100 flex items-center justify-center gap-2 shadow-lg"
  >
    <Plus class="w-5 h-5"/>
    Neue Party erstellen
  </a>

  <div class="bg-white/90 backdrop-blur-sm rounded-xl p-4 shadow-lg">
    <input
        type="text"
        bind:value={lobbyCode}
        placeholder="Party-Code eingeben..."
        class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-orange-500 focus:border-transparent mb-3 transition-all"
        maxLength={6}
    />
    <a href="/lobby/{lobbyCode}"
       class:opacity-50={!user.name.trim() || !lobbyCode.trim()}
       class:cursor-not-allowed={!user.name.trim() || !lobbyCode.trim()}
       class:pointer-events-none={!user.name.trim() || !lobbyCode.trim()}
       class="w-full bg-gradient-to-r from-blue-500 to-blue-600 hover:from-blue-600 hover:to-blue-700 disabled:from-gray-400 disabled:to-gray-500 text-white font-semibold py-3 px-6 rounded-lg transition-all transform hover:scale-105 disabled:hover:scale-100 flex items-center justify-center gap-2">
      <Users class="w-5 h-5"/>
      Party beitreten
    </a>
  </div>
</div>

<style>
</style>