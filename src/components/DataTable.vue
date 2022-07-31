<template>
  <div class="data-table">
    <div class="data-table__filter">
      <!-- My code starts here -->
      <div class="filterContainer">
        <!-- поиск сделал по id. В задании не было сказано как искать решил по id искать, можно сделать любой поиск-->
        <input class="searchInput" createF="createF" v-bind:value ="moneyFilter" @change="createF"/> Enter number of element
      <div class="filterSearch" v-if="moneyFilter !== 0">
        <span>Search result: ID: {{rt[moneyFilter-1].id}}, Date: {{rt[moneyFilter-1].date}}, Name: {{rt[moneyFilter-1].name}}, Money: {{rt[moneyFilter-1].money}}</span>
      </div>
      </div>
    </div>
      <div create="create" v-if="moneyFilter !== 0"></div>
        <div class="groupBox invisible">
          <div class="itemBoxLength" itemsLength="itemsLength" v-for="item,idx in itemsLength" :key='item'>
            <div class="itemBox"><span>{{keyItems[idx]}}</span> </div>
          </div>
        </div>
      <div :key='item.id' v-for="(item) in limitItems">
          <div class="groupBox invisible">
              <div class="itemBox  ">{{ item.id }} </div>
              <div class="itemBox  ">{{ item.date }}</div>
              <div class="itemBox  ">{{ item.money }}</div>
              <div class="itemBox  ">{{ item.name }}</div>
          </div>
          <div class="border">
        <div class="groupBox visible" :key='item1.name' v-for="(item1) in itemsLength">
        <!-- при схлоповании макета добавил ID uppercase, на основном макете поленился только с заглавной буквы, сделал максимально
         чтобы можно было применять компонент в дальнейшем (все необходимое в computed свойствах). Нужно только стили в scss и убрать массив значений в одельный файл -->
          <div class="itemBox idItem" v-if="(item1-1) === 0">{{keyItems [item1-1]}}
             <div> {{ item[keyItems[item1-1]] }}</div>
            </div>
            <div class="itemBox idItemFirstLetter" v-if="(item1-1) !== 0"><span>{{keyItems [item1-1]}}</span>
             <div> {{ item[keyItems[item1-1]] }}</div>
            </div>
        </div>
          </div>
    </div>
    <div class="data-table__paginator">
      <!-- пагинация работает, даже если расширять массив данных добавляются страницы по 4 элемента -->
      <ui-pagination @click="limitItems"
        v-model="page"
        :pages="pageCount"
      ></ui-pagination>
    </div>
  </div>
</template>

<script>
export default {

  name: 'DataTable',
  props: {
    rows: {
      type: Array,
      required: true,
    },
    columns: {
      type: Array,
      required: true,
    },
    isCashed: {
      type: Boolean,
      value: true,
    },
  },

  data: () => ({
    page: 1,
    pageSize: 4,
    moneyFilter: 0,
    date: [],
    rt: [
      {
        id: 1,
        date: '2020-02-24',
        name: 'Bob Lee',
        money: 1000,
      },
      {
        id: 2,
        date: '2020-02-25',
        name: 'John Seek',
        money: 1999.99,
      },
      {
        id: 3,
        date: '2020-02-26',
        name: 'Harry Smith',
        money: 3000.5,
      },
      {
        id: 4,
        date: '2020-02-27',
        name: 'Alex Morphy',
        money: 4000,
      },
      {
        id: 5,
        date: '2020-02-28',
        name: 'Ben Gold',
        money: 5000,
      },
      {
        id: 6,
        date: '2020-02-28',
        name: 'Tim Black',
        money: 10000,
      },
      {
        id: 7,
        date: '2020-02-30',
        name: 'Jimmy Elephant',
        money: 1000000,
      },
    ],
  }),
  // Добавил метод
  methods: {
    createF(event) {
      this.moneyFilter = event.target.value;
      return this.moneyFilter;
    },
  },
  computed: {
    pageCount() {
      return Math.ceil(this.rt.length / this.pageSize);
    },
    // Добавил несколько computed
    itemsLength() {
      console.log(this.keyItems.length);
      return this.keyItems.length;
    },
    keyItems() {
      const keyItem = this.rt.find((item) => item.id === 1);
      return Object.keys(keyItem);
    },
    // в данном свойстве сделал запись в LocalStorage, кнопка clear cache очищает хранилище.
    // Просто надо было что-то с кэшем делать, в задании ничего не сказано. Без сохранения кэша, получается
    // наличие избыточного интерфейса. ПРОВЕРЬТЕ LOCAL STORAGE в браузере.
    limitItems() {
      // if (this.page) {
      const start = (this.page - 1) * this.pageSize;
      console.log(`start ${start}`);
      const end = start + this.pageSize;
      localStorage.setItem('test', JSON.stringify(this.rt));
      return this.rt.slice(start, end);
    },
    valueItems() {
      const valueItem = this.rt.find((item) => item.id === this.moneyFilter);
      return Object.values(valueItem);
    },
  },
};
</script>
<!-- Стили добавил в самом компонент. Верстка на флексах, все ок. Адаптация по 768px работает -->
<style>
  .itemBoxLength {
    width: 100%;
    display: flex;
    justify-content: space-around;
  }
  .filterSearch {
    margin-top: 30px;
    margin-bottom: 30px;
  }
  .filterContainer {
    display: flex;
    align-items: flex-end;
    flex-direction: column;
  }
  .idItem{
    text-transform: uppercase;
  }
  .searchInput {
    border: 1px solid black;
     width: 100px;
     margin-bottom: 5px;

  }
  span::first-letter{
    text-transform: uppercase;
  }
  .groupBox{
    border: 1px solid rgb(0, 0, 0);
    display: flex;
    justify-content: space-around;
  }
  .itemBox{
    width: 100%;
    padding: 5px;
    display: flex;
    justify-content: space-around;
    border-left: 1px solid black;
    border-right: 1px solid black;
  }
   .visible {
    display: none;
    border-right: 1px solid black;
  }

@media(max-width: 768px) {
  .groupBox{
    border: 1px solid rgb(0, 0, 0);
    display: flex;
    flex-direction: column;
  }
  .itemBox{
    border: none;
    text-align: center;
    flex-direction: column;
  }

  .invisible {
    display: none;
  }
  .visible {
    display: flex;
    border: none;
  }
  .border{
    border: 1px solid black;
  }
}
</style>
