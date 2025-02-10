<script setup>
import { storeToRefs } from "pinia";
import { onMounted, ref } from "vue";
import { useBanner } from "@/stores";

const banner = useBanner(); // ✅ Store ব্যবহার করছি
const { banners } = storeToRefs(banner); 

const getBannerData = async () => {
  console.log("Calling getBannerData..."); // ✅ ফাংশন চালু হচ্ছে কিনা চেক করো

  try {
    const res = await banner.getData(); // ✅ Store থেকে Data কল করছি
    console.log("API Response in Component:", res); // ✅ API Response Component-এ আসছে কিনা চেক করো

    if (res) {
      bannerData.value = res; // ✅ Data ঠিকমতো সেট করছি
      console.log("Updated bannerData:", bannerData.value);
    } else {
      console.error("No data returned from API");
    }
  } catch (error) {
    console.error("API Call Failed:", error);
  }
};

// ✅ Component লোড হলে getBannerData() কল করো
onMounted(() => {
  getBannerData();
});


// All Import File  Code Is Here......................................................................................................

// All Variable  Code Is Here.....................................................................................................

// API Calling Code Is Here.....................................................................................................

// All Function  Code Is Here.....................................................................................................

defineProps({
  title: String,
});
</script>


<template>
  <div>
     <!--=====================================
                  BANNER PART START
      =======================================-->
      <section class="inner-section single-banner">
          <div class="container">
              <h2>Shop Page</h2>
          </div>
      </section>
      <!--=====================================
                  BANNER PART END
      =======================================-->
  </div>
</template>

<style scope>


.single-banner {
  background-size: cover !important;
  background: var(--primary-color);
  padding: 20px 0px;
  text-align: center;
  position: relative;
  z-index: 1;
}

.single-banner::before {
  position: absolute;
  content: "";
  top: 0px;
  left: 0px;
  width: 100%;
  height: 100%;
  /* background: -webkit-gradient(linear, left top, right top, from(rgba(150, 25, 16, 0.8)), to(rgba(194, 191, 25, 0.8)));
  background: linear-gradient(to right,rgb(0 0 0 / 80%),rgb(0 0 0 / 80%)); */
  background-color: var(--primary-color);

  z-index: -1;
}

.single-banner h2 {
  font-size: 25px;
  color: var(--white);
  text-transform: uppercase;
}

</style>
