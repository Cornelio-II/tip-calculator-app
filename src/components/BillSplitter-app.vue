<template>
   <div class="container">
        <div class="input-section">
                <label for="bill">Bill</label>
                <input id="bill" v-model.number="bill" placeholder="0.00">

                <label>Select Tip %</label>
                <div class="tip-options">
                    <button v-for="tip in tipOptions" :key="tip" @click="selectTip(tip)"
                        :class="{ active: selectedTip === tip }">{{ tip }}%
                    </button>
                    <input id="customTipValue" v-model.number="customTipValue"
                        @input="onCustomTipInput" placeholder="Custom" />
                </div>
                <label for="people" id="people">Number of People</label>
                <input id="people" v-model.number="people" placeholder="0">
        </div>
        <div class="output-section">
            <p>Tip Amount / person</p>
            <div class="output-value">${{ formattedTipAmountPerPerson }}</div>

            <p>Total / person</p>
            <div class="output-value">${{ formattedTotalPerPerson }}</div>

            <button class="reset-button" :disabled="!canReset" @click="reset">RESET</button>
            </div>
    </div>
</template>

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
            this.people = null;
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
    box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
    overflow: hidden;
}
.input-section{ 
    margin-top: -1rem;
}
.output-section {
    background-color: var(--very-dark-cyan);
    color: var(--white);
    margin: 1.6rem;
    border-radius: 15px;
}
.input-section, .output-section{
    padding: 2rem;
    flex: 1;
}
.output-value{
   color: var(--strong-cyan);
   font-size: 46px;
}
 .tip-options{
    display: grid;
    grid-template-columns:  repeat(3, 1fr);
    gap: 1rem;
    margin-top: 1rem;  
    border-radius: 5px;
    transition: all 300ms ease-in-out;
}
.customTip{
    padding: 0.5rem 1rem;
    font-size: 1.6rem;
}
.reset-button {
    background-color: var(--strong-cyan);
    color: var(--very-dark-cyan);
    width: 100%;
    padding: 0.5rem;
    margin-top: 2rem;
}
.reset-button:disabled{
    background-color: var(--dark-grayish-cyan);
    cursor: not-allowed;
}
@media (max-width: 768px){
   .container{
    flex-direction: column;
   }
}
</style>