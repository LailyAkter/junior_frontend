
<script>
  import "../../app.css";
  import { onMount } from 'svelte';
  import Chart from 'chart.js/auto';
  import { countryDataStore } from '../../countryDataStore';

  onMount(async () => {
    try {
      const response = await fetch('https://restcountries.com/v3.1/all');
      const data = await response.json();
      countryDataStore.set(data);
    } catch (error) {
      console.error('Error fetching data:', error);
    }


    // Fetch and process data 
    // polar chart
    let data = $countryDataStore.slice(0, 10);
    const indexNumbers = Array.from({ length: data.length }, (_, i) => i + 1);
    data = data.map((country, i) => ({
      label: country.name.common,
      data:`${indexNumbers[i]}`,
    }));
    console.log(data)
    renderChart(data);

  });


  function renderChart(data) {
    var ctx = document.getElementById('polarAreaChart');

    Chart = new Chart(ctx, {
      
      type: 'polarArea',
      data: {
        labels: data.map((item) => item.label),
        datasets: [
          {
           data: data.map((item) => item.data),
           
          },
        ],
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          legend: {
            display: true,
             position: 'bottom',
          },
        },
      },
    });
  }

</script>

  

<div class="flex p-2">
  <div class="w-1/2 p-2 shadow-lg shadow-white-500/50 border mx-4">
      <table class=" text-sm text-left text-gray-500 dark:text-gray-400">
        <thead>
          <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
            <th>Flag</th>
            <th>Name</th>
            <th>Population</th>
            <th>CIOC</th>
            <th>UN Member Status</th>
            <th>Currencies</th>
            <th>Languages</th>
          </tr>
        </thead>
        <tbody>

          {#each $countryDataStore as country}
            <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
              <td class="px-4 py-2">
                <img src={country.flags.png} alt={country.name.common} width="30" />
              </td>
              <td class="px-4 py-2">{country.name.common}</td>
              <td class="px-4 py-2">{country.population}</td>
              <td class="px-4 py-2">{country.cioc}</td>
              <td class="px-4 py-2">
                <button class="rounded-full bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4">{country.unMember ? 'Yes' : 'No'}</button>
              </td>
              <td class="px-4 py-2">{Object.keys(country.currencies).join(', ')}</td>
              <td class="px-4 py-2">{Object.values(country.languages).join(', ')}</td>
            </tr>
          {/each}
            
        </tbody>
      </table>
  </div>
  <div class="w-1/2 p-2 shadow-lg shadow-white-500/50 border" style="height:700px">
    <h2 class="text-lg font-semibold border-b-2 " >Countries</h2>
    <div style="width: 500px; height: 400px;margin-left: 100px;margin-top: 100px;">
      <canvas id="polarAreaChart"></canvas>
    </div>
    
  </div>
</div>

