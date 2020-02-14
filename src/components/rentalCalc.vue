<template>
  <section>
    <div class="purchase">
      <h1>Paremeters</h1>
      <a href="https://www.redfin.com/city/4147/CO/Colorado-Springs/filter/property-type=house,min-price=50k,max-price=250k,min-beds=3,viewport=39.03711124967014:38.713887348714714:-104.59480259820465:-104.91973715762957" target="_blank">Colorado Springs 3BD/2BA under $250K</a>
      <h1>Address</h1>
      <input v-model="address" type="text" placeholder="Enter an address">
      <h1>Purchase Price</h1>
      <input v-model.number="purchasePrice"  type="number" placeholder="purchasePrice">
      <p>Purchase Price is: {{ purchasePrice }}</p>

      <input v-model.number="downPayment"  type="number" placeholder="Downpayment">
      <p>Downpayment is: {{ downPayment }}</p>
      <p>{{ downPayment }} is {{ downpaymentCalculated }}% of the purchase price</p>
      <p v-if="downpaymentCalculated < 20">PMI is typtically required for properties with less than 20% down.</p>
    </div>
    <div class="operating-costs">
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

       <h4>Vacancy Rate</h4>
      <input v-model.number="vacancyRate" type="number" placeholder="vacancy Rate">

       <h4>Property Management</h4>
      <input v-model.number="managementFee" type="number" placeholder="management Fee">

    </div>

    <div class="income">
      <h4>Income</h4>
      <h5>Monthly Rent</h5>
      <input v-model.number="monthlyRent" type="number" placeholder="Monthly Rent">

      <p>Expect monthly rent less vacancy: {{ totalMonthlyRent }}</p>
      <small>Rental Estiamtes:</small> <a href="https://www.rentometer.com/" target="_blank">https://www.rentometer.com</a>

      <p>Property Management</p>
      {{ propertyManagementCut }}
    </div>

    <div class="mortgage">
      <a href="http://clients.ryanstratton.com/mortgage-calculator/" target="_blank">http://clients.ryanstratton.com/mortgage-calculator/</a>
      <input v-model.number="mortgage" type="number" placeholder="mortgage">
      <p>Mortage payment: {{ mortgage }}</p>
      <p>Yearly: {{ mortgage * 12}}</p>
    </div>
    <div class="add">
      <h5>Total Income</h5>
      Monthly: {{ totalIncome }}
      Yearly: {{ totalIncome * 12 }}
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

    <div>
      <div v-if="monthlyAmountWithMortgage > 1"> <img src="../assets/yes.jpg" alt=""></div>
      <div v-if="monthlyAmountWithMortgage < 1"> <img src="../assets/no.jpg" alt=""></div>
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
      return this.maintenance + this.propertyTax + this.estimatedInsurance;
    },
    estimatedInsurance() {
      return this.purchasePrice * (.0033);
    },
    downpaymentCalculated() {
      return this.downPayment / this.purchasePrice * 100
    },
    totalMonthlyRent() {
      return ((1 - (this.vacancyRate / 100)) * this.monthlyRent);
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

div {
  margin: 5px;
}
.purchase {
  display: inline-block;
  width: 30%;
  padding: 5px;
}
.operating-costs {
  display: inline-block;
  width: 30%;
  padding: 5px;

}
.income {
  display: inline-block;
  width: 30%;
  padding: 5px;

}

.add {
  display: inline-block;
  width: 30%;
  border: 1px solid green;
  padding: 5px;
}
</style>
