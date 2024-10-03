<script setup>
import { reactive } from 'vue'


const data = reactive({
  logoUrl: '',
  date: '',
  invoiceNo: '',

  invoicetoName: '',
  invoicetoEmail: '',
  invoicetoAddress: '',
  invoicetoCountry: '',

  payToName: '',
  payToEmail: '',
  payToAddress: '',
  payToCountry: '',

  items: [
    {
      description: '',
      rate: '',
      quantity: '',
      amount: '',
    }
  ],
  Subtotal: '',
  Tax: '',
  Total: '',

  notes: '',

})


const addItem = () => {
  data.items.push({
    description: '',
    rate: '',
    quantity: '',
    amount: '',
  })
}

const removeItem = (index) => {
  data.items.splice(index, 1)
}

const getSubtotal = () => {
  let subtotal = 0
  data.items.forEach(item => {
    subtotal += item.amount
  })
  data.Subtotal = subtotal
  return subtotal
}


const getTotal = () => {
  let total = 0
  total = data.Subtotal + (data.Subtotal * data.Tax) / 100
  data.Total = total
  return total
}


const print = () => {
  const printContent = document.getElementById('print').innerHTML;
  const originalContent = document.body.innerHTML;

  document.body.innerHTML = printContent; 
  window.print();
  document.body.innerHTML = originalContent; 

  window.location.reload();
};

</script>

