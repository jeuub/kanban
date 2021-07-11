<template>
  <div class="container">
    <header>
      <h1>Убийца Убийцы трелло</h1>
      <b-button class="button" @click="toggleTheme">Смена темы</b-button>
    </header>
    <b-button class="col-2" variant="danger" @click="open = true"
      >Создать</b-button
    >
    <vue-modaltor
      :visible="open"
      @hide="open = false"
      bg-overlay="rgba(128, 128, 128, 0.644)"
    >
      <template #header>
        <div class="col-12 row pt-3">
          <h2 class="text-center">Добавьте новую карточку</h2>
        </div>
      </template>
      <template #body>
        <div class="col-12 px-3">
          <b-form @submit="add" @reset="reset" class="mb-5 py-2">
            <b-form-input
              id="input-1"
              v-model="newTask.name"
              placeholder="Новая карточка"
              required
            ></b-form-input>
            <b-form-textarea
              class="mt-3"
              id="textarea"
              placeholder="Описание"
              v-model="newTask.description"
              rows="3"
            ></b-form-textarea>
            <b-form-select
              class="form-select mt-3 mb-3"
              id="input-2"
              v-model="newTask.level"
              :options="levels"
              required
            ></b-form-select>
            <div class="row pt-3">
              <b-button type="submit" variant="primary" class="mx-auto col-2"
                >Создать</b-button
              >
              <b-button type="reset" variant="danger" class="mx-auto col-2"
                >Выйти</b-button
              >
            </div>
          </b-form>
        </div>
      </template>
    </vue-modaltor>
    <div class="row mt-3">
      <div class="col-md-4">
        <div class="alert backlog">
          <h3 class="text-center">План {{ arrBacklog.length }}</h3>
          <card
            v-bind:del="del"
            v-bind:edit="edit"
            v-bind:left="leftOne"
            v-bind:right="rightOne"
            namearr="backlog"
            v-bind:arr="arrBacklog"
          ></card>
        </div>
      </div>

      <div class="col-md-4">
        <div class="alert inprog">
          <h3 class="text-center">В работе {{ arrInProgress.length }}</h3>
          <card
            v-bind:del="del"
            v-bind:edit="edit"
            v-bind:left="leftOne"
            v-bind:right="rightTwo"
            namearr="progress"
            v-bind:arr="arrInProgress"
          ></card>
        </div>
      </div>

      <div class="col-md-4">
        <div class="alert done">
          <h3 class="text-center">Готово {{ arrDone.length }}</h3>
          <card
            v-bind:del="del"
            v-bind:edit="edit"
            v-bind:left="leftTwo"
            v-bind:right="rightTwo"
            namearr="done"
            v-bind:arr="arrDone"
          ></card>
        </div>
      </div>
    </div>
    <footer>Бодур Эййюб 201-321</footer>
  </div>
</template>

<script>
import card from "./components/card.vue";

export default {
  name: "App",
  components: {
    card,
  },

  mounted() {
    localStorage.getItem("theme") == "theme-light"
      ? (document.documentElement.className = "theme-light")
      : (document.documentElement.className = "theme-dark");
      console.log(localStorage.getItem("theme"))
  },

  data() {
    return {
      levels: [1, 2, 3],
      amount: 1,
      newTask: {
        name: null,
        date: null,
        level: 1,
        number: null,
        description: null,
      },
      arrBacklog: [
        {
          name: "баг",
          date: "12.7.2021",
          level: 1,
          description: "Сделать загрузку в локал сторедж",
          number: 1,
        },
      ],
      arrInProgress: [],
      arrDone: [],
      open: false,
    };
  },
  methods: {
    add(event) {
      event.preventDefault();
      if (this.newTask) {
        this.amount++;
        this.arrBacklog.push({
          name: this.newTask.name,
          level: this.newTask.level,
          number: this.amount,
          description: this.newTask.description,
          date: this.makeCalculate(),
        });
        this.newTask = {};
        this.open = false;
      }
    },
    reset() {
      this.newTask.name = null;
      this.newTask.level = null;
      this.newTask.date = null;
      this.newTask.description = null;
      this.open = false;
    },
    edit(namearr, index, editTask) {
      if (editTask) {
        switch (namearr) {
          case "backlog":
            this.arrBacklog.splice(index, 1);
            this.arrBacklog.push({
              name: editTask.name,
              level: editTask.level,
              description: editTask.description,
              date: this.makeCalculate(),
            });
            break;
          case "progress":
            this.arrInProgress.splice(index, 1);
            this.arrInProgress.push({
              name: editTask.name,
              level: editTask.level,
              description: editTask.description,
              date: this.makeCalculate(),
            });
            break;
          case "done":
            this.arrDone.splice(index, 1);
            this.arrDone.push({
              name: editTask.name,
              level: editTask.level,
              description: editTask.description,
              date: this.makeCalculate(),
            });
            break;
        }
      }
    },
    makeCalculate() {
      return this.$moment().format("DD.MM.YYYY");
    },
    setTheme(themeName) {
      localStorage.setItem("theme", themeName);
      document.documentElement.className = themeName;
    },
    toggleTheme() {
      if (localStorage.getItem("theme") === "theme-dark") {
        this.setTheme("theme-light");
      } else {
        this.setTheme("theme-dark");
      }
    },
    rightOne(index, element) {
      this.arrBacklog.splice(index, 1), this.arrInProgress.push(element);
    },
    leftOne(index, element) {
      this.arrInProgress.splice(index, 1), this.arrBacklog.push(element);
    },
    rightTwo(index, element) {
      this.arrInProgress.splice(index, 1), this.arrDone.push(element);
    },
    leftTwo(index, element) {
      this.arrDone.splice(index, 1), this.arrInProgress.push(element);
    },
    del(index, arr) {
      arr.splice(index, 1);
    },
  },
};
</script>

<style>
@import "./style.css";
</style>
