<script>
import { theme } from "../../../stores/theme.js";

export default {
  data() {
    return {
      theme,
    };
  },

  mounted() {
    let validThemes = ["light", "dark"];
    let theme = this.getSavedTheme();

    if (!validThemes.includes(theme)) theme = this.getSystemScheme();

    switch (theme) {
      case "dark":
        this.theme.light = false;
        break;
      default:
        this.theme.light = true;
    }

    this.toggle();
  },

  methods: {
    onClick() {
      this.theme.light = !this.theme.light;
    },
    toggle() {
      const metaThemeEl = document.querySelector("meta[name=theme-color]");
      const htmlEl = document.querySelector("html");

      if (!this.theme.light) {
        metaThemeEl.setAttribute("content", "#04293a");
        htmlEl.setAttribute("theme", "dark");
        // document.body.classList.add("dark");
        // document.body.classList.remove("light");
        return;
      }

      metaThemeEl.setAttribute("content", "#bebebe");
      htmlEl.setAttribute("theme", "light");
      // document.body.classList.add("light");
      // document.body.classList.remove("dark");
    },
    getSystemScheme() {
      if (
        window.matchMedia &&
        window.matchMedia("(prefers-color-scheme: dark)").matches
      ) {
        return "dark";
      }

      return "light";
    },
    getSavedTheme() {
      return localStorage.getItem("dimanda_theme");
    },
    saveTheme() {
      if (this.theme.light)
        return localStorage.setItem("dimanda_theme", "light");

      return localStorage.setItem("dimanda_theme", "dark");
    },
  },

  watch: {
    "theme.light"(newVal) {
      this.toggle();
      this.saveTheme();
    },
  },
};
</script>

<template>
  <div class="theme_switch">
    <button
      class="theme_switch__container"
      aria-label="Toggle color mode"
      title="Toggle color mode"
      @click="onClick"
    >
      <div class="light" :class="{ visible: theme.light }"></div>
      <div class="moon" :class="{ visible: !theme.light }">
        <div class="star"></div>
        <div class="star small"></div>
      </div>
    </button>
  </div>
</template>

<style scoped lang="scss">
.theme_switch {
  display: inline-block;
}

.theme_switch__container {
  width: 32px;
  height: 32px;
  box-sizing: border-box;
  padding: 12px;
  background: none;
  border: none;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}

.light {
  width: 50%;
  height: 50%;
  position: absolute;
  pointer-events: none;
  opacity: 0;
  transform: scale(0.6) rotate(0deg);
  transition: transform 0.3s ease-in, opacity 0.2s ease-in 0.1s;

  /*  white transparent for Safari  */
  //   background: radial-gradient(
  //     circle,
  //     rgba(0, 0, 0, 0),
  //     rgba(0, 0, 0, 0) 50%,
  //     #f0f0f0 50%
  //   );
  background: radial-gradient(
    circle,
    #fff,
    var(--color-font) 50%,
    var(--color-font) 50%
  );
}

.light:before {
  content: "";
  position: absolute;
  display: block;
  width: 100%;
  height: 100%;
  background: radial-gradient(
    circle,
    var(--bg-primary) 30%,
    var(--color-font) 50%,
    var(--bg-primary) 31%,
    var(--color-font) 50%
  );
  // radial-gradient(circle, #000 30%, #fff 31%, #fff 50%, var(--color-font) 50%)
  transform: rotate(45deg);
}

.light.visible {
  pointer-events: auto;
  opacity: 1;
  transform: scale(1) rotate(180deg);
  transition: transform 0.3s ease-in, opacity 0.2s ease-in 0.1s;
}

.moon {
  width: 50%;
  height: 50%;
  pointer-events: none;
  position: absolute;
  left: 12.5%;
  top: 18.75%;
  background-color: rgba(0, 0, 0, 0);
  border-radius: 50%;
  box-shadow: 9px 3px 0px 0px #f0f0f0;
  opacity: 0;
  transform: scale(0.3) rotate(65deg);
  transition: transform 0.3s ease-in, opacity 0.2s ease-in 0.1s;
}

.moon.visible {
  pointer-events: auto;
  opacity: 1;
  transform: scale(1) rotate(0deg);
  transition: transform 0.3s ease-in, opacity 0.2s ease-in 0.1s;
}

.star {
  position: absolute;
  top: 25%;
  left: 5%;
  display: block;
  width: 0px;
  height: 0px;
  border-right: 7px solid rgba(0, 0, 0, 0);
  border-bottom: 5px solid #f0f0f0;
  border-left: 7px solid rgba(0, 0, 0, 0);
  transform: scale(0.55) rotate(35deg);
  opacity: 0;
  transition: all 0.2s ease-in 0.4s;
}

.star:before {
  border-bottom: 5px solid #f0f0f0;
  border-left: 3px solid rgba(0, 0, 0, 0);
  border-right: 3px solid rgba(0, 0, 0, 0);
  position: absolute;
  height: 0;
  width: 0;
  top: -3px;
  left: -5px;
  display: block;
  content: "";
  transform: rotate(-35deg);
}

.star:after {
  position: absolute;
  display: block;
  color: red;
  top: 0px;
  left: -7px;
  width: 0px;
  height: 0px;
  border-right: 7px solid rgba(0, 0, 0, 0);
  border-bottom: 5px solid #f0f0f0;
  border-left: 7px solid rgba(0, 0, 0, 0);
  transform: rotate(-70deg);
  content: "";
}

.moon.visible .star {
  opacity: 0.8;
}

.star.small {
  transform: scale(0.35) rotate(35deg);
  position: relative;
  top: 50%;
  left: 37.5%;
  opacity: 0;
  transition: all 0.2s ease-in 0.45s;
}

.moon.visible .star.small {
  opacity: 0.7;
  transform: scale(0.45) rotate(35deg);
}
</style>
