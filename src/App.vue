<template>
  <div id="app" :class="{ 'text-dark': !nightMode, 'text-light': nightMode }">
    <Navbar
      @scroll="scrollTo"
      @nightMode="switchMode"
      :nightMode="nightMode"
    />

    <div class="parent">
      <Home id="home" :nightMode="nightMode" />
      <About id="about" :nightMode="nightMode" />
      <Skills id="skills" :nightMode="nightMode" />
      <Portfolio id="portfolio" :nightMode="nightMode" />
      <Contact id="contact" :nightMode="nightMode" />
      <Footer :nightMode="nightMode" />
    </div>
  </div>
</template>

<script>
import Navbar from "./components/Navbar.vue";
import Home from "./components/Home.vue";
import About from "./components/About.vue";
import Skills from "./components/Skills.vue";
import Portfolio from "./components/Portfolio.vue";
import Contact from "./components/Contact.vue";
import Footer from "./components/Footer.vue";
import info from "../info";

export default {
  name: "App",

  components: {
    Navbar,
    Home,
    About,
    Skills,
    Portfolio,
    Contact,
    Footer,
  },

  data() {
    return {
      nightMode: false,
      config: info?.config || {},
    };
  },

  created() {
    const savedMode = localStorage.getItem("nightMode");
    this.nightMode = savedMode === "true";
  },

  mounted() {
    this.handleInitialScroll();
  },

  methods: {
    switchMode(mode) {
      this.nightMode = mode;
      localStorage.setItem("nightMode", mode);
    },

    scrollTo(section) {
      if (section === "home") {
        window.scrollTo({
          top: 0,
          behavior: "smooth",
        });
        return;
      }

      const el = document.getElementById(section);
      if (!el) return;

      window.scrollTo({
        top: el.offsetTop - 35,
        behavior: "smooth",
      });
    },

    handleInitialScroll() {
      const hash = window.location.hash.replace("#", "");
      if (!hash) return;

      this.$nextTick(() => {
        const el = document.getElementById(hash);
        if (!el) return;

        window.scrollTo({
          top: el.offsetTop - 35,
          behavior: "smooth",
        });
      });
    },
  },
};
</script>

<style>
#app {
  font-family: "Montserrat", sans-serif;
  width: 100%;
}

.parent {
  margin-top: 38px;
  padding-top: 40px;
  position: relative;
}

.text-light {
  color: #d3d2d2 !important;
}

.text-dark {
  color: #2c3e50 !important;
}
</style>