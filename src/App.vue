<template>
  <div class="roulette-container">
    <Roulette
      ref="wheel"
      :items="items"
      @wheel-end="handleWheelEnd"
      :size="rouletteSize"
      display-indicator
      :centered-indicator="true"
      :indicator-position="'top'"
      :first-item-index="{ value: 0 }"
      :wheel-result-index="{ value: null }"
      :display-shadow="true"
      :duration="isResetting ? 0 : 5"
      :result-variation="0"
      :easing="'ease'"
      :counter-clockwise="false"
      :horizontal-content="false"
      :display-border="true"
      :base-display="true"
      :base-size="100"
      :base-display-shadow="false"
      :base-display-indicator="false"
      :base-background="'#000000'"
      @click="launchAndResetWheel"
    >
      <template #baseContent>
        <div class="wheel-base" @click.stop="launchAndResetWheel">Empezar</div>
      </template>
    </Roulette>
    <div v-if="showPopup" class="popup">
      <div class="popup-content">
        <div class="popup-close" @click="rejectGift">X</div>
        <p class="popup-text">
          ¡Has ganado {{ selectedItem ? selectedItem.name : "" }}!
        </p>
        <ConfettiExplosion
          v-if="showPopup"
          color="#b61924"
          :particleCount="200"
          :force="0.3"
        ></ConfettiExplosion>
        <div class="popup-buttons">
          <button class="popup-button" @click="acceptGift">Aceptar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Roulette } from "vue3-roulette";
import ConfettiExplosion from "vue-confetti-explosion";

