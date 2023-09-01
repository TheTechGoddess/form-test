<template>
  <div
    class="md:py-10 md:pl-20 md:pr-16 md:w-full mx-5 -mt-20 md:mx-0 md:mt-0 rounded-lg bg-white md:bg-none px-6 py-8 self-center absolute md:relative"
  >
    <div
      class="flex flex-col justify-center items-center content-center my-16 md:mb-40"
    >
      <img src="~/assets/images/icon-thank-you.svg" alt="" class="w-24 h-24" />
      <h1 class="text-4xl text-[#022857] font-bold my-5">Thank You</h1>
      <p class="text-[#B9BABE] font-medium w-[90%]">
        Thanks for confirming your subscription! We hope you have fun using our
        platform. If you ever need support, please feel free to email us at
        support@loremgaming.com.
      </p>
      <!-- <p class="text-[#B9BABE] font-medium">
        We hope you have fun using our platform.
      </p>
      <p class="text-[#B9BABE] font-medium">
        If you ever need support, please feel free to
      </p>
      <p class="text-[#B9BABE] font-medium">
        email us at support@loremgaming.com.
      </p> -->
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
