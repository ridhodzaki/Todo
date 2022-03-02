<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        @keyup.enter="addTask"
        v-model="newTask"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Add Task"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        @click="task.done = !task.done"
        v-for="(task, index) in tasks"
        :key="task.title"
        :class="{ 'done bg-blue-1': task.done }"
        v-ripple
        clickable
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>

    <!-- Total -->
    <q-item v-for="data in datas" :key="data.isi" v-ripple clickable>
      <q-item-section>
        <q-item-label>{{ data.isi }}</q-item-label>
      </q-item-section>
    </q-item>
    <div v-if="!tasks.length" class="no-task absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">No Tasks</div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: "PageIndex",
  data() {
    return {
      newTask: "",
      datas: [],
      tasks: [
        // {
        //   title: "Get",
        //   done: false,
        // },
        // {
        //   title: "Post",
        //   done: true,
        // },
        // {
        //   title: "Put",
        //   done: false,
        // },
      ],
    };
  },
  created: {
    total(isi) {
      for (isi in isi) {
        console.log(isi);
      }
    },
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Really delete?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          // console.log('>>>> OK')
          this.tasks.splice(index, 1);
          this.$q.notify("Task Delete");
        });
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false,
      });
      this.datas.isi = this.newTask;
      console.log(this.data);
      this.newTask = "";
    },
  },
};
</script>
<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-task {
  opacity: 0.5;
}
</style>
