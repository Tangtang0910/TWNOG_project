<template>
  <nav ref="navbar" class="fixed top-0 left-0 right-0 z-50 bg-amber-10 bg-white text-yellow-900 px-10 py-2 border-b-4 border-amber-400">
    <div class="mx-auto flex justify-between items-center">
      <!-- Logo部分 -->
      <img :src="logo" alt="Logo" class="cursor-pointer w-28 h-12" @click="goToHomePage">

      <!-- 導航鏈接 -->
      <div class="hidden md:flex lg:flex space-x-4 whitespace-nowrap">
        <router-link to="/call-for-speakers" class="hover:bg-amber-400 rounded-md px-6 py-3">徵求演講</router-link>
        <router-link to="/transportation" class="hover:bg-amber-400 rounded-md px-6 py-3">交通方式</router-link>
        <div class="relative" @mouseenter="mouseEnterEvent" @mouseleave="mouseLeaveEvent">
          <div :class="['cursor-pointer rounded-md px-6 py-3', isEventHover ? 'bg-amber-400' : 'hover:bg-amber-400']">歷年活動</div>
          <!-- 展開的歷年活動 -->
          <div :class="{'hidden': !isEventHover, 'flex': isEventHover, 'flex-col': isEventHover, 'space-y-2': isEventHover, 'absolute': isEventHover, 'top-full': isEventHover, 'z-10': isEventHover}" class="bg-amber-100 border-b-2 border-amber-200 rounded-md w-full">
            <a href="#" class="hover:text-sky-500 rounded-md px-4 py-3">TWNOG 1</a>
            <a href="#" class="hover:text-sky-500 rounded-md px-4 py-3">TWNOG 2</a>
            <a href="#" class="hover:text-sky-500 rounded-md px-4 py-3">TWNOG 3</a>
            <a href="#" class="hover:text-sky-500 rounded-md px-4 py-3">TWNOG 4</a>
          </div>
        </div>
        <router-link to="/volunteers" class="hover:bg-amber-400 rounded-md px-6 py-3">志工團隊</router-link>
        <router-link to="/collaborators" class="hover:bg-amber-400 rounded-md px-6 py-3">合作夥伴</router-link>
      </div>

      <!-- 手機端選單按鈕 -->
      <svg ref="toggle" @click="toggleMenu" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="cursor-pointer md:hidden w-6 h-6">
        <path stroke-linecap="round" stroke-linejoin="round" d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5" />
      </svg>
    </div>
  </nav>
  
  <!-- 手機版選單 -->
  <div ref="menu" :class="{'hidden': !isMenuOpen, 'flex': isMenuOpen, 'flex-col': isMenuOpen, 'space-y-3': isMenuOpen, 'sticky top-16 z-50': true}" class="md:hidden items-center bg-amber-100 text-yellow-900 border-b-4 border-t-4 border-amber-200 py-3">
    <router-link to="/call-for-speakers" class="hover:bg-amber-400 rounded-md px-4 py-2">
      <span class="border-b border-amber-300 py-2 px-5">徵求演講</span>
    </router-link>
    <router-link to="/transportation" class="hover:bg-amber-400 rounded-md px-4 py-2">
      <span class="border-b border-amber-300 py-2 px-5">交通方式</span>
    </router-link>
    <!-- 使用 div 觸發顯示隱藏 -->
    <div @click="clickEvent" class="cursor-pointer hover:bg-amber-400 rounded-md px-4 py-2">
      <span class="border-b border-amber-300 py-2 px-5">歷年活動</span>
    </div>
    <!-- 展開的菜單部分 -->
    <div :class="{'hidden': !isEventClick, 'flex': isEventClick, 'flex-col': isEventClick, 'space-y-2': isEventClick, 'py-3': isEventClick}" class="w-full items-center bg-amber-300 text-white">
      <a href="#" class="hover:text-sky-600 rounded-md px-4 py-1">TWNOG 1</a>
      <a href="#" class="hover:text-sky-600 rounded-md px-4 py-1">TWNOG 2</a>
      <a href="#" class="hover:text-sky-600 rounded-md px-4 py-1">TWNOG 3</a>
      <a href="#" class="hover:text-sky-600 rounded-md px-4 py-1">TWNOG 4</a>
    </div>
    <router-link to="/volunteers" class="hover:bg-amber-400 rounded-md px-4 py-2">
      <span class="border-b border-amber-300 py-2 px-5">志工團隊</span>
    </router-link>
    <router-link to="/collaborators" class="hover:bg-amber-400 rounded-md px-4 py-2">
      <span class="border-b border-amber-300 py-2 px-5">合作夥伴</span>
    </router-link>
  </div>
</template>

<script>
import logo from '../assets/logo.png';

export default {
  name: 'NavBar',
  data() {
    return {
      logo,
      isMenuOpen: false,
      isEventClick: false,
      isEventHover: false,
    };
  },
  methods: {
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
    },
    clickEvent() {
      this.isEventClick = !this.isEventClick;
    },
    mouseEnterEvent() {
      this.isEventHover = true;
    },
    mouseLeaveEvent() {
      this.isEventHover = false;
    },
    handleGlobalClick(e) {
      if (this.$refs.menu && !this.$refs.menu.contains(e.target) && !this.$refs.toggle.contains(e.target)) {
        this.isMenuOpen = false;
        this.isEventClick = false;
      }
    },
    goToHomePage() {
      this.$router.push('/');
      window.scrollTo(0, 0); //回到頁面頂部
    },
  },
  mounted() {
    document.addEventListener('click', this.handleGlobalClick);
    this.$router.afterEach(() => {
      this.isMenuOpen = false;
    });
  },
  beforeDestroy() {
    document.removeEventListener('click', this.handleGlobalClick);
  }
}
</script>
