<template>
  <div class="calculator">
    <div class="display">{{ current || '0'}}</div>
    <div role="button" class="btn" @click="clear">C</div>
    <div role="button" class="btn" @click="sign">+/-</div>
    <div role="button" class="btn" @click="percent">%</div>
    <div role="button" class="btn operator" @click="setOperator('divide')">/</div>
    <div role="button" class="btn" @click="enter('7')">7</div>
    <div role="button" class="btn" @click="enter('8')">8</div>
    <div role="button" class="btn" @click="enter('9')">9</div>
    <div role="button" class="btn operator" @click="setOperator('multiply')">X</div>
    <div role="button" class="btn" @click="enter('4')">4</div>
    <div role="button" class="btn" @click="enter('5')">5</div>
    <div role="button" class="btn" @click="enter('6')">6</div>
    <div role="button" class="btn operator" @click="setOperator('subtract')">-</div>
    <div role="button" class="btn" @click="enter('1')">1</div>
    <div role="button" class="btn" @click="enter('2')">2</div>
    <div role="button" class="btn" @click="enter('3')">3</div>
    <div role="button" class="btn operator" @click="setOperator('add')">+</div>
    <div role="button" class="btn" @click="del">DEL</div>
    <div role="button" class="btn" @click="enter('0')">0</div>
    <div role="button" class="btn" @click="decimal">.</div>
    <div role="button" class="btn operator" @click="equalsTo">=</div>
  </div>
</template>

<script>
export default {
    data() {
        return{
            current: '',
            initialValue: '',
            operator: '',
            finalValue: '',
            reset: false
        }
    },
    methods: {
        clear() {
            this.current = '';
            this.operator = '';
            this.initialValue = '';
            this.finalValue = '';
        }, 
        sign() {
            const value = this.current.charAt(0) === "-" ? this.current.slice(1) : `-${this.current}`
            this.current = value === '-' ? '' : value;
        }, 
        percent() {
            this.current = `${parseFloat(this.current) / 100}`
        },
        enter(num) {
            this.current = this.reset ? '' : this.current  
            this.current += `${num}`
            this.reset = false
        },
        decimal() {
            this.current = this.reset ? '' : this.current 
            if(!this.current.includes('.')) {
                this.current === '' ? this.current = '0.' : this.current += '.'
                this.reset = false
            }
        },
        del() {
            this.current = this.current ? this.current.slice(0,-1) : this.current;
        },
        setOperator(operator) {
            this.operator = operator
            this.initialValue = this.current
            this.reset = true
        },
        equalsTo() {
            if(this.current.charAt(-1) !== '.') {
                this.finalValue = this.initialValue ? this.current : ''
                this.finalValue ? this.operation(this.operator, this.initialValue, this.finalValue) : null
                this.initialValue = ''
                this.operator = ''
                this.reset = true
            }
        },
        operation(operator, num1, num2) {
            switch (operator) {
                case 'add':
                    this.current = this.add(num1, num2)
                    this.initialValue = ''
                    this.finalValue = ''
                    break;
                case 'subtract':
                    this.current = this.subtract(num1, num2)
                    this.initialValue = ''
                    this.finalValue = ''
                    break;
                case 'multiply':
                    this.current = this.multiply(num1, num2)
                    this.initialValue = ''
                    this.finalValue = ''
                    break;
                case 'divide':
                    this.current = this.divide(num1, num2)
                    this.initialValue = ''
                    this.finalValue = ''
                    break;
            }
        },
        add(x,y) {
            return `${parseFloat(x) + parseFloat(y)}`
        },
        subtract(x,y) {
            return `${parseFloat(x) - parseFloat(y)}`
        },
        multiply(x,y) {
            return `${parseFloat(x) * parseFloat(y)}`
        },
        divide(x,y) {
            return `${parseFloat(x) / parseFloat(y)}`
        },
    }
}
</script>

<style scoped>
    .calculator {
        width: 360px;
        margin: 0 auto;
        font-size: 32px;
        text-align: center;
        color: black;
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-auto-rows: minmax(50px, auto);
        border: 1px solid #999;
    }
    .display {
        grid-column: 1/5;
        background: #333;
        color: #ffff;
        border: 1px solid #999;
        text-align: end;
        padding-right: 10px;
    }
    .zero {
        grid-column: 1/3;
    }
    .btn {
        cursor: pointer;
        background: #eeee;
        border: 1px solid #333;
    }
    .btn:active {
        opacity: 0.8;
        transition: 0.4s;
        transform: scaleZ(10);
    }

    .operator {
        background: orangered;
        color: #ffff;
    }
</style>

