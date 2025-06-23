<script lang="ts">
  import { Plus, Trash2 } from 'lucide-svelte';

  let expenses: {timestamp: string, name: string, product: string, price: number}[] = $state([]);
  const totalExpenses = $derived(expenses.reduce((sum, expense) => sum + expense.price, 0));

  let product = $state('');
  let price = $state(0);
  let participants = $state<{name: string, avatar: string}[]>([]);

  const drinkPresets = [
    {name: 'Bier', price: 4.50},
    {name: 'Cocktail', price: 8.00},
    {name: 'Shot', price: 3.00},
    {name: 'Longdrink', price: 6.500},
    {name: 'Wasser', price: 2.50},
    {name: 'Cola', price: 3.50},
  ];

  const addExpense = (product: string, price: number) => {
    const timestamp = new Date().toTimeString().slice(0, 5);
    expenses.push({timestamp, name: 'TODO', product, price});
  };

  const removeExpense = (expenseToRemove: {timestamp: string, name: string, product: string, price: number}) => {
    expenses = expenses.filter(expense => expense === expenseToRemove);
  };
</script>

<div class="space-y-4">
  <div class="bg-white/90 backdrop-blur-sm rounded-xl p-4 shadow-lg">
    <h3 class="font-semibold text-gray-800 mb-3">Schnell hinzufügen</h3>
    <div class="grid grid-cols-2 gap-2">
      {#each drinkPresets as preset (preset.name)}
        <button onclick={() => {product = preset.name; price = preset.price;}}
                class="flex items-center gap-2 p-3 bg-gradient-to-r from-orange-400 to-pink-400 text-white rounded-lg hover:from-orange-500 hover:to-pink-500 transition-all transform hover:scale-105 shadow-md">
          <span class="font-medium text-sm">{preset.name}</span>
        </button>
      {/each}
    </div>
  </div>

  <div class="bg-white/90 backdrop-blur-sm rounded-xl p-4 shadow-lg">
    <h3 class="font-semibold text-gray-800 mb-3">Eigener Eintrag</h3>
    <div class="space-y-3">
      <input type="text"
             bind:value={product}
             placeholder="Was hast du bestellt?"
             class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent transition-all"
             maxLength={30}/>
      <div class="flex gap-2">
        <input type="number"
               step="0.01"
               bind:value={product}
               placeholder="Preis"
               class="flex-1 px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent transition-all"/>
        <button onclick={() => addExpense(product, price)}
                disabled={!product.trim() || !price}
                class="bg-green-500 hover:bg-green-600 disabled:bg-gray-400 text-white px-6 py-3 rounded-lg transition-all transform hover:scale-105 disabled:hover:scale-100">
          <Plus class="w-5 h-5"/>
        </button>
      </div>
    </div>
  </div>

  <div class="bg-white/90 backdrop-blur-sm rounded-xl p-4 shadow-lg">
    <div class="flex justify-between items-center mb-3">
      <h3 class="font-semibold text-gray-800">Deine Ausgaben</h3>
      <div class="text-xl font-bold text-green-600">
        {totalExpenses.toFixed(2)}€
      </div>
    </div>
    <div class="space-y-2 max-h-80 overflow-y-auto">
      {#if !expenses.length}
        <p class="text-gray-500 text-center py-4">Noch keine Ausgaben</p>
      {/if}
      {#each expenses as expense (expense.timestamp)}
        <div class="flex justify-between items-center p-3 bg-gray-50 rounded-lg hover:bg-gray-100 transition-colors">
            <span class="flex-1 font-medium">
              {expense.name}
            </span>
          <div class="flex items-center gap-2">
            <span class="text-green-600 font-semibold">{expense.price.toFixed(2)}€</span>
            <button onclick={() => removeExpense(expense)}
                    class="p-1 text-red-500 hover:text-red-700 hover:bg-red-50 rounded-full transition-colors"
                    title="Ausgabe löschen">
              <Trash2 class="w-4 h-4"/>
            </button>
          </div>
        </div>
      {/each}
    </div>
  </div>

  <div class="bg-white/90 backdrop-blur-sm rounded-xl p-4 shadow-lg">
    <h3 class="font-semibold text-gray-800 mb-3">Teilnehmer ({participants.length})</h3>
    <div class="space-y-2">
      {#each participants as participant (participant.name)}
        <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg hover:bg-gray-100 transition-colors">
          <div class="flex items-center gap-3">
            <span class="text-2xl">{participant.avatar}</span>
            <div>
              <span class="font-medium">{participant.name}</span>
            </div>
          </div>
          <span class="text-sm text-gray-600 font-medium">TODO€</span>
        </div>
      {/each}
    </div>
  </div>
</div>