<template>
  <div class="bg_color bg-gray-300 p-5">
    <div class="mx-auto bg-white max-w-7xl my-5 rounded">
      <div class="content_wrap py-10 px-10">
        <header class="head_area flex justify-between items-center flex-shrink-0">
          <div class="logo flex items-center flex-shrink-0">
            <label class="w-[100px]">Logo Url :</label>
            <input type="text" v-model="data.logoUrl" class="input !w-[500px]">
          </div>
          <div>
            <h2 class="text-3xl">Invoice</h2>
          </div>
        </header>

        <main>
          <div class="date_and_invoiceno flex justify-between items-center py-4 my-4 border-y border-gray-300">
            <div class="input_wrap">
              <label for="date">Date: </label>
              <input type="date" id="date" v-model="data.date" class="input">
            </div>
            <div class="input_wrap">
              <label for="invoiceno" class="w-[140px]">Invoice No: </label>
              <input type="number" id="invoiceno" v-model="data.invoiceNo" class="input">
            </div>
          </div>

          <div class="invoice_info flex justify-between items-start mb-10">
            <div class="invoiceto text-left">
              <label class="block font-bold" for="">Invoiced To: </label>
              <div class="input_wrap mb-2">
                <label for="invoicetoName">Name: </label>
                <input type="text" id="invoicetoName" v-model="data.invoicetoName" class="input">
              </div>
              <div class="input_wrap mb-2">
                <label for="invoicetoEmail">Email: </label>
                <input type="text" id="invoicetoEmail" v-model="data.invoicetoEmail" class="input">
              </div>
              <div class="input_wrap mb-2">
                <label for="invoicetoAddress">Address: </label>
                <input type="text" id="invoicetoAddress" v-model="data.invoicetoAddress" class="input">
              </div>
              <div class="input_wrap mb-2">
                <label for="invoicetoPhone">Country: </label>
                <input type="text" id="invoicetoCountry" v-model="data.invoicetoCountry" class="input">
              </div>
            </div>
            <div class="payTo text-right">
              <label class="block font-bold" for="">Pay To: </label>
              <div class="input_wrap mb-2">
                <label>Name: </label>
                <input type="text" v-model="data.payToName" class="input">
              </div>
              <div class="input_wrap mb-2">
                <label>Email: </label>
                <input type="text" v-model="data.payToEmail" class="input">
              </div>
              <div class="input_wrap mb-2">
                <label>Address: </label>
                <input type="text" v-model="data.payToAddress" class="input">
              </div>
              <div class="input_wrap mb-2">
                <label>Country: </label>
                <input type="text" v-model="data.payToCountry" class="input">
              </div>
            </div>
          </div>

          <div class="items_wrap py-5 border-y border-gray-300">
            <table class="w-full">
              <thead class="bg-gray-200">
                <tr>
                  <th class="text-left min-w-[500px] px-3 py-2">Description</th>
                  <th class="text-left px-3 py-1">Rate</th>
                  <th class="text-left px-3 py-1">Quantity</th>
                  <th class="text-left px-3 py-1">Amount</th>
                </tr>
              </thead>

              <tbody>
                <tr v-for="(item, index) in data.items" class="group">
                  <td class="text-left"><input type="text" v-model="item.description" class="input"></td>
                  <td class="text-left"><input type="number" v-model="item.rate" class="input"></td>
                  <td class="text-left "><input type="number" v-model="item.quantity" class="input"></td>
                  <td class="text-left "><span>$ {{ item.amount = item.quantity * item.rate }}</span></td>
                  <td class="text-left cursor-pointer opacity-0 group-hover:opacity-100">
                    <button @click="removeItem(index)"><svg xmlns="http://www.w3.org/2000/svg" fill="none"
                        viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M6 18 18 6M6 6l12 12" />
                      </svg> </button>
                  </td>
                </tr>
              </tbody>

            </table>
          </div>
          <div class="mt-5">
            <button @click="addItem" class="btn_add">Add Item</button>
          </div>

          <div class="calculator_area mt-5 flex justify-end">
            <div>
              <div class="input_wrap mb-2">
                <label class="block font-bold" for="">Subtotal: </label>
                <input type="number" :value="getSubtotal()" class="input" readonly>
              </div>
              <div class="input_wrap mb-2">
                <label class="block font-bold" for="">Tax: </label>
                <input type="number" v-model="data.Tax" class="input"><span>%</span>
              </div>
              <div class="input_wrap mb-2">
                <label class="block font-bold" for="">Total: </label>
                <input type="number" :value="getTotal()" class="input" readonly>
              </div>
            </div>
          </div>
          <div class="div">
            <label class="block font-bold" for="">Notes: </label>
            <textarea rows="3" v-model="data.notes" class="input"></textarea>
          </div>

          <div class="mt-5">
            <button @click="print" class="btn_print">Print</button>
          </div>

        </main>
      </div>
    </div>
  </div>

  <!-- print view -->
  <div class="bg_color bg-gray-300 p-5 hidden">
    <div id="print" class="mx-auto bg-white max-w-7xl my-5 rounded print_view">
      <div class="content_wrap py-10 px-10">
        <header class="head_area flex justify-between items-center">
          <div class="logo w-[200px]">
            <img :src="data.logoUrl" alt="">
          </div>
          <div>
            <h2 class="text-3xl">Invoice</h2>
          </div>
        </header>

        <main>
          <div class="date_and_invoiceno flex justify-between items-center py-4 my-4 border-y border-gray-300">
            <div class="input_wrap">
              <label for="date">Date: </label>
              <span>{{ data.date }}</span>
            </div>
            <div class="input_wrap">
              <label for="invoiceno">Invoice No: </label>
              <span>#{{ data.invoiceNo }}</span>
            </div>
          </div>

          <div class="invoice_info flex justify-between items-start mb-10">
            <div class="invoiceto text-left">
              <label class="block font-bold mb-3" for="">Invoiced To: </label>
              <div class="input_wrap">
                <p>{{ data.invoicetoName }}</p>
              </div>
              <div class="input_wrap">
                <p>{{ data.invoicetoEmail }}</p>
              </div>
              <div class="input_wrap">
                <p>{{ data.invoicetoAddress }}</p>
              </div>
              <div class="input_wrap mb-2">
                <p>{{ data.invoicetoCountry }}</p>
              </div>
            </div>
            <div class="payTo !text-right">
              <label class="block font-bold" for="">Pay To: </label>
              <div class="input_wrap justify-end">
                <p>{{ data.payToName }}</p>
              </div>
              <div class="input_wrap justify-end">
                <p>{{ data.payToEmail }}</p>
              </div>
              <div class="input_wrap justify-end">
                <p>{{ data.payToAddress }}</p>
              </div>
              <div class="input_wrap justify-end">
                <p>{{ data.payToCountry }}</p>
              </div>
            </div>
          </div>

          <div class="items_wrap py-5 border-y border-gray-300">
            <table class="w-full">
              <thead class="bg-gray-200">
                <tr>
                  <th class="text-left min-w-[500px] px-3 py-2">Description</th>
                  <th class="text-left px-3 py-1">Rate</th>
                  <th class="text-left px-3 py-1">Quantity</th>
                  <th class="text-left px-3 py-1">Amount</th>
                </tr>
              </thead>

              <tbody>
                <tr v-for="(item, index) in data.items" class="group border border-gray-300">
                  <td class="text-left">
                    <p>{{ item.description }}</p>
                  </td>
                  <td class="text-left">
                    <p>{{ item.rate }}</p>
                  </td>
                  <td class="text-left ">
                    <p>{{ item.quantity }}</p>
                  </td>
                  <td class="text-left ">
                    <p>$ {{ item.amount }}</p>
                  </td>
                </tr>
              </tbody>

            </table>
          </div>

          <div class="calculator_area mt-5 flex justify-end bg-gray-100">
            <div class="px-3 py-2 w-[200px]">
              <div class="input_wrap justify-between mb-2 border-b border-gray-300">
                <label class="block font-bold w-[100px]" for="">Subtotal: </label>
                <p>$ {{ data.Subtotal }}</p>
              </div>
              <div class="input_wrap justify-between mb-2 border-b border-gray-300">
                <label class="block font-bold w-[100px]" for="">Tax: </label>
                <p>{{ data.Tax }} %</p>
              </div>
              <div class="input_wrap justify-between mb-2 border-b border-gray-300">
                <label class="block font-bold w-[100px]" for="">Total: </label>
                <p>$ {{ data.Total }}</p>
              </div>
            </div>
          </div>
          <div v-if="data.notes" class="mt-5 text-center">
            <p><strong>Notes: </strong>{{ data.notes }}</p>
          </div>

        </main>
      </div>
    </div>
  </div>
</template>

