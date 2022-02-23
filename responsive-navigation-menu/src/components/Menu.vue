<template>
  <header :class="{ 'scrolled-nav': scrolledNav }">
    <nav v-if="responsiveNav === false">
      <div class="logo">
        <a href="#"> <i class="fa-brands fa-reddit-alien"></i></a>
      </div>
      <ul>
        <li><a href="#">home</a></li>
        <li><a href="#">about</a></li>
        <li><a href="#">portfolio</a></li>
        <li><a href="#">contact</a></li>
      </ul>
    </nav>
    <nav v-else>
      <div class="logo">
        <a href="#"> <i class="fa-brands fa-reddit-alien"></i></a>
      </div>
      <div class="menuIcon">
        <a href="#"> <i @click="openMenu" class="fa-solid fa-bars"></i></a>
      </div>
    </nav>
    <transition name="mobile-nav">
      <ul v-show="dropdownMenu === true" class="responsiveList">
        <li>
          <a href=""> <i class="fa-brands fa-reddit-alien"></i></a>
        </li>
        <li><a href="#">home</a></li>
        <li><a href="#">about</a></li>
        <li><a href="#">portfolio</a></li>
        <li><a href="#">contact</a></li>
      </ul>
    </transition>
  </header>
</template>

<script setup>
import { ref, onMounted } from "vue";

const responsiveNav = ref(false);
const windowWidth = ref(null);
const dropdownMenu = ref(false);
const windowScrollY = ref(null);
const scrolledNav = ref(null);

onMounted(() => {
  checkWindowSize();
  window.addEventListener("scroll", updateScroll);
});

const updateScroll = () => {
  windowScrollY.value = window.scrollY;
  if (windowScrollY.value > 10) {
    scrolledNav.value = true;
    return;
  }
  scrolledNav.value = false;
};

const checkWindowSize = () => {
  windowWidth.value = window.innerWidth;
  if (windowWidth.value < 901) {
    responsiveNav.value = true;
    return;
  }
  dropdownMenu.value = false;
  responsiveNav.value = false;
};

window.addEventListener("resize", checkWindowSize);
const openMenu = () => {
  dropdownMenu.value = !dropdownMenu.value;
};
</script>

<style lang="scss">
header {
  background-color: rgba(0, 0, 0, 0.8);
  z-index: 99;
  width: 100%;
  position: fixed;
  transition: 0.5s ease all;
  color: #fff;
  padding: 10px 70px;

  nav {
    display: flex;
    align-items: center;
    justify-content: space-between;

    .logo {
      a {
        text-decoration: none;
        i {
          font-size: 55px;
          color: #fff;
        }
      }
    }

    .menuIcon {
      a {
        text-decoration: none;
        i {
          font-size: 25px;
          color: #fff;
          transition: all 1s;

          &:hover {
            transform: rotate(180deg);
          }
        }
      }
    }

    ul {
      display: flex;
      align-items: center;
      gap: 50px;
      li {
        list-style: none;

        a {
          text-decoration: none;
          text-transform: uppercase;
          color: #fff;
          transition: all 0.4s;
          position: relative;

          &::after {
            content: "";
            width: 50%;
            height: 3px;
            color: #fff;
            background: #fff;
            position: absolute;
            transition: all 0.4s;
            top: 20px;
            left: 0;
          }

          &:hover {
            color: #333;

            &::after {
              width: 100%;
            }
          }
        }
      }
    }
  }

  .mobile-nav-enter-active,
  .mobile-nav-leave-active {
    transition: 1s ease all;
  }

  .mobile-nav-enter-from,
  .mobile-nav-leave-to {
    transform: translateX(-250px);
  }

  .mobile-nav-enter-to,
  .mobile-nav-leave-from {
    transform: translateX(0);
  }

  .responsiveList {
    display: flex;
    flex-direction: column;
    position: fixed;
    width: 100%;
    max-width: 250px;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.97);
    top: 0;
    left: 0;
    gap: 60px;
    padding: 20px 30px;

    li {
      list-style: none;
      border-bottom: 2px solid #fff;
      padding-bottom: 15px;

      &:first-child {
        border: none;
      }

      a {
        text-decoration: none;
        color: #fff;
        text-transform: uppercase;
        transition: all 0.5s;

        &:hover {
          color: #333;
        }

        i {
          font-size: 45px;
        }
      }
    }
  }

  .scrolled-nav {
    background-color: #000;
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
      0 2px 4px -1px rgba(0, 0, 0, 0.06);

    nav {
      padding: 8px 0;

      .logo {
        a {
          i {
            font-size: 35px;
          }
        }
      }
    }
  }
}
</style>