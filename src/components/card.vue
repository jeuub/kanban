<template>
          <draggable class="list-group kanban-column" :list='arr' group='tasks'>
            <b-card no-body class="mb-1"  v-for="(elements, index) in arr" :key="elements">
                  <b-card-body :key="index + 'asdasd'">
                  <h2 class="text-center">Задача № : {{elements.number}}</h2>
                  <h3 class="text-center">Описание:</h3>                     
                  <b-card-text>{{elements.description}}</b-card-text>              
                  <b-card-text class="priority-idx" :priority="elements.level">{{elements.level}}</b-card-text>
                  <b-card-text>Дата создания : {{elements.date}}</b-card-text>
                  <div class="row mx-auto row-sdf">
                        <b-button v-bind:disabled="namearr == 'backlog'" class="col-8" variant="primary" @click="left(index, elements)">Назад</b-button>
                        <b-button class="col-8" variant="primary" @click="openView(elements.name, elements.level, elements.date, elements.description)">Изменить</b-button>
                        <b-button v-if = "namearr != 'done'" class="col-8" variant="primary" @click="right(index, elements)">Далее</b-button>
                        <b-button v-else class="col-8" variant="danger" @click="del(index, arr)">Удалить</b-button>
                  </div>
                  </b-card-body>
              <vue-modaltor :visible="open" @hide="open=false" bg-overlay="gray">
                <template #header>
                  <div class="col-12 row pt-3">
                    <h2 class="text-center">Изменить карточку</h2>
                  </div>
                </template>
                <template #body>
                    <div class="col-11 mx-auto">
                    <b-form @submit="edit(namearr, index, editTask)" @reset="reset(elements.name, elements.level, elements.date, elements.description)" class="mb-5">
                      <b-form-input
                        id="input-1"
                        v-model="editTask.name"
                        placeholder="Новая карточка"
                        required
                      ></b-form-input>
                      <b-form-textarea class="mt-3"
                      id="textarea"
                      placeholder="Описание"
                      v-model="editTask.description"
                      rows="3"
                    ></b-form-textarea>
                      <b-form-select class="form-select mt-3 mb-3"
                        id="input-2"
                        v-model="editTask.level"
                        :options="levels"
                        required
                      ></b-form-select>
                      <div class="row pt-3">
                      <b-button @click="open=false" type="submit" variant="primary" class="mx-auto col-2">Изменить</b-button>
                      <b-button type="reset" variant="danger" class="mx-auto col-2">Сбросить</b-button>
                      </div>
                    </b-form>
                    </div>
                  </template>
              </vue-modaltor>
            </b-card>
          </draggable>
</template>

<script>
import draggable from 'vuedraggable';
export default {
  name: 'card',
  components: {
    draggable
  },
  props: {
    arr: Array,
    right: Function,
    left: Function,
    namearr: String,
    edit: Function,
    del: Function
  },
  data(){
    return{
      levels: [1, 2, 3],
      editTask: {
        name: null,
        date: null,
        level: null,
        description: null
      },
      open: false
    }
  },
  methods:{
    openView(name, level, date, description){
      this.editTask.name = name
      this.editTask.level = level
      this.editTask.date = date
      this.editTask.description = description
      this.open=true
      console.log('working')
    },
    reset(name, level, date, description){
      this.editTask.name = name
      this.editTask.level = level
      this.editTask.date = date
      this.editTask.description = description
      this.open = false
    },
    naming(index){
      let naming = 'accordion-' + index
      return naming
    }
    }
}
</script>

<style scoped>
.kanban-column{
  min-height: 300px;
}
</style>
