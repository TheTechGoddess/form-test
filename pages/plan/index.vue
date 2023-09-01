<template>
  <div
    class="md:py-10 md:px-20 mx-5 -mt-20 md:mx-0 md:mt-0 rounded-lg bg-white md:bg-none px-6 py-8 self-center absolute md:relative"
  >
    <div>
      <h1 class="text-4xl text-[#022857] font-bold">Select your plan</h1>
      <p class="text-[#B9BABE] mt-2 font-medium">
        you have the option of monthly or yearly billing
      </p>
      <!-- plans section -->
      <div class="mt-10 md:flex md:justify-between w-full">
        <div
          v-for="(plan, index) in plans"
          :key="index"
          class="px-5 py-3 md:p-5 border border-[#DBDBDD] w-full my-4 md:my-0 flex md:flex-col md:w-36 hover:border-[#022857] hover:border-2 rounded-lg mr-5"
          :class="{
            'md:h-40': activeOption === 'monthly',
            'md:h-44': activeOption === 'yearly',
          }"
          @click="savePlanToLocalStorage(plan)"
        >
          <img
            :src="plan.icon"
            alt=""
            class="mr-6 md:mr-0 w-10 -mt-5 md:-mt-0"
          />
          <div class="md:mt-10">
            <p class="text-[#022857] font-bold">{{ plan.name }}</p>
            <p class="text-[#9A9BA0] text-sm">
              {{
                activeOption === "monthly"
                  ? `$${plan.monthlyPrice}/mo`
                  : `$${(
                      plan.monthlyPrice * 12 -
                      2 * plan.monthlyPrice
                    ).toFixed(2)}/yr`
              }}
            </p>
            <p
              v-if="activeOption === 'yearly'"
              class="text-sm text-[#082759] mt-1"
            >
              2 months free
            </p>
          </div>
        </div>
      </div>

      <button
        class="flex items-center focus:outline-none my-3 p-3 justify-center content-center w-full rounded-lg bg-[#F8F9FE] mt-8"
        @click="toggleService"
      >
        <span
          class="font-semibold mr-3"
          :class="{
            'text-[#00295B]': activeOption === 'monthly',
            'text-[#B2B2BC]': activeOption === 'yearly',
          }"
          >Monthly</span
        >
        <div class="relative w-10 h-5 rounded-full bg-[#042858]">
          <div
            :class="[
              'absolute left-0 ml-0.5 top-0.5 w-4 h-4 rounded-full bg-white transition-transform',
              activeOption === 'yearly'
                ? 'transform translate-x-full mr-0.5'
                : '',
            ]"
          ></div>
        </div>
        <span
          class="font-semibold ml-3"
          :class="{
            'text-[#00295b]': activeOption === 'yearly',
            'text-[#b2b2bc]': activeOption === 'monthly',
          }"
          >Yearly</span
        >
      </button>
      <div class="mt-20 flex justify-between w-[100%]">
        <nuxt-link to="/" class="self-start"
          ><p class="text-[#B8BBC4] font-medium mt-2">Go Back</p></nuxt-link
        >
        <nuxt-link to="/add-ons" class="self-end">
          <button class="bg-[#174A89] text-white w-32 rounded py-3 font-medium">
            Next Step
          </button>
        </nuxt-link>
      </div>
    </div>
  </div>
</template>
<script>
import arcade from "~/assets/images/icon-arcade.svg";
import advanced from "~/assets/images/icon-advanced.svg";
import pro from "~/assets/images/icon-pro.svg";
export default {
  data() {
    return {
      plans: [
        { name: "Arcade", icon: arcade, monthlyPrice: 9 },
        { name: "Advanced", icon: advanced, monthlyPrice: 12 },
        { name: "Pro", icon: pro, monthlyPrice: 15 },
      ],
      activeOption: "monthly", // Initial active option
    };
  },
  methods: {
    toggleService() {
      this.activeOption =
        this.activeOption === "monthly" ? "yearly" : "monthly";
    },
    savePlanToLocalStorage(plan) {
      // Create an object with plan details
      const selectedPlan = {
        name: plan.name,
        price: plan.monthlyPrice,
        billingFrequency: this.activeOption,
      };

      // Convert the object to a JSON string and save it in local storage
      localStorage.setItem("selectedPlan", JSON.stringify(selectedPlan));

      // Save the billing option separately
      localStorage.setItem("billingOption", this.activeOption);

      // Navigate to the /add-ons page
      this.$router.push("/add-ons");
    },
  },
};
</script>
