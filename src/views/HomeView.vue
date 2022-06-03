<template>
  <div class="main">
    <div class="toDo__container">
      <div class="inputAndBtn">
        <input type="text" v-model="inputValue" />
        <button
          class="btn"
          @click="
            {
              addTask(inputValue);
            }
          "
        >
          Add to list
        </button>
      </div>
      <div class="progress__parent">
        <div
          :style="{ width: `${calculateProgress()}%` }"
          class="progress"
        ></div>
      </div>
      <div class="buttons">
        <button
          class="btn"
          @click="
            {
              changeFilter(`all`);
            }
          "
        >
          Show all
        </button>
        <button
          class="btn"
          @click="
            {
              changeFilter(`incomplete`);
            }
          "
        >
          Show incomplete
        </button>
        <button
          class="btn"
          @click="
            {
              changeFilter(`completed`);
            }
          "
        >
          Show complete
        </button>
      </div>
      <div v-for="(task, index) in filterBycompletion" :key="index">
        <input type="checkbox" v-model="task.completed" />
        <span
          @click="toggleCompleted(task)"
          :class="{ completed: task.completed }"
          >{{ task.title }}</span
        >
        <button class="remove" @click="removeTask(task)">X</button>
      </div>
    </div>
    <div class="services__container">
      <h2>Services</h2>
      <div
        :class="{ selected: service.isSelected, service }"
        @click="
          {
            toggleSelected(service);
          }
        "
        v-for="(service, index) in services"
        :key="index"
      >
        <span>{{ service.title }}</span>
        <span>${{ service.price.toFixed(2) }}</span>
      </div>
      <hr width="100%" />
      <div class="total">
        <span>Total:</span>
        <span>${{ calculateTotal().toFixed(2) }}</span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent } from "vue";
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src
export type task = {
  title: string;
  completed: boolean;
};
export type Service = {
  title: string;
  price: number;
  isSelected: boolean;
};
export default defineComponent({
  name: "HomeView",
  data: () => ({
    filter: "all",
    inputValue: "",
    totalSum: 0,
    progress: 0,
    tasks: [
      {
        title: "Aizskriet uz bod'",
        completed: false,
      },
    ],
    services: [
      {
        title: "Web Development",
        price: 300,
        isSelected: false,
      },
      {
        title: "Design",
        price: 400,
        isSelected: false,
      },
      {
        title: "Integration",
        price: 250,
        isSelected: false,
      },
      {
        title: "Training",
        price: 220,
        isSelected: false,
      },
    ],
  }),
  methods: {
    calculateTotal: function (): number {
      let total = 0;
      this.services.forEach((service) => {
        if (service.isSelected) {
          total += service.price;
        }
      });
      return total;
    },
    calculateProgress: function (): number {
      let completed = 0;
      this.tasks.forEach((task) => {
        if (task.completed) {
          completed++;
        }
      });
      return (completed / this.tasks.length) * 100;
    },
    changeFilter: function (filterValue: string) {
      this.filter = filterValue;
    },
    addTask: function (input: string) {
      if (input.length > 0) {
        this.tasks.push({
          title: input,
          completed: false,
        });
        this.inputValue = "";
      }
    },
    removeTask: function (task: task) {
      this.tasks.splice(this.tasks.indexOf(task), 1);
    },
    toggleCompleted: function (task: task) {
      task.completed = !task.completed;
    },
    toggleSelected: function (service: Service) {
      service.isSelected = !service.isSelected;
    },
  },
  computed: {
    filterBycompletion: function () {
      return this.tasks.filter((task) => {
        if (this.filter === "all") {
          return true;
        } else if (this.filter === "completed") {
          return task.completed;
        } else if (this.filter === "incomplete") {
          return !task.completed;
        }
      });
    },
    // incompleteTasks: function () {
    //   return this.tasks.filter((task) => !task.completed);
    // },
    // completeTasks: function () {
    //   return this.tasks.filter((task) => task.completed);
    // },
  },
});
</script>
<style lang="scss">
@import "../styles/main.scss";
</style>
