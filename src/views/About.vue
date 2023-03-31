<template>

  <h3>Payment</h3>
<!--  <div v-if="error" class="alert alert-danger">{{ error }}</div>-->
  <error-c :message="error"/>
  <form novalidate @submit.prevent="onSave">
    <div class="row">
      <div class="col-md-6">
        <div><strong>Shipping Information</strong></div>
        <div class="form-group">
          <label for="name">Name</label>
          <input id="name" type="text" class="form-control" placeholder="Your Name" v-model="payment.shipping.fullname">
        </div>
        <div class="form-group">
          <label for="company">Company Name</label>
          <input id="company" type="text" class="form-control" placeholder="Comapny Name"
                 v-model="payment.shipping.companyname">
        </div>


        <div class="form-group">
          <label for="adress1">Adress</label>
          <input id="adress1" type="text" class="form-control" placeholder="Street Adress"
                 v-model="payment.shipping.adress1">
        </div>
        <div class="form-group">
          <label for="adress2">Suite/Apartment</label>
          <input
              id="adress2"
              type="text"
              class="form-control"
              placeholder="Comapny Name"
              v-model="payment.shipping.adress2">
        </div>

        <div class="form-row">
          <div class="form-group col-md-2">
            <label for="cityTown">City</label>
            <input id="cityTown" type="text" class="form-control" placeholder="e.g. New York"
                   v-model="payment.shipping.cityTown">
          </div>
          <div class="form-group col-md-2">
            <label for="stateProvince">State</label>
            <select id="stateProvince" class="form-control" v-model="payment.shipping.stateProvice">
              <option v-for="s in states" :key="s.abberviation">{{ s.name }}</option>
              <option v-for="s in states" :key="s.abberviation">{{ stateFormat(s) }}</option>
              <!--              <option v-for="s in states" :key="s.abberviation" :value="s.abberviation">{{s.name}}</option>-->
            </select>
          </div>
          <div class="form-group col-md-2">
            <label for="postalCode">Zip Code</label>
            <input id="postalCode" type="text" class="form-control" placeholder="e.g. Newj York"
                   v-model="payment.shipping.postalCode">
          </div>
          <div class="form-group">
            <input type="submit" value="Next" class="btn btn-success"/>
          </div>
        </div>
      </div>
      <div class="col-md-6">
        <div><strong>Billing Information</strong></div>
        <div class="form-check">
          <input type="checkbox" id="sameAsShipping" class="form-check-input" v-model="payment.billing.sameAsShipping"/>
          <label for="sameAsShipping" class="form-check-label">Same As Shipping</label>
        </div>
        <div class="form-group col-md-6 ">
          <label for="adress1">Adress</label>
          <input id="adress1"
                 type="text"
                 placeholder="Stret Adress"
                 v-model="payment.billing.adress1"
                 :disabled="payment.billing.sameAsShipping">
        </div>

        <div><strong>Credit Card</strong></div>
        <div class="form-group">
          <label for="ccNumber">Credit Card Number</label>
          <input
              class="form-control"
              type="text"
              id="ccNumber"
              v-model="payment.creditcard.number"
          >
        </div>
        <div class="form-group">
          <label for="nameOnCard">Name on Card</label>
          <input
              class="form-control"
              type="text"
              id="nameOnCard"
              v-model="payment.creditcard.nameOnCard"
          >
        </div>
        <div class="form-row">
          <div class="form-group col-md-4">
            <label for="expirationMonth">Expiration Month</label>
            <select
                class="form-control"
                v-model="payment.creditcard.expirationMonth"
            >
              <option v-for="m in months" :key="m.number" :value="m.number">{{ m.name }}</option>
            </select>
          </div>
          <div class="form-group col-md-4">
            <label for="expirationYear">Expiration year</label>
            <select
                class="form-control"
                v-model="payment.creditcard.expirationYear"
            >
              <option v-for="y in years" :key="y" :value="y">{{ y }}</option>
            </select>
          </div>
          <div class="form-group">
            <label for="cvv">CVV Code</label>
            <input
            class="form-control"
            type="text"
            id="cvv"
            v-model="payment.creditcard.cvv"
            >
          </div>

        </div>
      </div>
    </div>
  </form>

  <div>
    <pre>{{ payment }}</pre>
  </div>
</template>
<script>
import {computed, ref, watch} from "vue";
import states from "@/lookup/states";
import formatters from "@/formatters";
import months from "@/lookup/months";
import ErrorC from "@/components/Error.vue";


export default {
  name: 'AboutM',
  components: {ErrorC},

  setup() {
    const payment = ref({
      shipping: {
        fullname: "Mateusz"
      },
      billing: {
        sameAsShipping: false
      },
      creditcard: {}
    });

    const error = ref("");

    function onSave() {
      error.value = "We can't sabe yet";
    }

    const zipcode = computed({
      get: () => payment.value.postalCode,
      set: (val) => {
        if (val && typeof val === "string") {
          if (val.length <= 5 || val.indexOf("-") > -1) {
            payment.value.postalCode = val;
          } else {
            payment.value.postalCode = `${val.substring(0, 5)}-${val.substring(5)}`;
          }
        }
      }
    });

    watch(
        //What to watch
        () => payment.value.billing.sameAsShipping,
        //What to do
        () => {
          if (payment.value.billing.sameAsShipping) {
            payment.value.billing.adress1 = "";
          }
        });

    const years = Array.from({length: 10}, (_, i) => i + 2023)

    return {
      payment,
      states,
      onSave,
      ...formatters,
      zipcode,
      error,
      months,
      years
    };
  }

}
</script>