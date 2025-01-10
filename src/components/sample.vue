<template>
    <div class="container">
        <div class="splitter">
            <h1>SPLITTER</h1>
            <div class="input-section">
                <div class="input-group">
                    <label for="bill">Bill</label>
                    <input type="number" v-model="bill" id="bill" />
                </div>
                <div class="input-group">
                    <label>Select Tip %</label>
                    <div class="tip-buttons">
                        <button v-for="tip in tipOptions" :key="tip" @click="selectTip(tip)">
                            {{ tip }}%
                        </button>
                        <button @click="customTip = true">Custom</button>
                    </div>
                    <input v-if="customTip" type="number" v-model="customTipValue" placeholder="Enter custom tip %" />
                </div>
                <div class="input-group">
                    <label for="people">Number of People</label>
                    <input type="number" v-model="people" id="people" />
                </div>
            </div>
            <div class="output-section">
                <div class="output-group">
                    <label>Tip Amount / person</label>
                    <div class="output-value">{{ tipAmountPerPerson }}</div>
                </div>
                <div class="output-group">
                    <label>Total / person</label>
                    <div class="output-value">{{ totalPerPerson }}</div>
                </div>
                <button @click="reset">RESET</button>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            bill: 142.55,
            tipOptions: [5, 10, 15, 25, 50],
            selectedTip: 15,
            customTip: false,
            customTipValue: 0,
            people: 5,
        };
    },
    computed: {
        tipAmountPerPerson() {
            const tipPercentage = this.customTip ? this.customTipValue : this.selectedTip;
            const tipAmount = (this.bill * tipPercentage) / 100;
            return (tipAmount / this.people).toFixed(2);
        },
        totalPerPerson() {
            const tipPercentage = this.customTip ? this.customTipValue : this.selectedTip;
            const tipAmount = (this.bill * tipPercentage) / 100;
            const totalAmount = this.bill + tipAmount;
            return (totalAmount / this.people).toFixed(2);
        },
    },
    methods: {
        selectTip(tip) {
            this.selectedTip = tip;
            this.customTip = false;
        },
        reset() {
            this.bill = 0;
            this.selectedTip = 15;
            this.customTip = false;
            this.customTipValue = 0;
            this.people = 1;
        },
    },
};
</script>
<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #e0f7fa;
}

.splitter {
  background-color: #ffffff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  margin-bottom: 20px;
}

.input-section,
.output-section {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.input-group,
.output-group {
  display: flex;
  flex-direction: column;
}

label {
  font-weight: bold;
  margin-bottom: 5px;
}

input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.tip-buttons {
  display: flex;
  gap: 10px;
}

button {
  padding: 10px;
  border: none;
  border-radius: 5px;
  background-color: #00796b;
  color: #ffffff;
  cursor: pointer;
}

button:hover {
  background-color: #004d40;
}

.output-value {
  font-size: 1.5em;
  font-weight: bold;
}
</style>
