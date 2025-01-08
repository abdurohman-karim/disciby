<template>
  <div class="bg-cyan-900 text-light min-h-screen">
    <header class="py-3 bg-dark text-white shadow-md">
      <div class="container mx-auto flex justify-between items-center">
        <a href="/" class="text-white text-decoration-none flex items-center">
          <svg class="me-2" width="40" height="32" role="img" aria-label="Bootstrap"><use xlink:href="#bootstrap"></use></svg>
          <span class="text-lg font-semibold">GMC Community</span>
        </a>

        <ul class="flex space-x-6 text-lg">
          <li><a href="index.html" class="text-white hover:text-secondary transition">Home</a></li>
          <li><a href="per_day.html" class="text-secondary hover:text-white transition">Per/day</a></li>
          <li><a href="per_week.html" class="text-white hover:text-secondary transition">Per/week</a></li>
          <li><a href="per_month.html" class="text-white hover:text-secondary transition">Per/month</a></li>
        </ul>

        <div class="text-end">
          <a href="day_calc.html" class="btn btn-success text-white">Date Calculator</a>
          <a href="https://github.com/GMCommunityUZ" class="btn btn-light me-2">Join us</a>
          <a href="https://t.me/abdurohman_karimov" target="_blank" class="btn btn-primary">Developer</a>
        </div>
      </div>
    </header>

    <div class="bg-cyan-400 text-center py-10">
      <h1 class="text-light text-4xl font-bold">Calc by Day</h1>
    </div>

    <div class="container mx-auto py-10 px-4 sm:px-6 lg:px-8 mb-5">
      <div class="space-y-6">
        <div class="flex flex-col">
          <label for="days" class="text-white text-lg font-medium">Days</label>
          <input v-model="days" type="number" id="days" class="bg-white py-2 px-3 rounded-md border-2 border-light focus:outline-none focus:ring-2 focus:ring-primary" placeholder="Total Days" />
        </div>

        <div class="flex flex-col">
          <label for="deposit" class="text-white text-lg font-medium">Start Deposit (USD)</label>
          <input v-model="deposit" type="number" id="deposit" class="bg-white py-2 px-3 rounded-md border-2 border-light focus:outline-none focus:ring-2 focus:ring-primary" placeholder="1000$" />
        </div>

        <div class="flex flex-col">
          <label for="profit" class="text-white text-lg font-medium">Profit per/day (%)</label>
          <input v-model="profit" type="number" id="profit" class="bg-white py-2 px-3 rounded-md border-2 border-light focus:outline-none focus:ring-2 focus:ring-primary" placeholder="Percent of Profit" />
        </div>

        <div class="flex items-center space-x-2">
          <input v-model="redeposit" type="checkbox" id="redeposit" class="text-light rounded-full" />
          <label for="redeposit" class="text-white text-lg">Re-Deposit Every Day</label>
        </div>

        <div class="flex justify-center">
          <button @click="calculate" class="flex justify-items-center items-center bg-cyan-300 px-10 py-3 ">Calculate</button>
        </div>

        <hr class="bg-light my-6" />
        <div v-html="tableHtml" class="table-responsive"></div>
      </div>
    </div>

    <footer class="bg-secondary py-6">
      <div class="container mx-auto text-center">
        <span class="text-light">GMC Community &copy; <span>{{ currentYear }}</span></span>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      days: 0,
      deposit: 1000,
      profit: 0,
      redeposit: false,
      tableHtml: '',
      currentYear: new Date().getFullYear(),
    };
  },
  methods: {
    calculate() {
      let total = 0;
      let growth = 0;
      let string = `<table class="table-auto w-full mx-auto mt-6 max-w-4xl border-collapse">
        <thead>
          <tr class="bg-gray-800 text-white">
            <th class="px-4 py-2 text-left">Day</th>
            <th class="px-4 py-2 text-left">Initial</th>
            <th class="px-4 py-2 text-left">Deposit</th>
            <th class="px-4 py-2 text-left">Profit</th>
            <th class="px-4 py-2 text-left">Total</th>
          </tr>
        </thead>
        <tbody>`;

      for (let i = 0; i < this.days; i++) {
        string += "<tr class='hover:bg-gray-700'>";
        string += `<td class="px-4 py-2">${i + 1}</td>`;
        string += `<td class="px-4 py-2">$ ${total.toFixed(2)}</td>`; // initial
        if (i == 0) {
          total += this.deposit;
        }
        if (this.redeposit && i > 0) {
          total += this.deposit;
        }

        if (!this.redeposit && i > 0) {
          this.deposit = 0;
        }

        string += `<td class="px-4 py-2">$ ${this.deposit.toFixed(2)}</td>`; // deposit
        growth = total * (this.profit / 100);
        string += `<td class="px-4 py-2">$ ${growth.toFixed(2)}</td>`; // profit
        total += total * (this.profit / 100);
        string += "<td class='px-4 py-2'>$ " + total.toFixed(2) + "</td></tr>"; // total
      }
      string += `</tbody></table>`;
      this.tableHtml = string;
    },
  },
};
</script>

<style scoped>
/* Additional styles for customizations can be added here */
</style>