export default {
  components: {
    Roulette,
    ConfettiExplosion,
  },
  data() {
    return {
      items: [
        {
          id: 1,
          name: "un bombón",
          htmlContent:
            "<img src='assets/candy_1f36c.png' style='height: 15%'/>",
          textColor: "white",
          background: "#3AD8CE",
          disabled: false,
          weight: 3,
        },
        {
          id: 2,
          name: "una pelota antiestrés",
          htmlContent:
            "<img src='assets/person-in-lotus-position_medium-light-skin-tone_1f9d8-1f3fc_1f3fc.png' style='height: 15%'/>",
          textColor: "white",
          background: "#FFC622",
          disabled: false,
          weight: 3,
        },
        {
          id: 3,
          name: "un bombón",
          htmlContent:
            "<img src='assets/candy_1f36c.png' style='height: 15%'/>",
          textColor: "white",
          background: "#3AD8CE",
          disabled: false,
          weight: 3,
        },
        {
          id: 4,
          name: "una licencia de Bounsel Flow Premium",
          htmlContent:
            "<img src='assets/bounsel_favicon_golden.png' style='height: 15%'/>",
          textColor: "white",
          background: "#4C20D0",
          disabled: false,
          weight: 1,
        },
        {
          id: 5,
          name: "un bombón",
          htmlContent:
            "<img src='assets/candy_1f36c.png' style='height: 15%'/>",
          textColor: "white",
          background: "#3AD8CE",
          disabled: false,
          weight: 3,
        },
        {
          id: 6,
          name: "una pelota antiestrés",
          htmlContent:
            "<img src='assets/person-in-lotus-position_medium-light-skin-tone_1f9d8-1f3fc_1f3fc.png' style='height: 15%'/>",
          textColor: "white",
          background: "#FFC622",
          disabled: false,
          weight: 3,
        },
        {
          id: 7,
          name: "un bombón",
          htmlContent:
            "<img src='assets/candy_1f36c.png' style='height: 15%'/>",
          textColor: "white",
          background: "#3AD8CE",
          disabled: false,
          weight: 3,
        },
        {
          id: 8,
          name: "una licencia de Bounsel Flow Standard",
          htmlContent:
            "<img src='assets/bounsel_favicon_white.png' style='height: 15%'/>",
          textColor: "white",
          background: "#6D46E2",
          disabled: false,
          weight: 1,
        },
        {
          id: 9,
          name: "un bombón",
          htmlContent:
            "<img src='assets/candy_1f36c.png' style='height: 15%'/>",
          textColor: "white",
          background: "#3AD8CE",
          disabled: false,
          weight: 3,
        },
        {
          id: 10,
          name: "una pelota antiestrés",
          htmlContent:
            "<img src='assets/person-in-lotus-position_medium-light-skin-tone_1f9d8-1f3fc_1f3fc.png' style='height: 15%'/>",
          textColor: "white",
          background: "#FFC622",
          disabled: false,
          weight: 3,
        },
      ],
      showPopup: false,
      selectedItem: null,
      isResetting: false,
      chocolateCount: localStorage.getItem("chocolateCount")
        ? JSON.parse(localStorage.getItem("chocolateCount"))
        : 0,
      stressGamesCount: localStorage.getItem("stressGamesCount")
        ? JSON.parse(localStorage.getItem("stressGamesCount"))
        : 0,
      premiumLicenseCount: localStorage.getItem("premiumLicenseCount")
        ? JSON.parse(localStorage.getItem("premiumLicenseCount"))
        : 0,
      standardLicenseCount: localStorage.getItem("standardLicenseCount")
        ? JSON.parse(localStorage.getItem("standardLicenseCount"))
        : 0,
      maxChocolates: localStorage.getItem("maxChocolates")
        ? JSON.parse(localStorage.getItem("maxChocolates"))
        : 140,
      maxStressGames: localStorage.getItem("maxStressGames")
        ? JSON.parse(localStorage.getItem("maxStressGames"))
        : 60,
      maxPremiumLicenses: localStorage.getItem("maxPremiumLicenses")
        ? JSON.parse(localStorage.getItem("maxPremiumLicenses"))
        : 2,
      maxStandardLicenses: localStorage.getItem("maxStandardLicenses")
        ? JSON.parse(localStorage.getItem("maxStandardLicenses"))
        : 1,
    };
  },
  computed: {
    rouletteSize() {
      if (window.innerWidth < 768) {
        return window.innerHeight * 0.4;
      } else {
        return window.innerWidth * 0.4;
      }
    },
  },
  methods: {
    spinWheel() {
      let enabledItems = this.items.filter(
        (item) => !item.disabled && !this.hasReachedMaxLimit(item)
      );

      if (enabledItems.length === 0) {
        alert(
          "Todos los elementos están deshabilitados o han alcanzado su límite máximo. No puedes girar la ruleta."
        );
        return;
      }

      let totalWeight = enabledItems.reduce(
        (total, item) => total + item.weight,
        0
      );
      let randomNum = Math.random() * totalWeight;
      for (let i = 0; i < enabledItems.length; i++) {
        if (randomNum < enabledItems[i].weight) {
          return enabledItems[i];
        }
        randomNum -= enabledItems[i].weight;
      }
      return enabledItems[0];
    },

    launchAndResetWheel() {
      if (!this.isResetting) {
        this.$refs.wheel.launchWheel();
      }
    },
    resetWheel() {
      this.isResetting = true;
      this.$refs.wheel.reset();

      setTimeout(() => {
        this.isResetting = false;
      }, 10);
    },
    handleWheelEnd(item) {
      if (!this.isResetting) {
        this.selectedItem = item;
        this.attempts = 0;
        this.checkItemAndRelaunchWheel();
      } else {
        this.isResetting = false;
      }
    },
    checkItemAndRelaunchWheel() {
      if (this.hasReachedMaxLimit(this.selectedItem) && this.attempts < 2) {
        this.attempts++;
        this.launchAndResetWheel();
      } else if (this.attempts >= 2) {
        let enabledItems = this.items.filter((item) => !item.disabled);
        this.selectedItem =
          enabledItems[Math.floor(Math.random() * enabledItems.length)];
        this.showPopup = true;
      } else {
        this.showPopup = true;
      }
    },

    hasReachedMaxLimit(item) {
      switch (item.name) {
        case "un bombón":
          return this.chocolateCount >= this.maxChocolates;
        case "una pelota antiestrés":
          return this.stressGamesCount >= this.maxStressGames;
        case "una licencia de Bounsel Flow Premium":
          return this.premiumLicenseCount >= this.maxPremiumLicenses;
        case "una licencia de Bounsel Flow Standard":
          return this.standardLicenseCount >= this.maxStandardLicenses;
        default:
          return false;
      }
    },

    handleMaxLimits() {
      if (this.selectedItem) {
        if (this.selectedItem.name === "un bombón") {
          this.chocolateCount++;
          localStorage.setItem(
            "chocolateCount",
            JSON.stringify(this.chocolateCount)
          );
        }
        if (this.selectedItem.name === "una pelota antiestrés") {
          this.stressGamesCount++;
          localStorage.setItem(
            "stressGamesCount",
            JSON.stringify(this.stressGamesCount)
          );
        }
        if (this.selectedItem.name === "una licencia de Bounsel Flow Premium") {
          this.premiumLicenseCount++;
          localStorage.setItem(
            "premiumLicenseCount",
            JSON.stringify(this.premiumLicenseCount)
          );
        }
        if (
          this.selectedItem.name === "una licencia de Bounsel Flow Standard"
        ) {
          this.standardLicenseCount++;
          localStorage.setItem(
            "standardLicenseCount",
            JSON.stringify(this.standardLicenseCount)
          );
        }
      }
    },

    acceptGift() {
      this.showPopup = false;
      this.$nextTick(() => {
        this.handleMaxLimits();
        this.resetWheel();
      });
    },
    rejectGift() {
      this.showPopup = false;
      this.$nextTick(() => {
        this.resetWheel();
      });
    },
  },
};
</script>

<style>
body {
  overflow: hidden;
}

.roulette-container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.popup-close {
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
  color: black;
}

.spin-button {
  margin-right: 20px;
  padding: 10px 20px;
  background-color: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
}

.wheel-base {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  user-select: none;
  font-family: "Open Sans", sans-serif;
  font-size: 18px;
  background-color: #e50f49;
  color: white;
}

.popup {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);
  width: 400px;
  font-family: "Open Sans", sans-serif;
}

.popup-content {
  display: flex;
  flex-direction: column;
  align-items: center;
}

@media screen and (max-width: 767px) {
  .popup {
    width: 90%;
    max-width: 300px;
  }
}

@media screen and (max-width: 767px) {
  .roulette-container {
    height: auto;
    padding: 20px;
  }
}

.popup-text {
  text-align: center;
  margin-bottom: 20px;
}

.popup-button {
  padding: 10px 20px;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  background: #4c20d0;
  color: white;
}

.popup-button-separator {
  width: 10px;
}

.wheel-container-indicator:before {
  left: 50%;
}
</style>
