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
    <section class="banner-section">
      <img src="@/assets/images/banner-img.png" alt="" width="100%"/>
    </section>
    <!--=====================================
                    BANNER PART END
        =======================================-->
  </div>
</template>

<style scope></style>
