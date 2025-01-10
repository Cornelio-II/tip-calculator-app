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
                    <div class="output-value">{{ tipAmountPerPerson }}
                    </div>
                </div>
                <div class="output-group">
                    <label>Total / person</label>
                    <div class="output-value">{{ totalPerPerson }}
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
            people: null,
        };
    },
    computed: {
        tipAmountPerPerson() {
            let tipAmount;
            if (this.customTipValue) {
                tipAmount = (this.bill * this.customTipValue) / 100; 
            } else  {
                tipAmount = (this.bill * this.selectedTip) / 100; 
            }

            return (tipAmount / this.people).toFixed(2);
},
        
        totalPerPerson() {
            let tipAmount;
            let totalTipAmount ; 
           if (this.customTipValue) { 
                tipAmount = (this.bill * this.customTipValue) / 100;

            } else {
                tipAmount = (this.bill * this.selectedTip) / 100;
            } 
            totalTipAmount = this.bill + tipAmount; 
            return (totalTipAmount / this.people).toFixed(2);
        }
       
    },
    methods: {
        selectTip(tip) {
            this.selectedTip = tip;
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