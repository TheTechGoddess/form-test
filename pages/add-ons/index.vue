<template>
  <div
    class="md:py-10 md:pl-20 md:pr-16 md:w-full mx-5 -mt-20 md:mx-0 md:mt-0 rounded-lg bg-white md:bg-none px-6 py-8 self-center absolute md:relative"
  >
    <div>
      <h1 class="text-4xl text-[#022857] font-bold">Pick add-ons</h1>
      <p class="text-[#B9BABE] mt-2 font-medium">
        Add-ons help enhance your gaming experience
      </p>
      <div
        v-for="(item, index) in items"
        :key="index"
        @click="toggleItemCheck(index)"
        class="flex justify-between items-center content-center mt-10 p-3 md:py-3 md:px-5 w-full md:w-[120%] border rounded-lg border-[#7C72D0]"
      >
        <div class="flex">
          <div
            class="h-6 w-6 mr-6 justify-center flex self-center rounded bg-[#FFF] border border-[#B9BABE]"
            :class="{
              'bg-[#453FFB] ': item.checked,
            }"
          >
            <span v-if="item.checked">&#10004;</span>
          </div>
          <div>
            <p class="text-[#022857] font-bold mb-0.5">{{ item.title }}</p>
            <p class="text-[#B9BABE] text-sm mt-0.5">{{ item.description }}</p>
          </div>
        </div>
        <p class="text-[#7C72D0]">${{ formatPrice(item.price) }}</p>
      </div>

      <div class="mt-20 flex justify-between w-full md:w-[120%]">
        <nuxt-link to="/" class="self-start"
          ><p class="text-[#B8BBC4] font-medium mt-2">Go Back</p></nuxt-link
        >
        <button
          @click="save"
          class="bg-[#174A89] text-white w-32 rounded py-3 font-medium self-end"
        >
          Next Step
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      items: [
        {
          title: "Online Service",
          description: "Access to multiplayer games",
          price: "1",
          checked: false,
        },
        {
          title: "Larger Storage",
          description: "Extra 1TB of cloud save",
          price: "2",
          checked: false,
        },
        {
          title: "Customizable Profile",
          description: "Custom theme on your profile",
          price: "2",
          checked: false,
        },
      ],
      activeOption: "monthly",
    };
  },
  mounted() {
    const storedBillingFrequency = localStorage.getItem("billingOption");
    if (storedBillingFrequency) {
      this.activeOption = storedBillingFrequency;
    }
  },
  methods: {
    toggleItemCheck(index) {
      // Toggle the 'checked' property of the clicked item
      this.items[index].checked = !this.items[index].checked;
    },
    formatPrice(price) {
      // console.log(this.activeOption);
      if (this.activeOption === "yearly") {
        // Replace "/mo" with "/yr" at the end of the price
        return price * 10 + "/yr";
      }
      return price + "/mo"; // Add "/mo" for monthly billing
    },
    save() {
      const checkedItems = this.items.filter((item) => item.checked);
      localStorage.setItem("checkedItems", JSON.stringify(checkedItems));
      this.$router.push("/summary");
    },
  },
};
</script>
