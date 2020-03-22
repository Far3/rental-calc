<template>
  <section class="container">
    <div class="col-30 purchase">
      <h1>Acquisition Information</h1>
      <label for="purchasePrice">Purchase Price ($):</label>
      <input v-model.number="purchasePrice"  type="number" placeholder="purchasePrice" name="purchasePrice">
      
      <div>
        <label for="downPayment">Down Payment ($):</label>
        <input v-model.number="downPayment"  type="number" placeholder="Downpayment" name="Downpayment">
      </div>

      <div>
        <strong>{{ downPayment }} is {{ downpaymentPercentage }}% of the purchase price.</strong>
      </div>
        <div>
            <label for="address">Address: </label>
            <input v-model="address" type="text" placeholder="Enter an address" name="address">
            <ul>
              <li>
                  <a href="https://www.redfin.com/city/5155/CO/Denver/filter/property-type=house,min-price=50k,max-price=300k,min-beds=3,viewport=39.99003:39.53293:-104.55196:-105.15827,no-outline" target="_blank">Denver 3BD+ under $300K</a>
              </li>
            </ul>
            <p>Property Tax search tool: </p> <a v-if="address" :href="`https://google.com/search?q=${address} + assessor`" target="_blank">{{ address }} parcel information</a>
            <div>
              <a href="http://gisapp.adcogov.org/quicksearch/QuickSearch.aspx">Adams County Tax Assesor</a>
            </div>

          </div>
          <div class="monthly-yearly-numbers">
            <p>Purchase Price is: ${{ purchasePrice }}</p>
            <p>Downpayment is: ${{ downPayment }}</p>
            <h5>Yearly Expenses</h5>
            <p>Maint: {{ yearlyMaintenance }}
            <p>Taxes: {{ yearlyPropertyTax }}</p>
            <p>Insurance: {{ yearlyInsurance }}</p>
            <p>Vacany: {{ yearlyVacancyExpense }}</p>
            <p>property management: {{ yearlyPropertyManagementExpense }}</p>
            <strong>Total Yearly Expenses: {{ totalYearlyExpenses }}</strong>
            <h5>Monthly Expenses</h5>
            <p>Maint: {{ monthlyMaintenance }}</p>
            <p>Taxes: {{ monthlyPropertyTax }}</p>
            <p>Insurance: {{ monthlyInsurance }}</p>
            <p>Vacany: {{ monthlyVacancyExpense }}</p>
            <p>Property managment: {{ monthlyPropertyManagementExpense }}</p>
            <strong>Total Monthly Expenses: {{ totalMonthlyExpenses }}</strong>
            <h5>Monthly Income</h5>
            <p>Total monthly income: {{ totalMonthlyRent }}</p>
            <h5>Yearly Income</h5>
            <p>Gross yearly income: {{ grossYealryRent }}</p>
        </div>
    </div>
 
    <div class="col-30 operating-costs">
      <h2>Ongoing operating costs</h2>
      <div>
        <label for="propertyTax">Property Tax</label>
        <input v-model.number="yearlyPropertyTax" name="propertyTax" type="number" placeholder="Property Tax">
      </div>
      <div>
        <label for="monthlyInsurance">Monthly Insurance</label>
        <input v-model.number="monthlyInsurance" name="monthlyInsurance" type="number" placeholder="monthlyInsurance">
        <small>
          Average insurance for CO is .33% of the purchase price.{{ this.purchasePrice / 12 * (.0033) }}
        </small>
      </div>

      <div>
        <label for="monthlyMaintenance">Monthly Maintenance</label>
        <input v-model.number="monthlyMaintenance" name="monthlyMaintenance" type="number" placeholder="monthlyMaintenance">
        <small>That is {{ ((monthlyMaintenance * 12) / purchasePrice) * 100 }} of the purchase price <span v-if="monthlyRent">and {{ (monthlyMaintenance / monthlyRent) * 100 }}% of the rent.</span></small>
        <small>A general rule is 1% of the property value per year or 5-10% of the rent.</small>
      </div>
      
      <div>
        <label for="vacancyRate">Vacancy Rate (%)</label>
        <input v-model.number="vacancyRate" name="vacancyRate" type="number" placeholder="vacancy Rate">
        {{ monthlyVacancyExpense }}
      </div>

      <div>
        <label for="propertyManagement">Property Management (%)</label>
        <input v-model.number="managementFee" name="propertyManagement" type="number" placeholder="management Fee">
      </div>

    </div>
    <div class="col-30 income">
      <h4>Income</h4>
      <div>
        <label for="monthlyRent">Monthly Rent ($)</label>
        <input v-model.number="monthlyRent" name="monthlyRent" type="number" placeholder="Monthly Rent">
        <small>Rental Estiamtes:</small> <a href="https://www.rentometer.com/" target="_blank">https://www.rentometer.com</a>
      </div>
      <div>
        <label for="monthlymortgage">Mortage</label>
        <input v-model.number="monthlymortgage" name="monthlymortgage" type="number" placeholder="monthlymortgage">
      </div>
    </div>
    <div class="col-30 calculations">
      <h4>Calculations</h4>
      <p>Monthly NOI: {{ monthlyNetOperatingIncome }}</p>
      <p>Yearly NOI: {{ yearlyNetOperatingIncome }}</p>
      <h5>Debt Service</h5>
      <p>Monlthy Mortage: {{ monthlymortgage }}</p>
      <p> Yearly Mortage: {{ yearlyMortage }}</p>
      <h5>Cap rate: </h5>
      <p>Cap Rate: {{ capRate }}</p>
      <p>Cash On Cash: {{ cashOnCash }}</p>
      <p>Monthly Cash Flow with Mortage: {{ monthlyCashFlowWithMortage }}</p>
      <p>Yearly Cash Flow with Mortage: {{ yearlyCashFlowWithMortage }}</p>

    </div>
    <div class="mortgage-container">
      <iframe :src="`https://www.calculator.net/mortgage-calculator.html?chouseprice=${purchasePrice}&cdownpayment=${downpaymentPercentage}&cdownpaymentunit=p&cloanterm=30&cinterestrate=${interestRate}&cstartmonth=3&cstartyear=2020&caddoptional=0&cpropertytaxes=0&cpropertytaxesunit=p&chomeins=0&chomeinsunit=d&cpmi=0&cpmiunit=d&choa=0&choaunit=d&cothercost=0&cothercostunit=d&cmop=0&cptinc=0&chiinc=0&choainc=0&cocinc=0&cexma=0&cexmsm=3&cexmsy=2020&cexya=0&cexysm=3&cexysy=2020&cexoa=0&cexosm=3&cexosy=2020&caot=0&xa1=0&xm1=3&xy1=2020&xa2=0&xm2=3&xy2=2020&xa3=0&xm3=3&xy3=2020&xa4=0&xm4=3&xy4=2020&xa5=0&xm5=3&xy5=2020&xa6=0&xm6=3&xy6=2020&xa7=0&xm7=3&xy7=2020&xa8=0&xm8=3&xy8=2020&xa9=0&xm9=3&xy9=2020&xa10=0&xm10=3&xy10=2020&csbw=1&printit=1`"
      border="0"
      width="800"
      height="500"
      id="mortageID"
      ></iframe>
      <a :href="`https://www.calculator.net/mortgage-calculator.html?chouseprice=${purchasePrice}&cdownpayment=${downpaymentPercentage}&cdownpaymentunit=p&cloanterm=30&cinterestrate=${interestRate}&cstartmonth=3&cstartyear=2020&caddoptional=0&cpropertytaxes=0&cpropertytaxesunit=p&chomeins=0&chomeinsunit=d&cpmi=0&cpmiunit=d&choa=0&choaunit=d&cothercost=0&cothercostunit=d&cmop=0&cptinc=0&chiinc=0&choainc=0&cocinc=0&cexma=0&cexmsm=3&cexmsy=2020&cexya=0&cexysm=3&cexysy=2020&cexoa=0&cexosm=3&cexosy=2020&caot=0&xa1=0&xm1=3&xy1=2020&xa2=0&xm2=3&xy2=2020&xa3=0&xm3=3&xy3=2020&xa4=0&xm4=3&xy4=2020&xa5=0&xm5=3&xy5=2020&xa6=0&xm6=3&xy6=2020&xa7=0&xm7=3&xy7=2020&xa8=0&xm8=3&xy8=2020&xa9=0&xm9=3&xy9=2020&xa10=0&xm10=3&xy10=2020&csbw=1&printit=0`" target="_blank">Mortage Link Results</a>
    </div>  
  </section>
