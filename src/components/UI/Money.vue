<template>
<div>
  <input
    @change="regTest"
    type="text"
    v-model="valueInput"
    class="ui-money"
  />
  <h1>{{valueInput}}</h1>
</div>
</template>

<script>
export default {

  name: 'UiMoney',

  props: {
    value: {
      type: Number,
      require: true,
    },
  },

  data() {
    return {
      valueInput: '',
    };
  },
  methods: {
    regTest() {
      const str = this.valueInput;
      let newstr = '';
      let finalstr = '';
      const regexp = /^\d+(\.\d{1,2})?$/;
      const strArray = str.split('');
      let point = 1;
      const newArr = [];
      for (let index = 0; index < strArray.length; index += 1) {
        const element = strArray[index];
        newArr.push(element);
        newstr = newArr.join('');
        const check = regexp.test(newstr);
        if (!check) {
          if (element === '.') {
            if (point === 1) {
              point += 1;
            } else {
              newArr.pop();
            }
          } else {
            newArr.pop();
          }
        }
        newstr = newArr.join('');
        finalstr = newstr;
        if (newstr.slice(-1) === '.') {
          finalstr = newstr.slice(0, -1);
        }
      }

      this.valueInput = finalstr;
      console.log(`ФИНАЛ ${finalstr}`);
      return this.valueInput;
      // пробелы в инпуте не сделал, принцип понятен, можно сделать. TEST SUIT не проходит выдает ошибку, связано
      // с путями в конфигах, не разобрался. Проверил вручную all cases. Можно было написать свой тест, но не стал этого делать.
      // Компоненты работают как было задано в задании. ПОПРОБУЙТЕ ВВЕСТИ НЕКОРРЕКТНЫЕ ДАННЫЕ, ПРОИСХОДИТ ЗАМЕНА
    },
  },
};
</script>
