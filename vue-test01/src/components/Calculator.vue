<template>
  <div class="calculator">
    <div v-show="error" class="calculator-error">Ошибка! {{ error }}</div>
    <div class="calculator-enter">
      <input v-model.number="operand1" type="number" />
      <input v-model.number="operand2" type="number" />
      <p>= {{ result }}</p>
      <!-- <p>= {{ fibResult }}</p> -->
    </div>
    <div class="calculator-keyboard">
      <button
        v-for="operand in operands"
        v-bind:key="operand"
        v-bind:title="operand"
        v-bind:disabled="isFillOperands"
        @click="calculate(operand)"
      >
        {{ operand }}
      </button>
      <button @click="clear">C</button>
    </div>

    <input type="checkbox" v-model="isVirtShow" id="virtshow" class="virtual" />
    <label for="virtshow">Show virtual keybord</label>
    <div class="virtual-box" v-show="isVirtShow">
      <div class="virtual-numkeys">
        <button
          v-for="numkey in numkeys"
          v-bind:key="numkey"
          v-bind:title="numkey"
          @click="setOperand(numkey, picked)"
        >
          {{ numkey }}
        </button>
        <button @click="del(picked)">del</button>
      </div>
      <div class="virtual-operands">
        <input
          type="radio"
          name="picked"
          value="0"
          v-model="picked"
          id="operand1"
        />
        <label for="operand1">first operand</label>
        <input
          type="radio"
          name="picked"
          value="1"
          v-model="picked"
          id="operand2"
        />
        <label for="operand2">second operand</label>
      </div>
    </div>

    <div class="strange-message" v-show="result != 0">
      <template v-if="result < 0">Получилось отрицательное число</template>
      <template v-else-if="result < 100">Результат в первой сотне</template>
      <template v-else>Получилось слишком большое число</template>
    </div>
    <div class="logs">
      <div v-for="(log, id) in logs" v-bind:key="id">{{ log }}</div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';

export default {
  name: 'Calculator',
  data() {
    return {
      operand1: 0,
      operand2: 0,
      result: 0,
      // fibResult: 0,
      error: '',
      numkeys: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'],
      operands: ['+', '-', '/', '*'],
      picked: 0,
      isVirtShow: false,
      logs: {}, // здесь будем хранить наши логи
    };
  },
  methods: {
    add() {
      this.result = parseInt(this.operand1) + parseInt(this.operand2);
      this.fibResult = this.fibb1 + this.fibb2;
    },
    substract() {
      this.result = this.operand1 - this.operand2;
      this.fibResult = this.fibb1 - this.fibb2;
    },
    divide() {
      if (this.operand2 === 0) {
        this.error = 'Делить на 0 нельзя!';
      } else {
        this.result = this.operand1 / this.operand2;
        this.fibResult = this.fibb1 / this.fibb2;
      }
    },
    multiply() {
      this.result = this.operand1 * this.operand2;
      this.fibResult = this.fibb1 * this.fibb2;
    },
    calculate(operation = '+') {
      this.error = '';
      switch (operation) {
        case '+':
          this.add();
          break;
        case '-':
          this.substract();
          break;
        case '*':
          this.multiply();
          break;
        case '/':
          this.divide();
          break;
      }
      const currentData = Date.now(); //это ключ, как id, это не дата
      // console.log(currentData);
      const value = `${this.operand1}${operation}${this.operand2}=${this.result}`;
      Vue.set(this.logs, currentData, value);
      // this.$set(this.logs, currentData, value);
    },
    clear() {
      this.error = '';
      this.result = this.operand1 = this.operand2 = 0;
    },
    fib(n) {
      return n <= 1 ? n : this.fib(n - 1) + this.fib(n - 2);
    },
    setOperand(numkey, operand) {
      let strNumber = '';
      if (operand == 0) {
        strNumber = this.operand1.toString();
        strNumber += numkey;
        console.log(strNumber);
        this.operand1 = parseInt(strNumber);
      }
      if (operand == 1) {
        strNumber = this.operand2.toString();
        strNumber += numkey;
        this.operand2 = parseInt(strNumber);
      }
    },
    del(operand) {
      let strNumber = '';
      if (operand == 0) {
        strNumber = this.operand1.toString();
        strNumber = strNumber.slice(0, -1) == '' ? '0' : strNumber.slice(0, -1);
        this.operand1 = parseInt(strNumber);
      }
      if (operand == 1) {
        strNumber = this.operand2.toString();
        strNumber = strNumber.slice(0, -1) == '' ? '0' : strNumber.slice(0, -1);
        this.operand2 = parseInt(strNumber);
      }
    },
  },
  computed: {
    isFillOperands() {
      return this.operand1 == 0 || this.operand2 == 0;
    },
    // fibb1() {
    //   return this.fib(this.operand1);
    // },
    // fibb2() {
    //   return this.fib(this.operand2);
    // },
  },
};
</script>

<style lang="scss" scoped>
.calculator {
  margin-top: 30px;
  padding: 40px;
  background-color: lavenderblush;
  color: darkblue;
  font-size: 24px;
  &-enter {
    border: 1px solid darkblue;
    border-bottom: transparent;
    width: 98%;
    margin: 0 auto;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    padding-top: 30px;
    & input {
      box-sizing: border-box;
      display: block;
      width: 94%;
      margin-bottom: 20px;
      padding: 10px;
      outline: none;
      border: none;
      font: inherit;
      color: inherit;
    }
  }
  &-keyboard {
    border: 1px solid darkblue;
    border-top: transparent;
    width: 98%;
    margin: 0 auto;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: row;
    gap: 10px;
    padding-bottom: 30px;
    & button {
      display: block;
      margin-bottom: 20px;
      padding: 10px;
      outline: none;
      border: 1px solid darkblue;
      font: inherit;
      color: inherit;
      box-shadow: 5px 4px 10px 0 #00008b70;
      background-color: lavenderblush;
      cursor: pointer;
      &:disabled {
        cursor: default;
        box-shadow: none;
      }
    }
  }
  &-error {
    color: red;
  }
}
.virtual {
  margin-top: 40px;

  & label {
    font-size: 20px;
  }
}
.strange-message {
  margin-top: 30px;
}
.logs {
  margin-top: 30px;
}
</style>
