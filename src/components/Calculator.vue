<template>
    <div class="container">
        <div class="calculator">
            <div class="result">{{ displayValue || 0 }}</div>
            <div class="buttons">
                <div class="btn operator" @click="sqrt()">√</div>
                <div class="btn operator" @click="clearLast()">CE</div>
                <div class="btn operator" @click="clear()">C</div>
                <div class="btn operator" @click="action('/')">/</div>
                <div class="btn number" @click="addNum('7')">7</div>
                <div class="btn number" @click="addNum('8')">8</div>
                <div class="btn number" @click="addNum('9')">9</div>
                <div class="btn operator" @click="action('*')">*</div>
                <div class="btn number" @click="addNum('4')">4</div>
                <div class="btn number" @click="addNum('5')">5</div>
                <div class="btn number" @click="addNum('6')">6</div>
                <div class="btn operator" @click="action('-')">-</div>
                <div class="btn number" @click="addNum('1')">1</div>
                <div class="btn number" @click="addNum('2')">2</div>
                <div class="btn number" @click="addNum('3')">3</div>
                <div class="btn operator" @click="action('+')">+</div>
                <div class="btn number" @click="addNum('0')">0</div>
                <div class="btn number" @click="decimal()">.</div>
                <div class="btn equals" @click="equals()">=</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            displayValue: "",
            value: "",
            prevValue: "",
            operator: "",
            prevOperator: "",
            btnType: "",
        };
    },
    methods: {
        clear() {
            this.displayValue = "";
            this.value = "";
            this.prevValue = "";
            this.operator = "";
            this.btnType = "";
        },
        clearLast() {
            if (this.btnType === "number") {
                this.value = "";
                this.displayValue = "";
            }
        },
        action(btn) {
            this.prevOperator = this.operator;
            this.btnType = "operator";
            this.operator = btn;
            if (this.prevOperator) {
                try {
                    this.prevValue = eval(
                        this.prevValue + this.prevOperator + this.value
                    );
                } catch (err) {
                    return;
                }
            } else {
                this.prevValue = this.value;
            }
            this.displayValue = this.prevValue;
            this.value = "";
        },
        addNum(btn) {
            this.btnType = "number";
            if (this.value.length < 14) {
                this.value += btn + "";
                this.displayValue = this.value;
            }
        },
        decimal() {
            this.btnType = "number";
            if (!this.value) {
                this.value = "0" + this.value;
            }
            if (!this.value.includes(".")) {
                this.value += "." + "";
                this.displayValue = this.value;
            }
        },
        sqrt() {
            let tempValue = Math.sqrt(this.value).toString();
            this.value = tempValue.substr(0, 14);
            this.displayValue = this.value;
        },
        equals() {
            this.btnType = "operator";
            let tempValue = "";
            try {
                tempValue = eval(
                    this.prevValue + this.operator + this.value
                ).toString();
            } catch (err) {
                return;
            }
            this.value = tempValue.substr(0, 14);
            this.displayValue = this.value;
            this.prevValue = "";
            this.operator = "";
        },
    },
};
</script>

<style scoped>
.container {
    display: flex;
    justify-content: center;
}
.calculator {
    width: 240px;
    background: rgb(202, 193, 193);
    padding: 15px;
    border-radius: 10px;
}
.result {
    text-align: right;
    line-height: 30px;
    font-size: 28px;
    background: rgb(236, 233, 233);
    margin-top: 15px;
    margin-bottom: 15px;
    border-radius: 10px;
    padding: 10px;
}
.buttons {
    display: grid;
    grid-template-columns: auto auto auto auto;
    gap: 5px;
}
.btn {
    border: 1px solid;
    border-radius: 4px;
    text-align: center;
    line-height: 50px;
    font-size: 22px;
    min-width: 40px;
    cursor: pointer;
}
.btn:active {
    transform: scale(0.9);
}
.equals {
    grid-column-start: 3;
    grid-column-end: 5;
    background: rgb(5, 204, 5);
}
.equals:hover {
    background: rgb(12, 136, 12);
}
.number {
    background: rgb(201, 199, 199);
}
.number:hover {
    background: rgb(139, 139, 139);
}
.operator {
    background: rgb(179, 179, 179);
}
.operator:hover {
    background: rgb(139, 139, 139);
}
</style>