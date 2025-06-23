<script lang="ts">
  import '../app.css';

  let {children} = $props();

  // <div className="min-h-screen bg-gradient-to-br from-purple-400 via-pink-500 to-red-500 p-4">
</script>

<div class="min-h-screen bg-gradient-to-br from-amber-400 via-orange-500 to-red-600 p-4">
  <div class="max-w-md mx-auto">
    {@render children()}
  </div>
</div>

<style>
</style>