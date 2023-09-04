<script>
  import "../app.css";
</script>

<style>
  /* Add your component styles here */
</style>

<!-- Your layout component's markup -->
<div>
  <nav class="bg-blue-500 p-4">
    <div class="container mx-auto">
      <div class="flex items-center justify-between">
        <div class="text-white font-semibold text-xl">
          <a href="#">Your Logo</a>
        </div>
        <div class="hidden md:flex space-x-4">
          <a href="/layout">Layout</a>
          <a href="/chart">Charts</a>
          <a href="/map">Maps</a>
        </div>
      </div>
    </div>
  </nav>
</div>


<slot />
