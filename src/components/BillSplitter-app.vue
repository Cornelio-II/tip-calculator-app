<template>
    <div class="container">
        <div class="splitter">
            <div class="input-section">
                <div class="input-group">
                    <label for="bill">Bill</label>
                    <input type="number" id="bill" v-model="bill" placeholder="0.00">
                </div>
                <div class="input-group">
                    <label>Select Tip %</label>
                    <div class="tip-options">
                        <button  v-for="tip in tipOptions" :key="tip" 
                        @click="selectTip(tip)">{{ tip }}%</button>
                        <!-- <button @click="customTip = true">Custom</button> -->
                        <input type="number"  v-model="customTipValue" placeholder="Custom" />
                    </div>
                    <div class="input-group">
                        <label for="people" id="people">Number of People</label>
                        <input type="number" id="people" v-model="people">
                    </div>
                </div>
            </div>
            <div class="output-section">
                <div class="output-group">
                    <label>Tip Amount / person</label>
                    <div class="output-value">{{ formattedTipAmountPerPerson }}
                    </div>
                </div>
                <div class="output-group">
                    <label>Total / person</label>
                    <div class="output-value">{{ formattedTotalPerPerson }}
                    </div>
                </div>
                <button @click="reset">RESET</button>
            </div>
        </div>
    </div>
</template>

<script>
export default{
    data(){
        return {
            bill: null,
            tipOptions: [5, 10, 15, 25, 50],
            selectedTip: null,
            customTipValue: null,
            people: 1
        };
    },
    computed: {
        tipAmountPerPerson() {
            let tipAmount = 0;
            let billAmount = parseFloat(this.bill) || 0.00;
            let peopleCount = parseFloat(this.people) || 1;

            let tipPercentage;
            if (this.customTipValue > 0) {
                 tipPercentage = this.customTipValue;    
            } else  {
                tipPercentage = this.selectedTip;
            }
            if (!isNaN(billAmount) && !isNaN(peopleCount)){
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
           if (this.customTipValue > 0) { 
                tipPercentage = this.customTipValue;
            } else {
                tipPercentage = this.selectedTip;
            } 
            if(!isNaN(billAmount) && !isNaN(peopleCount)){
               tipAmount = (billAmount * tipPercentage) / 100;
               totalTipAmount = billAmount + tipAmount;
               return (totalTipAmount / peopleCount).toFixed(2);
            } else {
              return "0.00";
            }
        },
        formattedTipAmountPerPerson(){
            // Ternary operator
            // return isNaN(this.tipAmountPerPerson) ? "0.00" : this.tipAmountPerPerson;

            // Same as above code 
            if(isNaN(this.tipAmountPerPerson)){
               return "0.00";
            } else {
                return this.tipAmountPerPerson;
            }
        },
        formattedTotalPerPerson(){
            // Ternary operator
            // return isNaN(this.totalPerPerson) ? "0.00" : this.totalPerPerson;
            if(isNaN(this.totalPerPerson)){
                return "0.00";
            } else {
                return this.totalPerPerson;
            }
        },
    },
    methods: {
        selectTip(tip) {
            this.selectedTip = tip;
            this.customTipValue = null // Clear customTipValue when a standard tip is selected
        },
        reset(){
            this.bill = null;
            this.selectedTip = null;
            this.customTipValue = null;
            this.people = 1;
        }
    }
}
</script>