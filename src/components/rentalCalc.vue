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
        <p>Purchase Price is: ${{ purchasePrice }}</p>
        <p>Downpayment is: ${{ downPayment }}</p>
        <strong>{{ downPayment }} is {{ downpaymentCalculated }}% of the purchase price.</strong>
      </div>
        <div>
            <h3>Preset Criteria</h3>
            <label for="address">Address: </label>
            <input v-model="address" type="text" placeholder="Enter an address" name="address">
            <ul>
              <li>
                <a href="https://www.redfin.com/city/4147/CO/Colorado-Springs/filter/property-type=house,min-price=50k,max-price=250k,min-beds=3,viewport=39.03711124967014:38.713887348714714:-104.59480259820465:-104.91973715762957" target="_blank">Colorado Springs 3BD/2BA under $250K</a>
              </li>
              <li>
                  <a href="https://www.redfin.com/city/5155/CO/Dencol/filter/property-type=house,min-price=50k,max-price=300k,min-beds=3,viewport=39.99003:39.53293:-104.55196:-105.15827,no-outline" target="_blank">Denver 3BD+ under $300K</a>
              </li>
            </ul>

          </div>
          <div class="final-output">
            <h5>Total Income</h5>
            Monthly: {{ totalIncome }}
            Yearly: {{ totalIncome  }}
            <h5>Total expenses</h5>
            Monthly: {{ totalExpenses / 12 }}
            Yearly: {{ totalExpenses }}
            <h5>NOI (not including Mortgage)</h5>
            Monthly: {{ totalIncome - (totalExpenses / 12) }}
            Yearly: {{ totalIncome * 12 - totalExpenses }}

            <h5>WITH Mortgage</h5>
            Monthly: {{ (totalIncome - (totalExpenses / 12)) - mortgage }}
            Yearly: {{ (totalIncome * 12 - (totalExpenses)) - (mortgage * 12) }}
        </div>
    </div>
 
    <div class="col-30 operating-costs">
      <h2>Ongoing operating costs</h2>
      <h3>Property Tax</h3>
      <input v-model.number="propertyTax" type="number" placeholder="Property Tax">
      <p>Property Tax search tool: </p> <a :href=" `https://property.spatialest.com/co/elpaso/#/search?page=1&term=${address}`" target="_blank">{{ address }} parcel information</a>
      <p>Monthly property tax is: {{ propertyTax / 12 }}</p>
      <p>Yearly property tax is: {{ propertyTax }}</p>

      <h3>Insurance</h3>
      <input v-model.number="estimatedInsurance" type="number" placeholder="estimatedInsurance">
        Average insurance for CO is .33%
      <p>Monthly insurance is: {{ estimatedInsurance / 12 }}</p>
      <p>Yearly insurance is: {{ estimatedInsurance }}</p>

      <h4>Maintenance</h4>
      <input v-model.number="maintenance" type="number" placeholder="maintenance">
      <p>Monthly maintenance is: {{ maintenance / 12 }}</p>
      <p>Yearly maintenance is: {{ maintenance }}</p>
      <p>That is {{ (maintenance / purchasePrice) * 100 }} of the purchase price.</p>
      <small>A general rule is 1% of the property value per year or 5-10% of the rent.</small>

      <h4>Vacancy Rate (%)</h4>
      <input v-model.number="vacancyRate" type="number" placeholder="vacancy Rate">
      {{ vacancyExpense }}

      <h4>Property Management</h4>
      <input v-model.number="managementFee" type="number" placeholder="management Fee">

    </div>
    <div class="col-30 income">
      <h4>Income</h4>
      <h5>Monthly Rent</h5>
      <input v-model.number="monthlyRent" type="number" placeholder="Monthly Rent">

      <p>Expect monthly rent less vacancy: {{ totalMonthlyRent }}</p>
      <small>Rental Estiamtes:</small> <a href="https://www.rentometer.com/" target="_blank">https://www.rentometer.com</a>

      <p>Property Management</p>
      {{ propertyManagementCut }}

      <a href="http://clients.ryanstratton.com/mortgage-calculator/" target="_blank">http://clients.ryanstratton.com/mortgage-calculator/</a>
      <input v-model.number="mortgage" type="number" placeholder="mortgage">
      <p>Mortage payment: {{ mortgage }}</p>
      <p>Yearly: {{ mortgage * 12}}</p>
    </div>
    <div style="display:block;overflow:hidden;width:800px;">
      <iframe :src="`https://www.calculator.net/mortgage-calculator.html?chouseprice=${purchasePrice}&cdownpayment=${downpaymentCalculated}&cdownpaymentunit=p&cloanterm=30&cinterestrate=${interestRate}&cstartmonth=3&cstartyear=2020&caddoptional=0&cpropertytaxes=0&cpropertytaxesunit=p&chomeins=0&chomeinsunit=d&cpmi=0&cpmiunit=d&choa=0&choaunit=d&cothercost=0&cothercostunit=d&cmop=0&cptinc=0&chiinc=0&choainc=0&cocinc=0&cexma=0&cexmsm=3&cexmsy=2020&cexya=0&cexysm=3&cexysy=2020&cexoa=0&cexosm=3&cexosy=2020&caot=0&xa1=0&xm1=3&xy1=2020&xa2=0&xm2=3&xy2=2020&xa3=0&xm3=3&xy3=2020&xa4=0&xm4=3&xy4=2020&xa5=0&xm5=3&xy5=2020&xa6=0&xm6=3&xy6=2020&xa7=0&xm7=3&xy7=2020&xa8=0&xm8=3&xy8=2020&xa9=0&xm9=3&xy9=2020&xa10=0&xm10=3&xy10=2020&csbw=1&printit=1`"
      border="0"
      width="800"
      height="500"
      ></iframe>
      <a :href="`https://www.calculator.net/mortgage-calculator.html?chouseprice=${purchasePrice}&cdownpayment=${downpaymentCalculated}&cdownpaymentunit=p&cloanterm=30&cinterestrate=${interestRate}&cstartmonth=3&cstartyear=2020&caddoptional=0&cpropertytaxes=0&cpropertytaxesunit=p&chomeins=0&chomeinsunit=d&cpmi=0&cpmiunit=d&choa=0&choaunit=d&cothercost=0&cothercostunit=d&cmop=0&cptinc=0&chiinc=0&choainc=0&cocinc=0&cexma=0&cexmsm=3&cexmsy=2020&cexya=0&cexysm=3&cexysy=2020&cexoa=0&cexosm=3&cexosy=2020&caot=0&xa1=0&xm1=3&xy1=2020&xa2=0&xm2=3&xy2=2020&xa3=0&xm3=3&xy3=2020&xa4=0&xm4=3&xy4=2020&xa5=0&xm5=3&xy5=2020&xa6=0&xm6=3&xy6=2020&xa7=0&xm7=3&xy7=2020&xa8=0&xm8=3&xy8=2020&xa9=0&xm9=3&xy9=2020&xa10=0&xm10=3&xy10=2020&csbw=1&printit=0`" target="_blank">Mortage Link Results</a>
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
    totalIncome() {
      return this.totalMonthlyRent - this.propertyManagementCut;
    },
    monthlyAmountWithMortgage() {
      return (this.totalIncome - (this.totalExpenses / 12) - this.mortgage );
    },
    totalExpenses() {
      return (this.maintenance + this.propertyTax + this.estimatedInsurance);
    },
    estimatedInsurance() {
      return this.purchasePrice * (.0033) || this.insurance;
    },
    downpaymentCalculated() {
      return this.downPayment / this.purchasePrice * 100
    },
    vacancyExpense() {
      return ((this.vacancyRate / 100)* this.monthlyRent);
    },
    totalMonthlyRent() {
      return this.monthlyRent;
    },
    propertyManagementCut() {
      return this.totalMonthlyRent * (this.managementFee / 100);
    }
  },
  data() {
    return {
      //General House information
      address: '',
      //Purchase Expenses
      purchasePrice: 250000,
      downPayment: 25000,
      interestRate: 4,
      closingCosts: 2500,
      //Recurring Operating expenses
      propertyTax: null,
      insurance: null,
      hoa: null,
      maintenance: null,
      otherCosts: null,
      mortgage: null,
      //Income
      monthlyRent: null,
      vacancyRate: null,
      managementFee: null
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
  font-size: 22px;
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
  width: 30%;
  margin:15px;
  vertical-align: top;
}

.final-output {
}
</style>
