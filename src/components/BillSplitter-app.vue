<script>
export default {
  data() {
    return {
      bill: null,
      tipOptions: [5, 10, 15, 25, 50],
      selectedTip: null,
      customTipValue: null,
      people: null,
      isCustomTipActive: false,
    };
  },
  computed: {
    tipAmountPerPerson() {
      let tipAmount = 0;
      let billAmount = parseFloat(this.bill) || 0.0;
      let peopleCount = parseFloat(this.people);

      let tipPercentage;
      if (this.isCustomTipActive && this.customTipValue > 0) {
        tipPercentage = this.customTipValue;
      } else {
        tipPercentage = this.selectedTip;
      }
      if (!isNaN(billAmount) && !isNaN(peopleCount)) {
        tipAmount = (billAmount * tipPercentage) / 100;
        return (tipAmount / peopleCount).toFixed(2);
      } else {
        return "0.00";
      }
    },
    totalPerPerson() {
      let tipAmount = 0;
      let billAmount = parseFloat(this.bill) || 0.0;
      let peopleCount = parseFloat(this.people);

      let tipPercentage;
      let totalTipAmount;
      if (this.isCustomTipActive && this.customTipValue > 0) {
        tipPercentage = this.customTipValue;
      } else {
        tipPercentage = this.selectedTip;
      }
      if (!isNaN(billAmount) && !isNaN(peopleCount)) {
        tipAmount = (billAmount * tipPercentage) / 100;
        totalTipAmount = billAmount + tipAmount;
        return (totalTipAmount / peopleCount).toFixed(2);
      } else {
        return "0.00";
      }
    },
    formattedTipAmountPerPerson() {
      // Ternary operator
      // return isNaN(this.tipAmountPerPerson) ? "0.00" : this.tipAmountPerPerson;

      // Same as above code
      if (isNaN(this.tipAmountPerPerson)) {
        return "0.00";
      } else if (
        this.people === 0 ||
        this.bill === 0 ||
        this.people < 0 ||
        this.bill < 0
      ) {
        return "0.00";
      } else {
        return this.tipAmountPerPerson;
      }
    },
    formattedTotalPerPerson() {
      // Ternary operator
      // return isNaN(this.totalPerPerson) ? "0.00" : this.totalPerPerson;
      if (isNaN(this.totalPerPerson)) {
        return "0.00";
      } else if (
        this.people === 0 ||
        this.bill === 0 ||
        this.people < 0 ||
        this.bill < 0
      ) {
        return "0.00";
      } else {
        return this.totalPerPerson;
      }
    },
    canReset() {
      // Ternary operator
      return (
        this.bill || this.people || this.selectedTip || this.customTipValue
      );

      //same as above code
      // if (this.bill || this.people || this.selectedTip || this.isCustomTipActive){
      //   return true;
      // } else {
      //   return false;
      // }
    },
  },
  methods: {
    selectTip(tip) {
      this.selectedTip = tip;
      // this.customTipValue = null; // Clear customTipValue when a standard tip is selected

      this.isCustomTipActive = false;
    },
    onCustomTipInput() {
      this.isCustomTipActive = true;
    },
    reset() {
      this.bill = null;
      this.selectedTip = null;
      this.customTipValue = null;
      this.people = null;
      this.isCustomTipActive = false;
    },
  },
};
</script>

<template>
  <main>
    <article>
      <header>
        <img src="/src/assets/images/logo.svg" alt="billSplitterlogo.svg" />
      </header>
      <section>
        <div class="container">
          <div class="input-section">
            <label for="bill">Bill</label>
            <input id="bill" v-model.number="bill" placeholder="0.00" />
            <img
              class="icon"
              src="/src/assets/images/icon-dollar.svg"
              alt="Dollar Icon"
            />
            <span v-if="bill === 0" class="error-message">Can't be zero</span>
            <span v-else-if="bill < 0" class="error-message"
              >Can't be negative</span
            >

            <label for="selectTip">Select Tip %</label>
            <div class="tip-options">
              <button
                id="selectTip"
                v-for="tip in tipOptions"
                :key="tip"
                @click="selectTip(tip)"
                :class="{ active: selectedTip === tip }"
              >
                {{ tip }}%
              </button>
              <input
                id="customTipValue"
                v-model.number="customTipValue"
                @input="onCustomTipInput"
                placeholder="Custom"
              />
            </div>
            <label for="people">Number of People</label>
            <input id="people" v-model.number="people" placeholder="0" />
            <img
              class="icon"
              src="/src/assets/images/icon-person.svg"
              alt="Person Icon"
            />
            <span v-if="people === 0" class="error-message">Can't be zero</span>
            <span v-else-if="people < 0" class="error-message">
              Can't be negative</span
            >
          </div>
          <div class="output-section">
            <div class="output-group">
              <div class="label-name-output">
                <p >Tip Amount</p>
                <p class="label-person">/ person</p>
              </div>
              <span>${{ formattedTipAmountPerPerson }}</span>
              <div class="label-name-output">
                <p class="label-total">Total</p>
                <p class="label-person">/ person</p>
              </div>  
              <span>${{ formattedTotalPerPerson }}</span>
              <button class="reset-button" :disabled="!canReset" @click="reset">
                RESET
              </button>
            </div>
          </div>
        </div>
      </section>
      <footer>
          <p>
            Challenge by
            <a href="https://www.frontendmentor.io/challenges" target="_blank"
              >Frontend Mentor</a
            >. Coded by
            <a href="https://github.com/Cornelio-II/tip-calculator-app" target="_blank">Cornelio</a>.
          </p>
      </footer>
    </article>
  </main>
