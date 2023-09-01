<template>
  <div
    class="md:py-10 md:pl-20 md:pr-16 md:w-full mx-5 -mt-20 md:mx-0 md:mt-0 rounded-lg bg-white md:bg-none px-6 py-8 self-center absolute md:relative"
  >
    <div>
      <h1 class="text-4xl text-[#022857] font-bold">Finishing Up</h1>
      <p class="text-[#B9BABE] mt-2 font-medium">
        Double-check everything looks OK before confirming.
      </p>
      <div class="mt-10 md:w-[120%]">
        <div class="bg-[#F8F9FE] p-4 rounded-lg">
          <div class="flex justify-between" v-if="selectedPlan">
            <div>
              <p class="text-[#022857] font-bold">
                {{ selectedPlan.name }}
                <span>({{ activeOption }})</span>
              </p>
              <nuxt-link to="/plan">
                <p class="underline text-[#b9babe] mt-1 hover:text-[#6E69C3]">
                  change
                </p>
              </nuxt-link>
            </div>
            <p class="text-[#022857] font-bold">
              ${{ formatPrice(selectedPlan.price) }}
            </p>
          </div>
          <hr class="my-4" />
          <div
            class="flex justify-between my-2"
            v-for="(selectedAddOn, index) in selectedAddOns"
            :key="index"
          >
            <p class="text-[#b9babe] font-semi-bold">
              {{ selectedAddOn.title }}
            </p>
            <p class="text-[#55667C] font-semi-bold">
              +${{ formatPrice(selectedAddOn.price) }}
            </p>
          </div>
          <div class="flex justify-between"></div>
        </div>
        <div class="flex justify-between px-4 py-5">
          <p class="text-[#b9babe] font-semi-bold">Total(per {{ per }})</p>
          <p class="font-bold text-[#6E69C3] text-lg">
            ${{ formatPrice(total) }}
          </p>
        </div>
      </div>
      <div class="mt-24 flex justify-between w-full md:w-[120%]">
        <nuxt-link to="/add-ons" class="self-start"
          ><p class="text-[#B8BBC4] font-medium mt-2">Go Back</p></nuxt-link
        >
        <button
          @click="save"
          class="bg-[#6E69C3] text-white w-32 rounded py-3 font-medium self-end"
        >
          Confirm
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      activeOption: "",
      per: "",
      selectedPlan: null,
      selectedAddOns: [],
    };
  },
  mounted() {
    const storedBillingFrequency = localStorage.getItem("billingOption");
    if (storedBillingFrequency) {
      this.activeOption = storedBillingFrequency;
    }
    // Retrieve the selected plan details from local storage
    const selectedPlanData = localStorage.getItem("selectedPlan");
    if (selectedPlanData) {
      this.selectedPlan = JSON.parse(selectedPlanData);
    }

    const selectedAddOnsData = localStorage.getItem("checkedItems");
    if (selectedAddOnsData) {
      this.selectedAddOns = JSON.parse(selectedAddOnsData);
    }
    if (this.activeOption === "yearly") {
      this.per = "year";
    } else {
      this.per = "month";
    }
  },
  methods: {
    formatPrice(price) {
      // console.log(this.activeOption);
      if (this.activeOption === "yearly") {
        // Replace "/mo" with "/yr" at the end of the price
        return price * 10 + "/yr";
      }
      return price + "/mo"; // Add "/mo" for monthly billing
    },
    save() {
      this.$router.push("/summary/thank-you");
    },
  },
  computed: {
    total() {
      let totalPrice = 0;

      // Check if selectedPlan is defined and has a price greater than 0
      if (this.selectedPlan && parseFloat(this.selectedPlan.price) > 0) {
        totalPrice += parseFloat(this.selectedPlan.price);
      }

      // Iterate through selectedAddOns and add their prices
      for (const selectedAddOn of this.selectedAddOns) {
        if (parseFloat(selectedAddOn.price) > 0) {
          totalPrice += parseFloat(selectedAddOn.price);
        }
      }

      // If totalPrice is still 0, return 0, otherwise return the total with 2 decimal places
      return totalPrice === 0 ? 0 : totalPrice.toFixed(2);
    },
  },
};
</script>