</template>
   
<script>
export default {
  name: 'rentalCalc',
  props: {
    msg: String
  },
  computed: {
    //Monthly Calculations
    totalMonthlyExpenses() {
      return this.totalYearlyExpenses / 12;
    },
    monthlyNetOperatingIncome() {
      return this.totalMonthlyRent - this.totalMonthlyExpenses;
    },
    monthlyVacancyExpense() {
      return ((this.vacancyRate / 100) * this.monthlyRent);
    },
    totalMonthlyRent() {
      return this.monthlyRent;
    },
    monthlyPropertyManagementExpense() {
      return this.totalMonthlyRent * (this.managementFee / 100);
    },
    monthlyPropertyTax() {
      return this.yearlyPropertyTax / 12;
    },
    monthlyCashFlowWithMortage() {
      return this.monthlyNetOperatingIncome - this.monthlymortgage;
    },
    //Yearly Calculations
    yearlyMaintenance() {
      return this.monthlyMaintenance * 12;
    },
    yearlyInsurance() {
      return this.monthlyInsurance * 12;
    },
    yearlyPropertyManagementExpense() {
      return this.monthlyPropertyManagementExpense * 12;
    },
    yearlyMortage() {
      return this.monthlymortgage * 12;
    },
    grossYealryRent() {
      return this.monthlyRent * 12;
    },
    yearlyNetOperatingIncome() {
      return this.grossYealryRent - this.totalYearlyExpenses;
    },
    totalYearlyExpenses() {
      return ((this.monthlyMaintenance * 12) + this.yearlyPropertyTax + (this.monthlyInsurance * 12) + (this.monthlyVacancyExpense * 12) + (this.monthlyPropertyManagementExpense * 12));
    },
    yearlyCashFlowWithMortage() {
      return this.yearlyNetOperatingIncome - this.yearlyMortage;
    },
    //Valuation Calculations
    capRate() {
      return (this.yearlyNetOperatingIncome / this.purchasePrice) * 100; 
    },
    cashOnCash() {
      return ((this.yearlyNetOperatingIncome - this.yearlyMortage) / this.downPayment) * 100;
    },
    downpaymentPercentage() {
      return this.downPayment / this.purchasePrice * 100
    },

  },
  data() {
    return {
      //General House information
      address: '',
      //Purchase Expenses
      purchasePrice: 94000,
      downPayment: 18800,
      interestRate: 4,
      closingCosts: 2500,
      //Recurring Operating expenses
      monthlyInsurance: 43,
      hoa: null,
      monthlyMaintenance: 70,
      otherCosts: null,
      yearlyPropertyTax: 1700,
      monthlymortgage: 365,
      //Income
      monthlyRent: 1000,
      vacancyRate: 5,
      managementFee: 10
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input, label ,p ,h1 ,h2 ,h3 ,h4, h5, small {
  color: #515151; 
  font-family: 'Calibri';
}
input {
  border: 2px solid green;
}

p {
  font-size: 16px;
}
label, input {
  font-size: 16px;
  padding: 5px;
  margin: 5px;
}
.col-30 {
  border: 1px solid #DBDBDB;
  border-top-color: #D1CFFF;
  padding: 10px;
  display: inline-block;
  width: 25%;
  margin:15px;
  vertical-align: top;
}

.mortgage-container {
}
</style>