</template>

<style scoped>
.container {
  display: flex;
  margin-top: 2rem;
  justify-content: space-between;
  background-color: var(--white);
  border-radius: 15px;
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  width: 70vw;
  place-items: center;
}
.output-section {
  background-color: var(--very-dark-cyan);
  color: var(--white);
}
.input-section,
.output-section {
  border-radius: 15px;
  padding: 2rem;
  margin: 2rem;
  width: 100%;
  height: 460px;
  place-content: center;
}
.output-group {
  display: flex;
  flex-direction: column;
  padding: 0.3rem;
}
.tip-options {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  margin-top: 1rem;
  border-radius: 5px;
  transition: all 300ms ease-in-out;
}
.label-total {
  margin-top: 8rem;
}

.label-person {
  margin-top: -1.2rem;
  color: var(--grayish-cyan);
}
span {
  color: var(--strong-cyan);
  font-size: clamp(1.5rem, 1.1479rem + 1.5023vw, 2.5rem);
  margin-left: 100px;
  text-align: right;
  margin-top: -5rem;
  padding-left: 2rem;
}
.label-name-output{
  font-size: clamp(0.875rem, 0.831rem + 0.1878vw, 1rem);
  text-align: left;
}

.reset-button {
  display: flex;
  flex-direction: column;
  background-color: var(--strong-cyan);
  color: var(--very-dark-cyan);
  width: 100%;
  padding: 0.5rem;
  margin-top: 6rem;
}

.reset-button:hover {
  background-color: hsl(170, 82%, 49%);
}

.reset-button:disabled {
  background-color: var(--dark-grayish-cyan);
  cursor: not-allowed;
}

.icon {
  display: flex;
  margin-top: -1.3rem;
  padding-bottom: 15px;
  padding-left: 20px;
  transform: translateY(-50%);
  pointer-events: none;
}

.error-message {
  color: red;
  font-size: 0.9rem;
  margin-top: -5.5rem;
  padding-bottom: 5rem;
  display: block;
  text-align: right;
}

@media (min-width: 300px) and (max-width: 359px) {
  .container {
    display: flex;
    flex-direction: column;
    width: 100%;
    transition: all 300ms ease-in-out;
  }
  .input-section {
    width: 100%;
    height: auto;
  }

  .output-section {
    width: 90%;
    height: auto;
    padding: 1rem;
  }

  .tip-options {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
  }

  input {
    font-size: 20px;
  }
  input::placeholder {
    font-size: 1rem;
  }
  
  .error-message {
    position: relative;
    font-size: 15px;
    top: 5rem;
  }

  .label-person {
    margin-top: -10px;
  }

}
@media (min-width: 360px) and (max-width: 440px) {
  .container {
    display: flex;
    flex-direction: column;
    width: 100%;
    transition: all 300ms ease-in-out;
  }

  .input-section {
    width: 100%;
    height: auto;
  }

  .output-section {
    width: 90%;
    height: auto;
    padding: 1rem;
  }

  .tip-options {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
  }

  
  input::placeholder {
    font-size: 1.2rem;
  }
  .error-message {
    position: relative;
    font-size: 15px;
    top: 5rem;
  }

  .label-person {
    margin-top: -10px;
  }

}
@media (min-width: 441px) and (max-width: 599px) {
  .container {
    display: flex;
    flex-direction: column;
    width: 100%;
    transition: all 300ms ease-in-out;
  }

  .input-section {
    width: 100%;
    height: auto;
  }

  .output-section {
    width: 90%;
    height: auto;
    padding: 1rem;
  }

  .tip-options {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
  }
  .error-message {
    position: relative;
    font-size: 15px;
    top: 5rem;
  }

  .label-person {
    margin-top: -10px;
  }

}
@media (min-width: 600px) and (max-width: 849px) {
  .container {
    display: flex;
    flex-direction: column;
    width: 100%;
    transition: all 300ms ease-in-out;
  }

  .input-section {
    width: 100%;
    height: auto;
  }

  .output-section {
    width: 90%;
    height: auto;
    padding: 1rem;
  }

  .tip-options {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
  }

  .label-person {
    margin-top: -10px;
  }
}
@media (min-width: 850px) and (max-width: 1050px) {
  .container {
    display: flex;
    flex-direction: column;
    width: 100%;
    transition: all 300ms ease-in-out;
  }

  .input-section {
    width: 100%;
    height: auto;
  }

  .output-section {
    width: 90%;
    height: auto;
    padding: 1rem;
  }

  .tip-options {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
  }
  .label-person {
    margin-top: -10px;
  }
}
</style>
