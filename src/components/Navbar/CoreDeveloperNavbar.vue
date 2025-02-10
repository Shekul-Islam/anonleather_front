<script setup>
// All Import File  Code Is Here......................................................................................................
import { useAuth, useNotification, useCategory } from "@/stores";
import { useRouter, useRoute } from "vue-router";
import { storeToRefs } from "pinia";
import { ref, onMounted } from "vue";
import axiosInstance from "@/services/axiosService.js";
// All Variable  Code Is Here.....................................................................................................
const auth = useAuth();
const router = useRouter();
const route = useRoute();
const { user, loading } = storeToRefs(auth);
const notify = useNotification();
const category = useCategory();
const { categories } = storeToRefs(category);
const campaignDataShowing = ref("");
// API Calling Code Is Here.....................................................................................................

// All Function  Code Is Here.....................................................................................................

const logout = async () => {
  const res = await auth.logout();
  if (res.success) {
    router.push({ name: "homePage" });
    notify.Success("Logout Successfully Done");
  }
};


// sticky Header

const stickyHeader = () => {
  const mainHeaderNavSection = document.querySelector(".navbar-part");

  window.addEventListener("scroll", () => {
    const scrollTopWindow = window.pageYOffset;
    const scrollScreenSize = window.screen.width;

    if (scrollTopWindow > 80 && scrollScreenSize > 768) {
      mainHeaderNavSection.classList.add("header-sticky");
    } else {
      mainHeaderNavSection.classList.remove("header-sticky");
    }
  });
};

// sticky Header

onMounted(() => {
  stickyHeader();
});
</script>

<template lang="">
  <div>
    <!--=====================================
                    NAVBAR PART START
        =======================================-->
        <nav class="navbar-part">
            <div class="container">
                <div class="row">
                    <div class="col-xl-2 col-lg-3">
                        <div class="banner-category">
                            <div class="banner-category-head">
                              <i class="fas fa-bars"></i>
                              <span>top categories</span>
                            </div>
                            <ul class="banner-category-list">
                              <template v-if="!categories?.data">
                                <CategorySkeleton />
                              </template>
                              <template v-else>
                                <li class="banner-category-item" v-for="(category, index) in categories?.data" :key="index">
                                  <router-link :to="{ name: 'shopPage', query: { category: category?.id }}">
                                    <i class="flaticon-vegetable"></i>
                                    <span>{{ category?.name }}</span>
                                    <i class="fa-solid fa-arrow-right ms-auto fs-6" @click.prevent="toggleDropdown(index, category)" v-if="category?.sub_category?.length > 0"></i>
                                  </router-link>
                                </li>
                              </template>
                            </ul>
                        </div>
                    </div>
                    <div class="col-xl-4 col-lg-3"></div>
                    <div class="col-xl-6 col-lg-6">
                        <div class="navbar-content">
                            <ul class="navbar-list">
                                <li class="navbar-item dropdown">
                                    <router-link :to="{ name: 'homePage' }" class="navbar-link" :class="{ 'text-warning': $route.name === 'homePage' }">Home</router-link>
                                </li>
                                <li class="navbar-item dropdown-megamenu">
                                    <router-link :to="{ name: 'shopPage' }" class="navbar-link" :class="{ 'text-warning': $route.name === 'shopPage' }">Shop</router-link>
                                </li>
                                <li class="navbar-item">
                                    <router-link :to="{ name: 'aboutPage' }" class="navbar-link" :class="{ 'text-warning': $route.name === 'aboutPage' }">About Us</router-link>
                                </li>
                                <li class="navbar-item">
                                     <router-link :to="{ name: 'contactPage' }" class="navbar-link" :class="{ 'text-warning': $route.name === 'contactPage' }">Contact Us</router-link>
                                </li>
                                <li class="navbar-item" v-if="campaignDataShowing == 'flashdeal'">
                                     <router-link :to="{ name: 'flashDealPage' }" class="navbar-link" :class="{ 'text-warning': $route.name === 'flashDealPage' }">Flash Deal</router-link>
                                </li>
                                <li class="navbar-item" v-if="campaignDataShowing == 'campaign'">
                                     <router-link :to="{ name: 'campaignPage' }" class="navbar-link" :class="{ 'text-warning': $route.name === 'campaignPage' }">Campaign Page</router-link>
                                </li>
                                <li class="navbar-item">
                                     <router-link :to="{ name: 'blogPage' }" class="navbar-link" :class="{ 'text-warning': $route.name === 'blogPage' }">Blog</router-link>
                                </li>
                                <!-- <li class="navbar-item dropdown">
                                    <a class="navbar-link dropdown-arrow" href="#">authentic</a>
                                    <ul class="dropdown-position-list" v-if="user.user">
                                        <li><router-link :to="{name: 'Profile'}">Profile</router-link></li>
                                        <li><router-link :to="{name: 'OrderList'}">My Order</router-link></li>
                                        <li ><a href="" @click.prevent="logout()">Logout<span v-show="loading" class="spinner-border spinner-border-sm mr-1"></span></a></li>
                                    </ul>
                                    <ul class="dropdown-position-list" v-else>
                                        <li><router-link :to="{name:'Login'}">login</router-link></li>
                                        <li><router-link :to="{name: 'Registration'}">register</router-link></li>
                                    </ul>
                                </li> -->
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </nav>
        <!--=====================================
                    NAVBAR PART END
        =======================================-->
  </div>
</template>

<style>

.banner-category-head {
  justify-content: start;
}

.active-link {
  color: var(--secondary-color) !important;
  font-weight: bold;
}

.navbar-section{
    padding: 5px 0px; 
    display:inline-block;
}
.navbar-section span a{
    padding: 10px 20px; 
    display:inline-block;
    font-size: 15px;
    color: var(--white);
}
.navbar-section span a:hover{
    color: var(--secondary-color);
    cursor: pointer;
}

.navbar-link {
    padding: 10px 0px;
}

.header-sticky {
  z-index: 99999;
  width: 100%;
  position: fixed;
  background-color: rgb(255, 255, 255);
  top: 0;
  padding: 5px 0 10px 0;
  border-radius: 0 0 30px 30px;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}

.header-sticky .banner-category-head{
  margin-top: 8px;
}

@keyframes pulse {
  0% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.1);
    opacity: 0.8;
  }
  100% {
    transform: scale(1);
    opacity: 1;
  }
}

.main-order-btn {
  animation: pulse 1s infinite;
  font-size: 20px;
  background-color: var(--secondary-color);
  color: var(--primary);
  padding: 5px 15px;
  border-top: 2px solid var(--primary);
  border-left: 2px solid var(--primary);
  border-bottom: 2px solid var(--primary);
  border-right: 2px solid var(--primary);
  transition: 0.3s ease-in-out background-color;
}

.main-order-btn:hover {
  background-color: var(--secondary-color);
  color: white;
  transition: 0.3s ease-in-out background-color;
  border-color: var(--secondary-color);
}

.banner-category-list {
  width: 12%;
}

.active-text-color {
  color: var(--primary) !important;
}

@media (max-width: 1440px) {
  .banner-category-list {
    width: 14%;
  }
}

@media (max-width: 1199px) {
  .banner-category-list {
    width: 19%;
  }
}

@media (max-width: 1024px) {
  .banner-category-list {
    width: 21% !important;
  }
}

@media (max-width: 991px) {
}
</style>
