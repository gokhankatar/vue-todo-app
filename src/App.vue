<template>
  <teleport to='body'>
    <i :class="darkMode ? 'bx bxs-sun' : 'bx bxs-moon'" id="modeIcon" @click="darkMode = !darkMode"></i>
  </teleport>
  <div class="top">
    <h3>Total Tasks : {{ totalTasks }}</h3>
    <h3>Completed Tasks : {{ completedTasks }}</h3>
    <h3>Remain Tasks : {{ remainTasks }}</h3>
  </div>
  <div class="bottom">
    <input type="text" v-model="model" placeholder="Write your task :)" id="input" :class="{ 'editing': isEdit }"
      @keypress.enter="addTask">
    <ul class="tasks" v-for="(item, index) of dataList" :key="index">
      <li>
        <span class="text" :class="{ completed: item.complete }" @click="completeTask(item)">
          {{ item.text }}
        </span>
        <div class="icons">
          <i class='bx bxs-edit-alt' id="editIcon" @click="editTask(item, index)"></i>
          <i class='bx bxs-trash' id="trashIcon" @click="deleteTask(item, index)"></i>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
/* eslint-disable */

export default {
  name: 'App',
  components: {

  },
  data() {
    return {
      model: '',
      dataList: [],
      isEdit: false,
      darkMode: false,
    }
  },
  methods: {
    addTask() {
      this.dataList.push({
        text: this.model,
        complete: false
      });
      this.model = '';
      this.isEdit = false;
    },
    completeTask(item) {
      item.complete = !item.complete
    },
    deleteTask(item, index) {
      this.dataList.splice(index, 1);
    },
    editTask(item, index) {
      this.model = item.text;
      this.isEdit = true;
      this.dataList.splice(index, 1);
    }
  },
  computed: {
    totalTasks() {
      return this.dataList.length;
    },
    completedTasks() {
      return this.dataList.filter(data => data.complete).length;
    },
    remainTasks() {
      return this.dataList.filter(data => !data.complete).length;
    }
  },
  watch: {
    darkMode() {
      document.body.classList.toggle('dark-mode');
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Teko:wght@300..700&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  position: relative;
}

:root {
  --colorPrimary: #fff;
  --colorSecondary: #000;
  --colorGray: rgb(153, 150, 150);
  --colorBlue: blue;
}

.dark-mode {
  --colorPrimary: #000;
  --colorSecondary: #fff;
  --colorBlue: rgb(31, 156, 156);
  --colorGray: rgb(199, 198, 198);
}


body {
  background-color: var(--colorPrimary);
  font-family: "Teko", sans-serif;
  font-optical-sizing: auto;
  font-weight: 500;
  font-style: normal;
  font-size: 1rem;
}

#modeIcon {
  position: absolute;
  top: 1%;
  right: 5%;
  font-size: 1.5rem;
  border: 1px solid var(--colorSecondary);
  padding: .5rem;
  border-radius: 50%;
  transition: all .2s ease-in;
  cursor: pointer;
  color: var(--colorSecondary);
}

#modeIcon:hover {
  background: var(--colorSecondary);
  color: var(--colorPrimary);
}

#app {
  border: none;
  border-radius: 1.5rem;
  box-shadow: 0 0 2rem var(--colorSecondary);
  padding: 1.5rem 1rem;
  width: 75vw;
  margin: 3rem auto 1rem;

  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.top {
  display: flex;
  flex-direction: column;
  color: var(--colorSecondary);
  border-bottom: 2px solid var(--colorSecondary);
}

.tasks {
  list-style: none;
}

.tasks li {
  margin: 1rem 0;
  padding: 1rem;
  border-radius: 2.5rem;
  letter-spacing: 1px;
  cursor: pointer;
  background: var(--colorGray);

  display: flex;
  justify-content: space-between;
}

.icons {
  flex: 0 10%;

  display: flex;
  gap: .5rem;
  justify-content: flex-end;
}

.text {
  flex: 1;
  color: var(--colorPrimary);
  letter-spacing: 1px;
  font-size: 1.2rem;
  transition: all .1s ease;
}

.text:hover {
  text-transform: uppercase;
}

.editing {
  color: var(--colorBlue) !important;
  text-transform: uppercase;
  border: 2px solid var(--colorBlue) !important;
}

li i {
  color: var(--colorPrimary);
  font-size: 1.3rem;
  transition: all .1s ease-in;
}

#editIcon:hover {
  color: var(--colorBlue);
}

#trashIcon:hover {
  color: red;
}

#input {
  padding: 1rem 1.5rem;
  border: 1px solid green;
  border-radius: 1.5rem;
  width: 70%;
  outline: none;
  background: rgb(240, 235, 235);
  color: green;
  letter-spacing: 1px;
  font-weight: 700;
}

.completed {
  text-decoration: line-through;
  color: green;
}
</style>
