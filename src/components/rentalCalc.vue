<template>
  <section class="container">
    <div class="col-30 purchase">
      <h1>Investment Summary</h1>
        <div>
          <label for="purchasePrice">Purchase Price</label>
          <input v-model.number="purchasePrice"  type="number" placeholder="purchasePrice" name="purchasePrice">
        </div>
        <div>
          <label for="upfrontCost">Upfront Cost</label>
          <input v-model.number="upfrontCost"  type="number" placeholder="upfrontCost" name="upfrontCost">
        </div>

        <div>
          <label for="monthlyRent">Rent Estimate</label>
          <input v-model.number="monthlyRent" name="monthlyRent" type="number" placeholder="Monthly Rent">
        </div>
      <div>
        <p>Monthly NOI: {{ monthlyNetOperatingIncome }}</p>
        <p>Cap Rate: {{ capRate }}</p>
        <p>Cash On Cash: {{ cashOnCash }}</p>
      </div> 

    </div>

    <div class="col-30 operating-costs">
      <h2>Ongoing operating costs</h2>
      <div>
        <label for="propertyTax">Property Taxes</label>
        <input v-model.number="yearlyPropertyTax" name="propertyTax" type="number" placeholder="Property Tax">/year
      </div>
      <div>
        <label for="monthlyInsurance">Insurance</label>
        <input v-model.number="monthlyInsurance" name="monthlyInsurance" type="number" placeholder="monthlyInsurance">/month
      </div>

      <div>
        <label for="monthlyMaintenance">Maintenance</label>
        <input v-model.number="monthlyMaintenance" name="monthlyMaintenance" type="number" placeholder="monthlyMaintenance">/month
      </div>
      
      <div>
        <label for="vacancyRate">Vacancy Expense</label>
        <input v-model.number="vacancyRate" name="vacancyRate" type="number" placeholder="vacancy Rate">
        {{ monthlyVacancyExpense }}
      </div>

      <div>
        <label for="propertyManagement">Property Management Expense</label>
        <input v-model.number="managementFee" name="propertyManagement" type="number" placeholder="management Fee">
      </div>
      <strong>Total Monthly Expenses: {{ totalMonthlyExpenses }}</strong>
      <div>
        <label for="monthlymortgage">Mortgage</label>
        <input v-model.number="monthlymortgage" name="monthlymortgage" type="number" placeholder="monthlymortgage">/month
      </div>
      <div>
        <label for="monthlyPmi">PMI</label>
        <input v-model.number="monthlyPmi" name="monthlyPmi" type="number" placeholder="monthly Pmi">/month
        <p>.75% of loan amount: {{ (purchasePrice - downPayment) * .0075 / 12}}</p>
      </div>

    </div>
    <div class="col-30">
      <h2>Acquisiton Costs</h2>
      <div>
        <label for="downPayment">Down Payment</label>
        <input v-model.number="downPayment"  type="number" placeholder="Downpayment" name="Downpayment">
      </div>
      <div>
        <label for="closingCosts">Closing Costs</label>
        <input v-model.number="closingCosts"  type="number" placeholder="Closing Costs" name="closingCosts">
      </div>
      <div>
        <label for="interestRate">Interest Rate</label>
        <input v-model.number="interestRate"  type="number" placeholder="Interest Rate" name="interestRate">
      </div>
      <div>
        <label for="downpaymentPercentage">Downpayment Percentage</label>
        <input v-model.number="downpaymentPercentage"  type="number" placeholder="Downpayment Percentage" name="downpaymentPercentage">
      </div>
    </div>
    <div class="col-30">
      <h4>Cash Flow Calculations</h4>
      <p>Short TermMonthly income after PITI: {{ monthlyAfterPiti }}</p>
      <p>Medium Term: {{ monthlyCashFlowWithMorgtage }}</p>
      <p>Long Term: {{ longTermCashFlow }}</p>

      <p></p>
    </div>
    <div class="col-50">
        <div class="mortgage-container">
        <iframe :src="`https://www.calculator.net/mortgage-calculator.html?chouseprice=${purchasePrice}&cdownpayment=${downpaymentPercentage}&cdownpaymentunit=p&cloanterm=30&cinterestrate=${interestRate}&cstartmonth=3&cstartyear=2020&caddoptional=0&cpropertytaxes=0&cpropertytaxesunit=p&chomeins=0&chomeinsunit=d&cpmi=0&cpmiunit=d&choa=0&choaunit=d&cothercost=0&cothercostunit=d&cmop=0&cptinc=0&chiinc=0&choainc=0&cocinc=0&cexma=0&cexmsm=3&cexmsy=2020&cexya=0&cexysm=3&cexysy=2020&cexoa=0&cexosm=3&cexosy=2020&caot=0&xa1=0&xm1=3&xy1=2020&xa2=0&xm2=3&xy2=2020&xa3=0&xm3=3&xy3=2020&xa4=0&xm4=3&xy4=2020&xa5=0&xm5=3&xy5=2020&xa6=0&xm6=3&xy6=2020&xa7=0&xm7=3&xy7=2020&xa8=0&xm8=3&xy8=2020&xa9=0&xm9=3&xy9=2020&xa10=0&xm10=3&xy10=2020&csbw=1&printit=1`"
        border="0"
        width="800"
        height="500"
        id="MorgtageID"
        ></iframe>
        <a :href="`https://www.calculator.net/mortgage-calculator.html?chouseprice=${purchasePrice}&cdownpayment=${downpaymentPercentage}&cdownpaymentunit=p&cloanterm=30&cinterestrate=${interestRate}&cstartmonth=3&cstartyear=2020&caddoptional=0&cpropertytaxes=0&cpropertytaxesunit=p&chomeins=0&chomeinsunit=d&cpmi=0&cpmiunit=d&choa=0&choaunit=d&cothercost=0&cothercostunit=d&cmop=0&cptinc=0&chiinc=0&choainc=0&cocinc=0&cexma=0&cexmsm=3&cexmsy=2020&cexya=0&cexysm=3&cexysy=2020&cexoa=0&cexosm=3&cexosy=2020&caot=0&xa1=0&xm1=3&xy1=2020&xa2=0&xm2=3&xy2=2020&xa3=0&xm3=3&xy3=2020&xa4=0&xm4=3&xy4=2020&xa5=0&xm5=3&xy5=2020&xa6=0&xm6=3&xy6=2020&xa7=0&xm7=3&xy7=2020&xa8=0&xm8=3&xy8=2020&xa9=0&xm9=3&xy9=2020&xa10=0&xm10=3&xy10=2020&csbw=1&printit=0`" target="_blank">Morgtage Link Results</a>
      </div>
    </div>
      <div class="col-30">
      <h4>Final Report</h4>
      <div class="monthly-yearly-numbers">
        <h5>Yearly Income</h5>
        <p>Gross yearly income: {{ grossYealryRent }}</p>
        <h5>Yearly Expenses</h5>
        <p>Property Taxes: {{ yearlyPropertyTax }}</p>
        <p>Property Insurance: {{ yearlyInsurance }}</p>
        <p>Maintenance: {{ yearlyMaintenance }}
        <p>Vacany: {{ yearlyVacanyExpense }}</p>
        <p>Property Management: {{ yearlyPropertyManagementExpense }}</p>
        <strong>Yearly Expenses without mortgage: {{ totalYearlyExpenses }}</strong>
        <p>Yearly Morgtage: {{ yearlyMorgtage }}</p>
        <strong>Yearly Expenses with mortgage: {{ yearlyExpensesWithMortage }}</strong>
        <hr>
        <h5>Monthly Income</h5>
        <p>Total monthly income: {{ totalMonthlyRent }}</p>
        <h5>Monthly Expenses</h5>
        <p>Maint: {{ monthlyMaintenance }}</p>
        <p>Taxes: {{ monthlyPropertyTax }}</p>
        <p>Insurance: {{ monthlyInsurance }}</p>
        <p>Vacany: {{ monthlyVacancyExpense }}</p>
        <p>Property managment: {{ monthlyPropertyManagementExpense }}</p>
        <strong>Total Monthly Expenses without mortgage: {{ totalMonthlyExpenses }}</strong>
        <p>Monlthy Morgtage: {{ monthlymortgage }}</p>
        <strong>Total Monthly Expenses with mortgage: {{ monthlyExpensesWithMortage }}</strong>
    </div>
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
    monthlyAfterPiti() {
      return this.monthlyRent - this.monthlymortgage - this.monthlyPropertyTax - this.monthlyPropertyTax;
    },
    longTermCashFlow() {
      return (this.monthlyRent / 2) - this.monthlymortgage;
    },
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
    monthlyCashFlowWithMorgtage() {
      return this.monthlyNetOperatingIncome - this.monthlymortgage;
    },
    monthlyExpensesWithMortage() {
      return this.totalMonthlyExpenses + this.monthlymortgage;
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
    yearlyMorgtage() {
      return this.monthlymortgage * 12;
    },
    yearlyVacanyExpense() {
      return this.monthlyVacancyExpense * 12;
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
    yearlyCashFlowWithMorgtage() {
      return this.yearlyNetOperatingIncome - this.yearlyMorgtage;
    },
    yearlyExpensesWithMortage() {
      return this.totalMonthlyExpenses + this.yearlyMorgtage;
    },
    //Valuation Calculations
    capRate() {
      return (this.yearlyNetOperatingIncome / this.purchasePrice) * 100; 
    },
    cashOnCash() {
      return ((this.yearlyNetOperatingIncome - this.yearlyMorgtage) / this.downPayment) * 100;
    },
    downpaymentPercentage() {
      return this.downPayment / this.purchasePrice * 100
    },
    upfrontCost() {
      return this.downPayment + this.closingCosts;
    },
    closingCosts() {
      return this.purchasePrice * .03;
    }
  },
  data() {
    return {
      //General House information
      address: '',
      //Purchase Expenses
      purchasePrice: 94000,
      downPayment: 18800,
      interestRate: 4,
      //Recurring Operating expenses
      monthlyInsurance: 43,
      hoa: null,
      monthlyMaintenance: 70,
      otherCosts: null,
      yearlyPropertyTax: 1700,
      monthlymortgage: 365,
      monthlyPmi: 0,
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

.col-50 {
  border: 1px solid #DBDBDB;
  border-top-color: #D1CFFF;
  padding: 10px;
  display: inline-block;
  width: 50%;
  margin:15px;
  vertical-align: top;
}

</style>
