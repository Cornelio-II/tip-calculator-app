<template>
    <div class="bill splitter">
        <div class="container">
            <div class="input-section">
                <div class="input-group">
                    <label for="bill">Bill</label>
                    <input id="bill" v-model.number="bill" placeholder="0.00">
                </div>
                <div class="input-group">
                    <label>Select Tip %</label>
                    <div class="tip-options">
                        <button v-for="tip in tipOptions" :key="tip" @click="selectTip(tip)"
                         :class="{ active: selectedTip === tip }">{{ tip
                            }}%</button>
                        <input 
                        id="customTipValue" v-model.number="customTipValue" @input="onCustomTipInput"
                        placeholder="Custom" />
                    </div>
                    <div class="input-group">
                        <label for="people" id="people">Number of People</label>
                        <input id="people" v-model.number="people">
                    </div>
                </div>
            </div>
            <div class="output-section">
                <div class="output-group">
                    <p>Tip Amount / person</p>
                    <div class="output-value">{{ formattedTipAmountPerPerson }}
                    </div>
                </div>
                <div class="output-group">
                    <p>Total / person</p>
                    <div class="output-value">{{ formattedTotalPerPerson }}
                    </div>
                </div>
                <button class="reset-button" :disabled="!canReset" @click="reset">
                    RESET
                </button>
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
            customTipValue: null,
            people: 5,
            isCustomTipActive: false,
        };
    },
    computed: {
        tipAmountPerPerson() {
            let tipAmount = 0;
            let billAmount = parseFloat(this.bill) || 0.00;
            let peopleCount = parseFloat(this.people) || 1;

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
            let billAmount = parseFloat(this.bill) || 0.00;
            let peopleCount = parseFloat(this.people) || 1;

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
            } else {
                return this.tipAmountPerPerson;
            }
        },
        formattedTotalPerPerson() {
            // Ternary operator
            // return isNaN(this.totalPerPerson) ? "0.00" : this.totalPerPerson;
            if (isNaN(this.totalPerPerson)) {
                return "0.00";
            } else {
                return this.totalPerPerson;
            }
        },
        canReset(){
            // Ternary operator
             return this.bill || this.people || this.selectedTip || this.customTipValue;

             //same as above code 
            // if (this.bill || this.people || this.selectedTip || this.customTipValue){
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
        onCustomTipInput(){
          this.isCustomTipActive = true;
        },
        reset() {
            this.bill = null;
            this.selectedTip = null;
            this.customTipValue = null;
            this.people = 1;
            this.isCustomTipActive = false; 
        }
    }
}
</script>

<style scoped>
.container{
    display: flex;
    background-color: var(--white);
    border-radius: 15px;
    box-shadow: 0 10px 15px rgba(0, 0, 0, 0.2);
    overflow: hidden;
}

.input-section{
    background-color: var(--white);
}
.output-section {
    background-color: var(--very-dark-cyan);
    color: var(--white);
}
.input-section, .output-section{
 padding: 2rem;
 flex: 1;
}
</style>