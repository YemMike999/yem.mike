<template>
  <section
    class="home"
    :class="{ dark: nightMode, light: !nightMode }"
    :style="themeStyle"
  >
    <div class="container">
      <div class="row align-items-center">

        <!-- IMAGE -->
        <div class="col-md-6 text-center">
          <div class="img-wrap" :class="{ 'img-wrap-dark': nightMode }">
            <img :src="picture" alt="profile" class="profile-img" />
          </div>
        </div>

        <!-- TEXT -->
        <div class="col-md-6 pt-4">

          <h1 class="hero-title">
            <span class="hello">Hello, I'm</span>
            <span class="name-wrap">
              <span class="name">{{ typedName }}</span>
              <span class="caret">|</span>
            </span>
          </h1>

          <div class="underline"></div>

          <p
            class="desc"
            :class="{ 'desc-dark': nightMode }"
            v-html="description"
          ></p>

          <!-- SOCIAL -->
          <div class="pb-3">
            <button
              class="icon-btn"
              :class="{ 'icon-btn-dark': nightMode }"
              @click="openExternal('linkedin')"
            >
              <i class="fab fa-linkedin"></i>
            </button>

            <button
              class="icon-btn"
              :class="{ 'icon-btn-dark': nightMode }"
              @click="openExternal('github')"
            >
              <i class="fab fa-github"></i>
            </button>

            <button
              class="icon-btn"
              :class="{ 'icon-btn-dark': nightMode }"
              @click="openExternal('facebook')"
            >
              <i class="fab fa-facebook-f"></i>
            </button>
          </div>

          <!-- RESUME -->
          <a
            v-if="links.resume"
            class="btn-accent"
            :href="links.resume"
            download
          >
            Download Resume
          </a>

        </div>
      </div>
    </div>
  </section>
</template>

<script>
import info from "../../info";

export default {
  name: "Home",

  props: {
    nightMode: Boolean,
  },

  data() {
    return {
      fullName: "Yem Mike",
      typedName: "",
      typeIndex: 0,
      phase: "typing",
      timer: null,
    };
  },

  computed: {
    themeStyle() {
      return {
        "--accent": info?.theme?.accent || "#ff7a00",
        "--accentSoft":
          info?.theme?.accentSoft || "rgba(255,122,0,0.15)",
      };
    },
    picture() {
      return info?.flat_picture || "";
    },
    description() {
      return info?.description || "";
    },
    links() {
      return info?.links || {};
    },
  },

  mounted() {
    this.startTyping();
  },

  beforeUnmount() {
    clearTimeout(this.timer);
  },

  methods: {
    openExternal(key) {
      const url = this.links?.[key];
      if (!url) return;
      window.open(url, "_blank", "noopener,noreferrer");
    },

    startTyping() {
      const typeSpeed = 110;
      const deleteSpeed = 60;
      const pauseTime = 2000;

      const tick = () => {
        if (this.phase === "typing") {
          this.typeIndex++;
          this.typedName = this.fullName.slice(0, this.typeIndex);

          if (this.typeIndex === this.fullName.length) {
            this.phase = "pause";
            this.timer = setTimeout(tick, pauseTime);
            return;
          }

          this.timer = setTimeout(tick, typeSpeed);
          return;
        }

        if (this.phase === "pause") {
          this.phase = "deleting";
          this.timer = setTimeout(tick, 400);
          return;
        }

        // deleting
        this.typeIndex--;
        this.typedName = this.fullName.slice(0, this.typeIndex);

        if (this.typeIndex === 0) {
          this.phase = "typing";
          this.timer = setTimeout(tick, 500);
          return;
        }

        this.timer = setTimeout(tick, deleteSpeed);
      };

      tick();
    },
  },
};
</script>

<style scoped>
.home {
  padding: 80px 0;
  transition: 0.4s ease;
}

/* DARK MODE */
.home.dark {
  background: #111417;
  color: #ffffff;
}

/* LIGHT MODE */
.home.light {
  background: #ffffff;
  color: #111111;
}

/* IMAGE */
.img-wrap {
  padding: 14px;
  border-radius: 20px;
  background: #f3f3f3;
  transition: 0.3s;
}
.img-wrap-dark {
  background: #1d2226;
}

.profile-img {
  width: 320px;
  border-radius: 18px;
}

/* TITLE */
.hero-title {
  font-size: 46px;
  font-weight: 900;
}

.name {
  color: var(--accent);
}

.caret {
  color: var(--accent);
  animation: blink 0.8s infinite;
}

@keyframes blink {
  0%,49% { opacity:1 }
  50%,100% { opacity:0 }
}

.underline {
  height: 4px;
  width: 120px;
  background: var(--accent);
  border-radius: 50px;
  margin: 10px 0 15px;
}

/* DESCRIPTION */
.desc {
  font-size: 14px;
  line-height: 1.8;
  color: #444;
}
.desc-dark {
  color: #ccc;
}

/* SOCIAL */
.icon-btn {
  width: 44px;
  height: 44px;
  margin-right: 10px;
  border-radius: 12px;
  border: none;
  cursor: pointer;
  background: #f0f0f0;
  transition: 0.3s;
}
.icon-btn-dark {
  background: #2a2f33;
  color: #fff;
}

.icon-btn:hover {
  background: var(--accent);
  color: #fff;
}

/* BUTTON */
.btn-accent {
  padding: 12px 18px;
  border-radius: 12px;
  border: 1px solid var(--accent);
  color: var(--accent);
  text-decoration: none;
  font-weight: bold;
  transition: 0.3s;
}
.btn-accent:hover {
  background: var(--accent);
  color: #111;
}
</style>